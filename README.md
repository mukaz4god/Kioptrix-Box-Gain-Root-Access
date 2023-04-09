# Kioptrix Box 
The repository demostrates how to gain root access into the kioptrix box. 
Note:  The rule of engaagement to to acquire at least two ways to gain "root" access to the target machine. Let's get started.
Username: john
Password: TwoCows2

# Walkthrough

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230748530-5a7e5599-2d8b-4191-ae5e-9dd00530a8e5.png">
<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230748567-d39b4bfa-f6ce-4fad-8482-6d6f69405cda.png">
<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230748578-2dcffeb4-bef2-40d3-a478-a0c7551905fe.png">

Since we have only been provided with login credentials for the target machine and our attempts to obtain the target's IP address have been unsuccessful, we have resorted to using our attacker machine(192.168.151.154) to try and obtain this information.

Step 1: Given that we know our own IP address which is 192.168.151.154, we have used the "netdiscover" and "arp-scan -l" commands to attempt to identify the target IP address. The results of this operation are displayed in the accompanying screenshot, and we can deduce that the target Ip address is 192.168.151.155

<img width="960" alt="image" src="https://user-images.githubusercontent.com/5961735/230748764-03667866-a449-480a-a35f-bbfc7afcf328.png">

Step 2: To gather information about the target, we have executed a Nmap scan using the command "sudo nmap -sT -A -sC -p- -oX kioptrix.xml 192.168.151.155". The various arguments used in the command include "-sT" for TCP-Connect Scan, "-A" for detecting the operating system, identifying the version, and running script scanning, "-sC" for running default scripts, "-p-" to scan all ports, "-oX" to export the output in an XML file named "kioptrix.xml," and finally the IP address of the target.
<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749058-a9eee5fe-644c-4254-8cd7-8b79f5ea967d.png">
<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749066-ca1d8334-025c-4908-94f2-5c0120e92977.png">

Step 3: From the Nmap result from step above, there are 6 open ports which are :
 Port   Protocol    Name          State   Info <br/>
 22     tcp         ssh           open    OpenSSH 2.9p2   protocol 1.99 <br/>
 80     tcp         http          open    Apache httpd 1.3.20(Unix)   Red-Hat/Linux mod_ssl/2.8.4 OpenSSL/0.9 <br/>
 111    tcp         rpcbind       open    2 RPC #100000 <br/>
 139    tcp         netbios-ssn   open    Samba smbd workgroup: 4MYGROUP <br/>
 443    tcp         https         open    Apache 1.3.20(Unix)   Red-Hat/Linux mod_ssl/2.8.4 OpenSSL/0.9 <br/>
 32768  tcp         status        open    1 RPC #100024 <br/>
 
 fot the purpose of this walkthrough, we'll be goign for the low hanging fruit which are http & Samba SMB
 
 Step 4: Run a Web Content Scanner to determine the hidden directory and this is demostrated in the below snapshot.
 <img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749424-47162c9b-d3a8-44b0-b362-d380fd81cb06.png">

 
