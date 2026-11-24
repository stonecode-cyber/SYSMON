 Sysmon a tool used to monitor and log events on Windows, is commonly used by enterprises as part of their monitoring and logging solutions. Part of the Windows Sysinternals package, Sysmon is similar to Windows Event Logs with further detail and granular control.

Sysmon ince installed become a resident across system reboots to monitor and log system activity to the Windows event log. It provides detailed information about process creations, network connections, and changes to file creation time.

Sysmon gathers detailed and high-quality logs as well as event tracing that assists in identifying anomalies in your environment. Sysmon is most commonly used in conjunction with security information and event management (SIEM) system or other log parsing solutions that aggregate, filter, and visualize events. When installed on an endpoint, Sysmon will start early in the Windows boot process. In an ideal scenario, the events would be forwarded to a SIEM for further analysis. 
 Sysmon includes 29 different types of Event IDs, all of which can be used within the config to specify how the events should be handled and analyzed. 

**Event ID 1: Process Creation**:This event will look for any processes that have been created. You can use this to look for known suspicious processes or processes with typos that would be considered an anomaly.

**Event ID 3: Network Connection**:The network connection event will look for events that occur remotely his will include files and sources of suspicious binaries as well as opened ports.

**Event ID 7: Image Loaded**: This event will look for DLLs loaded by processes, which is useful when hunting for DLL Injection and DLL Hijacking attacks. It is recommended to exercise caution when using this Event ID as it causes a high system load. This event will use the Image, Signed, ImageLoaded, and Signature XML tags.

**Event ID 8: CreateRemoteThread** The CreateRemoteThread Event ID will monitor for processes injecting code into other processes. The CreateRemoteThread function is used for legitimate tasks and applications. However, it could be used by malware to hide malicious activity. 

**Event ID 11: File Created** This event ID is will log events when files are created or overwritten the endpoint.
**Event ID 12 / 13 / 14: Registry Event**: This event looks for changes or modifications to the registry
**Event ID 15: FileCreateStreamHash**: This event will look for any files created in an alternate data stream.
**Event ID 22: DNS Event**: is event will log all DNS queries and events for analysis. 

**SYSMON INSTALLATION**

To install Sysmon, i make use of my command prompt, run it as an administrator and run the command  **_Sysmon.exe -accepteula -i ..\Configurations\swift.xml_**

**FILTERING WITH EVENT VEIWER**
To filter evnt on event veiwer, i click on the action, then filter  current log and i input the event id i want to filter, in this case i want to check the login event wkith ID 4624.

<img width="476" height="358" alt="sysmon pre 1" src="https://github.com/user-attachments/assets/427e1163-0572-4cb0-852f-ef95f4bdee8e" />

<img width="329" height="337" alt="sysmon pre 2" src="https://github.com/user-attachments/assets/d6967a50-26c5-4af7-a92a-9f7d51487edd" />

<img width="296" height="326" alt="sysmon pre 3" src="https://github.com/user-attachments/assets/b1b1953d-774d-4793-a1c5-1986972d00d2" />

**FILTERING EVENT WITH POWERSHELL**
To view and filter events with PowerShell we will be using Get-WinEvent along with XPath queries. We can use any XPath queries that can be found in the XML view of events. We will be using wevutil.exe to view events once filtered.

**### SYSMON INVESTIGATION**

**Investigation 1 : In this investigation, your team has received reports that a malicious file was dropped onto a host by a malicious USB. They have pulled the logs suspected and have tasked you with running the investigation for it.
What is the full registry key of the USB device calling svchost.exe in Investigation 1?**

During analysis of the extracted event logs,  i identified two instances of Sysmon Event ID 13 (Registry Value Set)  which signifies  registry modifications  that are directly linked to the dropped payload originating from the malicious USB device.

<img width="742" height="294" alt="sysmon unvestigation 1" src="https://github.com/user-attachments/assets/17e0a23c-6a29-46a0-a14c-769bb240e0ca" />

These reveal the registry key to be : **_HKLM\System\CurrentControlSet\Enum\WpdBusEnumRoot\UMB\2&37c186b&0&STORAGE#VOLUME#_??_USBSTOR#DISK&VEN_SANDISK&PROD_U3_CRUZER_MICRO&REV_8.01#4054910EF19005B3&0#\FriendlyName_**

**What is the device name when being called by RawAccessRead in Investigation 1?**

In the log, i discovered a raw accessRead in the log with Event ID 9. i analyse the log with XML view to view the device name to be  **_Device\HarddiskVolume3_**  as shown in the screenshot below:
<img width="485" height="371" alt="sysmon investigastion task 2" src="https://github.com/user-attachments/assets/171567f5-04ed-4352-bdda-f72a0596e3cb" />

**What is the first exe the process executes in Investigation 1?**

The first event In the extrated log, which is the proccess-terminated with EvenT ID 5, WHICH  i think indicate a file execusion. I open the log with XML view to confirm the file excuted to be **_rundll32.exe_** as shown in the screenshot below:

<img width="487" height="374" alt="SYsmon investigation task 3" src="https://github.com/user-attachments/assets/4040bcb5-ae5c-4939-abd4-970af7de9092" />

**What is the full path of the payload in Investigation 2?**

I open  the extracted log of investigation 2, analys the events and reveal the path of the payload to be :
**_C:\Users\IEUser\AppData\Local\Microsoft\Windows\Temporary Internet Files\Content.IE5\S97WTYG7_**

What is the full path of the file the payload masked itself as in Investigation 2?

I opened the extrated log of the second investigation for analysis to find out what the full path of the masked payload:

<img width="488" height="381" alt="sysmon investigation task 5" src="https://github.com/user-attachments/assets/65245a12-c23b-49f3-a921-17c042be8c3d" />

**What signed binary executed the payload in Investigation 2 ?**

The second event in the log, with event ID 3 Network connection detection  the adversaries ip address is  **_10.0.2.18_** 

<img width="493" height="370" alt="sysmon investigation task 6" src="https://github.com/user-attachments/assets/11501d0a-ee9f-4fbd-8a8a-366e8b3fcaec" />

What back connect port is used in Investigation 2?

The connection port as revealed innthe log is 4443 as shown in  the screenshot below:

<img width="496" height="377" alt="Sysmon investigation task 8" src="https://github.com/user-attachments/assets/b42f47e6-e9ab-4cfa-a8b3-67ebd36c929a" />

**What is the IP of the suspected adversary in Investigation 3.1?**
The Ip adrress of the adversary is **_172.30.1.253_**

<img width="476" height="372" alt="sysmon investigation task 9" src="https://github.com/user-attachments/assets/c5eb7659-ddf3-42df-bf14-2dc350dfd746" />


**What is the hostname of the affected endpoint in Investigation 3.1?**

<img width="479" height="362" alt="System investigation task 10" src="https://github.com/user-attachments/assets/db0c85e8-676a-44e2-9933-e7bfb0a3782c" />

**What is the hostname of the C2 server connecting to the endpoint in Investigation 3.1?**

<img width="491" height="368" alt="SYSMON INVESTIGATION TASK 11" src="https://github.com/user-attachments/assets/edbc0b9f-8ff2-4636-b849-87adc1e8832a" />

**What PowerShell launch code was used to launch the payload in Investigation 3.1?**

analysing the event ID 13,  we have only 2 and closely analhysis indicated that the first event is used to execute the paylod i as shown in the screenshot below:

<img width="487" height="382" alt="sysmon task 12" src="https://github.com/user-attachments/assets/f561b891-0269-47cd-867d-2ef70c3231b8" />
The  code  **_"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -c "$x=$((gp HKLM:Software\Microsoft\Network debug).debug);start -Win Hidden -A \"-enc $x\" powershell";exit;_**

**What is the IP of the adversary in Investigation 3.2?**

in the extracted log, since i am alooking for ip addrress, i consider event wit**_h Network connection detection_** with **_event ID 3_** which reveal the detination IP address  to be **_172.168.103.188_** as shown in the screenshot below:

<img width="480" height="362" alt="SYSMON TASK 14" src="https://github.com/user-attachments/assets/cac280a9-09a8-43fb-97ac-c87f37869271" />

**What is the full path of the payload location in Investigation 3.2?**

<img width="487" height="375" alt="SYSMON TASK 15" src="https://github.com/user-attachments/assets/11e5faa7-a52c-45b8-a4ec-e5ca41381f68" />

The full path of the pay load as shown above is found in  process creation with event id 1 to be **_c:\users\q\AppData:blah.txt_**

**What was the full command used to create the scheduled task in Investigation 3.2?**

I found the full command used in the  process creation evend id 1 as **_"C:\WINDOWS\system32\schtasks.exe" /Create /F /SC DAILY /ST 09:00 /TN Updater /TR "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -NonI -W hidden -c \"IEX ([Text.Encoding]::UNICODE.GetString([Convert]::FromBase64String($(cmd /c ''more < c:\users\q\AppData:blah.txt'''))))\""_**

As shown below: 

<img width="497" height="391" alt="sysmon task 16" src="https://github.com/user-attachments/assets/ce2536b6-50ac-457b-b122-d220db6a1051" />

**What process was accessed by schtasks.exe that would be considered suspicious behavior in Investigation 3.2?**

For this tsk i look at process creation in event id 10 to which reveal  the path to b**_e C:\WINDOWS\system32\lsass.ex_** but simce we only needthat process path that look suspicious **_lsass.exe (Local Security Authority Subsystem Service)_**

<img width="491" height="363" alt="sysmon tsk 17" src="https://github.com/user-attachments/assets/acbf96ae-9ec7-427a-95e9-9770cbb88d66" />

**What is the IP of the adversary in Investigation 4?**

<img width="484" height="359" alt="sysmon task 18" src="https://github.com/user-attachments/assets/edd6c05b-6ad2-4137-bc2c-951461a5c678" />

**What port is the adversary operating on in Investigation 4?**

<img width="417" height="367" alt="sysmon task 19" src="https://github.com/user-attachments/assets/34e6544a-1f69-4204-bdfe-c58201a14a24" />

In the same network connection detection event id 3 we can finf the port 80 HTTP as the connectying port for the adversary.








