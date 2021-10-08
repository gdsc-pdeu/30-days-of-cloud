# Quest-6: Deploy to Kubernetes in Google Cloud
## Lab-6: Deploy to Kubernetes in Google Cloud: Challenge Lab

<details> 
  <summary><b>Task 1: Create a Docker image and store the Dockerfile</b></summary>
  <br/>
  <p>
    
```
gsutil cat gs://cloud-training/gsp318/marking/setup_marking.sh | bash
```
```
gcloud source repos clone valkyrie-app
```
```
cd valkyrie-app
```
```
cat > Dockerfile <<EOF
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
EOF
```
```
docker build -t valkyrie-app:v0.0.1 .
```
```
cd ../marking
```
```
./step1.sh
```

  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Test the created Docker image</b></summary>
  <br/>
  <p>
    
```
cd ../valkyrie-app
```
```
docker run -p 8080:8080 valkyrie-app:v0.0.1 &
```
```
cd ../marking
```
```
./ste
```
 
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Push the Docker image in the Container Repository</b></summary>
  <br/>
  <p>
    
```
cd ..
```
```
cd valkyrie-app
```
```
docker tag valkyrie-app:v0.0.1 gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1
```
```
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1
```
```
sed -i s#IMAGE_HERE#gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1#g k8s/deployment.yaml
```





    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Create and expose a deployment in Kubernetes</b></summary>
  <br/>
  <p>
    
```
sed -i s#IMAGE_HERE#gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.1#g k8s/deployment.yaml
```
```
gcloud container clusters get-credentials valkyrie-dev --zone us-east1-d
```
```
kubectl create -f k8s/deployment.yaml
```
```
kubectl create -f k8s/service.yaml
```
```
git merge origin/kurt-dev
```
```
kubectl edit deployment valkyrie-dev
```
 
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Update the deployment with a new version of valkyrie-app</b></summary>
  <br/>
  <p>
    
```
docker build -t gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.2 .
```
```
docker push gcr.io/$GOOGLE_CLOUD_PROJECT/valkyrie-app:v0.0.2
```
```
kubectl edit deployment valkyrie-dev
```
```
docker ps
```
```
docker kill container_id
```
  
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 6: Create a pipeline in Jenkins to deploy your app</b></summary>
  <br/>
  <p>
    
```
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
```
```
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

1. Open web-preview and login as admin with password from last command
2. Go to click credentials -> Jenkins -> Global Credentials
3. Go to Click add credentials
4. Select Google Service Account from metadata
5. Click ok

6. Click new item
7. Name valkyrie-app to Multibranch Pipeline option and click OK.
8. Now go to the next page, in the Branch Sources section, click Add Source and select git.
9. Paste the HTTPS clone URL of your sample-app repo in Cloud Source Repositories https://source.developers.google.com/p/YOUR_PROJECT/r/valkyrie-app into the Project Repository field.
10. Set credentials to qwiklabs and Click on ok
11. We can change color by following Command

```
sed -i "s/green/orange/g" source/html.go
```
```
sed -i "s/YOUR_PROJECT/$GOOGLE_CLOUD_PROJECT/g" Jenkinsfile
```
```
git config --global user.email "you@example.com"
```
```    
git config --global user.name "student"
```
```         
git add .
```
```
git commit -m "built pipeline init"
```
```
git push 
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
