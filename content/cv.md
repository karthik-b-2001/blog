+++
title = "CV"
type = "page"
+++

Currently pursuing a Master of Science in Computer Science at Northeastern University (4.0 GPA). Software engineer with 2 years of experience in backend systems, data pipelines, and cloud infrastructure. Currently studying genetic algorithms and evolutionary computation.

You can connect with me by writing to me at [karthikbharadwajds@gmail.com](mailto:karthikbharadwajds@gmail.com).

[📄 Download Resume (PDF)](/files/Karthik_B_resume_main.pdf)

---

<br>

## Education (ಶಿಕ್ಷಣ)

<br>

> **Northeastern University** — M.S. Computer Science  
> `Expected Dec 2027` · `Boston, MA` · `GPA: 4.0`

```
Algorithms · DBMS · Foundations of AI · Programming Design Paradigms · Machine Learning · NLP
```

<br>

> **R V College of Engineering** — B.E. Computer Science and Engineering  
> `May 2023` · `Bangalore, India` ·

<br>




<br>

---

<br>

## Experience (ಅನುಭವ)

<br>

> **Digitide Solutions Ltd** — Software Engineer  
> `Sep 2024 – Jun 2025` · `Bangalore, India`

- Developed an NLP-powered chatbot that integrated AI models with backend services, enabling warehouse staff to query inventory via natural language and reducing manual lookup time by 60%.
- Delivered RESTful API-driven SaaS modules for AB-InBev serving 500+ daily users, including report generation, admin dashboards, and bulk data workflows.
- Architected and deployed backend services on Azure using Docker containers, reducing release turnaround time by 2 days.

<br>

> **zevvo** — Full Stack Engineer  
> `Jan 2024 – Jul 2024` · `Bangalore, India`

- Built scalable backend systems from scratch for bookings, billing, and catalog management with MongoDB schema design, JWT authentication, and RESTful API conventions, onboarding 50+ active users within the first quarter.
- Integrated third-party payment and notification SDKs into the backend service layer, enabling real-time transaction processing and reducing checkout drop-off by 15%.
- Streamlined CI/CD via GitHub Actions, deploying Dockerized services to AWS (ECS, S3, ECR) with auto-scaling and increasing deploy frequency by 40%.

<br>

> **Sun Precision Tools** — Software Engineer  
> `Aug 2023 – Dec 2023` · `Bangalore, India`

- Engineered a software system from the ground up to replace a paper-based invoice generation process, owning architecture, implementation, and rollout across the shop floor.
- Developed digital gauge lifecycle management from zero, designing state tracking, transitions, and reporting workflows that gave operations teams real-time visibility into calibration and compliance status.

<br>

> **Crestron Electronics** — Software Development Intern  
> `Mar 2023 – Jul 2023` · `Bangalore, India`

- Optimized .NET Core REST APIs with rewritten SQL queries, cutting average response time by 28% across production endpoints.
- Elevated unit test coverage from 65% to 89% through Jest suites, reducing post-deployment bugs and shortening the QA feedback loop.

<br>

---

<br>

## Projects (ಯೋಜನೆಗಳು)

<br>

### [VyasaGraph — Mahabharata Knowledge Graph & RAG Chatbot](https://github.com/karthik-b-2001/vyasagraph)

```
Python · FastAPI · React 19 · TypeScript · Neo4j · ChromaDB · spaCy · Ollama · Wikidata SPARQL · Tailwind CSS
```

- Built a knowledge graph over the 1.8M-word Mahabharata corpus with 228 characters and 315 relationships sourced from three independent pipelines: spaCy dependency-parsed attestations from the original text, a structured CSV dataset, and Wikidata SPARQL queries, with human review and deduplication across all sources.
- Implemented a graph-augmented RAG pipeline combining Neo4j Cypher queries with ChromaDB semantic search (960 embedded chunks via sentence-transformers) to ground LLM answers in both structured facts and source text, served through a FastAPI streaming backend and a React chat interface with real-time citation display.

<br>

### [ParetoFolio — Portfolio Optimization Engine](https://github.com/karthik-b-2001/ParetoFolio)

```
Python · FastAPI · Pydantic v2 · React 19 · TypeScript · Tailwind CSS · Recharts
```

- Built a responsive React 19 frontend with interactive data visualization and a typed FastAPI backend, serving a RESTful API for multi-objective portfolio optimization across 75 US equities.
- Implemented NSGA-II from scratch: SBX crossover, polynomial mutation, tournament selection, and crowding distance, producing Pareto-optimal portfolios with an interactive efficient frontier and Sharpe-ranked table.

<br>

### [Gojo Satoru Hand-Tracking VFX Engine](https://github.com/karthik-b-2001/gojo-handtracker)

```
Python · OpenCV · MediaPipe · NumPy · Streamlit · WebRTC
```

- Built a real-time, markerless VFX engine that overlays Jujutsu Kaisen inspired cursed-energy effects onto a live webcam feed driven entirely by hand gestures. Tracks both hands at 21 landmarks each, classifies five distinct gestures, and maps each to a different effect including hand-following auras, energy orbs, a charged beam, and a full-frame Domain Expansion with background segmentation.
- Implemented the effects layer with a 200-particle physics system (attraction, tangential spin, life-decay), additive glow compositing, and confidence-mask alpha blending from MediaPipe selfie segmentation. Ported to the browser via Streamlit + streamlit-webrtc with per-session isolated state.

<br>

### [SportsTV Streaming Data Warehouse](https://github.com/karthik-b-2001/sportstv-streaming-warehouse)

```
R · MySQL 8 · SQLite · R Markdown · Kimball Star Schema
```

- Engineered a bulk-batched, idempotent ETL data pipeline in R with surrogate-key resolution and partition pruning, achieving a 99%+ source-to-warehouse match rate across 2M+ streaming transactions.
- Modeled a Kimball-style star schema with five conformed dimensions and a RANGE-partitioned fact table on cloud-hosted MySQL 8, consolidating heterogeneous data sources into a unified analytical warehouse.

<br>

### [Java Calendar Application](https://github.com/karthik-b-2001/multiCal)

```
Java · Swing · JUnit · PIT Mutation Testing · MVC
```

- Built a calendar application in Java with MVC architecture, supporting recurring event series, timezone handling, and multiple calendar views through extensive use of design patterns (Builder, Command, Strategy, Visitor, Decorator, Factory Method).
- Full GUI in Java Swing with rigorous test coverage via JUnit and mutation testing using PIT, achieving high mutation kill rates that validated test suite effectiveness beyond simple line coverage.

<br>

### Autonomous Vehicle Perception Pipeline

```
Python · PyTorch · YOLOv3 · OpenCV · NumPy
```

- Fine-tuned YOLOv3 on a custom traffic sign dataset, achieving 88% mAP for real-time detection across varying lighting and weather conditions.
- Created a multi-sensor fusion pipeline combining camera and distance inputs for autonomous steering and braking, enabling lane-keeping without manual waypoint programming.

<br>

---

<br>

## Skills (ಕೌಶಲ್ಯ)

| *In all my years of tinkering with computers, this is what stuck.* | |
|:--|:--|
| **Programming Languages** | `Python` · `SQL` · `Java` · `TypeScript` · `JavaScript` · `R` · `C++` · `C#` |
| **Backend** | `FastAPI` · `Node.js` · `.NET Core` · `Express.js` · `SQLAlchemy` · `REST APIs` · `Microservices` |
| **Frontend** | `React.js` · `Next.js` · `Angular` · `Tailwind CSS` |
| **Databases** | `PostgreSQL` · `MySQL` · `MongoDB` · `SQLite` · `Redis` · `Firebase` · `Neo4j` · `ChromaDB` |
| **Cloud & DevOps** | `AWS` · `GCP` · `Azure` · `Docker` · `Kubernetes` · `GitHub Actions` · `CI/CD` |
| **ML & AI** | `PyTorch` · `scikit-learn` · `OpenCV` · `YOLOv3` · `CNNs` · `Transfer Learning` · `NLP` · `spaCy` · `RAG` |
| **Practices** | `API Design` · `System Design` · `Distributed Systems` · `Caching` · `OOP` · `Agile` · `TDD` · `Git` |

<br>

---

<br>

## Languages (ಭಾಷೆಗಳು)

> **English** — Fluent  
> **Kannada (ಕನ್ನಡ)** — Fluent  
> **Japanese (日本語)** — Intermediate