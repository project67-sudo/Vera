# VERA – Changelog

This file summarizes the development work documented for **Milestone 2** of VERA – AI-Powered News Verification System.

> **Baseline note:** The Milestone 1 source snapshot was not available for a direct file-by-file comparison. The entries below describe the Milestone 2 project state and documented development work. They distinguish features present in the Flask application from newer standalone experiments; they should not be read as proof that every listed file was newly created after Milestone 1.

## Milestone 2 – 22 November 2026

### Application and user interface
- Continued development of the Flask-based web application.
- Organized the application around a browser interface (`index.html`, `login.html`) and Flask routes in `app.py`.
- Retained SQLite-based account storage through the login-related code.
- Exposed feature routes for news retrieval, summarization, recommendations, sentiment, coverage checking, clustering, article comparison and image checking.

### News analysis modules
- Maintained article retrieval using NewsAPI and Google News RSS.
- Included article extraction using `newspaper3k`, Requests and BeautifulSoup fallback logic.
- Included article comparison using text extraction and similarity analysis.
- Continued the extractive summarization module.
- Continued sentiment analysis using available sentiment/NLP utilities.
- Included coverage-based news checking that compares an input article with related reporting. Results are framed as corroboration/coverage signals, not proof of truth.
- Included story clustering using TF-IDF and DBSCAN in the integrated application path.
- Included reverse-image search through SerpAPI / Google Lens.

### V2 and V3 module development
- Added or maintained separate V2/V3 implementations for selected modules:
  - Summarization: `summary_v2.py`, `summary_v3.py`
  - Story clustering: `story_clusterer_v2.py`, `story_clusterer_v3.py`
  - News recommendation: `hybrid_recommender_v3.py`
  - Fake-news classification experiments: `fake_news_detection_v2.py`, `fake_news_detection_v3.py`
- The V3 summarizer uses sentence scoring and redundancy reduction.
- The V3 clustering module supports semantic embeddings with a TF-IDF fallback and duplicate/noise handling.
- The V3 recommender combines text relevance, semantic relevance, freshness and content-quality signals.
- The V3 classifier supports a supervised TF-IDF pipeline with candidate classifiers and confidence-based `UNVERIFIED` handling.
- These newer files are standalone implementations unless explicitly connected to the Flask routes. The running application should not be described as using V2/V3 automatically without verifying its imports and route calls.

### Prompt development and testing
- Recorded prompt iterations for four selected development tasks: P5 (summarization), P6 (story clustering), P7 (recommendation) and P13 (fake-news classification).
- Documented V1, V2 and V3 prompt versions, including the intended changes, reasons and observed results where available.
- Added test evidence for summarization, clustering, recommendation and fake-news classification.
- Recorded both successful and partial/failing outcomes rather than presenting all tests as passes.
- The project uses classical NLP and machine-learning methods at runtime; ChatGPT prompts were used during development and are not runtime LLM prompts.

### Documentation and delivery
- Prepared a project README with setup, configuration, feature and API-route descriptions.
- Prepared system architecture, workflow, module and data-flow diagrams.
- Updated the Milestone 2 report with implementation details, prompt history, testing evidence, limitations and team contributions.
- Added a supporting technical report.

### Known limitations carried into Milestone 2
- Similarity and matching coverage do not establish factual truth.
- Article extraction can fail on paywalled, bot-protected or dynamically rendered websites.
- Sentiment results depend on the quality and domain fit of the labels/tools.
- Clustering thresholds can be sensitive to short texts and paraphrases.
- The V3 fake-news classifier requires a suitable labelled dataset and a trained model; synthetic examples alone do not establish real-world accuracy.
- Live end-to-end testing depends on valid API credentials and external service availability.
- V2/V3 modules require integration and end-to-end validation before being treated as part of the deployed application.

## Repository maintenance / setup notes
- Keep API keys and secrets outside source control; configure them through environment variables.
- Do not commit `.env`, passwords or private user databases.
- Keep test inputs and outputs, prompt versions, diagrams and the report in the repository's documentation/test-data structure.

---
**Project:** VERA – AI-Powered News Verification System  
**Team ID:** PE-B13-38  
**Team:** VERA  
**Batch:** B13-24
