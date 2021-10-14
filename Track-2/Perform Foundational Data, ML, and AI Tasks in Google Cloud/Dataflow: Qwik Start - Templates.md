# Quest-2: Perform Foundational Data, ML, and AI Tasks in Google Cloud
## Lab-3: Dataflow: Qwik Start - Templates

<details> 
  <summary><b>Task 1: Ensure that the Dataflow API is successfully enabled</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, enter "Dataflow API" in the top search bar. Click on the result for Dataflow API.
2. Click Manage.
3. Click Disable API.
4.If asked to confirm, click Disable.
5. Click Enable.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create a Cloud BigQuery Dataset and Table Using Cloud Shell</b></summary>
  <br/>
  <p>
    
1. Run the following command to create a dataset called taxirides
    ```
    bq mk taxirides
    ```
2. Now that you have your dataset created, you'll use it in the following step to instantiate a BigQuery table. Run the following command to do so:
    ```
    bq mk \
      --time_partitioning_field timestamp \
      --schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
      timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
      passenger_count:integer -t taxirides.realtime
    ```
3. Create a storage bucket
    - Now that we have our table instantiated, let's create a bucket. Run the following commands to do so:
    ```
    export BUCKET_NAME=<your-unique-name>
    ```
    ```
    gsutil mb gs://$BUCKET_NAME/
    ```

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create a Cloud BigQuery Dataset and Table Using the Cloud Console</b></summary>
  <br/>
  <p>
    
1. Click on the three dots next to your project name under Explorer section, then click Create dataset. Input taxirides as your dataset ID:
2. Click on the three dots next to taxirides dataset and select Open. Then select CREATE TABLE in the right-hand side of the console.
3. In the Destination > Table Name input, enter realtime.
4. Under Schema, toggle the Edit as text slider and enter the following:
    ```
    ride_id:string,point_idx:integer,latitude:float,longitude:float,timestamp:timestamp,
    meter_reading:float,meter_increment:float,ride_status:string,passenger_count:integer
    ```
5. Create a storage bucket
    - Go back to the Cloud Console and navigate to Cloud Storage > Browser > Create bucket:
    - Give your bucket a unique name. Leave all other default settings, then click Create.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Run the Pipeline</b></summary>
  <br/>
  <p>
    
1. From the Navigation menu find the Big Data section and click on Dataflow.
2. Click on + Create job from template at the top of the screen.
3. Enter iotflow as Job name for your Cloud Dataflow job.
4. Under Dataflow Template, select the Pub/Sub Topic to BigQuery template.
5. Under Input Pub/Sub topic, enter:
    ```
    projects/pubsub-public-data/topics/taxirides-realtime
    ```
6. Under BigQuery output table, enter the name of the table that was created:
    ```
    myprojectid:taxirides.realtime
    ```
7. Add your bucket as Temporary Location:
    ```
    gs://Your_Bucket_Name/temp
    ```
8. Click the Run job button.
    
  </p>
</details>
<br/>



<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Google Cloud Dataflow supports batch processing.
  - [X] True 
  - [ ] False
    
- Q. Which Dataflow Template used in the lab to run the pipeline?
  - [ ] Pub/Sub to BigQuery
  - [ ] Bulk Compress Cloud Storage Files
  - [X] Cloud Storage Text to BigQuery
    
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
