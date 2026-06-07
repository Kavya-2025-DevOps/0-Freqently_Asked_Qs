**Frequestly Asked**

1. Difference b/w Docker Image and Docker Container 
2. Difference between COPY and ADD
3. Difference between CMD and ENTRY POINT
4. Write a Docker File
5. What is Multi Stage Build and write an Example.
6. 



**Others Prepare**

Level 1: Core Docker Fundamentals

1.	What problem does Docker solve compared to traditional virtualization?  
In traditional virtualization the virtual machines use independent OS systems for each virtual machine and this make them heavier and slow.
But in docker containers they share the host OS, making them lightweight and fast to start and it also ensures the consistency across the environments (dev, test, production) by packaging the application and their dependencies together. It also reduces the compatibility issues and improves the scalability and deployment speed

2.	Difference between Docker image and container  
  Docker Image:  
  Definition: A read-only template with application code, libraries, and dependencies
  State: Static and immutable  
  Purpose: Used to create containers  
  Storage: Stored on disk (e.g., in registries like Docker Hub)  
  Lifecycle: Built once and reused  
  Analogy: Like a class in programming  
  **  
  Docker Container:
  Definition: A running instance of a Docker image  
  State: Dynamic and can be modified during execution  
  Purpose: Executes the application  
  Storage: Runs in memory with a writable layer  
  Lifecycle: Created, started, stopped, and deleted  
  Analogy: Like an object (instance of a class)  



4.	How can a container be modified?  
A Docker container can be modified in a few practical ways:
•	Inside the running container: You can execute commands (docker exec) to install packages, edit files, or change configurations. These changes affect only that container instance.
•	Writable layer: Containers have a thin writable layer on top of the image, so any changes (files, logs, configs) are stored there during runtime.
•	Commit changes: You can save the modified container as a new image using docker commit, preserving the updates.
•	Best practice: Instead of modifying live containers, update the Dockerfile and rebuild the image for consistent and repeatable changes.


5.	What is a Dockerfile?  
A Dockerfile is a text file that contains step-by-step instructions to automatically build a Docker image. It defines the base image, application code, dependencies, and commands needed to run the app. By using a Dockerfile, you can create consistent and repeatable environments across different systems.
  
6.	Difference between:  
a.	COPY vs ADD  
Aspect	COPY	ADD
Function	Copies files from host to container	Copies files + supports extra features
Features	Simple file copy only	Can extract compressed files and download from URLs
Use case	Preferred for basic copying	Used when needing auto-extraction or remote download
Behavior	Predictable and straightforward	More complex due to additional functionality

b.	RUN vs CMD  
Aspect	RUN	CMD
Purpose	Executes commands during image build	Specifies default command to run when container starts
Execution time	Build time	Runtime
Result	Creates new image layers	Does not create layers, just sets default command
Overriding	Cannot be overridden at runtime	Can be overridden using docker run
Example use	Install packages (apt-get install)	Start app (node app.js)

c.	CMD vs ENTRYPOINT  
Aspect	CMD	ENTRYPOINT
Purpose	Sets default command/arguments	Sets the main command to run
Overriding	Easily overridden by docker run command	Not easily overridden (needs --entrypoint)
Flexibility	Flexible, used for default behavior	Rigid, used for fixed execution
Behavior	Runs only if no command is given	Always executes when container starts
Usage together	Provides default arguments to ENTRYPOINT	Works with CMD to form full command
Example analogy	Default parameters	Main executable  

6.	Differences between docker run vs docker start  
Aspect	docker run	docker start
Purpose	Creates and starts a new container from an image	Starts an existing stopped container
Container state	Always creates a fresh container	Reuses the same container
Image usage	Requires a Docker image	Does not use image (container already exists)
First-time use	Used when running a container for the first time	Used for restarting stopped containers
Customization	Can pass new configs, ports, environment variables	Uses existing configuration
Example	docker run ubuntu	`docker start  

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

