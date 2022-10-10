# HT3 - Dropbox API Testing With Postman/Newman and Jenkins integration 
## Purpose
This project implements automated tests for the following Dropbox API scenarios:
- Upload File
- Get File Meta Data
- Delete File

### Tools
- Newman v5 by Postman Labs: https://github.com/postmanlabs/newman
- Newman htmlextra reporter: https://github.com/DannyDainton/newman-reporter-htmlextra
- Jenkins automation server

## Installation
To install, clone the repository and install all the dependencies: `npm i`

## Setup
You would need:
- Test Dropbox account
- Dropbox app credentials and authorization code
- Configure project

### Test Dropbox Account
If you don't have a dropbox account to use for testing, create one by the following link: https://www.dropbox.com/register

### Obtain Dropbox OAuth2.0 Credentials 
To configure an Oauth2.0 authorization for our project to your test account you need to request Dropbox to create an _app_. To do that:
- Go to the [Dropbox App Console](https://www.dropbox.com/developers/apps) and log in to your test account
- Select `Create App` and provide parameters:
	- Scoped access = New
	- Choose the type of access you need = Full Dropbox 
  - Name your app = you can use any unique name here (Dropbox requires that app names should be unique globally)
- Write down the following credentials:
  - App key
  - App Secret
- Now you need to get a single-use authorization code for your app to access your test account. To get it, construct and open in browser the following link: 
`https://www.dropbox.com/oauth2/authorize?client_id=%App Key Here%&response_type=code&token_access_type=offline`
Confirm access to your app and copy provided Authorization Code

### Configure Project
Open `globals.js` config file and put credentials obtained on the previous step (App Key, App Secret and Auth Code) there:
```
    {
      "type": "any",
      "value": "%App Key here%",
      "key": "appKey"
    },
    {
      "type": "any",
      "value": "%App Secret here%",
      "key": "appSecret"
    },
    {
      "type": "any",
      "value": "%Authotization Code here%",
      "key": "appAuthCode"
    }
```

## Usage - Running The Test Suite
You can initate a test run from the command line by typing
```
npm run test
```
## Reports
By default this project uses 2 reporters: 
- Newman CLI reporter that provides test run results to console
- Newman HTML reporter that generates html report for each run and saves it to the `newman` directory

## Jenkins integration
All changes to this repository are automatically verified via the Jenkins build:
http://139.144.79.230:8080/blue/pipelines/
There are 2 pipelines:
- API Testing - standard Jenkins build job. Configuration is available in jenkins/api_testing.xml
- HT3_Dropbox_API_Testing - pipeline that is defined in Jenkins file in this repository. Configuration is in jenkins/Jenkinsfile (automatically synced with Jenkins server)


## Implemented Tests
This project implements automated tests for the following Dropbox API scenarios:
- Upload File
- Get File Meta Data
- Delete File

### 1. Upload File
API documentation: https://www.dropbox.com/developers/documentation/http/documentation#files-upload

These tests cover a simple file upload scenario.
- Positive cases:
	- TestFile.txt is correctly uploaded to Dropbox using the unique filename (to avoid possible conflicts)
- Negative cases:
	- Request is not authentified
  - Wrong request type (GET isntead of POST)
  - Request has no arguments
  - Request has no mandatory path argument

### 2. Get File Meta Data
API documentation: https://www.dropbox.com/developers/documentation/http/documentation#files-get_metadata

These tests cover a scenario to get file information (meta data):
- Positive cases:
	- Meta data for the recently uploaded file (by the previous test) is correctly obtained and matches expected values
- Negative cases:
	- Request is not authentified
  - Wrong request type (GET isntead of POST)
  - Request has no arguments
  - Request has no mandatory path argument
  - Requested file does not exist

### 3. Delete File
API documentation: https://www.dropbox.com/developers/documentation/http/documentation#files-delete

These tests cover a simple file deletion scenario:
- Positive cases:
	- Recently uploaded file is correctly deleted
- Negative cases:
	- File does not exist
