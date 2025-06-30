# Patient-Centered Communication in Cancer Research Analysis

## Overview

This project provides a pipeline for extracting, processing, and analyzing scientific publication data related to patient-centered communication in cancer research. The analysis aims to identify emerging topics and assess author sentiment across different communication themes in oncology research.

## Research Questions

1. Which patient-centered communication topics emerge most strongly in oncology research?
2. How does author sentiment vary across those topics?

## Project Structure

```
practicum/
├── 1_data_retrieval/      # Data extraction and web scraping
├── 2_data_processing/     # Data cleaning and preprocessing
├── 3_analysis/            # Topic modeling and sentiment analysis
├── requirements.txt       # Project dependencies
└── README.md              # This file
```

## Installation

### Option 1: Automated Setup (Recommended)

1. **Install Python (3.12 or earlier)**

2. **Install required packages:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Download NLTK data:**
   ```python
   import nltk
   nltk.download('punkt')
   nltk.download('stopwords')
   nltk.download('wordnet')
   ```

3. **Install Chrome WebDriver for web scraping:**
   - macOS: `brew install chromedriver`
   - Other platforms: Download from https://chromedriver.chromium.org/

## Usage

### Component 1: Data Retrieval

**Module:** `1_data_retrieval/`

#### Publication Data Extraction (`xdd_relpub_retrieval.py`)
- Retrieves publication snippets from xDD API
- Filters results for cancer-related patient communication studies (2020+)
- Outputs: `data/xdd_pubs.json`


#### Web Scraping (`pub_crawler.py`)
- Scrapes full-text content from DOI URLs
- Extracts structured sections (abstract, methods, results, conclusions)
- Requires Chrome WebDriver for Selenium
- Outputs: HTML files in `data/pages/`


### Component 2: Data Processing

**Module:** `2_data_processing/`

#### HTML Parsing (`html_parse_clean.py`)
- Converts HTML files to structured JSON
- Standardizes section headers
- Removes citations and formatting artifacts
- Outputs: `data/pub_content_all.json`, `data/pub_content_filtered.json`


#### Text Preprocessing (`preprocessing.py`)
- Tokenizes and normalizes text
- Removes stopwords and domain-specific terms
- Applies lemmatization and POS filtering
- Outputs: `data/clean_pub_content_filtered.json`


### Component 3: Analysis

**Module:** `3_analysis/`

#### Exploratory Data Analysis (`pub_analysis.ipynb` and `analysis.py`)
- Computes token and n-gram frequency distributions
- Generates visualizations and reports

#### Topic Modeling (`pub_analysis.ipynb` and `analysis.py`)
- Applies BERTopic for topic discovery
- Generates visualizations and reports

#### Sentiment Analysis (`pub_analysis.ipynb` and `analysis.py`)
- Performs sentiment analysis using DistilBERT
- Generates visualizations and reports

#### Combined Topic Modeling and Sentiment Analysis (`pub_analysis.ipynb` and `analysis.py`)
- Performs combined topic modeling and sentiment analysis approach using BERTopic and DistilBERT
- Generates visualizations and reports

## Key Features

### Topic Modeling
- **BERTopic**: Topic clustering using BERT embeddings, sklearn CountVectorizer, UMAP, and HDBSCAN
- **Visualization**: Word clouds, interactive plots, and topic distribution charts
- **Multi-word Themes**: N-gram extraction for medical terminology

### Sentiment Analysis
- Uses DistilBERT fine-tuned on SST-2 dataset
- Classifies documents as positive/negative with confidence scores
- Aggregates sentiment by topic

## Configuration

### API Parameters (APIConfig)
- **Search terms**: "patient-centered communication" + "cancer"
- **Date range**: Publications from January 2020 to June 2025
- **Result limit**: 1000 publications maximum
- **Minimum hits**: 2+ occurrences of both search terms

### Text Processing (TextProcessing)
- **Target sections**: Abstract, introduction, methods, results, recent_findings, discussion, and conclusions
- **Text cleaning**: Citation removal, text normalization
- **Stop word removal**: Domain-specific and general stop words
- **Minimum word length**: 4 characters

### Topic Modeling (TopicModeling)
- **BERTopic**: Configurable n-gram ranges, clustering parameters
- **Coherence metric**: C_v (correlates best with human interpretation)
- **Advanced n-gram extraction**: 2-4 word phrases for medical terminology

### Sentiment Analysis (SentimentConfig)
- **Model**: DistilBERT fine-tuned on SST-2

### Web Scraping (ScrapingConfig)
- **Request delays**: 2 seconds between requests

## Output Files

| File | Description |
|------|-------------|
| `xdd_pubs.json` | Raw publication metadata from API |
| `pub_content_all.json` | Parsed content from all sections |
| `pub_content_filtered.json` | Filtered content (conclusions only) |
| `clean_pub_content_filtered.json` | Preprocessed text ready for analysis |
| `no_content_doi_list.txt` | Failed scraping attempts log |

## Identified Limitations
1. **Web Scraping**: Some publishers block automated access (paywalls to access full-text)
2. **Content Filtering**: May miss relevant content in non-standard formats
3. **Language**: English publications only
4. **Temporal Scope**: Limited to 01/2020-06/2025 publications