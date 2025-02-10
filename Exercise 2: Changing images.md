# Changing Images

In this exercise, we’ll learn how to modify an existing Docker image, and commit it as a new one.To accomplish this, we’ll modify the ubuntu:16.04 image to include the ping utility.


## Prerequisites: 

First download the ubuntu:16.04 image with docker pull. (You might already have this if you have completed the previous tutorial).

![image](https://github.com/user-attachments/assets/1fe25d11-492e-4959-ac7f-6e97e58ed2e3)


## Modifying an image 

Let’s run the image in a new container and install the ping utility.  

1. First start the container with /bin/bash: 

![image](https://github.com/user-attachments/assets/233a8cbb-aa47-44cb-a86d-4e6504aeef8e)

2. Try running ping in the terminal: 


![image](https://github.com/user-attachments/assets/19b3018f-7ebc-4869-b768-21d1f742a1bc)


The command doesn’t exist. The Ubuntu image for Docker only has the bare minimum of software installed to operate the container. That’s okay though: we can 	install the ping command.  

3. But first we’ll update our software list. 

In Debian-based Linux environments (such as Ubuntu), you can install new software using the apt package manager. For those who have experience with	Macs, this program is the equivalent of homebrew. By default. To reduce the image size, the Ubuntu image doesn’t have a list of the available software packages. We need to update the list of available software: 


![image](https://github.com/user-attachments/assets/b789a1ce-320a-458c-8eb9-d8c98b27e72e)


4. Now we can install the ping command. 

Call apt-get install iputils-ping to install the package containing pin


![image](https://github.com/user-attachments/assets/5f396a89-afda-4282-93d5-72c41f1dfab1)


![image](https://github.com/user-attachments/assets/2101fa6b-01e1-4734-bf4d-801b703bd30e)


5. Finally, we should be able to use ping. 

Ping your favourite website. When you’ve seen enough, Ctrl+C to interrupt, then exit 	the container.  

![image](https://github.com/user-attachments/assets/e945464a-720d-4e8c-aba0-493500fc960b)


## Committing changes 

Installing ping isn’t very special in itself. But what if you wanted to have ping on all of your ubuntu containers? You’d have to redo this installation each time you spin up a new container, and that isn’t much fun. 

The Docker way is to create a new image. There are two ways to do this: 1) build a new image from scratch or 2) commit a container state as a new image. We’ll cover how to do #1 in the “Building images” exercise, but we can do #2 now. 

1. Let’s find our container to create the new image from. 

Fortunately, we have a Docker container with our ping utility already installed from the previous steps. It should be stopped right now, but let’s find its container ID. 

![image](https://github.com/user-attachments/assets/59394fcc-0f45-4c4e-8f64-f1ecfbb00ce9)


2. Now let’s commit it as a new image 

Docker commit takes a container, and allows you to commit its changes as a new 	image 

![image](https://github.com/user-attachments/assets/dad60c59-42aa-4db4-a202-809eb9cb421b)


Pass the container ID, an author, commit message, and give it the name <DockerHub username>/ping: 

![image](https://github.com/user-attachments/assets/51ae43a6-a7a3-4f68-bab7-f6ca05dc1961)


Then check docker images to see your new image: 

![image](https://github.com/user-attachments/assets/f22cca28-b746-4a4c-893e-f053a644ae89)


3. Finally run your new image in a new container to see it in action!

![image](https://github.com/user-attachments/assets/38abce60-b964-44de-a98b-71ac88623ee6)


## Tips if you are getting errors: 

Make sure your container is running before trying to commit 

Make sure you installed the ping utility on the container that you are trying to commit to 

Log in to find your dockerhub username on docker desktop. Sign up with your personal account if you do not have. 








