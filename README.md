# Deploying ROS apps using Docker

## Why deploy ROS apps?
<img width="200" height="200" src="img.picturequotes.com/2/579/578697/lack-of-communication-quote-2-picture-quote-1.jpg" style="float:left;">
Apps developed using the Robotic Operating System can be made repeatable and reproducible. Why don't we make them deployable? Let's go through some hardware drivers which want to use ROS tools. They includes cameras, depth cameras, laser cameras, robots, audio, inertial units, GPS, joysticks. Sounds cool ;) !

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
* [Configuring network proxy for using Docker] (https://stackoverflow.com/questions/23111631/cannot-download-docker-images-behind-a-proxy)
* [Official Docker library for ROS](https://registry.hub.docker.com/_/ros/)
* [Official documentation from Docker on ROS](https://docs.docker.com/samples/library/ros/)
* [Official documentation from ROS on Docker](http://wiki.ros.org/docker)
* [Official website of Open Robotics](https://www.osrfoundation.org/)
* [Docker on ROS slack channel](https://rosorg.slack.com/messages/docker/)
* [FAQ on Docker at ROS website](http://answers.ros.org/questions/scope:all/sort:activity-desc/tags:Docker/page:1/)
