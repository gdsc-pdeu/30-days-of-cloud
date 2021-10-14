# Quest-2: Perform Foundational Data, ML, and AI Tasks in Google Cloud
## Lab-6: Google Cloud Speech API: Qwik Start

<details> 
  <summary><b>Task 1: Create an API Key</b></summary>
  <br/>
  <p>
    
1. To create an API key, click Navigation menu > APIs & services > Credentials:
    
2. Then click Create credentials:
  
3. In the drop down menu, select API key:

4. Copy the key you just generated and click Close.
    
5. In order to perform next steps please connect to the instance provisioned for you via ssh. Open the Navigation menu and select Compute Engine. You should see the following provisioned linux instanc
    
6. Click on the SSH button. You will be brought to an interactive shell. In the command line, enter in the following, replacing <YOUR_API_KEY> with the key you just copied:
    ```
    export API_KEY=<YOUR_API_KEY>
    ```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create your Speech API request</b></summary>
  <br/>
  <p>
    
1. Create request.json in SSH command line. You'll use this to build your request to the speech API:
    ```
    touch request.json
    ```

2. Now open the request.json using your preferred command line editor (nano, vim, emacs) or gcloud. Add the following to your request.json file, using the uri value of the sample raw audio file
    ```
    {
      "config": {
          "encoding":"FLAC",
          "languageCode": "en-US"
      },
      "audio": {
          "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
      }
    }
    ```
    
    
  </p>
</details>
<br/>
    
    
<details> 
  <summary><b>Task 3: Call the Speech API</b></summary>
  <br/>
  <p>
    
1. Pass your request body, along with the API key environment variable, to the Speech API with the following curl command (all in one single command line):
    ```
    curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
    "https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
    ```

2. You created an Speech API request then called the Speech API. Run the following command to save the response in a result.json file:
    ```
    curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
    "https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
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
