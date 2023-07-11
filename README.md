# Teh Codes

  // Gists  
  
 https://gist.github.com/timsonner  
 
 // New Gist

 // Generate shellcode for windows 64  

     msfvenom -p windows/x64/exec CMD=calc.exe -f raw -o shellcode.bin
 
 https://gist.github.com  
 
 // Find processes running on a port - Windows  
 
     netstat -ano | findstr <port_number> 
    
 // Get info of process - Windows  
 
     tasklist /FI "PID eq <process_id>"  
     
 // Get location of process - Windows  
 
     wmic process where ProcessId=<process_id> get ExecutablePath
 
 // List domain users - Windows  
 
     net user /domain
 
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
     
 // Set primary and secondary DNS - Windows
 
     Get-NetAdapter  
     Set-DnsClientServerAddress -InterfaceIndex <Interface Number> -ServerAddresses ("8.8.8.8","8.8.4.4")  
     
 // Join to domain - Windows
 
     Add-Computer -DomainName <domain name>
     
 // Get current domain - Windows
 
     (Get-WmiObject Win32_ComputerSystem).Domain

 // Network connections dialog - Windows
 
     ncpa.cpl
     
 // Advanced Rename - Windows
 
     sysdm.cpl  
 
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
     
 // Firefox for Windows: 
 
    curl -L -o firefox-latest.exe "https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=en-US"  
    
 // Windows key from firmware/BIOS - Linux 

    sudo strings /sys/firmware/acpi/tables/MSDM
    
 // Windows key from Registry - Windows 11

    Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ClipSVC\"

 // Windows key from Registry - Windows 10  
 
    Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform"

 // Add current user to vboxusers group - Linux

    sudo usermod -a -G vboxusers $USER

 // DefectDojo Setup - Linux

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
    
 // OpenVAS - Kali  

    gvm-manage-certs -a -f  
    runuser -u _gvm -- gvmd --get-users --verbose  
    runuser -u _gvm -- gvmd --modify-setting 78eceaec-3385-11ea-b237-28d24461215b --value <user id>  
     
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

// Lokinet - Linux
 
     sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg  
     echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list 

 // Right-Click context menu - Windows 11  
 
     reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
    
 // Unzip a disc image - Linux
 
     unxz -v <Path to image>.img.xz  
 // Wine64 - MacOS
 
     brew install wine-stable
     WINEARCH=win64 WINEPREFIX=~/.wine64 winecfg  

# Nmap notes  

## Switches  

-O: Detect operating system  
-A: Aggressive  
-v: Verbose  
-vv: Double verbose  
-T\<int>: Timing  
-p \<int>: Port  
-p-: All ports  
-script=\<category or path to script>: Run script  
--script-help: Help on Script  
-Pn: Disable ping scan  
-iR \<int>: Random IP 
--script-updatedb: Update scripts.db  
--script-help "<string>": Script help  
  -f: Fragment packets  
--mtu \<int>: Maximum transmission unit size  
--data-length: Append random data to packet  
--scan-delay <time>ms: Delay between packets  
--badsum: Firewall check  
-n: No DNS resolve  
--dns-servers: Specify DNS servers  
-R: reverse-DNS lookup for all hosts (live/dead)

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
    
-PS80,443,8080
    
Script Help:  
nmap --script-help "smb-* and discovery" 

Find script:
ls -l /usr/share/nmap/scripts/*dns* 

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
      <h3 align="center">Polaroid humblebrag</h3>
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
      <h3 align="center">Polaroid humblebrag</h3>
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
      <h3 align="center">Polaroid humblebrag</h3>
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
      <h3 align="center">Polaroid humblebrag</h3>
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
