# Goal

The goal is to detect the use of screen capture by the threat actor, BadPatch, by looking for specific image files created by malicious software. 

# Categorization

This is categorized as Collection by Mitre ATT&CK.

# Strategy Abstract

The strategy will function as follows:
* Record all .jpg file creation events in sysmon.
* Alert on any files created that match the BadPatch naming conventions. 

# Technical Context

Screen Capture may occur as a feature of a remote access tool.​

It can also be done through native utilities or API calls, such as CopyFromScreen, xwd, or screencapture. ​

RTF Exploit: allows threat actors to execute arbitrary commands via malicious RTF files.

AutoIt is an open-source automation language for Windows. It is a BASIC-like script, designed to automate the Microsoft Windows user interface. Exploits of this include running it to enact various destructive tasks.

BadPatch is a Windows Trojan used in Gaza Hackers-linked campaign​. Found 6 domains connected to Palestine that did not have any legitimate activity. ​These included:
* Pal4u[.]net
* Pal2me[.]net
* Pay2earn[.]net
* Shop8d[.]net
* Ts4shope[.]net
* pal4news[.]net

The attacks were made using both spear phishing and the RTF exploit to download their Window malware. The malware fit into four categories:​
* Microsoft Visual Basic Malware​
* **AutoIt malware**
* AutoIt downloader & dropper​
* Android malware​

AutoIt will attempt to detect if its being run in a VM. ​It will delete Chrome and Firefox cached password files to force the user to re-enter passwords and pick them up on a keylogger. ​It can then take screenshots then exfiltrate them using SMTP (port 26) as "GDIPlus_Image1.jpg" and "GDIPlus_Image2.jpg", etc.​​

# Blind Spots and Assumptions

A blind spot that may occur is if the threat actor creates a file other than a .jpg. ​

We are acting on the assumption that they will continue to use a consistent image naming scheme and that sysmon is running correctly. 

# False Positives

The user creates an image with the same naming conventions as BadPatch.​ It may create multiple positives on the same instance of a file creation. 

# Validation

Splunk can be used to detect Event ID 11 from sysmon. Using the following query, you will be able to identify instances of the malware: 
```
source="log-name.source-type" host="host-name" sourcetype="source-type"
| search "Event ID"=11 "GDIPlus"
```

# Priority

This is a very high priority because if images are being taken that you are unaware of, the attacker is already on your system. They have the ability to do other things than take screenshots and should be removed from the system immediately. 

# Response

In response, you should isolate the infected machine. ​From there, you need to roll-back to a previous backup that is not infected by the malware. ​You also need to identify how the malware got on to the machine to stop it from happening again. This can be done by analyzing the infected version and looking at possible visits to the websites that were mentioned above or look at possible infection by phishing. 

# Additional Resources

- [BadPatch Fortinet](https://www.fortinet.com/blog/threat-research/badpatch-campaign-uses-python-malware​)
- [Screen Capture T1113](https://attack.mitre.org/techniques/T1113/​)
- [BadPatch Unit42](https://unit42.paloaltonetworks.com/unit42-badpatch/​)
