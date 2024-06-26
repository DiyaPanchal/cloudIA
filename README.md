# Cloud Computing

# 21BCP421 G-12 , Div-6 , Sem-6

## Steps

Hey everyone, In this blog post we will be deploying a full stack web application using Docker. The application will be split into three microservices.

1. Frontend Container of ReactJs
2. Backend Container of NodeJs
3. Database Container of MongoDB

Let's first understand Docker's definition and the reasons for its necessity before beginning the deployment. Applications may be developed, tested, and deployed in containers with the help of the Docker platform. Code, runtime, system libraries, system tools, and settings are all included in containers, which are portable, standalone, and executable software packages. Because containers are segregated from the underlying host and from one another, the same program can execute in several contexts without requiring any modifications. This facilitates the consistent and repeatable deployment of apps.

In addition, Docker offers container management tools like Docker Compose, which lets you create and execute multi-container applications. In this blog, we will be using Docker Compose to define and run our full stack web application. Let’s start

1. Create a full stack application or use an existing one built on ReactJs, NodeJs and MongoDB.
   
I have used an application of my own which is Netflix-Clone or use any existing application you have. Just make sure it has the same tech-stack.

This is the file structure of our application:

  ![image info](./ss/1.png)

2. Create a repository on Docker Hub to store your Docker images.
Go to Docker Hub and create a new repository to store your Docker images. You can create a public or private repository depending on your requirements. Once you have created the repository, note down the repository name as we will need it later to push our Docker images to the repository.

This is what your repository should look like

  ![image info](./ss/2.png)

3. Create a Dockerfile for each of the services.
Dockerfile is a text file that contains a set of instructions that are used to build a Docker image. The Docker image is a lightweight, standalone, and executable package of software that includes everything needed to run an application. The Dockerfile contains instructions to build the image, such as the base image, working directory, dependencies, and commands to run the application.


   3.1 Dockerfile for ReactJs server at path client/Dockerfile
   
   Before creating the Dockerfile for the ReactJs server, make sure you have a build script in your package.json file. 
      1. Now, change your current working directory to client
      2. create a Dockerfile in the client folder
   
   This is what your final Dockerfile should look like:
       ![image info](./ss/3.png)
   
   Now we will build the Docker image from the created Dockerfile and push it to the Docker Hub repository, follow the below steps carefully
   
   Build the Docker image using the following command:
   
      ![image info](./ss/4.png)
      ![image info](./ss/5.png)
          
   Replace <DockerHubUsername> with your Docker Hub username and <RepositoryName> with the repository name of the created repository in step 2.
   
   This command will build the Docker image for the ReactJs server using the Dockerfile in the client folder.
   
   Lets check if the image is created successfully by running the following command:                                                                     
         ![image info](./ss/6.png)
         ![image info](./ss/7.png)
         
   This command will list all the Docker images on your system. You should see the Docker image for the ReactJs server in the list.
   
      1. Now we will see if the image is running successfully by running the following command:
         
          ![image info](./ss/8.png)
   
          ![image info](./ss/9.png)
   
          ![image info](./ss/10.png)
         
   This command will run the Docker image for the ReactJs server on port 3000. You should be able to access the ReactJs server at http://localhost:3000.
   
      2. Push the Docker image to the Docker Hub repository using the following command:
         
         ![image info](./ss/11.png)
         
         ![image info](./ss/12.png)
         
   This command will push the Docker image to the Docker Hub repository. You can check the Docker Hub repository to see if the image has been pushed successfully.
                                                                                                                                                                                      ![image info](./ss/13.png)
   
   
   3.2 Creating Dockerfile for NodeJs server
   
      1. Now, change your current working directory to client
         
      2. Create a Dockerfile in the client folder
         
   This is what your final Dockerfile should look like:
           ![image info](./ss/14.png)
           
   Now we will build the Docker image from the created Dockerfile for the server and push it to the Docker Hub repository, follow the below steps carefully
   
   - Build the Docker image using the following command:
     
      ![image info](./ss/15.png)
      ![image info](./ss/16.png)
   
   Replace <DockerHubUsername> with your Docker Hub username and <RepositoryName> with the repository name of the created repository in step 2.
   
   This command will build the Docker image for the Nodejs server using the Dockerfile in your local repository.
   
   Note: We will not run the server image as it requires a connection to the MongoDB database. We will run the server image after creating the MongoDB image.
   
    - Push the Docker image to the Docker Hub repository using the following command:
      
      ![image info](./ss/17.png)
      ![image info](./ss/18.png)
      
   This command will push the Docker image to the Docker Hub repository. You can check the Docker Hub repository to see if the image has been pushed successfully.
   
      ![image info](./ss/19.png)
   
   Now we have our client and server images pushed to the Docker Hub repository and ready to run. We now will pull an already created MongoDB image from the Docker Hub and 
   run it.
   
   3.3 Pulling MongoDB image from Docker Hub
   
   Pull the MongoDB image from the Docker Hub using the following command:
   
      ![image info](./ss/20.png)
      
      ![image info](./ss/21.png)

4. Run our application with the created docker imagesPermalink
Now that we have all our images ready, we can run our application by running all the docker containers and exposing the ports on which they will run so all three microservices can communicate with each other.

Follow the below commands to run your containers:
     ![image info](./ss/22.png)
     ![image info](./ss/32.png)

Now that we have created the containers, we need to create a network that we will connect to the server and the mongo container. We will do this so that we can get the IP of the mongo container and use it in the connection string as mongoose.connect('mongodb:<CONTAINER_IP>:27017:<DB_NAME>')

This is how your create a docker network:
     ![image info](./ss/23.png)

Now you attach this network to the mongodb container: docker network connect <NETWORK_NAME> <CONTAINER_ID>
      ![image info](./ss/24.png)

You can get the container IP using docker inspect <CONTAINER_NAME>
      ![image info](./ss/25.png)

After this, use this container IP or container ID in the the mongoose connection string and build your server image and run it again. Attach the network to the server container too to allow communication between server and database. Note: You do not have to connect the network to the client container
      ![image info](./ss/26.png)
      ![image info](./ss/27.png)
      ![image info](./ss/28.png)

Once all the containers are up and running, you can test the app at http://localhost:3000

This is how the website will look like:
      ![image info](./ss/29.png)
      ![image info](./ss/30.png)
      ![image info](./ss/31.png)

We have successfully deployed our three-tier MERN stack application with docker!

## Conclusion

Utilizing Docker networks and containerizing your MERN stack application, you've created a development environment that is scalable, effective, and portable. This method facilitates smooth deployment to production, simplifies cooperation, and expedites development workflows.

Recall that this is just the start! Examine other production environment improvements, such as integrating with container orchestration systems like Kubernetes and managing volume for permanent data storage. Docker will remain a useful tool in your development toolbox as your MERN application expands.
