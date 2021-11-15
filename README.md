<H1 align="center"> 🥷 iOS Hacking Notes [App]</h1>
<p align="center"> A repository where I dump my iOS app hacking notes📚 - Recommended for beginners </p>
<p align="center"> Don't forget to Star 🌟 the repository and Follow for updates🙂  </p>




![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/aqua.png)

<a href="https://twitter.com/InfosecTweet">
  <img align="right" target="_blank" src="https://visitor-badge.glitch.me/badge?page_id=bugdisclose.iOS-Hacking-Notes&left_color=655BE1&right_color=green" />
</a>

<a href="https://twitter.com/InfosecTweet">
  <img align="right" alt="InfosecTweet's Twitter" target="_blank" width="22px" src="https://raw.githubusercontent.com/peterthehan/peterthehan/master/assets/twitter.svg" />
</a>

## Table of Content
* [Jailbreak](#Jailbreak)
* [SSH Setup](#open-ssh-)
* [Frida Setup](#frida-setup)
* [Objection Setup](#objection)
* [Installing Application from Untrusted Source](#installing-vulnerable-application-from-untrusted-source)
* [Basic iOS Security Testing](#basics-ios-app-security-testing)
* [Traffic Analysis iOS](#traffic-analysis-ios)
* [Local Storage Analysis](#local-storage-analyis)
* [Dumping Secrets From Keychain](#dumping-secrets-from-keychain)
* [Authorization Vulnerability](#authorization-vulnerability)
* [Insecure Loggin](#insecure-logging)
* [Sensitive Data in Pasteboard](#sensitive-data-in-pasteboard)
* [Webview XSS](#webview-xss)
* [Decrypting iOS app Downloaded from App Store](#decrypt-ios-application-downloaded-from-app-store)
* [Jailbreak Protection Bypass Using Objection](#jailbreak-protection-bypass-using-objection)

---

#### Jailbreak
Jailbreak iphone Device - use [checkra1n](https://checkra.in/)
Check if you're able to open Cydia : if yes then congrats device is jailbroken.😉

Install below tweak packages from Cydia store:
1. OpenSSH - System & Phone
2. SSL Kill Switch 2 - Phone
3. Flex 3 - Phone
4. Darwin CC tools - Phone
5. Filza file manager - Phone
6. Frida - System & Phone
7. Objection - System

#### Open SSH : 
Open ssh in sytem terminal enter your device ip like ssh root@wifiip >> Use default password : _alpine_

#### Frida Setup:
Install frida on to iphone and system both : add sourcer https://build.froda.re	
##### check frida installation

Run ```frida-ps -Uai``` : to list all the packages installed on device

#### Objection:
```objection --gadget “app_package_name_here” explore``` : it’ll open hooked application

#### Installing Vulnerable Application from untrusted source

1. Add apple account to Xcode
2. create a sample project and generate provisioning profile.
3. Install sample application 
4. Now the provisioning certificate is generated for sample project that tell that app can be installed in a particular device
5. now to locate provisioning profile nevigate to project directory > Products > file.app > show in finder > embedded.mobileprovision 
6. Copy the same file and paste in folder where untrusted application is stored.
7. Install applesign using >> ```npm install -g applesign``` command
8. List out available signing identities using ```applesign -L``` command you will get result like : 0349B1F2DB9E7E1437607230BDE43775BDB78403
9. Run: ```applesign -i 0349B1F2DB9E7E1437607230BDE43775BDB78403 app.ipa -m embedded.mobileprovision```  to sign app.
10. You’ll get another signed build of the application
11. Now again go to window and device simulator select new build to install in device.

### Basics iOS App Security Testing: 

#### Traffic Analysis iOS:
Configure proxy with burp suite and install SSL kill switch 2

#### Local storage analyis:
1. perform all the actions available in app
2. login through SSH in iOS
3. ```cd /var/mobile/Containers/Data/Application/``` list all the UUIDs available
4. ```find -type d -name cst.package``` you’ll get the location of application directory
5. Explore local storage
6. get files in local system using sftp >> sftp root@iphone_wifi >> alpine default password and then ```get /filepath/hello.db``` it will pull file in local
7. basic SQlite3 command ```sqlite3 hello.db``` >> .tables >> ```select * from tablename ;```


#### Dumping Secrets from Keychain
1. get Shell to device and create a dir ```/tmp/keychain```
2. through sftp ```put keychain_dumper /tmp/keychain```
3. run ```./keychain_dumper > keychain.txt``` and put password in device it will give text file
4. Put keychain.txt file to computer and see into vim.

### Client Side Validation

#### Authorization vulnerability 
1.  Stop the and reopen the app if its not closing session (it means session is stored locally somewhere)
2.  go to localstorage and pull the file search for tokens
3. use ```objection --gadget "cst.app" explore```
4. Try to attack token

#### Insecure logging	
1. go to the xcode
2. navigate to window > device and simulators > open console > then start activities to see logs

#### Sensitive Data in pasteboard
1. Open objection ```objection —-gadget com.package.name explore```
2. type objection terminal ```ios pasteboard monitor``` >> copy anything

#### WebView XSS 
1. Capture the request and manipulate content related to HTML

### Advance Attacks

#### Decrypt iOS application downloaded from App Store:
Whenever we download iOS app from apple store that are by default encrypted (not everything) we can decrypt the classes and read the functions & logics etc.

1. Download https://github.com/AloneMonkey/frida-ios-dump
2. edit dump.py and add your iphone ssh credentials.
3. Check the package name which you wanted to decrypt.
4. Run command ```python3 dump.py com.package_name```
5. You’ll get decrypted .ipa file.


#### Jailbreak Protection Bypass Using Objection

1. ```objection --gadget "com.package.name" explore -s "ios jailbreak disable"```
2. ```objection --gadget "com.package.name" explore```
3. ```ios jailbreak disable```
