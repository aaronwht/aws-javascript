# Deploy React to AWS

![Deploy React to AWS](https://www.aaronwht.com/images/s3-build/deploy-react-to-aws.jpg)


### Overview
The code in this repository is configured to deploy to AWS S3 using AWS CodePipeline. The coorelating server-side Node.js code may be found at https://github.com/aaronwht/aws-javascript-api.

Coorelating Video Tutorial
https://www.youtube.com/watch?v=5fyAgfe67Qg

### Running locally
Create an ```.env``` file in the root of your project as pictured below.  As a convention, ```.env``` files store environment variables and are not checked into code repositiories as they often include sensative, or environment-related, information.  This is typically done by including the ```.env``` file in your ```.gitignore``` file.

![.env file](https://www.aaronwht.com/images/s3-build/env-variables.png)

Create the variable ```REACT_APP_API``` and set it's value to ```http://localhost:8080/```. (include the suffix slash)  This will point to the coorelating API endpoint specified above.

Run the below commands to install NPM packages locally and run the application.

```npm install```

```npm start```

The application should run locally.

The ```buildspec.yml``` file (code below) runs on the AWS build server.
```
version: 0.1
phases:
    install:
        commands:
        - npm install
        - npm run build
        - aws s3 cp build s3://$S3_BUCKET --recursive --exclude 'index.html'
        - aws s3 cp build/index.html s3://$S3_BUCKET
```

```npm install``` installs NPM packages on the AWS build server.

```npm run build``` creates a production version of the application on the AWS build server in the ```build``` folder.

```aws s3 cp build``` uses the AWS CLI (which is automatically installed on the build server) to recursively copy the contents of the ```build``` folder to the AWS S3 bucket using the environment variable ```$S3_BUCKET```.  This environment variable's value is used by the ```AWS build pipeline``` (displayed below).

![environment variable](https://www.aaronwht.com/images/s3-build/pipeline-envs.png)
