# Quest-2: Perform Foundational Data, ML, and AI Tasks in Google Cloud
## Lab-2: Dataprep: Qwik Start

<details> 
  <summary><b>Task 1: Create a Cloud Storage bucket in your project</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, select Navigation menu > Cloud Storage > Browser.
2. Click Create bucket.
3. In the Create a bucket dialog, Name the bucket a unique name. Leave other settings at their default value.
4. Click Create.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Initialize Cloud Dataprep</b></summary>
  <br/>
  <p>
    
1. Select Navigation menu > Dataprep.
2. Check to accept the Google Dataprep Terms of Service, then click Accept.
3. Check to authorize sharing your account information with Trifacta, then click Agree and Continue.
4. Click Allow to allow Trifacta to access project data.
5. Click your student username to sign in to Cloud Dataprep by Trifacta. Your username is the Username in the left panel in your lab.
6. Click Allow to grant Cloud Dataprep access to your Google Cloud lab account.
7. Check to agree to Trifacta Terms of Service, and then click Accept.
8. Click Continue on the "First time set up" screen to create the default storage location.


    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create a flow</b></summary>
  <br/>
  <p>
    
1. Click Flows icon, then the Create button, then select Blank Flow :
2. Click on Untitled Flow, then name and describe the flow. Since this lab uses 2016 data from the United States Federal Elections Commission 2016, name the flow "FEC-2016", and then describe the flow as "United States Federal Elections Commission 2016".
3. Click OK.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Import datasets</b></summary>
  <br/>
  <p>
    
1. Click Add Datasets, then select the Import Datasets link.
2. In the left menu pane, select Cloud Storage to import datasets from Cloud Storage, then click on the pencil to edit the file path.
3. Type gs://spls/gsp105 in the Choose a file or folder text box, then click Go.
4. You may have to widen the browser window to see the Go and Cancel buttons.
5. Click us-fec/.
6. Click the + icon next to cn-2016.txt to create a dataset shown in the right pane. Click on the title in the dataset in the right pane and rename it "Candidate Master 2016".
7. In the same way add the itcont-2016.txt dataset, and rename it "Campaign Contributions 2016".
8. Both datasets are listed in the right pane; click Import & Add to Flow.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Prep the candidate file</b></summary>
  <br/>
  <p>
    
1. By default, the Candidate Master 2016 dataset is selected. In the right pane, click Edit Recipe.
2. Column5 provides data from 1990-2064. Widen column5 (like you would on a spreadsheet) to separate each year. Click to select the tallest bin, which represents year 2016.
3. In the Suggestions panel on the right, in the Keep rows section, click Add to add this step to your recipe.
4. In Column6 (State), hover over and click on the mismatched (red) portion of the header to select the mismatched rows.
5. To correct the mismatch, click X in the top of the Suggestions panel to cancel the transformation, then click on the flag icon in Column6 and change it to a "String" column.
6. Filter on just the presidential candidates, which are those records that have the value "P" in column7. In the histogram for column7, hover over the two bins to see which is "H" and which is "P". Click the "P" bin.
7. In the right Suggestions panel, click Add to accept the step to the recipe.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 6: Wrangle the Contributions file and join it in</b></summary>
  <br/>
  <p>
    
1. Click on FEC-2016 (the dataset selector) at the top of the grid view page.
2. Click to select the grayed out Campaign Contributions.
3. In the right pane, click Add > Recipe, then click Edit Recipe.
4. Click the recipe icon at the top right of the page, then click Add New Step.
5. Insert the following Wrangle language command in the Search box
    ```
    replacepatterns col: * with: '' on: `{start}"|"{end}` global: true
    ```
6. Click Add to add the transform to the recipe.
7. Add another new step to the recipe. Click New Step, then type "Join" in the Search box.
8. Click Join datasets to open the Joins page.
9. Click on "Candidate Master 2016" to join with Campaign Contributions 2016-2, then Accept in the bottom right.
10. On the right side, hover in the Join keys section, then click on the pencil (Edit icon).
11. In the Add Key panel, in the Suggested join keys section, click column2 = column11.
12. Click Save and Continue.
    - Columns 2 and 11 open for your review.
13. Click Next, then check the checkbox to the left of the "Column" label to add all columns of both datasets to the joined dataset.
14. Click Review, and then Add to Recipe to return to the grid view.
    
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
