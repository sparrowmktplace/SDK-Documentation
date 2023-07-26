---
layout: default
title: Quick Start
parent: Getting Started
nav_order: 1
---

# **Quick Start**

### You can build Sparrow apps that will automate most of your development tasks by using the following steps.

#### 1. [Initial Requirements](#inital-requirements)
#### 2. [Install Sparrow CLI](#install-the-sparrow-cli)
#### 3. [Develop an App](#develop-an-app)
#### 4. [Test the App](#test-a-front-end-app)
#### 5. [Validate and Pack](#validate-and-pack)

---

## **Inital requirements**

### **Install NVM**:

&emsp;&emsp; NVM (Node Version Manager) is a tool that allows you to easily manage multiple versions of Node.js on your system. Here are the steps to install NVM
 
 - On a Mac or Linux, follow the installation instructions provided on the NVM GitHub page.

- On Windows, download the nvm-setup.zip file from the NVM release channel, extract the files, and run the installer.

- To verify that NVM is installed correctly, open a terminal window and run the following commands

```bash 
nvm --version
```

### **Install Node:**

- To install Node.js 18 via nvm, follow the given steps.

```bash 
nvm install 18
```
- To use Node.js 18

```bash
nvm use 18
```

- To verify that the node is installed correctly, run the following commands

```bash 
node –-version
```

## **Install the sparrow CLI**

- To install the latest CLI version, run the following command.

```bash
npm install https://cdn.sparrow.io/ssdk/latest.tgz -g
```
- Run the following command to verify the CLI installation.

 ```bash
 ssdk version
 ```
 ![image-1](../../assets/image1.png)
 
## **Develop an App:**

&emsp; You can use the following steps to create an app that displays a sample text in the ‘app & integration’ section of setting page.

- From the command line, navigate to the empty directory in which you want to create an app.

- Run the following command:
```bash
 ssdk create
```
- Select your_first_app. A new app is created based on the your_first_app template.

 ![image-1](../../assets/image2.png)

 **The following directories and files are created as a result of the ssdk create command.**

| DIRECTORY/FILE | DESCRIPTION |
| ------------- |:-------------:|
| app/*      | Contains all the files required for the front-end component of an app. The JS file follows the ES5 standard.|
| app/index.html     | Contains files to render front-end components of an app. This is the first page that is loaded when the app is activated. When building an app, if the app uses Data methods, Request method, Installation Parameters, or Data Storage, update the index.html file with the following reference to client.js: `<script src=”https://sparrow.cloudfront.net/client.js"></script>`     |
| app/scripts | Contains all the javascript files required to support the front-end functionality of an app.  |
| app/scripts/app.js |  Contains the app logic to display. |
| app/styles|  Contains the styles required for the front-end components of an app. |
|app/styles/styles.css | Contains CSS rules that are incorporated to HTML files, when referenced.|
| app/styles/images | Contains images that can be used in the app. |
| app/styles/images/icon.svg | Contains the app icon. The icon file should be of SVG type with a resolution of 64 x 64 pixels. |
| config/* | Contains the installation parameters and oauth configuration files. |
| config/iparams.json* | Contains all the installation parameters whose values are set when the app is installed. For more information, see Installation Parameters. |
| manifest.json* | Contains details such as the platform version the app uses, product to which the app belongs, event listeners for serverless apps, SMI functions that can be invoked from the app’s front end component.|
| README.md | Contains additional instructions, information, and code-related specifications pertaining to the app.|


**Note : When building an app, do not modify these file/folder names.**

## **Test a front-end app**

- From the command line, navigate to the directory that contains the app related files and run the following command.

```bash
ssdk run 
```
- Upon running ssdk run command you will be prompted to install a certificate on your browser for establishing https connection between your loacal app and your surveysparrow account.

![image-5](../../assets/image5.png)
- After you agree to install ssl certificate on your browser, the app will start running on https.

![image-6](../../assets/image6.png)
- Log in to your SurveySparrow account.

- To the SurveySparrow account URL, append `?dev=true`

&emsp;&emsp;&emsp;Example URL:
&emsp;`https://subdomain.surveysparrow.com/?dev=true`

- To allow the Chrome browser to connect to the test server that runs on HTTP,

 &emsp;&emsp;&emsp; - Navigate to Settings -> Advanced -> Privacy and security -> Site settings -> Insecure content.

 &emsp;&emsp;&emsp; - In the Allow section, click Add and enter the account URL.
Example URL:` https://subdomain.surveysparrow.com`

- To test **your_first_app**, from the homepage > Settings, select “apps & integrations”. You can see your app **“your_first_app”** displayed in app listing section

## **Validate and Pack:**

To check if the app is error-free and package it for submission, follow the given steps.

- To validate the code, run the following command.
```bash
ssdk validate [--app-dir DIR]
```
Here, DIR is the relative or absolute path to the project directory. If there are errors in the code, corresponding violations are displayed.

- To pack the app for submission, run the following command.

&emsp;&emsp;&emsp;&emsp;`ssdk pack [--app-dir DIR]`= The command generates the dist/my_first_app.zip file.

- To publish the app to the Sparrow Marketplace, navigate to the Sparrow developer portal and upload the packed file.



