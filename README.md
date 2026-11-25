# CS 582 – Assignment 1

---

**Siddhi Dabholkar**  
**UIN:** 661384505  
**Email:** sdabh@uic.edu  

---

## The assignment1.py - Vector Space Model Information Retrieval System (TF-IDF, Cosine Similarity)  

- Preprocesses documents and queries by removing SGML tags, stopwords, punctuation, numbers, and short tokens.  
- Applies Porter stemming (with a simplified fallback if NLTK is not available).  
- Builds an inverted index with TF–IDF weighting (TF raw counts, IDF = log(N/df)).  
- Ranks documents for each query using cosine similarity.  
- Outputs ranked retrieval results and evaluates them using precision and recall at various cutoffs.  

---

## Preprocessing Steps  

For both documents and queries:  
1. Removed SGML tags (e.g., `<TITLE>`, `<TEXT>`).  
2. Kept only title + text content.  
3. Converted text to lowercase.  
4. Removed punctuation and digits.  
5. Split on whitespace.  
6. Removed stopwords (using NLTK English stopword list).  
7. Applied Porter stemming (or fallback rules).  
8. Removed tokens of length ≤ 2.  
9. If a word becomes a stopword after stemming, I have removed it.  

---

## Indexing & Retrieval  

- **Indexing:**  
  * Each term gets a unique ID.  
  * Postings list stores `(doc_id, raw_tf)`.  
  * Document vectors are weighted as:  
    \[
    w_{t,d} = tf_{t,d} \times \log \left(\frac{N}{df_t}\right)
    \]  
  * Document norms are precomputed for cosine normalization.  

- **Query Processing:**  
  * Same preprocessing pipeline as documents.  
  * Query vector weighted similarly with TF–IDF.  
  * Cosine similarity between query and document vectors.  

- **Ranking:**  
  * Documents ranked in descending order of similarity.  
  * Output written as `(query_id, document_id)` pairs.  

---

## Outputs  

The program produces three output files in the `output/` directory inside the Assignment1 folder:  

1. **ranked_results.txt**  
   - Ranked results for each query.  
   - Format:  
     ```
     <query_id> <doc_id>
     ```

2. **evaluation_metrics.txt**  
   - Precision and Recall results.  
   - Reports macro-averaged values at K ∈ {10, 50, 100, 500}.  
   - Includes per-query breakdown.  

Additionally, average precision and recall are printed to the console.  

---

## How to Run  

### Requirements
- Python 3.8+  

### Files Required  
Make sure all of the below mentioned files are in a single folder `Assignment1`:  
- `assignment1.py` (this program)  
- `cranfield.tar.gz` (dataset)  
- `queries.txt`  
- `relevance.txt`  

### Command to Run  
For me:  
```bash
python assignment1.py "C:\Users\<yourname>\Desktop\CS 582 HOMEWORKS\Assignment1"
```

For you: copy the path of your Assignment1 folder and run:  
```bash
python assignment1.py "(location of Assignment1 folder)"
```

- On first run: program automatically extracts `cranfield.tar.gz` into `cranfieldDocs/`.  
- On subsequent runs: extraction is skipped if `cranfieldDocs/` already exists.  

Outputs appear in:  
```
Assignment1/output/
```

---

## Inference & Results  

From the Cranfield dataset (10 queries evaluated):  
- Precision decreases as K increases.  
- Recall improves as K increases.  
- At K=10: precision = 0.20, recall = 0.18.  
- At K=500: recall ≈ 1.0, precision ≈ 0.02.  

**Precision and Recall Table (macro-averaged over queries):**

| K   | Precision | Recall  |
|-----|-----------|---------|
| 10  | 0.2000    | 0.1847 |
| 50  | 0.0980    | 0.4224 |
| 100 | 0.0670    | 0.5332 |
| 500 | 0.0234    | 0.9375 |

This shows the expected trade-off in IR: higher cutoff improves recall but reduces precision.  

---

## Notes  

- Stopword removal is applied both before and after stemming.  
- Tokens ≤ 2 characters are discarded.  
- If NLTK’s Porter Stemmer is unavailable, a simplified fallback is used.  
- Code avoids re-extracting the dataset on each run.  
- Paths are input via command-line argument (`base_dir`) and not hardcoded.  
