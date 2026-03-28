# News-Classifier: AI Robustness & Misinformation Detection
News-Classifier is a Natural Language Processing (NLP) and machine learning project focused on Threat Intelligence and model security. Designed to detect and classify news articles as either True or Fake, this system evaluates the robustness and reliability of multiple classification algorithms against fabricated information. The repository reflects a cybersecurity and computer engineering approach to identifying vulnerabilities in AI models and mitigating the spread of adversarial clickbait and AI-generated fake news in digital ecosystems.

## Repository Structure
* `main.ipynb`: The core Jupyter Notebook containi all functionalities for data preprocessing, data validation, model training, evaluation, and visualization.
* `True.csv`: Dataset containing authentic news articles.
* `Fake.csv`: Dataset containing fabricated news articles.
* `News.csv`: The combined, cleaned, and preprocessed dataset outputted by the notebook.
* `externalArticles.cvs`: A collection of external articles used for out-of-sample testing of the trained classifiers and AI-generated texts used for real-world robustness and adversarial testing.

## Methodology & Functionalities

### 1. Data Ingestion & Validation
* **Reading & Inspection**: The system reads `True.csv` and `Fake.csv`, printing their shapes, columns, and checking for any missing values across all columns.
* **Duplicate Removal**: Identifies and removes duplicate entries from both datasets prior to merging.
* **Merging & Labeling**: Assigns a label of `1` to true news and `0` to fake news, then merges them into a single dataset using `pd.concat`.
* **Data Integrity Checks**: Utilizes `assert` statements to programmatically verify that the merged dataset has the correct shape and contains zero duplicates.

### 2. Data Preprocessing & Text Cleaning
* **Feature Engineering**: Combines the `title` and `text` columns into a single `full_text` feature, dropping the redundant `title`, `text`, `subject`, and `date` columns.
* **Text Normalization**: Applies a comprehensive cleaning function that:
  * Lowercases all text.
  * Removes punctuation while explicitly retaining numbers, as they can be useful for classification.
  * Removes extra spaces and replaces repetitions of punctuation marks.
  * Removes emojis by encoding to and decoding from ASCII.
  * Fixes English contractions using the `contractions` library.
  * Strips out URLs using regular expressions.
  * Removes English stopwords utilizing `nltk.corpus.stopwords`.
* **Data Export**: Saves the fully cleaned dataset to a new file named `News.csv`.

### 3. Feature Extraction & Data Splitting
* **Train/Test Split**: Divides the cleaned dataset into an 80% training set and a 20% testing set using a fixed random state for reproducibility.
* **TF-IDF Vectorization**: Converts the raw text data into a matrix of TF-IDF features (`TfidfVectorizer`), chosen over `CountVectorizer` for its efficiency in evaluating word frequency importance.

### 4. Model Implementation & Tuning
The following classifiers are trained and appended to a tracking list for comparative analysis:
* **Random Forest Classifier**
* **K-Nearest Neighbors (KNN)**: Initially tested with 5 neighbors, then rigorously tuned using `GridSearchCV` (with 5-fold cross-validation) across parameters `[1, 3, 5, 7, 9, 188]` to determine the optimal `n_neighbors` value.
* **Logistic Regression**
* **Multinomial Naive Bayes**

### 5. Evaluation & Visualization
Each trained model is evaluated iteratively on the test set to identify limitations, generating:
* **Classification Reports**: Detailing precision, recall, f1-score, support, and overall accuracy.
* **Graphical Visualizations**: Plotting accuracy scores, confusion matrices (showing true/false positives and negatives), and ROC AUC curves to visualize the tradeoff between true positive and false positive rates.

### 6. External Testing
To verify real-world robustness against potential attackers using AI for clickbait or disinformation campaigns, the models undergo reliability tests using completely unseen inputs derived from `externalArticles.cvs`. This phase includes:
**Real-World Benchmarking**: Testing against verified real-world sources (e.g., The Times, The Sun).
* **Adversarial Simulation**: Simulating realistic attack scenarios by exposing the models to anomalous inputs and AI-generated texts.
* **Performance Metrics**: Calculating predictions and plotting respective metrics to conduct a comparative analysis of the best classification capabilities under stress.


## 🛠 Technologies & Libraries

**Core Language**
* `Python 3.x`

**Data Manipulation**
* `pandas`: For data ingestion, merging, and structural manipulation.
* `numpy`: For numerical operations and array handling.

**Natural Language Processing (NLP)**
* `nltk`: For text tokenization, stopword removal, and text normalization.
* `contractions`: For expanding English contractions during the text cleaning phase.
* `re` (Regular Expressions): For URL and special character filtering.

**Machine Learning & Modeling**
* `scikit-learn`: Utilized for TF-IDF feature extraction (`TfidfVectorizer`), model training (Random Forest, KNN, Logistic Regression, Naive Bayes), hyperparameter tuning (`GridSearchCV`), and performance evaluation.

**Data Visualization**
* `matplotlib` & `seaborn`: For plotting ROC AUC curves, confusion matrices, and accuracy comparisons.
