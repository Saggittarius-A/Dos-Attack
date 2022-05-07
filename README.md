# Dos-Attack

## Idea of the project

The purpose of the project is to experiment the SlowITe attack on MQTT

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
`____________command to change the config file`<br>
Add the following lines and save it <br>
`allow_anonymous true`<br>
`listener 1883`<br>

Now run the code from the command given above

## Testing code
### Case 1
Edit the config file to add `log_dest topic`
Then save the file and reboot system <br>

Subscribe and publish to the broker using the following commands<br>
`_________________`<br>
`_________________`<br>
Now you will be able to see each clients deatails while they are trying to connect to the broker in real time.
Making attack possible in this scenerio

### Case 2
For authentication add the following lines in configuration file<br>
`allow_anonymous true`<br>
`__________________`<br>
now try to execute the attack. It wont be able to stop the broker because of the authentication

## Prevention
