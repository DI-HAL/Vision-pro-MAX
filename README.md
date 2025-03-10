# Object Detection and Navigation Aid for the Visually Impaired  

![Python](https://img.shields.io/badge/python-3.11-blue)  
![License](https://img.shields.io/badge/license-MIT-green)  

A Raspberry Pi-based real-time object detection system designed to assist visually impaired individuals. This project identifies obstacles like manholes, potholes, and bumps using YOLO and a Hailo 8L TPU. Detected objects are announced through audio feedback, helping users navigate safely.  

---

## Features  
- **Real-Time Object Detection**: Identifies obstacles such as manholes, potholes, bumps, etc.  
- **Audio Feedback**: Announces detected objects using text-to-speech technology.  
- **Efficient Inference**: Utilizes the Hailo TPU for fast and accurate detection.  
- **Customizable**: Easily adjust detection thresholds or add new objects.  

---

## System Requirements  

### Hardware  
- Raspberry Pi 5  
- Hailo 8L TPU  
- USB Camera  
- NVMe SSD (optional, for faster storage)  

### Software  
- Raspberry Pi OS (Debian 12 Bookworm)  
- Python 3.11  
- GStreamer 1.0  

---

## Installation  

### Step 1: Clone the Repository  
```bash
git clone https://github.com/DI-HAL/Vision-pro-MAX.git  
cd Vision-pro-MAX
```
###Step 2: Install System Dependencies
```bash
sudo apt update && sudo apt install -y gstreamer1.0-tools mpg321 python3-pip
```
Step 3: Install Python Dependencies
```bash
pip install -r requirements.txt
```
Step 4: Set Up Hailo Environment
Install the Hailo SDK and drivers by following the Hailo Documentation.

Step 5: Connect Hardware
Plug in the USB camera and Hailo TPU to the Raspberry Pi.
Ensure the camera is functional by running:
```bash
gst-launch-1.0 v4l2src ! videoconvert ! autovideosink
```
Step 6: Run the Application
Start object detection and audio feedback with:

```bash
python object_detection_and_speech.py
```
Automatic Startup on Boot
To ensure the application starts automatically when the Raspberry Pi boots:

Create a systemd service file:

```bash
sudo nano /etc/systemd/system/object-detection.service
```
Add the following configuration:
ini
```bash
[Unit]  
Description=Object Detection with Audio Feedback  
After=multi-user.target  

[Service]  
Type=simple  
ExecStart=/usr/bin/python3 /home/pi/your-repo-name/object_detection_and_speech.py  
WorkingDirectory=/home/pi/your-repo-name  
StandardOutput=inherit  
StandardError=inherit  
Restart=always  
User=pi  

[Install]  
WantedBy=multi-user.target
```
Enable and start the service:

```bash
sudo systemctl enable object-detection.service  
sudo systemctl start object-detection.service
```
Now the detection system will automatically launch on boot.

Usage
Power on the Raspberry Pi and wait for the application to start.
As objects are detected by the camera, you will hear announcements like "manhole detected" or "bump detected."
To customize detection behavior or add new objects, edit the app_callback function in object_detection_and_speech.py.
How It Works
Detection Pipeline
This project uses a GStreamer-based detection pipeline:

Video frames are captured using the USB camera.
Frames are processed with YOLO using the Hailo TPU for efficient inference.
Detected objects are parsed, and their labels are converted to audio feedback.
Text-to-Speech Integration
Detected object labels are passed to a text-to-speech system (gTTS), which generates audio announcements in real-time.

File Structure
plaintext
```bash
project/  
├── detection_pipeline/          # GStreamer pipeline files  
├── hailo_rpi_common.py          # Hailo helper functions  
├── object_detection_and_speech.py # Main script  
├── requirements.txt             # Python dependencies  
├── README.md                    # Documentation
```
Example Detection Logic
Here's how the detection and audio feedback system is implemented:

Detection Callback:

The app_callback function is triggered for every frame processed.
It extracts detections such as label, confidence, and bbox.
Detected objects with confidence greater than 0.5 are added to a list.
Speech Output:

If new objects are detected, a thread triggers text-to-speech to announce detections.
Example: If a "manhole" is detected, the system announces, "Manhole detected."
Cooldown Management:

To avoid repetitive audio announcements, a cooldown timer ensures TTS is not triggered more than once every two seconds.

Customization
Adding New Objects
To add support for detecting new objects:

Update the YOLO model to include the desired classes.
Modify the app_callback function in object_detection_and_speech.py to handle new classes.
Adjusting Detection Thresholds
Edit the confidence threshold in the app_callback function:

python
```bash
if confidence > 0.5:  # Adjust the threshold here
``` 
Language Support
To change the language of audio feedback, update the gTTS language parameter:

python
```bash
tts = gTTS(text=text, lang='en')  # Replace 'en' with your preferred language code
```
Future Plans
Multi-Language Audio Feedback: Support for additional languages.
Battery Integration: Make the system portable.
Enhanced Object Categories: Add detection for traffic signs and other obstacles.
Troubleshooting
Common Issues
No Audio Playback:

Ensure mpg321 is installed:
```bash
sudo apt install mpg321
```
Camera Not Detected:

Verify the camera connection and run a test with:
```bash
gst-launch-1.0 v4l2src ! videoconvert ! autovideosink
```
Detection Freezes or Stutters:

Reduce the detection threshold to decrease computational load.
Ensure the Hailo TPU is connected and functioning correctly.
Contributing
Contributions are welcome! Please submit pull requests or open issues for any suggestions or bugs.

License
This project is licensed under the MIT License. See the LICENSE file for details.

Acknowledgments
Hailo for their powerful edge inference hardware.
gTTS for text-to-speech functionality.
The open-source community for making this project possible.
vbnet

```bash
 


have to figure out whats next







