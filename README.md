# fortran-task
A simple Hello World example using Python on Rising Cloud. You can find all the instructions to build this example at: https://risingcloud.com/docs/fortran-task
This guide will walk you through the steps needed to build and run a Fortrain Hello World! app on Rising Cloud using an Ubuntu 20.04 base image.  Note that if you want to build this on Ubuntu 22.04 some of the dependencies will be different.

# 1. Install the Rising Cloud Command Line Interface (CLI)
In order to run the Rising Cloud commands in this guide, you will need to [install](https://risingcloud.com/docs/install) the Rising Cloud Command Line Interface. This program provides you with the utilities to setup your Rising Cloud Task or Web Service, upload your application to Rising Cloud, check the status of your app, and more.

# 2. Login to Rising Cloud using the CLI
Using a command line console (called terminal on Mac OS X and command prompt on Windows) run the Rising Cloud login command. The interface will request your Rising Cloud email address and password.

```risingcloud login```

# 3. Build your Application
In your project directory, create the following file using your text editor

```helloworld.f```

Edit the **helloworld.f** file and type the following into it, and save:

```
program hello
print *,"Hello World!"
end program hello
```

This app will write Hello World! to the console each time it’s run.

# 4. Initialize your Rising Cloud Task
On your console, navigate to your your project directory, and initialize a new task using the following command and replacing **$TASK** with your unique task name.  Use the **-s** flag to tell Rising Cloud this will be a serverless task.

```risingcloud init -s $TASK```

**NOTE** Your unique task name must be at least 12 characters long and consist of only alphanumeric characters and hyphens (-). This task name is unique to all tasks on Rising Cloud. A unique URL will be provided to you for sending jobs to your task including your task name.  If a task name is not available, the CLI will return with an error so you can try again.

This creates a **risingcloud.yaml** file in your project directory. This file will be used to configure the build instructions for your application.

# 5. Configure Your Build Parameters
In the **risingcloud.yaml**, change the deps line to this:

```
deps:
  - apt-get update
  - apt-get install -y gfortran
  - gfortran -ffree-form helloworld.f
```

Change your run line to this:

```run: ./a.out```

# 6. Build and Deploy Your Rising Cloud Task
Use the **push** command to push your updated risingcloud.yaml to your Task on Rising Cloud.

```risingcloud push```

Use the **build** command to zip, upload, and build your app on Rising Cloud.

```risingcloud build```

Use the **deploy** command to deploy your app as soon as the build is complete.  Change **$TASK** to your unique task name.

```risingcloud deploy $TASK```

Alternatively, you could also use a combination to push, build and deploy all at once.

```risingcloud build -r -d```

Rising Cloud will now build out the infrastructure necessary to run and scale your application including networking, load balancing and DNS.  Allow DNS a few minutes to propogate and then your app will be ready and available to use!


# 7. Queue Jobs for your Rising Cloud Task

**Send jobs to your new app in the Portal**

- Log into your Rising Cloud Team: <u>[https://my.risingcloud.com/](https://my.risingcloud.com/)</u>
- Select your task from the Dashboard.
- Click on Jobs in the left menu.
- Click New Job Request.  
- Send the following request to your task, leave the curly brackets.

```{ }```

Rising Cloud may will take a few moments to spin-up your and proces your request.  In the future it will learn from the apps usage patterns to anticipate usage, making instances available in advance of need, while also spinning down instances when not needed.  

Click the twisty to open up your Job, and then click Console to see your output

Congratulations! You’ve successfully created and used a Rising Cloud Task!
