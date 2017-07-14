# Deploy React and Node to Docker
This exercise will take your React + Node code and deploy a containerized web application to a server. There are no specific skills needed for this tutorial beyond a basic comfort with the command line and using a text editor.

## Get and test the code

### Clone repo
Lets start and clone the repository:

```
git clone https://github.com/bmnicolae/docker-react-node.git
cd docker-react-node./
```

### Start React
We want to start and test the react app.

```
cd code/react-redux-realworld-example-app/
npm install
npm start
```

### Start Node
Please install MongoDB Community Edition ([instructions](https://docs.mongodb.com/manual/installation/#tutorials)) and run it by executing `mongod` to start node app.

```
cd code/node-express-realworld-example-app/
npm install
npm run dev
```
## Docker files

### React image

First build the React project:
```
npm run build
```

Let's create our first Docker image, create in code/react-redux-realworld-example-app/ the file Dockerfile:

Pull from docker hub Nginx image
```
FROM nginx
MAINTAINER [your name]
```

Copy the build files
```
COPY build /usr/share/nginx/html
```


Now we can build React Nginx image
```
docker build -t react-nginx .
```

and test it:
```
docker run -d -p 8080:80 react-nginx
```


### Node image


Now is the time for our second Docker image, create in code/node-express-realworld-example-app/ the file Dockerfile:

Pull from docker hub Node image
```
FROM node:8.1.4-alpine
MAINTAINER [your name]
```

Prepare app directory
```
RUN mkdir /code
WORKDIR /code
```

Install dependencies
```
ADD package.json /code/
RUN npm install
```

Bundle app source
```
ADD . /code/
```

Expose the app port
```
EXPOSE 8000
```

Start the app
```
CMD [ "npm", "start" ]
```

Now we can build node image
```
docker build -t node-api .
```

and test it:
```
docker run -p 3000:8000 node-api
```
Error?

## Docker compose