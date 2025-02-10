# Building Images

In this exercise, we’ll learn how to create a new Docker image. 

To do this, like in “Changing Images”, we’ll add the ping utility to the ubuntu:16.04 image. The outcome from each of these exercises should be equivalent. 


## Prerequisites 

First download the ubuntu:16.04 image with docker pull. (You might already have this if you have completed the previous tutorial.

![image](https://github.com/user-attachments/assets/44cfab67-53ed-44b6-bee5-bcd3045b144b)


If you still have the image from the previous “Changing images”, let’s also remove that now. Be sure to docker rm any containers based on that image first, otherwise deleting the image will fail.  


![image](https://github.com/user-attachments/assets/98ed0f7b-3658-4f95-9f37-06d38d5f42b8)


## Creating a Dockerfile 

Like the “Changing Images” exercise, we’ll install ping on top of ubuntu to create a new image. Unlike that exercise, however, we will not run or modify any containers to do so.  

Instead, we’ll use Dockerfile, The Dockerfile is a “recipe” of sorts, that contains a list of instructions on how  to build a new image. 

1. Create a new file named Dockerfile in your working directory:

![image](https://github.com/user-attachments/assets/d47b7148-0352-4a35-a220-9ccede469601)


2. Open Dockerfile with your favourite editor. 

3. Inside the file, we’ll need to add some important headers.


![image](https://github.com/user-attachments/assets/ab4bf878-c293-47f9-a286-57665c71c69a)


The FROM directive specifies what base image this new image will be built upon. 	(Ubuntu in our case.) 

The LABEL directive adds a label to the image. Useful for adding metadata. 

Add the following two lines to the top of your file 


![image](https://github.com/user-attachments/assets/c0fdad7a-bcc3-486c-9fbf-0885d6d937b8)


Running docker images, we can see our newly built image. 


![image](https://github.com/user-attachments/assets/7d54b2d3-f58d-4965-bb87-f70d45683d07)


## Optimizing the Dockerfile 

Looking at new image, we can see it is 159MB in size versus its base image of 117MB. That's a pretty big change in size for installing some utilities. That will take more disk space and add additional time to pushes/pulls of this image. 

But why is it that much bigger? The secret is in the RUN commands. As mentioned before, any filesystem changes are committed after the RUN command completes. This includes any logs, or temporary data written to the filesystem which might be completely inconsequential to our image. 

In our case, the use of apt-get generates a lot of this fluff we don't need in our image. We'll need to modify these RUN directives slightly. 

We can start with removing any old logs after the install completes. Adding the following to the bottom of the Dockerfile: 


![image](https://github.com/user-attachments/assets/f341ea13-726f-4696-adc4-a9aa8cc0331a)


Then after rerunning build, our images now like: 


![image](https://github.com/user-attachments/assets/be4ffa87-e855-4e53-a7d5-7cf91a6a11f3)


There are many other useful directives available in the Dockerfile. 

## Some important ones: 

COPY: Copy files from your host into the Docker image. 

WORKDIR: Specify a default directory to execute commands from. 

CMD: Specify a default command to run. 

ENV: Specify a default environment variable. 

EXPOSE: Expose a port by default. 

ARG: Specify a build-time argument (for more configurable, advanced builds.) 

Since our Dockerfile is build for ping, let's add the ENV and CMD directives. 


![image](https://github.com/user-attachments/assets/a0bbce97-bdc4-406a-90e8-3714ae9e69a3)


Now we can ping without going in the container : 


![image](https://github.com/user-attachments/assets/bee7bea0-3703-434a-99c2-3ef597bfd58b)















