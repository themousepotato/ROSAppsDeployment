# Deploying ROS apps using Docker

## What is Docker ?
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

In a way, Docker is a bit like a virtual machine. But unlike a virtual machine, rather than creating a whole virtual operating system, Docker allows applications to use the same Linux kernel as the system that they're running on and only requires applications be shipped with things not already running on the host computer. This gives a significant performance boost and reduces the size of the application.

## Why deploy ROS apps?
Apps developed using the Robotic Operating System can be made repeatable and reproducible. Why don't we make them deployable? Let's go through some hardware drivers which want to use ROS tools. They includes cameras, depth cameras, laser cameras, robots, audio, inertial units, GPS, joysticks. Sounds cool :stuck_out_tongue_winking_eye: !

There are variety of ROS ecosystems also. Have a look at them.

![ROS ecosystems](https://raw.githubusercontent.com/TheMousePotato/ROSAppsDeployment/master/images/ecosyss.png)

Let's try to combine our areas of interest.

![ROS development matrix](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/matrix1.png)

Not finished yet!

![ROS development matrix](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/matrix2.png)

Time has come to re-invent the convensional approach. The cool thing about docker is that the container has no need to account of changes in another container. Let's try to make it simpler to understand. I'm maintaining a container for running apps required for joystick(say). My friend does that for camera. Then, my friend don't need to bother about dependencies for running my joystick apps. In the same way, I don't have to keep track of his development. Now the interesting thing comes to action. We can still communicate. Wow! Let's see it in action.

## How to use Docker to ship ROS apps?
As we have learnt to make things easier so far, let's try to implement it step by step. Links to installation and proxy setup for Docker are given in the end of this documentation. After that, let's start.

### Volumes
By default Docker uses `/root/.ros/` directory to keep track of the logs and debugging info. If you want to change it to your home directory(say 'ubuntu'), perform :

> $ docker run -v "/home/ubuntu/.ros/:/root/.ros/" ros

### Devices
Some application may require device access for acquiring images from connected cameras, control input from human interface device, or GPUS for hardware acceleration. This can be done using the --device run argument to mount the device inside the container, providing processes inside hardware access.

### Networks
Although one process per container is recommended, Docker networks can also be extended to use several running ROS apps. See documentation on [NetworkSetup](http://wiki.ros.org/ROS/NetworkSetup).

### Start by an example
For the communication between our ROS nodes, there should be a virtual network. In this short example, we’ll create a virtual network, spin up a new container running roscore advertised as the master service on the new network, then spawn a message publisher and subscriber process as services on the same network.

## Conclusion
Let's have a look at a funny cartoon on convensional approach on robotics.

![Cartoon making fun of convensional robotics](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/cartoon2.jpg)

Now, see how it has changed.

![Image of deployable robotics](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/simplify.png)

## Some ROS apps shipped using Docker
* [Caffe](https://github.com/ruffsl/ros_caffe/tree/master/docker)
* [Gazemo demos](https://github.com/ruffsl/gazebo_docker_demos)

## Reference and further reading
* [Docker for beginners - tutorial by DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)
* [Configuring network proxy for using Docker](https://stackoverflow.com/questions/23111631/cannot-download-docker-images-behind-a-proxy)
* [Official Docker library for ROS](https://registry.hub.docker.com/_/ros/)
* [Official documentation from Docker on ROS](https://docs.docker.com/samples/library/ros/)
* [Official documentation from ROS on Docker](http://wiki.ros.org/docker)
* [Official website of Open Robotics](https://www.osrfoundation.org/)
* [Docker on ROS slack channel](https://rosorg.slack.com/messages/docker/)
* [FAQ on Docker at ROS website](http://answers.ros.org/questions/scope:all/sort:activity-desc/tags:Docker/page:1/)
