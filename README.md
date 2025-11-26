# E-Commerce Customer Segmentation & Monetary Value Prediction

 Customer segmentation using RFM analysis and K-Means clustering combined with cluster-wise Random Forest regression for monetary value prediction.

## 📊 Project Overview

This project addresses two critical business questions:

### A) How to segment customers based on buying behavior?
Using **RFM segmentation + K-Means clustering** to identify distinct customer groups.

### B) How to predict how much a customer will spend?
Using **Random Forest regression** trained separately for each cluster for accurate predictions.

### 🎯 Business Applications
- Customer Lifetime Value (CLV) prediction
- Marketing targeting & personalization
- Recommendation strategy optimization
- Risk & churn monitoring

---

## 📁 Dataset

**Source:** [Online Retail Dataset](https://www.kaggle.com/datasets/carrie1/ecommerce-data?utm_source=chatgpt.com) ( Kaggle)

### Dataset Structure
The dataset contains line-item level transaction records:

| Column | Description |
|--------|-------------|
| `InvoiceNo` | Unique order identifier |
| `CustomerID` | Unique customer identifier |
| `StockCode` | Product ID |
| `Quantity` | Number of units purchased |
| `UnitPrice` | Price per unit |
| `Description` | Product description |
| `InvoiceDate` | Transaction timestamp |

**Note:** Each row represents one product line within an order. Multiple rows with the same `CustomerID` + `InvoiceNo` (but different `StockCode`) are normal and expected.

---

## 🧹 Data Cleaning

The following preprocessing steps were applied:

- ✅ Remove missing `CustomerID` (unidentifiable customers)
- ✅ Remove missing `Description` (essential for product clarity)
- ✅ Remove negative values in `Quantity` and `UnitPrice` (returns/cancellations)
- ✅ Convert `InvoiceDate` to datetime format (`%d-%m-%Y %H:%M`)
- ✅ Create `Sales` column: `Quantity × UnitPrice`

---

## 🔍 RFM Feature Engineering

**RFM** = Recency, Frequency, Monetary

Customer-level aggregated features:

| Feature | Definition |
|---------|------------|
| **Recency** | Days since last purchase |
| **Frequency** | Number of unique invoices |
| **Monetary** | Total sales amount |

**Reference Date:** `max(InvoiceDate) + 1 day`

---

## 🤖 K-Means Clustering

### Features Used
- Recency
- Frequency  
- Monetary

### Process
1. Standardize RFM features
2. Test K from 2 to 10 using **Silhouette Score**
3. Select optimal K automatically
4. Train final K-Means model
5. Assign cluster labels to each customer
6. Visualize clusters using PCA components

### Results
- **Best K:** 2 clusters
- **Cluster 0:** Low-value, low-frequency customers
- **Cluster 1:** High-value, VIP customers

---

## 🏷️ Segment Labeling

Clusters are mapped to business segments:

| Cluster | Segment Label |
|---------|---------------|
| Cluster 0 | Low Value / At-Risk Customers |
| Cluster 1 | High Value / VIP Customers |

*Automatically adjustable based on average monetary value per cluster.*

---

## ⚙️ Additional Feature Engineering

To improve prediction accuracy, additional features are engineered:

| Feature | Definition |
|---------|------------|
| `TotalQuantity` | Sum of all units purchased |
| `UniqueProducts` | Number of distinct products purchased |
| `AvgOrderValue` | Monetary / Frequency |

---

## 🎯 Model Training (Cluster-Wise Random Forest)

### Why Separate Models per Cluster?
Training separate Random Forest models for each cluster improves prediction accuracy by capturing cluster-specific patterns.

### Prediction Features
- Recency
- Frequency
- TotalQuantity
- UniqueProducts
- AvgOrderValue

### Pipeline (per cluster)
1. Split data into train/test sets
2. Scale features using StandardScaler
3. Train RandomForestRegressor
4. Evaluate using R², MSE, MAE
5. Save scaler + model in dictionary

### Performance
- **Cluster 0:** R² ≈ 0.83 (High performance)
- **Cluster 1:** R² ≈ 0.78 (Moderate performance)

---

## 🔮 Prediction Functions

### 1. Predict Customer Cluster
```python
predict_cluster(recency, frequency, monetary)
```
Assigns a new customer to a cluster based on RFM values.

### 2. Predict Customer Monetary Value
```python
predict_customer_value(
    recency=20,
    frequency=12,
    total_quantity=200,
    unique_products=10,
    average_order_value=300
)
```

**Process:**
1. Predict cluster using RFM values
2. Prepare full feature set
3. Scale using cluster-specific scaler
4. Predict monetary value using cluster-specific Random Forest model

**Output:**
- Assigned cluster
- Predicted monetary value

---

## 📊 Visualizations

The project includes the following visualizations:

- 📦 Boxplot for `UnitPrice` distribution
- 🎨 PCA scatter plot (2D cluster visualization)
- 🔥 Cluster centroid heatmap

---

## 🧩 Why Two Separate Models?

### Clustering Model
- **Purpose:** Group customers into segments
- **Features:** RFM only (3 columns)
- **Algorithm:** K-Means

### Prediction Model  
- **Purpose:** Predict monetary value
- **Features:** Engineered features (5 columns)
- **Algorithm:** Random Forest (per cluster)

### Benefits
✅ Avoids data leakage  
✅ Higher prediction accuracy  
✅ Cluster-wise specialization

---

## 📂 Project Structure

```
📁 E-Commerce-Customer-Segmentation/
│
├── 📓 ecommerce_sales_clusters.ipynb    # Jupyter notebook
├── 🐍 ecommerce_sales_clusters.py       # Python script
├── 📁 data/
│   └── OnlineRetail.csv                 # Dataset
└── 📄 README.md                          # This file
```

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```
---

## 📈 Key Results

| Metric | Value |
|--------|-------|
| **Optimal K** | (k=2, Silhouette Score=0.8958) |(k=2, WCSS=9012.64) 
| **Segments** | Low Value / At-Risk, High Value / VIP |
| **Cluster 0 R²** | ~0.83 |
| **Cluster 1 R²** | ~0.78 |

### End-to-End Pipeline
✅ Takes customer inputs  
✅ Predicts cluster assignment  
✅ Predicts monetary value

---

## 🛠️ Technologies Used

- **Python 3.8+**
- **pandas** - Data manipulation
- **numpy** - Numerical computing
- **scikit-learn** - Machine learning (K-Means, Random Forest)
- **matplotlib & seaborn** - Data visualization

---

## 📝 Key Takeaways

This project demonstrates:

✅ Business logic implementation (RFM analysis)  
✅ Unsupervised learning (K-Means clustering)  
✅ Supervised learning (Random Forest regression)  
✅ Feature engineering techniques  
✅ End-to-end ML pipeline (Segmentation → Prediction)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Sourav Mondal**



---

## ⭐ Show your support

Give a ⭐️ if this project helped you!
