- When we will run `docker build .` , then docker will look for a file with name as Dockerfile - means it will not run Dockerfile.dev. We need a way to
  run Dockerfile.dev

- docker build -f {fileName} {source} , where -f specififes the file name

`docker build -f Dockerfile.dev .`

- Building a docker file will return the image id of the container
- We can use this image id to run the app

`docker run -p 3000:3000 <imageId>`

- Afer we have started our app inside the container , we observed that whenever we are making any change in the react component file and saving it then changes are not getting reflected. We can either fix this problem by building the docker file again and again , but this will be very much time consuming .So we have to find some other method to fix this problem

- When we are copying our local file/folder inside docker container , then we are copying the folder . Any other change will not reflect untill we create a build of that.To fix this issue, we have something called <b>docker volume</b> . In docker container we are going to store the reference of this file and folders on the docker container

`docker run -p 3000:3000 -v /app/node_modules -v$(pwd):/app <imageId> `

In above we are bookmarking the node_modules folder and mapping the present working directory in to the /app folder

`docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app  sha256:181db76b8fd337c7f6928da33d92b28b72b932de6d8450b023a28448d3632965`

we are bookmarking the node_modules folder here because we do not have node_modules folder in local . Here we are telling the docker container that please do not look for the node_modules as a reference.

- We cannot run the above commands again and again , so we can create a docker-compose.yml files which we can build to run the app inside the container .
  Running `docker-compose up ` command here will give error as it will be not able to find the file with name as <b>Dockerfile</b>

- <b>Overriding Dockerfile: </b>

```
  build:
    context: .
    dockerfile: Dockerfile.dev
```

- Using volumes , we are storing the reference to the local folder from our docker container .Now the question arises Do we need to copy all files & folders in our docker file ?

No , now there is no need to copy all docker files and folders.

- We have to run our tests too for the app : - for this we can make a new service or - or attach an instance to our already running service

### Methods to run test inside the container

<ul>
<li>
We have to run our app inside the docker container . Then we have to execute a command inside that docker container
<b>e.g</b><code> docker exec -it 17640c24aa97 npm run test</code>, where 17640c24aa97 is the id of the container
</li>
<li>We have to override the <b>CMD</b> command in docker file if we are creating a new service to run tests of our application</li>
</ul>


### Attaching to the container
```docker attach fc6e5aca42b ```, where fc6e5aca42b is a container id . 
When we are using docker attach , we are attaching to the stdin on the container.If we want to execute command we will attach to the terminal (start.js) not to the npm.

``` docker exec -it d71482aa92a0 sh ``` where d71482aa92a0  is a container id ,this command will execute the shell inside the container

### nginx
 - Takes incoming traffic & responds to it with some static file
 - when someone visits our app then we should return only two files - index.html & main.js
 - we have to create a separate docker file for production build

