## Bluetooth Device Compatibility 
----------------------------------
This driver works with most Bluetooth 2.x adapters. Bluetooth 3.0 and 4.0 should work as well. 
It is recommended that you have bluez-5.37 or higher installed. 
Please report other compatible and incompatible Bluetooth adapters through a pull request to this page.

### Adapters that are known to work

* Cambridge Silicon Radio, Ltd Bluetooth Dongle (Bluetooth 4.0) 

### Adapters that are known not to work ##

* Linksys USBBT100 version 2 (Bluetooth 1.1)
* USB device 0a12:0x0001

### Distributions that are supported ###
ps3joy.py and ps3joy_node.py  supports the following verisons of Ubuntu : 

Ubuntu 9.04 or higher

### ROS API ### 
#### ps3joy.py ####
This is a joystick driver for the PS3 joystick. It can be launched without being in root. 

#### ps3joy_node.py #### 
This is a joystick driver for the PS3 joystick, and it must be launched as root. This is 
also a ROS node for receiving controller feedback data and publish diagnostics. 
Diagnostics consists of connection type, battery status and charging status. 

Launching this joystick driver is recommended for testing. 
 
### Pairing the PS3 Joystick with the Bluetooth Dongle ### 
Make sure the ps3joy depenedencies are already installed and compiled:
 
``` 
rosdep install ps3joy 
rosmake ps3joy
``` 
1. Plug in the PS3 joystick with a usb cable and plug in your bluetooth dongle 

2. To load the dongle's MAC address into the PS3 joystick use: 
``` 
sudo bash 
rosrun ps3joy sixpair 
``` 
You should expect to see something like the following:
 
```
Current Bluetooth master: 00:15:83:ed:3f:21
Setting master bd_addr to 00:15:83:ed:3f:21
```
3. It is not good practice to continue to run as root. Press Ctrl -D to get out of root. 

#### Potential Issues ####
If you recieve the following message on your terminal, it's possible your bluetooth is disabled. 
```
Current Bluetooth master: 00:15:83:ed:3f:21
Unable to retrieve local bd_addr from `hcitool dev`.
Please enable Bluetooth or specify an address manually.
``` 

1. Check the status of your bluetooth by entering the following: 
``` 
sudo systemctl status bluetooth
``` 
You may see something like this: 

```
● bluetooth.service - Bluetooth service
   Loaded: loaded (/lib/systemd/system/bluetooth.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:bluetoothd(8)
```

If you do, that means your bluetooth service is disabled. To enable it enter 
```
sudo systemctl start bluetooth
sudo systemctl status bluetooth  
```
After running these commands your bluetooth service should be up and running: 

```
● bluetooth.service - Bluetooth service
   Loaded: loaded (/lib/systemd/system/bluetooth.service; disabled; vendor preset: enabled)
   Active: active (running) since Thu 2017-06-29 16:21:43 EDT; 16s ago
     Docs: man:bluetoothd(8)
 Main PID: 27362 (bluetoothd)
   Status: "Running"
   CGroup: /system.slice/bluetooth.service
           └─27362 /usr/local/libexec/bluetooth/bluetoothd
```
Retry the commands that were mentioned in step 2 for pairing the PS3 joystick. 

2. Run the following command: 
``` 
hciconfig hci0 reset 
``` 
followed by: 
```
sudo bash 
rosrun ps3joy sixpair
``` 
### Command-Line Options ### 
This applies to both ps3joy.py and ps3joy_node.py 
```
rosrun ps3joy ps3joy.py --help
usage: ps3joy.py [--inactivity-timeout=<n>] [--no-disable-bluetoothd] [--redirect-output] [--continuous-output]=<f>
<n>: inactivity timeout in seconds (saves battery life).
<f>: file name to redirect output to.
``` 

`--inactivity-timeout` 
  This may be useful for saving battery life and reducing contention on the 2.4 GHz network.Your PS3 controller 
  will shutdown after a given amount of time of inactivity.  

`--no-disable-bluetoothd` 
   ps3joy.py will not take down bluetoothd. Bluetoothd must be configured to not handle input device, otherwise
   you will receive an error saying "Error binding to socket". 

`--redirect-output`
   This can be helpful when ps3joy.py is running in the backgound. This will allow the standard output
   and error messages to redirected into a file.   

`--continuous-output`
   This will output continuous motion streams and as a result this will no longer leave extended periods of 
   no messages on the /joy topic. ( This only works for ps3joy.py. Entering this parameter in ps3joy_node.py will
   result in the parameter being ignored.)

### Connecting the Joystick via Bluetooth ### 
You can now unplug your joystick from your computer but your bluetooth dongle should be plugged in.
Now, initiate a connection between your joystick and your bluetooth dongle. 

``` 
sudo bash 
rosrun ps3joy ps3joy.py 
``` 
You will see a prompt on your terminal that looks like this: 
```
Waiting for connection. Disconnect your PS3 joystick from USB and press the pairing button.
```

After removing the usb cable press the playstation home or pairing button. Open a new terminal 
and restart your bluetooth. 
```
sudo systemctl restart bluetooth 
``` 
You should then see: 
```
Connection is Activated. 
```
