# Web-Scraping-and-Analysis

Scraping 1,000 British Airways reviews from Skytrax and performing NLP-based customer sentiment analysis — a British Airways Data Science internship project.

## Overview

This project is a single Jupyter notebook (`NLP.ipynb`) that covers two stages:

**1. Web scraping**

- Source: [British Airways reviews on Skytrax](https://www.airlinequality.com/airline-reviews/british-airways) (airlinequality.com).
- Method: `requests` fetches 10 paginated listing pages (100 reviews per page), and `BeautifulSoup` (`html.parser`) extracts the review text (from `div.text_content`) and the star rating (from `div.rating-10` → `span[itemprop="ratingValue"]`), skipping 5-star placeholder entries.
- Result: a pandas DataFrame of 1,000 reviews with ratings on a 1–10 scale, saved to `data/BA_reviews.csv`.

**2. Analysis**

- **Data cleaning**: strips the "Trip Verified / Not Verified" prefix from each review and converts the `ratings` column from strings to numeric.
- **NLP text processing** (NLTK): removal of punctuation and numbers, tokenization, lemmatization with `WordNetLemmatizer` (POS-tag aware) and stopword removal.
- **Sentiment analysis**: VADER (`nltk.sentiment.SentimentIntensityAnalyzer`) scores the lemmatized reviews; the compound score (−1 to +1) is grouped as *Positive* (≥ 0.5), *Negative* (< 0) or *Neutral* (otherwise).
- **Visualization** (matplotlib + wordcloud): word cloud of the review corpus, bar chart of rating distribution, pie charts for top-5 ratings and sentiment composition, and a scatter plot comparing star ratings with VADER compound scores.
- **Key findings** (reported in the notebook): ~53.7% of reviews are negative, 19.9% neutral and 25.4% positive; the average rating is 4.256/10 with a standard deviation of ~3.31, i.e. a wide spread of customer experiences.

## Tech Stack

- Python 3 (Jupyter notebook, `ipykernel`)
- `requests` — HTTP requests to the listing pages
- `beautifulsoup4` — HTML parsing
- `pandas` — data storage and manipulation
- `nltk` — tokenization, POS tagging, lemmatization, stopwords, VADER sentiment
- `matplotlib` — charts
- `wordcloud` — word cloud generation
- `tqdm` — imported for progress display

There is no requirements file in the repo; install the packages above directly (see below).

## Project Structure

```
.
├── NLP.ipynb            # End-to-end pipeline: scraping, cleaning, NLP, sentiment analysis, visualizations
├── data/
│   └── BA_reviews.csv   # Scraped dataset: 1,000 British Airways reviews with 1–10 ratings
└── README.md
```

## How to Run

1. Install the dependencies:

   ```bash
   pip install requests beautifulsoup4 pandas nltk matplotlib wordcloud tqdm
   ```

2. Download the NLTK data the notebook needs (the notebook has the download calls commented out, so run this once in the notebook or a Python shell):

   ```python
   import nltk
   nltk.download('all')
   ```

3. Open `NLP.ipynb` in Jupyter (e.g. `jupyter notebook`) and run the cells in order:
   - The first code cells scrape Skytrax live and (re)write `data/BA_reviews.csv`. They require internet access to airlinequality.com. To change the dataset size, adjust `pages` and `page_size` in the scraping cell.
   - The remaining cells operate on the in-memory DataFrame built by those scraping cells (the notebook does not re-read the CSV) and perform the cleaning, NLP processing, VADER sentiment scoring, and the visualizations in one pass.
