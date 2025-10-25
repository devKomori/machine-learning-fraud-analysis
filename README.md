# Analysis of Machine Learning Models for Phishing and Fraud Detection

## Introduction

A phishing email is a fraudulent message from a cybercriminal who impersonates a trusted source to trick you into revealing sensitive information, such as passwords, credit card numbers, or bank details. In this report predictions will be made to distiguish if an email is phishing/fraudulent, or a real email.

The goal of this project is to compare different machine learning models that can distinguish between phishing and legitimate emails based on their content.
We plan to use the columns `subject`, `body`, `sender`, and `urls` to predict the `label` column, which indicates whether an email is phishing or legitimate.

## Data Selection

This project is using the [Phishing Email Dataset](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset) from Kaggle. This dataset contains approximately 82,500 emails, with 42,891 labeled as phishing and 39,595 as legitimate. The emails are sourced from Enron, Ling, CEAS, Nazario, SpamAssassin, and Nigerian Fraud emails. For this project, we will focus on the CEAS subset, as it provides more relevant features and is less dated.

Both significant preprocessing and feature engineering were performed on the data:

    Data Munging:
        The receiver and date columns were dropped, as they contained many missing values and/or were deemed to have low predictive impact.
        Missing values in the subject column (28 instances) were filled with empty strings.
        The sender, subject, and urls columns were converted to string types for consistent processing.
    
    Feature Engineering:
        n_urls: A new feature was created to count the total number of URLs found in an email's subject, body, and urls fields.
        subject_len: A feature for the character length of the subject line.
        body_len: A feature for the character length of the email body.
        predictor: To prepare the data for vectorization, a single text feature was created by combining the content of the subject, body, sender, and urls columns.

## Methods
    
**Tools Used:**
<br>
Python, Jupyter Notebook, Pandas, Matplotlib, and Seaborn.<br>
Scikit-learn was the primary library used for machine learning, including:


    TfidfVectorizer for text data vectorization.

    train_test_split for creating training and test sets.

    Metrics: accuracy_score, classification_report, confusion_matrix, roc_curve, roc_auc_score, and cohen_kappa_score.

    XGBoost was also used as an additional powerful ensemble model.

Keeping in mind that the core of this project is text classification, we chose to use the following tools/models:
    
    TfidfVectorizer was chosen over a simple count vectorizer because it weights words based on their importance (term frequency-inverse document frequency) rather than just their raw count. This is highly effective for text classification, as it surfaces words that are unique and important to a specific class (e.g., "phishing").

    A diverse set of six classification models was chosen to compare their performance, speed, and characteristics on this high-dimensional text data:
       
    Multinomial Naive Bayes (MultinomialNB): A classic, fast baseline model that works very well with Term Frequency-Inverse Document Frequency text data.

    Logistic Regression (LogisticRegression): A robust, fast, and highly interpretable linear model that performs well on high-dimensional, sparse data.

    Linear SVC (LinearSVC): Known for its high performance and efficiency on sparse data like TF-IDF vectors. It is often a top performer for text classification.

    K-Nearest Neighbors (KNeighborsClassifier): A non-linear, instance-based model chosen to see if a different decision-making approach would be effective.

    Random Forest (RandomForestClassifier): A powerful tree-based ensemble method that can capture complex, non-linear interactions between features.

    XGBoost (XGBClassifier): A state-of-the-art gradient-boosting ensemble model, chosen for its typically high accuracy.

The models were split into an 80% training set and a 20% test set. Each model's performance was evaluated based on accuracy, training/prediction time, Cohen's Kappa Coefficient, and a detailed classification report (precision/recall/F1-score).

## Results

To speak to the matter of answering our research question, we found that machine learning models, particularly LinearSVC, are extremely effective at distinguishing between phishing and legitimate emails in this dataset. All six models achieved test accuracy scores above 98.9%, with Linear SVC reaching 99.80%.

Here is a summary of the model results on the 20% test set:

| Model               | Test Accuracy | Cohen's Kappa | Training Time (s)  | Prediction Time (s)  |
|---------------------|---------------|---------------|--------------------|----------------------|
| K-Nearest Neighbors | 99.58%        | 0.991         | 0.01               | 10.62                |
| Naive Bayes         | 98.95%        | 0.979         | 0.02               | 0.00                 |
| Logistic Regression | 99.40%        | 0.988         | 0.79               | 0.00                 |
| Random Forest       | 99.35%        | 0.987         | 5.78               | 0.09                 |
| Linear SVC          | 99.80%        | 0.996         | 0.15               | 0.00                 |
| XGBoost             | 99.55%        | 0.991         | 28.83              | 0.04                 |

As a brief explanation of what the metrics mean in the project’s context:

    Accuracy: All models correctly identified the email type (phishing or legit) over 98.9% of the time. Linear SVC was correct 99.8% of the time.

    Precision & Recall (F1-Score): All models achieved scores of ~0.99 or 1.00 for both classes. This means the models are excellent at two critical tasks:

        High Precision: When the model predicts an email is phishing, it is almost certainly correct (minimizing "false positives" that would send a legit email to spam).

        High Recall: The model successfully finds almost all the actual phishing emails (minimizing "false negatives" that would let a phishing email into the inbox).

    Confusion Matrix: The matrices for all models showed very few errors. For example, the Linear SVC model only misclassified 16 out of 7,831 emails.

    ROC/AUC: The AUC (Area Under the Curve) scores for all models were ~0.99, indicating a near-perfect ability to distinguish between the two classes.

    Cohen's Kappa Coefficient: All models scored >0.97, which represents "almost perfect" agreement between the predictions and the actual labels, showing that the high accuracy isn't just due to a lucky guess or class imbalance.

    Time: While XGBoost and Random Forest were highly accurate, they were the slowest to train. KNN was fast to train but exceptionally slow at prediction. Linear SVC, Logistic Regression, and Naive Bayes provided the best balance of speed and top-tier accuracy.

## Discussion

Our results strongly imply that the textual content of phishing emails contains clear, detectable patterns that machine learning models can learn. The high performance of relatively simple linear models like Linear SVC and Logistic Regression is significant. It shows that robust and highly accurate phishing detection can be achieved with models that are also incredibly fast to train and deploy, making them practical for real-world applications.

Some perspectives for future research are:

    Modern Datasets: The CEAS dataset is from 2008. Phishing attacks have evolved significantly, using more sophisticated social engineering, better grammar, and image-based tactics. Future research must test these models on a modern dataset (e.g., 2023-2025) to assess their real-world performance today.

    Feature Expansion: This model relied only on text. Future models could be improved by incorporating other features, such as email header metadata, sender domain reputation, IP geolocation, and even image analysis (using OCR or computer vision) to detect logos or links hidden in images.

What would you do differently if you had more time or a larger dataset?

    Hyperparameter Tuning: Given more time, we would perform hyperparameter tuning (e.g., GridSearchCV) on all models, especially XGBoost and Random Forest, to see if their performance could be optimized further.

    Deep Learning: With a larger, more modern dataset, we would explore deep learning models for text classification (like LSTMs or Transformers/BERT) to see if they can capture more nuanced semantic meanings and deception tactics that TF-IDF might miss.

## Summary

This project successfully built and compared six machine learning models for the task of phishing email detection.

The most important finding is that all models, from simple baselines like Naive Bayes to complex ensembles like XGBoost, were highly effective, achieving over 98.9% accuracy on the test set. The Linear SVC model emerged as the top performer, delivering the highest test accuracy (99.80%) and Cohen's Kappa score (0.996) while also being one of the fastest models to train (0.15 seconds).

This demonstrates that machine learning, and specifically text vectorization with TfidfVectorizer paired with linear models, is a powerful, efficient, and highly accurate tool for combating email fraud.
