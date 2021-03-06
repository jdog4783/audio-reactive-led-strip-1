# Audio Reactive LED Strip

[![Build Status](https://travis-ci.org/segfault16/audio-reactive-led-strip.svg?branch=develop)](https://travis-ci.org/segfault16/audio-reactive-led-strip)

Real-time LED strip music visualization using Python and the ESP8266 or Raspberry Pi.

The works in this project is based on [https://github.com/scottlawsonbc/audio-reactive-led-strip](https://github.com/scottlawsonbc/audio-reactive-led-strip).


# Getting started (local machine)

## Python Dependencies
Visualization code is compatible with Python 2.7 or 3.5. A few Python dependencies must also be installed:
- Numpy
- Scipy (for digital signal processing)
- PyAudio (for recording audio with microphone)

On Windows machines, the use of [Anaconda](https://www.continuum.io/downloads) is **highly recommended**. Anaconda simplifies the installation of Python dependencies, which is sometimes difficult on Windows.

### Installing dependencies with Anaconda
Create a [conda virtual environment](http://conda.pydata.org/docs/using/envs.html) (this step is optional but recommended)
```
conda create --name visualization-env python=3.5
source activate visualization-env
```
On Mac, you need to install portaudio
```
brew install portaudio
```

Install dependencies using pip and the conda package manager
```
conda install numpy scipy

pip install pyaudio matplotlib jsonpickle flask
```

## Visualization Server

For local development we need to somehow visualize the RGB data.
For this you can use [openpixelcontrol](https://github.com/zestyping/openpixelcontrol).

- Clone the project parallel to this repository
- Follow the instructions to compile the project
- Use the compiled binary `gl_server` to start an OpenGL visualization server on port 7890 to visualize the LED strip configuration for the demo program

```
../openpixelcontrol/bin/gl_server -l layouts/demo_layout.json
```

## Running the demo

```
python filtergraphDemo.py
# For more information:
python filtergraphDemo.py -h
# e.g.
python filtergraphDemo.py -N 300 -D FadeCandy --device_candy_server 192.168.9.241:7890 -C spectrum
# Running on RaspberryPi:
sudo python filtergraphDemo.py
```

## Running unit tests

```
python -m unittest discover
# To run a single test:
python -m unittest tests.test_opc_server.Test_OPC_Server.test_serverReceives
```

# Getting started (raspberry pi)

## Install dependencies
```
sudo apt-get remove python2.7
sudo apt-get autoremove
sudo apt-get update
sudo apt-get install python3-numpy python3-scipy python3-pyaudio python3-matplotlib python3-jsonpickle
```

## Install rpi_ws281x
```
git clone https://github.com/rpi-ws281x/rpi-ws281x-python
cd rpi-ws281x-python
git submodule update --init --recursive
cd library
sudo python3.5 setup.py install
```

## Audio configuration

See [Audio setup on RaspberryPi](docs/audio_setup_pi.md).

## Server 

```
python3 server.py
```

## Run as service

e.g. by copying the following file to `/etc/systemd/system/ledserver.service`

```
[Unit]
Description=Audio-reactive LED Strip
After=network.target

[Service]
ExecStart=/usr/bin/python3 server.py -D RaspberryPi
WorkingDirectory=/home/pi/projects/audio-reactive-led-strip
StandardOutput=inherit
StandardError=inherit
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

and starting the service with `sudo systemctl start ledserver`

# Development

All files under [resources](./resources) are generated by running `npm run build` in [server](./server).
