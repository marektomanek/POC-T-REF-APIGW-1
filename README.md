# Api Gateway Service

This is example of api gateway service created for educational purposes. The gateway provides single point of access to all other services. After initial authentication a JWT token is created based on user rights and roles configured in remote ANAT/LDAP service. The token is returned to the FE (front-end) application and used for all following api calls.

Service also provides additional fuctions:
* **Interactive testing using Swagger/OAS UI**. All provided endpoints are thus **self-documented via the Swagger document** which is accessible using browser from the root (/) url path of the running server
* **Debug log** via browser to better support troubleshooting

Those functions are protected by basic auth using credentials (admin/admin)

## Getting Started with local dev/test system

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

Implementation requires Nodejs and NPM

### Installing

* Clone repository from [Github](https://github.com/JiriHusak-lab/POC-T-REF-APIGW)
* Run npm install (to get all dependencies).
* Run npm start

#### Local Dev environment

Dev and test mode is not using docker. It can be started by  **npm run localDev** command that enables hot-reloading.
Local dev mode comes with following features and limitation: 
* Ability to generate/mock JWT token without dependency on remote ANAT/LDAP service
* Four hardcoded identities mocking real LDAP registry (/mockData)

For local dev modes the system uses .env in the root directory of the project (see bellow environment vars. section)

#### Testing

There test scenarios for **login** and **login/verify** APIs. Testing is based on JEST. 

To run tests:
* Start local server (npm run dev/localDev)
* Run tests **npm run test**

#### IBM Cloud environment
For development and demonstration purposes there is an possibility to also deploy an instance in the IBM Cloud on following url: [API Gateway DEV instance](https://xc4ezcdtcc4z247-gateway-api-poc.eu-de.mybluemix.net/). This instance is updated only on request and there is NO guarantee to be always updated to the latest version.

### Environment Variables
There are numerous bootstrap and configuration variables that must be available to properly run the service:
* VERSION=1.0.0 (Arbitrary value)
* RUNTIME_MODE=localDev (Supported modes are localDev, cloud, container)
* GATEWAY_ADMIN=admin (basic auth credentials to access swagger and web log)
* GATEWAY_PASSWORD=admin
* CLIENT_ID=aclient (Organization unit for LDAP queries)

* APP_ANAT_SHOULD_USE_MOCK=true (true/false for using mock or remote ANAT/LDAP service)
* APP_ANAT_URL='http://localhost:3005/login' (if previous entry is true, than defines remote endpoint for ANAT/LDAP service)
* APP_ANAT_HOST='localhost' (Hostname of ANAT/LDAP service for JWG signing and verification)
* APP_ANAT_TOKEN_EXP='30s' (Time duration/expiration of generated JWT token, only applicable for mock mode)

* APP_JOURNAL_URL='https://wmj-ibm-demo-app.trineckezelezarny-15729-56325c34021cf286d0e188cc291cdca2-0001.us-east.containers.appdomain.cloud/' (remote URL for material journal app)
* APP_JOURNAL_GET_PATH=journal
* APP_JOURNAL_POST_PATH=tbd

* APP_MATERIAL_URL='https://mms-ibm-demo-app.trineckezelezarny-15729-56325c34021cf286d0e188cc291cdca2-0001.us-east.containers.appdomain.cloud/'
* APP_MATERIAL_GET_PATH=Materials/mms
* APP_MATERIAL_POST_PATH=Materials/mms

## OpenShift Deployment
For non local deployment there is a script **npm run docker** which uses ./config/.docker.env file to provide required environment variables

Add additional notes about how to deploy this on a live system...