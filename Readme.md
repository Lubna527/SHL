# 🔍 SHL Assessment Recommender System

This project automates the **scraping, enrichment, recommendation, and serving** of SHL assessments using NLP techniques and web APIs. It allows recruiters and hiring managers to input job descriptions and get relevant SHL assessment recommendations in seconds.

> 🚀 **Live Demo**: [Gradio App](https://9f9c78fa70f32f940e.gradio.live)

---

## 📌 Features

* ✅ Scrapes SHL product catalog (courses & entities)
* ✅ Enriches each item with job levels, duration, languages, etc.
* ✅ Saves data in `.json` and `.xlsx` formats
* ✅ Uses sentence embeddings for semantic recommendation
* ✅ Evaluates model with Recall\@K and MAP\@K
* ✅ Interactive UI (Gradio) + REST API (Flask + Ngrok)

---


## 🔧 Setup Instructions

1. **Install dependencies**:

```bash
pip install fastapi nest-asyncio pyngrok uvicorn scikit-learn flask gradio sentence-transformers numpy
```

2. **Run the scraper and enrichment scripts** to generate `shl_data_enriched.json`.

3. **Launch the Gradio App**:

```bash
python app_gradio.py
```

Or use the hosted version here: 👉 [https://9f9c78fa70f32f940e.gradio.live](https://9f9c78fa70f32f940e.gradio.live)

4. **Launch the Flask API**:

```bash

---

## 📡 API Endpoints

| Endpoint                 | Method | Description                                         |
| ------------------------ | ------ | --------------------------------------------------- |
| `/shl-ai-apis/health`    | GET    | Health check                                        |
| `/shl-ai-apis/recommend` | POST   | Get top 10 recommended assessments based on a query |

### Sample Request

```json
POST /shl-ai-apis/recommend
{
  "query": "Looking for a data analyst skilled in Python and Excel. Duration 45 minutes."
}
```

```python
#Run this code
import requests
import json

# Making a GET request to the health endpoint
res = requests.get("https://f540-35-199-157-109.ngrok-free.app/shl-ai-apis/health")
print(res.status_code)
# Pretty print the JSON response from the health check endpoint
print(json.dumps(res.json(), indent=4))

# Query to send in the POST request
query = {
    "query": "I want to hire a content writer who is good at SEO and English"
}

# Making a POST request to the recommend endpoint
res = requests.post("https://f540-35-199-157-109.ngrok-free.app/shl-ai-apis/recommend", json=query)
print(res.status_code)
# Pretty print the JSON response from the recommend endpoint
print(json.dumps(res.json(), indent=4))
```
Returns top assessments matching the request.

---

## 🧪 Evaluation Metrics

Included test suite evaluates:

* **Recall\@K**
* **MAP\@K**

---

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


