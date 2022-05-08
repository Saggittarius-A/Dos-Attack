# Dos-Attack

## Idea of the project

The purpose of the project is to experiment the SlowITe attack on MQTT broker on raspberrypi.

## normal functioning of MQTT

### Subscriber
`mosquitto_sub -h <IP address> -t <topic>`
### publisher
`mosquitto_pub -h <IP address> -t <topic> -m "message"`

## Prerequisites

First of all, you have to type the following line on your shell in order to install and manage the other software packages, written in Python, used in this project (for more info click [here](https://linuxize.com/post/how-to-install-pip-on-ubuntu-18.04/)).

```bash
sudo apt install python3-pip
```
Then, you have to install on Ubuntu:

 - [paho-mqtt](https://pypi.org/project/paho-mqtt/): provides a client class which enable applications to connect to an MQTT broker to publish messages, and to subscribe to topics and receive published messages.
	  ```bash
	 sudo pip3 install paho-mqtt
	```
 - [tqdm](https://pypi.org/project/tqdm/): shows a progress in a loop, using a smart progress meter.
	 ```bash
	 sudo pip3 install tqdm
	```
 - [Mosquitto MQTT Broker](https://mosquitto.org/): open source message broker that implements the MQTT protocol versions 5.0, 3.1.1 and 3.1. 
	 ```bash
	 sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
	 sudo apt-get update
	 sudo apt-get install mosquitto
	 sudo apt-get install mosquitto-clients
	```
	After that, Mosquitto is installed as a service and should start automatically after install. To test if it is running, use commands `netstat -at` (you should see the Mosquitto broker running on port 1883) or `sudo service mosquitto status`.
  
  ## Usage
**MQTT_SlowDoS.py**
```bash
$ python3 MQTT_SlowDoS.py -a broker_address [-p broker_port] [-k keep_alive] [-h | --help]
```


**Arguments:**
| Flag           | Description    | 
| :------------- | :----------: | 
|  -h, --help    | Show help message  | 
| -a             | IP address of the MQTT broker (mandatory flag) | 
| -p             | Port of the MQTT broker (default: 1883) | 
| -k             | Keep-Alive parameter used in the MQTT protocol (default: 60 sec) | 


## Run the code
`sudo nano /etc/mosquitto/mosquitto.conf` command to change the config file <br>
Add the following lines and save it <br>
`allow_anonymous true`<br>
`listener 1883`<br>

Now run the code from the command given above

## Testing code

### Making configuration changes only on broker[here considering broker as raspberrypi]

### Case 1
Go inside config by `sudo nano /etc/mosquitto/mosquitto.conf`<br>
Edit the config file to add `log_dest topic` and `log_dest syslog`<br>
Then save the file and reboot system <br>

Subscribe and publish to the broker using the following commands<br>
`mosquitto_sub -v -h hostname -t '$SYS/broker/log/#'`<br>
`mosquitto_pub -h hostname -t '$SYS/broker/log/N' -m msg`<br>
Now you will be able to see each clients details while they are trying to connect to the broker in real time as shown below.

![image](https://user-images.githubusercontent.com/57555177/167286358-af067ac8-5c2d-4e82-9dfd-8e9f9a262382.png)

Start attack in this scenerio and observe. There will be so many clients creation with the same ip address simultaniously. This way you can check if broker is under attack or not.

### Case 2
For authentication add the following lines in configuration file of broker here it is raspberrypi<br>
`allow_anonymous false`<br>
`mosquitto_passwd -c /etc/mosquitto/pwfile`<br>

Don't forget to reboot!!

To create new client run `sudo mosquitto_passwd -c /etc/mosquitto/pwfile username`

To check if authentication is working try to publish and subscribe by command below

`mosquitto_sub -h broker_address -u username -P password -t topic`

`mosquitto_pub -h broker_address -u username -P password -t topic -m msg`

Now try to execute the attack. It wont be able to stop the broker because of the authentication.

## Prevention
For the prevention authentication is good enough. Do same as mentioned above.

## How will attacker know if attack is successfull or not?
For this you have to make changes in the MQTT_SlowDoS.py file by----

<b>Step1</b>: Comment the part of code as shown below
![iot_img_1](https://user-images.githubusercontent.com/57555177/167286729-94f72086-bf1d-48d5-a81f-2fb27ab3f9ac.png)

<b>Step2</b>: Uncomment part of code as shown below
![iot_img_2](https://user-images.githubusercontent.com/57555177/167286744-7d0fcfcc-5482-4140-bd76-9cac7f55670d.png)

<b>Step3</b>: Run the code by python3 MQTT_SlowDoS.py -a hostname -p 1883 -k 60
Now observe the output if you gets connection refused then attack is unsuccessful otherwise successfull.


