## Disclaimer:
The information contained herein is for educational purposes only. The author disclaims any liability from any damages that may result from the user, proper or improper, of the information contained in this file.

### Purpose:
[FakeMurk](https://github.com/MercuryWorkshop/fakemurk) enables developer mode while still enrolled on a Chromebook, thereby also granting access to WiFi passwords, however, doing so [can readily alert sysadmins](https://github.com/MercuryWorkshop/fakemurk/issues/12) to suspicious activity. This guide presents a way to just get the WiFi password if it is the only thing of interest to red team, while not being as loud and hopefully not being detected until long after operation.

### Requirements:
1. Access to https://luphoria.com/netlog-policy-password-tool on a separate, personallay controlled device; the attacker machine.
2. A clean USB drive formatted as EXFAT. (for OPSEC purposes, it should be securely wiped before operation in case ChromeOS logs or forwards the contents of this drive)
3. An enterprise enrolled Chromebook, where `chrome://net-export` is NOT blocked by enterprise policy. This Chromebook will be refered to as the "target machine" from here on out.
4. Login credentials or being already logged into the target machine.

### Note: 
The procedure which follows does NOT prevent the creation of ALL potential evidence. It only minimizes evidence available and what evidence it does produce is less suspcious from an immediate perspective, making detection merely *unlikely* until long after exploitation. Vigilant log analysis may/may not be sufficient to uncover operations.

### Optional: 
Download the site https://luphoria.com/netlog-policy-password-tool offline, using a VPN or preferably TOR and disconnect from the internet when viewing it on the attacker device. May be desired to keep operations regarding the site anonymous and safe.

### Steps to acquire password:
1. On the target machine, enable wifi but disconnect from all networks.

Explanation: This prevents the target machine from phoning home at the moment. Any logs that are created may still be forwarded later, but this can decrease transmitted information. The reason why WiFi isn't completely disabled is because it interferes with `chrome://net-export` and prevents it from stashing the WiFi password, which we want.

4. Follow the steps here, up UNTIL the point where it directs the user to "Upload file.": https://luphoria.com/netlog-policy-password-tool
   
Explanation: We don't want anything unusual in the search history of the target machine to be uncovered later. Internal pages like chrome://net-export don't get logged to the search history of Google Chrome, so by NOT visiting this page https://luphoria.com/netlog-policy-password-tool on the target machine we leave no search history behind to indicate that anything has ever happened.

3. Insert the operational USB drive into the target machine.
4. Copy `chrome-net-export-log.json` to the USB drive. As soon as it finishes copying, eject the USB drive ASAP and disconnect it.
5. Delete `chrome-net-export-log.json` from the target machine.
6. Without turning the WiFi back on, shut down the target machine. The target machine can now be "safely" (see Note) be powered back on, and if needed, set back to where it originally was.
7. Exfiltrate with USB drive.
8. Connect the USB drive to the attacker machine.
9. Upload `chrome-net-export-log.json` from the USB drive to https://luphoria.com/netlog-policy-password-tool. 
10. Write down the WiFi password in a secure location and use for further purposes. 
