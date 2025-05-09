# 📘 SHL Assessment Recommender – Documentation

## 🧩 Overview

This project performs **web scraping**, **data enrichment**, **storage**, **recommendation**, and **API serving** for SHL assessments using SHL's public product catalog. It leverages NLP techniques for recommending relevant assessments based on job descriptions.

---

## 📂 Module Breakdown

### 1. **Dependency Installation**

```python
!pip install fastapi nest-asyncio pyngrok uvicorn scikit-learn flask
!pip install gradio sentence-transformers numpy
```

Installs required libraries for web scraping, data science, embedding models, and API/UI frameworks.

---

### 2. **Scraping Assessment Data**

#### `extract_data_from_page(soup)`

Extracts assessments or entities from a SHL HTML page using BeautifulSoup. Pulls:

* Course/Entity ID
* Assessment Name
* Link
* Remote Support Indicator
* Adaptive Indicator
* Key Tags

#### `scrape_all_pages_by_type(content_type)`

Loops through paginated SHL catalog pages by content type (1 = course, 2 = entity), and aggregates data.

---

### 3. **Storing Scraped Data**

```python
with open("shl_data_basic.json", "w") ...
```

Saves the scraped basic metadata into a JSON file.

---

### 4. **Enriching Assessment Metadata**

#### `extract_detail_fields(item)`

Enhances each scraped assessment by visiting its detailed page to extract:

* Description
* Job Levels
* Languages
* Assessment Duration

Uses `ThreadPoolExecutor` to parallelize enrichment.

---

### 5. **Data Conversion to Excel**

Converts both raw and enriched JSON into Excel files using `pandas`.

---

### 6. **Embedding and Recommender Engine**

#### `load_and_prepare_data(filepath)`

Cleans and augments JSON data:

* Generates text for embedding
* Normalizes duration
* Maps keys to readable types

#### Embedding Model

```python
model = SentenceTransformer('sentence-transformers/msmarco-MiniLM-L12-cos-v5')
```

Generates sentence embeddings for assessments using the [Sentence Transformers](https://www.sbert.net/) library.

#### `recommend_assessments(user_query, max_duration=None)`

* Converts query to embedding
* Computes cosine similarity with assessment embeddings
* Returns top N (10) relevant assessments, optionally filtering by duration

---

### 7. **Evaluation Metrics**

Includes functions to evaluate the recommender:

* `calculate_recall_at_k()`
* `calculate_map_at_k()`
* `evaluate_model()`
  Takes test queries with ground truth and returns average recall\@k and MAP\@k.

---

### 8. **Gradio Web App**

```python
gr.Interface(...)
```

A simple UI to input job descriptions and return recommended SHL assessments using the Gradio library.

---

### 9. **Flask API with Ngrok Tunnel**

#### Endpoints

* `POST /shl-ai-apis/recommend`: Takes a query and returns assessment recommendations
* `GET /shl-ai-apis/health`: Simple health check endpoint

#### Ngrok Integration

Creates a public URL to expose the Flask app without manual hosting.

---

## 🧪 Example API Usage

### Health Check

```bash
GET /shl-ai-apis/health
```

### Get Recommendations

```bash
POST /shl-ai-apis/recommend
{
  "query": "I want to hire a content writer who is good at SEO and English"
}
```

Returns top assessments matching the request.

---

## 📌 Technologies Used

* **Python**: Core language
* **BeautifulSoup**: HTML scraping
* **Requests**: HTTP client
* **Pandas**: Data manipulation
* **Sentence-Transformers**: Embeddings
* **Gradio**: Interactive UI
* **Flask**: REST API
* **Ngrok**: Public tunneling
* **ThreadPoolExecutor**: Concurrency

---

## ✅ Final Outputs

* `shl_data_basic.json`: Raw scraped data
* `shl_data_enriched.json`: Enhanced with detailed metadata
* `shl_data_enriched.xlsx`, `shl_data_basic.xlsx`: Excel exports
* `/shl-ai-apis/recommend`: Recommendation endpoint
* Gradio interface for interactive testing


