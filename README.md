# openipcmonster-jovision-gk7205-ptz
OpenIPC Custom Build for No Name Jovision GK7205V200 PTZ WiFi Camera
<img width="900" height="759" alt="shopping" src="https://github.com/user-attachments/assets/80bb90bc-26af-42f9-a204-3f7a84f6f547" />

This is a no-name concealed WiFi spy camera that is popularly sold around the world on platforms like Amazon. There are two versions of this camera, a "Bluetooth speaker" and a "wireless Qi charger". Both cameras have the same internals and run the same firmware:


Chipset: Goke GK7205V200
Image sensor: Galaxycore GC2053
SPI flash: 16MB
WiFi: Realtek RTL8188FU (connected over GPIO)
The camera has a stepper motor which has a 90 degree pan function (no tilt or zoom).


The camera mainboard itself has no manufacturer markings, and the camera is powered via a battery independently from the USB-C interface that also powers the Bluetooth speaker or charger component of the device. The battery can charge via USB-C, or the camera can be run entirely off the battery. There is an unused MicroUSB port on the camera mainboard, which was found to provide a more stable power source for operation - the cameras have trouble handling their multiple power sources and when the battery runs low or the USB-C power interface is stretched between providing power to the camera's "disguise function" and the camera, all sorts of strange hardware malfunctions begin to show up - the RTL WiFi chip browns out and does not show up in the system, the stepper motor struggles with moving smoothly, etc. 

On stock firmware, the cameras are connected to a Chinese cloud based platform called Closeli, and use two different apps (Cloud365 and toopcam) for control. Older versions come with Cloud365, newer ones come with toopcam despite the apps being the exact same in every respect, which highlights the principle idea that these cameras may be running off of an infrastructure that operates in a grey area with Chinese law, storing data with no protection and in a manner that makes these cameras unsafe for global customers.
The Chinese apps are laden with banner ads, and the firmware sends repeated requests to Chinese domains and IP addresses.
Stock, the camera has several open ports, including an empty page at port 80, RTSP running at port 8554, and a delightful open Telnet port at 8357. Despite RTSP, the camera does not have Onvif.

Upon teardown, the camera mainboard has a very clear and well produced UART serial interface, with holes for GND, RX, TX and 3V3. These type of cameras rarely ever have this kind of clarity and often use terrible tiny copper pads - this is not the case here. 
Once hooked to a UART interface, the camera shows it is running Hisilicon Linux and the stock firmware shows elements and firmware upgrade URLs tracing back to JoVision, a Chinese IP camera manufacturer, though no camera or even mainboard is shown on any of their global or Chinese websites matching this hardware (deepening the mystery of who actually manufactures or sells these devices).

Given it's common architecture and components, I thought it would be an easy task to create an OpenIPC build for these cameras. 
I WAS VERY WRONG.
Creating these custom builds required intensive work on correctly building the WiFi driver and making it initialize on boot, then an exhaustive amount of research into the PTZ stepper motor to find the GPIO pins that move the pan motor and how to make it work in OpenIPC. 
There were challenges to even get OpenIPC to correctly build firmware, then issues with the correct drivers, then issues with the GPIO pin assignments - this is not an easy task. 
Once I had a working build, I thought I was in the clear - reassembling the camera and allowing it to run untouched. After several days, I noticed the camera was disconnected and found it was due to power management issues. 
Despite 16MB of SPI flash, the GK7205 is easily overwhelmed by tasks the processor handled normally on the stock firmware, including SD card recording, OSD subtitles and especially WiFi connection on boot. 

These builds represent some radical adaption from stock firmware and special coding and translation to run in a stable way. This is my first time ever creating a build or doing this kind of work. This work will be continuously updated until a true solution to the power management issues are found and the camera is successfully disconnected from parasitic Chinese cloud services and Onvif compatible so it can truly be used in a safe way. 
