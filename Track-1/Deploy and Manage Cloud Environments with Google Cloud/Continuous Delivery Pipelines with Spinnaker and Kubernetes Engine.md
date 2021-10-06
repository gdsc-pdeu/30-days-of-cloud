# Quest-4: Deploy and Manage Cloud Environments with Google Cloud
## Lab-2: Continuous Delivery Pipelines with Spinnaker and Kubernetes Engine

<details> 
  <summary><b>Task 1: Setup and Requirements</b></summary>
  <br/>
  <p>

1. Activate Cloud Shell    
2. Google Kubernetes Engine
    a. In the cloud shell environment type the following command to set the zone:
    ```
    gcloud config set compute/zone us-central1-f
    ```
    b. Create a Kubernetes Engine using the Spinnaker tutorial sample application:
    ```
    gcloud container clusters create spinnaker-tutorial \
    --machine-type=n1-standard-2
    ``` 
    c. Create the service account:
    ```
    gcloud iam service-accounts create spinnaker-account \
    --display-name spinnaker-account
    ```
    d. Store the service account email address and your current project ID in environment variables for use in later commands:
    ```
    export SA_EMAIL=$(gcloud iam service-accounts list \
    --filter="displayName:spinnaker-account" \
    --format='value(email)')
    ```
    ```
    export PROJECT=$(gcloud info --format='value(config.project)')
    ```
    e. Bind the storage.admin role to your service account:
    ```
    gcloud projects add-iam-policy-binding $PROJECT \
    --role roles/storage.admin \
    --member serviceAccount:$SA_EMAIL
    ```
    f. Download the service account key. In a later step, you will install Spinnaker and upload this key to Kubernetes Engine:
    ```
    gcloud iam service-accounts keys create spinnaker-sa.json \
     --iam-account $SA_EMAIL
    ```

  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Set up Cloud Pub/Sub to trigger Spinnaker pipelines</b></summary>
  <br/>
  <p>
    
1. Create the Cloud Pub/Sub topic for notifications from Container Registry.
    ```
    gcloud pubsub topics create projects/$PROJECT/topics/gcr
    ```
2. Create a subscription that Spinnaker can read from to receive notifications of images being pushed.
    ```
    gcloud pubsub subscriptions create gcr-triggers \
    --topic projects/${PROJECT}/topics/gcr
    ```
3. Give Spinnaker's service account permissions to read from the gcr-triggers subscription.
    ```
    export SA_EMAIL=$(gcloud iam service-accounts list \
        --filter="displayName:spinnaker-account" \
        --format='value(email)')
    ```
    ```
    gcloud beta pubsub subscriptions add-iam-policy-binding gcr-triggers \
        --role roles/pubsub.subscriber --member serviceAccount:$SA_EMAIL``
    ```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Deploying Spinnaker using Helm</b></summary>
  <br/>
  <p>
    
1. Download and install the helm binary:
    ```
    wget https://get.helm.sh/helm-v3.1.1-linux-amd64.tar.gz
    ```

2. Unzip the file to your local system:
    ```
    tar zxfv helm-v3.1.1-linux-amd64.tar.gz
    ```
    ```
    cp linux-amd64/helm .
    ```

3. Grant Helm the cluster-admin role in your cluster:
    ```
    kubectl create clusterrolebinding user-admin-binding \
        --clusterrole=cluster-admin --user=$(gcloud config get-value account)
    ```

4. Grant Spinnaker the cluster-admin role so it can deploy resources across all namespaces:
    ```
    kubectl create clusterrolebinding --clusterrole=cluster-admin \
        --serviceaccount=default:default spinnaker-admin
    ```

5. Add the stable charts deployments to Helm's usable repositories (includes Spinnaker):
    ```
    ./helm repo add stable https://charts.helm.sh/stable
    ./helm repo update
    ```

6. Still in Cloud Shell, create a bucket for Spinnaker to store its pipeline configuration:
    ```
    export PROJECT=$(gcloud info \
        --format='value(config.project)')
    ```
    ```
    export BUCKET=$PROJECT-spinnaker-config
    ```
    ```
    gsutil mb -c regional -l us-central1 gs://$BUCKET
    ```

7. Run the following command to create a spinnaker-config.yaml file, which describes how Helm should install Spinnaker:
    ```
    export SA_JSON=$(cat spinnaker-sa.json)
    export PROJECT=$(gcloud info --format='value(config.project)')
    export BUCKET=$PROJECT-spinnaker-config
    cat > spinnaker-config.yaml <<EOF
    gcs:
      enabled: true
      bucket: $BUCKET
      project: $PROJECT
      jsonKey: '$SA_JSON'
    dockerRegistries:
    - name: gcr
      address: https://gcr.io
      username: _json_key
      password: '$SA_JSON'
      email: 1234@5678.com
    # Disable minio as the default storage backend
    minio:
      enabled: false
    # Configure Spinnaker to enable GCP services
    halyard:
      spinnakerVersion: 1.19.4
      image:
        repository: us-docker.pkg.dev/spinnaker-community/docker/halyard
        tag: 1.32.0
        pullSecrets: []
      additionalScripts:
        create: true
        data:
          enable_gcs_artifacts.sh: |-
            \$HAL_COMMAND config artifact gcs account add gcs-$PROJECT --json-path /opt/gcs/key.json
            \$HAL_COMMAND config artifact gcs enable
          enable_pubsub_triggers.sh: |-
            \$HAL_COMMAND config pubsub google enable
            \$HAL_COMMAND config pubsub google subscription add gcr-triggers \
              --subscription-name gcr-triggers \
              --json-path /opt/gcs/key.json \
              --project $PROJECT \
              --message-format GCR
    EOF
    ```


8. Use the Helm command-line interface to deploy the chart with your configuration set:
    ```
    ./helm install -n default cd stable/spinnaker -f spinnaker-config.yaml \
               --version 2.0.0-rc9 --timeout 10m0s --wait
    ```

9. After the command completes, run the following command to set up port forwarding to Spinnaker from Cloud Shell:
    ```
    export DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" \
        -o jsonpath="{.items[0].metadata.name}")
    ```
    ```
    kubectl port-forward --namespace default $DECK_POD 8080:9000 >> /dev/null &
    ```

10. To open the Spinnaker user interface, click the Web Preview icon at the top of the Cloud Shell window and select Preview on port 8080.


  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Building the Docker image</b></summary>
  <br/>
  <p>
    
1. In Cloud Shell tab and download the sample application source code:

   ```
   gsutil -m cp -r gs://spls/gsp114/sample-app.tar .
   ```
   
2. Unpack the source code:
    ```
    mkdir sample-app
    tar xvf sample-app.tar -C ./sample-app
    ```
3. Change directories to the source code:
    ```
    cd sample-app
    ```
4. Set the username and email address for your Git commits in this repository. Replace [USERNAME] with a username you create:
    ```
    git config --global user.email "$(gcloud config get-value core/account)"
    ```
    ```
    git config --global user.name "[USERNAME]"
    ```
5. Make the initial commit to your source code repository:
    ```
    git init
    ```
    ```
    git add .
    ```
    ```
    git commit -m "Intial Commit"
    ```
6. Create a repository to host your code:
    ```
    gcloud source repos create sample-app
    ```
    ```
    git config credential.helper gcloud.sh
    ```
7. Add your newly created repository as remote:
    ```
    export PROJECT=$(gcloud info --format='value(config.project)')
    ```
    ```
    git remote add origin https://source.developers.google.com/p/$PROJECT/r/sample-app
    ```
8. Push your code to the new repository's master branch:
    ```
    git push origin master
    ```
9. Check that you can see your source code in the Console by clicking Navigation Menu > Source Repositories.
10. Click sample-app.
11. In the Cloud Platform Console, click Navigation menu > Cloud Build > Triggers.
12. Click Create trigger.
13. Set the following trigger settings:

    - Name: sample-app-tags
    - Event: Push new tag
    - Select your newly created sample-app repository.
    - Tag: v1.*
    - Configuration: Cloud Build configuration file (yaml or json)
    - Cloud Build configuration file location: /cloudbuild.yaml
14. Click CREATE.
15. Create the bucket:
    ```
    export PROJECT=$(gcloud info --format='value(config.project)')
    ```
    ```
    gsutil mb -l us-central1 gs://$PROJECT-kubernetes-manifests
    ```
16. Enable versioning on the bucket so that you have a history of your manifests:
    ```
    gsutil versioning set on gs://$PROJECT-kubernetes-manifests
    ```
17. Set the correct project ID in your kubernetes deployment manifests:
    ```
    sed -i s/PROJECT/$PROJECT/g k8s/deployments/*
    ```
18. Commit the changes to the repository:
    ```
    git commit -a -m "Set project ID"
    ```
19 In Cloud Shell, still in the sample-app directory, create a Git tag:
    ```
    git tag v1.0.0
    ```
20. Push the tag:
    ```
    git push --tags
    ```
21. Go to the Cloud Console. Still in Cloud Build, click History in the left pane to check that the build has been triggered. If not, verify that the trigger was configured properly in the previous section.

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Configuring your deployment pipelines</b></summary>
  <br/>
  <p>
    
1. Download the 1.14.0 version of spin:

   ```
   curl -LO https://storage.googleapis.com/spinnaker-artifacts/spin/1.14.0/linux/amd64/spin
   ```
   
2. Make spin executable:
    ```
    chmod +x spin
    ```
3. Use spin to create an app called sample in Spinnaker. Set the owner email address for the app in Spinnaker:
    ```
    ./spin application save --application-name sample \
                        --owner-email "$(gcloud config get-value core/account)" \
                        --cloud-providers kubernetes \
                        --gate-endpoint http://localhost:8080/gate
    ```
4. From your sample-app source code directory, run the following command to upload an example pipeline to your Spinnaker instance:
    ```
    export PROJECT=$(gcloud info --format='value(config.project)')
    sed s/PROJECT/$PROJECT/g spinnaker/pipeline-deploy.json > pipeline.json
    ./spin pipeline save --gate-endpoint http://localhost:8080/gate -f pipeline.json
    ```
5. In the Spinnaker UI, click Applications at the top of the screen to see your list of managed applications. sample is your application. If you don't see sample, try refreshing the Spinnaker Applications tab.
6. Click sample to view your application deployment.
7. Click Pipelines at the top to view your applications pipeline status.
8. Click Start Manual Execution and select Deploy for the Pipeline and then click Run to trigger the pipeline this first time.
9. Click Execution Details to see more information about the pipeline's progress.
10. Click a stage to see details about it.
11. Hover over the yellow "person" icon and click Continue.
12. To view the app, select Infrastructure > Load Balancers in the top of the Spinnaker UI.
13. Scroll down the list of load balancers and click Default, under service sample-frontend-production. You will see details for your load balancer appear on the right side of the page. If you do not, you may need to refresh your browser.
14. Scroll down the details pane on the right and copy your app's IP address by clicking the clipboard button on the Ingress IP. The ingress IP link from the Spinnaker UI may use HTTPS by default, while the application is configured to use HTTP.
15. Paste the address into a new browser tab to view the application. You might see the canary version displayed, but if you refresh you will also see the production version.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 6: Triggering your pipeline from code changes</b></summary>
  <br/>
  <p>
    
1. From your sample-app directory, change the color of the app from orange to blue:

   ```
   sed -i 's/orange/blue/g' cmd/gke-info/common-service.go
   ```
   
2. Tag your change and push it to the source code repository:
    ```
    git commit -a -m "Change color to blue"
    ```
    ```
    git tag v1.0.1
    ```
    ```
    git push --tags
    ```
3. In the Console, in Cloud Build > History, wait a couple of minutes for the new build to appear. You may need to refresh your page. Wait for the new build to complete, before going to the next step.
4. Return to the Spinnaker UI and click Pipelines to watch the pipeline start to deploy the image. The automatically triggered pipeline will take a few minutes to appear. You may need to refresh your page.
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
