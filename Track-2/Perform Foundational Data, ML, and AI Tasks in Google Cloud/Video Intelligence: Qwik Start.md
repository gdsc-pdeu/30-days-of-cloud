# Quest-2: Perform Foundational Data, ML, and AI Tasks in Google Cloud
## Lab-7: Video Intelligence: Qwik Start

<details> 
  <summary><b>Task 1: Set up authorization</b></summary>
  <br/>
  <p>
    
1. In Cloud Shell run the following command to create a new service account named quickstart
    ```
    gcloud iam service-accounts create quickstart
    ```
    
2. Create a service account key file, replacing <your-project-123> with your Qwiklabs Project ID:
    ```
    gcloud iam service-accounts keys create key.json --iam-account quickstart@<your-project-123>.iam.gserviceaccount.com
    ```
  
3. Now authenticate your service account, passing the location of your service account key file:
    ```
    gcloud auth activate-service-account --key-file key.json
    ```

4. Copy the key you just generated and click Close.
    
5. Obtain an authorization token using your service account:
    ```
    gcloud auth print-access-token
    ```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Make an annotate video request</b></summary>
  <br/>
  <p>
    
1. Run this command to create a JSON request file with the following text, and save it as request.json :
    ```
    cat > request.json <<EOF
    {
       "inputUri":"gs://spls/gsp154/video/train.mp4",
       "features": [
           "LABEL_DETECTION"
       ]
    }
    EOF
    ```

2. Use curl to make a videos:annotate request passing the filename of the entity request:
    ```
    curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @request.json
    ```
    
3. Use this script to request information on the operation by calling the v1.operations endpoint. Replace the PROJECTS, LOCATIONS and OPERATION_NAME with the value you just received in the previous command:
    ```
    curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/projects/PROJECTS/locations/LOCATIONS/operations/OPERATION_NAME'
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
