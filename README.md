# Google Cloud Platform Kubernetes Cluster Terraform Module

## Cluster deployment
In order to deploy kubernetes cluster in GCP. You need to clone a cluster-infrastructure repository from Fuchicorp and go to the kube-cluster folder 
```

git clone
```



### Set up Google Cloud Platform 
First you will need to authenticate to Google Cloud platform to be able to set-up from local
```
gcloud auth login
```

After you login you will need to set the project bay following command

```
gcloud projects list # Get the project ID and use it in the next command
gcloud config set project $YOUR PROJECT ID 
```
This script creates a service account and binds the following roles to it. It also creates JSON `cluster-service-account.json` file for your service account and bucket. When you run the script you have to provide as user input unique bucket name ( in our example it is example-bucket)
```
source setup-google-platform.sh example-bucket
```



### Create terraform configuration 
Create `cluster.tfvars` file with following content:
```
google_project_id        = ""
cluster_name             = "project-cluster"
cluster_version          = "1.16"
google_bucket_name       = ""
deployment_environment   = ""
google_credentials_json  = "cluster-service-account.json"
deployment_name          = "cluster-infrastructure"
google_region            = "us-west1-b"
```

### Terraform apply 
Use the set-env.sh file to be able to set up local environment variables
```    
source set-env.sh cluster.tfvars
```

We need to plan all changes before applying them. 
```
terraform plan -var-file=$DATAFILE ## Displays what would be executed
```

For applying all changes we need to run the following command:
```
terraform apply  -var-file=$DATAFILE
```



### The terraform will create following resources on Google Cloud Platform
1. Kubernetes cluster 'project-cluster`
2. Google Service Account `cluster-service-account`

After you have deployed you should use following command to get `~/.kube/config` to be able to get access to kubernetes cluster
```
gcloud container clusters get-credentials project-cluster --zone us-west1-b
```
