# Quest-2: Perform Foundational Data, ML, and AI Tasks in Google Cloud
## Lab-5: Cloud Natural Language API: Qwik Start

<details> 
  <summary><b>Task 1: Create an API Key</b></summary>
  <br/>
  <p>
    
1. First, you will set an environment variable with your PROJECT_ID which you will use throughout this codelab:
    ```
    export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
    ```
    
2. Next, create a new service account to access the Natural Language API:
    ```
    gcloud iam service-accounts create my-natlang-sa \
      --display-name "my natural language service account"
    ```
3. Then, create credentials to log in as your new service account. Create these credentials and save it as a JSON file "~/key.json" by using the following command:
    ```
    gcloud iam service-accounts keys create ~/key.json \
      --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
    ```
4. Finally, set the GOOGLE_APPLICATION_CREDENTIALS environment variable. The environment variable should be set to the full path of the credentials JSON file you created, which you can see in the output from the previous command:
    ```
    export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"
    ```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Make an Entity Analysis Request</b></summary>
  <br/>
  <p>
    
1. Click on the SSH button. You will be brought to an interactive shell. Remain in this SSH session for the rest of the lab.

2. Run the following gcloud command:
    ```
    gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'." > result.json
    ```
    
3. Run the below command to preview the output of result.json file.
    ```
    cat result.json
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
