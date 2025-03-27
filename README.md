

## **🎯 Lesson Agenda**
1️⃣ Introduction to Cloud Dataproc  
2️⃣ Why Use Dataproc?  
3️⃣ Dataproc Architecture  
4️⃣ Setting Up a Dataproc Cluster (Hands-on)  
5️⃣ Submitting a Spark Job  
6️⃣ Monitoring & Managing Clusters  
7️⃣ Cleaning Up Resources  
8️⃣ Assignment & Q&A  

---

## **📢 1️⃣ Introduction to Cloud Dataproc** (5 min)  
Start with a simple explanation:  
> **"Cloud Dataproc is a managed service in Google Cloud that helps us run big data workloads using Apache Spark and Hadoop without worrying about infrastructure management."**  

### **🔹 Key Features:**  
✅ **Fast** – Clusters start in less than 90 seconds  
✅ **Fully Managed** – No need to set up Spark/Hadoop manually  
✅ **Integrated with GCP** – Works well with **BigQuery, Cloud Storage, etc.**  
✅ **Cost-Effective** – Pay **only when the cluster is running**  

---

## **📢 2️⃣ Why Use Dataproc? (Real-Life Example)** (5 min)  
💡 **Example:**  
> "Imagine you own a bakery and need to process thousands of customer orders to find the most popular cakes. Instead of manually analyzing the data, you can use Dataproc to quickly run a Spark job and get insights."  

### **Comparison: Manual vs Dataproc**
| Manual Hadoop Setup | Cloud Dataproc |
|---------------------|---------------|
| Takes hours/days | Starts in seconds |
| Manual cluster management | Fully managed |
| Fixed-cost | Pay-as-you-go |

---

## **📢 3️⃣ Dataproc Architecture** (5 min)  
Draw this simple architecture on a **whiteboard** or show a **diagram**:  

🔹 **Dataproc Cluster** consists of:  
- 🖥️ **Master Node** – Controls job execution  
- 🔄 **Worker Nodes** – Process data in parallel  
- 📦 **GCS or BigQuery** – Stores input/output data  

---

## **📢 4️⃣ Creating a Dataproc Cluster (Hands-On)** (10 min)  
### **🛠️ Step 1: Enable Dataproc API**  
1. Go to **Google Cloud Console** → **APIs & Services**  
2. Search for **Dataproc API** and enable it  

### **🛠️ Step 2: Create a Dataproc Cluster (Using Console)**  
1. Go to **GCP Console → Dataproc**  
2. Click **"Create Cluster"**  
3. **Enter Cluster Name**: `my-dataproc-cluster`  
4. **Region**: Select a region (e.g., `us-central1`)  
5. **Cluster Type**: Choose **Standard** (2 workers)  
6. **Click "Create"**  

### **🛠️ Step 3: Create a Dataproc Cluster (Using gcloud CLI)**  
Ask students to run this command in **Cloud Shell**:

```sh
gcloud dataproc clusters create my-cluster \
    --region=us-central1 \
    --single-node
```

---

## **📢 5️⃣ Submitting a Spark Job (Hands-On)** (10 min)  
### **Step 1: Create a PySpark Script (`wordcount.py`)**
Ask students to **copy and save** this script:

```python
from pyspark.sql import SparkSession

# Create Spark session
spark = SparkSession.builder.appName("WordCount").getOrCreate()

# Read a text file from GCS
text_file = spark.read.text("gs://public-datasets-sample/shakespeare.txt")

# Count word occurrences
word_counts = text_file.selectExpr("explode(split(value, ' ')) as word") \
                       .groupby("word") \
                       .count()

# Show top 10 words
word_counts.orderBy("count", ascending=False).show(10)

# Stop Spark session
spark.stop()
```

### **Step 2: Upload Script to Cloud Storage**
```sh
gsutil cp wordcount.py gs://your-bucket-name/
```

### **Step 3: Submit the Job to Dataproc**
```sh
gcloud dataproc jobs submit pyspark gs://your-bucket-name/wordcount.py \
    --cluster=my-cluster --region=us-central1
```

**📌 Explain**:  
- This runs the **word count program** on the **Dataproc cluster**.  
- The results will be printed in **Dataproc Jobs Logs**.  

---

## **📢 6️⃣ Monitoring & Managing Clusters** (5 min)  
### **🔍 Check Job Status**
- Go to **Dataproc → Jobs** in the Console  
- Click on the submitted job → View **Logs**  
- If using CLI:
  ```sh
  gcloud dataproc jobs list --region=us-central1
  ```

### **🔍 View Cluster Details**
```sh
gcloud dataproc clusters describe my-cluster --region=us-central1
```

---

## **📢 7️⃣ Cleaning Up Resources** (2 min)  
To **save money**, delete the cluster after use:

```sh
gcloud dataproc clusters delete my-cluster --region=us-central1
```

---

## **📢 8️⃣ Assignment for Students**
### **🎯 Task:**
1. Create a **multi-node** Dataproc cluster.  
2. Upload and run a **different PySpark script** (e.g., sorting words instead of counting).  
3. Submit the job and **analyze the logs**.  

📌 **Bonus Challenge:**  
- Run a **Hive query** on Dataproc.  
- Use **BigQuery** as an output storage.  

---

## **🎤 Wrap-Up & Q&A**
📌 **Summarize:**  
- Dataproc makes big data processing **easy and fast**.  
- It’s **fully managed**, cost-effective, and **integrates with GCP**.  
- You can run **Spark, Hadoop, Hive, and more** without managing infrastructure.  

Ask students:  
✅ **"What was the most interesting part?"**  
✅ **"Any doubts or challenges faced?"**  

---

## 🎁 **Extra Learning Resources**
1. 🔗 [Official Dataproc Documentation](https://cloud.google.com/dataproc/docs)  
2. 🎥 [Google Cloud Dataproc YouTube Playlist](https://www.youtube.com/playlist?list=PLgxF72Xb_7xS77J6yzt3fDBPc6-IJxsci)  
3. 📖 [GCP Free Tier (Try Dataproc for Free)](https://cloud.google.com/free)  

---

### 🎯 **Now You're Ready to Teach Dataproc Like a Pro! 🚀**
Would you like **slides** or **a demo script** for the session? 😊
