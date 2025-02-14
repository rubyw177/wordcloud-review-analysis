# Word Cloud Analysis

## Overview
This project performs **word cloud analysis** on textual data extracted from an **Excel file containing user reviews**. The goal is to **analyze user sentiments** by classifying reviews and visualizing frequently occurring words in **GOOD** and **BAD** reviews. The project also supports **automatic translation** of non-English reviews to ensure consistent analysis.

## Features
- Reads and processes an **Excel file** containing user reviews.
- Classifies reviews into **"GOOD"** and **"BAD"** categories based on **star ratings**.
- Translates non-English reviews using **Deep Translator**.
- Cleans and preprocesses text by **removing stopwords and punctuations**.
- Generates **word clouds** to visualize the most frequently used words.
- Allows customization of the word cloud **shape** using a black-and-white **mask image**.

## Dependencies
Ensure you have the required Python libraries installed before running the notebook:
```bash
pip install deep_translator wordcloud pandas matplotlib numpy openpyxl
```

## Process Flow
### **Step 1: Load Data**
The first step is to load the review dataset from an **Excel file**.
```python
import pandas as pd
review_file_name = "reviews.xlsx"
df = pd.read_excel(review_file_name)
```

### **Step 2: Review Classification**
Reviews are classified based on **star ratings**:
- **GOOD reviews**: Ratings ≥ 4.
- **BAD reviews**: Ratings ≤ 2.

```python
good_review_min_star = 4
bad_review_max_star = 2

df['Sentiment'] = df['Star'].apply(lambda x: 'GOOD' if x >= good_review_min_star else ('BAD' if x <= bad_review_max_star else 'NEUTRAL'))
```

### **Step 3: Translation of Non-English Reviews**
To ensure a uniform analysis, non-English reviews are translated into English using **Deep Translator**.
```python
from deep_translator import GoogleTranslator

def translate_text(text):
    return GoogleTranslator(source='auto', target='en').translate(text)

df['Translated_Review'] = df['Review'].apply(translate_text)
```

### **Step 4: Text Preprocessing**
To improve analysis, text preprocessing removes **punctuations, stopwords, and converts text to lowercase**.
```python
import re
from wordcloud import STOPWORDS

def clean_text(text):
    text = re.sub(r'[^a-zA-Z ]', '', text)  # Remove special characters and numbers
    text = text.lower()  # Convert to lowercase
    words = text.split()
    words = [word for word in words if word not in STOPWORDS]  # Remove stopwords
    return ' '.join(words)

df['Cleaned_Review'] = df['Translated_Review'].apply(clean_text)
```

### **Step 5: Generate Word Clouds**
Word clouds are generated separately for **GOOD** and **BAD** reviews using a **mask image** (black-and-white `.jpg` format).
```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
from PIL import Image
import numpy as np

wc_image = "mask_image.jpg"  # Black-and-white mask image
mask = np.array(Image.open(wc_image))

def generate_wordcloud(text, title):
    wordcloud = WordCloud(width=800, height=400, background_color='white', mask=mask).generate(text)
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis("off")
    plt.title(title)
    plt.show()

generate_wordcloud(" ".join(df[df['Sentiment'] == 'GOOD']['Cleaned_Review']), "Good Reviews Word Cloud")
generate_wordcloud(" ".join(df[df['Sentiment'] == 'BAD']['Cleaned_Review']), "Bad Reviews Word Cloud")
```

## How to Use the Notebook
1. **Set the path** to the Excel file containing review data:
   ```python
   review_file_name = "path/to/excel_file.xlsx"
   ```
2. **Define thresholds** for classifying reviews:
   ```python
   good_review_min_star = 4
   bad_review_max_star = 2
   ```
3. **Specify the word cloud mask image** (must be black-and-white `.jpg`):
   ```python
   wc_image = "path/to/image.jpg"
   ```
4. **Run the notebook** to process data and generate word clouds.

## Key Takeaways
- **Word clouds help identify key themes** in positive and negative reviews.
- **Preprocessing removes irrelevant words** to improve visualization clarity.
- **Translation enables analysis across different languages** for global datasets.
- **Customization with mask images enhances visualization appeal**.
