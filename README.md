# Summer Challenge on Automatic Evaluation of Handwritten Answersheets, under NCVPRIPG'24 - An OCR-Based Question Answer Extraction

## Overview

This repository contains a Python script that processes images containing questions and extracts answers using Optical Character Recognition (OCR) techniques. The script uses EasyOCR to detect text in images, processes the detected text to extract answers, and compares the extracted answers with ground truth to calculate accuracy and Mean Absolute Error (MAE).

## Challenge Details
https://vl2g.github.io/challenges/AutoEval2024/#leaderboard

## Requirements

- Python 3.6+
- EasyOCR
- OpenCV
- FuzzyWuzzy
- Scikit-learn
- TQDM
- Pytesseract
- Pandas

## Installation

1. Clone this repository:
    ```bash
    git clone <repository_url>
    cd <repository_folder>
    ```

2. Create a virtual environment and activate it:
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

3. Install the required packages:
    ```bash
    pip install easyocr opencv-python fuzzywuzzy scikit-learn tqdm pytesseract pandas
    ```

4. Ensure that Tesseract is installed on your system. Refer to [Tesseract Installation Guide](https://github.com/tesseract-ocr/tesseract) for instructions.

## Usage

1. Place your images in a folder (e.g., `./Sample_Data/Sample_Data`).

2. Prepare a CSV file containing the ground truth answers (`ModelAnswer.csv`).

3. Run the script:
    ```bash
    python script.py
    ```

## Script Description

The script performs the following steps:

1. **Initialize EasyOCR Reader**: The EasyOCR reader is initialized to use GPU.

2. **OCR Transformation**: The `easyocr_transformation` function reads an image, detects text, and returns a DataFrame containing the detected text along with their bounding box coordinates.

3. **Hybrid Similarity**: The `hybrid_similarity` function calculates a similarity measure between two texts using both cosine similarity and fuzzy matching.

4. **Comparison to True/False**: The `compare_to_true_false` function compares detected text with 'TRUE' and 'FALSE' using hybrid similarity.

5. **Similarity Check**: The `is_similar` function checks if a text is similar to any target strings using fuzzy matching.

6. **Answer Extraction**: The `clean_and_extract_answers` function processes the detected text to extract answers and organizes them into a DataFrame.

7. **Handling Unknown Answers**: The `handle_unknown_answers` function processes 'UNKNOWN' answers by converting them to 'TRUE' or 'FALSE' based on similarity.

8. **Accuracy Calculation**: The `calculate_accuracy` function calculates the accuracy of the extracted answers compared to the ground truth.

9. **MAE Calculation**: The `calculate_mae` function calculates the Mean Absolute Error (MAE) of the extracted answers compared to the ground truth.

10. **Processing All Images**: The `process_and_save_cleaned_answers_for_all_images` function processes all images, cleans the answers, saves them, and calculates accuracy and MAE.

11. **Image Orientation Correction**: The `get_image_orientation` and `correct_image_orientation` functions correct the orientation of an image using Tesseract's OSD.

## Output

- `leaderboard_results.csv`: Contains the filenames and predicted marks for each image.
- `leaderboard_results_all_metrics.csv`: Contains the filenames, predicted marks, MAE, and accuracy for each image.

## Example

```python
# Load ground truth model answers
ground_truth = pd.read_csv('./ModelAnswer.csv')

# Folder containing images
image_folder = './Sample_Data/Sample_Data'  # Replace with the path to your folder

# List of all image files in the folder
image_paths = [os.path.join(image_folder, f) for f in os.listdir(image_folder) if f.endswith('.jpg')]

# Process all images, clean answers, save them, and calculate accuracy and MAE
process_and_save_cleaned_answers_for_all_images(image_paths, ground_truth)
