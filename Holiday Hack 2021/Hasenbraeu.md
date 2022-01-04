# Hasenbraeu - IPv6 Sandbox
This challenge is to find a lost password, saved improperly like a sticky note, somewhere on a remote machine.  
The hosts are using IPv6 to make the challenge more interesting.

The IPv6 syntax sheet provided with the challenge: https://gist.github.com/chriselgee/c1c69756e527f649d0a95b6f20337c2f



My First step was to identify machines I can access.  I used this ping6 command
```
ping6 ff02::1 -c2
```
![image](https://user-images.githubusercontent.com/54121441/148121727-2bab6caf-125f-4292-aa6d-4458910df92a.png)

Next I will run some nmap scans.  Since curl is an available tool, I image I will find a web server on one of the machines.

I'll show some example commands, and then screenshots of my actual commandssince with this challenge I had trouble copying from the terminal.  The syntax I used for the nmap scans are: 
```
nmap -T4 -6 2001:7b8:666:ffff::1:42%eth0 
```
And the Results: 

![image](https://user-images.githubusercontent.com/54121441/148122907-fcc08f58-3c40-41bd-832e-e41b8f509a9c.png)


My third nmap scan identified an http server, lets use curl with syntax from the IPv6 sheet:

```
curl http://[2001:7b8:666:ffff::1:42]:8080/ --interface eth0
```
We got a response:

![image](https://user-images.githubusercontent.com/54121441/148123538-c55d31db-e2ab-47e1-8632-bfcf3abe7d52.png)

Lets connect to that other port, cslistener at port 9000.
```
nc -6 2001:7b8:666:ffff::1:42%eth0 9000
```

Looks like we recieved the password
![image](https://user-images.githubusercontent.com/54121441/148124403-2a5a675b-0f9b-45b6-bc44-1f3ab8cc73ee.png)

