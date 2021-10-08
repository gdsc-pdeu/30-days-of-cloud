# Quest-6: Deploy to Kubernetes in Google Cloud
## Lab-5: Continuous Delivery with Jenkins in Kubernetes Engine
<details> 
  <summary><b>Task 1: Clone the repository</b></summary>
  <br/>
  <p>
    
1. To get set up, open a new session in Cloud Shell and run the following command to set your zone us-east1-d:
    ```
    gcloud config set compute/zone us-east1-d
    ```
2. Then clone the lab's sample code:
    ```
    git clone https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes.git
    ```
3. Now change to the correct directory:
    ```
    cd continuous-deployment-on-kubernetes
    ```

  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Provisioning Jenkins</b></summary>
  <br/>
  <p>
    
1. Creating a Kubernetes cluster
    a. Now, run the following command to provision a Kubernetes cluster:
    ```
    gcloud container clusters create jenkins-cd \
    --num-nodes 2 \
    --machine-type n1-standard-2 \
    --scopes "https://www.googleapis.com/auth/source.read_write,cloud-platform"
    ```
    b. Before continuing, confirm that your cluster is running by executing the following command:
    ```
    gcloud container clusters list
    ```
    c. Now, get the credentials for your cluster:
    ```
    gcloud container clusters get-credentials jenkins-cd
    ```
    d. Kubernetes Engine uses these credentials to access your newly provisioned cluster—confirm that you can connect to it by running the following command:
    ```
    kubectl cluster-info
    ```
 
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Setup Helm</b></summary>
  <br/>
  <p>
    
1. Add Helm's stable chart repo:

   ```
   helm repo add jenkins https://charts.jenkins.io
   ```
   
2. Ensure the repo is up to date:
    ```
    helm repo update
    ```

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Configure and Install Jenkins</b></summary>
  <br/>
  <p>
    
1. Download the custom values file:
   ```
   gsutil cp gs://spls/gsp330/values.yaml jenkins/values.yaml
   ```
   
2. Use the Helm CLI to deploy the chart with your configuration settings.
    ```
    helm install cd jenkins/jenkins -f jenkins/values.yaml --wait
    ```
3. Once that command completes ensure the Jenkins pod goes to the Running state and the container is in the READY state:
    ```
    export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
    kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
    ```
  
4. Now, check that the Jenkins Service was created properly:
    ```
    kubectl get svc
 
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Connect to Jenkins</b></summary>
  <br/>
  <p>
    
1. The Jenkins chart will automatically create an admin password for you. To retrieve it, run:

   ```
   printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
   ```
   
2. To get to the Jenkins user interface, click on the Web Preview button in cloud shell, then click Preview on port 8080:
3. You should now be able to log in with username admin and your auto-generated password.
  
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 6: Deploying the Application</b></summary>
  <br/>
  <p>
    
1. In Google Cloud Shell, navigate to the sample application directory:
    
     ```
     cd sample-app
     ```
   
2. Create the Kubernetes namespace to logically isolate the deployment:
    ```
    kubectl create ns production
    ```
3. Create the production and canary deployments, and the services using the kubectl apply commands:
    ```
    kubectl apply -f k8s/production -n production
    ```
    ```
    kubectl apply -f k8s/canary -n production
    ```
    ```
    kubectl apply -f k8s/services -n production
    ```
    
4. Scale up the production environment frontends by running the following command:
    ```
    kubectl scale deployment gceme-frontend-production -n production --replicas 4
    ```
    
5. Now confirm that you have 5 pods running for the frontend, 4 for production traffic and 1 for canary releases (changes to the canary release will only affect 1 out of 5 (20%) of users):
    ```
    kubectl get pods -n production -l app=gceme -l role=frontend
    ```
    
6. Also confirm that you have 2 pods for the backend, 1 for production and 1 for canary:
    ```
    kubectl get pods -n production -l app=gceme -l role=backend
    ```
    
7. Retrieve the external IP for the production services:
    ```
    kubectl get service gceme-frontend -n production
    ```
    
8. Now, store the frontend service load balancer IP in an environment variable for use later:
    ```
    export FRONTEND_SERVICE_IP=$(kubectl get -o jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
    ```
  
9. Confirm that both services are working by opening the frontend external IP address in your browser. Check the version output of the service by running the following command (it should read 1.0.0):
    ```
    curl http://$FRONTEND_SERVICE_IP/version
    ```
  
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 7: Creating the Jenkins Pipeline</b></summary>
  <br/>
  <p>
    
1. Create a copy of the gceme sample app and push it to a Cloud Source Repository:
    ```
    gcloud source repos create default
    ```
    ```
    git init
    ```
   
2. Initialize the sample-app directory as its own Git repository:
    ```
    git config credential.helper gcloud.sh
    ```
3. Run the following command:
    ```
    git remote add origin https://source.developers.google.com/p/$DEVSHELL_PROJECT_ID/r/default
    ```
    
4. Set the username and email address for your Git commits. Replace [EMAIL_ADDRESS] with your Git email address and [USERNAME] with your Git username:
    ```
    git config --global user.email "[EMAIL_ADDRESS]"
    ```
    ```
    git config --global user.name "[USERNAME]"
    ```
    
5. Add, commit, and push the files:
    ```
    git add .
    ```
    ```
    git commit -m "Initial commit"
    ```
    ```
    git push origin master
    ```
    
6. Adding your service account credentials
    
    a. In the Jenkins user interface, click Manage Jenkins in the left navigation then click Manage Credentials.
    
    b. Click Jenkins
    
    c. Click Global credentials (unrestricted).
    
    d. Click Add Credentials in the left navigation.
    
    e. Select Google Service Account from metadata from the Kind drop-down and click OK.
    
    
7. Creating the Jenkins job
    
    a. Click New Item in the left navigation:
    
    b. Name the project sample-app, then choose the Multibranch Pipeline option and click OK.
    
    c. On the next page, in the Branch Sources section, click Add Source and select git.
    
    d. Paste the HTTPS clone URL of your sample-app repo in Cloud Source Repositories into the Project Repository field. Replace [PROJECT_ID] with your Project ID
    
    ```
    https://source.developers.google.com/p/[PROJECT_ID]/r/default
    ```
    
    e. From the Credentials drop-down, select the name of the credentials you created when adding your service account in the previous steps
    .
    f. Under Scan Multibranch Pipeline Triggers section, check the Periodically if not otherwise run box and set the Interval value to 1 minute.
    
    g. Your job configuration should look like this:
    
    h. Click Save leaving all other options with their defaults
    
 
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 8: Creating the Development Environment</b></summary>
  <br/>
  <p>
    
1. Creating a development branch
    a. Create a development branch and push it to the Git server:
    ```
    git checkout -b new-feature
    ```
   
2. Modifying the pipeline definition
    a. Open the Jenkinsfile in your terminal editor, for example vi:
    ```
    vi Jenkinsfile
    ```
    ```
    i
    ```
    b. Add your PROJECT_ID to the REPLACE_WITH_YOUR_PROJECT_ID value. (Your PROJECT_ID is your Project ID found in the CONNECTION DETAILS section of the lab. You can also run gcloud config get-value project to find it.
    ```
    PROJECT = "REPLACE_WITH_YOUR_PROJECT_ID"
    APP_NAME = "gceme"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "jenkins-cd"
    CLUSTER_ZONE = "us-east1-d"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    JENKINS_CRED = "${PROJECT}"
    ```
    c. Save the Jenkinsfile file: hit Esc then (for vi users):
    ```
    :wq
    ```
3. Modify the site
    a. Open html.go:
    ```
    vi html.go
    ```
    b. Start the editor:
    ```
    i
    ```
    c. Change the two instances of div class="card blue" with following:
    ```
    div class="card orange"
    ```
    d. Save the html.go file: press Esc then:
    ```
    :wq
    ```
    e. Open main.go:
    ```
    vi main.go
    ```
    f. Start the editor:
    ```
    i
    ```
    g. The version is defined in this line:
    ```
    const version string = "1.0.0"
    ```
    h. Update it to the following:
    ```
    const version string = "2.0.0"
    ```
    i. Save the main.go file one more time: Esc then:
    ```
    :wq
    ```
  

    
  </p>
</details>
<br/>


<details> 
  <summary><b>Task 9: Kick off Deployment</b></summary>
  <br/>
  <p>
    
1. Commit and push your changes:
    ```
    git add Jenkinsfile html.go main.go
    ```
    ```
    git commit -m "Version 2.0.0"
    ```
    ```
    git push origin new-feature
    ```
   
2. Once that's all taken care of, start the proxy in the background:
    ```
    kubectl proxy &
    ```
    
3. If it stalls, press Ctrl + C to exit out. Verify that your application is accessible by sending a request to localhost and letting kubectl proxy forward it to your service:
    ```
    curl \
    http://localhost:8001/api/v1/namespaces/new-feature/services/gceme-frontend:80/proxy/version
    ```
  

    
  </p>
</details>
<br/>


<details> 
  <summary><b>Task 10: Deploying a Canary Release</b></summary>
  <br/>
  <p>
    
1. Create a canary branch and push it to the Git server:
    ```
    git checkout -b canary
    ```
    ```
    git push origin canary
    ```
   
2. In Jenkins, you should see the canary pipeline has kicked off. Once complete, you can check the service URL to ensure that some of the traffic is being served by your new version. You should see about 1 in 5 requests (in no particular order) returning version 2.0.0.
    ```
    export FRONTEND_SERVICE_IP=$(kubectl get -o \
    jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
    ```
    ```
    while true; do curl http://$FRONTEND_SERVICE_IP/version; sleep 1; done
    ```
    
3. If you keep seeing 1.0.0, try running the above commands again. Once you've verified that the above works, end the command with Ctrl + C.
  

    
  </p>
</details>
<br/>


<details> 
  <summary><b>Task 11: Deploying to production</b></summary>
  <br/>
  <p>
    
1. Create a canary branch and push it to the Git server:
    ```
    git checkout master
    ```
    ```
    git merge canary
    ```
    ```
    git push origin master
    ```
   
2. In Jenkins, you should see the master pipeline has kicked off. Once complete (which may take a few minutes), you can check the service URL to ensure that all of the traffic is being served by your new version, 2.0.0.
    ```
    export FRONTEND_SERVICE_IP=$(kubectl get -o \
    jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
    ```
    ```
    while true; do curl http://$FRONTEND_SERVICE_IP/version; sleep 1; done
    ```
    
3. You can also navigate to site on which the gceme application displays the info cards. The card color changed from blue to orange. Here's the command again to get the external IP address so you can check it out:
    ```
    kubectl get service gceme-frontend -n production
    ```
  

    
  </p>
</details>
<br/>






<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Which are the following Kubernetes namespaces used in the lab?
  - [ ] production
  - [X] default
  - [X] helm
  - [ ] jenkins
  - [X] kube-system

 
- Q. The Helm chart is a collection of files that describe a related set of Kubernetes resources.
  - [X] True
  - [ ] False
    
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
