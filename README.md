# Security Engineering - Week 4 exercises

## Task 3: BurpSuite

### Subtask 1: Intercepting

In the screenshot below you can see the listing of the last POST and GET requests with timestamps and port, and the entire edited POST request in raw format. In particular, I modified the User-Agent by typing my name and an household item: "Martina Vianello + lamp". 
![Edited POST](./images/edited_POST.png)


### Subtask 2: Repeater

A new cookie called dvwaSession is created every time the "Generate" button is pressed. Using the repeater functionality I could see that everytime the request was sent again the value of the cookie was re-generated exploiting the time the request was sent. I can understand this by looking at the original POST request where the dvwaSession cookie has a certain value and at the response that I got after sending the request again (after some time), where the value of the cookie is simply incremented. You can see this in the screenshot below where both the request and the response are visible. 
![dvwaSession cookie](./images/dvwaSession_cookie.png)


### Subtask 3: Intruder

Out of all 24 attempts, only 2 were successful in my opinion: request 1 (admin; password) and 6 (gordonb; abc123). I ended up with this conclusion because, as you can see in the screenshots below, those two requests were the only ones with response length longer than the other ones. To confirm my theory, I looked at the response of the two requests and found the sentence "welcome to the protected area" insted of "username and/or password incorrect".
![bruteforce admin](./images/cluster_bomb_attack_admin.png)
![bruteforce gordonb](./images/cluster_bomb_attack_gordonb.png)


### Subtask 4: Decoder

The encoded and decoded results are in the screenshot below.
![decoder](./images/decoder.png)



### Subtask 5

I changed the password from "cc" to "bags" and run the command to bruteforce the password. The result of the command can be seen in this screenshot:
![bruteforce](./images/bags.png)

The whole process to run the command took 32 minutes and 40 seconds. The command I executed is the following:

docker run --rm -it vanhauser/hydra -V -f -I -l admin -x 4:4:a "http-get-form://host.docker.internal/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie:PHPSESSID=j0b4ju6sg85q93lsj4prm5kus2; security=low:F=Username and/or password incorrect"

I set both the minimum and maximum parameters to 4 because I knew the password was made of 4 letters (all lowercase). It is evident that it takes much more effort and time to bruteforce a password made of 4 letters instead of a simpler one like "cc".

