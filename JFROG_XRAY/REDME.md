SCANNING DOCKER IMAGES USING X-RAY
-----------------------------------
## PRE-REQUISUITE:
------------------

* We need a Virtual machine (VM) with docker installed in it.
   * [Refer Here](https://get.docker.com/) for docker installation steps.
![preview](IMAGES.png/xray1.png)
* Let us pull a jenkins image
    * The command is `docker image pull jenkins/jenkins` 
![preview](IMAGES.png/xray2.png)

* Now let us integrate docker with jfrog
* Below are the steps to integrate docker with jfrog.

* Login into your jfog cloud account. 
![preview](IMAGES.png/xray3.png)
![preview](IMAGES.png/xray4.png)
![preview](IMAGES.png/xray5.png)
![preview](IMAGES.png/xray6.png)
![preview](IMAGES.png/xray7.png)
![preview](IMAGES.png/xray8.png)

* To setup docker client execute the commands in virtual machine
![preview](IMAGES.png/xray9.png)

`Note` Password will not be visible.
![preview](IMAGES.png/xray10.png)
* Now to push the docker image to the jfrog artifact repository,  follow the below steps:
![preview](IMAGES.png/xray11.png)
![preview](IMAGES.png/xray12.png)
![preview](IMAGES.png/xray13.png)

* Now check the jfog articatory for the docker image.
![preview](IMAGES.png/xray14.png)

* Let us now install `docker desktop` on our local windows system for UI.

![preview](IMAGES.png/xray15.png)

* I have selected windows because I want to scan the image visually.
    * Paste the above command in your windows termainal as an Administrator.
* Let us perform linux scanning after this it totally depends on CLI
![preview](IMAGES.png/MODIFIED.png)
![preview](IMAGES.png/xray17.png)
![preview](IMAGES.png/xray18.png)
![preview](IMAGES.png/xray19.png)
![preview](IMAGES.png/xray20.png)

Click on the package installed and it will take time to get installed
![preview](IMAGES.png/xray21.png)

* Now let us install Jfrog extension
![preview](IMAGES.png/xray22.png)
![preview](IMAGES.png/xray23.png)
![preview](IMAGES.png/xray24.png)

* Here I have clicked on create new environment
![preview](IMAGES.png/xray25.png)
![preview](IMAGES.png/xray26.png)
![preview](IMAGES.png/xray27.png)
![preview](IMAGES.png/xray28.png)

* Now give your Docker-Hub credentials to login
![preview](IMAGES.png/xray29.png)

* If you have successfully configured your credentials you would see your docker hub images.
![preview](IMAGES.png/xray30.png)
![preview](IMAGES.png/xray31.png)
![preview](IMAGES.png/xray32.png)
![preview](IMAGES.png/xray33.png)
![preview](IMAGES.png/xray34.png)
* We can also scan the image by installing jfrog cli on our virtual maxhine.
[Refer Here](https://jfrog.com/xray-scan-cli/) for instructions to install and scan jfrog cli and images..
![preview](IMAGES.png/xray35.png)
![preview](IMAGES.png/xray36.png)
![preview](IMAGES.png/xray37.png)

* To scan images use the following command
  * `jf docker scan jenkins/jenkins`
  
![preview](IMAGES.png/xray38.png)


-----------------------------------------------------------THE-END--------------------------------------------------------------------------------
