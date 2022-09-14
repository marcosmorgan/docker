# docker

## Tweetorial

Installing software can be tedious and time-consuming. And then you get the incompatible new software version, and you have to start again from scratch, and your old scripts no longer work. Frustration. No bueno. 

Solution to the problem: Docker.

A thread on how to get started. 

So what is Docker? It is software that allows you to run many virtual machines from your own 💻. 

For example, if you are on a Mac and want to run a Linux program, you can use Docker to recreate that Linux machine with that program. 

For commonly used programs, most likely, your trusted developer already has a docker virtual machine with the program already installed and ready to use!

## 0.0 Docker images & containers

Before we start, I want to introduce two concepts; docker images and docker containers. The only two new concepts you will need.

A docker image is a copy of the "virtual machine.”

A docker container is a docker image that is actually running. 

Just to understand this a little better. From the same docker image, you can launch multiple containers that could be doing different things, such as running different programs.

What I want to show you today are the basic concepts and commands that will give you 80% of Docker functionality.

First, I will show you how to get docker images.

Second, I will show you two ways to launch and use docker containers.

Third, we will cover how to generate new images.

First, where do I get the docker images from? If you just google “docker my_favority_program,” you will probably land on the hub.docker.com page with the info of an image with your program put together by a developer.

Next, you just need to pull the image from the internet with the following command.

## 1.1 $docker pull a_developer/my_favorite_image

Also, an image can be loaded from a single file using the load command.

## 1.2 $docker load --input my_favorite_image.tar

Yes, you can put all of an image in a tar file and move it around as you please. We will see how to generate these files below. 

You can get as many images on your computer as you can fit. Each one is around 1 to 10 Gigs big. To keep track of them, use the “images” command.

## 1.3 $docker images

This will give you a list of all your images. 

Eventually, you will need to clean some of them. Just use the following:

## 1.4 $docker rmi rando_dev/useless_image

OK, that was easy. Now, how do I launch a container and start using the program? 

There are at least two ways of doing this, and I will show you both. 

The most intuitive way is to use the container interactively. You can do so as follows.

## 2.1.0 $docker run -it my_favorite_image bash

After running this command, you will have access to the terminal inside the container.

[container]$ my_favorite_program -h

Yes, that's it. Now you can run your program.

To see what containers are running, use:

## 2.1.1 $docker ps

This will show you all the containers running and their IDs.

And remember to close your containers after you are done with them using their IDs with the following command.

## 2.1.2. $docker rm c0NtaiN3RiD 

However, in many cases, we would just want to call the program inside the container directly from our computer terminal without interacting with the container's internal terminal. We can do that as follows: 

## 2.2 $docker run --rm my_favorite_program -h

There is one problem remaining. You still need to give the container access to the files on your computer so that it can get some input data and then spit the output. How do we do that? We use the -v and -w flags.

Let's see them one at a time.

## 2.3.1 $docker run --rm -v $(pwd):$(pwd) my_favorite_program -h

This will give the container access to the current working directory (i.e., $(pwd)) and all its subdirectories. 

We are also creating the same path to the working directory in the container.

To make the current working directory in the computer also the current working directory in the container, we use the -w flag.

## 2.3.2  $docker run --rm -v $(pwd):$(pwd) -w $(pwd) my_favorite_program -h

Note that $(pwd) can be changed for other directories if you wish. 

So now you are good to go. You can run your program from your terminal, having access to all subdirectories and files in the current directory.

To finalize, we need to discuss how to handle needed software upgrades and how to install software inside preexisting ones. 

So, what happens when we need a new version of the software incompatible with previous scripts? 

We can still pull the old compatible versions when needed using the older version tag. Example.

## 3.1 $docker pull a_develper/pull my_favorite_image:1.0

You can find the different versions/tags of your image at its hub.docker.com page.

Now, what happens when there is no docker image available for a program we want to use? 

We can install it on top of a preexisting docker image. I will show you two ways to do this. 

## 3.2.1 Save the container as an image.

One way to create an image with a given software is to launch an interactive container (see 2.1) and install the software from the command line as usual. 

Once that is done, we can convert the current container into a new image with:

$docker commit c0NtaiN3RiD  me/my_image:new_version

Of course, you can run into problems with the installation at this point, but you have several advantages. 

First, you can install the software on whatever computer type and version you want. 

Second, you can start with an image with the thorniest part of the installation already done. 

Third, since you can discard an image almost without cost, you do not have to worry too much about breaking stuff. (i.e., you do not have to worry about not breaking other software already installed.)

## 3.2.2 Dockerfiles

An alternative way to create images is to use dockerfiles. This is how you can install R and an R library on an Ubuntu computer. First, create a file, name it dockeRfile, and put the following inside:

FROM ubuntu 

RUN apt-get update 
RUN apt-get install --no-install-recommends -y \
      r-base 
RUN R -e 'install.packages("remotes")'

Then use the following command to generate the docker image:

$docker build -t me/docker_r:1.0 - < dockeRfile

If you want to save your new images as files, you can do it using the save command.

## 3.3 $docker save -o my_image_R.tar me/docker_r:1.0

Well, that's it for today. This is what we covered.

0. images & containers

1.1 $docker pull
1.2 $docker load
1.3 $docker images
1.4 $docker rmi 

2.1 Interactive containers -it
2.1.1 $docker ps
2.1.2 $docker rm 
2.2.  Non-interactive containers -rm
2.3.1 Mounting volumes -v 
2.3.2 Choosing the working directory -w 
3.1 docker tags
3.2.1 $docker commit
3.2.2 Dockerfiles
3.3 $docker save

This is the link to the Docker documentation, with the download page and installation steps. Hopefully, you will need less of the latter from now on. https://www.docker.com/

For the bioinformaticians, you can find a lot of relevant images here: https://hub.docker.com/u/staphb.  

Well, I hope you enjoyed the thread and are ready to go. I put together a file with the commands in this threat, together with some dockerfiles for you to play at: https://github.com/marcosmorgan/docker 




##

# Commands that work

## Get samtools from the web

```
docker pull staphb/samtools
```

## Save the image in a file

```
docker save -o staphb_samtools.tar staphb/samtools
```

## See what images you have

```
docker images
```

## Remove the image

```
docker rmi staphb/samtools
```

## Load the image from the file

```
docker load --input staphb_samtools.tar
```

## Use the image interactively

```
docker run -it staphb/samtools bash
```

## Check your containers

```
docker ps
```

## Remove a container using its ID

```
docker rm ffb4ae691d5e
```

## Run a container

```
docker run --rm staphb/samtools samtools
docker run --rm staphb/samtools pwd
docker run --rm staphb/samtools ls
```

## Load a volume to container and run different commands

```
docker run --rm -v $(pwd):$(pwd)  staphb/samtools samtools
docker run --rm -v $(pwd):$(pwd)  staphb/samtools pwd
docker run --rm -v $(pwd):$(pwd)  staphb/samtools ls
docker run --rm -v $(pwd):$(pwd)  staphb/samtools bash -c "cd ../my/path; ls"
```

## Define the working directory for the container

```
docker run --rm -v $(pwd):$(pwd) -w $(pwd) staphb/samtools samtools
docker run --rm -v $(pwd):$(pwd) -w $(pwd) staphb/samtools pwd
docker run --rm -v $(pwd):$(pwd) -w $(pwd) staphb/samtools ls
```

## Install software in container and save modified container as an image

First install r in the container

```
docker run -it staphb/samtools bash
[container]$ apt-get update
[container]$ apt-get install r-base
```

Then create the image

```
docker commit c0NtaiN3RiD  me/my_image:new_version
```

## Build an image from a dockerfile

```
docker build -t me/docker_r:1.0 - < dockeRfile
```

## Save the image as a .tar file

```
docker save -o my_image_R.tar me/docker_r:1.0
```
