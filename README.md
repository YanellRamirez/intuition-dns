<p align="center">
<img src="https://i.imgur.com/MwrkwEQ.png" alt="DNS Photo"/>
</p>

<h1>Understanding DNS</h1>
This lab focuses on DNS and its practical usage. DNS is a crucial concept in IT, and while many resources cover the theory behind it, this lab will give me hands-on experience configuring DNS records and seeing how they function in practice. This builds on a previous lab where I already have a client machine joined to my domain (ernestotest.com). I’m logged in as Jane Doe, an admin account, on both the client and domain controller VMs. To ensure the lab works as expected, it's important to be logged in with administrator privileges. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/5pjgtVz.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/Zl04Jyt.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/96xUgLF.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
While logged in as an admin on the client machine, open the Command Prompt. When you attempt to ping "mainframe", it will fail. Similarly, if you run nslookup "mainframe", the result will show that there is no DNS record for it. To fix this, you need to create a DNS A-record for "mainframe" on the domain controller.

Here are the steps to do that:

1.) Log in to the Domain Controller as an admin.

2.) Open DNS Manager:
- In the Server Manager, click Tools and select DNS to open the DNS Manager.

3.) Navigate to Forward Lookup Zones:
- In the DNS Manager, expand the server, and go to Forward Lookup Zones.
- Find your domain (in your case, ernestotest.com).

4.) Create a New Host (A-Record):
- Right-click on ernestotest.com and select New Host (A or AAAA).
- In the Name field, input mainframe.
- In the IP address field, enter the domain controller's IP address (this will allow "mainframe" to resolve to the domain controller).
- Make sure the option Create associated pointer (PTR) record is checked, if desired.

5.) Save the Record:
- Click Add Host, then click Done.

6.) Refresh the DNS Server:
- Right-click on your domain under Forward Lookup Zones, and select Refresh to update the DNS records.

7.) Test the DNS Resolution:
- Go back to the client VM, and try to ping "mainframe" again.
- This time, the ping should resolve successfully since the DNS A-record for "mainframe" has been created and updated.

By adding this A-record, you ensure that the client can resolve the "mainframe" hostname to the correct IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/nLlOGKl.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/p0VcajB.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/ds0SCKW.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/d4cXTCS.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
This exercise demonstrates how the DNS cache works. Here's what happens step by step:

1.) On the domain controller, the mainframe A-record's IP address is changed to 8.8.8.8 (Google's public DNS). After making this change, remember to refresh the DNS server so that the new record is updated.

2.) Testing the Change on the Client:
- When you ping "mainframe" from the client, it will still resolve to the old IP address because the DNS cache on the client still holds the old information.

3.) View the DNS Cache:
- Run the command ipconfig /displaydns in Command Prompt on the client machine.
- This will display the cached DNS entries, and you will notice that mainframe still has the old IP address.

4.) Clear the DNS Cache:
- In order to update the cache with the new IP address, run the command ipconfig /flushdns.
- This clears the DNS cache on the client machine.

5.) Ping "Mainframe" Again:
- After flushing the DNS cache, try pinging "mainframe" once more.
- This time, the ping should resolve to the new IP address (8.8.8.8) that was updated in the DNS server.

By clearing the DNS cache with ipconfig /flushdns, you're forcing the client to get the most up-to-date DNS information from the DNS server, ensuring it resolves the hostname to the correct, updated IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/rI2NYU7.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/FmVM0IU.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/dyWduSW.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
A CNAME record will now be made on the DNS server that will point "search" to Google. On the Forward Lookup Zones tab in the DNS Manager, open the tab that has the domain. Create a new CNAME record called search and point it to Google. Refresh the server to save the changes. On the client, pinging search and using nslookup will return the results of the CNAME record.
</p>
<br />

<h2>Lessons Learned </h2>

This lab helped me grasp how DNS functions on a computer and within a network. I now understand how DNS records can change, which can sometimes lead to connectivity issues if the old records are still cached. The DNS cache may still hold outdated information, and in those cases, it needs to be cleared to reflect the updated records. The ipconfig /flushdns command is frequently used as a troubleshooting tool, and I didn’t fully understand its purpose until performing this lab.

At the start of the lab, when I pinged "mainframe", the computer first checks the DNS cache. If there is no cached entry, it will then check the host file. If the host file doesn’t have an entry either, the system queries the DNS server. By creating a DNS record for mainframe on the DNS server, I ensured that the ping would successfully resolve.

Additionally, I learned about CNAME records, which create an alias for another domain name. For example, I created a CNAME record for "search" that pointed to Google. This mapping allows "search" to act as an alias for www.google.com, helping me understand the use of CNAME records for mapping domain names.
