---
layout: post
title: "How to connect VPN using Tunnelblick with one click for MacOS"
date: 2023-08-31 16:05:00 +0700
categories: TIPS
---

Hi guys, long time no see...  
In this article, i will show you a trick to `connect to VPN using Tunnelblick application with just one click`. Let's get started.  


> First of all, make sure you have installed Tunnelblick application and configured your VPN using .ovpn file. 

## Step 1: Install oauthtool.
Let's install oauthtool using Homebrew or some other way.
```shell
brew install oath-toolkit
```
Then, let's try to make sure it works well
```shell
oathtool --totp -b -d 6 [your_key]
```
Then, you have to decode your QR code to get the secrect key, you can refer to the following way:

1. Go to [https://qrcode-decoder.com/](https://qrcode-decoder.com/)
2. Then select QR code, after decoding, you will get the result like this: **otpauth://totp/XXX?secret=abcxyz**

Let's use this secret key to run oathtool:
```shell
oathtool --totp -b -d 6 abcxyz
```
the OTP will be displayed, just compare and make sure it's correct.  
## Step 2: Create file .sh and modify .ovpn
When we connect VPN, you have to fill in username and password+otp, Example:
```
username: luannt
password: luannt+OTP
```

Therefore, we need to create file .txt to store the credentials as follows:
```shell
touch vpn_credential.txt
```

Let's run the following command in order to enter your username/password into vpn_credential.txt, example on my Mac:
```shell
echo -e "luannt\nluannt`oathtool \
--totp -b -d 6 abcxyz`" > /Users/luannt/vpn/vpn_credential.txt
```

Checking vpn_credential.txt to make sure the username/password is correct, the result will be like below:
```text
luannt
luannt321557
```

Next, we will modify your **.ovpn** file that's configured on the Tunnelblick application in the following directory:
`Library/Application\ Support/Tunnelblick/Configurations/[VPN].tblk/Contents/Resources`

Let's modify **auth-user-pass** like this and don't forget to save it:

```text
...
auth-user-pass /Users/luannt/vpn/vpn_credential.txt
...
```

> Well, above step to tell VPN to read the credentials from our .txt file.

Finally, we'll create .sh file to execute a shell script that automatically opens the Tunnelblick application and connects to VPN.
```shell
touch connect_vpn.sh
```

Copy and paste the script bellow into file .sh:
```shell
#!/bin/bash
echo -e luannt\n`oathtool \
--totp -b -d 6 [key]`" > /Users/luannt/vpn/vpn_credential.txt
osascript -e "tell application \"/Applications/Tunnelblick.app\"" -e\
"connect \"Sendo\"" -e "end tell"
```

Some of the above information is described as follows:

**luannt**: username  
**key**: secret key that is decoded from QR code  
**Sendo**: VPN name that we configured, example:  

![connect vpn image](../docs/assests/tip_connect_vpn.png)

Let's execute the shell script file.
```shell
./connect_vpn.sh
```

If all goes well, the Tunnelblick application will start and automatically connect to the VPN.
## Step 3: Create icon on OSX Dock:

For this step, we will try to execute the shell script .sh from OSX Dock by:

Make your shell script executable:

```shell
chmod +x connect_vpn.sh
```

1. Rename your script to have a .app suffix:
   $ mv connect_vpn.sh connect_vpn.app
   You can change file icon that make it more excited, Let's see here: Change icons for files or folders on Mac - Apple Support 
2. Drag the script (.app) to the OSX dock.
3. Rename your script back to a .sh suffix:
4. Right-click the file in Finder, and click the "Get Info" option.
5. At the bottom of the window, set the shell script to open with the terminal.
   That's all, let's try clicking the icon on OSX Dock and enjoy the result.

# Summary
Just now, I showed you a trick to make connecting to a VPN easier and faster. Hope you will like it.  
Thanks for reading.