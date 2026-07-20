# Scikit-Learn & Python Data Science Technical Pair Coding Guide updated by Mohammad Sefidgar

A comprehensive, end-to-end practical guide containing **100 Q&A sections** for pair-coding interviews and hands-on technical assessments focusing on tabular prediction tasks using `pandas`, `numpy`, and `scikit-learn`.

---

## Section 1: Data Ingestion & Initial Inspection (Q1–Q10)

### Q1: How do you load a CSV file into Pandas and inspect its structure and data types?
**Code:**
```python
import pandas as pd

df = pd.read_csv("dataset.csv")
print(df.info())
print(df.head())
```
**Explanation:** `pd.read_csv()` ingests tabular data into a DataFrame, while `.info()` displays row counts, column names, non-null counts, and data types, and `.head()` previews the top rows. Using this early prevents silent data type mismatches (such as numbers ingested as strings) and gives immediate clarity on missingness before building models.

---

### Q2: How do you generate summary statistics for numerical and categorical features?
**Code:**
```python
# Summary for numerical columns
num_stats = df.describe()

# Summary for object/categorical columns
cat_stats = df.describe(include=["object", "category"])
```
**Explanation:** `df.describe()` summarizes central tendencies, dispersion, and range for numerical columns, while `include=['object', 'category']` reports unique counts, top categories, and frequencies. This essential initial check reveals extreme outliers, constant features, or class imbalances early in the workflow.

---

### Q3: How do you check the distribution of the target variable for a classification task?
**Code:**
```python
# Value counts and normalized proportions
print(df["target"].value_counts())
print(df["target"].value_counts(normalize=True))
```
**Explanation:** `value_counts(normalize=True)` yields class proportions to identify target imbalance. Knowing whether a dataset is imbalanced guides the choice of cross-validation strategies (e.g., Stratified K-Fold) and evaluation metrics (F1-score or ROC-AUC over plain accuracy).

---

### Q4: How do you check for missing values across all columns in a DataFrame?
**Code:**
```python
missing_summary = df.isnull().sum()
missing_pct = (df.isnull().sum() / len(df)) * 100
missing_df = pd.DataFrame(
    {"Missing Count": missing_summary, "Percentage": missing_pct}
)
print(missing_df[missing_df["Missing Count"] > 0])
```
**Explanation:** `isnull().sum()` quantifies null entries per feature. Calculating missingness percentages highlights features requiring imputation versus candidate features for deletion (typically those missing >50%), avoiding broken pipelines during Scikit-Learn model fitting.

---

### Q5: How do you inspect feature correlations with the target variable?
**Code:**
```python
numeric_df = df.select_dtypes(include=["number"])
correlations = numeric_df.corr()["target"].sort_values(ascending=False)
print(correlations)
```
**Explanation:** `corr()` calculates linear Pearson correlation coefficients between numerical attributes and the target. This provides a fast baseline assessment of linear predictive signal and alerts you to potential multicollinearity among predictor variables.

---

### Q6: How do you check for and remove duplicated rows in a dataset?
**Code:**
```python
duplicate_count = df.duplicated().sum()
print(f"Duplicates: {duplicate_count}")

# Remove duplicates while keeping the first occurrence
df_clean = df.drop_duplicates().reset_index(drop=True)
```
**Explanation:** `duplicated()` flags exact duplicate rows across all columns. Dropping identical records prevents data contamination across train and test sets, ensuring cross-validation scores reflect true unseen performance.

---

### Q7: How do you identify non-numeric features that need encoding?
**Code:**
```python
cat_cols = df.select_dtypes(include=["object", "category"]).columns.tolist()
num_cols = df.select_dtypes(include=["number"]).columns.tolist()

print(f"Categorical features: {cat_cols}")
print(f"Numerical features: {num_cols}")
```
**Explanation:** Filtering by dtype isolates categorical attributes from continuous numerical features. Identifying these columns explicitly allows you to route them into separate preprocessing transformers (e.g., `OneHotEncoder` vs. `StandardScaler`) within Scikit-Learn pipelines.

---

### Q8: How do you check cardinality (number of unique values) for categorical variables?
**Code:**
```python
cardinality = df.select_dtypes(include=["object", "category"]).nunique()
print(cardinality.sort_values(ascending=False))
```
**Explanation:** High cardinality features (e.g., zip codes or IDs with hundreds of distinct string values) cause memory issues and dimensionality explosions when dummy-encoded. Identifying high cardinality upfront signals the need for target encoding, grouping rare categories, or ordinal mapping.

---

### Q9: How do you detect potential numerical outliers using IQR (Interquartile Range)?
**Code:**
```python
Q1 = df["feature"].quantile(0.25)
Q3 = df["feature"].quantile(0.75)
IQR = Q3 - Q1

outliers = df[
    (df["feature"] < (Q1 - 1.5 * IQR)) | (df["feature"] > (Q3 + 1.5 * IQR))
]
print(f"Outlier count: {len(outliers)}")
```
**Explanation:** The Interquartile Range method identifies extreme observations lying beyond $1.5 \times \text{IQR}$ from the quartiles. Flagging outliers prevents linear models and neural networks from being heavily biased by skewed extreme values.

---

### Q10: How do you verify memory usage and optimize data types for large DataFrames?
**Code:**
```python
print(df.memory_usage(deep=True).sum() / 1024**2, "MB")

# Convert low-cardinality strings to category
for col in df.select_dtypes(include="object").columns:
    if df[col].nunique() / len(df) < 0.05:
        df[col] = df[col].astype("category")
```
**Explanation:** Converting repetitive string objects to Pandas `category` types reduces memory usage substantially and accelerates downstream grouping and sorting operations during exploratory analysis.

---

## Section 2: Data Cleaning & Missing Value Handling (Q11–Q25)

### Q11: How do you drop features with excessive missing values in Pandas?
**Code:**
```python
threshold = 0.4  # Drop columns missing over 40% of data
df_cleaned = df.dropna(thresh=int((1 - threshold) * len(df)), axis=1)
```
**Explanation:** `dropna()` with `thresh` retains columns meeting a minimum required count of valid values. Dropping features with high missingness eliminates noisy predictors that imputation cannot reliably reconstruct.

---

### Q12: How do you impute numerical columns with mean or median using Scikit-Learn?
**Code:**
```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="median")
X_num_imputed = imputer.fit_transform(df[["age", "income"]])
```
**Explanation:** `SimpleImputer(strategy='median')` replaces missing values with column medians, offering robustness against extreme outliers. Using Scikit-Learn's imputer instead of Pandas `.fillna()` ensures imputation values learned on training data can be seamlessly applied to unseen test sets.

---

### Q13: How do you impute categorical columns with the most frequent value (mode)?
**Code:**
```python
cat_imputer = SimpleImputer(strategy="most_frequent")
X_cat_imputed = cat_imputer.fit_transform(df[["city", "department"]])
```
**Explanation:** Replaces missing categorical values with the mode. This maintains category integrity without introducing synthetic category names, suitable for low-missingness nominal variables.

---

### Q14: How do you fill missing categorical values with a constant label like "Missing"?
**Code:**
```python
constant_imputer = SimpleImputer(strategy="constant", fill_value="Missing")
X_cat_imputed = constant_imputer.fit_transform(df[["category_col"]])
```
**Explanation:** Imputing with a distinct label treats missingness as an explicit informative signal rather than assuming data is missing completely at random (MCAR).

---

### Q15: How do you use KNNImputer for multivariate missing value imputation?
**Code:**
```python
from sklearn.impute import KNNImputer

knn_imp = KNNImputer(n_neighbors=5)
X_imputed = knn_imp.fit_transform(df[["feature1", "feature2", "feature3"]])
```
**Explanation:** `KNNImputer` estimates missing values by computing mean values from the $k$-nearest neighbors based on Euclidean distance. This preserves complex feature interactions far better than univariate mean/median imputation.

---

### Q16: How do you use IterativeImputer (MICE) for advanced feature imputation?
**Code:**
```python
from sklearn.experimental import enable_iterative_imputer  # noqa
from sklearn.impute import IterativeImputer

mice_imp = IterativeImputer(max_iter=10, random_state=42)
X_imputed = mice_imp.fit_transform(numeric_df)
```
**Explanation:** `IterativeImputer` models each feature with missing values as a function of other features in a round-robin fashion (Multivariate Imputation by Chained Equations). It produces highly accurate imputations when complex dependencies exist between features.

---

### Q17: How do you preserve missingness signals using MissingIndicator?
**Code:**
```python
from sklearn.impute import MissingIndicator

indicator = MissingIndicator()
missing_flags = indicator.fit_transform(df)
# Alternatively, use add_indicator parameter in SimpleImputer
imputer = SimpleImputer(strategy="median", add_indicator=True)
X_trans = imputer.fit_transform(df)
```
**Explanation:** `MissingIndicator` generates binary flags indicating where missing values originally existed. When missingness is non-random (MNAR - Missing Not At Random), these binary flags provide predictive power to downstream classifiers.

---

### Q18: How do you cap extreme outliers using quantile clipping in Pandas?
**Code:**
```python
lower = df["income"].quantile(0.01)
upper = df["income"].quantile(0.99)

df["income_clipped"] = df["income"].clip(lower=lower, upper=upper)
```
**Explanation:** `clip()` caps extreme values at designated lower and upper percentiles without removing rows. Winsorizing dataset tails preserves valuable sample size while preventing linear models and gradient metrics from destabilizing.

---

### Q19: How do you remove invalid or impossible records (e.g., negative age)?
**Code:**
```python
valid_mask = (df["age"] >= 0) & (df["age"] <= 120) & (df["income"] >= 0)
df_valid = df[valid_mask].copy()
```
**Explanation:** Logical boolean filtering strips dirty or corrupted system records. Cleaning impossible domain values ensures models do not fit impossible artifact boundaries.

---

### Q20: How do you handle string inconsistencies (casing, whitespace) in text features?
**Code:**
```python
df["city"] = df["city"].astype(str).str.strip().str.lower()
```
**Explanation:** Standardizing text values prevents identical labels (e.g., " New York", "New York", "new york") from being treated as distinct categories, keeping encoded feature spaces compact.

---

### Q21: How do you group low-frequency rare categories into an "Other" category?
**Code:**
```python
threshold = 0.02  # Less than 2% frequency
freq = df["category"].value_counts(normalize=True)
rare_cats = freq[freq < threshold].index

df["category_grouped"] = df["category"].replace(rare_cats, "Other")
```
**Explanation:** Merging infrequent levels into a single category prevents high-cardinality expansion and avoids creating rare dummy variables that cause overfitting.

---

### Q22: How do you convert invalid string numeric values (e.g., "$1,200") to clean floats?
**Code:**
```python
df["price"] = (
    df["price"]
    .astype(str)
    .str.replace("$", "", regex=False)
    .str.replace(",", "", regex=False)
    .astype(float)
)
```
**Explanation:** Stripping formatting symbols converts string representations back into clean numeric dtypes necessary for mathematical matrix transformations.

---

### Q23: How do you handle infinite values in Pandas numeric features?
**Code:**
```python
import numpy as np

# Replace positive/negative infinity with NaN, then impute or drop
df.replace([np.inf, -np.inf], np.nan, inplace=True)
df.fillna(df.median(numeric_only=True), inplace=True)
```
**Explanation:** Division-by-zero operations can generate `inf` values that crash Scikit-Learn algorithms. Replacing infinity with `NaN` allows standard imputer components to clean the data safely.

---

### Q24: How do you parse and clean date/timestamp columns in Pandas?
**Code:**
```python
df["datetime"] = pd.to_datetime(df["timestamp_col"], errors="coerce")
missing_dates = df["datetime"].isnull().sum()
```
**Explanation:** `pd.to_datetime()` with `errors='coerce'` converts raw timestamp strings into datetime objects while gracefully setting unparseable date strings to `NaT`.

---

### Q25: How do you drop duplicate rows based on a subset of primary key columns?
**Code:**
```python
df_dedup = df.drop_duplicates(
    subset=["user_id", "transaction_date"], keep="last"
)
```
**Explanation:** Deduplicating on composite business keys keeps only the most recent or relevant record, avoiding skewed target predictions from duplicate event logs.

---

## Section 3: Feature Encoding & Transformations (Q26–Q40)

### Q26: How do you one-hot encode nominal categorical features with Scikit-Learn?
**Code:**
```python
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(sparse_output=False, handle_unknown="ignore")
cat_encoded = ohe.fit_transform(df[["color", "type"]])
```
**Explanation:** `OneHotEncoder` transforms nominal string columns into binary dummy columns. Setting `handle_unknown='ignore'` prevents runtime errors during inference when encountering unseen categories in test data.

---

### Q27: How do you handle high cardinality features in OneHotEncoder using min_frequency?
**Code:**
```python
ohe = OneHotEncoder(
    min_frequency=0.05, handle_unknown="infrequent_if_exist", sparse_output=False
)
encoded = ohe.fit_transform(df[["city"]])
```
**Explanation:** `min_frequency` automatically collapses rare categories into an "infrequent" category, capping sparse matrix dimensions while retaining major signals.

---

### Q28: How do you encode ordinal categorical variables with explicit order?
**Code:**
```python
from sklearn.preprocessing import OrdinalEncoder

education_order = [["High School", "Bachelors", "Masters", "PhD"]]
ord_enc = OrdinalEncoder(categories=education_order)
df["education_encoded"] = ord_enc.fit_transform(df[["education"]])
```
**Explanation:** `OrdinalEncoder` maps ordered categories to ascending integers while preserving rank relationships (e.g., PhD > Masters > Bachelors).

---

### Q29: How do you apply Target Encoding to high-cardinality categorical variables?
**Code:**
```python
from sklearn.preprocessing import TargetEncoder

te = TargetEncoder(smooth="auto", cv=5)
# Fit on features and target variable
X_encoded = te.fit_transform(df[["zip_code"]], df["target"])
```
**Explanation:** `TargetEncoder` replaces each categorical value with the expected target value, using internal cross-validation smoothing to prevent data leakage and target memorization.

---

### Q30: How do you standardize numeric features with StandardScaler?
**Code:**
```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(df[["age", "income"]])
```
**Explanation:** `StandardScaler` shifts data to mean 0 and variance 1. Scaling is critical for distance-based algorithms (KNN, SVMs) and gradient-descent optimized models (Logistic/Linear Regression).

---

### Q31: How do you scale numeric features to a bounded range [0, 1] using MinMaxScaler?
**Code:**
```python
from sklearn.preprocessing import MinMaxScaler

minmax = MinMaxScaler(feature_range=(0, 1))
X_scaled = minmax.fit_transform(df[["feature1", "feature2"]])
```
**Explanation:** `MinMaxScaler` scales features into a fixed range. It preserves exact zero entries in sparse datasets and is ideal for neural network inputs sensitive to feature scales.

---

### Q32: How do you scale data with extreme outliers using RobustScaler?
**Code:**
```python
from sklearn.preprocessing import RobustScaler

robust = RobustScaler()
X_scaled = robust.fit_transform(df[["income", "transactions"]])
```
**Explanation:** `RobustScaler` subtracts the median and divides by the Interquartile Range (IQR). Because median and IQR are unaffected by extreme values, scaling remains stable despite heavy outliers.

---

### Q33: How do you log-transform heavily right-skewed features?
**Code:**
```python
import numpy as np

# log1p computes log(1 + x) safely for non-negative values containing zeros
df["income_log"] = np.log1p(df["income"])
```
**Explanation:** `np.log1p` stabilizes variance and transforms highly skewed distributions (like revenue or prices) closer to normal distributions, improving linear model estimation.

---

### Q34: How do you transform features into Gaussian distributions using PowerTransformer?
**Code:**
```python
from sklearn.preprocessing import PowerTransformer

pt = PowerTransformer(method="yeo-johnson")
X_trans = pt.fit_transform(df[["skewed_feature"]])
```
**Explanation:** `PowerTransformer` applies optimal monotonic power transformations (Box-Cox or Yeo-Johnson) to stabilize variance and minimize skewness, enabling models that assume normality to perform better.

---

### Q35: How do you discretize continuous features into ordinal bins using KBinsDiscretizer?
**Code:**
```python
from sklearn.preprocessing import KBinsDiscretizer

binner = KBinsDiscretizer(
    n_bins=4, encode="ordinal", strategy="quantile"
)
df["age_binned"] = binner.fit_transform(df[["age"]])
```
**Explanation:** `KBinsDiscretizer` groups continuous features into discrete intervals, allowing linear models to capture non-linear, non-monotonic segment relationships.

---

### Q36: How do you extract year, month, day, and day of week from datetime features?
**Code:**
```python
df["year"] = df["datetime"].dt.year
df["month"] = df["datetime"].dt.month
df["day"] = df["datetime"].dt.day
df["dayofweek"] = df["datetime"].dt.dayofweek
df["is_weekend"] = df["dayofweek"].isin([5, 6]).astype(int)
```
**Explanation:** Extracting individual temporal components exposes non-linear trends, monthly seasonality, and day-of-week patterns directly to tabular algorithms.

---

### Q37: How do you capture cyclical time features (e.g., hours/months) using sine/cosine encoding?
**Code:**
```python
import numpy as np

df["sin_month"] = np.sin(2 * np.pi * df["month"] / 12)
df["cos_month"] = np.cos(2 * np.pi * df["month"] / 12)
```
**Explanation:** Sine/Cosine encoding preserves cyclical continuity (e.g., month 12 and month 1 are adjacent), preventing models from treating cyclical transitions as sudden numerical jumps.

---

### Q38: How do you generate interaction terms and polynomial features?
**Code:**
```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(degree=2, interaction_only=True, include_bias=False)
X_poly = poly.fit_transform(df[["featA", "featB"]])
```
**Explanation:** `PolynomialFeatures` generates feature products ($A \times B$), allowing linear estimators to model non-linear feature interactions explicitly.

---

### Q39: How do you apply custom feature transformations using FunctionTransformer?
**Code:**
```python
from sklearn.preprocessing import FunctionTransformer

custom_scaler = FunctionTransformer(func=np.sqrt, validate=True)
X_custom = custom_scaler.fit_transform(df[["count_feature"]])
```
**Explanation:** `FunctionTransformer` wraps arbitrary Python functions (e.g., square roots, custom scaling) into valid Scikit-Learn transformers compatible with Scikit-Learn Pipelines.

---

### Q40: How do you convert text column features into numerical vectors using TfidfVectorizer?
**Code:**
```python
from sklearn.feature_extraction.text import TfidfVectorizer

tfidf = TfidfVectorizer(max_features=100, stop_words="english")
X_text = tfidf.fit_transform(df["description_text"]).toarray()
```
**Explanation:** `TfidfVectorizer` converts unstructured text into numerical TF-IDF feature matrices, penalizing frequently occurring words to highlight unique informative terms.

---

## Section 4: Feature Selection & Dimensionality Reduction (Q41–Q50)

### Q41: How do you drop low-variance (quasi-constant) features?
**Code:**
```python
from sklearn.feature_selection import VarianceThreshold

selector = VarianceThreshold(threshold=0.01)
X_high_var = selector.fit_transform(X)
selected_cols = X.columns[selector.get_support()]
```
**Explanation:** `VarianceThreshold` identifies and removes features whose variance falls below a set cutoff. Dropping near-constant features eliminates uninformative noise, reducing training latency and model complexity.

---

### Q42: How do you select top features based on statistical tests using SelectKBest?
**Code:**
```python
from sklearn.feature_selection import SelectKBest, f_classif

selector = SelectKBest(score_func=f_classif, k=10)
X_selected = selector.fit_transform(X, y)
```
**Explanation:** `SelectKBest` computes ANOVA F-scores between each feature and the target label, keeping the top $k$ scoring features to simplify the feature space.

---

### Q43: How do you select top features using Mutual Information?
**Code:**
```python
from sklearn.feature_selection import SelectKBest, mutual_info_classif

selector = SelectKBest(score_func=mutual_info_classif, k=10)
X_selected = selector.fit_transform(X, y)
```
**Explanation:** Mutual information measures non-linear dependencies between features and targets, capturing complex relationships that standard linear correlation metrics miss.

---

### Q44: How do you perform Recursive Feature Elimination (RFE)?
**Code:**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.feature_selection import RFE

estimator = RandomForestClassifier(n_estimators=50, random_state=42)
rfe = RFE(estimator=estimator, n_features_to_select=10, step=1)
X_rfe = rfe.fit_transform(X, y)
```
**Explanation:** `RFE` recursively trains the base estimator, pruned by smallest feature importance weights at each pass, leaving the optimal predictive feature subset.

---

### Q45: How do you select features based on model feature importances (SelectFromModel)?
**Code:**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.feature_selection import SelectFromModel

clf = RandomForestClassifier(n_estimators=100, random_state=42)
sfm = SelectFromModel(estimator=clf, threshold="mean")
X_selected = sfm.fit_transform(X, y)
```
**Explanation:** `SelectFromModel` drops features whose importance score falls below a threshold (e.g., mean importance), retaining only impactful attributes.

---

### Q46: How do you use L1 (Lasso) regularization for automated feature selection?
**Code:**
```python
from sklearn.linear_model import LogisticRegression
from sklearn.feature_selection import SelectFromModel

lasso = LogisticRegression(penalty="l1", solver="liblinear", C=0.1)
sfm = SelectFromModel(estimator=lasso)
X_sparse = sfm.fit_transform(X, y)
```
**Explanation:** L1 regularization forces non-informative feature weights strictly to zero, serving as an embedded feature selector ideal for sparse data models.

---

### Q47: How do you remove highly collinear features using a correlation matrix?
**Code:**
```python
import numpy as np

corr_matrix = X.corr().abs()
upper_tri = corr_matrix.where(
    np.triu(np.ones(corr_matrix.shape), k=1).astype(bool)
)
to_drop = [
    col for col in upper_tri.columns if any(upper_tri[col] > 0.85)
]
X_uncorrelated = X.drop(columns=to_drop)
```
**Explanation:** Identifying feature pairs with absolute correlation above 0.85 and dropping one reduces variance and stabilizes regression model coefficient estimates.

---

### Q48: How do you reduce dataset dimensionality using Principal Component Analysis (PCA)?
**Code:**
```python
from sklearn.decomposition import PCA

pca = PCA(n_components=0.95)  # Retain 95% variance
X_pca = pca.fit_transform(X_scaled)
print(f"Components retained: {pca.n_components_}")
```
**Explanation:** PCA projects correlated features into orthogonal principal components. Setting `n_components=0.95` retains 95% of total variance while reducing dimensionality and noise.

---

### Q49: How do you evaluate the explained variance ratio in PCA?
**Code:**
```python
import numpy as np

pca = PCA(n_components=10).fit(X_scaled)
cum_var = np.cumsum(pca.explained_variance_ratio_)
print("Cumulative Variance Ratio:", cum_var)
```
**Explanation:** Monitoring cumulative explained variance helps determine the minimum component count needed to represent dataset signal accurately without overfitting.

---

### Q50: How do you reduce dimensionality for sparse matrices using TruncatedSVD?
**Code:**
```python
from sklearn.decomposition import TruncatedSVD

svd = TruncatedSVD(n_components=20, random_state=42)
X_svd = svd.fit_transform(X_sparse_matrix)
```
**Explanation:** Unlike PCA, `TruncatedSVD` does not center data, allowing efficient dimensionality reduction on sparse matrices (such as TF-IDF outputs) without memory exhaustion.

---

## Section 5: Data Splitting & Leakage Avoidance (Q51–Q60)

### Q51: How do you split a dataset into training and testing sets?
**Code:**
```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```
**Explanation:** Holds out 20% of data for final evaluation. Fixing `random_state` ensures identical split reproducibility across experiment iterations.

---

### Q52: How do you perform Stratified Train-Test Splitting for imbalanced classification?
**Code:**
```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
```
**Explanation:** `stratify=y` guarantees that training and testing subsets maintain identical target class proportions, preventing rare target classes from being omitted from test evaluation.

---

### Q53: How do you implement Standard K-Fold Cross-Validation?
**Code:**
```python
from sklearn.model_selection import KFold, cross_val_score
from sklearn.ensemble import RandomForestClassifier

kf = KFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(
    RandomForestClassifier(), X, y, cv=kf, scoring="accuracy"
)
print("CV Scores:", scores)
```
**Explanation:** K-Fold splits data into $K$ equal parts, iteratively training on $K-1$ folds and validating on the remaining fold to yield reliable generalized metric estimates.

---

### Q54: How do you implement Stratified K-Fold Cross Validation?
**Code:**
```python
from sklearn.model_selection import StratifiedKFold, cross_val_score

skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=skf, scoring="f1_macro")
```
**Explanation:** Stratified K-Fold enforces target class balance across every fold split, making it the standard validation choice for imbalanced classification tasks.

---

### Q55: How do you implement GroupKFold for grouped/clustered data?
**Code:**
```python
from sklearn.model_selection import GroupKFold, cross_val_score

gkf = GroupKFold(n_splits=5)
# Ensure same patient/user ID never spans both train and validation folds
scores = cross_val_score(model, X, y, cv=gkf, groups=df["patient_id"])
```
**Explanation:** `GroupKFold` prevents samples sharing the same group identifier (e.g., multiple entries from the same user or patient) from appearing in both train and validation sets, eliminating data leakage.

---

### Q56: How do you perform TimeSeriesSplit for temporal datasets?
**Code:**
```python
from sklearn.model_selection import TimeSeriesSplit, cross_val_score

tscv = TimeSeriesSplit(n_splits=5)
scores = cross_val_score(model, X_time, y_time, cv=tscv)
```
**Explanation:** `TimeSeriesSplit` uses an expanding window validation structure where training sets only contain past observations relative to validation folds, preserving natural temporal ordering.

---

### Q57: How do you prevent data leakage when calculating preprocessing parameters?
**Code:**
```python
# WRONG: Fit scaler on entire dataset X
# scaler.fit(X)

# CORRECT: Fit ONLY on training data
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```
**Explanation:** Fitting scalers or imputers on the full dataset before splitting leaks test set statistical information into training matrices. Fitting exclusively on `X_train` isolates test evaluation completely.

---

### Q58: How do you evaluate multiple scoring metrics simultaneously with cross_validate?
**Code:**
```python
from sklearn.model_selection import cross_validate

scoring = ["accuracy", "precision_macro", "recall_macro", "f1_macro"]
cv_results = cross_validate(
    model, X, y, cv=5, scoring=scoring, return_train_score=False
)
print("Mean F1:", cv_results["test_f1_macro"].mean())
```
**Explanation:** `cross_validate` evaluates multiple performance metrics in a single pass while tracking fit and score timings across validation iterations.

---

### Q59: How do you handle target variable scaling safely without leakage?
**Code:**
```python
from sklearn.compose import TransformedTargetRegressor
from sklearn.linear_model import Ridge
from sklearn.preprocessing import StandardScaler

ttr = TransformedTargetRegressor(
    regressor=Ridge(), transformer=StandardScaler()
)
ttr.fit(X_train, y_train)
y_pred = ttr.predict(X_test)
```
**Explanation:** `TransformedTargetRegressor` scales target outputs during training and automatically back-transforms predicted outputs to original scale units during inference.

---

### Q60: How do you create custom train/test validation splits based on explicit dates?
**Code:**
```python
train_mask = df["date"] < "2023-01-01"
test_mask = df["date"] >= "2023-01-01"

X_train, y_train = X[train_mask], y[train_mask]
X_test, y_test = X[test_mask], y[test_mask]
```
**Explanation:** Explicit temporal splits accurately simulate deployment conditions where models rely on historical data to predict future events.

---

## Section 6: Scikit-Learn Pipelines & ColumnTransformer (Q61–Q72)

### Q61: How do you build a basic Sequential Pipeline for single feature types?
**Code:**
```python
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

num_pipeline = Pipeline(
    [("imputer", SimpleImputer(strategy="median")), ("scaler", StandardScaler())]
)
X_num_prep = num_pipeline.fit_transform(X_numeric)
```
**Explanation:** `Pipeline` chains transformers and models into a single unit, ensuring operations fit and execute sequentially without manual step management.

---

### Q62: How do you construct a ColumnTransformer to handle mixed feature types?
**Code:**
```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, StandardScaler

preprocessor = ColumnTransformer(
    transformers=[
        ("num", StandardScaler(), ["age", "fare"]),
        ("cat", OneHotEncoder(handle_unknown="ignore"), ["embarked", "sex"]),
    ]
)
X_preprocessed = preprocessor.fit_transform(X)
```
**Explanation:** `ColumnTransformer` routes designated subset columns into tailored transformation blocks, recombining outputs into a single feature array.

---

### Q63: How do you build an End-to-End Predictive Pipeline with Preprocessing and Estimator?
**Code:**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.pipeline import Pipeline

full_pipeline = Pipeline(
    [("preprocessor", preprocessor), ("classifier", RandomForestClassifier())]
)

full_pipeline.fit(X_train, y_train)
predictions = full_pipeline.predict(X_test)
```
**Explanation:** Combining preprocessing transformers and final estimators into one top-level pipeline enables end-to-end model training, prediction, and cross-validation while completely preventing leakage.

---

### Q64: How do you configure ColumnTransformer to preserve pandas DataFrame outputs?
**Code:**
```python
preprocessor.set_output(transform="pandas")
X_trans_df = preprocessor.fit_transform(X_train)
print(type(X_trans_df))  # Output is a pandas DataFrame
```
**Explanation:** Calling `.set_output(transform="pandas")` ensures transformers output structured Pandas DataFrames with preserved feature column names rather than raw NumPy arrays.

---

### Q65: How do you pass unhandled columns through a ColumnTransformer?
**Code:**
```python
preprocessor = ColumnTransformer(
    transformers=[("num", StandardScaler(), ["age"])], remainder="passthrough"
)
```
**Explanation:** `remainder='passthrough'` keeps unlisted columns untouched, incorporating them directly alongside transformed outputs.

---

### Q66: How do you extract transformed feature names from a ColumnTransformer?
**Code:**
```python
preprocessor.fit(X_train)
feature_names = preprocessor.get_feature_names_out()
print(feature_names)
```
**Explanation:** `get_feature_names_out()` maps generated output array indices back to original feature names and encoding labels for downstream feature importance visualization.

---

### Q67: How do you construct a custom Transformer using BaseEstimator and TransformerMixin?
**Code:**
```python
from sklearn.base import BaseEstimator, TransformerMixin


class LogTransformer(BaseEstimator, TransformerMixin):

    def __init__(self, offset=1):
        self.offset = offset

    def fit(self, X, y=None):
        return self

    def transform(self, X):
        return np.log1p(X + self.offset)
```
**Explanation:** Inheriting from `BaseEstimator` and `TransformerMixin` creates native Scikit-Learn components equipped with standard `.fit_transform()` capabilities and parameter inspection.

---

### Q68: How do you combine parallel transformations using FeatureUnion?
**Code:**
```python
from sklearn.decomposition import PCA
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.pipeline import FeatureUnion

union = FeatureUnion(
    transformer_list=[
        ("pca", PCA(n_components=5)),
        ("stats", StandardScaler()),
    ]
)
```
**Explanation:** `FeatureUnion` executes distinct transformation routines concurrently on input matrices, concatenating their output features side-by-side.

---

### Q69: How do you access specific pipeline steps by name or index?
**Code:**
```python
# Access by step name
rf_model = full_pipeline.named_steps["classifier"]

# Access by index position
scaler_step = full_pipeline.steps[0][1]
```
**Explanation:** Inspecting inner pipeline components lets you extract learned estimator properties (like model weights or decision trees) without breaking pipeline encapsulation.

---

### Q70: How do you handle unknown target encoding values inside a pipeline?
**Code:**
```python
from sklearn.preprocessing import TargetEncoder

te = TargetEncoder(smooth="auto", cv=5, handle_unknown="value", value=0.0)
```
**Explanation:** Setting `handle_unknown='value'` prevents target encoders from failing when encountering novel categorical labels during test set evaluation.

---

### Q71: How do you incorporate feature selection steps inside a pipeline?
**Code:**
```python
from sklearn.feature_selection import SelectKBest, f_classif

pipeline_with_select = Pipeline(
    [
        ("prep", preprocessor),
        ("select", SelectKBest(score_func=f_classif, k=10)),
        ("clf", RandomForestClassifier()),
    ]
)
```
**Explanation:** Embedding feature selection directly inside the pipeline ensures selection criteria are computed strictly on training folds during cross-validation, avoiding leakage.

---

### Q72: How do you apply step conditional execution in Scikit-Learn pipelines?
**Code:**
```python
from sklearn.pipeline import Pipeline

# Set transformer step to 'passthrough' to disable it
pipe = Pipeline([("scaler", "passthrough"), ("clf", RandomForestClassifier())])
```
**Explanation:** Passing `'passthrough'` bypasses specific execution steps, allowing flexible experimentation with optional transformations during hyperparameter searches.

---

## Section 7: Model Building & Imbalance Handling (Q73–Q84)

### Q73: How do you establish a baseline classification model using DummyClassifier?
**Code:**
```python
from sklearn.dummy import DummyClassifier

dummy = DummyClassifier(strategy="most_frequent")
dummy.fit(X_train, y_train)
baseline_acc = dummy.score(X_test, y_test)
print(f"Baseline Accuracy: {baseline_acc}")
```
**Explanation:** `DummyClassifier` generates non-informative benchmark predictions (such as predicting the majority class). Machine learning models must outperform this metric baseline to prove predictive value.

---

### Q74: How do you fit and evaluate a Logistic Regression model?
**Code:**
```python
from sklearn.linear_model import LogisticRegression

log_reg = LogisticRegression(max_iter=1000, C=1.0, random_state=42)
log_reg.fit(X_train_scaled, y_train)
y_pred = log_reg.predict(X_test_scaled)
```
**Explanation:** `LogisticRegression` models log-odds using a linear baseline combination of input features. Scaling input features helps optimization solvers converge efficiently within `max_iter` limits.

---

### Q75: How do you apply Ridge (L2) and Lasso (L1) Regularization in Linear Models?
**Code:**
```python
from sklearn.linear_model import Lasso, Ridge

# L2 Regularization (Ridge)
ridge = Ridge(alpha=1.0).fit(X_train_scaled, y_train)

# L1 Regularization (Lasso)
lasso = Lasso(alpha=0.1).fit(X_train_scaled, y_train)
```
**Explanation:** Ridge ($L2$) shrinks regression coefficients to mitigate extreme variance from multicollinearity. Lasso ($L1$) drives non-essential coefficients to zero, performing embedded feature selection.

---

### Q76: How do you train a Random Forest Classifier and extract feature importances?
**Code:**
```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)
rf.fit(X_train, y_train)
importances = pd.Series(rf.feature_importances_, index=X_train.columns)
print(importances.sort_values(ascending=False))
```
**Explanation:** `RandomForestClassifier` fits an ensemble of decision trees over bootstrapped samples. Extracting `feature_importances_` highlights key predictors driving tree splits.

---

### Q77: How do you train Gradient Boosting models using HistGradientBoostingClassifier?
**Code:**
```python
from sklearn.ensemble import HistGradientBoostingClassifier

hgb = HistGradientBoostingClassifier(max_iter=100, random_state=42)
hgb.fit(X_train, y_train)
```
**Explanation:** `HistGradientBoostingClassifier` bins continuous features into discrete histograms, speeding up gradient boosting over large datasets while natively supporting missing values.

---

### Q78: How do you handle class imbalance using the class_weight parameter?
**Code:**
```python
from sklearn.ensemble import RandomForestClassifier

rf_balanced = RandomForestClassifier(
    class_weight="balanced", random_state=42
)
rf_balanced.fit(X_train, y_train)
```
**Explanation:** `class_weight='balanced'` adjusts loss misclassification penalties inversely proportional to class frequencies, forcing models to treat minority instances with higher importance.

---

### Q79: How do you apply SMOTE oversampling to rectify class imbalance?
**Code:**
```python
from imblearn.over_sampling import SMOTE

smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)
```
**Explanation:** SMOTE generates synthetic minority instances along feature line segments connecting neighboring minority samples, balancing class distributions without duplicate copying.

---

### Q80: How do you construct an Imbalanced-Learn Pipeline with SMOTE?
**Code:**
```python
from imblearn.pipeline import Pipeline as ImbPipeline
from imblearn.over_sampling import SMOTE
from sklearn.ensemble import RandomForestClassifier

imb_pipe = ImbPipeline(
    [
        ("preprocessor", preprocessor),
        ("smote", SMOTE(random_state=42)),
        ("clf", RandomForestClassifier()),
    ]
)
imb_pipe.fit(X_train, y_train)
```
**Explanation:** Standard Scikit-Learn pipelines cannot execute resampling methods. Using `imblearn.pipeline.Pipeline` applies SMOTE resampling exclusively during training fits, avoiding test set contamination.

---

### Q81: How do you build a Voting Ensemble Classifier?
**Code:**
```python
from sklearn.ensemble import (
    VotingClassifier,
    RandomForestClassifier,
    HistGradientBoostingClassifier,
)
from sklearn.linear_model import LogisticRegression

voting_clf = VotingClassifier(
    estimators=[
        ("lr", log_reg),
        ("rf", rf),
        ("hgb", hgb),
    ],
    voting="soft",
)
voting_clf.fit(X_train_scaled, y_train)
```
**Explanation:** `VotingClassifier` aggregates predictions from distinct estimators. Soft voting averages predicted class probabilities across base estimators, delivering robust generalized predictions.

---

### Q82: How do you train a Stacking Classifier?
**Code:**
```python
from sklearn.ensemble import StackingClassifier

stack_clf = StackingClassifier(
    estimators=[("rf", rf), ("hgb", hgb)],
    final_estimator=LogisticRegression(),
    cv=5,
)
stack_clf.fit(X_train, y_train)
```
**Explanation:** Stacking uses base estimator predictions as input features to train a meta-estimator, learning how to combine individual base model strengths.

---

### Q83: How do you calibrate model output probabilities using CalibratedClassifierCV?
**Code:**
```python
from sklearn.calibration import CalibratedClassifierCV

calibrated_clf = CalibratedClassifierCV(estimator=rf, method="sigmoid", cv=5)
calibrated_clf.fit(X_train, y_train)
prob_calibrated = calibrated_clf.predict_proba(X_test)
```
**Explanation:** Algorithms like SVMs or Random Forests often produce uncalibrated confidence scores. Probability calibration aligns predicted probability values with true empirical frequencies.

---

### Q84: How do you build a multi-output classification model?
**Code:**
```python
from sklearn.multioutput import MultiOutputClassifier
from sklearn.ensemble import RandomForestClassifier

multi_target_clf = MultiOutputClassifier(RandomForestClassifier())
multi_target_clf.fit(X_train, y_train_multi)
```
**Explanation:** `MultiOutputClassifier` extends single-output estimators to fit independent classifiers for each target label in multi-label prediction problems.

---

## Section 8: Model Evaluation & Metrics (Q85–Q93)

### Q85: How do you compute a comprehensive Classification Report?
**Code:**
```python
from sklearn.metrics import classification_report

y_pred = model.predict(X_test)
print(classification_report(y_test, y_pred))
```
**Explanation:** `classification_report()` breaks down precision, recall, and F1-scores per class alongside macro and weighted averages, offering immediate diagnostic insight into model performance.

---

### Q86: How do you compute and plot a Confusion Matrix?
**Code:**
```python
from sklearn.metrics import ConfusionMatrixDisplay, confusion_matrix

cm = confusion_matrix(y_test, y_pred)
disp = ConfusionMatrixDisplay(confusion_matrix=cm)
disp.plot(cmap="Blues")
```
**Explanation:** Confusion matrices display True Positives, False Positives, True Negatives, and False Negatives, making misclassification patterns clear across target classes.

---

### Q87: How do you compute ROC-AUC score and plot the ROC Curve?
**Code:**
```python
from sklearn.metrics import RocCurveDisplay, roc_auc_score

y_probs = model.predict_proba(X_test)[:, 1]
print("ROC-AUC Score:", roc_auc_score(y_test, y_probs))

RocCurveDisplay.from_predictions(y_test, y_probs)
```
**Explanation:** ROC-AUC measures classification separation performance across all discrimination thresholds, proving essential for evaluating models under imbalanced class distributions.

---

### Q88: How do you plot Precision-Recall curves for highly imbalanced targets?
**Code:**
```python
from sklearn.metrics import PrecisionRecallDisplay, average_precision_score

ap_score = average_precision_score(y_test, y_probs)
print("Average Precision:", ap_score)

PrecisionRecallDisplay.from_predictions(y_test, y_probs)
```
**Explanation:** Precision-Recall curves focus directly on minority positive class predictions, making them superior to ROC curves for extremely imbalanced targets.

---

### Q89: How do you adjust decision thresholds to optimize Precision or Recall?
**Code:**
```python
import numpy as np

# Apply custom decision threshold (e.g., 0.3 instead of default 0.5)
custom_threshold = 0.3
y_pred_custom = (y_probs >= custom_threshold).astype(int)
```
**Explanation:** Shifting the prediction threshold lower increases recall (catching more positive instances), allowing business objectives to drive trade-offs between precision and recall.

---

### Q90: How do you compute common regression metrics (MAE, MSE, RMSE, R²)?
**Code:**
```python
import numpy as np
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

mae = mean_absolute_error(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = r2_score(y_test, y_pred)

print(f"MAE: {mae:.2f} | RMSE: {rmse:.2f} | R2: {r2:.2f}")
```
**Explanation:** MAE measures average absolute error magnitude, RMSE penalizes large outlier errors more heavily, and $R^2$ indicates the proportion of target variance explained by model features.

---

### Q91: How do you compute Log Loss (Cross-Entropy Loss)?
**Code:**
```python
from sklearn.metrics import log_loss

loss = log_loss(y_test, y_probs)
print("Log Loss:", loss)
```
**Explanation:** Log loss measures probability calibration quality, heavily penalizing confident but incorrect classification predictions.

---

### Q92: How do you compute Permutation Feature Importance for model-agnostic evaluation?
**Code:**
```python
from sklearn.inspection import permutation_importance

result = permutation_importance(
    model, X_test, y_test, n_repeats=10, random_state=42
)
sorted_importances_idx = result.importances_mean.argsort()
```
**Explanation:** Permutation importance measures score drops when individual features are randomly shuffled on test data, yielding model-agnostic feature importance without tree-bias artifacts.

---

### Q93: How do you plot Partial Dependence Plots (PDP) to inspect feature effects?
**Code:**
```python
from sklearn.inspection import PartialDependenceDisplay

PartialDependenceDisplay.from_estimator(model, X_test, features=["age", "income"])
```
**Explanation:** Partial Dependence Plots visualize marginal feature relationships on predicted outcomes, revealing whether model responses are linear, monotonic, or complex.

---

## Section 9: Hyperparameter Tuning (Q94–Q97)

### Q94: How do you tune model hyperparameters using GridSearchCV?
**Code:**
```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "classifier__n_estimators": [50, 100],
    "classifier__max_depth": [5, 10, None],
}

grid_search = GridSearchCV(
    full_pipeline, param_grid, cv=5, scoring="f1_macro", n_jobs=-1
)
grid_search.fit(X_train, y_train)
print("Best Params:", grid_search.best_params_)
```
**Explanation:** `GridSearchCV` evaluates all specified hyperparameter combinations via cross-validation, guaranteeing systematic parameter space exploration.

---

### Q95: How do you perform efficient randomized hyperparameter searches using RandomizedSearchCV?
**Code:**
```python
from scipy.stats import randint
from sklearn.model_selection import RandomizedSearchCV

param_dist = {
    "classifier__n_estimators": randint(50, 300),
    "classifier__max_depth": [3, 5, 10, 20, None],
}

rand_search = RandomizedSearchCV(
    full_pipeline, param_distributions=param_dist, n_iter=20, cv=5, random_state=42
)
rand_search.fit(X_train, y_train)
```
**Explanation:** `RandomizedSearchCV` samples a fixed number of parameter settings (`n_iter`) from specified distributions, searching wide hyperparameter spaces much faster than exhaustive grid search.

---

### Q96: How do you use HalvingGridSearchCV for fast parameter optimization on large datasets?
**Code:**
```python
from sklearn.experimental import enable_halving_search_cv  # noqa
from sklearn.model_selection import HalvingGridSearchCV

halving_search = HalvingGridSearchCV(
    full_pipeline, param_grid, factor=3, cv=5, random_state=42
)
halving_search.fit(X_train, y_train)
```
**Explanation:** `HalvingGridSearchCV` evaluates candidates on small subsets of data, iteratively discarding low-performing parameters while increasing resource allocation for top candidates.

---

### Q97: How do you access the best estimator and results DataFrame from Search Objects?
**Code:**
```python
best_model = grid_search.best_estimator_
results_df = pd.DataFrame(grid_search.cv_results_)

# Sort results by validation score rank
results_df = results_df.sort_values(by="rank_test_score")
print(
    results_df[["params", "mean_test_score", "std_test_score"]].head()
)
```
**Explanation:** Inspecting `.cv_results_` as a DataFrame enables error analysis, revealing whether parameters overfit or require wider sampling ranges.

---

## Section 10: Model Persistence & Deployment (Q98–Q100)

### Q98: How do you serialize and load trained Scikit-Learn models using Joblib?
**Code:**
```python
import joblib

# Save model pipeline to disk
joblib.dump(full_pipeline, "model_pipeline.joblib")

# Load model pipeline from disk
loaded_pipeline = joblib.load("model_pipeline.joblib")
predictions = loaded_pipeline.predict(X_test)
```
**Explanation:** `joblib` serializes large Scikit-Learn pipeline objects containing NumPy array structures into compact disk files for production deployment.

---

### Q99: How do you format raw production inference records into DataFrame format for pipelines?
**Code:**
```python
raw_input = {
    "age": [34],
    "income": [65000.0],
    "embarked": ["S"],
    "sex": ["female"],
}

input_df = pd.DataFrame(raw_input)
prediction = full_pipeline.predict(input_df)
probability = full_pipeline.predict_proba(input_df)[:, 1]
```
**Explanation:** Converting raw inference payloads into Pandas DataFrames matching original feature schema names ensures pipelines apply transformations without schema mismatches.

---

### Q100: How do you construct a complete end-to-end prediction export pipeline?
**Code:**
```python
# Train final pipeline on all available data
full_pipeline.fit(X, y)

# Generate predictions on unseen inference records
unseen_df = pd.read_csv("unseen_data.csv")
unseen_df["predicted_target"] = full_pipeline.predict(unseen_df)
unseen_df["prediction_confidence"] = full_pipeline.predict_proba(
    unseen_df.drop(columns=["predicted_target"])
)[:, 1]

# Export predictions to CSV
unseen_df.to_csv("final_predictions.csv", index=False)
```

### Q101: Industry Standard Gradient Boosting Libraries
While `HistGradientBoostingClassifier` is included in Scikit-Learn, many interviewers specifically expect you to know how to plug in **XGBoost**, **LightGBM**, or **CatBoost**.

* **Key syntax to know:**
  ```python
  from lightgbm import LGBMClassifier
  # Plugs directly into your Sklearn Pipeline:
  lgb_model = LGBMClassifier(n_estimators=100, learning_rate=0.05, random_state=42)
  ```

### Q102: Custom Metrics via `make_scorer`
Interviewers frequently ask you to optimize for a **custom business metric** (e.g., *"False Negatives cost us $500, but False Positives only cost $50"*).

* **Key syntax to know:**
  ```python
  from sklearn.metrics import make_scorer

  def custom_business_cost(y_true, y_pred):
      # Custom logic
      return cost

  custom_scorer = make_scorer(custom_business_cost, greater_is_better=False)
  # Use in GridSearchCV(..., scoring=custom_scorer)
  ```

### Q103: Group Aggregations in Pandas (`groupby.transform`)
Before passing data into a Sklearn pipeline, you are often asked to engineer domain features using Pandas group statistics (e.g., user average spend vs. current transaction).

* **Key pattern to memorize:**
  ```python
  df['user_avg_spend'] = df.groupby('user_id')['amount'].transform('mean')
  df['spend_ratio'] = df['amount'] / (df['user_avg_spend'] + 1e-5)
  ```

### Q104: Handling Column Name Loss in Pipelines
Knowing how to output Pandas DataFrames from pipelines (`preprocessor.set_output(transform="pandas")`) is crucial when an interviewer asks: *"Which feature had the highest SHAP or Permutation Importance score?"*

---

## How You Will Actually Be Evaluated

Having the syntax in your notes is step one; passing the interview depends on three execution traits:

* **Muscle Memory:** You should be able to write a basic `ColumnTransformer` + `Pipeline` setup in under 3 minutes without stopping to check documentation syntax.
* **Thinking Out Loud:** Explain *why* you are choosing median imputation over mean, or why you are selecting `PR-AUC` over `ROC-AUC` for imbalanced targets.
* **Defensive Coding:** Proactively checking `df.isnull().sum()` or verifying shapes (`X_train.shape`, `X_test.shape`) before fitting shows seniority.

