# Inverted Index Clustering Compression

Course project for **Information Retrieval and Web Search (IRWS)** focused on improving inverted index compression through **document clustering** and **DocID reassignment** techniques.

The project investigates how grouping similar documents together can reduce posting list d-gaps and improve compression efficiency for large-scale inverted indexes.

---

# Project Goal

Traditional inverted indexes store posting lists sorted by document IDs.  
By reassigning document IDs according to document similarity, smaller d-gaps can be obtained, leading to better compression performance for gap-based encoding methods such as Variable Byte and PForDelta.

This project explores scalable alternatives to the classical TSP-based DocID reassignment approach using clustering algorithms.

---

# Implemented Requirements

## Mandatory Requirements

### Slide 13 + 16 Requirements

- TermID mapping and document set transformation
- Stream clustering (single scan)
- TSP on medoids
- DocID reassignment
- d-gap computation
- Average bits per posting calculation
- Compression evaluation using:
  - Variable Byte
  - Gamma
  - Delta
- Compression rate comparison between clustering methods
- Execution time vs. number of clusters analysis
- Variable Byte compressed binary index materialization
- PForDelta compressed binary index materialization
- Decompression time evaluation for top-100 frequent terms
- Full collection processing (~800K RCV1 documents)

All mandatory project requirements are implemented.

---

# Implemented Methods

## 1. Stream Clustering (Single Scan)

Documents are processed sequentially and assigned to the nearest cluster using Jaccard distance.

If the minimum distance exceeds a radius threshold, a new cluster is created.

### Features
- Single pass over the collection
- Scalable for large datasets
- Medoid-based representation

---

## 2. Two-Scan Stream Clustering

A hierarchical variant of stream clustering using two different radius values:

- First pass: coarse clustering
- Second pass: refinement inside clusters

This improves cluster quality while remaining scalable.

---

## 3. K-Medoids Clustering

Implemented as an alternative clustering strategy.

### Pipeline
1. Sample a subset of the collection
2. Apply K-Medoids on the sample
3. Obtain k medoids
4. Assign remaining documents to the nearest medoid using Jaccard distance
5. Apply TSP on medoids
6. Perform DocID reassignment
7. Evaluate compression performance

### Motivation
K-Medoids is more robust to sparse binary document representations and directly supports Jaccard distance.

---

## 4. K-Means Clustering

Implemented using TF-IDF document vectors.

### Pipeline
1. Compute normalized TF-IDF vectors
2. Sample a subset of the collection
3. Apply K-Means clustering
4. Obtain cluster centroids
5. Assign remaining documents to the nearest centroid using cosine similarity
6. Apply TSP on centroids
7. Perform DocID reassignment
8. Evaluate compression performance

### Motivation
K-Means provides scalable vector-space clustering and allows centroid-based ordering.

---

# Compression Techniques

## Variable Byte Encoding
- Gap-based integer compression
- Binary index materialization implemented

## PForDelta
- Block-based integer compression
- Binary index materialization implemented

## Gamma Encoding
- Elias Gamma compression evaluation

## Delta Encoding
- Elias Delta compression evaluation

---

# Processing Pipeline

1. Parse and preprocess documents
2. Build inverted index using SPIMI
3. Map vocabulary terms to termIDs
4. Transform documents into sets/vectors
5. Cluster documents
6. Reorder clusters using TSP
7. Reassign DocIDs
8. Compute d-gaps
9. Compress posting lists
10. Store compressed indexes on disk
11. Evaluate compression and decompression performance

---

# Dataset

## RCV1 Collection
- Approximately 800,000 documents
- Full collection processing implemented

---

# Evaluation

The project evaluates:

- Compression rate
- Average bits per posting
- d-gap distributions
- Execution time
- Number of clusters
- Compression vs clustering strategy
- Decompression speed
- Variable Byte vs PForDelta performance

---

# Experimental Comparisons

The following clustering strategies are compared:

- Single-scan Stream Clustering
- Two-scan Stream Clustering
- K-Medoids
- K-Means

The comparison includes:
- Compression rate
- Runtime
- Cluster count
- Average bits/posting
- Decompression performance

---

# Technologies

- Python
- NumPy
- scikit-learn
- SciPy
- Jupyter Notebook
- Google OR-Tools

---

# Repository Structure

```text
├── FILES/
├── lexicon.idx
├── posting.idx
├── RCV1 SPIMI indexer_PROJECT.ipynb
├── README.md
```

---

# Running the Project

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

---

# Research Motivation

Documents sharing many common terms tend to produce smaller posting list gaps when assigned nearby DocIDs.

This project investigates whether clustering-based document ordering can improve inverted index compression in a scalable way compared to traditional TSP-based approaches.

---

# References

- Introduction to Information Retrieval — Manning, Raghavan, Schütze
- Faster Top-k Document Retrieval Using Block-Max Indexes (SIGIR 2011)
- Google OR-Tools Documentation

---

# Author

Esma Nur Kocakaya  
MSc Computer Science and Information Technology  
Ca' Foscari University of Venice
