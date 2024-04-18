# CSC453-LoRaWAN-Project
This Project is for NCSU's CSC453 LoRaWAN project.


Group Members: Allyson Smith, Steven Bui, Sumedh Hanamantappa, Will Hazlehurst

The Source Code was adapted from the instructions provided from [this article](https://www.disk91.com/2023/technology/internet-of-things-technology/lets-get-started-with-helium-rak11300-rp2040-sx1262/).

## Initial Setup
For this project, we used MeteoScientific's helium network console. First, we created an Application named 'Test'. Then, we created a device profile that uses the US915 bandwidth and uses the MAC Version LoRaWAN 1.0.3. We automatically generated the Device EUI and App Key, which can we seen in the source code.

## Setup Instructions
Plugging the RAK11300 into the computer with the provided USB cable, we must intsall the necessary libraries and boards in the Arduino IDE. After that, there are a few steps that must be taken.
Retrieve your console's DeviceEUI and AppKey. Then, in the source code, modify the nodeDeviceEUI, nodeAppEUI, and nodeAppKey variables as follows:
nodeDeviceEUI: MSB hex array of your console's Device EUI. For example, if the DeviceEUI is 69ca6c0d4c58b44b, then nodeDeviceEUI will be { 0x69, 0xCA, 0x6C, 0x0D, 0x4C, 0x58, 0xB4, 0x4B }.
nodeAppEUI: This will be all 0's, since this network just uses joinEUIs instead of AppEUIs. For this, nodeAppEUI will be { 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00 }.
nodeAppKey: This will follow the same MSB hex array as the nodeDeviceEUI, just with 16 bytes instead of 8.

There are a few bugs that need to be fixed in order to get things running properly.
- Line 67: `bool send_now = false;` must be changed to `volatile bool send_now = false;`
- Line 282: Comment out or remove the line `Serial.println("Sending frame now...");`

## Connecting to the Helium Network
After completing all of these steps, upload the sketch to the board, and you should see a message that reads:
```
=====================================
Welcome to RAK11300 LoRaWan!!!
Type: OTAA
Region: US915
=====================================
```
This means your board is now running the LoRaWAN software! Now, if you're getting the error message:
```
OTAA join failed!
Check your EUI's and Keys's!
Check if a Gateway is in range!
```
This most likely means you are not within range of a LoRa gateway. If this is the case, use [this map](https://explorer.helium.com/) to find the nearest online gateway. Once within range, give it another try. If successful, you should start receiving messages like:
```
OTAA Mode, Network Joined!
Sending frame...
lmh_send ok count 1
TX finished
Sending frame...
lmh_send ok count 2
TX finished
...
```
If that is the case, check your helium console logs, and you should see some output, which includes some device statistics.
If you wish to transmit different data, modify the source code to transmit the data you wish to send to the console.
