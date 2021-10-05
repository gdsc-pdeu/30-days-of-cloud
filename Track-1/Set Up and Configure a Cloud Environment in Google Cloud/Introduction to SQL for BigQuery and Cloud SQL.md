# Quest-3: Set Up and Configure a Cloud Environment in Google Cloud
## Lab-2: Introduction to SQL for BigQuery and Cloud SQL

<details> 
  <summary><b>Task 1: Exploring the BigQuery Console</b></summary>
  <br/>
  <p>
    
1. Open BigQuery Console
    a. In the Google Cloud Console, select Navigation menu > BigQuery:
2. Uploading queryable data.
    a. Click on the + ADD DATA link then select Explore public datasets:
    b. In the search bar, enter "london" and press Enter, then select the London Bicycle Hires tile, then View Dataset.
3. Running SELECT, FROM, and WHERE in BigQuery
    a. Copy and paste the following command in to the query EDITOR:
    ```
    SELECT end_station_name FROM `bigquery-public-data.london_bicycles.cycle_hire`;
    ```
 
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: More SQL Keywords: GROUP BY, COUNT, AS, and ORDER BY</b></summary>
  <br/>
  <p>
    
1. To get a better picture of what this keyword does, clear the query from the editor, then copy and paste the following command:
    ```
    SELECT start_station_name FROM `bigquery-public-data.london_bicycles.cycle_hire` GROUP BY start_station_name;
    ```
2. Add the COUNT function to our previous query to figure out how many rides begin in each starting point. Clear the query from the editor, then copy and paste the following command and then click Run
    ```
    SELECT start_station_name, COUNT(*) FROM `bigquery-public-data.london_bicycles.cycle_hire` GROUP BY start_station_name;
    ```
3. Add an AS keyword to the last query we ran to see this in action. Clear the query from the editor, then copy and paste the following command:
    ```
    SELECT start_station_name, COUNT(*) AS num_starts FROM `bigquery-public-data.london_bicycles.cycle_hire` GROUP BY start_station_name;
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Working with Cloud SQL</b></summary>
  <br/>
  <p>
    
1. Back in the BigQuery Console, this should have been the last command that you ran:
    ```
    SELECT start_station_name, COUNT(*) AS num FROM `bigquery-public-data.london_bicycles.cycle_hire` GROUP BY start_station_name ORDER BY num DESC;
    ```
2. In the Query Results section click SAVE RESULTS > CSV(local file) > SAVE. This initiates a download, which saves this query as a CSV file. Note the location and the name of this downloaded file—you will need it soon.
3. Clear the query EDITOR, then copy and run the following in the query editor:
    ```
    SELECT end_station_name, COUNT(*) AS num FROM `bigquery-public-data.london_bicycles.cycle_hire` GROUP BY end_station_name ORDER BY num DESC;
    ```
4. In the Query Results section click SAVE RESULTS > CSV(local file) > SAVE. This initiates a download, which saves this query as a CSV file. Note the location and the name of this downloaded file—you will need it in the following section.
5. Upload CSV files to Cloud Storage
    a. Select Navigation menu > Cloud Storage > Browser, and then click CREATE BUCKET.
    b. Enter a unique name for your bucket, keep all other settings as default, and click Create:
    c. Click UPLOAD FILES and select the CSV that contains start_station_name data. Then click Open. Repeat this for the end_station_name data.
    d. Rename your start_station_name file by clicking on the three dots next to on the far side of the file and click rename. Rename the file to start_station_data.csv.
    e. Rename your end_station_name file by clicking on the three dots next to on the far side of the file and click rename. Rename the file to end_station_data.csv.
6. Create a Cloud SQL instance
    a. In the console, select Navigation menu > SQL. Click CREATE INSTANCE.
    b. From here, you will be prompted to choose a database engine. Select MySQL.
    c. Now enter in a name for your instance (like "qwiklabs-demo") and enter a secure password in the Password field (remember it!), then click CREATE INSTANCE:
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: New Queries in Cloud SQL</b></summary>
  <br/>
  <p>
    
1. Activate Cloud Shell
2. Run the following command in Cloud Shell to connect to your SQL instance, replacing qwiklabs-demo if you used a different name for your instance:
    ```
    gcloud sql connect  qwiklabs-demo --user=root
    ```
3. Run the following command at the MySQL server prompt to create a database called bike:
    ```
    CREATE DATABASE bike;
    ```
4. Make a table inside of the bike database by running the following command
    ```
    USE bike;
    CREATE TABLE london1 (start_station_name VARCHAR(255), num INT);
    ```
5. Create another table named "london2" by running the following command:
    ```
    USE bike;
    CREATE TABLE london2 (end_station_name VARCHAR(255), num INT);
    ```
6. Upload CSV files to tables
    a. eturn to the Cloud SQL console. You will now upload the start_station_name and end_station_name CSV files into your newly created london1 and london2 tables.
    b. In your Cloud SQL instance page, click IMPORT.
    c. In the Cloud Storage file field, click Browse, and then click the arrow opposite your bucket name, and then click start_station_data.csv. Click Select.
    d. Select CSV as File format.
    e. Select the bike database and type in "london1" as your table.
    f. Click Import:
7. Do the same for the other CSV file.
    a. In your Cloud SQL instance page, click IMPORT.
    b. In the Cloud Storage file field, click Browse, and then click the arrow opposite your bucket name, and then click end_station_data.csv Click Select.
    c. Select CSV as File format.
    d. Select the bike database and type in "london2" as your table.
    e. Click Import:
8. Return to your Cloud Shell session and run the following command at the MySQL server prompt to inspect the contents of london1:
    ```
    SELECT * FROM london1;
    ```
    ```
    SELECT * FROM london2;
    ```
9. DELETE keyword
    ```
    DELETE FROM london1 WHERE num=0;
    DELETE FROM london2 WHERE num=0;
    ```
10. INSERT INTO keyword
    ```
    INSERT INTO london1 (start_station_name, num) VALUES ("test destination", 1);
    ```
11. UNION keyword
    ```
    SELECT start_station_name AS top_stations, num FROM london1 WHERE num>100000
    UNION
    SELECT end_station_name, num FROM london2 WHERE num>100000
    ORDER BY top_stations DESC;
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. A keyword that specifies the fields (e.g. column values) that you want to pull from your dataset.
  - [ ] FROM
  - [ ] WHERE
  - [X] SELECT
- Q. Specifies what table or tables we want to pull our data from
  - [X] FROM
  - [ ] WHERE
  - [ ] SELECT
- Q. Allows us to filter tables for specific column values.
  - [ ] FROM
  - [ ] WHERE
  - [X] SELECT
- Q. A fully-managed petabyte-scale data warehouse that runs on the Google Cloud.
  - [X] BigQuery
  - [ ] Google Cloud
  - [ ] Cloud SQL
  - [ ] Cloud Storage bucket
- Q. Projects contain datasets, and datasets contain tables.
  - [X] True
  - [ ] False
- Q. With BigQuery, you can access datasets shared publicly from other Google Cloud projects.
  - [X] True
  - [ ] False   
- Q. Aggregates rows that share common criteria (e.g. a column value) and will return all of the unique entries found for such criteria.
  - [ ] AS
  - [X] GROUP BY
  - [ ] COUNT
  - [ ] ORDER BY 
- Q. A SQL function will count and return the number of rows that share common criteria.
  - [ ] AS
  - [ ] GROUP BY
  - [X] COUNT
  - [ ] ORDER BY  
- Q. Creates an alias of a table or column.
  - [X] AS
  - [ ] GROUP BY
  - [ ] COUNT
  - [ ] ORDER BY 
- Q. Sorts the returned data from a query in ascending or descending order based on a specified criteria or column value.
  - [ ] AS
  - [ ] GROUP BY
  - [ ] COUNT
  - [X] ORDER BY 
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
