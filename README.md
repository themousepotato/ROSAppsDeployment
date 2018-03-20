# Deploying ROS apps using Docker

## What is Docker ?
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

In a way, Docker is a bit like a virtual machine. But unlike a virtual machine, rather than creating a whole virtual operating system, Docker allows applications to use the same Linux kernel as the system that they're running on and only requires applications be shipped with things not already running on the host computer. This gives a significant performance boost and reduces the size of the application.

## Why deploy ROS apps?
Apps developed using the Robotic Operating System can be made repeatable and reproducible. Why don't we make them deployable? Let's go through some hardware drivers which want to use ROS tools. They includes cameras, depth cameras, laser cameras, robots, audio, inertial units, GPS, joysticks. Sounds cool :stuck_out_tongue_winking_eye: !

There are variety of ROS ecosystem also. Have a look at them.

![ROS ecosystems](https://raw.githubusercontent.com/TheMousePotato/ROSAppsDeployment/master/images/ecosyss.png)

Let's try to combine our areas of interest.

![ROS development matrix](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/matrix1.png)

Not finished yet!

![ROS development matrix](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/matrix2.png)

Time has come to re-invent the convensional approach.

![Cartoon making fun of convensional robotics](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/cartoon2.jpg)

See how beautiful it is!

![Image of deployable robotics](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/simplify.png)


## Some ROS apps shipped using Docker
* [Caffe](https://github.com/ruffsl/ros_caffe/tree/master/docker)
* [Gazemo demos](https://github.com/ruffsl/gazebo_docker_demos)

## Reference
* [Docker for beginners - tutorial by DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)
* [Configuring network proxy for using Docker](https://stackoverflow.com/questions/23111631/cannot-download-docker-images-behind-a-proxy)
* [Official Docker library for ROS](https://registry.hub.docker.com/_/ros/)
* [Official documentation from Docker on ROS](https://docs.docker.com/samples/library/ros/)
* [Official documentation from ROS on Docker](http://wiki.ros.org/docker)
* [Official website of Open Robotics](https://www.osrfoundation.org/)
* [Docker on ROS slack channel](https://rosorg.slack.com/messages/docker/)
* [FAQ on Docker at ROS website](http://answers.ros.org/questions/scope:all/sort:activity-desc/tags:Docker/page:1/)
