# 🔍 SHL Assessment Recommender System

This project automates the **scraping, enrichment, recommendation, and serving** of SHL assessments using NLP techniques and web APIs. It allows recruiters and hiring managers to input job descriptions and instantly get relevant SHL assessment recommendations.

> 🚀 **Live Demo**: [Gradio App](https://9f9c78fa70f32f940e.gradio.live)

---

## 📌 Features

- ✅ Scrapes SHL product catalog (courses & entities)
- ✅ Enriches each item with job levels, duration, languages, etc.
- ✅ Saves data in `.json` and `.xlsx` formats
- ✅ Uses sentence embeddings for semantic recommendation
- ✅ Evaluates model with Recall@K and MAP@K
- ✅ Interactive UI (Gradio) + REST API (Flask + Ngrok)

---

## 🔧 Setup Instructions

1. **Install dependencies**:

```bash
pip install fastapi nest-asyncio pyngrok uvicorn scikit-learn flask gradio sentence-transformers numpy
````

2. **Run the scraper and enrichment scripts** to generate `shl_data_enriched.json`.

3. **Launch the Gradio App**:


Or use the hosted version here: 👉 [https://9f9c78fa70f32f940e.gradio.live](https://9f9c78fa70f32f940e.gradio.live)

4. **Launch the Flask API**:


## 📡 API Endpoints

| Endpoint                 | Method | Description                                         |
| ------------------------ | ------ | --------------------------------------------------- |
| `/shl-ai-apis/health`    | GET    | Health check                                        |
| `/shl-ai-apis/recommend` | POST   | Get top 10 recommended assessments based on a query |

### 🔁 Example API Usage (Python)

```python
import requests
import json

# Health check
res = requests.get("https://f540-35-199-157-109.ngrok-free.app/shl-ai-apis/health")
print(res.status_code)
print(json.dumps(res.json(), indent=4))

# Recommendation query
query = {
    "query": "I want to hire a content writer who is good at SEO and English"
}
res = requests.post("https://f540-35-199-157-109.ngrok-free.app/shl-ai-apis/recommend", json=query)
print(res.status_code)
print(json.dumps(res.json(), indent=4))
```

---

## 📊 Example Output

```json
{
  "Assessment Name": "Data Analyst Assessment",
  "Job Level": "Entry",
  "Duration (min)": 45,
  "Remote Testing": "Yes",
  "Adaptive/IRT": "No",
  "Test Type(s)": "Ability & Aptitude, Knowledge & Skills",
  "Languages": "English",
  "Description": "This assessment evaluates core data analytics skills in Excel and Python..."
}
```

---

## 🧪 Evaluation Metrics

Evaluation functions use labeled test data and return:

* **Recall\@K**
* **MAP\@K**

To test model performance, run:


---

## 🧩 Project Architecture (Module Overview)

### 📘 1. Scraper Module

* **`extract_data_from_page(soup)`**: Parses SHL catalog HTML using BeautifulSoup.
* **`scrape_all_pages_by_type(content_type)`**: Aggregates data for "course" and "entity" types.

Saves data to `shl_data_basic.json`.

---

### 🔍 2. Enrichment Module

* **`extract_detail_fields(item)`**: Visits individual assessment pages and enriches with:

  * Job Level
  * Duration
  * Languages
  * Description
* Parallelized using `ThreadPoolExecutor`.

Saves enriched data to `shl_data_enriched.json` and Excel files.

---

### 📈 3. Embedding & Recommendation

* Uses `sentence-transformers/msmarco-MiniLM-L12-cos-v5` to embed job descriptions and items.
* Cosine similarity used to rank assessments.

```python
recommend_assessments(query="Need a data engineer skilled in Python", max_duration=60)
```

Returns top N recommended assessments.

---

### 🎛️ 4. Gradio Web App

A simple UI with input box and table view using:

```python
gr.Interface(...)
```

Run locally with `python app_gradio.py` or use [live link](https://9f9c78fa70f32f940e.gradio.live).

---

### 🌐 5. Flask REST API + Ngrok

Endpoints:

* `GET /shl-ai-apis/health`
* `POST /shl-ai-apis/recommend`

Exposes model via public URL using `pyngrok`.

---

### 📏 6. Evaluation Script

* `evaluate_model()` uses test cases and metrics:

  * `calculate_recall_at_k()`
  * `calculate_map_at_k()`

---

## 🧰 Technologies Used

* **Python 3.8+**
* **BeautifulSoup** – Web scraping
* **Requests** – HTTP client
* **Pandas / NumPy** – Data processing
* **Sentence-Transformers** – Text embeddings
* **Gradio** – Web UI
* **Flask + Ngrok** – REST API deployment
* **ThreadPoolExecutor** – Multithreading

---

## ✅ Final Output Files

| File                     | Description                               |
| ------------------------ | ----------------------------------------- |
| `shl_data_basic.json`    | Raw scraped data                          |
| `shl_data_enriched.json` | Enriched metadata with all fields         |
| `shl_data_enriched.xlsx` | Excel version of enriched data            |

---


```
