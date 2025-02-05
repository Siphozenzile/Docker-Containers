## Running Containers

In this exercise, we’ll learn the basics of pulling images, starting, stopping, and removing containers. 

## Prerequisites: 

Install docker Desktop: https://hub.docker.com/

Study docker curriculum: https://docker-curriculum.com/ (Optional) 

Keep docker desktop running in background  

## Pulling an Image 

To run containers, we’ll first need to pull some images. 

1. Let’s see what images we have currently on our machine, by running docker images:

![image](https://github.com/user-attachments/assets/cf9252ea-4e51-4322-8606-ef3e135b55f9)


2.	On a fresh docker installation, we should have no images. Let’s pull one from 		dockerhub 

We usually pull images from DockerHub by tag: 


![image](https://github.com/user-attachments/assets/cc549f3a-403e-4050-b53e-7290ea4b86e3)


We can search for images using docker search <keyword> 

![image](https://github.com/user-attachments/assets/e25a6232-2463-4822-85e3-8a4956c352f4)


You can also find images online at https://hub.docker.com/

Run docker pull ubuntu:16.04 to pull an image of Ubuntu 16.04 from DockerHub. 


![image](https://github.com/user-attachments/assets/db8b9c1b-6a2d-489f-b459-0b7973a9d3ec)


3. We can also pull different versions on the same image. 

Run docker pull ubuntu:16.10 to pull an image of Ubuntu 16.10 

![image](https://github.com/user-attachments/assets/8f465e1e-8d56-4c41-9559-1ec4c5d42f22)


Then if we run docker images again, we should get: 


![image](https://github.com/user-attachments/assets/587def58-a046-4012-81f7-ffa5bad5d7e6)


4. Over time, your machine can collect a lot of images, so it’s nice to remove unwanted images. 

Run docker rmi <IMAGE ID> to remove the Ubuntu 16.10 image we won’t be using. 

![image](https://github.com/user-attachments/assets/670d0958-61b6-4a62-8ef0-9f7530e2a4e9)


Alternatively, you can delete images by tag or by a partial image ID. In the previous example, the following would have been equivalent: 

Docker rmi 31 

Docker rmi ubuntu:16.10 

Running docker images should reflect the deleted image.  

![image](https://github.com/user-attachments/assets/a9b716e4-ac67-4255-96fe-02feac3ba966)

### Running our container 

Using the Ubuntu 16.04 image we downloaded, we can run our first container. Unlike a traditional virtualization framework like VirtualBox or VMWare, we can’t just start a virtual machine running this image without anything else: we have to give it a command to run. 

The command can be anything you want, as long as it exists on the image. In the case of the Ubuntu image, it’s a Linux kernel with many of the typical applications you’d find in a basic Linux environment. 


1. Let’s do a very simple example. Run docker run ubuntu:16.04 /bin/echo ‘Hello World!’ 

![image](https://github.com/user-attachments/assets/5830e7d9-abd5-45ec-9376-58245a709100)


The /bin/echo command is a really simple application that just prints whatever you 	give it to the terminal. We passed it “Hello world!”, so it prints Hello world!  to the 	terminal. 

When you run the whole docker run command, it creates a new container from the 	image being specified, then runs the command inside the container. From the 	previous example, the Docker container started, then ran the /bin/echo command 	in the container. 


2. Let’s check what containers we have after running this. Run docker ps

![image](https://github.com/user-attachments/assets/b69419ae-34af-49b7-8309-9e88e8f31a26)

The ps command doesn’t show stopped containers by default, add the –a flag. 


![image](https://github.com/user-attachments/assets/b16c9bc3-3255-4613-a77a-4837df68f046)


Okay, there’s our container. But why is the status “Exited”? 

Docker containers only run as long as the command it starts with is running. In our 	example, it ran /bin/echo successfully, printed some output, then exited with status 	code 0 (which means no errors.) When docker saw this command exit, the 		container stopped. 


3. Let’s do something a bit more interactive. Run docker run ubuntu:16.04 /bin/bas

![image](https://github.com/user-attachments/assets/35e3796b-f6e5-46f5-8d06-7d232b2a4dc7)


Notice nothing happened. When we run docker ps –a 


![image](https://github.com/user-attachments/assets/631a13a5-ae19-47d8-8dbe-c0e19f0b4309)


The container exited instantly. Why? We were running the /bin/bash command, 	which is an interactive program. However, the docker run command doesn’t run 	interactively by default, therefore the /bin/bash command exited, and the container 	stopped. 

Instead, let’s add the –it flags, which tells Docker to run the command interactively 	with your terminal 


![image](https://github.com/user-attachments/assets/aae410e0-9c95-41a8-b162-70adb42a9367)


This looks a lot better. This means you’re in a BASH session inside the Ubuntu 	container. Notice you’re running as root and the container ID that follows. 

You can now use this like a normal Linux shell. Try pwd and ls to look at the file 	system 


![image](https://github.com/user-attachments/assets/95e00dd6-d466-473f-aa87-9e45863384bb)


You can type exit to end the BASH session, terminating the command and stopping the container.


![image](https://github.com/user-attachments/assets/1d5703ff-73f1-4671-b14b-ddba02c19a95)


4. By default your terminal remains attached to the container when you run docker run. What if you don’t want to remain attached? 

By adding the –d flag, we can run in detached mode, meaning the container will 	continue to run as long as the command is, but won’t print the output. 

Let’s run /bin/sleep 3600, which will run the container idly for 1 hour. 


![image](https://github.com/user-attachments/assets/4600861a-ae63-4413-9de6-b627fad3d758)


If we check the container, we can see it’s running the sleep command in a new container. 

![image](https://github.com/user-attachments/assets/f0bfd5fd-203f-4c65-83b3-fd9ce8fc6cb1)


5. Now that the container is running in the background, what if we want to reattach it? 

Conceivably, if this were something like a web server or other process where we	might like to inspect logs while it runs, it’d be useful to run something on the 		container without interrupting the current process. 

To this end, there is another command , called docker exec. Docker exec runs a commmand within a container that is already running. It works exactly like docker 	run, except instead of taking an image ID, it takes a container ID 

This makes the docker exec command useful for tailing logs, or “SSHing” into an active container 

Let’s do that now, running the following, passing the first few characters of the container ID: 


![image](https://github.com/user-attachments/assets/86973355-b97c-4124-a765-cfa94c30dd22)


The container ID appearing at the front of the BASH prompt tells us we’re inside the 	container. Once inside a session, we can interact with the container like any SSH 	session. 

Let’s list the running processes: 


![image](https://github.com/user-attachments/assets/8176ec46-73f2-48aa-ad12-8be0f4284690)


There we can see our running /bin/sleep 3600 command. Whenever we’re done, we 	can type exit to exit our current BASH session, and leave the container running.


![image](https://github.com/user-attachments/assets/5df5ce35-29e8-438e-aab5-ea5d1a2d7bfa)


And finally checking docker ps, we can see the container is still running. 

6. Instead of waiting 1 hour for this command to stop (and the container exit), what if we’d like to stop the Docker container now? 

To that end, we have the docker stop and the docker kill commands. The prior is a 	graceful stop, whereas the latter is a forceful one. 

Let’s use docker stop, passing it the first few characters of the container name we 	want to stop. 


![image](https://github.com/user-attachments/assets/9fefbcd9-d4fc-4fb8-a0cb-98c0ed50b2c0)


Then checking docker ps –a... 


![image](https://github.com/user-attachments/assets/336c1160-9e58-48b5-a132-d6c83b834d46)


We can see that it exited with code 137, which in Linux world means the command 	was likely aborted with a kill –9 command. 


### Removing containers 

7. After working with Docker containers, you might want to delete old, obsolete ones.

![image](https://github.com/user-attachments/assets/8cf6e96d-35af-4710-ae1a-7ade45abf250)


From our previous example, we can see with docker ps –a that we have a container 	hanging around. 

To remove this we can use the docker rm command which removes stopped containers. 

![image](https://github.com/user-attachments/assets/cad6e0dc-3ae0-42a6-8eb4-a99000d3bb63)

A nice shortcut for removing all containers from your system is docker rm $(docker 	ps –a -q) 

It can be tedious to remove old containers each time after you run them. To address 	this, Docker also allows you to specify the –rm flag to the docker run command, 	which will remove the container after it exits. 

![image](https://github.com/user-attachments/assets/7c8a4a8d-ebcc-4ea5-a8b8-95f7e0521e5a)






