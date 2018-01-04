title: Angular Server Side Rendering on AWS Lambda
date: 2017-12-31 19:18:48
tags: [Angular, Angular Universal, Serverless, AWS Lambda]
categories: [Angular, Angular Universal, AWS Lambda]
---

This is an example of [Angular Universal](https://angular.io/guide/universal) app running on AWS Lambda.
The finished sample code: [jhmt/ng-ssr-on-aws-lambda](https://github.com/jhmt/ng-ssr-on-aws-lambda)

#### Prerequesites
* Node.js (version 6.9.1 or upper)
* AWS account (for deploying to AWS Lambda)
* [Serverless framework installed](https://www.npmjs.com/package/serverless)
* [AWS Credentials](https://serverless.com/framework/docs/providers/aws/guide/credentials/)

#### Used frameworks/technologies
* [Angular CLI](https://cli.angular.io/)
* [Webpack](https://webpack.js.org/)
* [Express](http://expressjs.com/)
* [Serverless framework](https://serverless.com/)

## How does AWS Lambda render html?
Angular Universal works as a middleware for web server frameworks.
e.g. [Express](http://expressjs.com/), [Hapi](https://hapijs.com/) and [ASP.NET](https://www.asp.net/).
Meanwhile, an Express app can work on AWS Lambda with using "aws-serverless-express" node module provided by AWS. In this sample, AWS Lambda renders html with [aws-serverless-express](https://github.com/awslabs/aws-serverless-express) and [Angular Express Engine](https://github.com/angular/universal/tree/master/modules/express-engine).


## Clone and run universal-starter
<pre class="brush: bash;">
git clone https://github.com/angular/universal-starter.git
</pre>


## Prepare deploy
1. Move "server.ts" and "webpack.server.config.js" in another folder (e.g. "/ssr")
2. Split the "listen" block into the new file to start in order to reuse the app on AWS Lambda.
3. Modify "webpack:server" script in "package.json" to find the moved webpack file.
4. Make sure "npm run build:ssr" works fine.

## Initialize Serverless
1. create "serverless.yml" file. 
<pre class="brush: bash;">
serverless create --template aws-nodejs --name ng-ssr-aws-lambda
</pre>

## Create Lambda entry point
1. create a new folder to store lambda function handler and webpack config (e.g. "lambda")
2. create a handler file (e.g. index.ts)
3. create a webpack config file.

## Modify serverless.yml file
1. modify to route GET services to "lambda" folder

<pre class="brush: js;">
service: ng-ssr-aws-lambda
provider:
  name: aws
  runtime: nodejs6.10
  region: ap-southeast-2
  memorySize: 128
plugins:
- serverless-webpack
custom:
  webpack: lambda/webpack.config.js
functions:
  main:
    handler: lambda/index.handler
    events:
    - http:
        path: /
        method: get
    - http:
        path: "{proxy+}"
        method: get
</pre>

## Deploy
<pre class="brush: bash;">
"npm run build:ssr && serverless deploy"
</pre>
If you got the errors of "TS2304: Cannot find name 'AsyncIterator' or TS2304: Cannot find name 'AsyncIterable'", add the following in "tsconfig.json"
<pre class="brush: xml;">
"lib": [
     "es2017",
     "dom",
     "esnext.asynciterable"
   ]
</pre>


---
