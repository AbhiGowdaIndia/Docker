# DOcker Network

* When we install docker, network driver called docker0 (docker zero) is also installed and the type of the network is "Bridge".

* In Docker, a **bridge** network is the default network type

* Types of network:

  * Bridge

  * Host

  * Null or None

  * Overlay

  * Macvlan



#### Communication between containers

* Docker will use the ip series from 172.17.0.1 up to 172.17.0.1/16.

* out these Ip series, 171.17.0.1 is used by gateway and container will get assigned IP from 171.17.0.2

* Containers on the same bridge can communicate via IP.

* User-defined bridges allow container name resolution (DNS).

* To check the IP address of the containers

  ```cmd
  docker inspect bridge
  ```

* We can use Ping or telnet command to check weather we can connect from one container to another container.

* Example:

  * WE have two containers, CO1 with IP 172.17.0.2 and CO2 wit IP 172.17.0.3

  * Get inside the container CO1

    ```cmd
    docker exec -it CO1 bash
    ```

  * Now ping container CO2 using IP

    ```cmd
    ping 172.17.0.2
    ```

* To create network in docker

  **docker network create --driver \<type-of-network> \<network-name>**

  ```cmd
  docker network create --driver bridge my-bridge
  ```

* To create acontainer in the netwrok that we created

  ```cmd
  docker run -it -d --name CO1 -p 8080:80 --network my-bridge nginx
  ```

  * If we do not specify the network explicitly it will create the container in default network **bridge**.

* In default network we can not able to ping a cintainer using its name from another container. We have to use IP address.

* But in custom network, we can ping a container in the same network using its name also.

* Example:

  * Consider both CO1 and CO2 are i network **my-bridge**.
    
  * get inside CO1

    ```cmd
    docker exec -it CO1 bash
    ```

  * Ping Container CO2

    ```cmd
    ping CO2
    ```

* **Host** network is the network of the host machine

* If we want to create a container in **host** network we need to specify it while creating the container.

  ```cmd
  docker run -it -d --name co1 -p 8080:80 --network host nginx
  ```

* Inspect the host network to check container

  ```cmd
  docker inspect host
  ```

* If we ceate container in the host network only there is no need for port mapping

  ```cmd
  docker run -it -d --name co2 nginx
  ```

* To connect container from one network to another network we need to establish a connection between those network

  Example:

  * Consider, containers CO1 and CO2 are in default network **bridge** and containers CO4 and CO5 are in my custom network **my-bridge**.

  * Now, to connect from CO1 to network **my-bridge**

  **docker network connect <network_which_I_want_to_connect> <from_which_container_I_want_to_connect>**

  ```cmd
  docker network connect my-bridge CO1
  ```

  * Now this CO1 can connect with all the containers in network **my-bridge**.

  * get inside container

    ```cmd
    ping CO3
    ping CO4
    ```

* To disconnect from a network

  ```cmd
  docker network disconnect my-bridge CO1
  ```








  

