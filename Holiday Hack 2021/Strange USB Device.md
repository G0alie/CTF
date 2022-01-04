# Strange USB Device

This challenge is to identify the user associated with the USB rubber ducky attack.  We are provided a python tool called Mallard to assist with reverse engineering of the ducky script.

The USB device is mounted to /mnt/USBDEVICE.

I used the following Mallard command to investigate the script, by default this script is called inject.bin.  IT returned some strings including a base64 decode bash command in the output

```
python3 mallard.py --file /mnt/USBDEVICE/inject.bin
```
I copied that bash command into my terminal and decoded the base64 to find the username in the encoded command.

![image](https://user-images.githubusercontent.com/54121441/148126545-bf00523e-8c38-4928-9d4c-678417009fa2.png)
