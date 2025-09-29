# Infosys_Springboard - Milestone 1: Code Representation, Parsing, and Model-Based Analysis

This repository contains the solution for Milestone 1 of the Infosys Springboard program, focusing on developing a pipeline for analyzing small Python code snippets.

## 1. Approach and Methodology

The solution implements a multi-stage pipeline using Python libraries suitable for analyzing source code:

1.  **Data Preparation:** A set of 10 diverse Python code snippets (simple functions, classes, decorators, generators, and exception handling) are defined.
2.  **Code Representation and Parsing (AST/Tokens):** Each snippet is processed using Python's built-in `ast` module (Abstract Syntax Tree) for structural analysis and the `tokenize` module for lexical analysis.
3.  **Model-Based Analysis (Code Embeddings):** The raw code strings are converted into dense vector embeddings using pre-trained **Sentence Transformer models**. These embeddings serve as the "model-based analysis" step, capturing the semantic and structural context of the code in a format suitable for machine learning algorithms.
4.  **Dimensionality Reduction and Visualization:** The high-dimensional embeddings are reduced to 2D using **Principal Component Analysis (PCA)** and **t-distributed Stochastic Neighbor Embedding (t-SNE)**. The resulting plots visualize the semantic clustering of the code snippets, allowing for qualitative analysis.

---

## 2. Pipeline Implementation Details

### 2.1 Code Representation and Parsing

The Python `ast` module was used to parse each code snippet into its Abstract Syntax Tree. This structural representation allowed for the extraction of metadata:

* **Structural Features (Parsing):** We specifically extracted a list of `functions`, `classes`, and `imports` present in each snippet.
* **Lexical Features (Representation):** The `tokenize` module was used to extract a preview of the raw tokens (keywords, identifiers, operators, etc.).

**Libraries Used:** `ast`, `tokenize`, `io`

### 2.2 Model-Based Analysis (Semantic Embedding)

**Goal:** Transform the syntactically parsed and raw text code into a semantic vector space.

* **Models Selected (Sentence Transformers):**
    * `all-MiniLM-L6-v2` (Fast, general-purpose sentence embedding)
    * `distilroberta-base` (RoBERTa-based model, generally robust)
    * `all-mpnet-base-v2` (High-performance general-purpose model)

The raw code strings were passed to these models to generate dense, numerical representations (embeddings) that capture the meaning and context of the code.

### 2.3 Clustering and Visualization

The core task of **model-based analysis** often involves interpreting the similarity between representations. We used two techniques to visualize this in 2D:

* **PCA (Principal Component Analysis):** A linear dimensionality reduction technique that finds the directions (principal components) that maximize variance. It is useful for retaining global structure.
* **t-SNE (t-distributed Stochastic Neighbor Embedding):** A non-linear dimensionality reduction technique that excels at maintaining local structure and revealing clusters in high-dimensional data. A conservative perplexity value was chosen due to the small size of the dataset.

---

## 3. Observations and Analysis

The visualizations (saved in the `results` folder) and parsed data lead to the following key observations:

1.  **Effective Parsing:** The Python `ast` module accurately extracted the structural components (functions, classes, imports) from the diverse snippets, demonstrating a successful parsing step.
2.  **Semantic Grouping (Clustering):**
    * **Snippet_01 (Simple Function) and Snippet_03 (Simple Function/Import) / Snippet_06 (Lambda/Function):** These simple, non-class, and non-complex structural snippets tend to be clustered together by the models (particularly evident in the MPNet and MiniLM embeddings), indicating high semantic similarity for basic procedural code.
    * **Snippet_02 (Counter Class) and Snippet_10 (Graph Class):** These two object-oriented examples are consistently clustered together and placed far away from the purely functional/procedural snippets across all models and visualizations (PCA/t-SNE). This confirms the models successfully captured the distinct "class structure" feature during embedding.
    * **Decorator (`snippet_07`) and Closure (`snippet_09`):** These two snippets, which both involve nested function definitions and functional programming concepts, often appear closer to each other than to simple procedural code, suggesting the models capture the functional pattern despite the difference in their exact purpose.
3.  **Model Sensitivity:** The visualizations show that the choice of embedding model (MiniLM, DistilRoBERTa, MPNet) significantly impacts the final coordinates and the tightness of the clusters, illustrating that the "model-based analysis" step is sensitive to the underlying representation model.

---

## 4. Execution and Reproducibility

The entire pipeline is implemented in the provided Google Colaboratory notebook.

1.  **Environment:** Google Colaboratory (Python 3 environment).
2.  **Dependencies:** Ensure the `sentence-transformers` library is installed (handled in the first cell of the notebook).
    ```bash
    !pip install sentence-transformers
    ```
3.  **Execution:** Run all cells sequentially in the `Milestone1.ipynb` notebook. The plots will be generated, saved to the `results` folder, and displayed at the end.

---
***

**Action Item:** Create your public repository named `Infosys_Springboard`, implement the folder structure above, include the executed notebook, and paste the content above into your `README.md`. Finally, share the repository link via email.
