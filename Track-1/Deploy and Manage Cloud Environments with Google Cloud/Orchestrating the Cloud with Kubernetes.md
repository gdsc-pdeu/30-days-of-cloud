# Quest-4: Deploy and Manage Cloud Environments with Google Cloud
## Lab-1: Orchestrating the Cloud with Kubernetes

<details> 
  <summary><b>Task 1: Setup and Requirements</b></summary>
  <br/>
  <p>

1. Activate Cloud Shell    
2. Google Kubernetes Engine
    a. In the cloud shell environment type the following command to set the zone:
    ```
    gcloud config set compute/zone us-central1-b
    ```
    b. After you set the zone, start up a cluster for use in this lab:
    ```
    gcloud container clusters create io
    ``` 
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Get the sample code</b></summary>
  <br/>
  <p>
    
1. Clone the GitHub repository from the Cloud Shell command line:

   ```
   gsutil cp -r gs://spls/gsp021/* .
   ```
   
2. Change into the directory needed for this lab:
    ```
    cd orchestrate-with-kubernetes/kubernetes
    ```
3. List the files to see what you're working with:
    ```
    ls
    ```

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Quick Kubernetes Demo</b></summary>
  <br/>
  <p>
    
1. The easiest way to get started with Kubernetes is to use the kubectl create command. Use it to launch a single instance of the nginx container:

   ```
   kubectl create deployment nginx --image=nginx:1.10.0
   ```
   
2. In Kubernetes, all containers run in a pod. Use the kubectl get pods command to view the running nginx container:
    ```
    kubectl get pods
    ```
3. Once the nginx container has a Running status you can expose it outside of Kubernetes using the kubectl expose command:
    ```
    kubectl expose deployment nginx --port 80 --type LoadBalancer
    ```
4. List our services now using the kubectl get services command:
    ```
    kubectl get services
    ```
5. Add the External IP to this command to hit the Nginx container remotely:
    ```
    curl http://<External IP>:80
    ```

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Creating Pods</b></summary>
  <br/>
  <p>
    
1. Run the following:

   ```
   cat pods/monolith.yaml
   ```
   
2. Create the monolith pod using kubectl:
    ```
    kubectl create -f pods/monolith.yaml
    ```
3. Examine your pods. Use the kubectl get pods command to list all pods running in the default namespace:
    ```
    kubectl get pods
    ```
4. Once the pod is running, use kubectl describe command to get more information about the monolith pod:
    ```
    kubectl describe pods monolith
    ```

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Interacting with Pods</b></summary>
  <br/>
  <p>
    
1. In the **2nd terminal**, run this command to set up port-forwarding:

   ```
   kubectl port-forward monolith 10080:80
   ```
   
2. Now in the **1st terminal** start talking to your pod using curl:
    ```
    curl http://127.0.0.1:10080
    ```
3. Now use the curl command to see what happens when you hit a secure endpoint:
    ```
    curl http://127.0.0.1:10080/secure
    ```
4. Try logging in to get an auth token back from the monolith:
    ```
    curl -u user http://127.0.0.1:10080/login
    ```
5. Logging in caused a JWT token to print out. Since Cloud Shell does not handle copying long strings well, create an environment variable for the token.
    ```
    TOKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')
    ```
6. Enter the super-secret password "password" again when prompted for the host password.
7. Use this command to copy and then use the token to hit the secure endpoint with curl
    ```
    curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
    ```
8. Use the kubectl logs command to view the logs for the monolith Pod.
    ```
    kubectl logs monolith
    ```
9. Open a 3rd terminal and use the -f flag to get a stream of the logs happening in real-time:
    ```
    kubectl logs -f monolith
    ```
10. Now if you use curl in the 1st terminal to interact with the monolith, you can see the logs updating (in the 3rd terminal)
    ```
    curl http://127.0.0.1:10080
    ```
11. Use the kubectl exec command to run an interactive shell inside the Monolith Pod. This can come in handy when you want to troubleshoot from within a container:
    ```
    kubectl exec monolith --stdin --tty -c monolith /bin/sh
    ```
12. For example, once you have a shell into the monolith container you can test external connectivity using the ping command:
    ```
    ping -c 3 google.com
    ```
13. Be sure to log out when you're done with this interactive shell.
    ```
    exit
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 6: Creating a Service</b></summary>
  <br/>
  <p>
    
1. If you've changed directories, make sure you return to the ~/orchestrate-with-kubernetes/kubernetes directory:

   ```
   cd ~/orchestrate-with-kubernetes/kubernetes
   ```
   
2. Explore the monolith service configuration file:
    ```
    cat pods/secure-monolith.yaml
    ```
3. Create the secure-monolith pods and their configuration data:
    ```
    kubectl create secret generic tls-certs --from-file tls/
    kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
    kubectl create -f pods/secure-monolith.yaml
    ```
4. Explore the monolith service configuration file:
    ```
    cat services/monolith.yaml
    ```
5. Use the kubectl create command to create the monolith service from the monolith service configuration file:
    ```
    kubectl create -f services/monolith.yaml
    ```
6. Use the gcloud compute firewall-rules command to allow traffic to the monolith service on the exposed nodeport:
    ```
    gcloud compute firewall-rules create allow-monolith-nodeport \
      --allow=tcp:31000
    ```
7. First, get an external IP address for one of the nodes.
    ```
    gcloud compute instances list
    ```
8. Now try hitting the secure-monolith service using curl:
    ```
    curl -k https://<EXTERNAL_IP>:31000
    ```

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 7: Adding Labels to Pods</b></summary>
  <br/>
  <p>
    
1. You can see that you have quite a few pods running with the monolith label.

   ```
   kubectl get pods -l "app=monolith"
   ```
   
2. But what about "app=monolith" and "secure=enabled"?
    ```
    kubectl get pods -l "app=monolith,secure=enabled"
    ```
3. Use the kubectl label command to add the missing secure=enabled label to the secure-monolith Pod. Afterwards, you can check and see that your labels have been updated.
    ```
    kubectl label pods secure-monolith 'secure=enabled'
    kubectl get pods secure-monolith --show-labels
    ```
4. Now that your pods are correctly labeled, view the list of endpoints on the monolith service:
    ```
    kubectl describe services monolith | grep Endpoints
    ```
5. Test this out by hitting one of our nodes again.
    ```
    gcloud compute instances list
    curl -k https://<EXTERNAL_IP>:31000
    ```

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 8: Creating Deployments</b></summary>
  <br/>
  <p>
    
1. Get started by examining the auth deployment configuration file.

   ```
   cat deployments/auth.yaml
   ```
   
2.  go ahead and create your deployment object:
    ```
    kubectl create -f deployments/auth.yaml
    ```
3. It's time to create a service for your auth deployment. Use the kubectl create command to create the auth service:
    ```
    kubectl create -f services/auth.yaml
    ```
4. Now do the same thing to create and expose the hello deployment:
    ```
    kubectl create -f deployments/hello.yaml
    kubectl create -f services/hello.yaml
    ```
5. And one more time to create and expose the frontend Deployment.
    ```
    kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
    kubectl create -f deployments/frontend.yaml
    kubectl create -f services/frontend.yaml
    ```
6. Interact with the frontend by grabbing it's External IP and then curling to it:
    ```
    kubectl get services frontend
    ```
    

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
