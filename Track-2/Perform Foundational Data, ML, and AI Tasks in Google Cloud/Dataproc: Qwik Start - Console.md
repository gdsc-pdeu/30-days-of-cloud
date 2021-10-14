# Quest-2: Perform Foundational Data, ML, and AI Tasks in Google Cloud
## Lab-4: Dataproc: Qwik Start - Console

<details> 
  <summary><b>Task 1: Create a cluster</b></summary>
  <br/>
  <p>
    
1. In the Cloud Platform Console, select Navigation menu > Dataproc > Clusters, then click Create cluster.
    
    | Field | Value |
    | :---: | :---: |
    | Name | example-cluster |
    | Region | us-central1 |
    | Zone | us-central1-a |
    
2. Click Create to create the cluster.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Submit a job</b></summary>
  <br/>
  <p>
    
1. Click Jobs in the left pane to switch to Dataproc's jobs view, then click Submit job:

2. Set the following fields to update Job. Accept the default values for all other fields.
    
    | Field | Value |
    | :---: | :---: |
    | Cluster | example-cluster |
    | Job type | Spark |
    | Main class or jar	| org.apache.spark.examples.SparkPi |
    | Jar files |	file:///usr/lib/spark/examples/jars/spark-examples.jar |
    | Arguments | 1000 (This sets the number of tasks.) |
    
3. Click Submit.

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: View the job output</b></summary>
  <br/>
  <p>
    
1. Update a cluster
2. Click on the three dots next to taxirides dataset and select Open. Then select CREATE TABLE in the right-hand side of the console.
    1. Select Clusters in the left navigation pane to return to the Dataproc Clusters view.
    2. Click example-cluster in the Clusters list. By default, the page displays an overview of your cluster's CPU usage.
    3. Click Configuration to display your cluster's current settings.
    4. Click Edit. The number of worker nodes is now editable.
    5. Enter 4 in the Worker nodes field.
    6. Click Save.
    7. To rerun the job with the updated cluster, you would click Jobs in the left pane, then click SUBMIT JOB.
    8. Set the same fields you set in the Submit a job section:
    
    | Field | Value |
    | :---: | :---: |
    | Cluster	| example-cluster |
    | Job type | Spark |
    | Main class or jar	| org.apache.spark.examples.SparkPi |
    | Jar files	| file:///usr/lib/spark/examples/jars/spark-examples.jar |
    | Arguments | 1000 (This sets the number of tasks.) |
    
3. Click Submit.
    
  </p>
</details>
<br/>


<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Dataproc helps users process, transform and understand vast quantities of data.
  - [X] True 
  - [ ] False
    
- Q. Which type of Dataproc job is submitted in the lab?
  - [ ] Hadoop
  - [ ] SparkSql
  - [ ] PySpark
  - [X] Spark
  - [ ] Pig
    
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
