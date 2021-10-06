# Quest-4: Deploy and Manage Cloud Environments with Google Cloud
## Lab-5: Deploy and Manage Cloud Environments with Google Cloud: Challenge Lab

<details> 
  <summary><b>Task 1: Create the production environment</b></summary>
  <br/>
  <p>

1. Go to Compute Engine > VM Instances > kraken-jumphost > SSH
2. Run the following there:
```	
cd /work/dm
```
```
sed -i s/SET_REGION/us-east1/g prod-network.yaml
```
```
gcloud deployment-manager deployments create prod-network --config=prod-network.yaml
```
```
gcloud config set compute/zone us-east1-b
```
```
gcloud container clusters create kraken-prod \
          --num-nodes 2 \
          --network kraken-prod-vpc \
          --subnetwork kraken-prod-subnet
```
```          
gcloud container clusters get-credentials kraken-prod
```
```
cd /work/k8s
```
```       
for F in $(ls *.yaml); do kubectl create -f $F; done
```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Setup the Admin instance</b></summary>
  <br/>
  <p>
    
```
gcloud config set compute/zone us-east1-b
```
```
gcloud compute instances create kraken-admin --network-interface="subnet=kraken-mgmt-subnet" --network-interface="subnet=kraken-prod-subnet"
```
```
Go to Monitoring > Alerting > Create Alerting Policy
```
```
Add Condition (use the kraken-admin instance ID)
```
```
Manage notification Channel > Create a new Email Channel
```
```
Click Done
```
  
  
  
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Verify the Spinnaker deployment</b></summary>
  <br/>
  <p>
    
```
gcloud config set compute/zone us-east1-b
```
```
gcloud container clusters get-credentials spinnaker-tutorial
```
```
DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" -o jsonpath="{.items[0].metadata.name}")
```
```
kubectl port-forward --namespace default $DECK_POD 8080:9000 >> /dev/null &
```
```
Web Preview > Preview on port 8080
```
```
Go to Applications > sample
```
```
Select Pipelines
```
```
Start Manual Execution > Pipeline: Deploy > Run
```
```
Click and continue the PUBSUB
```
```
While the MANUAL START is running, execute the following:
```
```
gcloud config set compute/zone us-east1-b
```
```
gcloud source repos clone sample-app
```
```
cd sample-app
```
```
touch a
```
```
git config --global user.email "$(gcloud config get-value account)"
```
```
git config --global user.name "Student"
```
```
git add .
```
```
git commit -m "change"
```
```
git tag v1.0.1
```
```
git push --tags
```
```
Now, return to the Spinnaker page and allow the "Continue to Deploy?" message.
```
```
Again, start another MANUAL START deployment (this time it will use the new changes)
```
```
Wait the deployment to finish.
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
