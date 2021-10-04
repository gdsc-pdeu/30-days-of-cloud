# Quest-2: Perform Foundational Infrastructure Tasks in Google Cloud
## Lab-1: Cloud Storage: Qwik Start - Cloud Console

<details> 
  <summary><b>Task 1: Create a Bucket</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, go to Navigation menu > Cloud Storage > Browser.
2. Click Create Bucket:
3. Enter your bucket information and click Continue to complete each step:
    a. Name your bucket: Enter a unique name for your bucket. For this lab, you can use your Project ID as the bucket name because it will always be unique.
    b. Choose Region for Location type and us-east1 (South Carolina) for Location.
    c. Choose Standard for default storage class.
    d. Choose Uniform for Access control.
4. Click Create.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Upload an object into the Bucket</b></summary>
  <br/>
  <p>
    
1. [kitten.png](https://cdn.qwiklabs.com/8tnHNHkj30vDqnzokQ%2FcKrxmOLoxgfaswd9nuZkEjd8%3D) Save the image as kitten.png, renaming it on download.
2. In the Cloud Storage browser page, click the name of the bucket that you created.
3. In the Objects tab, click Upload files.
4. In the file dialog, go to the file that you downloaded and select it.
5. Ensure the file is named kitten.png. If it is not, click the three dot icon for your file, select Rename from the dropdown, and rename the file to kitten.png.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Share an Object Publicly</b></summary>
  <br/>
  <p>
    
1. Click the Permissions tab above the list of files.
2. In the Public Access box, click Remove Public Access Prevention.
3. Click Confirm to acknowledge that objects within this bucket can be given public access.
4. Ensure the view is set to Members. Click Add to view the Add members pane.
5. In the New members box, enter allUsers.
6. In the Select a role drop-down, select Cloud Storage > Storage Object Viewer.
7. Click Save.
8. In the Are you sure you want to make this resource public? window, click Allow public access.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Create Folders</b></summary>
  <br/>
  <p>
    
1. In the Objects tab, click Create folder.
2. Enter folder1 for Name and click Create.
    - You should see the folder in the bucket with an image of a folder icon to distinguish it from objects.
    - Create a subfolder and upload a file to it:
3. Click folder1.
4. Click Create folder.
5. Enter folder2 for Name and click Create.
6. Click folder2.
7. Click Upload files.
8. In the file dialog, navigate to the screenshot that you downloaded and select it.
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Delete a folder</b></summary>
  <br/>
  <p>
    
1. Click the arrow next to Bucket details to return to the buckets level.
2. Select the bucket.
3. Select the checkbox next to folder1.
4. Click on the Delete button.
5. In the window that opens, type DELETE to confirm the deletion of the folder.
6. Click Delete to permanently delete the folder and all objects and subfolders in it.
    
  </p>
</details>
<br/>



<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Every bucket must have a unique name across the entire Cloud Storage namespace.
  - [X] True
  - [ ] False
 
- Q. Cloud Storage offers which storage classes:
  - [X] Nearline
  - [X] Archive
  - [X] Coldline
  - [X] Standard
    
- Q. Object names must be unique only within a given bucket
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
