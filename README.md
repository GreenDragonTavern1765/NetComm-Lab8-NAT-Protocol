# Lab 8 - NAT Protocol
## Definitions
**NAT Protocol (Network Address Translation)** - a process in which one or more local IP address is translated into one or more Global IP Address and vice versa in order to provide Internet access to the local hosts. Also, it does translations of port numbers, i.e. masks the port number of the host with another port number, in the packet that will be routed to the destination.

## Answers Part 1
Open the **NAT_home_side** file and answer the following questions.  You might find it useful to use a Wireshark filter so that only frames containing HTTP messages are displayed from the trace file. 
  1. What is the IP Address of the client?

  ![image](https://user-images.githubusercontent.com/89669624/141239028-dca2e3cc-dfca-45b2-93d0-1df58341287f.png)
Client IP Address = 192.168.1.100

  2. The client actually communicates with several different Google servers in order to implement "safe browsing." The main Google server that will serve up the main Google web page has IP address 64.233.169.104. In order to display only those frames containing HTPP messages that are sent to/from this Google server, enter expression **"http && ip.addr == 64.233.169.104"** into the filter.

  ![image](https://user-images.githubusercontent.com/89669624/141239709-c550b5da-77f5-4360-91b0-cae60d821092.png)

  3. Consider now the HTTP GET sent from the client to the GOogle server (whose IP address is IP address 64.233.169.104) at time 7.109267. What are the source and destination IP addresses and TCP source and destination ports on the IP datagram carrying this HTTP GET?

  ![image](https://user-images.githubusercontent.com/89669624/141240132-9423573a-c7b8-4069-b6ec-4a15eeced206.png)

Source (IP Address = 192.168.1.100, Port Number = 4335): Destination (IP Address = 64.233.169.104, Port Number = 80)

  4. At what time is the corresponding 200 OK HTTP message received from the Google server? What are the source and destination IP addresses and TCP rouce and destination ports on the IP datagram carrying this HTTP 200 OK message?

  ![image](https://user-images.githubusercontent.com/89669624/141245806-9b078259-717f-4b6a-be96-f608aaa77f0c.png)

Time = 7.685786

Source (IP Address = 64.233.169.104, Port Number = 80): Destination (IP Address = 192.168.1.100, Port Number = 4335)

  5. Recall that before a GET command can be sent to an HTTP server, TCP must first set up a connection using the three-way SYN/ACK handshake. At what time is the client-to-server TCP SYN segment sent that sets up the connection used by the GET sent at time 7.109267. What are the source and destination IP addresses and source and destination ports for the TCP SYN segment? What are the srouce and destination IP addresses and source and destination ports of the ACK sent in response to the SYN. At what time is this ACK received at the client?

**SYN Segment**

![image](https://user-images.githubusercontent.com/89669624/141246776-25bfb577-28ef-42f2-bbdf-af00e7d7dfd5.png)

Time = 7.075657

Source (IP Address = 192.168.1.100, Port Number = 4335): Destination (IP Address = 64.233.169.104, Port Number = 80)

**ACK Segment**

![image](https://user-images.githubusercontent.com/89669624/141247230-c9f5f999-7761-4bd4-b367-a4aac23bb1d8.png)

Time = 7.108986

Source (IP Address = 64.233.169.104, Port Number = 80): Destination (IP Address = 192.168.1.100, Port Number = 4335)

## Answers Part 2
Open the NAT_ISP_side.  Note that the time stamps in this file and in NAT_home_side are not synchronized since the packet captures at the two locations shown in Figure 1 were not started simultaneously. (Indeed, you should discover that the timestamps of a packet captured at the ISP link is actually less that the timestamp of the packet captured at the client PC)

  6. In the NAT_ISP_side trace file, find the HTTP GET message that was sent from the client to the Google server at time 7.109267 (where t = 7.109267 is time at which this was sent as recorded in the NAT_home_side trace file). At what time does the message appear in the NAT_ISP_side trace file? What are the source and destination IP addresses and TCP source and destination ports on the IP datagram carrying this HTTP GET? Which of these fields is the same, and which are different?

Time = 6.069168

![image](https://user-images.githubusercontent.com/89669624/141248299-1fecc052-f631-4a1d-9d6f-40665a679e90.png)

Source (IP Address = 71.192.34.104, Port Number = 4335): Destination (IP Address = 64.233.169.104, Port Number = 80), where the port numbers are the same, however the source IP Address has changed, as this is the ISP's address now and not the client.

  7. Are any fields in the HTTP GET message changed? Which of the following fields in the IP datagram carrying the HTTP GET are changed: Version, Header Length, Flags, Checksum. If any of these fields have changed give a reason stating why this field needed to change.

  8. In the NAT_ISP_side trace file, at what time is the first 200 OK HTTP message received from the Google server? What are the source and destination IP addresses and TCP source and destination ports on the IP datagram carrying this HTTP 200 OK message? Which fields are the same and which are different?

Time = 6.117570

![image](https://user-images.githubusercontent.com/89669624/141249306-4ec037cf-c52c-4305-a44c-83238a25c8b8.png)

Source (IP Address = 64.233.169.104, Port Number = 80): Destination (IP Address = 71.192.34.104, Port Number = 4335). The destination IP address has changed, as this is the ISP's IP Address and not the client as in the previous section.

  9. In the NAT_ISP_side trace file, at what time were the client-to-server TCP SYN segment and the server-to-client TCP ACK segment corresponding to the segments in question 5 captured? What are the source/destination IP addresses and source/destination TCP port numbers? Which were the same, and which changed?

**SYN Segment** Time = 6.035475

![image](https://user-images.githubusercontent.com/89669624/141249664-0fd8c64f-07e5-40fd-83f3-283762fdcf2c.png)

Source (IP Address = 71.192.34.104, Port Number = 4335): Destination (IP Address = 64.233.169.104, Port Number = 80). The only thing changing is the client is now 71.192.34.104, as the ISP is now retrieving the data

**ACK Segment** Time = 6.067775

![image](https://user-images.githubusercontent.com/89669624/141249727-c7b0d312-404c-4675-9135-7ec6e1d417b6.png)

Source (IP Address = 64.233.169.104, Port Number = 80): Destination (IP Address = 71.192.34.104, Port Number = 4335). The only thing changing is that the ISP (71.192.34.104) is now acting as the client.
