# Make you own Raspberry Pi Camera Stream

Create your own live stream from a Raspberry Pi using the Pi camera module. Build your own applications from here.

## How it works
The Pi streams the output of the camera module over the web via Flask. Devices connected to the same network would be able to access the camera stream via

```
<raspberry_pi_ip:5000> 
```

## Screenshots
| ![Setup](docs/pi-stream-client.jpg) | ![Live Pi Camera Stream](docs/pi-stream-screen-capture.jpg) |
|---|---|
| Pi Setup | Pi - Live Stream |

## Preconditions

* Raspberry Pi 4, 2GB is recommended for optimal performance. However you can use a Pi 3 or older, you may see a increase in latency.
* Raspberry Pi 4 Camera Module or Pi HQ Camera Module (Newer version)
* Python 3.7 (there are dependancy issues with opencv-contrib-python with >3.7)
* Enable SSH on your pi
```
sudo systemctl enable --now ssh
```
* If you have created a new user to run this (recommended), ensure they are part of the "video" group
```
sudo usermod -aG video <username>
```

## Library dependencies
Install the following dependencies to create camera stream.

```
sudo apt update 
sudo apt upgrade
sudo apt install libatlas-base-dev libjasper-dev libqtgui4 libqt4-test libhdf5-dev -y

pip3 install -r requirements.txt
```

## Step 1 – Cloning Raspberry Pi Camera Stream
Open up terminal and clone the Camera Stream repo:

```
cd /home/pi
git clone https://github.com/EbenKouao/pi-camera-stream-flask.git
```

## Step 2 – Launch Web Stream

Note: Creating an Autostart of the main.py script is recommended to keep the stream running on bootup.
```bash cd modules
sudo python3 /home/pi/pi-camera-stream-flask/main.py
```

## Step 3 – Autostart your Pi Stream


### SystemD

The most production-ready way to run this service, is via systemd. There is a sample config file in the docs directory.

There a couple modifications that need to be done:

```
vim docs/picamera.service
```

Modify the *AssertPathExists* variable to point to your location of the pi-camera-stream-flask repo

If, like me, you are using pyenv for the virtual env, you will need to modify the ExecStart path to use the shim for your virtualenv. As an example, mine looks like:
```/home/blair/.pyenv/versions/3.7.4/envs/stream_37/bin/python /opt/pi-camera-stream-flask/main.py```

Once these modifications are done, save the file.

```
sudo cp docs/picamera.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now picamera
```


### Profile Script
If you are not a fan of running this via systemd, you can do this by going editing the /etc/profile to:

```
sudo nano /etc/profile
```

Go the end of the and add the following (from above):

```
sudo python3 /home/pi/pi-camera-stream-flask/main.py
```

This would cause the following terminal command to auto-start each time the Raspberry Pi boots up. This in effect creates a headless setup - which would be accessed via SSH. 
Note: make sure SSH is enabled.



## Download Beta image of Raspberry Pi Camera Stream
Any troubles installing, try out the already compiled Raspberry Pi (Raspbian OS) Image of [Raspberry Pi Camera Stream](https://smartbuilds.io).
![Raspbian Camera Stream Image](img/readme/[].png)
