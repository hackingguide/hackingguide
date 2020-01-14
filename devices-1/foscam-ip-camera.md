---
description: >-
  This post is about a vulnerability assesment/analysis of a Foscam PT-series
  IP-camera.
---

# Foscam IP-camera

## Device specifications

| Type | Specification |
| :--- | :--- |
| Device name | Foscam FI9816P IP-camera |
| UID | 2RRH79GIM6GU4TYQZZZZEY2Z |
| MAC ID | 00626EE939C1 |
| System firmware version | 1.12.5.4 |
| Application firmware version | 2.81.2.35 |
| Plug-in version | 5.1.0.12 |



## Findings

During the installation process it becomes clear that these devices still come with a default username and password \(see image below\).

  ![](../.gitbook/assets/testnote.jpg)

After the installation is complete, either by setting the camera up over wifi or via cable, we are able to reach the IPCam Client. Here, we are greeted by a login form. 

![](../.gitbook/assets/ipcamclient.png)

Ofcourse, there is no HTTPS available. Lets utilize wireshark and see if we can capture some interesting data. By setting a display filter we can solely focus on the network traffic between us and the IPCam client.

![](../.gitbook/assets/1.png)

Now, when we submit a login request, we will be able to see the entire http request. By using the 'find packet' option, we can look for certain keywords in the packet bytes.

![](../.gitbook/assets/2%20%281%29.png)

The keyword 'username' gets found in a packet with the Push flag \(PSH, ACK\). When following the TCP stream we can see our request quite clearly.

```text
GET / HTTP/1.1
Host: 192.168.178.40:88
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://192.168.178.40:88/
Accept-Encoding: gzip, deflate
Accept-Language: nl-NL,nl;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: language=ENU; userName=Sec09; remenber=; pwd=
If-None-Match: "323347221"
If-Modified-Since: Mon, 17 Sep 2018 01:32:05 GMT
```

Despite the fact that the client uses HTTP, we're still not able to catch the complete login credentials. It seems like there is javascript present which scrambles the user input before completing the request. This does make it harder to retrieve a victims login credentials but not impossible. A possible method could be a Man in the middle attack with an exact copy of the client's login form but with the javascript function\(s\) taken out. Another method could be injecting javascript using mitm proxy.



