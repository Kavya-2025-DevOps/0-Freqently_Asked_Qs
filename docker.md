**Frequestly Asked**

1. Difference b/w Docker Image and Docker Container 
2. Difference between COPY and ADD
3. Difference between CMD and ENTRY POINT
4. Write a Docker File.
5. What is Multi Stage Build and write an Example.
6. Explain Docker Architecture.


##

**Others Prepare**

Level 1: Core Docker Fundamentals

1.	What problem does Docker solve compared to traditional virtualization?  
In traditional virtualization the virtual machines use independent OS systems for each virtual machine and this make them heavier and slow.
But in docker containers they share the host OS, making them lightweight and fast to start and it also ensures the consistency across the environments (dev, test, production) by packaging the application and their dependencies together. It also reduces the compatibility issues and improves the scalability and deployment speed

5.	What is a Dockerfile?  
A Dockerfile is a text file that contains step-by-step instructions to automatically build a Docker image. It defines the base image, application code, dependencies, and commands needed to run the app. By using a Dockerfile, you can create consistent and repeatable environments across different systems.

4.	How can a container be modified?  
A Docker container can be modified in a few practical ways:  
•	Inside the running container: You can execute commands (docker exec) to install packages, edit files, or change configurations. These changes affect only that container instance.  
•	Writable layer: Containers have a thin writable layer on top of the image, so any changes (files, logs, configs) are stored there during runtime.  
•	Commit changes: You can save the modified container as a new image using docker commit, preserving the updates.  
•	Best practice: Instead of modifying live containers, update the Dockerfile and rebuild the image for consistent and repeatable changes.  


2.	Difference between  
*a.	Docker image and container*    
  **Docker IMAGE:**  
  Definition: A read-only template with application code, libraries, and dependencies
  State: Static and immutable  
  Purpose: Used to create containers  
  Storage: Stored on disk (e.g., in registries like Docker Hub)  
  Lifecycle: Built once and reused  
  Analogy: Like a class in programming  
  **Docker CONTAINER:**  
  Definition: A running instance of a Docker image  
  State: Dynamic and can be modified during execution  
  Purpose: Executes the application  
  Storage: Runs in memory with a writable layer  
  Lifecycle: Created, started, stopped, and deleted  
  Analogy: Like an object (instance of a class)  

  *b.	COPY vs ADD*    
  **COPY**  
  Function: Copies files from host to container  
  Features: Simple file copy only  
  Use case: Preferred for basic copying  
  Behavior: Predictable and straightforward    
  **ADD**  
  Function: Copies files + supports extra features  
  Features: Can extract compressed files and download from URLs  
  Use case: Used when needing auto-extraction or remote download  
  Behavior: More complex due to additional functionality  


  *c.	RUN vs CMD*   
  **RUN**  
  Purpose: Executes commands during image build  
  Execution time: Build time  
  Result: Creates new image layers  
  Overriding: Cannot be overridden at runtime  
  Example: use	Install packages (apt-get install)  
  **CMD**  
  Purpose: Specifies default command to run when container starts
  Execution time: Runtime  
  Result: Does not create layers, just sets default command  
  Overriding: Can be overridden using docker run  
  Example: Start app (node app.js)   


 *d.	CMD vs ENTRYPOINT*    
 **CMD**  
 Purpose: Sets default command/arguments  
 Overriding: Easily overridden by docker run command  
 Flexibility: Flexible, used for default behavior  
 Behavior: Runs only if no command is given  
 Usage: Provides default arguments to ENTRYPOINT  
 Example: Default parameters
 **ENTRYPOINT**  
 Purpose: Sets the main command to run  
 Overriding: Not easily overridden (needs --entrypoint)  
 Flexibility: Rigid, used for fixed execution  
 Behavior: Always executes when container starts  
 Usage: Works with CMD to form full command  
 Example: Main executable  
 

 *e. docker run vs docker start*    
 **Docker RUN**  
 Purpose: Creates and starts a new container from an image  
 Container state: Always creates a fresh container  
 Image usage: Requires a Docker image  
 First-time use: Used when running a container for the first time  
 Customization: Can pass new configs, ports, environment variables  
 Example: docker run ubuntu  
 **Docker START**  
 Purpose: Starts an existing stopped container  
 Container state: Reuses the same container  
 Image usage: Does not use image (container already exists)  
 First-time use: Used for restarting stopped containers  
 Customization: Uses existing configuration  
 Example: docker start  

*f.	Alpine vs Ubuntu base images (pros/cons)*  
**Alpine**  

**Ubuntu base images**  


7.	What is a Docker layer?  
A Docker layer is a reusable, read-only unit that makes up a Docker image. Each instruction in a Dockerfile (like RUN, COPY) creates a new layer. Layers are stacked on top of each other to form the final image, which makes builds faster and efficient through caching and reuse.   

9.	How does Docker caching work?  
Docker caching works by reusing previously built layers of an image to speed up builds.
•	Each instruction in a Dockerfile (like FROM, RUN, COPY) creates a layer.  
•	During a rebuild, Docker checks if the instruction and its context (files, dependencies) have changed.  
•	If nothing changed, Docker reuses the cached layer instead of rebuilding it.  
•	If a change is detected, that layer and all subsequent layers are rebuilt.  
This makes builds much faster when only small parts of the Dockerfile are modified.  

10.	What is .dockerignore and why is it important?  
A (.dockerignore) file is used to specify files and folders that should be excluded when building a Docker image.
•	It works like .gitignore, preventing unnecessary files (e.g., node_modules, logs, temp files) from being sent to the     Docker daemon.  
•	Reduces build time by minimizing the build context size.  
•	Helps keep images smaller and more secure by excluding sensitive or irrelevant data.  
•	Improves caching efficiency by avoiding unwanted file changes.  

11.	 What happens when a container exits?  
When a Docker container exits, the following happens:  
•	The main process (PID 1) inside the container stops running.  
•	The container moves to a stopped (exited) state, but it is not deleted.  
•	Any data stored in the container’s writable layer remains (unless the container is removed).  
•	Logs and exit code can still be viewed using docker logs and docker inspect.  
•	You can restart it later using docker start, if needed.  

##

Level 2: Intermediate / Practical  

1.	How do you reduce Docker image size?  
You can reduce Docker image size using these practices:  
•	Use a smaller base image (e.g., alpine instead of full OS images).  
•	Use multi-stage builds to separate build dependencies from final runtime image.  
•	Remove unnecessary files using .dockerignore (logs, tests, node_modules, etc.).  
•	Combine RUN commands to reduce the number of layers.  
•	Clean up package caches (e.g., apt-get clean, rm -rf /var/lib/apt/lists/*).  
•	Install only required dependencies, avoid dev tools in production images.  
•	Use specific image tags instead of large or “latest” bloated images.  


2.	What is multi-stage build? Give a real use case.  
A multi-stage build in Docker is a technique where a Dockerfile uses multiple stages (FROM statements) to separate the build environment from the final runtime environment.  
•	In the first stage, you compile or build the application (with all build tools).  
•	In the final stage, you copy only the required output (like compiled code) into a smaller image.  
•	This helps create smaller, secure, and production-ready images.  
Real usecase / Example:For a Node.js or Java application:  
•	Stage 1: Install dependencies and build the project (npm install, npm run build or Maven build).  
•	Stage 2: Use a lightweight base image (like node:alpine or openjdk:jre) and copy only the built files.  
Result: Final image does not include source code, build tools, or unnecessary dependencies, making it much smaller and faster to deploy.  

4.	How do you pass environment variables securely?

6.	What are Docker volumes vs bind mounts?  
7.	Explain Docker networking?    
b.	bridge
c.	host
d.	overlay
8.	How containers communicate across hosts?  
9.	What is Docker Compose? When NOT to use it?  
10.	How do you debug a failing container?  
11.	How do you handle logs in Docker?  



##
Level 6: Scenario-Based Questions

Scenario 1: Your container works locally but fails in Kubernetes. Why?   
A container that works locally but fails in Kubernetes usually depends on assumptions that don't hold in the cluster environment—such as missing environment variables, networking differences, unavailable volumes, resource limits, security restrictions, or startup-order issues.  
I would start by checking pod status, events, logs, environment variables, and resource constraints using kubectl describe and kubectl logs to identify where the runtime environment differs from local Docker execution.  

Scenario 2: Docker image size is 1.5GB → reduce to <200MB.  
Scenario 3: Container crashes randomly in production. No logs.  
Scenario 4: High CPU usage in container but host is fine.  
Scenario 5: Deployment fails due to “port already in use”  
Scenario 6: You need zero-downtime deployment using Docker.  
Scenario 7: Docker build is very slow in Jenkins pipeline.  

