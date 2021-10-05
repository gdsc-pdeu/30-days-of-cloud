# Quest-3: Set Up and Configure a Cloud Environment in Google Cloud
## Lab-5: Managing Deployments Using Kubernetes Engine

<details> 
  <summary><b>Task 1: Activate Cloud SHell</b></summary>
  <br/>
  <p>
    
1. Set your working Google Cloud zone by running the following command, substituting the local zone as us-central1-a:
    ```
    gcloud config set compute/zone us-central1-a
    ```
2. Get the sample code for creating and running containers and deployments:
    ```
    gsutil -m cp -r gs://spls/gsp053/orchestrate-with-kubernetes .
    cd orchestrate-with-kubernetes/kubernetes
    ```
3. Create a cluster with five n1-standard-1 nodes (this will take a few minutes to complete):
    ```
    gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
    ```
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Learn about the deployment object</b></summary>
  <br/>
  <p>
    
1. Let's get started with Deployments. First let's take a look at the Deployment object. The explain command in kubectl can tell us about the Deployment object.
    ```
    kubectl explain deployment
    ```
2. We can also see all of the fields using the --recursive option.
    ```
    kubectl explain deployment --recursive
    ```
3. You can use the explain command as you go through the lab to help you understand the structure of a Deployment object and understand what the individual fields do.
    ```
    kubectl explain deployment.metadata.name
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create a deployment</b></summary>
  <br/>
  <p>
    
1. Update the deployments/auth.yaml configuration file:
    ```
    vi deployments/auth.yaml
    ```
2. Start the editor:
    ```
    i
    ```
3. Change the image in the containers section of the Deployment to the following:
    ```
    ...
    containers:
    - name: auth
      image: "kelseyhightower/auth:1.0.0"
    ...
    ```
4. Save the auth.yaml file: press <Esc> then type:
    ```
    :wq
    ```
5. Press <Enter>. Now let's create a simple deployment. Examine the deployment configuration file:
    ```
    cat deployments/auth.yaml
    ```
6. Go ahead and create your deployment object using kubectl create:
    ```
    kubectl create -f deployments/auth.yaml
    ```
7. Once you have created the Deployment, you can verify that it was created.
    ```
    kubectl get deployments
    ```
8. Once the deployment is created, Kubernetes will create a ReplicaSet for the Deployment. We can verify that a ReplicaSet was created for our Deployment:
    ```
    kubectl get replicasets
    ```
9. Finally, we can view the Pods that were created as part of our Deployment. The single Pod is created by the Kubernetes when the ReplicaSet is created.
    ```
    kubectl get pods
    ```
10. It's time to create a service for our auth deployment. You've already seen service manifest files, so we won't go into the details here. Use the kubectl create command to create the auth service.
    ```
    kubectl create -f services/auth.yaml
    ```
11. Now, do the same thing to create and expose the hello Deployment.
    ```
    kubectl create -f deployments/hello.yaml
    kubectl create -f services/hello.yaml
    ```
12. And one more time to create and expose the frontend Deployment.
    ```
    kubectl create secret generic tls-certs --from-file tls/
    kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
    kubectl create -f deployments/frontend.yaml
    kubectl create -f services/frontend.yaml
    ```
13. Interact with the frontend by grabbing its external IP and then curling to it.
    ```
    kubectl get services frontend
    ```
14. Scale a Deployment
    a. Now that we have a Deployment created, we can scale it. Do this by updating the spec.replicas field. You can look at an explanation of this field using the kubectl explain command again.
    ```
    kubectl explain deployment.spec.replicas
    ```
    b.The replicas field can be most easily updated using the kubectl scale command:
    ```
    kubectl scale deployment hello --replicas=5
    ```
    c. Verify that there are now 5 hello Pods running:
    ```
    kubectl get pods | grep hello- | wc -l
    ```
    d. Now scale back the application:
    ```
    kubectl scale deployment hello --replicas=3
    ```
    e. Again, verify that you have the correct number of Pods.
    ```
    kubectl get pods | grep hello- | wc -l
    ```
    

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Rolling update</b></summary>
  <br/>
  <p>
    
1. Trigger a rolling update
    a. To update your Deployment, run the following command:
    ```
    kubectl edit deployment hello
    ```

    b. Change the image in the containers section of the Deployment to the following:
    ```
    ...
    containers:
      image: kelseyhightower/hello:2.0.0
    ...
    ```

    c. Save and exit.
    
    d. See the new ReplicaSet that Kubernetes creates.:
    ```
    kubectl get replicaset
    ```

    e. You can also see a new entry in the rollout history:
    ```
    kubectl rollout history deployment/hello
    ```
    
2. Pause a rolling update
    a. If you detect problems with a running rollout, pause it to stop the update. Give that a try now:
    ```
    kubectl rollout pause deployment/hello
    ```
    b. Verify the current state of the rollout:
    ```
    kubectl rollout status deployment/hello
    ```

3. Resume a rolling update
    a. The rollout is paused which means that some pods are at the new version and some pods are at the older version. We can continue the rollout using the resume command.
    ```
    kubectl rollout resume deployment/hello
    ```
    b. When the rollout is complete, you should see the following when running the status command.
    ```
    kubectl rollout status deployment/hello
    ```
4. Rollback an update
    a. Use the rollout command to roll back to the previous version:
    ```
    kubectl rollout undo deployment/hello
    ```
    b. Verify the roll back in the history:
    ```
    kubectl rollout history deployment/hello
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Canary deployments</b></summary>
  <br/>
  <p>
    
1. First, create a new canary deployment for the new version:
    ```
    cat deployments/hello-canary.yaml
    ```
    
2. Now create the canary deployment:
    ```
    kubectl create -f deployments/hello-canary.yaml
    ```

3. After the canary deployment is created, you should have two deployments, hello and hello-canary. Verify it with this kubectl command:
    ```
    kubectl get deployments
    ```

4. Verify the canary deployment
    a. You can verify the hello version being served by the request:
    ```
    curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
    ```
  </p>
</details>
<br/>  
    
<details> 
  <summary><b>Task 6: Blue-green deployments</b></summary>
  <br/>
  <p>
    
1. The service
    a. First update the service:
    ```
    kubectl apply -f services/hello-blue.yaml
    ```
    
2. Updating using Blue-Green Deployment
    a. Create the green deployment:
    ```
    kubectl create -f deployments/hello-green.yaml
    ```
    b. Once you have a green deployment and it has started up properly, verify that the current version of 1.0.0 is still being used:
    ```
    curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
    ```
    c. Now, update the service to point to the new version:
    ```
    kubectl apply -f services/hello-green.yaml
    ```
    d. With the service is updated, the "green" deployment will be used immediately. You can now verify that the new version is always being used.
    ```
    curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
    ```

3. Blue-Green Rollback
    a. If necessary, you can roll back to the old version in the same way. While the "blue" deployment is still running, just update the service back to the old version.
    ```
    kubectl apply -f services/hello-blue.yaml
    ```
    b. Once you have updated the service, your rollback will have been successful. Again, verify that the right version is now being used:
    ```
    curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
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
