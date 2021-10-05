# Quest-3: Set Up and Configure a Cloud Environment in Google Cloud
## Lab-6: Set Up and Configure a Cloud Environment in Google Cloud: Challenge Lab

<details> 
  <summary><b>Task 1: Create development VPC manually</b></summary>
  <br/>
  <p>
    
```
gcloud compute networks create griffin-dev-vpc --subnet-mode custom
```
```
gcloud compute networks subnets create griffin-dev-wp --network=griffin-dev-vpc --region us-east1 --range=192.168.16.0/20
```
```
gcloud compute networks subnets create griffin-dev-mgmt --network=griffin-dev-vpc --region us-east1 --range=192.168.32.0/20
```
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create production VPC using Deployment Manager</b></summary>
  <br/>
  <p>
    
```
gsutil cp -r gs://cloud-training/gsp321/dm .
```
```
cd dm
```
```
sed -i s/SET_REGION/us-east1/g prod-network.yaml
```
```
gcloud deployment-manager deployments create prod-network \
    --config=prod-network.yaml
```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create bastion host</b></summary>
  <br/>
  <p>
    
```
cd ..
```
```
gcloud compute instances create bastion --network-interface=network=griffin-dev-vpc,subnet=griffin-dev-mgmt  --network-interface=network=griffin-prod-vpc,subnet=griffin-prod-mgmt --tags=ssh --zone=us-east1-b
```
```
gcloud compute firewall-rules create fw-ssh-dev --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-dev-vpc
```
```
gcloud compute firewall-rules create fw-ssh-prod --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-prod-vpc
```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Create and configure Cloud SQL Instance</b></summary>
  <br/>
  <p>
    
```
gcloud sql instances create griffin-dev-db --root-password password --region=us-east1
```
```
gcloud sql connect griffin-dev-db (Enter password: password)
```
```

CREATE DATABASE wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO "wp_user"@"%" IDENTIFIED BY "stormwind_rules";
FLUSH PRIVILEGES;
```
```
exit;
```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Create Kubernetes cluster</b></summary>
  <br/>
  <p>
    
```
gcloud container clusters create griffin-dev \
  --network griffin-dev-vpc \
  --subnetwork griffin-dev-wp \
  --machine-type n1-standard-4 \
  --num-nodes 2  \
  --zone us-east1-b
```
```
gcloud container clusters get-credentials griffin-dev --zone us-east1-b
```
    
  </p>
</details>
<br/>  
    
<details> 
  <summary><b>Task 6: Prepare the Kubernetes cluster</b></summary>
  <br/>
  <p>
    
```
gsutil cp -r gs://cloud-training/gsp321/wp-k8s .
```
```
cd wp-k8s
```
```
sed -i s/username_goes_here/wp_user/g wp-env.yaml
```
```
sed -i s/password_goes_here/stormwind_rules/g wp-env.yaml
```
```
kubectl create -f wp-env.yaml
```
```
gcloud iam service-accounts keys create key.json \
    --iam-account=cloud-sql-proxy@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com
```
```
kubectl create secret generic cloudsql-instance-credentials \
    --from-file key.json
```




  </p>
</details>
<br/>

<details> 
  <summary><b>Task 7: Create a WordPress deployment</b></summary>
  <br/>
  <p>
    
```
cd ~/wp-k8s edit wp-deployment.yaml
```
```
Replace YOUR_SQL_INSTANCE with griffin-dev-db
```
```
kubectl create -f wp-deployment.yaml
```
```
kubectl create -f wp-service.yaml
```

  </p>
</details>
<br/>
    
<details> 
  <summary><b>Task 8: Enable monitoring</b></summary>
  <br/>
  <p>
    
1. Go to OPERATIONS > Monitoring
    
    a. Wait for the workspace creation to complete
    
2. Go to Uptime checks > CREATE UPTIME CHECKS
3. Now enter the info as below:
```
Title: WordPress Uptime
Check Type: HTTP
Resource Type: URL
Hostname: YOUR-WORDPRESS_ENDPOINT
Path: /
```

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 9: Provide access for an additional engineer</b></summary>
  <br/>
  <p>
    
1. Go to IAM & Admin > ADD
2. Enter the second username and give him Project > Editor access in Role

  </p>
</details>
<br/>



## Connect With Us:

**GDSC PDEU:**
- Join Us: https://gdsc.community.dev/pandit-deendayal-petroleum-university/
- YouTube: https://www.youtube.com/channel/UCv4guMYXw_6oc9KSmVlbebA
- GitHub: https://github.com/gdsc-pdeu
- LinkedIn: https://linkedin.com/company/developer-student-clubs-pdeu
- Instagram: https://www.instagram.com/dsc.pdeu/

**GDSC Lead - Jay Gohil:**
- Website: https://jay-gohil.me/
- LinkedIn: https://www.linkedin.com/in/jay--gohil/
- GitHub: https://github.com/gohil-jay
- Instagram: https://www.instagram.com/_jay.gohil/

**GCP Facilitator - Jay Patel:**
- Website: http://pateljay.me/
- LinkedIn: https://www.linkedin.com/in/--jaypatel--/
- GitHub: https://github.com/jaypatel31
- Instagram: https://www.instagram.com/jaypatel98196/
