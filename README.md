### **Practical 1: Web Scraping**
**Aim:** Scrape data from a webpage and store it into a CSV format.

import requests
from bs4 import BeautifulSoup
import pandas as pd

# Scrape quotes
url = 'https://quotes.toscrape.com/'
soup = BeautifulSoup(requests.get(url).text, 'html.parser')

quotes = [q.text for q in soup.find_all('span', class_='text')]
authors = [a.text for a in soup.find_all('small', class_='author')]

# Save to CSV
df = pd.DataFrame({'Quote': quotes, 'Author': authors})
df.to_csv('scraped_quotes.csv', index=False)
display(df.head())

---
### **Practical 2: Sentiment Analysis**
**Aim:** Implementation of Sentiment Analysis.

import nltk
from nltk.sentiment import SentimentIntensityAnalyzer

nltk.download('vader_lexicon', quiet=True)
sia = SentimentIntensityAnalyzer()

sentences = [
    "I absolutely love learning Natural Language Processing!",
    "The weather today is extremely gloomy and depressing.",
    "We had a normal day at the office."
]

for s in sentences:
    score = sia.polarity_scores(s)['compound']
    sentiment = "Positive" if score >= 0.05 else "Negative" if score <= -0.05 else "Neutral"
    print(f'"{s}" -> {sentiment} ({score})')

---
### **Practical 3: Text Preprocessing**
**Aim:** Implementation of standard Text Preprocessing techniques (Tokenization, Lowercasing, Stopwords Removal, Stemming, and Lemmatization).

import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer, WordNetLemmatizer

nltk.download(['punkt', 'punkt_tab', 'stopwords', 'wordnet', 'omw-1.4'], quiet=True)

text = "The quick brown foxes are jumping over the lazy dogs."

# 1. Lowercasing
lowered = text.lower()
print("Lowercased:", lowered)

# 2. Tokenization
tokens = [w for w in word_tokenize(lowered) if w.isalnum()]
print("Tokens:", tokens)

# 3. Stopwords Removal
stop_words = set(stopwords.words('english'))
filtered = [w for w in tokens if w not in stop_words]
print("Stop Words Removed:", filtered)

# 4. Stemming & Lemmatization
stemmer = PorterStemmer()
lemmatizer = WordNetLemmatizer()
print("Stemmed:", [stemmer.stem(w) for w in filtered])
print("Lemmatized:", [lemmatizer.lemmatize(w) for w in filtered])

---
### **Practical 4: Parser in NLP**
**Aim:** Demonstrate parsing using parsers like NLTK's Chart/Recursive Descent Parser or SpaCy Dependency Parser.

import nltk
import spacy
from spacy import displacy

# 1. NLTK CFG Parsing
grammar = nltk.CFG.fromstring("""
  S -> NP VP
  VP -> V NP | V NP PP
  PP -> P NP
  V -> "saw"
  NP -> "Mary" | Det N | Det N PP
  Det -> "a" | "the"
  N -> "dog" | "park"
  P -> "in"
""")

sentence = "Mary saw a dog in the park".split()
parser = nltk.RecursiveDescentParser(grammar)
print("--- NLTK CFG Parser Trees ---")
for tree in parser.parse(sentence):
    tree.pretty_print()

# 2. SpaCy Dependency Parser Graph
print("\n--- SpaCy Dependency Parse Graph ---")
nlp = spacy.load("en_core_web_sm")
doc = nlp("Mary saw a dog in the park")
displacy.render(doc)

---
### **Practical 5: Feature Extraction Techniques**
**Aim:** Perform Feature Extraction techniques (CountVectorizer/TF-IDF) in an NLP task.

from sklearn.feature_extraction.text import TfidfVectorizer, CountVectorizer
import pandas as pd

corpus = [
    "Natural Language Processing is amazing.",
    "Feature extraction is a crucial step in NLP."
]

# Count Vectorizer
cv = CountVectorizer()
cv_df = pd.DataFrame(cv.fit_transform(corpus).toarray(), columns=cv.get_feature_names_out())
display("CountVectorizer:", cv_df)

# TF-IDF
tfidf = TfidfVectorizer()
tfidf_df = pd.DataFrame(tfidf.fit_transform(corpus).toarray(), columns=tfidf.get_feature_names_out())
display("TF-IDF:", tfidf_df)

---
### **Practical 6: One Hot Encoding**
**Aim:** Demonstrate One Hot Encoding representation of words or documents.

import numpy as np
import pandas as pd

words = "NLP models process text inputs".split()
unique = sorted(list(set(words)))

# Map each word to a one-hot vector
ohe_matrix = np.eye(len(unique))
df_ohe = pd.DataFrame(ohe_matrix, index=unique, columns=unique)
display(df_ohe.loc[words])

---
### **Practical 7: Bag-of-Words (BOW)**
**Aim:** Implement Bag-Of-Words model from scratch and analyze the frequency vectors.

import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer

documents = [
    "the quick brown fox",
    "jumped over the lazy dog"
]

# 1. BoW using Library (scikit-learn)
print("--- Bag of Words (using CountVectorizer Library) ---")
vectorizer = CountVectorizer()
bow_lib = vectorizer.fit_transform(documents).toarray()
df_lib = pd.DataFrame(bow_lib, columns=vectorizer.get_feature_names_out())
display(df_lib)

# 2. BoW from Scratch Logic
print("\n--- Bag of Words (from Scratch Custom Logic) ---")
vocab = sorted(list(set(" ".join(documents).split())))
bow_scratch = [[doc.split().count(word) for word in vocab] for doc in documents]
df_scratch = pd.DataFrame(bow_scratch, columns=vocab)
display(df_scratch)

---
### **Practical 8: N-Grams**
**Aim:** Build Character-level or Word-level Bigrams, Trigrams, and N-grams.

def get_ngrams(text, n):
    words = text.split()
    return [" ".join(words[i:i+n]) for i in range(len(words) - n + 1)]

sample = "Natural Language Processing is a subset of AI"
print("Bigrams (N=2):", get_ngrams(sample, 2))
print("Trigrams (N=3):", get_ngrams(sample, 3))
print("Quadgrams (N=4):", get_ngrams(sample, 4))

---
### **Practical 9: Term Frequency-Inverse Document Frequency (TF-IDF)**
**Aim:** Implement TF-IDF calculations to assign importance scores to terms.

import math
import pandas as pd

docs = ["nlp is great", "computers understand nlp", "great computers"]
unique_words = set(" ".join(docs).split())

# TF-IDF calculation
tfidf_data = []
for doc in docs:
    words = doc.split()
    scores = {}
    for word in unique_words:
        tf = words.count(word) / len(words)
        df = sum(1 for d in docs if word in d.split())
        idf = math.log(len(docs) / df)
        scores[word] = tf * idf
    tfidf_data.append(scores)

display(pd.DataFrame(tfidf_data))

---
### **Practical 10: Word Embedding in Natural Language Processing**
**Aim:** Demonstrate the working of Word Embeddings in NLP using Gensim's Word2Vec.

from gensim.models import Word2Vec

data = [["natural", "language", "processing", "is", "fun"], ["nlp", "is", "fun"]]
model = Word2Vec(sentences=data, vector_size=10, window=2, min_count=1, workers=1)

print("Vector for 'nlp':", model.wv['nlp'][:5])
print("Most similar to 'nlp':", model.wv.most_similar('nlp', topn=2))
