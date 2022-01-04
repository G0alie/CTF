# Thaw Frost Tower's Entrance

This challenge is to use the Wifidongle in our tools to turn up the thermostat and thaw out the door to Jack Frost's mansion

It is suggested that we use iwlist and iwconfig for this.  

My first action is to run a iwlist scan.  

```
iwlist scan
```
![image](https://user-images.githubusercontent.com/54121441/148133204-f4cb553f-ab39-4909-afe6-c1021b9cecd9.png)

This returns some info including the ESSID: FROST-Nidus-Setup
Ialso took note of the interface: wlan0

Next, I ran checked the man page and ran iwconfig on wlan0 for somee more info 

```
iwconfig wlan0
```

![image](https://user-images.githubusercontent.com/54121441/148133318-20cbc1c4-ad45-48c6-8082-f6ff9673f0a6.png)
