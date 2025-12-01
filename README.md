

# 📊 RFM Segmentation Pipeline

This repository provides a full Python implementation of **RFM (Recency–Frequency–Monetary) segmentation**, including outlier detection, K-Means clustering, segment stabilization over time, and customer lifecycle categorization.

The code is designed for CRM analytics, retail analysis, and customer behavior modeling.  
It supports **multi-period RFM**, **new customer identification**, and **custom segmentation rules**.

---

## 🔍 What is RFM?

RFM is a customer segmentation technique based on three behavioral metrics:

### **1️⃣ Recency (R)**  
How recently a customer made a purchase.  
- Smaller value → more recent → better customer.

### **2️⃣ Frequency (F)**  
How often the customer buys.  
- Higher value → more loyal.

### **3️⃣ Monetary (M)**  
How much money the customer spends.  
- Higher value → more valuable.

---

## 🎯 Why RFM?

RFM is widely used because it is:

- ✔️ Simple  
- ✔️ Interpretable  
- ✔️ Statistically effective  
- ✔️ Highly predictive of future behavior  
- ✔️ Perfect for marketing, retention, CRM, churn prediction  

---

## 🧠 How Segmentation Works in This Project

This implementation enhances classic RFM:

### **✔ 1. Calculate Recency, Frequency, Monetary**
For each customer and time period:

- `Recency = (PeriodEndDate – LastPurchaseDate)`  
- `Frequency = Number of invoices`  
- `Monetary = Total purchase amount`  
- Returns are handled using `NetMonetary = Monetary – Returns`

---

### **✔ 2. Outlier Detection**
Extreme values for R, F, M are removed using:

```

Z-Score > 3.0

````

Outliers are saved separately but still segmented.

---

### **✔ 3. Clustering Using K-Means**
Each of R, F, M is clustered independently into up to 5 clusters:

- RecencyCluster → ordered descending  
- FrequencyCluster → ordered ascending  
- MonetaryCluster → ordered ascending  

This ensures:

- Best customers get highest cluster scores  
- Worst customers get lowest scores  

---

### **✔ 4. RFM Score: R + F + M**

Example:

| R | F | M | Score |
|---|---|---|--------|
| 4 | 3 | 3 | **433** |
| 0 | 1 | 0 | **010** |

---

### **✔ 5. Segment Mapping (Business Rules)**

Scores are mapped to business-friendly segments:

| Segment | Meaning |
|--------|---------|
| **Champions** | Best customers with high RFM |
| **Loyal** | Frequent, valuable buyers |
| **Potential Loyalist** | Developing loyal customers |
| **Promising** | Early-stage but positive behavior |
| **Needs Attention** | Medium activity, risk of churn |
| **At Risk** | Declining behavior |
| **Hibernating** | Long time no purchase |
| **Lost** | No activity for a long period |

Mapping rules come from the dictionary:

```python
SEGMENT_MAP = {
    "Champions": {...},
    "Loyal": {...},
    "Potential Loyalist": {...},
    ...
}
````

---

### **✔ 6. Stable Segments**

Customer segments are stabilized across time:

* Sudden unrealistic jumps are prevented
* Only allowed transitions are permitted
* Otherwise, the previous segment is retained

This creates **smooth customer lifecycle curves**.

---

### **✔ 7. Identify New Customers**

For each category/product group:

```
If customer's first purchase year == current period year:
        -> Segment = "New Customer"
```

This identifies category-specific new customers.

---

## 📅 Multi-Period RFM

The pipeline automatically identifies:

* Monthly periods
* From your date dimension table (`YearMonthName`)
* Generates dynamic date ranges:

```python
custom_ranges = [(start_date, end_date), ...]
```

Then RFM is computed **per period per category**.

---

## 📂 Output Structure

The final output Excel file:

```
RFM_custom_ranges.xlsx
│
└── RFM_Data   (single sheet)
```

Columns include:

* CustomerCode
* Recency / Frequency / Monetary
* RFM clusters
* RFM Score
* Segment
* FinalSegment
* OutlierFlag (Normal / Outlier)
* Category
* Date

---

## 🚀 How to Run

```bash
pip install pandas numpy scikit-learn scipy xlsxwriter
```

Then:

```bash
python rfm_segmentation.py
```

Output will be saved as:

```
RFM_custom_ranges.xlsx
```

---

## 📈 Use Cases

* CRM segmentation
* Retention modeling
* VIP customer identification
* Churn prediction
* Campaign targeting
* Product-level customer classification
* Monthly customer lifecycle tracking

---

## 🛠 Technologies Used

* **Python**
* **Pandas**
* **NumPy**
* **Scikit-Learn**
* **SciPy**
* **XlsxWriter**
* **RFM Theory**
* **K-Means Clustering**

---

## ❤️ Contributing

Pull requests and improvements are welcome!
Please open an issue if you find bugs or want enhancements.

---

## 📜 License

This project is released under the **MIT License**.

```



