# Quest-1: Getting Started: Create and Manage Cloud Resources
## Lab-4: Kubernetes Engine: Qwik Start

<details> 
  <summary><b>Task 1: Set a default compute zone</b></summary>
  <br/>
  <p>
    
1. To set your default compute zone to us-central1-a, start a new session in Cloud Shell, and run the following command:
    ```
    gcloud config set compute/zone us-central1-a
    ```
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create a GKE cluster</b></summary>
  <br/>
  <p>
    
1. To create a cluster, run the following command, replacing [CLUSTER-NAME] with the name you choose for the cluster (for example:my-cluster).
   ```
   gcloud container clusters create [CLUSTER-NAME]
   ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Get authentication credentials for the cluster</b></summary>
  <br/>
  <p>
    
1. To authenticate the cluster, run the following command, replacing [CLUSTER-NAME] with the name of your cluster:
   ```
   gcloud container clusters get-credentials [CLUSTER-NAME]
   ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Deploy an application to the cluster</b></summary>
  <br/>
  <p>
    
1. To create a new Deployment hello-server from the hello-app container image, run the following kubectl create command:
   ```
   kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
   ```
2. To create a Kubernetes Service, which is a Kubernetes resource that lets you expose your application to external traffic, run the following kubectl expose command:
    ```
    kubectl expose deployment hello-server --type=LoadBalancer --port 8080
    ```
 3. To inspect the hello-server Service, run kubectl get:
    ```
    kubectl get service
    ```
 4. To view the application from your web browser, open a new tab and enter the following address, replacing [EXTERNAL IP] with the EXTERNAL-IP for hello-server.
    ```
    http://[EXTERNAL-IP]:8080
    ```
  </p>
</details>
<br/>


<details> 
  <summary><b>Task 5: Deleting the cluster</b></summary>
  <br/>
  <p>
    
1. To delete the cluster, run the following command:
   ```
   gcloud container clusters delete [CLUSTER-NAME]
   ```
2. When prompted, type Y to confirm.

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
