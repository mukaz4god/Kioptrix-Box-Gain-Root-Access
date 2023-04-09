# Kioptrix Box 
The repository demostrates how to gain root access into the kioptrix box. 
Note:  The rule of engaagement to gain "root" access to the target machine. Let's get started.
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

Step 3: From the Nmap result from step above, there are 6 open ports which are :<br/>
 Port   Protocol    Name          State   Info <br/>
 22     tcp         ssh           open    OpenSSH 2.9p2   protocol 1.99 <br/>
 80     tcp         http          open    Apache httpd 1.3.20(Unix)   Red-Hat/Linux mod_ssl/2.8.4 OpenSSL/0.9 <br/>
 111    tcp         rpcbind       open    2 RPC #100000 <br/>
 139    tcp         netbios-ssn   open    Samba smbd workgroup: 4MYGROUP <br/>
 443    tcp         https         open    Apache 1.3.20(Unix)   Red-Hat/Linux mod_ssl/2.8.4 OpenSSL/0.9 <br/>
 32768  tcp         status        open    1 RPC #100024 <br/>
 
We are able to determine the web server name, version and OS of the target, and we need to figuare the Samba version and for the the purpose of this walkthrough, we'll be going for the low hanging fruit which is the http or Samba SMB(Note: the version need to be determined).
 
 Step 4: Run a Web Content Scanner to determine the hidden directory and this is demostrated in the below snapshot.
 <img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749424-47162c9b-d3a8-44b0-b362-d380fd81cb06.png">

We have successfully identified some hidden web pages and have also discovered that the web server information is being displayed, this information on the server needs to be secured and hardened to prevent potential attacks.

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749516-452522bb-8364-4c87-b094-2c9e3b2ce5fb.png">
<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749527-3b8777bb-99a1-4ad6-ae76-33ae4fd0da72.png">

Step 5: Let first determine the Samba version and try to exploit it. Launch Meterspolit to research the smb version.

<img width="920" alt="image" src="https://user-images.githubusercontent.com/5961735/230749619-df50d2dc-4b3e-4f4b-8462-88b47f6ab1f4.png">

Search for smb and look for auxiliary module that scan for smb version .
<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749872-c62265b6-4a76-48cf-ac7e-e9e6196d49ee.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749876-40537a0c-3219-4281-90fd-ed280c6811b7.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749881-beb05980-c224-4237-a91d-7f8f7da619dd.png">

As you can see from the image above, we are able to detect the version of the smb running on the target machine. So let search for the exploit by using google or metasploit. 

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230749972-816a299d-d518-4599-ac54-873f61825b8d.png">

We discover the name of the vulnerability which is trans2open and also find a Rapid7 exploit which we are going to use to exploit the target vulnerability.

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230750055-337225a5-345f-4cc9-b8b7-abf319d8cc37.png">

So, the next screenshot are going to demostration the steps used to exploit the Samba vulnerability in order to get "root" access to our target.

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230750149-ac5b60a1-e2fe-4eaa-97f5-52a1ab1ea684.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230750156-688fe9b6-a4db-4839-b501-7ff163b6f574.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230750163-40e90ab8-6eb6-4c9c-b569-95c73d1786bf.png">

You could see that after running the exploit, the meterpreter session keep closing and Dried so we need to change our payload and run the exploit again and this is shown in the next screenshots

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230751458-d2dee3bd-2d40-4dd6-a987-8bff361582e2.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230751465-137b417e-2e9d-44d9-a425-d4013bf1cd08.png">

We are able to get a reverse shell and we gain "root" access to the target. This is displayed in the image below.

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230751555-749911c1-2b81-40f5-a154-7bd07bd4c98b.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230751564-141ae84d-614a-445d-8156-7bba5aff305a.png">


# Exploiting Mod_ssl/2.8.4 using Manual Exploitation

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753164-176bdf02-900f-471f-9496-fe38a0e9613c.png">

Step 1: Search for the Vulnerability using google. We decided to go with the HeltonWenik/Openluck github repo and follow the steps as how in the below screenshots

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753233-21046e51-a751-4fa7-b364-fa0e2010e4b7.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753240-f293ee76-bde9-45f3-9c53-071381968a87.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753249-7fd0cc49-a1da-408d-9fd6-d67e5c5c2306.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753252-bd155cef-33cd-4a30-bc56-cfcfbf62d828.png">

Step 2: The OpenLuck was complied and the payloads where extracted as shown below:

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753655-c7c2d321-f4d6-4a0c-82dc-78521e6bce06.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753664-17c9c3d8-6f86-4782-a203-5e8f6d97dfe3.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753675-8ee857ce-101c-49c2-a10b-99356ee1c6c0.png">

Step 3: As we already know that the target web server is running RedHat Linux with Apache server version is 1.3.20, you can use utilized either the 0x6a or 0x6b binary payloads to exploit the Mod_ssl/2.8.4 Vulnerabilities, and this is demostrated in the below screenshots.

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753918-5bf3ff61-e2f4-4832-9011-089d3d3fc0f8.png">

<img width="720" alt="image" src="https://user-images.githubusercontent.com/5961735/230753928-12c0b6d7-c028-4767-aad5-762450812603.png">

Step 4: Congratulations, We are able to gain "root" access to the target machine as seen above using the manual method with HeltonWenik/Openluck open-source tools.






























