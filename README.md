# Serverless - AWS Node.js Typescript

This project has been generated using the `aws-nodejs-typescript` template from the [Serverless framework](https://www.serverless.com/).

For detailed instructions, please refer to the [documentation](https://www.serverless.com/framework/docs/providers/aws/).

## Description
- In the path [/product-service] project it contains a nodejs application with typescript related to the product crud service using aws lambda function as code.
- In the path [/notification-service] project it contains a nodejs application with typescript related to notification service using aws lambda function as code.
 

## Installation/deployment instructions

Depending on your preferred package manager, follow the instructions below to deploy your project.

> **Requirements**: NodeJS `lts/fermium (v.14.15.0)`. If you're using [nvm](https://github.com/nvm-sh/nvm), run `nvm use` to ensure you're using the same Node version in local and in your lambda's runtime.

### Using NPM

- Run `npm i` to install the project dependencies
- Run `npx sls deploy` to deploy this stack to AWS

### Using Yarn

- Run `yarn` to install the project dependencies
- Run `yarn sls deploy` to deploy this stack to AWS

## Test your service

This template contains a single lambda function triggered by an HTTP request made on the provisioned API Gateway REST API `/hello` route with `POST` method. The request body must be provided as `application/json`. The body structure is tested by API Gateway against `src/functions/hello/schema.ts` JSON-Schema definition: it must contain the `name` property.

- requesting any other path than `/hello` with any other method than `POST` will result in API Gateway returning a `403` HTTP error code
- sending a `POST` request to `/hello` with a payload **not** containing a string property named `name` will result in API Gateway returning a `400` HTTP error code
- sending a `POST` request to `/hello` with a payload containing a string property named `name` will result in API Gateway returning a `200` HTTP status code with a message saluting the provided name and the detailed event processed by the lambda

> :warning: As is, this template, once deployed, opens a **public** endpoint within your AWS account resources. Anybody with the URL can actively execute the API Gateway endpoint and the corresponding lambda. You should protect this endpoint with the authentication method of your choice.

### Locally

In order to test the hello function locally, run the following command:

- `npx sls invoke local -f hello --path src/functions/hello/mock.json` if you're using NPM
- `yarn sls invoke local -f hello --path src/functions/hello/mock.json` if you're using Yarn

Check the [sls invoke local command documentation](https://www.serverless.com/framework/docs/providers/aws/cli-reference/invoke-local/) for more information.

### Remotely

Copy and replace your `url` - found in Serverless `deploy` command output - and `name` parameter in the following `curl` command in your terminal or in Postman to test your newly deployed application.

```
curl --location --request POST 'https://myApiEndpoint/dev/hello' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "Vinicius"
}'
```

## Template features

### Project structure

The project code base is mainly located within the `src` folder. This folder is divided in:

- `functions` - containing code base and configuration for your lambda functions
- `libs` - containing shared code base between your lambdas

```

notification-service ├── src
                │    ├── application
                │    │   ├── dtos
                │    │   │   ├── email-notification.dto.ts
                │    │   │   ├── purchase-approved.dto.ts
                │    │   │   
                │    │   │   
                │    │   └── usecases
                │    │         ├── purchase-approved.usecase.ts
                │    │           
                │    │          
                │    │          
                │    │          
                │    │           
                │    ├── domain
                │    │   ├── service
                │    │   │         ├── http
                │    │   │         │     └── http.service.contract.ts
                │    │   │         ├── notification
                │    │   │                   └──email-notification.service.contract.ts
                │    │   │   
                │    │   └── usecases
                │    │       └── email
                │    │           └── purchase-approved.usecase.contract.ts
                │    ├── infra
                │    │   ├── lambdas
                │    │   │   ├── send-email-notification.ts
                │    │   │   │
                │    │   │   │
                │    │   │   │
                │    │   │   │
                │    │   │   ├── services 
                │    │   │   │      ├── http
                │    │   │   │      │     └── axios.service.ts
                │    │   │   │      ├── notification
                │    │   │   │                └──email-notification.service.ts                 
                ├── package.json
                ├── serverless.yml              # Serverless service file
                ├── tsconfig.json               # Typescript compiler configuration
                ├── tsconfig.paths.json         # Typescript paths
                └── webpack.config.js           # Webpack configuration
                └── env.example

product-service ├── src
                │   ├── application
                │   │   ├── dtos
                │   │   │   ├── create-product.dto.ts
                │   │   │   ├── get-product.dto.ts
                │   │   │   ├── list-product.dto.ts
                │   │   │   └── update-product.dto.ts
                │   │   └── usecases
                │   │       └── product
                │   │           ├── create-product.usecase.ts
                │   │           ├── delete-product.usecase.ts
                │   │           ├── get-product.usecase.ts
                │   │           ├── list-product.usecase.ts
                │   │           └── update-product.usecase.ts
                │   ├── domain
                │   │   ├── entities
                │   │   │   └── Product.ts
                │   │   └── repositories
                │   │       └── product
                │   │           └── product.repository.contract.ts
                │   ├── infra
                │   │   ├── database
                │   │   │   └── dynamodb
                │   │   │       └── product.dynamo.repository.ts
                │   │   └── facade
                │   │   │       └── product-repository.facade.ts
                │   │   └── lambdas
                │   │             └── create-product.ts
                │   │             └── get-product.ts
                │   │             └── list-product.ts
                │   │             └── update-product.ts
                │   │     
                │   │ 
                │   │     
                │   │     
                │   │     
                ├── package.json
                ├── serverless.yml              # Serverless service file
                ├── tsconfig.json               # Typescript compiler configuration
                ├── tsconfig.paths.json         # Typescript paths
                └── webpack.config.js           # Webpack configuration

```

### 3rd party libraries

- [json-schema-to-ts](https://github.com/ThomasAribart/json-schema-to-ts) - uses JSON-Schema definitions used by API Gateway for HTTP request validation to statically generate TypeScript types in your lambda's handler code base
- [middy](https://github.com/middyjs/middy) - middleware engine for Node.Js lambda. This template uses [http-json-body-parser](https://github.com/middyjs/middy/tree/master/packages/http-json-body-parser) to convert API Gateway `event.body` property, originally passed as a stringified JSON, to its corresponding parsed object
- [@serverless/typescript](https://github.com/serverless/typescript) - provides up-to-date TypeScript definitions for your `serverless.ts` service file

### Advanced usage

Any tsconfig.json can be used, but if you do, set the environment variable `TS_NODE_CONFIG` for building the application, eg `TS_NODE_CONFIG=./tsconfig.app.json npx serverless webpack`

### Deploy AWS NPM Serverless lambdas functions
 - Inside the serverless.yml file you need to enter your aws account id, your region in iam.role.Resource
 - You must have the AWS CLI installed on your machine and when finished, log in with your credentials to the AWS console by entering the command: `aws configure`
 - Within the product-service project insert the command :
    - `serverless deploy --stage dev`  or
    - `serverless deploy --stage dev --aws-profile your_profile_aws_account`
