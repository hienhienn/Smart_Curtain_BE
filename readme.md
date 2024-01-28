# Introduce
This project is server app, which is apart of smart curtain system and written in Flask python.
![System Architecture](/image/System.png)
It provides APIs and websockets connect to any client app can connect and control smart curtain in their device.
# Prequisite
1. Install python and pip
## For linux
```bash
    sudo apt update
    sudo apt upgrade -y
    sudo apt install python3.10
    # Check pip version
    pip3 --version
    # If you don't have pip, run this bash and check again
    sudo apt install python3-pip
```
## For windows
Download and install [python 3.10 and pip](https://www.python.org/downloads/release/python-31013/) or use python=3.10 in a virtual environment
Check the result
```bash
    python --version
    pip --version
    # if you don't have pip, run this bash and check again
    python get-pip.py
```
2. Create a new HiveMQ Cloud or use your HiveMQ cluster connected to ESP32
You can create your HiveMq cloud cluster on free tier [here](https://console.hivemq.cloud/)
Create credentials with a username, password, and permissions in the Access Management tab.
![HiveMq Cloud](/image/HiveMq%20cluster.png)
**Note: Remember your username and password for this cluster.**
3. Connect to your MongoDB DB
You can use MongoDB Atlas or a local MongoDB.
**Note: Remember your database URL, username, and password**
# How to Build This Project in a Local Environment
1. Clone this project
```bash
    git clone https://github.com/nkt780426/Smart_Curtain_BE
```
2. Install all dependencies
## For linux
```bassh
    pip3 install -r requirements.txt
```
## For windows
```bash
    pip install -r requirements.txt
```
3. Update Database and HiveMq with your configuration in .env
4. Run project
## For linux
```bash
    python3 app.py
```
## For Windows
```bash
    python app.py
```
# How to expose this project to internet
# All APIs and Websockets to client
A client app that wants to control the smart curtain must use these APIs and Websockets.
1. Start this app and vist [http://localhost:5000](http://localhost:5000) to see all APIs and details of it
![APIs Documentation](/image/APIs%20document.png)
2. Connect to the following websockets
- **esp32_status**
- **inform**
- **auto_mode**
# Connecting to ESP32
This app publishes and subscribes the follow topic to HiveMQ cloud
1. Publish messages to topics
- auto_requests (QoS = 2)
```json
	{
		"status": true/false,
		"percent": ....,
		"correlation_data": ...
	}
```
- alarm_requests (QoS = 2)
```json
	{		
		"percent": ....,
		"correlation_data": ....
	}
```
2. Subcribe messages to topics
- esp32_status (QoS = 2)
```json
    {
		"activate": true/false
	}
```
- inform (QoS = 1)
```json
	{
		"indoor": ....,
		"outdoor": ....,
		"ledState": ....,
		"percent": ....,
	}
```
- auto_responses (QoS = 2)
```json
	{
		"status": true/false,
		"correlation_data": ....
	}
```
- alarm_responses (QoS = 2)
```json
	{
		"status": true/false,
		"auto_status": true/false,
		"correlation_data": ...
	}
```