# Module 2

## Serverless Data Analysis with Dataflow - Lab 1 : A Simple Dataflow Pipeline (Python) v1.3

*Objective*

* Setup a Python Dataflow project using Apache Beam
* Write a simple pipeline in Python
* Execute the query on the local machine
* Execute the query on the cloud

```sh

google4552907_student@cloudshell:~ (qwiklabs-gcp-1e7c10eadc587612)$ gcloud auth list

          Credentialed Accounts
ACTIVE  ACCOUNT
*       google4552907_student@qwiklabs.net

To set the active account, run:
    $ gcloud config set account `ACCOUNT`

google4552907_student@cloudshell:~ (qwiklabs-gcp-1e7c10eadc587612)$
google4552907_student@cloudshell:~ (qwiklabs-gcp-1e7c10eadc587612)$

```

```sh
google4552907_student@cloudshell:~ (qwiklabs-gcp-1e7c10eadc587612)$ gcloud config list project
[core]
project = qwiklabs-gcp-1e7c10eadc587612

Your active configuration is: [cloudshell-21073]
google4552907_student@cloudshell:~ (qwiklabs-gcp-1e7c10eadc587612)$
```
```sh
git clone https://github.com/GoogleCloudPlatform/training-data-analyst

BUCKET="qwiklabs-gcp-1e7c10eadc587612"
echo $BUCKET
```

```sh
# ERROR: google-cloud-storage 1.17.0 has requirement google-cloud-core<2.0dev,>=1.0.0, but you'll have google-cloud-core 0.29.1 which is incompatible.
sudo apt-get update
sudo apt-get upgrade
cd ~/training-data-analyst/courses/data_analysis/lab2/python
sudo ./install_packages.sh
```

```sh
cd ~/training-data-analyst/courses/data_analysis/lab2/python
nano grep.py
```

```sh
cd ~/training-data-analyst/courses/data_analysis/lab2/python
python grep.py
```

```sh
gsutil cp ../javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/*.java gs://$BUCKET/javahelp
```

```sh
echo $DEVSHELL_PROJECT_ID
echo $BUCKET
python grepc.py
```

## Serverless Data Analysis with Dataflow - Lab 2 : MapReduce in Dataflow (Python) v1.3

*Objective*

* Identify Map and Reduce operations
* Execute the pipeline
* Use command line parameters



```sh
git clone https://github.com/GoogleCloudPlatform/training-data-analyst

#ERROR: google-cloud-storage 1.17.0 has requirement google-cloud-core<2.0dev,>=1.0.0, but you'll have google-cloud-core 0.29.1 which is incompatible.
#ERROR: google-cloud-translate 1.6.0 has requirement google-cloud-core<2.0dev,>=1.0.0, but you'll have google-cloud-core 0.29.1 which is incompatible.
#ERROR: google-cloud-spanner 1.10.0 has requirement google-cloud-core<2.0dev,>=1.0.0, but you'll have google-cloud-core 0.29.1 which is incompatible.
#ERROR: google-cloud-spanner 1.10.0 has requirement grpc-google-iam-v1<0.13dev,>=0.12.3, but you'll have grpc-google-iam-v1 0.11.4 which is incompatible.
#ERROR: google-apitools 0.5.28 has requirement six>=1.12.0, but you'll have six 1.10.0 which is incompatible.
#ERROR: jsonschema 3.0.1 has requirement six>=1.11.0, but you'll have six 1.10.0 which is incompatible.
# ERROR: google-cloud-logging 1.12.0 has requirement google-cloud-core<2.0dev,>=1.0.0, but you'll have google-cloud-core 0.29.1 which is incompatible.

sudo apt-get update -y
sudo apt-get upgrade -y

sudo apt-get install python-pip -y

sudo pip install apache-beam[gcp] oauth2client==3.0.0
# downgrade as 1.11 breaks apitools
sudo sudo pip install --force six==1.10
sudo pip install -U pip
sudo pip -V
sudo pip install apache_beam

sudo pip install -U jsonschema
sudo pip install -U google-apitools 
sudo pip install -U google-cloud-spanner 
sudo pip install -U google-cloud-pubsub 
sudo pip install -U google-cloud-bigtable
sudo pip install -U google-cloud-bigquery 
sudo pip install -U google-cloud-core 
sudo pip install -U google-cloud-storage 
sudo pip install -U google-cloud-translate 
```

```sh
cd ~/training-data-analyst/courses/data_analysis/lab2/python
nano is_popular.py
```

```sh
cd ~/training-data-analyst/courses/data_analysis/lab2/python
python ./is_popular.py
python ./is_popular.py --output_prefix=/tmp/myoutput
```

## Serverless Data Analysis with Dataflow - Lab 3 : Side Inputs (Python) v1.3

*Objective*

* Read data from BigQuery into Dataflow
* Use the output of a pipeline as a side-input to another pipeline

```sh
git clone https://github.com/GoogleCloudPlatform/training-data-analyst

BUCKET="qwiklabs-gcp-3af0e86f6bd93162"
echo $BUCKET
echo $DEVSHELL_PROJECT_ID

cd ~/training-data-analyst/courses/data_analysis/lab2/python
sudo apt-get update -y
sudo apt-get upgrade -y
sudo ./install_packages.sh
```

```sql
SELECT
  content
FROM
  `fh-bigquery.github_extracts.contents_java_2016`
LIMIT
  10;

SELECT
  COUNT(*)
FROM
  `fh-bigquery.github_extracts.contents_java_2016`  
```

```sh
python JavaProjectsThatNeedHelp.py --bucket $BUCKET --project $DEVSHELL_PROJECT_ID --DirectRunner
python JavaProjectsThatNeedHelp.py --bucket $BUCKET --project $DEVSHELL_PROJECT_ID --DataFlowRunner
```
