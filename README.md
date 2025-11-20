## Project Description: SMS A2P vs P2P Classifier Deployment

This script (classifier.py) and supporting files train and deploy a Decision Tree model to classify SMS messages as either Application-to-Person (A2P) or Person-to-Person (P2P).

---

### 1. Training and Feature Engineering Logic

1.  **Dependencies**: The script installs required libraries: pandas, numpy, scikit-learn, streamlit, and pyngrok.
2.  **Data Loading**: It prompts the user to upload a CSV file containing the training data (assumed to have columns: 'message' and 'label').
3.  **Numeric Feature Creation**: Four custom numeric features are engineered for each message:
    * `message_length`: Total number of characters.
    * `num_digits`: Count of digits (0-9).
    * `num_upper`: Count of uppercase letters.
    * `num_special`: Count of non-alphanumeric, non-space characters (e.g., $, %, @).
4.  **TF-IDF Vectorization**: Text messages are transformed into numerical feature vectors using **TF-IDF (Term Frequency-Inverse Document Frequency)**, which weights words based on their frequency in a specific message relative to their frequency across all messages. English stop words are removed.
5.  **Feature Stacking**: The TF-IDF sparse matrix and the four numeric features are combined (`np.hstack`) to create the final feature set for the model.
6.  **Model Training**: A **Decision Tree Classifier** is trained on the combined feature set.
7.  **Model Persistence**: The trained classifier (`clf.joblib`) and the TF-IDF vectorizer (`vectorizer.joblib`) are saved using `joblib` so they can be loaded and used directly by the Streamlit application (`app.py`) for live predictions.

---

### 2. Streamlit Deployment Instructions

The Streamlit app (`app.py`) uses the saved model to classify new messages uploaded via a CSV file.

### ⚠️ IMPORTANT: Ngrok Authentication

To expose the locally running Streamlit app to the public internet, the script uses `pyngrok` and the `ngrok` service.

**You must replace the placeholder token in the script with your own authentication token.**

1.  **Sign Up/Log In**: Create an account or log in at the ngrok official website.
2.  **Get Your Token**: Find your authentication token on your ngrok dashboard.
3.  **Update Script**: Locate the following line in the `classifier.py` file:
    ```python
    !ngrok authtoken 35jt4mAXNJflqSJRTr0jMGZwZvh_2qwXreiazqpmBmD9gHc
    ```
4.  **Replace**: Change the example token (`35jt4...`) with your unique, personal ngrok authentication token.
5.  **Execution**: Once the script is run with your token, it will start a public tunnel and print the Streamlit public URL for access.
