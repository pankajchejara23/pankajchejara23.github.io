---
title: "Setting up MQTT server and Client"
author: "Pankaj Chejara"
date: "2022-01-13"
categories: [python, mqtt]
image: "./images/mqtt/p5.1.png"
code-block-background: true
highlight-style: github
toc: true
---



 This post offers an introduction to the <strong>MQTT</strong> (Message Queuing Telemetry Transport) protocol [1] and also demonstrates its usage with an example in Python (Just for info: telemetry means the collection of measurement data from a remote location and its transmission [wiki-link](https://en.wikipedia.org/wiki/Telemetry). 


<!-- wp:heading {""level"":3} -->
## What is MQTT?
<!-- /wp:heading -->

  
  MQTT  is a communication protocol which is actually developed for low-energy devices to transmit data with the low-bandwidth network.<br>
 It is a <strong>lightweight</strong>  protocol built on the top of TCP/IP. 
  

:::{.callout-tip}

Lightweight:</strong> In software/programs, lightweight specify the characteristic of low memory and CPU usage.

:::

  
 MQTT allows simple and efficient data transmission from sensors to IoT (Internet of Things) networks. Additionally, it also enables the easy integration of new sensors to the IoT network. 
  

<!-- wp:heading {""level"":5} -->
### MQTT Terminology
<!-- /wp:heading -->

  
 In MQTT, there are five main terminology 
  

<!-- wp:list -->
<ul><li><strong>Broker:</strong> It is the middleware between data sender (publisher) and data receiver (subscriber). You can consider it as a server which receives data from sender and forward it to the receiver.</li><li><strong>Publisher:</strong> In simple terms, you can think of a device which needs to send data to other parties. This device could be a sensor, laptop, and Raspberry pi board.</li><li><strong>Subscriber:</strong> It is the receiver of data sent by the publisher.</li><li><strong>Topic:</strong> Publisher and Subscriber do not know each other directly. When the publisher sends some data to Broker, it associates that data with a  topic . The topic is a string in the format of hierarchy e.g.  sensor/room-1/temprature  (it is just an example) where <strong>'/'</strong> is level separator and each string represents a level. You can define it as per your requirement. When the subscriber connects to the Broker, it specifies the topic in which it is interested in. Then, broker forward published messages to their corresponding subscribers on the basis of the topic. 
Here, you need to understand <strong>two operators</strong>  +  and  #. Before jumping to these operators, let's assume a scenario where we have four temperature sensors in four rooms. These sensors are sending their data with the following topics</li></ul>
<!-- /wp:list -->

<!-- wp:list {""ordered"":true} -->
<ol><li>home/bedroom/temperature</li><li>home/kitchen/temperature</li><li>home/dining-room/temperature</li><li>home/study-room/temperature


Now, in order to receive data from all four rooms, subscribers need to subscribe to the above topics. One way of doing that is to subscribe to each topic separately. Another way is to use  `+`  operator and simply subscribe to the topic  `home\+\temprature` .  +  operator here matches any single level ( +  is known as a single-level wildcard). Therefore,  home\+\temprature  will match any topic with three-level hierarchy starting with  home\  and ending with  \temprature .<br>
Coming to second operator  `#`  which matches multiple levels in the topic. For instance,  `home\#`  will match all the aforementioned four topics.

<strong>Some examples</strong><br>
Let's say we have multiple sensors which  are transmitting data with following topics</li></ol>
<!-- /wp:list -->

<!-- wp:list -->
* `school\class-1\group-1\audio`
* `school\class-1\group-2\audio`
* `school\class-2\group-1\audio`
* `school\class-2\group-2\audio`


:::{.callout-tip}

**Which topic to subscribe to receive all data from class-1 ?**
 `school\class-1\#`

**Which topic to subscribe to receive data from group-1?**
 `school\+\group-1\audio`

**Which topic to subscribe to receive data from entire school?**
 `school\#` 

:::

:::{.callout-error}

 `#`  operator can appear only once in a topic expression e.g.  school\#\group-1\#  is invalid.  A topic expression  #  matches every topic hence subscriber to this topic will receive every message. </blockquote>

:::
<ul><li><strong>Message:</strong> It is simply the data which needs to be sent.</li></ul>
<!-- /wp:list -->

<!-- wp:heading {""level"":3} -->
### Demonstration of MQTT in Python
<!-- /wp:heading -->

  
 Now, we will see a python example of using MQTT protocol for transmitting data. We will use a <strong>python package</strong> [paho-mqtt](https://pypi.org/project/paho-mqtt/ "" target = ""_blank) for creating publisher and subcriber. You can install it using following command 
  

```bash
 pip install paho-mqtt 

 ```
 Next, we need to <strong>set up a Broker</strong>. There are numerous option of this. You can either use a cloud-based broker or you can install a broker on a server in your network. There are multiple cloud services are available with can be used ([Complete list of broker servers](https://github.com/mqtt/mqtt.github.io/wiki/servers "" target = ""_blank)). I would like to mention [MaqiaTTo](https://www.maqiatto.com "" target = ""_blank) which is a free cloud-based MQTT broker. It can be used for testing purpose. In this tutorial, however, we are going to set-up a broker in the network. We will use [Mosquitto](https://mosquitto.org/ "" target = ""_blank) broker server. 
  

<!-- wp:heading {""level"":5} -->
#### Setting up Mosquitto Broker
<!-- /wp:heading -->

  
[Mosquitto](https://mosquitto.org/) is an open-source MQTT message broker. Following are the instructions for installing it on your systems. 
  

<!-- wp:list -->
<ul><li><strong>Windows users:</strong> Download and install [64bit-version](https://mosquitto.org/files/binary/win64/mosquitto-1.6.4-install-windows-x64.exe), [32bit-version](https://mosquitto.org/files/binary/win32/mosquitto-1.6.2-install-windows-x86.exe)(If you are not sure then install 32bit-version)</li><li><strong>Ubuntu Users:</strong> Run following commands

```bash 
sudo apt-add-repository ppa:mosquitto- dev/mosquitto-ppa
sudo apt-get update 
```
</li></ul>
<!-- wp:list -->
<ul><li><strong>Mac Users:</strong> First install brew package manager using following command
```bash 
/usr/bin/ruby \
 -e ""$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"" 
 ```


 Next, install Mosquitto 

``` bash
brew install mosquitto 
```

 </li></ul>


#### Starting Mosquitto Server

  
 Now, we are going to start our MQTT broker.<br>
 Run the following command [Mac users]
  
```bash
/usr/local/sbin/mosquitto -c /usr/local/etc/mosquitto/mosquitto.conf 
```
<!-- /wp:code -->

>  On ubuntu following command can be used to start or stop Mosquitto
    ```bash 
        sudo systemctl (start|stop) mosquitto
    ```
<!-- /wp:quote -->

<!-- wp:heading {""level"":5} -->
####  Writing Publisher and Subscriber codes
<!-- /wp:heading -->

  
 Now, we will develop our publisher and subscriber. Following are the steps for developing publisher and subscriber in Python. 
  

<!-- wp:list -->
<ul><li>Create client</li><li>Connect to broker</li><li>Publish/subscribe message</li><li>Connect callback function</li><li>Run the loop</li><li>Disconnect</li></ul>
<!-- /wp:list -->

  
 Let's understand these steps in details. 
 The first step is to <strong>create a client</strong>. For this, we will create a   `Client`  object from  `paho-MQTT`  python package. 
 
 The second step <strong>connects to the broker</strong>. In our case, we are running the broker on the same machine, therefore, we specify the broker's IP as  `127.0.0.1`  and port as  `1883`  (default port for Mosquitto broker). 
 
 In third step, you can <strong>publish or subcribe</strong> using  publish()  and  subscribe()  function. 
 
 Next, we need to write callback functions. It needs a bit of explanation. <strong>Callback</strong> functions are functions which are executed on the occurrence of particular events. Followings are the table showing the event and their corresponding callback function 
  

<!-- wp:table -->
<figure class=""wp-block-table""><table class=""""><tbody><tr><td> <strong>Event</strong> </td><td> <strong>Callback Function</strong> </td></tr><tr><td> Connection ACK </td><td> on_connect </td></tr><tr><td> Dis-connection ACK </td><td> on_disconnect </td></tr><tr><td> Publish ACK </td><td> on_publish</td></tr><tr><td> Subcription ACK </td><td> on_subcribe </td></tr><tr><td> Un-subcription ACK </td><td> on_unsubcribe </td></tr><tr><td> Message received </td><td> on_message </td></tr></tbody></table></figure>
<!-- /wp:table -->

  
 Let's take one example to understand it. 
 When a client connects to the broker, the broker sends an ACK (acknowledgment) to the client. This event triggers the execution of a callback function   `on_connect` . 
  
#### Running a loop: Why we need to start a loop?
When a publisher sends messages to the broker or subscriber receive messages from the broker, these messages are first stored in the buffer. Now, in order to process all messages (either for sending or receiving), we need to write a loop manually. Thanks to  `paho-MQTT` , it provides three functions for the same purpose, therefore, we don't need to write message processing loop. These functions are as following 
  
::: {.callout-tip}

<ul><li><strong>loop()</strong>: When you call this function, it will process any pending message sending or receiving action. This function waits for a particular time (you can specify using  timeout  parameter) for processing buffer for reading or sending a message. After, that its execution completes. Therefore, if you plan to use this function <strong>you need to call it regularly</strong>.</li><li><strong>loop_forever()</strong>: This function call results in indefinite execution of your program. This function automatically reconnects to the broker in case of disconnection. This function is blocking type function (you can understand it as an infinite for loop) and it returns when you  disconnect  with the broker.</li><li><strong>loop_start() &amp; loop_stop()</strong>:  loop_start()  function starts a new background thread and that thread regularly execute  loop()  function. You can stop this background thread using  loop_stop()  function.</li></ul>

:::

<!-- wp:heading {""level"":6} -->
#### Coding Publisher
<!-- /wp:heading -->

 As we have setup our broker server, now we move towards writing publisher code. 
  
```python

import paho.mqtt.client as MQTT

#  function
def connect_msg():
    print('Connected to Broker')

# function
def publish_msg():
    print('Message Published')

# Creating client
client = mqtt.Client(client_id='publisher-1')

# Connecting callback functions
client.on_connect = connect_msg
client.on_publish = publish_msg

# Connect to broker
client.connect(""127.0.0.1"",1883)

# if you experience Socket error then replace above statement with following one
# client.connect(""127.0.0.1"",1883,60)

# Publish a message with topic
ret= client.publish(""house/light"",""on"")

# Run a loop
client.loop()

```
 The above code publishes a message  on  with topic `house/light`. In this code, we have written two functions  connect_msg  and  publish_msg  (you can use any names for functions). We connected these functions to callback functions using  client.on_connect = connect_msg . We specified that  connect_msg  function is callback function and it will be called when the  Connection ACK  event occurs (events and their callbacks are given in the above table). In this program, we used the  loop  function which process pending action (sending or receiving) and then returns. 
  

<!-- wp:heading {""level"":6} -->
<h5>Coding Subscriber</h5>
<!-- /wp:heading -->

  
 As we have our publisher with topic  house\ligh  topic, we now develop our subscriber for the same topic. 
  

```python
import paho.mqtt.client as MQTT #import the client

# Function to process recieved message
def process_message(client, userdata, message):
    print(""message received "" ,str(message.payload.decode(""utf-8"")))
    print(""message topic="",message.topic)
    print(""message qos="",message.qos)
    print(""message retain flag="",message.retain)



# Create client
client = mqtt.Client(client_id=""subscriber-1"")

# Assign callback function
client.on_message = process_message

# Connect to broker
client.connect(broker_address,1883,60)

# Subscriber to topic
client.subscribe(""house/light"")

# Run loop
client.loop_forever() 
```

<!-- wp:heading {""level"":4} -->
<h4>Execution Results</h4>

<!-- /wp:html -->
[![](https://img.youtube.com/vi/xUoyJKHTrWs/0.jpg)](https://www.youtube.com/watch?v=xUoyJKHTrWs)
  
 References 
  

<!-- wp:list {""ordered"":true} -->
<ol><li>MQTT Version 5.0. Edited by Andrew Banks, Ed Briggs, Ken Borgendale, and Rahul Gupta. 07 March 2019. OASIS Standard. https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html. Latest version: https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html.</li></ol>
<!-- /wp:list -->"