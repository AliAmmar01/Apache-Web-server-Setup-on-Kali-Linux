## Introduction:
I set up an Apache web server on Kali Linux VM to host a 'Hello World' website. Then, I
accessed the website from another Kali Linux VM and sniffed the network traffic and analyzed it using Wireshark.
#### Task 1:
Apache Web Server Set up on Kali Linux VM (Kali 2) and HTML Web Page Creation:
I installed the Apache Web Server on “Kali 2” using the command: <br>
&emsp; &emsp;					***apt install apache2*** <br>
						
![image](https://github.com/user-attachments/assets/804fdf7b-1e25-4d2a-ab37-2aa833f4a088)

The Apache service was already installed so the output is as shown in the previous screen capture.


I then started the Apache Web service using the command: <br>
						***service apache2 start***
and using the command: <br>
						***service apache2 status***
						
we can notice that the service is up and running.
I then created a new .html file in the /var/www/html directory and named it “helloworld.html”.

![[Pasted image 20241009235725.png]]

And running the following command to test if it’s correctly working:
						***firefox localhost/helloworld.html***

![[Pasted image 20241010000108.png]]
And yes, it is working.

#### Task 2:
Accessing the Website from Kali Linux VM (kali linux 1):
Given that both machines are on the same network, we can access the webpage using the IP address of the VM on which the web service is running (Kali 2).
First, let’s find the IP address of the Kali Linux VM (Kali 2) on which the webpage is hosted.
We run the command:
								***ip ad***
							
![[Pasted image 20241010000123.png]]

As highlighted in the previous screen capture, the IP address of Kali 2 is 192.168.1.55.
I then accessed the “helloworld.html” webpage from the other Kali Linux VM (kali linux 1) as shown in the following screen capture:
						***firefox 192.168.1.55/helloworld.html***

![[Pasted image 20241010000133.png]]

#### Task 3:
Capturing Network Traffic using Wireshark and Analyzing HTTP Protocol:
Simultaneously, I ran Wireshark on the VM through which I was making the request for the “helloworld.html” webpage.

![[Pasted image 20241010000252.png]]

Specify the interface as “eth0” and start sniffing.

![[Pasted image 20241010000259.png]]

After getting a response for the initial request for “helloworld.html”, I stopped sniffing and started analyzing the network traffic.
I specified the capture filter: http.request.method == “GET” what lead me to the following HTTP 
Stream, found entirely here.

![[Pasted image 20241010000407.png]]

![[Pasted image 20241010000414.png]]

Starting with the analysis of the header of the GET Request for the “helloworld.html”:
- The first line indicates the request method (GET), the requested resource (/helloworld.html) and the HTTP version (HTTP/1.1).
- The Host field indicates the IP address of the web server hosting the webpage, in this case 192.168.1.55.
- The User-Agent field provides information about the client, such as the browser, operating system, and version.
- The Accept filed indicates the types of media (content types) that the client can process or understand, in this case, text/html.
- The Accept-Language field in an HTTP request header specifies the preferred language(s) for the response content, in this case, English.
- The Accept-Encoding field indicates the compression algorithms supported by the client.
- The Connection field manages the connection behavior between the client and server.
- The Upgrade-Insecure-Requests field is a security measure that a client includes to indicate a preference for a secure (HTTPS) connection, a value of 1 as in this case indicates that the browser prefers a secure HTTPS connection.
Moving onto the HTTP Response to the prior GET Request:
- The first line indicates the HTTP version (HTTP/1.1) and the status code (200 OK means that the request is valid and was processed correctly and will get responded to accordingly).
- The Date field indicates the date and time of the transmission of the HTTP Response.
- The Server field indicates the type and version of the HTTP server on which the webpage is hosted, in this case, Apache 2.4.58 is the version of the server just like we set it up.
- The Last-Modified field indicates the date and time when the requested resource was last modified on the server.
- The ETag (Entity Tag) field provides a unique identifier for a specific version of a resource on the server.
- The Accept-Ranges field indicates whether the server supports byte-range requests for a particular resource, and, in this case, it does.
- The Content-Length field indicates the size of the payload (body) in octets/bytes being sent to the client, 21 is its value in our case.
- The Connection field indicates the desired connection behavior between the client and the server, in the case of keep-alive, the connection should be kept open for multiple requests.
- The Content-Type field indicates the type of data or media contained in the response body, in this case, text/html.
- And finally, the response body: <h1>Hello World</h1> which is then rendered in the browser.
