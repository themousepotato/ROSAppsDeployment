# Deploying ROS apps using Docker

## What is Docker ?
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

Unlike a virtual machine, rather than creating a whole virtual operating system, Docker allows applications to use the same Linux kernel as the system that they're running on and only requires applications be shipped with things not already running on the host computer. This gives a significant performance boost and reduces the size of the application.

## Why deploy ROS apps?
Apps developed using the Robotic Operating System can be made repeatable and reproducible. Why don't we make them deployable? Let's go through some hardware drivers which want to use ROS tools. They includes cameras, depth cameras, laser cameras, robots, audio, inertial units, GPS, joysticks. Sounds cool :stuck_out_tongue_winking_eye: !

There are variety of ROS ecosystems also. Have a look at them.

![ROS ecosystems](https://raw.githubusercontent.com/TheMousePotato/ROSAppsDeployment/master/images/ecosyss.png)

Let's try to combine our areas of interest. First have a look at a normal software development environment matrix.

![Software development matrix](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/matrix1.png)

Now, see how the robotics development progress in a matrix:

![Robotics development matrix](https://github.com/TheMousePotato/ROSAppsDeployment/raw/master/images/matrix2.png)

Yuck! Too complex? Time has come to re-invent the convensional approach. The cool thing about docker is that a container has no need to account for the changes in another container. Let's try to make it simpler to understand. I'm maintaining a container for running apps required for joystick(say). My friend does that for camera. Then, my friend don't need to bother about dependencies for running my joystick apps. In the same way, I don't have to keep track of his development. So, I only need to install the libraries for my peripheral and so as my friend. If we are not deploying it using containers, then we have to work on a single space with all the packages installed. The worst thing about this non-deployable robotics is we have to commit each and every update for the libraries unnecessarily. We need to look after the libraries which we have no point of interest. Through containers, things get more simplification. Now the interesting thing comes to action. Our peripherals can still communicate. How? Each container has got an IP address and Docker act as a pathway and provides several network topologies to connect our nodes. Wow! Let's see it in action.

## How to use Docker to ship ROS apps?
As we have learnt to make things easier so far, let's try to implement it step by step. Links to installation and proxy setup for Docker are given in the end of this documentation. After that, let's start.

### Volumes
By default Docker uses `/root/.ros/` directory to keep track of the logs and debugging info. If you want to change it to your home directory(say 'ubuntu'), perform :

> $ docker run -v "/home/ubuntu/.ros/:/root/.ros/" ros

### Devices
Some application may require device access for acquiring images from connected cameras, control input from human interface device, or GPUS for hardware acceleration. This can be done using the --device run argument to mount the device inside the container, providing processes inside hardware access.

### Networks
Although one process per container is recommended, Docker networks can also be extended to use several running ROS apps. See documentation on [NetworkSetup](http://wiki.ros.org/ROS/NetworkSetup).

## Start by an example
For the communication between our ROS nodes, there should be a virtual network. In this short example, we’ll create a virtual network, spin up a new container running roscore advertised as the master service on the new network, then spawn a message publisher and subscriber process as services on the same network.

### Build image

>    Build a ROS image that includes ROS tutorials using this `Dockerfile`:

```
FROM ros:indigo-ros-base
# install ros tutorials packages
RUN apt-get update && apt-get install -y \
    ros-indigo-ros-tutorials \
    ros-indigo-common-tutorials \
    && rm -rf /var/lib/apt/lists/
```

>    Then to build the image from within the same directory:

```
docker build --tag ros:ros-tutorials .
```

### Create network

>    To create a new network foo, we use the network command:

```
docker network create foo
```

>    Now that we have a network, we can create services. Services advertise there location on the network, making it easy to resolve the location/address of the service specific container. We’ll use this make sure our ROS nodes can find and connect to our ROS master.

### Run services

>    To create a container for the ROS master and advertise it’s service:

```
 docker run -it --rm \
    --net foo \
    --name master \
    ros:ros-tutorials \
    roscore
```

 >   Now you can see that master is running and is ready manage our other ROS nodes. To add our talker node, we’ll need to point the relevant environment variable to the master service:

```
  docker run -it --rm \
    --net foo \
    --name talker \
    --env ROS_HOSTNAME=talker \
    --env ROS_MASTER_URI=http://master:11311 \
    ros:ros-tutorials \
    rosrun roscpp_tutorials talker
```

>    Then in another terminal, run the listener node similarly:

```
  docker run -it --rm \
    --net foo \
    --name listener \
    --env ROS_HOSTNAME=listener \
    --env ROS_MASTER_URI=http://master:11311 \
    ros:ros-tutorials \
    rosrun roscpp_tutorials listener
```

>    Alright! You should see listener is now echoing each message the talker broadcasting. You can then list the containers and see something like this:

```
  docker service ls
SERVICE ID          NAME                NETWORK             CONTAINER
67ce73355e67        listener            foo                 a62019123321
917ee622d295        master              foo                 f6ab9155fdbe
7f5a4748fb8d        talker              foo                 e0da2ee7570a
```

>    And for the services:

```
 docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED              STATUS              PORTS               NAMES
a62019123321        ros:ros-tutorials   "/ros_entrypoint.sh    About a minute ago   Up About a minute   11311/tcp           listener
e0da2ee7570a        ros:ros-tutorials   "/ros_entrypoint.sh    About a minute ago   Up About a minute   11311/tcp           talker
f6ab9155fdbe        ros:ros-tutorials   "/ros_entrypoint.sh    About a minute ago   Up About a minute   11311/tcp           master
```

### Introspection

 >   Ok, now that we see the two nodes are communicating, let get inside one of the containers and do some introspection what exactly the topics are:

```
  docker exec -it master bash
  source /ros_entrypoint.sh
```

>    If we then use rostopic to list published message topics, we should see something like this:

```
  rostopic list
/chatter
/rosout
/rosout_agg
```

### Tear down

>    To tear down the structure we’ve made, we just need to stop the containers and the services. We can stop and remove the containers using Ctrl^C where we launched the containers or using the stop command with the names we gave them:

```
  docker stop master talker listener
  docker rm master talker listener
```

### Compose

> Now that you have an appreciation for bootstrapping a distributed ROS example manually, lets try and automate it using docker-compose.

>    Start by making a folder named rostutorials and moving the Dockerfile we used earlier inside this directory. Then create a yaml file named docker-compose.yml in the same directory and paste the following inside:

```
version: '2'
services:
  master:
    build: .
    container_name: master
    command:
      - roscore
  
  talker:
    build: .
    container_name: talker
    environment:
      - "ROS_HOSTNAME=talker"
      - "ROS_MASTER_URI=http://master:11311"
    command: rosrun roscpp_tutorials talker
  
  listener:
    build: .
    container_name: listener
    environment:
      - "ROS_HOSTNAME=listener"
      - "ROS_MASTER_URI=http://master:11311"
    command: rosrun roscpp_tutorials listener
```
>    Now from inside the same folder, use docker-copose to launch our ROS nodes and specify that they coexist on their own network:

```
  docker-compose up -d
```

>    Notice that a new network named rostutorials_default has now been created, you can inspect it further with:

```
  docker network inspect rostutorials_default
```

>    We can monitor the logged output of each service, such as the listener node like so:

```
  docker-compose logs listener
```

>    Finally, we can stop and remove all the relevant containers using docker-copose from the same directory:

```
  docker-compose stop
  docker-compose rm
```

>    Note: the auto-generated network, rostutorials_default, will persist over the life of the docker engine or until you explicitly remove it using docker network rm.


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
* [Docker for beginners - workshop material](https://docker-curriculum.com/)
* [Docker cheatsheet](https://github.com/wsargent/docker-cheat-sheet)
* [Awesome Docker resources](https://github.com/veggiemonk/awesome-docker)
* [Configuring network proxy for using Docker](https://stackoverflow.com/questions/23111631/cannot-download-docker-images-behind-a-proxy)
* [Official Docker library for ROS](https://registry.hub.docker.com/_/ros/)
* [Official documentation from Docker on ROS](https://docs.docker.com/samples/library/ros/)
* [Official documentation from ROS on Docker](http://wiki.ros.org/docker)
* [Official website of Open Robotics](https://www.osrfoundation.org/)
* [Docker on ROS slack channel](https://rosorg.slack.com/messages/docker/)
* [FAQ on Docker at ROS website](http://answers.ros.org/questions/scope:all/sort:activity-desc/tags:Docker/page:1/)

## Contributions
Hey! If you've found something missing in this article, do suggest changes by sending PRs.
