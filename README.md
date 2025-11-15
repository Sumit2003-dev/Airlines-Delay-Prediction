# Airlines-Delay-Prediction
This project aimed to develop a machine learning model to predict the likelihood of a flight being delayed, using only a provided "Airlines" dataset. The secondary objective was to analyze airline-related features to identify the most significant factors contributing to these delays.
1.2 Methodology Overview
A 10-step data science workflow was implemented in Python, utilizing pandas for data manipulation, seaborn/matplotlib for visualization, and scikit-learn for machine learning. The process included data loading, comprehensive cleaning, exploratory data analysis (EDA), feature engineering, and a comparative evaluation of three models: Logistic Regression, Decision Tree, and Random Forest.
1.3 Key Findings & Model Performance
The dataset was found to be imbalanced, with approximately 34.4% of flights flagged as "Delayed" (1) and 65.6% as "On-Time" (0).
Among the trained models, the Random Forest Classifier provided the best and most robust performance. It was the most effective at balancing the trade-off between precision and recall, making it the most reliable predictor for the minority "Delayed" class.
Feature importance analysis (Step 8) identified the Flight number, departure Time, and flight Length as the top three predictors. Engineered features like Departure_Hour also ranked highly, confirming that time-of-day is a critical factor in delay prediction.

