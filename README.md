# OK Google Turn On Particle LED

Particle Photon

“The Photon is a $19 tiny Wi-Fi development kit for creating connected projects and products for the Internet of Things.”  This development board is extremely easy to use and has a cloud based programming interface with the feeling of an Arduino.  Since it's connected to Wi-Fi the Photon can easily be read and have its values changed through a simple web interface.  

Google Now

Google Now is a personal assistant that extends Google to give you intelligent recommendations, answer questions, perform tasks, and proactively give information based on your Google accounts.  One form of input into Google Now on your Chrome Browser or Android phone is to simply say “OK Google” and then your command and can be configured to listen for this command at all times.  Some example commands might be “OK Google, Where is my package? [only if the confirmation number is in your Gmail]”, “OK Google, What is this song?”, or “OK Google, Turn on Flashlight”.   

Tasker

Tasker is one of the most useful Android applications out there.  Although it’s not free, it’s worth every penny because of it’s so useful.  Tasker has “Profiles” which are basically triggers waiting for a certain condition or event to happen on your phone, such as battery life being less than 5% or arriving at a certain location by latitude/longitude.  After the Profile condition is met Tasker will run a task or set of tasks, such as turning on/off Wi-Fi or sending a SMS.

Pushbullet

Pushbullet connects all of your devices, making them feel like one when receiving notifications.  Pushbullet also has an API to create custom notifications which will also show up on all of your devices.

Hello World

This tutorial is a classic Hello World project for understanding the basics of Tasker, the Particle Photon, Pushbullet, and the interaction between all three.  The concept here is pretty simple.  The user will ask Google Now, “OK Google, Turn on my LED”, the LED connected to the Particle Photon will illuminate, and then Pushbullet will send notifications to all your devices saying the LED has been turned on.  Imagine the future possibilities after we complete this tutorial!!  Ready to get to work?

 

1

Make sure you can connect with the Particle Photon

The easiest way to make sure you can connect is by following Particle’s Getting Started guide found here:

https://docs.particle.io/guide/getting-started/start/photon/

2

Connect the Breadboard



Use the breadboard picture above to connect the LED in series with a 100Ω resistor from D0 to GND.  Even though the Photon produces a 3.3V it’s best to use a current limiting resistor for the LED.  The Photon can be powered either directly through VIN or by connecting a USB cable to your computer.

3

Program the Particle Photon

Login to https://build.particle.io/signup, open the Web-Connected LED Example, and make the following changes so we can see the state of the LED:

First we need to initialize the LED state variable
Add: int led1state = 0;
After: int led2 = D7;
Next, we will create the Spark variable called led1state so it can be accessed via the Particle API
Add: Spark.variable("led1state", &led1state, INT);
After: Spark.function("led",ledToggle);
Now let’s add the notification when the LED gets turned on
Add: Spark.publish("pushbullet", "LED Turned On!", 60, PRIVATE);
We also need to notification for when the LED is turned off
Add: Spark.publish("pushbullet", "LED Turned Off!", 60, PRIVATE);
4

Set up Tasker

Download both Tasker and AutoVoice
Make sure AutoVoice is enabled in the Accessibility Menu
Open AutoVoice
Press "Google Now Integration"  
Press "Not Enabled"  
Scroll down and press "AutoVoice Google Now Integration"  
Press the slider and accept the notification   
Add New Tasks
Add → Type "Turn LED On" → Net → HTTP Post  
HTTP Post Settings  
Server:Port → https://api.spark.io
Path → /v1/devices/<device_id>/led?access_token=<access_token>
Data/File → params=on
Add a second task for turning the LED Off by following the previous two steps again but rename the task to "Turn LED Off" and the Data/File setting must be "params=off"
Add New Profile
Add → Event → Plugin → AutoVoice → Recognized  
Recognized Settings
Click on the pencil for configuration    
Click on Speak Filter, say your command, and press the check mark to exit the configuration
Click on the "Turn On LED" task that was made earlier
Add a second profile for turning the LED Off by following the previous three steps again but say "Turn LED On" and use the "Turn Off LED" task made earlier
5

Create Particle Webhook for Pushbullet Notification

Create a Pushbullet API Access Token (if needed)  
Particle webhooks can only be made with the Particle Command Line Interface (CLI) which can be set up by following the directions at https://docs.particle.io/guide/tools-and-features/cli/electron/
Open your favorite text editor, place the following code in a file named pushbullet_webhook.json, and add in your Pushbullet API authorization token 
{ "eventName": "pushbullet", "url": "https://api.pushbullet.com/v2/pushes", "requestType": "POST", "headers": { "Authorization": "Bearer <your-pushbullet-authorization-token-here>", "Content-Type": "application/json" }, "json": { "type": "note", "title": "{{SPARK_EVENT_VALUE}}", "body": "" }, "mydevices": true }
Open the command line and navigate to pushbullet_webhook.json
Type "particle webhook create pushbullet_webhook.json" and wait for the response.
TIP: Download the Pushbullet app for notifications on your phone
 

This project and code can also be found on Github here: https://github.com/BridgeCityRobotics/OK-Google-Turn-On-Particle-LED

 
