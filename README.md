# Introduction 

This project is constructed by two docker Containers: an Application Docker Container and a Database Docker Container.
While the user submits ports in any browser, the Flask Router will link to the HTML5 file. In the HTML5 file, there is a canvas which user can use mouse to draw. The Router then saves the handwritten image and requests the prediction from the MNIST Keras model. After the result returns, the Router forwards the result to HTML5 and show it to user via browser and submits all two data to the Cassandra Database Container through the Docker Network Bridge.

The main job for this poject is to recognize the user's handwritten digit. User can draw the digit in the canvas and press `"predict it"` button to start to predict the image, and it will return the result on the right side of the page. User can also press `"clear it"` button to clear their drawing if he is not satisify with it. Each time, the prediction and date will be recorded in the Cassandra database. 

# DEMO
![](https://github.com/brandondeemo/docker_MNIST/blob/master/docker_MNIST/summary/20190826_210026.gif)



# Background

The main job of the project is to take fully use of docker to deploy a user handwriting digit recognition program into container. The goal of the project is to learn the frontier technology such as container Cassandra, Hadoop, Spark.etc and take these technologies into practices.

This project is deployed by Docker and used Canssandra database to record the predictions of user's handwritten digit and datetime. It used two Docker containers, Cassandra database container and our Application container, and they are connected by the Docker Network Bridge, which allows the communication between them.

It used Keras with backend TensorFlow to construct the MNIST model so that it can classify the user's handwritten digit. This MNIST model gets to 99.33% test accuracy after 4 epochs. *User can run mnist_train.py first to test the accuracy.* 

# UI
![](https://github.com/brandondeemo/docker_MNIST/blob/master/docker_MNIST/summary/UI.png)


# Requirements  
- [x] Python (3 or more) 
- [x] Docker
- [x] Cassandra Database

# Preparation 

To run project properly, here are several preparetion that you need to follow. 

__1. construct the Docker Network Bridge so that these to containers can commnuicate with each other.__

```Bash
docker network create mnist_test
```


__2. pull the Cassandra Database image from Docker Hub.__

```Bash 
docker pull Cassandra
```

__3. construct the Docker container for Cassandra image.__

```Bash 
docker run –network mnist_test -p 9042:9042 cassandra:latest 
```
  
__4. build the Application image from the Dockerfile.__

```Bash
docker build -t mnist:latest .
```

__5. run the container .__

```Bash
docker run -p 5000:5000 mnist:latest 
```

__6. submit the Ports (it should be 192.168.99.100:5000) to any web browser.__

__7. call cqlsh shell.__ 

```Bash
docker exec -it mnist_cassandra cqlsh 
```

__8. view the data recorded in Cassandra database.__ 

```Bash
USE mnist_key;
select*from record_data;
```




