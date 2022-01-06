# Thaw Frost Tower's Entrance

This challenge is to use the Wifidongle in our tools to turn up the thermostat and thaw out the door to Jack Frost's mansion

It is suggested that we use iwlist and iwconfig for this.  

My first action is to run a iwlist scan.  

```
iwlist scan
```
![image](https://user-images.githubusercontent.com/54121441/148133204-f4cb553f-ab39-4909-afe6-c1021b9cecd9.png)

This returns some info including the ESSID: FROST-Nidus-Setup
I also took note of the interface: wlan0

Next, I used iwconfig to connect to the access point

```
iwconfig wlan0 essid "FROST-Nidus-Setup"
```

We are returned a message and a URL that we can curl
![image](https://user-images.githubusercontent.com/54121441/148457034-5a67f41d-9773-4ba7-a8e9-14e718d7ee35.png)

```
curl http://nidus-setup:8080/
```

![image](https://user-images.githubusercontent.com/54121441/148457304-d1b399b3-fcf9-44f5-909b-01977bd83f23.png)

We have may need to register the thermostat, but lets check the apidoc.

![image](https://user-images.githubusercontent.com/54121441/148458071-c8c8c3d4-c79b-4324-ab0b-081361ab8f58.png)

The api to change the temperature does not require registering the device so we do not need to worry about that.
Lets test it out. 

First, lets test getting the temperature via api
```
curl -XGET http://nidus-setup:8080/api/cooler
```
![image](https://user-images.githubusercontent.com/54121441/148459372-ce335de5-2ec6-4d45-9f16-731dcb47881f.png)

That worked.  Now for our end goal, turning up the temperature to thaw out Frost Tower's front door.  I am going to shoot for above 0 degrees, as that is specifically what Jack warns against doing in the api.

```
curl -XPOST -H 'Content-Type: application/json' \
  --data-binary '{"temperature": 20}' \
  http://nidus-setup:8080/api/cooler
```

![image](https://user-images.githubusercontent.com/54121441/148459670-17229845-5e47-4e9a-acaf-887a4fea6438.png)

The Ice has melted!  We have completed the challenge and can now enter Frost Tower.
