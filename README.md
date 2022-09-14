# docker
Docker primer



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
