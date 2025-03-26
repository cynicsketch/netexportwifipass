## Disclaimer:
The information contained herein is for educational purposes only. The author disclaims any liability from any damages that may result from the user, proper or improper, of the information contained in this file.

### Purpose:
[FakeMurk](https://github.com/MercuryWorkshop/fakemurk) enables developer mode while still maintaining enterprise enrollment on a Chromebook, thereby also granting access to WiFi passwords, however, doing so [can readily alert sysadmins](https://github.com/MercuryWorkshop/fakemurk/issues/12) to suspicious activity. This guide presents a way to just get the WiFi password if it is the only thing of interest to red team, while not being as loud and hopefully not being detected until long after operation.

### Requirements:
1. Access to https://luphoria.com/netlog-policy-password-tool on a separate, personally controlled device; the attacker machine.
2. A clean USB drive formatted as EXFAT. (for OPSEC purposes, it should be securely wiped before operation in case ChromeOS logs or forwards the contents of this drive)
3. An enterprise enrolled Chromebook, where `chrome://net-export` is NOT blocked by enterprise policy. This Chromebook will be refered to as the "target machine" from here on out.
4. Login credentials to, or an already logged in target machine.
5. WiFi password set through management by Google Admin.

### Limitations: 
The procedure which follows does NOT prevent the creation of all potential evidence. It only minimizes evidence available and what evidence it does produce is less suspcious from an immediate perspective, making detection merely *unlikely* until long after exploitation. Advanced log analysis or [forensic acquistion](https://dfir.pubpub.org/pub/inkjsqrh/release/2) is sufficient to uncover operations. Methods to shred or corrupt data may reduce evidence further, but aren't generally accessible without enabling developer mode, which is exactly what we intend to avoid.

### Optional: 
Download the site https://luphoria.com/netlog-policy-password-tool offline, using a VPN or preferably TOR and disconnect from the internet when viewing it on the attacker device. This may be desired to keep operations regarding the site anonymous and safe.

### Steps to acquire password:
1. On the target machine, enable WiFi but disconnect from all networks.

Explanation: This prevents the target machine from phoning home at the moment. Any logs that are created may still be forwarded later, but this can decrease transmitted information. The reason why WiFi isn't completely disabled is because it interferes with `chrome://net-export` and prevents it from stashing the WiFi password, which we want. Alternatively, you could attempt to remove the WiFi card or shield it with a faraday bag, but that is out of the scope of this guide.

2. Follow the steps here, up UNTIL the point where it directs the user to "Upload file.": https://luphoria.com/netlog-policy-password-tool
   
Explanation: We don't want anything unusual in the search history of the target machine to be uncovered later. Internal pages like chrome://net-export don't get logged to the search history of Google Chrome, so by NOT visiting [this page](https://luphoria.com/netlog-policy-password-tool) on the target machine we leave no search history behind to indicate that anything has ever happened.

3. Insert the operational USB drive into the target machine.
4. Copy `chrome-net-export-log.json` to the USB drive. As soon as it finishes copying, eject the USB drive ASAP and disconnect it.
5. Delete `chrome-net-export-log.json` from the target machine and empty the trash. The data can still theoretically be recovered, but ChromeOS devices aren't known to read from underlying data/send it back at the moment. It may be found later, but not now. 
6. Without turning the WiFi back on, shut down the target machine. The target machine can now be "safely" (see "Limitations") be powered back on, and if needed, returned to its original condition.
7. Exfiltrate with USB drive. 
8. Upload `chrome-net-export-log.json` from the USB drive to https://luphoria.com/netlog-policy-password-tool on the attacker machine. 
9. Write down the WiFi password in a secure location and use for further purposes. 
