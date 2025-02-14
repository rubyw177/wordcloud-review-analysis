# Word Cloud Analysis

## Overview
This project performs word cloud analysis on textual data extracted from an Excel file containing user reviews. The notebook processes the data, translates reviews if necessary, and generates word clouds based on sentiment classification.

## Features
- Reads and processes an Excel file containing user reviews
- Classifies reviews into "GOOD" and "BAD" categories based on star ratings
- Translates non-English reviews using Deep Translator
- Generates word clouds based on sentiment analysis
- Allows for customization of word cloud shape using a black-and-white image

## Dependencies
To run this notebook, ensure you have the following Python packages installed:

```bash
pip install deep_translator wordcloud pandas matplotlib numpy
```

## Usage
1. Set the path to the Excel file containing review data:
   ```python
   review_file_name = "/path/to/your/excel_file.xlsx"
   ```
2. Define the rating thresholds for classification:
   ```python
   good_review_min_star = 4
   bad_review_max_star = 2
   ```
3. Specify the word cloud shape image (must be black-and-white, `.jpg` format):
   ```python
   wc_image = "/path/to/your/image.jpg"
   ```
4. Run the notebook to process data and generate word clouds.

## Example Code
```python
# Insert excel file path to this variable (ONLY .xlsx format)
review_file_name = "/content/ReviewAnalysis.xlsx"

# Define the minimum star rating for a review to be considered "GOOD"
good_review_min_star = 4

# Define the maximum star rating for a review to be considered "BAD"
bad_review_max_star = 2

# Word cloud shape (only black and white image, no grayscale, ONLY .jpg format)
wc_image = "/content/mask_logo.jpg"

!pip install deep_translator wordcloud
```

## Output
The notebook produces visual word clouds representing frequently occurring words in "GOOD" and "BAD" reviews, helping in sentiment analysis and review insights.
