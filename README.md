# ✅ EXP 1 — Create VM (Commands)

```bash id="x0krgb"
gcloud compute instances create my-vm \
--zone=us-central1-a \
--machine-type=e2-medium
```

Connect VM:

```bash id="hprlhg"
gcloud compute ssh my-vm --zone=us-central1-a
```
```bash id="hprlhg"
sudo apt install apache2 –y 
```
```bash id="hprlhg"
lscpu # CPU details 
free -h # Memory details
```

Delete VM:

```bash id="bf56f7"
gcloud compute instances delete my-vm --zone=us-central1-a
```

---

# ✅ EXP 2 — Cloud Shell & gcloud

```bash id="k65e8g"
gcloud init
```

```bash id="15y5jt"
gcloud config list
```

```bash id="0qpbwq"
gcloud projects list
```

```bash id="6y9c78"
gcloud config set project PROJECT_ID
```

```bash id="it04rf"
gcloud auth list
```

```bash id="i5l6zd"
gcloud compute instances create my-vm --zone=us-central1-c
```

```bash id="3fmx4k"
gcloud compute instances list
```

```bash id="l7qmkn"
gcloud compute instances delete my-vm --zone=us-central1-c
```

---

# ✅ EXP 3 — Cloud Function / Cloud Run

## main.py

```python id="1p4c04"
import os
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    name = os.environ.get("NAME", "World")
    return f"Hello {name}!"

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=8080)
```

---

## requirements.txt

```txt id="wlu2nh"
Flask==3.0.3
gunicorn==23.0.0
Werkzeug==3.0.3
```

---

## Commands

```bash id="9d8j96"
gsutil mb gs://your-bucket-name
```

```bash id="ycj3g9"
python3 main.py
```

```bash id="i0e94u"
gcloud run deploy --source .
```

---

# ✅ EXP 4 — App Engine

## main.py

```python id="a4l3vw"
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from App Engine with Auto Scaling!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

---

## requirements.txt

```txt id="ivc41u"
Flask==2.2.5
```

---

## app.yaml

```yaml id="x6z1ep"
runtime: python312

entrypoint: python main.py

runtime_config:
  operating_system: "ubuntu22"
  runtime_version: "3.12"

automatic_scaling:
  min_instances: 1
  max_instances: 5
```

---

## Commands

```bash id="t2x71q"
gcloud services enable appengine.googleapis.com
```

```bash id="hq4jsy"
gcloud app create
```

```bash id="z4c5wy"
gcloud app deploy
```

```bash id="70zyr6"
gcloud app browse
```

---

# ✅ EXP 5 — Cloud Storage

```bash id="e8q25s"
gsutil mb gs://my-bucket-name
```

```bash id="4n31t1"
gsutil cp README-cloudshell.txt gs://my-bucket-name/
```

```bash id="b48uk8"
gsutil ls
```

```bash id="djlwmx"
gsutil ls gs://my-bucket-name
```

```bash id="9xh6g8"
gsutil cp gs://my-bucket-name/README-cloudshell.txt .
```

```bash id="n8ur0u"
gsutil rm gs://my-bucket-name/README-cloudshell.txt
```

```bash id="0crx8l"
gsutil rb gs://my-bucket-name
```

# ✅ EXP 8 — VPC Networks

## Create Auto Mode VPC

```bash id="8mb7f8"
gcloud compute networks create test-network --subnet-mode=auto
```

---

## Create Subnet

```bash id="ol2tgr"
gcloud compute networks subnets create subnet-1 \
--network=test-network \
--region=us-east4 \
--range=10.10.0.0/16
```

---

## Create Custom VPC

```bash id="0yff2m"
gcloud compute networks create dev-vpc --subnet-mode=custom
```

---

## Create Custom Subnet

```bash id="mjlwmv"
gcloud compute networks subnets create dev-subnet \
--network=dev-vpc \
--region=us-west1 \
--range=10.10.0.0/24
```

---

## List Networks

```bash id="9o9k2k"
gcloud compute networks list
```

---

## Create Firewall Rule

```bash id="t8flhx"
gcloud compute firewall-rules create allow-dev-internal \
--network=dev-vpc \
--allow=tcp,udp,icmp \
--source-ranges=10.10.0.0/24
```

---

## Create VM

```bash id="w8o34n"
gcloud compute instances create dev-vm \
--zone=us-west1-a \
--network=dev-vpc \
--subnet=dev-subnet
```

---

## Delete VM

```bash id="w7btbm"
gcloud compute instances delete dev-vm \
--zone=us-west1-a
```

---

## Delete Firewall Rule

```bash id="2umkp7"
gcloud compute firewall-rules delete allow-dev-internal
```

---

## Delete VPC

```bash id="5mbm5q"
gcloud compute networks delete dev-vpc
```

---

# ✅ EXP 10 — Kubernetes Engine Cluster

## Set Project

```bash id="hdb0di"
gcloud config set project PROJECT_ID
```

---

## Create Repository

```bash id="6n3wwd"
gcloud artifacts repositories create my-repo \
--repository-format=docker \
--location=us-central1 \
--description="Docker repo"
```

---

## app.py

```python id="kq4crq"
print("Hello from Docker!")
```

---

## requirements.txt

```txt id="kff9i7"
Flask==3.3.0
```

---

## Dockerfile

```dockerfile id="ut5zqk"
FROM python:3.9-slim

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["python", "app.py"]
```

---

## Build Docker Image

```bash id="wyfjlwm"
docker build -t my-python-app .
```

---

## Run Container

```bash id="7kfd34"
docker run my-python-app
```

---

## Tag Image

```bash id="ukn24z"
docker tag my-python-app:latest \
us-west1-docker.pkg.dev/PROJECT-ID/my-repo/my-python-app:latest
```

---

## Push Image

```bash id="5s1rwb"
docker push \
us-west1-docker.pkg.dev/PROJECT-ID/my-repo/my-python-app:latest
```

---

## Create Kubernetes Cluster

```bash id="zcmj4r"
gcloud container clusters create my-cluster \
--zone us-central1-a \
--num-nodes=1
```

---

## Get Credentials

```bash id="5nvd88"
gcloud container clusters get-credentials my-cluster \
--zone us-central1-a
```

---

## Deploy Application

```bash id="bydxix"
kubectl create deployment my-custom-app \
--image=gcr.io/PROJECT-ID/my-custom-app
```

---

## Expose Application

```bash id="qhls1c"
kubectl expose deployment my-custom-app \
--type=LoadBalancer \
--port 80 \
--target-port 8080
```

---

## Get External IP

```bash id="jlwm2o"
kubectl get service my-custom-app
```

---

## Delete Cluster

```bash id="pqaxfj"
gcloud container clusters delete my-cluster \
--zone us-central1-a
```
# ✅ EXP 9 — Cloud Monitoring (VERY SIMPLE STEPS)

---

# 🔹 Step 1: Open Google Cloud

1. Open:

   ```id="j6cw6g"
   https://console.cloud.google.com
   ```

2. Login with Google account.

---

# 🔹 Step 2: Enable Monitoring API

1. Click:

   ```id="f6mjlwm"
   Navigation Menu (☰)
   ```

2. Go to:

   ```id="5bdscm"
   APIs & Services → Library
   ```

3. Search:

   ```id="93mrfv"
   Cloud Monitoring API
   ```

4. Click:

   ```id="8jlwm1"
   Enable
   ```

---

# 🔹 Step 3: Open Monitoring Dashboard

1. Click:

   ```id="gsgx7m"
   Navigation Menu (☰)
   ```

2. Go to:

   ```id="2b6jlp"
   Monitoring
   ```

3. Click:

   ```id="p4nl8h"
   Dashboards
   ```

---

# 🔹 Step 4: Create Dashboard

1. Click:

   ```id="2tmrm0"
   Create Dashboard
   ```

2. Enter dashboard name:

   ```id="17h5qf"
   MyDashboard
   ```

---

# 🔹 Step 5: Add Text Widget

1. Click:

   ```id="7c31aa"
   Add Widget
   ```

2. Select:

   ```id="gbd4uv"
   Text
   ```

3. Click:

   ```id="rjlwmv"
   Save
   ```

---

# 🔹 Step 6: Add Metric Widget

1. Again click:

   ```id="jlwm21"
   Add Widget
   ```

2. Select:

   ```id="5jlwm5"
   Metric
   ```

3. Choose:

   ```id="jbdlpv"
   GlobalBillingMetric
   ```

4. Select:

   ```id="9wwv9x"
   bytes ingested
   ```

5. Click:

   ```id="jlwmr8"
   Apply
   ```

---

# 🔹 Step 7: Select Graph

1. Choose:

   ```id="d2rybc"
   Line Chart
   ```

2. Click:

   ```id="7w24n0"
   Apply
   ```

---

# ✅ OUTPUT

👉 Dashboard created successfully
👉 Metrics displayed in graph format

---

# 🔥 SUPER SHORT MEMORY FLOW

```id="k4yqnl"
Enable API → Monitoring → Dashboard → Add Widget → Metric → Line Chart
```
# ✅ EXP 6 — Cloud SQL (VERY SIMPLE STEPS)

---

# 🔹 Step 1: Open Cloud SQL

1. Open Google Cloud Console
2. Click:

```id="svyqxl"
Navigation Menu (☰)
```

3. Go to:

```id="ehjlwm"
Cloud SQL
```

4. Click:

```id="0j8a2o"
Create Instance
```

---

# 🔹 Step 2: Select Database

1. Choose:

```id="4fgr53"
MySQL
```

2. Select:

```id="jlwm8d"
Enterprise Edition
```

3. Choose preset:

```id="gjlwm1"
Development
```

---

# 🔹 Step 3: Configure Instance

1. Enter:

```id="3e33dn"
Instance ID = myinstance
```

2. Set password.

3. Select:

```id="kq3dcx"
MySQL Version = 8.0
```

4. Choose:

```id="6rjlwm"
Region and Zone
```

5. Click:

```id="mr6n6t"
Create Instance
```

---

# 🔹 Step 4: Add Network

1. Open created instance.

2. Go to:

```id="1utvjlwm"
Connections → Networking
```

3. Click:

```id="kr9f9s"
Add Network
```

4. Enter:

* Network name
* Your IP address

5. Save.

---

# 🔹 Step 5: Connect Using Cloud Shell

Open Cloud Shell and run:

```bash id="dzprwk"
gcloud sql connect myinstance --user=root
```

Enter password.

---

# 🔹 Step 6: Create Database

```sql id="3r2d13"
CREATE DATABASE guestbook;
```

---

# 🔹 Step 7: Use Database

```sql id="jlwmvb"
USE guestbook;
```

---

# 🔹 Step 8: Create Table

```sql id="x7c4eo"
CREATE TABLE entries (
    guestName VARCHAR(255),
    content VARCHAR(255),
    entryID INT NOT NULL AUTO_INCREMENT,
    PRIMARY KEY(entryID)
);
```

---

# 🔹 Step 9: Insert Data

```sql id="c9xxqa"
INSERT INTO entries (guestName, content)
VALUES ("first guest", "I got here!");
```

```sql id="yjlr49"
INSERT INTO entries (guestName, content)
VALUES ("second guest", "Me too!");
```

---

# 🔹 Step 10: Display Data

```sql id="jlwmxf"
SELECT * FROM entries;
```

---

# ✅ OUTPUT

👉 Database created successfully
👉 Records inserted and displayed

---

# 🔥 MEMORY FLOW

```id="aq2g14"
Create Instance → Connect → Create DB → Create Table → Insert → Select
```

---

# ✅ EXP 7 — Pub/Sub (VERY SIMPLE STEPS)

---

# 🔹 Step 1: Create Topic

```bash id="7fwmqc"
gcloud pubsub topics create myTopic
```

---

# 🔹 Step 2: Create More Topics

```bash id="hjlwm1"
gcloud pubsub topics create Test1
```

```bash id="6o7qv8"
gcloud pubsub topics create Test2
```

---

# 🔹 Step 3: List Topics

```bash id="jlwm7r"
gcloud pubsub topics list
```

---

# 🔹 Step 4: Delete Extra Topics

```bash id="jlwmf3"
gcloud pubsub topics delete Test1
```

```bash id="3jlwm0"
gcloud pubsub topics delete Test2
```

---

# 🔹 Step 5: Create Subscription

```bash id="95jlwm"
gcloud pubsub subscriptions create \
--topic myTopic mySubscription
```

---

# 🔹 Step 6: Create More Subscriptions

```bash id="jlwm5q"
gcloud pubsub subscriptions create \
--topic myTopic Test1
```

```bash id="jlwm6q"
gcloud pubsub subscriptions create \
--topic myTopic Test2
```

---

# 🔹 Step 7: List Subscriptions

```bash id="jlwm2s"
gcloud pubsub topics list-subscriptions myTopic
```

---

# 🔹 Step 8: Delete Extra Subscriptions

```bash id="jlwm8n"
gcloud pubsub subscriptions delete Test1
```

```bash id="q8jlwm"
gcloud pubsub subscriptions delete Test2
```

---

# 🔹 Step 9: Publish Message

```bash id="jlwm4v"
gcloud pubsub topics publish myTopic \
--message "Hello"
```

---

# 🔹 Step 10: Publish More Messages

```bash id="jlwm9w"
gcloud pubsub topics publish myTopic \
--message "Publisher's name is GURU"
```

```bash id="jlwm1z"
gcloud pubsub topics publish myTopic \
--message "Publisher likes to eat FOOD"
```

---

# 🔹 Step 11: Pull Messages

```bash id="jlwm0x"
gcloud pubsub subscriptions pull mySubscription --auto-ack
```

---

# ✅ OUTPUT

👉 Topics and subscriptions created
👉 Messages published and received successfully

---

# 🔥 MEMORY FLOW

```id="jlwm3m"
Create Topic → Subscription → Publish → Pull
```

# lab8
