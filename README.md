http://blog.tafkas.net/2012/10/03/gathering-and-charting-temperatures-using-rrdtool-and-highcharts/

# Gathering and Charting Temperatures using RRDTool and Highcharts

tl;dr Checkout the charts on my RaspberryPi

For quite a long time I was looking for a way to monitor and record th temperature and humidity at my apartment. What was missing was a convenient, preferably wireless solution. After receiving my RaspberryPi I started to look into that more intensively.

USB-WDE1 Receiver

The USB Weather Data Receiver USB-WDE1 wirelessly receives data from various weather sensors of ELV at 868 MHz. The receiver is connected to a USB port on the computer, so no additional power supply is required. The data is transmitted via a simple serial ASCII protocol, which is well documented by ELV. The RasberryPi running Raspbian is used for the data acquisition allowing very little power consumption while being completely flexible.


The USB interface of the USB WDE1 is realized by the USB-serial converter CP 2102 of Silicon Labs. The responsible kernel module CP2101 for accessing the device is included in any modern Linux distribution. When connecting the USB-WDE1 should appear in the system once the appropriate messages:

    $ Dmesg
    usb 1-3.1: Product: ELV USB WDE1 weather data receiver
    usb 1-3.1: Manufacturer: Silicon Labs
    
The udev subsystem then also creates a corresponding device file, usually is the / dev/ttyUSB0. This device behaves as seen by a Linux application program such as a serial port and therefore can be accessed with any terminal program such as minicom. If you connect other USB-to-serial converter to the RaspberryPi, the device can also be called /dev/ttyUSB1 or similar. It is important to set the baud rate to 9600 bits/s.

A simple and universal way to output the data supplied by the receiver on the terminal provides to tool socat, which should also be part of any Linux distribution. You may have to re-install it via the package manager. Using

    socat / dev/ttyUSB0, B9600 STDOUT
    
    $1;1;;21,6;9,5;21,6;21,3;21,1;19,2;;;58;78;58;59;42;53;;;;;;;;0
    $1;1;;21,6;9,5;21,6;21,3;21,1;19,3;;;58;78;58;59;42;53;;;;;;;;0
    $1;1;;21,6;9,5;21,6;21,3;21,1;19,3;;;58;78;58;59;42;53;;;;;;;;0
