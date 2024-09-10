# ml-fantacalcio-2024-2025

## Introduction

This project aims to predict the number of **goals** and **assists** for Serie A players in a given season. Using these predictions, the model determines a **fair price** for each player, helping you avoid overpaying during an auction-style **fantasy football** draft.

### Why This Project?

In fantasy football, correctly valuing players is key to building a strong team. This project focuses on **Serie A players**, using **machine learning models** to predict player performance in terms of goals and assists. By combining statistical predictions with a pricing model, we aim to create a method for determining the "right price" for each player, minimizing the risk of overpaying in an auction.

## Intuition Behind the Project

In fantasy football auctions, it's easy to overpay for star players based on past performance or hype. However, what truly matters is their **expected contribution** in the upcoming season. This project tackles that issue by:

- Predicting future performance.
- Calculating the player's worth based on data-driven insights.

By quantifying player stats like **goals** and **assists**, and understanding how much value they bring to the team, we can arrive at a more **rational price** to pay during an auction.

## Core Components of the Project

The main components of this project are:

1. **Scraping real player performance data from FBref**.
2. **Training machine learning models** to predict goals and assists.
3. **Developing a price evaluation method** based on the predicted stats.

---

## 1. FBref Data Scraping

This notebook is responsible for scraping player performance data from the **FBref website**. It gathers detailed information about **Serie A players'** past performances, which will be used for training machine learning models later in the project.

### Key Steps:

1. **Web Scraping Setup**:
   - **Selenium** is used to automate navigation on the FBref website.
   - **BeautifulSoup** parses the HTML content of each player's profile page and extracts performance data.

2. **Data Scraping**:
   - The script collects **Serie A player names** and profile URLs from the main stats page.
   - Detailed performance statistics are scraped from each player's profile page (e.g., goals, assists, minutes played).
   - Data is stored in a **pandas DataFrame** for further analysis.

3. **Saving Data**:
   - The scraped data is saved to a CSV file (`all_player_performance_data.csv`), which will be used for training the models in later steps.

---

## 2. Dataset Cleaning and Creation

This notebook cleans the scraped dataset and creates a new one with **averaged performance metrics**. This cleaned dataset is crucial for training machine learning models to predict future player performance.

### Key Steps:

1. **Loading the Dataset**:
   - Load the raw data (`all_player_performance_data.csv`) from the scraping step.

2. **Cleaning the Data**:
   - Remove rows with invalid or missing values.
   - Ensure only valid season data (in the format YYYY-YYYY) is retained.

3. **Creating the Prediction Dataset**:
   - Compute averages of performance metrics like **goals** and **assists** from past seasons for each player.
   - Filter the dataset to include relevant data for the **2024-2025** season.

4. **Saving the Cleaned Data**:
   - Save the cleaned dataset as `prediction_data_2024_2025.csv`, which will be used for model training.

---

## 3. Model Training and Evaluation

This notebook trains various machine learning models to predict player **goals** based on historical performance data. The best-performing model is selected based on **Mean Squared Error (MSE)**.

### Key Steps:

1. **Data Loading and Preprocessing**:
   - Load the cleaned dataset (`prediction_data_2024_2025.csv`).
   - Use player stats (e.g., minutes played, shots, expected goals) as features (X) and **goals** as the target (y).
   - Use **OneHotEncoder** for linear models and **LabelEncoder** for tree-based models and SVR to process categorical columns.

2. **Model Training**:
   - Split the dataset into **training** and **test sets**.
   - Train various models:
     - Linear Regression
     - Ridge Regression
     - Lasso Regression
     - Decision Tree
     - Random Forest
     - Gradient Boosting
     - XGBoost
     - Support Vector Regression (SVR)

3. **Model Selection**:
   - Evaluate each model using **MSE** on the test set.
   - The **Ridge Regression** model achieved the lowest MSE (9.86) and was selected as the best model.
   - Save the best model as `best_model.pkl`.

---

## 4. Player Valuation

This notebook calculates the **fair price** for each player in a Serie A fantasy football auction based on their predicted **goals** and **assists**.

### Key Steps:

1. **Merging Predictions**:
   - Combine predicted goals (`predictions.csv`) and assists (`predictions_assists.csv`) into a single dataset.

2. **Score Calculation**:
   - Player **Score** is calculated using:
   ```math
   Score = 3 × (Predicted Goals) + (Predicted Assists)
This formula emphasizes **goals** while accounting for **assists**.

### Assigning Cost:
- The top 184 players by score are identified, and their total scores are used to calculate **cost per point**.
- Each player’s cost is based on their score and the overall budget (3720 units).

### Final Output:
- The resulting dataset includes each player’s **name**, **squad**, **predicted goals**, **assists**, **score**, and **assigned cost**.
- Save this data as `sorted_player_scores_with_cost.csv` for use in auctions.

---

## Requirements

To run this project, ensure the following Python libraries are installed:

- **pandas**
- **Selenium**
- **BeautifulSoup**
- **time**
- **webdriver_manager**
- **scikit-learn**
- **xgboost**
- **numpy**
- **re** (for regular expressions)

You'll also need the appropriate **WebDriver** (e.g., **ChromeDriver**) to interact with the FBref website.

---

## Conclusion

This project provides a data-driven approach to **predicting player performance** and **valuing players** in a fantasy football auction. The goal is to help you make smarter decisions during your drafts, avoiding overpayment and securing players at the right price.

Feel free to contribute and suggest improvements! Together, we can refine this project and **dominate the next Fantacalcio auctions**!
