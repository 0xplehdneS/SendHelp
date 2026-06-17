# SendHelp

PowerShell incident response and triage helper toolkit. Upload the file in MDE's script library, and run the bellow commands via the live terminal.

## Features

- Execute commands
- Execute encoded commands
- Query Zone.Identifier
- Query RunMRU
- Query registry keys
- Query and filter prefetch files
- Display running processes


## Usage
### **Get-Version**
Diplays currect version.
```powershell
C:\> run SendHelp Get-Version
Transcript started, output file is C:\ProgramData\Microsoft\Windows Defender Advanced Threat Protection\Temp\PSScriptOutputs\PSScript_Transcript_{9387BA8D-88A3-4725-BA4F-D84876C7F82C}.txt
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

      ___           ___           ___           ___           ___           ___           ___       ___     
     /\  \         /\  \         /\__\         /\  \         /\__\         /\  \         /\__\     /\  \    
    /::\  \       /::\  \       /::|  |       /::\  \       /:/  /        /::\  \       /:/  /    /::\  \   
   /:/\ \  \     /:/\:\  \     /:|:|  |      /:/\:\  \     /:/__/        /:/\:\  \     /:/  /    /:/\:\  \  
  _\:\~\ \  \   /::\~\:\  \   /:/|:|  |__   /:/  \:\__\   /::\  \ ___   /::\~\:\  \   /:/  /    /::\~\:\  \ 
 /\ \:\ \ \__\ /:/\:\ \:\__\ /:/ |:| /\__\ /:/__/ \:|__| /:/\:\  /\__\ /:/\:\ \:\__\ /:/__/    /:/\:\ \:\__\
 \:\ \:\ \/__/ \:\~\:\ \/__/ \/__|:|/:/  / \:\  \ /:/  / \/__\:\/:/  / \:\~\:\ \/__/ \:\  \    \/__\:\/:/  /
  \:\ \:\__\    \:\ \:\__\       |:/:/  /   \:\  /:/  /       \::/  /   \:\ \:\__\    \:\  \        \::/  / 
   \:\/:/  /     \:\ \/__/       |::/  /     \:\/:/  /        /:/  /     \:\ \/__/     \:\  \        \/__/  
    \::/  /       \:\__\         /:/  /       \::/__/        /:/  /       \:\__\        \:\__\              
     \/__/         \/__/         \/__/         ~~            \/__/         \/__/         \/__/                                         
 
 [+] Multi-Function Response Utility
 [+] v17/6/2026
 [+] Author: Thomas Papaloukas
```

<br>

### **Run-Command**
Used for running simple PowerShell commands.
```powershell
run Sendhelp "Run-Command <command>"
```
Example output:
```powershell
C:\> run SendHelp "Run-Command whoami"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

nt authority\system


C:\> run SendHelp "Run-Command ping 8.8.8.8"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________


Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=18ms TTL=116
Reply from 8.8.8.8: bytes=32 time=18ms TTL=116
Reply from 8.8.8.8: bytes=32 time=17ms TTL=116
Reply from 8.8.8.8: bytes=32 time=18ms TTL=116

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 17ms, Maximum = 18ms, Average = 17ms
```

<br>

### **Run-EncodedCommand**
Used for running more complex PowerShell commands, since MDE does not allow characters such as '(', ')', '|' within the ternminal. Encode the command as base64, and execute it.
```powershell
run Sendhelp "Run-EncodedCommand <b64_command>"
```
Example output:
```powershell
C:\> run SendHelp "Run-EncodedCommand R2V0LUNvbnRlbnQgQzpcVXNlcnNcU2VuZGhlbHBcRGVza3RvcFxzY3JpcHQucHMxIHwgU2VsZWN0LVN0cmluZyAiR2V0LVZlcnNpb24i"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

Decoded command: Get-Content C:\Users\Sendhelp\Desktop\script.ps1 | Select-String "Get-Version"


function Get-Version {
    "Get-Version" {
        Get-Version
```

<br>

### **Run-ZoneIdentifier**
Used for inspecting the Zone.Identifier stream of a file, if present.
```powershell
run Sendhelp "Run-ZoneIdentifier <file_path>"
```
Example output:
```powershell
C:\> run SendHelp "Run-ZoneIdentifier C:\Users\Sendhelp\Downloads\002bf42d8594e9bf7fdcfd2bc9a0a9ddff495f8289a589946ce421afcc3d30d6.zip"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

Zone.Identifier found for: C:\Users\Sendhelp\Downloads\002bf42d8594e9bf7fdcfd2bc9a0a9ddff495f8289a589946ce421afcc3d30d6.zip

[ZoneTransfer]
ZoneId=3
ReferrerUrl=https://bazaar.abuse.ch/download/002bf42d8594e9bf7fdcfd2bc9a0a9ddff495f8289a589946ce421afcc3d30d6/
HostUrl=https://bazaar.abuse.ch/download/16cd486636ce9fbb097d/
```

<br>

### **Run-RunMRU**
Extracts RunMRU entries from the registry to identify commands previously executed through the Windows Run dialog.
```powershell
run Sendhelp Run-RunMRU
```
Example output:
```powershell
C:\> run SendHelp Run-RunMRU
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

User SID      : S-1-5-21-1436740266-4157994359-4067780582-1000
Profile Path  : C:\Users\Sendhelp
Registry Path : Registry::HKEY_USERS\S-1-5-21-1436740266-4157994359-4067780582-1000\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU

a = whoami
b = ping google.gr
c = net user
d = regedit
```

<br>

### **Run-RegistryQuery**
Extracts registry values from a specified key for triage and investigation purposes. Avoid using `HKEY_CURRENT_USER`, since it is context-dependent. In MDE Live Response, 
it points to the user context of the process/session that Live Response is using (nt authority\system).
```powershell
run Sendhelp "Run-RegistryQuery <registry_key>"
```
Example output:
```powershell
C:\> run SendHelp "Run-RegistryQuery HKEY_USERS\S-1-5-21-1436740266-4157994359-4067780582-1000\SOFTWARE\POCKey"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

Registry Path: Registry::HKEY_USERS\S-1-5-21-1436740266-4157994359-4067780582-1000\SOFTWARE\POCKey

Key Name  | Key Value
_____________________

POCString = POCValue
```

<br>

### **Run-Prefetch**
Enumerates Windows Prefetch files to identify evidence of recent program execution.

```powershell
run Sendhelp Run-Prefetch
```
Example output:
```powershell
C:\> run SendHelp Run-Prefetch
Transcript started, output file is C:\ProgramData\Microsoft\Windows Defender Advanced Threat Protection\Temp\PSScriptOutputs\PSScript_Transcript_{8C5E227C-1386-46E6-9D3D-E647715C3BFA}.txt
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________


Name                                      LastWriteTime       Length
----                                      -------------       ------
SHUTDOWN.EXE-1692B741.pf                  14/10/2023 22:51:22   3171
POWERCFG.EXE-954C9186.pf                  14/10/2023 22:51:17   1973
POWERSHELL.EXE-CA1AE517.pf                14/10/2023 22:51:15  34409
COMPATTELRUNNER.EXE-B7A68ECC.pf           14/10/2023 22:51:13   3075
DLLHOST.EXE-94657348.pf                   14/10/2023 22:50:47   4039
```

<br>

### **Run-PrefetchQuery**
Filter Windows Prefetch files filtered by a process name.
```powershell
run Sendhelp "Run-PrefetchQuery <process>"
```
Example output:
```powershell
C:\> run SendHelp "Run-PrefetchQuery notepad"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________


Name                    LastWriteTime       Length
----                    -------------       ------
NOTEPAD.EXE-C5670914.pf 14/10/2023 22:19:21   9794                  14/10/2023 22:50:47   4039

C:\> run SendHelp "Run-PrefetchQuery malware"
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________

No prefetch files found matching: malware
```

<br>

### **Run-Processes**
Enumerates running processes.
```powershell
run Sendhelp "Run-PrefetchQuery <process>"
```
Example output:
```powershell

C:\> run SendHelp Run-Processes
_________________________________________

Roger that. Cooking output (robot noises) 
_________________________________________


Name                                                            Id       CPU        WS
----                                                            --       ---        --
chrome                                                        4528  6.703125  40226816
cmd                                                           6064  0.015625   2113536
Code                                                          6960  226.6875 108843008
CodeSetup-stable-6928394f91b684055b873eecb8bc281365131f1c     3236   0.21875    753664
CodeSetup-stable-6928394f91b684055b873eecb8bc281365131f1c.tmp 3176 26.015625   2252800
conhost                                                       5260         0    598016
csrss                                                          560  4.265625  54063104
csrss                                                          472    1.1875   1998848
ctfmon                                                        5324  5.546875   7143424
dasHost                                                       1720    0.1875  13844480
Discord                                                       5124   2.21875   5971968
Discord                                                       3160   0.03125   1269760
DiscordSystemHelper                                           6020  0.390625    
```
