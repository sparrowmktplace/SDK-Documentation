---
layout: default
title: Sparrow Cli
nav_order: 3
---

# **Sparrow CLI**


The Sparrow command-line interface tool that helps you quickly build, validate & test Sparrow apps. You can also use it to automate many common development tasks
To view the list of CLI commands, after you install the CLI, at the command prompt type ssdk and press Enter.

## **Commands**

The Sparrow command-line interface tool that helps you quickly build, validate & test Sparrow apps. You can also use it to automate many common development tasks

### Here are the list of commands which can be used in the command line interface

#### 1. [Create](#create)

#### 2. [Run](#run)

#### 3. [Validate](#validate)
#### 4. [Pack](#develop-an-app)

#### 5. [version](#develop-an-app)

## **Create:**

- The command creates an app in the specified directory, based on the default app template. If no directory is specified, the app is created in the current directory.

- `ssdk create [--app-dir DIR] [--products PRODUCT] [--template TEMPLATE]`

| OPTIONS | USAGE |
| ------------- |:-------------:|
| --app-dir | To specify the absolute or relative path of the project directory where the app is to be created. If the option is not used with the create command, the app is created in the current directory.|
|--products | To specify the product for which the app is created. If the option is not used with the create command, you are prompted to select a product. Select surveysparrow.|
|--template| To specify the template for which the app is created. If the option is not used with the create command, you are prompted to select a template. Select the your_first_app template to create a front-end app.|

## **Run:**

| OPTIONS | SHORT-HAND NOTATION | USAGE | DESCRIPTION |
| ------------- |:-------------:| :-------------:| -------------:|
|--app-dir| -d |General syntax: `$ssdk run -d <DIR>`<br/> `<DIR>` is the directory path to the folder containing the app that is tested locally through the ssdk run command. Example `$ssdk run -d /user/myFirstApp` |Specifies the directory path to the folder that contains the app that is tested locally.|
| –-skip-validation | -V | General syntax:`$ssdk run -V <Validation-type>`<br/> `“$ssdk run --skip-validation <Validation-type>`<br/> `<Validation-type>` is the type of validation check that is skipped.Example `$ssdk run -V lint`| Skips specific validation checks. The errors associated with the validation, if any, are not displayed when the app is tested locally.|


## **Validate:**

This command validates whether the app code is error-free. If there are errors in the code, corresponding violations are displayed after the command is run.

```bash
ssdk validate [--app-dir DIR] [--fix]
```

## **Pack:**

## **Version:**




