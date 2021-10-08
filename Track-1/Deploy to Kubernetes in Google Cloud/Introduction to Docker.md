# Quest-6: Deploy to Kubernetes in Google Cloud
## Lab-1: Introduction to Docker

<details> 
  <summary><b>Task 1: Hello World</b></summary>
  <br/>
  <p>
    
1. Open up Cloud Shell and enter the following command to run a hello world container to get started:
    ```
    docker run hello-world
    ```
2. Run the following command to take a look at the container image it pulled from Docker Hub:
    ```
    docker images
    ```
3. Let's run the container again:
    ```
    docker run hello-world
    ```
4. Finally, look at the running containers by running the following command:
    ```
    docker ps
    ```
5. The hello-world containers you ran previously already exited. In order to see all containers, including ones that have finished executing, run
    ```
    docker ps -a
    ```
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Build</b></summary>
  <br/>
  <p>
    
1. Let's build a Docker image that's based on a simple node application. Execute the following command to create and switch into a folder named test.

   ```
   mkdir test && cd test
   ```
   
2. Create a Dockerfile
    ```
    cat > Dockerfile <<EOF
    # Use an official Node runtime as the parent image
    FROM node:6
    # Set the working directory in the container to /app
    WORKDIR /app
    # Copy the current directory contents into the container at /app
    ADD . /app
    # Make the container's port 80 available to the outside world
    EXPOSE 80
    # Run app.js using node when the container launches
    CMD ["node", "app.js"]
    EOF
    ```
3. Run the following to create the node application:

    ```
    cat > app.js <<EOF
    const http = require('http');
    const hostname = '0.0.0.0';
    const port = 80;
    const server = http.createServer((req, res) => {
        res.statusCode = 200;
          res.setHeader('Content-Type', 'text/plain');
            res.end('Hello World\n');
    });
    server.listen(port, hostname, () => {
        console.log('Server running at http://%s:%s/', hostname, port);
    });
    process.on('SIGINT', function() {
        console.log('Caught interrupt signal and will exit');
        process.exit();
    });
    EOF
    ```
  
4. Note again the ".", which means current directory so you need to run this command from within the directory that has the Dockerfile:

    ```
    docker build -t node-app:0.1 .
    ```
  
5. Now, run the following command to look at the images you built:
    ```
    docker images
    ```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Run</b></summary>
  <br/>
  <p>
    
1. In this module, use this code to run containers based on the image you built:

   ```
   docker run -p 4000:80 --name my-app node-app:0.1
   ```
   
2. Open another terminal (in Cloud Shell, click the + icon), and test the server:
    ```
    curl http://localhost:4000
    ```
3. Close the initial terminal and then run the following command to stop and remove the container:
    ```
    docker stop my-app && docker rm my-app
    ```
  
4. Now run the following command to start the container in the background:
    ```
    docker run -p 4000:80 --name my-app -d node-app:0.1
    docker ps
    ```
  
5. Notice the container is running in the output of docker ps. You can look at the logs by executing docker logs [container_id].
    ```
    docker logs [container_id]
    ```

6. Let's modify the application. In your Cloud Shell, open the test directory you created earlier in the lab:
    ```
    cd test
    ```
    
7. Edit app.js with a text editor of your choice (for example nano or vim) and replace "Hello World" with another string:
    ```
    ....
    const server = http.createServer((req, res) => {
        res.statusCode = 200;
          res.setHeader('Content-Type', 'text/plain');
            res.end('Welcome to Cloud\n');
    });
    ....
    ```
    
8. Build this new image and tag it with 0.2:
    ```
    docker build -t node-app:0.2 .
    ```
   
9. Run another container with the new image version. Notice how we map the host's port 8080 instead of 80. We can't use host port 4000 because it's already in use.
    ```
    docker run -p 8080:80 --name my-app-2 -d node-app:0.2
    docker ps
    ```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Debug</b></summary>
  <br/>
  <p>
    
1. You can look at the logs of a container using docker logs [container_id]. If you want to follow the log's output as the container is running, use the -f option.

   ```
   docker logs -f [container_id]
   ```
   
2. Open another terminal (in Cloud Shell, click the + icon) and enter the following command:
    ```
    docker exec -it [container_id] bash
    ```
3. Look at the directory
    ```
    docker stop my-app && docker rm my-app
    ```
  
4. Now run the following command to start the container in the background:
    ```
    ls
    ```
  
5. Exit the Bash session:
    ```
    exit
    ```

6. You can examine a container's metadata in Docker by using Docker inspect:
    ```
    docker inspect [container_id]
    ```
    
7. Use --format to inspect specific fields from the returned JSON. For example:
    ```
    docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_id]
    ```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Publish</b></summary>
  <br/>
  <p>
    
1. You can find your project ID by running:

   ```
   gcloud config list project
   ```
   
2. Tag node-app:0.2. Replace [project-id] with your configuration..
    ```
    docker tag node-app:0.2 gcr.io/[project-id]/node-app:0.2
    ```
    ```
    docker images
    ```
3. Stop and remove all containers:
    ```
    docker stop $(docker ps -q)
    docker rm $(docker ps -aq)
    ```
  
4. You have to remove the child images (of node:6) before you remove the node image. Replace [project-id].
    ```
    docker rmi node-app:0.2 gcr.io/[project-id]/node-app node-app:0.1
    docker rmi node:6
    docker rmi $(docker images -aq) # remove remaining images
    docker images
    ```
  
5. At this point you should have a pseudo-fresh environment. Pull the image and run it. Remember to replace the [project-id].
    ```
    docker pull gcr.io/[project-id]/node-app:0.2
    docker run -p 4000:80 -d gcr.io/[project-id]/node-app:0.2
    curl http://localhost:4000
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
