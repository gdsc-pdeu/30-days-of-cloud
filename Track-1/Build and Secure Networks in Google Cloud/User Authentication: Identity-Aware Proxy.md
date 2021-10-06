# Quest-5: Getting Started: Create and Manage Cloud Resources
## Lab-1: User Authentication: Identity-Aware Proxy

<details> 
  <summary><b>Task 1: Download the Code</b></summary>
  <br/>
  <p>
    
1. Download the code from a public storage bucket and then change to the code folder:
    ```
    gsutil cp gs://spls/gsp499/user-authentication-with-iap.zip .
    ```
    ```
    unzip user-authentication-with-iap.zip
    ```
    ```
    cd user-authentication-with-iap
    ```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Deploy Application and Protect it with IAP</b></summary>
  <br/>
  <p>
    
1. Change from the main project folder to the 1-HelloWorld subfolder that contains code for this step.

   ```
   cd 1-HelloWorld
   ```
   
2. You can list each file in the shell using the cat command, as in:
    ```
    cat main.py
    ```
3. Deploy to App Engine
    a. Deploy the app to the App Engine Standard environment for Python.
    ```
    gcloud app deploy
    ```
    b. Enter that command:
    ```
    gcloud app browse
    ```
  
4. Restrict Access with IAP
    a. In the cloud console window, click the Navigation menu > Security > Identity-Aware Proxy.
    b. Click ENABLE API.
    c. Click GO TO IDENTITY-AWARE PROXY.
    d. Click CONFIGURE CONSENT SCREEN.
    e. Select Internal under User Type and click Create.
    f. Fill in the required blanks with appropriate values:
    
    | Field | Value |
    | :---: | :---: |
    | App name | IAP Example |
    | User support email | Select your Qwiklabs student email address from the dropdown. |
    | Application home page | The URL you used to view your app. You can find this again by running the gcloud app browse command in cloud shell again. |
    | Application privacy Policy link | The privacy page link in the app, same as the homepage link with /privacy added to the end |
    | Authorized domains| Click + ADD DOMAINThe hostname portion of the application's URL, e.g. iap-example-999999.appspot.com. You can see this in the address bar of the Hello World web page you previously opened. Do not include the starting https:// or trailing / from that URL. |
    | Developer Contact Information | Enter at least one email |
 
5. Click SAVE AND CONTINUE.

6. For Scopes, click SAVE AND CONTINUE.

7. For Summary, click BACK TO DASHBOARD.    
8. In Cloud Shell, run this command to disable the Flex API:
9. In Cloud Shell, run this command to disable the Flex API:
    ```
    gcloud services disable appengineflex.googleapis.com
    ```
10. Return to the Identity-Aware Proxy page and refresh it. You should now see a list of resources you can protect.
11. The domain will be protected by IAP. Click TURN ON.
    
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Test that IAP is turned on</b></summary>
  <br/>
  <p>
    
1. Open a browser tab and navigate to the URL for your app. A Sign in with Google screen opens and requires you to log in to access the app.
2. Sign in with the account you used to log into the console. You will see a screen denying you access.
3. Return to the Identity-Aware Proxy page of the console, select the checkbox next to App Engine app, and see the sidebar at the right of the page. 
4. Click ADD PRINCIPALS.
5. Enter your Qwiklabs Student email address.
6. Then, pick the Cloud IAP/IAP-Secured Web App User role to assign to that address.  
7. Click SAVE.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Access User Identity Information</b></summary>
  <br/>
  <p>
    
1. In Cloud Shell, change to the folder for this step:
    ```
    cd ~/user-authentication-with-iap/2-HelloUser
    ```
2. Deploy to App Engine
    ```
    gcloud app deploy
    ```
3. Turn off IAP
    - In the cloud console window, click Navigation menu > Security > Identity-Aware Proxy. Click the IAP toggle switch next to App Engine app to turn IAP off.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Use Cryptographic Verification</b></summary>
  <br/>
  <p>
    
1. In Cloud Shell, change to the folder for this step:
    ```
    cd ~/user-authentication-with-iap/3-HelloVerifiedUser
    ```
2. Deploy to App Engine
    ```
    gcloud app deploy
    ```
3. Test the Cryptographic Verification
    ```
    gcloud app browse
    ```
4. In the cloud console window, click the Navigation menu > Secrutiy > Identity-Aware Proxy.
5. Click the IAP toggle switch next to App Engine app to turn IAP on again
    
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
