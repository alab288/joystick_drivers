Playstation 3 Sixaxis Joystick Driver
=====================================

## Packages that need to be installed
 * joystick
 * libusb-dev
 * bluez (>=5.37)

## Pairing instructions

### Joystick and Bluetooth on Same Machine

If you can connect the joystick and the bluetooth dongle into the same
computer.

Connect the joystick to the computer using a USB cable.

Load the bluetooth dongle's MAC address into the ps3 joystick using:

```
sudo bash
rosrun ps3joy sixpair
```

### Joystick and Bluetooth on Different Machines

If you cannot connect the joystick to the same computer as the dongle.

Find out the bluetooth dongle's MAC address by running (on the computer that has the bluetooth dongle):

```
hciconfig
```

If this does not work, you may need to run:

```
sudo hciconfig hci0 up
```

and retry:

```
hciconfig
```

Plug the PS3 joystick into some other computer using a USB cable.

Replace the joystick's mac address in the following command: 

```
sudo rosrun ps3joy sixpair 01:23:45:67:89:ab
```

## Starting the PS3 joystick

### Create Joystick Device

```
rosrun ps3joy ps3joy.py
```

This should make a joystick appear at `/dev/input/js?`, where `?` is the joystick id.

You can check that it is working with:

```
jstest /dev/input/js?
```

### Publish ROS Joy Messages

Open a new terminal and reboot bluez and run joy with:

```
sudo systemctl restart bluetooth
rosrun joy joy_node
```

Open a new terminal and echo the joy topic to verify:

```
rostopic echo joy
```

## Troubleshooting

### Joystick won't pair (keeps flashing even after the pair button is pressed)

Open the Bluez device list, select the joystick, and make the joystick a trusted device.
