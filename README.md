<p align="center">
<img width="270" height="270" alt="image" src="https://github.com/user-attachments/assets/5d1a9b9c-283c-4440-b0aa-8a5aa57ba376" />



</p>

<h1>Practicing with DNS</h1>
In this project, my goal was to practice working with DNS records and common troubleshooting techniques. The main things I learned were how to create A and CNAME records, how to clear a local cache and get information from the actual DNS server, and how to use commands such as ping, nslookup, ipconfig, ipconfig /displaydns, and ipconfig /flushdns in Powershell. This project utilizes the VMs (dc-1 and client-1) that I created in the project titled "VM Creation in Microsoft Azure." <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS Manager
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 11 (24H2)

<h2>DNS Exercises</h2>

<p>
<img width="1095" height="636" alt="image" src="https://github.com/user-attachments/assets/5c35eda0-45f9-4d7e-8d8a-c064fdc1d6cb" />


</p>
<p>
To start, I logged into the VMs for dc-1 and client-1 as jane_admin. On dc-1, I opened Powershell and pinged "Mainframe" (just a random word). Nothing came up because the word "Mainframe" is not cached in the DNS of dc-1. I confirmed this by typing ipconfig /displaydns.
</p>
<br />

<p>
<img width="927" height="572" alt="image" src="https://github.com/user-attachments/assets/81a416fc-cfc0-4353-bd42-7e7c83dc9bc2" />


</p>
<p>
I then opened the files menu in notepad. I went to the System32 > drivers > etc folder and located the hosts file. 
</p>
<br />

<p>
<img width="1415" height="737" alt="image" src="https://github.com/user-attachments/assets/98dae576-11c9-4e51-8ef1-e09767c8aa72" />


</p>
<p>
Upon opening the file, I could view and edit a sample HOSTS file in notepad. 
</p>
<br />

<p>
<img width="1256" height="695" alt="image" src="https://github.com/user-attachments/assets/f14ad02a-8d78-4fa2-9f65-77448ccbca31" />


</p>
<p>
I then added the localhost address and the word "Mainframe" at the bottom of the hosts file (and saved it). I could now ping Mainframe in Powershell successfully. 
</p>
<br />

<p>
<img width="787" height="697" alt="image" src="https://github.com/user-attachments/assets/a345e53b-073e-437e-b469-5e7435cf5c2f" />


</p>
<p>
Next, I selected the DNS option in Windows Administrative Tools to open DNS manager. 
</p>
<br />

<p>
<img width="850" height="301" alt="image" src="https://github.com/user-attachments/assets/0d3b6cb4-ab9e-4353-8bfd-5473ba3b80ee" />


</p>
<p>
I then went to Powershell and used ipconfig to obtain the IPv4 address (needed later).
</p>
<br />

<p>
<img width="947" height="715" alt="image" src="https://github.com/user-attachments/assets/bb3c5777-3a51-4971-943b-898d08e86715" />


</p>
<p>
In DNS Manager, I opened the mydomain.com folder and right clicked to create a new host record.
</p>
<br />

<p>
<img width="936" height="650" alt="image" src="https://github.com/user-attachments/assets/3cff2b2c-5a58-40c0-964c-7c6a9bb41e41" />


</p>
<p>
When the New Host box opened, I entered "mainframe2" as the name and put in the IPv4 address from earlier. I made sure neither of the boxes were checked, then clicked the Add Host button.
</p>
<br />

<p>
<img width="927" height="647" alt="image" src="https://github.com/user-attachments/assets/07c79018-5080-45f6-8934-ad9e5dfd6e78" />


</p>
<p>
Mainframe2 was now visable in the mydomain.com folder as a Host A record.
</p>
<br />

<p>
<img width="610" height="276" alt="image" src="https://github.com/user-attachments/assets/de807da5-b2ad-4fea-815d-5462e4f2daee" />


</p>
<p>
I then went back to Powershell and pinged mainframe2. This time, dc-1 had to reach out to the actual DNS server, as apposed to using the local DNS cache or localhost file.
</p>
<br />

<p>
<img width="962" height="635" alt="image" src="https://github.com/user-attachments/assets/939d6bdb-2670-473b-a6b9-d2a78f1b0e98" />


</p>
<p>
Next, I went to the properties menu of mainframe2 in DNS Manager. I changed the previous IP address into Google's IP address (8.8.8.8) and checked the box to update the PTR record.
</p>
<br />

<p>
<img width="813" height="557" alt="image" src="https://github.com/user-attachments/assets/03e24cb4-6893-4afe-b903-69e6e7ccb730" />


</p>
<p>
I then opened the client-1 VM and waited a couple minutes (after changing the IP address on dc-1). After waiting, I opened Powershell on client-1 and pinged mainframe2, which showed the IP address 8.8.8.8 as intended. I went back to dc-1 and changed the IP address of mainframe2 back to 10.0.0.4, then quickly pinged it again on client-1. The address still came up as 8.8.8.8, meaning that client-1 was relying on information in the local DNS cache rather than the true IP address.
</p>
<br />

<p>
<img width="1090" height="627" alt="image" src="https://github.com/user-attachments/assets/de24b817-0a54-44b5-aa4d-2719bca44871" />


</p>
<p>
I closed out of Powershell on client-1 and logged back in, this time selecting the "run as administrator" option. I then used the flushdns command to clear the local DNS cache. I pinged mainframe2 after the flush and the IP address displayed correctly as 10.0.0.4.
</p>
<br />

<p>
<img width="977" height="657" alt="image" src="https://github.com/user-attachments/assets/ed74b9ba-a980-427a-ad76-42b5c33834a4" />


</p>
<p>
Next, I went back to DNS Manager in dc-1 and clicked the option to create a new CNAME file in the mydomain.com folder. I named the new record "search" and entered the FQDN for Google.com.
</p>
<br />

<p>
<img width="557" height="452" alt="image" src="https://github.com/user-attachments/assets/cd8fb64b-5582-4ce4-a1f8-1eb108826d41" />


</p>
<p>
Finally, I pinged "search" in client-1's Powershell and found the IP address 142.250.68.36 (IP address for Google.com). I also used the nslookup command which displayed 10.0.0.4 and 172.217.14.100 (another IP address for Google). I then finished by using the ipconfig /flushdns command. This concludes the DNS Practice project.
</p>
<br />


