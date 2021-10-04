# Quest-2: Perform Foundational Infrastructure Tasks in Google Cloud
## Lab-2: Cloud IAM: Qwik Start

<details> 
  <summary><b>Task 1: The IAM console and project level roles</b></summary>
  <br/>
  <p>
    
1. Return to the Username 1 Cloud Console page.
2. Select Navigation menu > IAM & Admin > IAM. You are now in the "IAM & Admin" console.
3. Click +ADD button at the top of the page and explore the project roles associated with Projects by clicking on the "Select a role" dropdown menu:
4. Click CANCEL to exit out of the "Add principal" panel.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Explore editor roles</b></summary>
  <br/>
  <p>
    
1. Now switch to the **Username 2** console.   
2. Navigate to the IAM & Admin console, select Navigation menu > IAM & Admin > IAM.
3. Search through the table to find Username 1 and Username 2 and examine the roles they are granted.
    - Username 2 has the "Viewer" role granted to it.
    - The +ADD button at the top is grayed out—if you try to click on it you get the required permission message
4. Switch back to the Username 1 console for the next step.
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Prepare a resource for access testing</b></summary>
  <br/>
  <p>
    
1. Create a Cloud Storage bucket with a unique name. From the Cloud Console, select Navigation menu > Cloud Storage > Browser.
2. Click CREATE BUCKET.
3. Update the following fields, leave all others at their default values:
    
    | Property | Value |
    | :---:   | :-: |
    | Name: | globally unique name (create it yourself!) and click CONTINUE. |
    | Location Type: | Multi-Region |
4. Click CREATE.
5. Upload a sample file
    a. On the Bucket Details page click UPLOAD FILES button.
    b. Browse your computer to find a file to use. Any text or html file will do.
    c. Click on the three dots at the end of the line containing the file and click Rename.
    d. Rename the file ‘sample.txt'.
    e. Click RENAME.
6. Verify project viewer access
    a. Switch to the Username 2 console.
    b. From the Console, select Navigation menu > Cloud Storage > Browser. Verify that this user can see the bucket.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Remove project access</b></summary>
  <br/>
  <p>
    
1. Switch to the **Username 1** console
2. Remove Project Viewer for Username 2
    a. Select Navigation menu > IAM & Admin > IAM. Then click the pencil icon next to Username 2.
    b. Remove Project Viewer access for Username 2 by clicking the trashcan icon next to the role name. Then click SAVE.
3. Verify that Username 2 has lost access
    a. Switch to Username 2 Cloud Console. Ensure that you are still signed in with Username 2's credentials and that you haven't been signed out of the project after permissions were revoked. If signed out, sign in back with the proper credentials.
    b. Navigate back to Cloud Storage by selecting Navigation menu > Cloud Storage > Browser.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Add Storage permissions</b></summary>
  <br/>
  <p>
    
1. Switch to Username 1 console. Ensure that you are still signed in with Username 1's credentials. If you are signed out, sign in back with the proper credentials.
2. In the Console, select Navigation menu > IAM & Admin > IAM.
3. Click + ADD button and paste the Username 2 name into the New principals field.
4. In the Select a role field, select Cloud Storage > Storage Object Viewer from the drop-down menu.
5. Click SAVE.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Verify access</b></summary>
  <br/>
  <p>
    
1. Switch to the Username 2 console. You'll still be on the Storage page.
    a. Username 2 doesn't have the Project Viewer role, so that user can't see the project or any of its resources in the Console. However, this user has specific access to Cloud Storage, the Storage Object Viewer role - check it out now.
2. Click the Activate Cloud Shell icon to open the Cloud Shell command line. If prompted click Continue.
3. Open up a Cloud Shell session and then enter in the following command, replace [YOUR_BUCKET_NAME] with the name of the bucket you created earlier:
    ```
    gsutil ls gs://[YOUR_BUCKET_NAME]
    ```
4. As you can see, you gave Username 2 view access to the Cloud Storage bucket.
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
