# Google-Assistent-Pi-

# Prequire:
**Raspberry pi**

**Microphone**

**Speaker**


# Setup
## System Update
    sudo rpi-update
## Audio Setup
```the 3.5mm audio jack on raspberry pi did not support audio in, it only support audio out, so we have to use USB device for audio in, and 3.5mm audio jack for audio out ```
### for 3.5mm audio jack speaker
use below command to switch the audio output to audio jack instead of HDMI

    sudo amixer cset numid=3 1

**OR** 

    sudo raspi-config
-> ```Advanced Options``` -> ```Audio``` -> ```Force 3.5 ('headphone') jack```

### if the audio quality is really bad, try below command to fix it
In the end of ```/boot/config.txt``` add the following line: ```audio_pwm_mode=2```
### microphone setup

**right click on the top right 'voice' icon and select ``` Sound Preferences```, manual choose the audio input device, if it does not work, try the blow method to enable the microphone** 

1. Find your recording and playback devices:
- Locate your USB microphone in the list of capture hardware devices. Write down the card number and device number.

      arecord -l
2. Create a new file named ```.asoundrc``` in the home directory (```/home/pi```). Make sure it has the right slave definitions for microphone and speaker; use the configuration below but replace ```<card number>``` and ```<device number>``` with the numbers you wrote down in the previous step. Do this for both ```pcm.mic``` and ```pcm.speaker```.
    ```
    pcm.!default {
      type asym
      capture.pcm "mic"
    }
    pcm.mic {
      type plug
      slave {
        pcm "hw:<card number>,<device number>"
      }
    }
    ```
### Verify that recording and playback work:
- Adjust the playback volume. (Press the up arrow key to set the playback volume level to 100.)

       alsamixer
  
- Play a test sound (this will be a person speaking). Press Ctrl+C when done. If you don't hear anything when you run this, check your speaker connection.
  
      speaker-test -t wav
      
 - Record a short audio clip.
 
        arecord --format=S16_LE --duration=5 --rate=16000 --file-type=raw out.raw
 
 - Check the recording by replaying it. If you don't hear anything, you may need to check the recording volume in alsamixer.
 
        aplay --format=S16_LE --rate=16000 out.raw

## Project Setup
### get project folder
    sudo apt install git
    git clone https://github.com/google/aiyprojects-raspbian.git AIY-projects-python
### install dependence
```
sudo pip3 install --upgrade pip setuptools wheel
sudo pip3 install google-assistant-grpc==0.1.0
sudo pip3 install google-assistant-library==0.1.0
sudo pip3 install google-cloud-speech==0.30.0
sudo pip3 install gpiozero
sudo pip3 install paho-mqtt
sudo pip3 install picamera
sudo pip3 install pillow
sudo pip3 install RPi.GPIO
sudo pip3 install google_auth_oauthlib
```
### link aiy dependence file

    sudo bash
    echo "/home/<your_folder_location>/AIY-projects-python/src" > /usr/local/lib/python3.5/dist-packages/aiy.pth
    exit

## Credentials Setup

refer to ```https://aiyprojects.withgoogle.com/voice-v1/```->```User’s Guide```->```SETTING UP YOUR DEVICE```
follow it until step 12

## test with demo
inside the project folder:

    src/examples/voice/assistant_library_demo.py
    
**for the first time use, it will open a browser and ask you to give permission to it, click on ACCEPT and close the program and run it again**



