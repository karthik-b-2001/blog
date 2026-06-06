---
title: "VyasaGraph: Building a Mahabharata Knowledge Graph and RAG Chatbot"
date: 2026-06-05
description: "How I built a knowledge graph and RAG pipeline over 1.8M words of the Mahabharata using Neo4j, ChromaDB, spaCy, and Ollama"
type: "post"
tags: ["Project", "NLP", "Knowledge Graphs"]
image: "/images/vyasagraph-og.png"
---

I had some time this summer and a question that had been sitting with me: how do large language models actually retrieve and reason over data? Not the transformer internals or the attention math, but the stuff that happens before inference. How does data need to be shaped so a model can do something useful with it?

I wanted to build something to figure that out. And I wanted a corpus that was messy enough to actually teach me something.

## Why the Mahabharata?

I grew up hearing these stories. Bhishma's oath, the dice game, Karna's loyalty, Arjuna's doubt on the battlefield. But beyond the personal connection, the Mahabharata is a genuinely difficult text to work with computationally. 1.8 million words across 18 books. Hundreds of named characters. Relationships that are familial, political, divine, and adversarial, sometimes all at once.

Characters go by multiple names. Arjuna is also Partha, Dhananjaya, Savyasachi, Gudakesha. The same person might be referred to differently in consecutive paragraphs. X is the son of Y, who is an incarnation of Z, who fought alongside W in a battle three generations ago. If you want to stress-test a data pipeline, this is the text to do it with.

## Discovering Graph Databases

My research led me to Neo4j, and it clicked immediately.

The Mahabharata isn't a list of facts. It's a web of relationships: who is kin to whom, who killed whom, who fought in which war, who was allied with which kingdom. Modeling this in a relational database would mean endless JOIN tables and queries that spiral out of control. A graph database handles this naturally. Characters are nodes, relationships are edges, and you traverse them with simple Cypher queries.

Want to know all the allies of Arjuna's enemies? That's a few hops through the graph. In SQL, good luck.

When I loaded the first few hundred character nodes and saw them connected by webs of kinship and conflict in Neo4j Browser, it was clear this was the right tool.

## Teaching a Machine to Read Sanskrit Names

Off-the-shelf NLP models don't know what to do with mythological Indian texts. spaCy's English models are trained on news articles and web text. They identify "Barack Obama" as a person just fine. "Dhritarashtra"? "Yudhishthira"? These get misclassified or ignored entirely.

Entity recognition was one challenge, but the harder problem was relationship extraction. The text doesn't say "Arjuna ALLIED_WITH Krishna." It says something like "And so Partha, guided by the son of Devaki, rode forth into battle." I used spaCy's dependency parsing to extract relationships from grammatical structure: epithet mining ("X, son of Y") and SVO (subject-verb-object) triples for kill relationships. Each extracted relationship was then scored by attestation, meaning how many independent sentences in the corpus support it. A relationship mentioned across 7 sentences in 3 different parvas is near-certain. One that shows up once in a chaotic battle scene is suspect.

Every extraction went through human review. Mine.

## Building the Graph from Three Sources

I didn't want to rely on a single extraction method. The final knowledge graph is built from three independent pipelines, merged and deduplicated:

The first was the attestation-based NLP extraction from the text itself, which gave me 91 relationships. The second was Wikidata, where I wrote SPARQL queries against all Mahabharata characters (tagged with P1441 = Q8276) to pull parents, spouses, siblings, children, kills, and conflicts. That gave me 245 relationships, with Sanskrit diacritics normalized to English transliterations. The third was a structured CSV dataset of family relationships (father/son, mother/son, husband/wife, brothers), which added another 67.

After merging and deduplication, the graph has 228 character nodes and 315 relationships. 66 of those relationships are confirmed by multiple independent sources, which gave me real confidence in the data quality.

This is probably what I'm proudest of in the project. Most knowledge graphs over texts like this either use a single pre-built dataset or just throw an LLM at the text and trust whatever comes out. Neither approach gives you traceability. In VyasaGraph, every relationship is traceable to its origin: which pipeline produced it, how many sources confirm it.

## When the Graph Gets It Wrong

It still got things wrong though.

Here's an early version of the KILLED relationship subgraph:

![Incorrect KILLED relationships in the graph](/images/vyasagraph-graph-mistake.png)

Karna killing Satyaki, Satyaki killing Drona, Abhimanyu killing Lakshmana and Vikarna. Some of these are partially right, some are flat out wrong, and the directionality is off on others. Even with structured SVO extraction, battle scenes in the Mahabharata are dense and confusing. Multiple warriors are mentioned in rapid succession with kill verbs flying everywhere, and the parser sometimes attaches the wrong subject to the wrong object.

Compare that to the MARRIED_TO subgraph, which came out clean:

![Correct MARRIED_TO relationships in the graph](/images/vyasagraph-graph-correct.png)

All five Pandavas married to Draupadi, Arjuna also married to Subhadra, Pandu married to Kunti and Madri. This worked because marriages in the text are stated directly and unambiguously. No inference needed.

The pattern was consistent. Explicit relationships (marriages, parentage) extract reliably. Contextual relationships (kills, alliances) need careful handling and validation. That's exactly why the multi-source approach matters: the Wikidata kill data could catch errors in the NLP extraction, and vice versa.

## Why Both Vector Search and Graph Traversal?

This was the part I was most curious about going in.

RAG (Retrieval-Augmented Generation) retrieves relevant context from an external source at query time instead of relying on the LLM's built-in knowledge. But plain vector search has limits.

ChromaDB with sentence embeddings (all-MiniLM-L6-v2, embedded locally) can answer "Tell me about Bhishma" well enough. It finds semantically similar text chunks and hands them to the model. But ask "Who are Bhishma's nephews and which ones fought against him in the Kurukshetra war?" and it falls apart. That question needs relational reasoning: traversing family trees and battle allegiances, not just finding similar paragraphs.

So the pipeline uses both. ChromaDB handles semantic retrieval (finding relevant narrative passages from the 960 embedded chunks), Neo4j handles structural queries (traversing the relationship graph), and the LLM (llama3.1:8b running locally through Ollama) puts it all together. The answer gets streamed back through FastAPI to a React 19 frontend with entity detection, expandable graph facts, and source citations.

The first time it correctly answered a multi-hop question by combining a graph traversal with a text chunk, that was a good moment.

## What I Learned

The biggest thing: the quality of your retrieval pipeline matters more than the quality of your model. A decent model with good context will outperform a powerful model with bad context.

I also came away with a real appreciation for the NLP challenges in non-English and historical texts. The tools are good, but they're built for modern English. Adapting them to the Mahabharata took real work.

And building a system that can reason about stories I grew up with was pretty cool.

The project is open source on [GitHub](https://github.com/karthik-b-2001/VyasaGraph).