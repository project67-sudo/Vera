# VERA – AI-Powered News Verification System

**VERA** is a college mini-project that brings several news-analysis tools into one Flask web application. It helps users explore news coverage, summarize articles, compare stories, inspect sentiment, group related articles, and investigate images using reverse-image search.

> **Important:** VERA is an analysis and corroboration aid, not a definitive fact-checker. Similarity, sentiment, model predictions, or the presence/absence of matching coverage do not prove whether a news claim is true or false.

## 1. Main Features

- **News feed:** Retrieves headline data by category using NewsAPI.
- **Article summarization:** Extracts article text and creates an offline, extractive summary.
- **News recommendation:** Retrieves Google News RSS results and ranks relevant articles.
- **Sentiment analysis:** Estimates sentiment using available sentiment tools and related-news signals.
- **Coverage / credibility check:** Looks for related coverage and compares text similarity. Lack of matching coverage should be treated as unverifiable, not automatically fake.
- **Story clustering:** Groups submitted article texts using TF-IDF and DBSCAN in the integrated V1 path. Separate V2/V3 modules provide more advanced clustering experiments.
- **Article comparison:** Extracts multiple articles and calculates similarity information.
- **Reverse-image search:** Uses Google Lens results through SerpAPI to find visually similar images and pages.
- **User accounts:** The project includes SQLite-backed account functionality in its login-related code.

## 2. Technology Stack

- **Language:** Python
- **Web framework:** Flask
- **Frontend:** HTML, CSS and JavaScript (`index.html`, `login.html`)
- **Data storage:** SQLite
- **Text processing / machine learning:** scikit-learn, TF-IDF, cosine similarity, Naive Bayes, Logistic Regression, DBSCAN and related utilities
- **Article extraction:** newspaper3k, Requests and BeautifulSoup
- **News retrieval:** NewsAPI and Google News RSS (`feedparser`)
- **Image search:** SerpAPI / Google Lens
- **Optional sentiment / NLP tools:** VADER, TextBlob and SentenceTransformer where used by the relevant module

## 3. Project Structure

The runnable application files are in `VERA-main/Project_Vera/`.

```text
Project_Vera/
├── app.py                         # Main Flask application and API routes
├── index.html                     # Main web interface
├── login.html                     # Login interface
├── login.py                       # Login-related code
├── news_fetcher.py                # NewsAPI headline retrieval
├── summary.py                     # Integrated extractive summarizer
├── summary_v2.py                  # Experimental V2 summarizer
├── summary_v3.py                  # Standalone V3 summarizer
├── hybrid_recommender.py          # Integrated RSS recommendation module
├── hybrid_recommender_v3.py      # Standalone V3 recommender
├── sentiment.py                   # Sentiment wrapper
├── sentiment_dynamic.py           # Dynamic sentiment analysis
├── falseNewsChecker.py            # Coverage-based credibility check
├── fake_news_detection_v2.py     # V2 ML classifier
├── fake_news_detection_v3.py     # Standalone V3 ML classifier
├── story_clusterer_semantic.py   # Integrated TF-IDF/DBSCAN clusterer
├── story_clusterer_v2.py         # V2 semantic clustering
├── story_clusterer_v3.py         # Standalone V3 clustering
├── ArticleClustering.py           # Additional clustering implementation
├── article_compare.py             # Multi-article comparison
├── false_image_check.py           # Reverse-image search functions
├── all-data.csv                   # Sentiment-related data
├── hybrid_news_dataset.csv        # Recommendation-related dataset
├── requirements.txt               # Python dependencies
└── users.db                        # SQLite database file (if supplied/created)
```

The archive also contains documentation and configuration files outside `Project_Vera/`. Keep the supplied folder structure when opening the project.

## 4. Setup and Installation

### Requirements

- Python 3.10 or newer is recommended.
- Internet access is needed for NewsAPI, Google News RSS, article extraction and SerpAPI features.
- API credentials are required for services that enforce API access.

### Create a virtual environment

Open a terminal in `VERA-main/Project_Vera/`:

```bash
python -m venv .venv
```

Activate it:

**Windows PowerShell**
```powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**
```bat
.venv\Scripts\activate
```

**macOS / Linux**
```bash
source .venv/bin/activate
```

### Install dependencies

The supplied `requirements.txt` should be checked before use: its final dependency line combines `bcrypt` and `flask-cors` without a line break. Install the dependencies with:

```bash
python -m pip install Flask requests newsapi-python newspaper3k scikit-learn pandas numpy scipy nltk lxml beautifulsoup4 feedparser tldextract Pillow cssselect bcrypt flask-cors
```

If newspaper3k or lxml reports a platform-specific installation error, install the required system/build dependencies for your operating system and retry.

## 5. Configuration and API Keys

Configure credentials as environment variables. Do not commit real keys, passwords, database files containing personal information, or private configuration files to a public repository.

Example for **Windows PowerShell**:

```powershell
$env:NEWSAPI_KEY="your_newsapi_key"
$env:SERPAPI_KEY="your_serpapi_key"
$env:FLASK_SECRET="replace_with_a_long_random_secret"
```

Example for **macOS / Linux**:

```bash
export NEWSAPI_KEY="your_newsapi_key"
export SERPAPI_KEY="your_serpapi_key"
export FLASK_SECRET="replace_with_a_long_random_secret"
```

The application reads `NEWSAPI_KEY` and `SERPAPI_KEY` from the environment in `app.py`. The login application code also supports `FLASK_SECRET`.

**Security action before publishing:** inspect the supplied source archive and remove any embedded API credentials. If a real key has ever been included in a shared ZIP, source file, screenshot or public repository, revoke/rotate it with the provider and replace it with an environment-variable lookup. Do not copy a key from the supplied source into this README.

## 6. Run the Application

From the `Project_Vera/` directory, run:

```bash
python app.py
```

Then open:

```text
http://127.0.0.1:5000
```

Stop the local server with `Ctrl+C`.

Run the application from the directory containing `app.py` so that the HTML files, CSV files and local database paths resolve as expected.

## 7. API Routes

The Flask application defines the following routes:

| Route | Method | Purpose |
|---|---|---|
| `/` | GET | Main news page |
| `/category/<category>` | GET | News by category |
| `/api/summary` | POST | Summarize an article URL |
| `/api/recommend` | POST | Recommend news for a query |
| `/api/sentiment` | POST | Analyze sentiment |
| `/api/analyze` | POST | Run a news analysis operation |
| `/api/image_check` | POST | Reverse-image investigation |
| `/api/cluster` | POST | Cluster article text |
| `/api/compare_articles` | POST | Compare multiple articles |
| `/api/latest_news` | GET | Retrieve latest news data |
| `/api/false_news` | POST | Check related news coverage |
| `/api/false_image` | POST | Run image-related checking |

The request fields and response structures are implemented in `app.py` and the corresponding module functions. Refer to those files when integrating another client or preparing API tests.

## 8. V1, V2 and V3 Implementations

The repository contains multiple generations of selected modules:

- **V1:** Original modules used by the integrated application routes.
- **V2:** Improved standalone implementations for selected tasks.
- **V3:** Further-developed standalone modules with additional handling, scoring, metrics or model options.

The presence of a V2 or V3 file does **not** mean that version is automatically used by the running Flask application. The current `app.py` imports the V1 module names for several features. Check the imports and route functions before claiming a V2/V3 module is integrated.

The V3 fake-news classifier requires a labelled CSV dataset with `text` and `label` columns and a trained model before it can make model-based predictions. A prediction confidence is not proof of factual correctness. The standalone V3 modules should be tested separately unless they have been explicitly connected to the Flask routes.

## 9. Data and Testing

- `all-data.csv` is included as a sentiment-related dataset.
- `hybrid_news_dataset.csv` is included for recommendation-related functionality.
- Use real article URLs and text samples when testing extraction, summarization, comparison and clustering.
- Test empty input, invalid URLs, unavailable websites, duplicate articles, short text, missing data and unavailable API credentials.
- Record expected output, actual output and Pass/Fail status for each test.
- Live API-dependent features require valid credentials and network access; offline fixture tests do not demonstrate live-service availability.

## 10. Limitations

- Article extraction can fail on paywalled, bot-protected or dynamically rendered websites.
- Search results and available news coverage depend on external providers and network availability.
- Similarity-based corroboration cannot establish factual truth.
- Sentiment estimates may be affected by domain-specific language and weak labels.
- Clustering quality depends on text length, model availability and thresholds.
- Reverse-image search finds matches; it does not by itself determine whether an image has been manipulated or identify its original publication.
- V2/V3 standalone modules require separate integration and end-to-end testing before they can be described as part of the live application.

## 11. Team

**Project:** VERA – AI-Powered News Verification System  
**Team ID:** PE-B13-38  
**Team name:** VERA  
**Batch:** B13-24  
**Faculty mentor:** Indrajeet Gupta

| Member | Enrollment number |
|---|---|
| Krish Astwal | S24CSEU1383 |
| Aryan Singh | S24CSEU1386 |
| Arpit Tyagi | S24CSEU1304 |
| Anay Singh | S24CSEU0630 |
| Harshit Raj | S24CSEU0734 |
| Jatin Sheoran | S24CSEU0673 |

## 12. Project Disclaimer

VERA is an educational prototype developed as a college mini-project. It provides news analysis, similarity checks and supporting information to help users investigate news items. It should not be treated as an authoritative fact-checking service or used as the sole basis for important decisions.
