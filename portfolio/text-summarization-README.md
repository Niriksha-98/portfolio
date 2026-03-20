# text-summarization-streaming

containerized ml pipeline for topic modeling and real-time text summarization. built to explore how you take an ml model out of a notebook and into something that actually runs reliably.

## what it does

two parts:

1. **topic modeling** — uses LDA (Latent Dirichlet Allocation) to identify non-overlapping topics in a document corpus. visualizes results with 90% precision on key topics.

2. **real-time summarization** — spark streaming pipeline that summarizes incoming text documents as they arrive. modular component design so you can swap the model without touching the pipeline.

## results

- **90% precision** on non-overlapping key topic identification via LDA
- production-ready containerized deployment with Docker
- modular architecture — model components are independently replaceable

## architecture

```
document stream
      ↓
  Spark Streaming (ingestion)
      ↓
  preprocessing (tokenization, stopword removal)
      ↓
  ┌─────────────────────────────┐
  │  topic modeling (LDA)       │  ← batch, run on corpus
  │  summarization model        │  ← streaming, real-time
  └─────────────────────────────┘
      ↓
  MongoDB (results storage)
      ↓
  Tableau visualization (topics)
```

## stack

- **ml**: scikit-learn LDA, custom summarization model
- **streaming**: Apache Spark Streaming
- **storage**: MongoDB
- **visualization**: Tableau
- **deployment**: Docker, docker-compose
- **language**: Python

## setup

```bash
git clone https://github.com/niriksha-narendra/text-summarization-streaming
cd text-summarization-streaming

# run with docker-compose
docker-compose up --build
```

requires Docker and docker-compose. no other local dependencies.

## things i learned

- LDA needs careful hyperparameter tuning — number of topics and alpha/beta priors have a huge effect on topic coherence
- spark streaming requires thinking about state differently than batch — you have to decide what to do with late-arriving data
- containerizing ml models forces you to be explicit about dependencies in a way that running locally doesn't

## structure

```
├── docker-compose.yml
├── Dockerfile
├── src/
│   ├── topic_model/       # LDA training and inference
│   ├── summarizer/        # streaming summarization model
│   ├── streaming/         # Spark Streaming pipeline
│   └── utils/
├── notebooks/             # exploratory analysis
├── data/
│   └── sample/            # sample documents for testing
└── requirements.txt
```
