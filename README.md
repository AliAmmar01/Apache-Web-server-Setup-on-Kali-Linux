### Introduction:
I set up an Apache web server on Kali Linux VM to host a 'Hello World' website. Then, I accessed the website from another Kali Linux VM and sniffed the network traffic and analyzed it using Wireshark.
### Part 1:
##### Apache Web Server Set up on Kali Linux VM (Kali 2) and HTML Web Page Creation: <br>
I installed the Apache Web Server on “Kali 2” using the command: <br>
						***apt install apache2***<br>
						
![image](https://github.com/user-attachments/assets/804fdf7b-1e25-4d2a-ab37-2aa833f4a088)

The Apache service was already installed so the output is as shown in the previous screen capture.

![image](https://github.com/user-attachments/assets/e353aefa-6b18-4320-a7fe-d1239b22edbf)


I then started the Apache Web service using the command: <br>
						***service apache2 start*** <br>
and using the command: <br>
						***service apache2 status*** <br>
						
we can notice that the service is up and running. <br>
I then created a new .html file in the /var/www/html directory and named it “helloworld.html”.

![image](https://github.com/user-attachments/assets/3bbe2f5d-e4fd-4d5f-9070-f2b3c496af36)

And running the following command to test if it’s correctly working: <br>
						***firefox localhost/helloworld.html*** <br> 

![image](https://github.com/user-attachments/assets/0c566141-5113-4a9d-8e93-266b01ff6f8e)

And yes, it is working.

### Part 2:
##### Accessing the Website from Kali Linux VM (kali linux 1): <br>
Given that both machines are on the same network, we can access the webpage using the IP address of the VM on which the web service is running (Kali 2).
First, let’s find the IP address of the Kali Linux VM (Kali 2) on which the webpage is hosted.
We run the command: <br>
								***ip ad*** <br>
							
![image](https://github.com/user-attachments/assets/2cb5de9c-edc5-4f7f-b708-cac3ef5742ba)

As highlighted in the previous screen capture, the IP address of Kali 2 is 192.168.1.55.
I then accessed the “helloworld.html” webpage from the other Kali Linux VM (kali linux 1) as shown in the following screen capture: <br>
						***firefox 192.168.1.55/helloworld.html*** <br>

![image](https://github.com/user-attachments/assets/658d3f24-2fd6-45e4-ac78-5f1bec85f516)

### Part 3:
##### Capturing Network Traffic using Wireshark and Analyzing HTTP Protocol:
Simultaneously, I ran Wireshark on the VM through which I was making the request for the “helloworld.html” webpage.

![image](https://github.com/user-attachments/assets/9dd0d7a8-272b-40a9-8aa3-9893d434649a)

Specify the interface as “eth0” and start sniffing.

![image](https://github.com/user-attachments/assets/9b6cb838-e372-46f8-b066-4f95b26fa181)

After getting a response for the initial request for “helloworld.html”, I stopped sniffing and started analyzing the network traffic.
I specified the capture filter: http.request.method == “GET” what lead me to the following HTTP 
Stream, found entirely here.

![image](https://github.com/user-attachments/assets/a863d5cb-0d5b-4c9f-ac44-e53d9a792e27)

![image](https://github.com/user-attachments/assets/1b39ff34-95a1-424b-85a1-e31090ecbdd7)

Starting with the analysis of the header of the GET Request for the “helloworld.html”:
<ul>
<li>The first line indicates the request method (GET), the requested resource (/helloworld.html) and the HTTP version (HTTP/1.1).
<li>The Host field indicates the IP address of the web server hosting the webpage, in this case 192.168.1.55.
<li>The User-Agent field provides information about the client, such as the browser, operating system, and version.
<li>The Accept filed indicates the types of media (content types) that the client can process or understand, in this case, text/html.
<li>The Accept-Language field in an HTTP request header specifies the preferred language(s) for the response content, in this case, English.
<li>The Accept-Encoding field indicates the compression algorithms supported by the client.
<li>The Connection field manages the connection behavior between the client and server.
<li>The Upgrade-Insecure-Requests field is a security measure that a client includes to indicate a preference for a secure (HTTPS) connection, a value of 1 as in this case indicates that the browser prefers a secure HTTPS connection.
</ul>
Moving onto the HTTP Response to the prior GET Request:
<ul>
<li>The first line indicates the HTTP version (HTTP/1.1) and the status code (200 OK means that the request is valid and was processed correctly and will get responded to accordingly).
<li>The Date field indicates the date and time of the transmission of the HTTP Response.
<li>The Server field indicates the type and version of the HTTP server on which the webpage is hosted, in this case, Apache 2.4.58 is the version of the server just like we set it up.
<li>The Last-Modified field indicates the date and time when the requested resource was last modified on the server.
<li>The ETag (Entity Tag) field provides a unique identifier for a specific version of a resource on the server.
<li>The Accept-Ranges field indicates whether the server supports byte-range requests for a particular resource, and, in this case, it does.
<li>The Content-Length field indicates the size of the payload (body) in octets/bytes being sent to the client, 21 is its value in our case.
<li>The Connection field indicates the desired connection behavior between the client and the server, in the case of keep-alive, the connection should be kept open for multiple requests.
<li>The Content-Type field indicates the type of data or media contained in the response body, in this case, text/html.
<li>And finally, the response body: <!--<h1>Hello World</h1>--> which is then rendered in the browser.
</ul>
