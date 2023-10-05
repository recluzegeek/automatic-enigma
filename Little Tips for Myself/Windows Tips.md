---
title: Windows Tips
updated: 2023-06-30 15:51:10Z
created: 2022-08-23 06:25:01Z
latitude: 50.11092210
longitude: 8.68212670
altitude: 0.0000
---

## 1\. Running a Command in the Background in Windows

```
start /b program-name

// if you want to run normcap in the background
// following command will do the magic for you

start /b normcap - ocr
```

- For more info, ==**help start**==

## 2\. Disabling Normal Browser Window

- You can play around with the browser policies available at `chrome://policy` or `edge://policy`
    - One of the policy for poking our nose is and is of **DWORD** size
        `IncognitoModeAvailabilty` or `InPrivateModeAvailability` for chrome and edge respectively.
- To alter these policies, open registry editor and go to software policies
    - For edge, `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge`
    - For chrome, `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome`
- **Flags**
    - `0 -> normal behaviour`
    - `1 -> disable private window/mode`
    - `2 -> disable normal window/mode or force private window/mode`
- **Don't forget to logout and log back in to have these changes in their place**

## 3. Forcing Dark Mode on PDF
```
var cover = document.createElement("div"); 
let css = ` 
    position: fixed; 
    pointer-events: none; 
    top: 0; 
    left: 0; 
    width: 100vw; 
    height: 100vh; 
    background-color: white; 
    mix-blend-mode: difference; 
    z-index: 1; 
` 
cover.setAttribute("style", css); 
document.body.appendChild(cover);


```

## 4. Using LibreWolf Browser
- Use librewolf as an alternative to chromium-based browsers. 
- Enhances security and fool the sites that uses device fingerprinting using JavaScript 

## 5. Checking Browser Privacy
### a. CoverYourTrack[](https://coveryourtracks.eff.org/)
### b. DeviceInfo[](https://www.deviceinfo.me/)
## 6. Disable Media Controls appearing on LockScreen
### a. FireFox based browsers
- Go to `about:config` and set `media.hardwaremediakeys.enabled` to `false`
### b. Chromium based Browsers

## 6. Instant File Searching Application
-	Everything
## 7. Custom Windows ISO 
-	Download windows iso directly from the microsoft server without installing any shitty software, by opening the site in any operating system other than windows. This will provide you direct link to the ISO file.
-	MSMG Toolkit
-	Component Removal Windows
-	or search "Windows 10 custom iso"

## 8. Security without Borders ([Hardening Tools](https://www.securitywithoutborders.org/blog/2020/07/06/hardentools-2-is-out.html))
-	Windows update system is often considered as Universal Backdoor by security researchers. 
## 9. Windows AME
-	Use **christitus winutil** to remove some of the windows pre-installed bloatware making it snappier. Run the following command in the powershell with adminstrator rights.
	-	`iwr -useb https://christitus.com/win | iex`

## 10. Dark Mode for PDFs' - Firefox
- Open the profile directory by going to **More Troubleshoot Information** under **Help Tab**.
-  C:\Users\Muhammad Salmanc\AppData\Roaming\librewolf\Profiles\fvtuhtcz.default-default 
- Create new folder named `chrome`, a new css file named `userContent.css` and place the following lines in the file:
```css
/* #viewerContainer > #viewer > div.spread > .page > .canvasWrapper > canvas */		/* dark pdf in spread mode */
#viewerContainer > #viewer > .page > .canvasWrapper > canvas {			/* dark pdf in no-spread mode */
    filter: grayscale(100%);
    filter: invert(100%);
}
```
- Open config window of the firefox by going to `about:config` and set the preference `toolkit.legacyUserProfileCustomizations.stylesheets` and set it to `true`
- Restart the firefox and see the changes...

## 11. Convert MBR to GPT using Diskpart
### [Reference](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/change-an-mbr-disk-into-a-gpt-disk)
- Warning: May occur data loss as diskpart requires `clean` command to convert disk
- Open Diskpart with adminstrative privilliges `diskpart`
- Select the desired disk from the output of the `list disk`
- Use `clean` command to clear/erase all the volumes/partitions from the disk with focus
- Convert focused disk format to GPT by `convert gpt` 
## 12. Windows App Location
-	C:/Program Files/WindowsApps (Hidden items)
## 13. VSCode Extensions
- Live Server
- Autorename Tag
- Quokka.js

## 14. IP-Splitting in OPENVPN
- Get the ip address of the site,
	 `nslookup times.gift.edu.pk`
- Then change the ovpn configuration file by adding the following lines
	
	```
	route 104.18.3.161 255.255.255.255 net_gateway	
	route 104.18.2.161 255.255.255.255 net_gateway
	```
- Save the configuration file and restart the connection

## 15. Cursor and ChatGPT-Academic
## 16. Apache2 and Apache Guacamole
## 17. Cloudron
## 18. XMPP Server
## 19. NoMAD BSD
## 20. Everything - Dedicated Searching Application
## 21. AutoHotKeyScript
- For modifying the Windows Predefined shortcuts
	- One such example for assigning `Win + S` to `Everything`
```java
		#Requires AutoHotkey v2.0

		#s::Run "C:\Program Files\Everything\Everything.exe"

```
## 22. TreeSize - For listing files and directories
- Sometimes pretty useful
## 23. One Click Firewall - OCF
## 24. Sumatra PDF Reader for Viewing PDF
## 25. Tree Command for Graphical Display
```
tree /F /A "C:\USERS\Muhammad Salmanc\DESKTOP\JS\MERN"
```

![4bf5675227f531bd176df8f477423444.png](../_resources/4bf5675227f531bd176df8f477423444.png)