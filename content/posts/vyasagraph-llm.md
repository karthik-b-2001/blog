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

I built a custom NER pipeline using attestation-based scoring: cross-referencing extracted entities against known character lists from Wikidata and the text itself to build confidence that "Partha" and "Arjuna" refer to the same person. Watching the entity resolution improve with each iteration was one of the more satisfying parts of this project.

Relationship extraction was harder. The text doesn't say "Arjuna ALLIED_WITH Krishna." It says something like "And so Partha, guided by the son of Devaki, rode forth into battle." Turning that into a structured triple (Arjuna, ALLIED_WITH, Krishna) required dependency parsing, pattern matching, and a lot of manual validation against ground truth data I pulled from Wikidata's SPARQL endpoint.

## When the Graph Gets It Wrong

And it got things wrong. A lot.

Here's an early version of the KILLED relationship subgraph:

![Incorrect KILLED relationships in the graph](/images/vyasagraph-graph-mistake.png)

Karna killing Satyaki, Satyaki killing Drona, Abhimanyu killing Lakshmana and Vikarna. Some of these are partially right, some are flat out wrong, and the directionality is off on others. The extraction pipeline was too aggressively inferring KILLED edges from co-occurrence. Two warriors mentioned in the same battle passage doesn't mean one killed the other.

Compare that to the MARRIED_TO subgraph, which came out clean:

![Correct MARRIED_TO relationships in the graph](/images/vyasagraph-graph-correct.png)

All five Pandavas married to Draupadi, Arjuna also married to Subhadra, Pandu married to Kunti and Madri. This worked because marriage relationships in the text are stated explicitly. There's no inference needed.

The takeaway was clear. Relationship types that are stated directly in the source (marriages, parentage) extract reliably. Relationship types that require contextual reasoning (kills, alliances, betrayals) need much more careful handling. This is where ground truth validation became essential, not as a nice-to-have, but as a core part of the pipeline.

## Why Both Vector Search and Graph Traversal?

This was the part I was most curious about going in.

RAG (Retrieval-Augmented Generation) is the pattern where you retrieve relevant context from an external source at query time instead of relying on the LLM's baked-in knowledge. But plain vector search has limits.

ChromaDB with sentence embeddings can answer "Tell me about Bhishma" well enough. It finds semantically similar text chunks and hands them to the model. But ask "Who are Bhishma's nephews and which ones fought against him in the Kurukshetra war?" and it falls apart. That question needs relational reasoning: traversing family trees and battle allegiances, not just finding similar paragraphs.

So the pipeline uses both. ChromaDB handles semantic retrieval (finding relevant narrative passages), Neo4j handles structural queries (traversing relationships), and the LLM (running locally through Ollama) puts it all together into a coherent answer.

The first time it correctly answered a multi-hop question by combining a graph traversal with a text chunk, that was a good moment.

## The Pipeline

The data flows like this: the raw text (all 18 books) gets parsed and chunked. spaCy runs NER over the chunks to identify characters. The relationship extraction pipeline pulls out structured triples, which get loaded into Neo4j as the knowledge graph. The same chunks get embedded and stored in ChromaDB.

At query time, the user's question hits both stores. The vector store returns relevant passages, the graph returns relevant relationships. Both get combined into prompt context, and Ollama generates the answer, streamed through a FastAPI backend to a React 19 frontend.

Conceptually simple. Making each piece work reliably on messy mythological text is where the time went.

## What I Learned

The biggest thing: the quality of your retrieval pipeline matters more than the quality of your model. A decent model with good context will outperform a powerful model with bad context every time.

I also came away with a real appreciation for the NLP challenges in non-English and historical texts. The tools are good, but they're built for modern English. Adapting them to something like the Mahabharata takes work.

And building a system that can reason about stories I grew up with was pretty cool.

The project is open source on [GitHub](https://github.com/karthik-b-2001/VyasaGraph). If you're into knowledge graphs, RAG, or the Mahabharata, feel free to reach out.