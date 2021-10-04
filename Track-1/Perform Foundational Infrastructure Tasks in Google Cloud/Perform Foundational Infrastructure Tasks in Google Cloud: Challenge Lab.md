# Quest-2: Perform Foundational Infrastructure Tasks in Google Cloud
## Lab-6: Perform Foundational Infrastructure Tasks in Google Cloud: Challenge Lab

<details> 
  <summary><b>Task 1: Create a bucket</b></summary>
  <br/>
  <p>
    
1. Navigation menu > STORAGE > Storage > Create Bucket
2. Name your bucket > Enter GCP Project ID > Continue
3. Choose where to store your data > Region: us-east1 > Continue
4. Use default for the remaining
5. Create
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create a Pub/Sub topic</b></summary>
  <br/>
  <p>

1. Navigation menu > BIG DATA > Pub/Sub
2. Create Topic > Name: Jooli > Create Topic
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create the thumbnail Cloud Function</b></summary>
  <br/>
  <p>
    
1. Navigation menu > COMPUTE > Cloud Functions > Create Function
2. Use the following config:
3. Name: CFJooli Region: us-east1 Trigger: Cloud Storage Event type: Finalize/Create Bucket: BROWSE > Select the qwiklabs bucket
4. Remaining default > Next
5. Runtime: Node.js 10 Entry point: thumbnail
6. Add the code appropiately
7. Download the image from URL
8. Navigation menu > STORAGE > Storage > Select your bucket > Upload files
9. Refresh bucket
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Remove the previous cloud engineer</b></summary>
  <br/>
  <p>
    
1. Navigation menu > IAM & Admin
2. Search for the "Username 2" > Edit > Delete Role
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
