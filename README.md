- When we will run ```docker build .``` , then docker will look for a file with name as Dockerfile - means it will not run Dockerfile.dev. We need a way to
run Dockerfile.dev

- docker build -f {fileName} {source} , where -f specififes the file name

``` docker build -f Dockerfile.dev . ```
  
  - Building a docker file will return the image id of the container 
  -  We can use this image id to run the app

  ``` docker run -p 3000:3000 <imageId> ```

- Afer we have started our app inside the container , we observed that whenever we are making any change in the react component file and saving it then changes are not getting reflected. We can either fix this problem by building the docker file again and again , but this will be very much time consuming .So we have to find some other method to fix this problem

- When we are copying our local file/folder inside docker container , then we are copying the folder . Any other change will not reflect untill we create a build of that.To fix this issue, we have something called <b>docker volume</b> . In docker container we are going to store the reference of this file and folders on the docker container

```docker run -p 3000:3000 -v /app/node_modules -v$(pwd):/app <imageId> ```

In above we are bookmarking the node_modules folder and mapping the present working directory in to the /app folder

```docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app  sha256:181db76b8fd337c7f6928da33d92b28b72b932de6d8450b023a28448d3632965```

we are bookmarking the node_modules folder here because we do not have node_modules folder in local . Here we are telling the docker container that please do not look for the node_modules as a reference.

- We cannot run the above commands again and again , so we can create a docker-compose.yml files which we can build to run the app inside the container .
Running ```docker-compose up ``` command here will give error as it will be not able to find the file with name as <b>Dockerfile</b>

- <b>Overriding Dockerfile: </b>
```
  build:
    context: .
    dockerfile: Dockerfile.dev 
```

- Using volumes , we are storing the reference to the local folder from our docker container .Now the question arises Do we need to copy all files & folders in our docker file ? 

No , now there is no need to copy all docker files and folders.

- 