# bug2_5.6.19
A kernel bug in dvb_usb_device_init
Due to the specific malformed usb descriptor file, the linux kernel is unable to recognize the usb device correctly, thus matching the wrong device driver, which further prevents the kernel from allocating memory correctly for the usb device, and ultimately leads to a crash of the linux kernel. The function dvb_usb_device_init in drivers/media/usb/dvb-usb/dvb-usb-init.c did not check whether the value of the incoming parameter struct dvb_usb_device_properties *props was valid, and assigned a malformed value to udev when allocating heap space, causing heap overflow and kernel crash.
 ![image](https://github.com/wanrenmi/bug2_5.6.19/assets/42407501/f36ee44c-8de5-47fb-88cd-6e25534ffad1)
The following is the input usb descriptor file:
 ![image](https://github.com/wanrenmi/bug2_5.6.19/assets/42407501/6c6487ba-471f-43a1-9c20-a3c1211089d1)

Among them, the first 18 bytes are the device descriptor, and the latter is the config descriptor. Insert the above file as a real or simulated USB device into a host using the linux kernel (It is currently certain that kernel version <=5.6.19. The result of the vulnerability triggering situation is shown in the figure below:
![image](https://github.com/wanrenmi/bug2_5.6.19/assets/42407501/6a3a98ae-ab9f-4cce-b58c-a38aff21eb20)
![image](https://github.com/wanrenmi/bug2_5.6.19/assets/42407501/7408611e-0e30-45cc-bdfe-54b9c6d5b2f6)
