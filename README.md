<div align="center">

<img src="https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white"/>
<img src="https://img.shields.io/badge/PySpark-RDD%20API-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white"/>
<img src="https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/Google%20Colab-Ready-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white"/>

# 📡 PySpark RDD Analytics — Telecom Usage Dataset

**Domain:** Telecom Analytics &nbsp;|&nbsp; **Engine:** Apache Spark (RDD API) &nbsp;|&nbsp; **Language:** Python 3

</div>

---

## 📌 Overview

This notebook applies the **Apache Spark RDD API** to a simulated telecom usage dataset, covering 46 analytical exercises across core Spark primitives — `map`, `filter`, `flatMap`, `reduceByKey`, `sortByKey`, `sortBy`, and `distinct`. The dataset captures customer-level Call, Data, and SMS transactions across Indian cities, and all transformations are built using **low-level RDD operations** (no DataFrames or SQL) to develop a strong foundational understanding of Spark internals.

---

## 🗂️ Repository Structure

```
PySpark_RDD_Telecom_Analytics/
│
├── notebook/
│   └── PySpark_RDD_Telecom_Analytics.ipynb   # Main analysis notebook (46 exercises)
├── data/
│   └── telecom_rdd_data.txt                  # Raw dataset (comma-separated, no header)
└── README.md
```

---

## 📋 Dataset Schema

The dataset is a plain text file with no column headers. Each row is a comma-separated telecom transaction record.

| Field         | Index | Type   | Description                              |
|---------------|-------|--------|------------------------------------------|
| `customer_id` | 0     | int    | Unique customer identifier               |
| `name`        | 1     | str    | Customer name                            |
| `city`        | 2     | str    | City of the customer                     |
| `usage_type`  | 3     | str    | Type of usage: `Call` / `Data` / `SMS`   |
| `amount`      | 4     | int    | Usage amount in units                    |
| `date`        | 5     | str    | Transaction date (`YYYY-MM-DD`)          |

**Sample record:**
```
1001,John,Delhi,Call,300,2024-01-01
```

---

## 📊 Analysis Covered

| Exercise Range | Topics                                                             |
|----------------|--------------------------------------------------------------------|
| Q1 – Q10       | `map`, `filter`, `distinct`, `count`, `reduceByKey`, `sortByKey`   |
| Q11 – Q20      | Descending sorts, date-level aggregations, city-level ranking      |
| Q21 – Q30      | Composite keys `(city, usage_type)`, min/max per type, top-N slice |
| Q31 – Q46      | `flatMap`, chained transforms, normalization, grand-total reduce   |

**Selected exercises include:**
- Total usage amount per city, per date, per usage type
- Maximum and minimum usage per city using `reduceByKey`
- Top 5 highest transactions using `sortByKey(...).take(5)`
- Lowest 3 transactions by amount
- Grand total usage across all cities via chained `reduce`
- Distinct usage types extracted via `flatMap`
- City name normalization and case-insensitive filtering
- Composite key analysis: `(city, date)` and `(city, usage_type)`

---

## 🚀 Getting Started

### Prerequisites

- Python 3.x
- PySpark (`pip install pyspark`)
- Google Colab (recommended) or a local Spark environment

### Installation

```bash
pip install pyspark
```

### Running in Google Colab

```python
!pip install pyspark
from pyspark.sql import SparkSession
spark = SparkSession.builder.master("local").appName("Telecom_RDD_Analytics").getOrCreate()
sc = spark.sparkContext
```

Upload `telecom_rdd_data.txt` to `/content/sample_data/` in your Colab session, then run all cells sequentially.

---

## 💡 Key Concepts Demonstrated

**SparkSession & SparkContext initialisation**
```python
spark = SparkSession.builder.master("local").appName("Telecom_RDD_Analytics").getOrCreate()
sc = spark.sparkContext
```

**Parsing a raw text file into a structured RDD**
```python
raw_rdd = sc.textFile("/content/sample_data/telecom_rdd_data.txt")
telecom_rdd = raw_rdd.map(lambda row: row.split(","))
```

**Aggregating with `reduceByKey`**
```python
total_usage_per_city = telecom_rdd \
    .map(lambda row: (row[2], int(row[4]))) \
    .reduceByKey(lambda x, y: x + y)
```

**Composite key grouping**
```python
city_type_count = telecom_rdd \
    .map(lambda row: ((row[2], row[3]), 1)) \
    .reduceByKey(lambda x, y: x + y)
```

**Top-N with `sortByKey + take`**
```python
top_five_transactions = telecom_rdd \
    .map(lambda row: (int(row[4]), row)) \
    .sortByKey(ascending=False) \
    .take(5)
```

**Grand total via chained `reduce`**
```python
grand_total = telecom_rdd \
    .map(lambda row: (row[2], int(row[4]))) \
    .reduceByKey(lambda x, y: x + y) \
    .map(lambda x: x[1]) \
    .reduce(lambda x, y: x + y)
```

---

## 🛠️ Technologies Used

![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=flat-square&logo=apachespark&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-RDD%20API-E25A1C?style=flat-square&logo=apachespark&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)

---

<div align="center">

*Built by **Radhika Deshpande** · PySpark RDD exercises on a simulated telecom usage dataset*

</div>
