# Quest-1: Getting Started: Create and Manage Cloud Resources
## Lab-6: Create and Manage Cloud Resources: Challenge Lab

<details> 
  <summary><b>Task 1: Create a project jumphost instance</b></summary>
  <br/>
  <p>
    

1. Run the following from the **Cloud Terminal**:
    ```yaml
    gcloud compute instances create nucleus-jumphost \
              --network nucleus-vpc \
              --zone us-east1-b  \
              --machine-type f1-micro  \
              --image-family debian-9  \
              --image-project debian-cloud \
              --scopes cloud-platform \
              --no-address
    ```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create a Kubernetes service cluster</b></summary>
  <br/>
  <p>
    

1. gcloud config set compute/zone us-east1-b
2. gcloud container clusters create nucleus-webserver1
3. gcloud container clusters get-credentials nucleus-webserver1
4. kubectl create deployment hello-app --image=gcr.io/google-samples/hello-app:2.0
5. kubectl expose deployment hello-app --type=LoadBalancer --port 8080
6. kubectl get service - (Wait for 1 Min)
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Setup an HTTP load balancer</b></summary>
  <br/>
  <p>
    
  ```
  cat << EOF > startup.sh
  #! /bin/bash
  apt-get update
  apt-get install -y nginx
  service nginx start
  sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
  EOF
  ```

1 .Create an instance template :
  ```
  gcloud compute instance-templates create web-server-template \
  --metadata-from-file startup-script=startup.sh \
  --network nucleus-vpc \
  --machine-type g1-small \
  --region us-east1
  ```

2 .Create a managed instance group :
  ```
  gcloud compute instance-groups managed create web-server-group \
  --base-instance-name web-server \
  --size 2 \
  --template web-server-template \
  --region us-east1
  ```

3 .Create a firewall rule to allow traffic (80/tcp) :
  ```
  gcloud compute firewall-rules create web-server-firewall \
  --allow tcp:80 \
  --network nucleus-vpc
  ```

4 .Create a health check :
  ```
  gcloud compute http-health-checks create http-basic-check
  ```

5 .Create a backend service and attach the manged instance group :
  ```
  gcloud compute instance-groups managed \
  set-named-ports web-server-group \
  --named-ports http:80 \
  --region us-east1
  ```

6 .Create a backend service and attach the manged instance group :
  ```
  gcloud compute backend-services create web-server-backend \
  --protocol HTTP \
  --http-health-checks http-basic-check \
  --global
  ```

7 .Create a URL map and target HTTP proxy to route requests to your URL map :
  ```
  gcloud compute backend-services add-backend web-server-backend \
  --instance-group web-server-group \
  --instance-group-region us-east1 \
  --global
  ```
  ```
  gcloud compute url-maps create web-server-map \
  --default-service web-server-backend
  ```
  ```
  gcloud compute target-http-proxies create http-lb-proxy \
  --url-map web-server-map
  ```
8 .Create a forwarding rule :
   ```
    gcloud compute forwarding-rules create http-content-rule \
    --global \
    --target-http-proxy http-lb-proxy \
    --ports 80
   ```
   ```
    gcloud compute forwarding-rules list
   ```
9. WAIT FOR 5-10 MIN THEN CHECK PROGRESS....
    
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
