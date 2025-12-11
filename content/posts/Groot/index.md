---
title: "Underthewire groot war games"
date: 2025-06-11
description: This is a writeup on groot from underthewire wargames.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "underthewire wargames groot series" # Here you can write a small summary of the post if needed
tags: [powershell, SSH]
categories: [Underthewire, AD]
---

## Groot level 0-1

The goal of this challenge is for us to log in using the credentials provided to us in the Slack channel.

```powershell
The goal of this level is to log into the game. Do the following in order to achieve this goal.

1. Obtain the initial credentials via the #StartHere channel on our Slack (link). Once you are in the channel, scroll to the top to see the credentials.

2.
 After obtaining the credentials, connect to the server via SSH. You 
will need an SSH client such as Putty. The host that you will be 
connecting to is groot.underthewire.tech, on port 22.

3. When prompted, use the credentials for the applicable game found in the #StartHere Slack channel.

4. You have successfully connected to the game server when your path changes to “PS C:\Users\Groot1\desktop>”.
```

![image.png](image.png)

After getting the credentials, we can then use them to log in via ssh as shown below, is the terminal of the instance after successful log in to the challenge.

```powershell
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\Groot1\desktop>
```

## Groot level 1-2

The objective of this challenge, is to find the md5 hash of the system’s host file and use the last five alphanumeric characters as the password for level 2.

```powershell
The password for groot2 is the last five alphanumeric characters of the MD5 hash of this system’s hosts file.

NOTE:
– The password will be lowercase no matter how it appears on the screen.

IMPORTANT:
Once
 you feel you have completed the Groot1 challenge, start a new 
connection to the server, and log in with the username of Groot2 and 
this password will be the answer from Groot1. If successful, close out 
the Groot1 connection and begin to solve the Groot2 challenge. This 
concept is repeated over and over until you reach the end of the game.
```

We shall use the command below.

```powershell
 get-filehash -algorithm md5 C:\Windows\System32\
drivers\etc\hosts
```

Below is the breakdown of the command used.

`Get-FileHash`

- A PowerShell cmdlet that computes the hash value for a file
- Used for file integrity checking and verification

`Algorithm MD5`

- Specifies the MD5 hashing algorithm (128-bit hash value)
- Alternatives include SHA1, SHA256, SHA384, SHA512, etc.

`C:\Windows\System32\drivers\etc\hosts`

- The path to the Windows hosts file
- This plaintext file maps hostnames to IP addresses before DNS lookup

Here is the output of the command.

```powershell
PS C:\users\Groot1\desktop> get-filehash -algorithm md5 C:\Windows\System32\
drivers\etc\hosts

Algorithm       Hash
---------       ----
MD5             [REDACTED]

PS C:\users\Groot1\desktop>
```

After getting the hash, we can now use the last alphanumeric characters as our password for the next level.

## Groot level 2-3

For this level, we are required to find the word that is made up of letters in the range of `1,481,110` to `1,481,117` 

```powershell
The password for groot3 is the word that is made up from the letters in 
the range of 1,481,110 to 1,481,117 within the file on the desktop.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

Since the host key has already been added to the known hosts list, we do not need an additional option. Below is the command we shall use.

```powershell
(gc .\elements.txt)[1481110..1481117] -join ""
```

Below is the break-down of the command.

`gc .\elements.txt`

- `gc` is the alias for `Get-Content`
- Reads the content of `elements.txt` in the current directory
- Returns the file's content as an array of characters (since PowerShell treats strings as character arrays)

`[1481110..1481117]`

- Extracts characters at index positions 1,481,110 through 1,481,117
- PowerShell uses zero-based indexing, so these are the 1,481,111th to 1,481,118th characters
- The `..` Creates a range of indices

`join ""`

- Combines the 8 selected characters into a single string
- The empty quotes `""` specify no separator between characters

Here is the output of the command used.

```powershell
PS C:\users\Groot2\desktop> (gc .\elements.txt)[1481110..1481117] -join ""
 [REDACTED]
PS C:\users\Groot2\desktop>
```

With that, we have the password for our next level.

## Groot level 3-4

In this challenge, we are required to find the number of times the word `beetle` is listed in the file on the desktop.

```powershell
The password for groot4 is the number of times the word “beetle” is listed in the file on the desktop.
```

We shall use the following command.

```powershell
(type ".\words.txt").split(' ') -like "beetle" |
 measure-object
```

Below is a breakdown of the command.

`type ".\words.txt"`

- `type` is an alias for `Get-Content`
- Reads the content of "words.txt" in the current directory
- Equivalent to `Get-Content ".\words.txt"`

`.split(' ')`

- Splits the file content into an array of words using space as the delimiter
- Note: This simple splitting may not handle all cases (like punctuation or multiple spaces)

`like "beetle"`

- Filters the array to only include elements that match "beetle"
- `like` allows wildcard matching (though not used here)

`| measure-object`

- Counts the number of items that passed the filter
- Returns an object with count, average, sum, etc. (though only count is relevant here)

Here is the output of the command.

```powershell
PS C:\users\Groot3\desktop> (type ".\words.txt").split(' ') -like "beetle" |
 measure-object

Count    : [REDACTED]
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

## Groot level 4-5

Below is the objective of the challenge.

```powershell
The password for groot5 is the name of the Drax subkey within the HKEY_CURRENT_USER (HKCU) registry hive.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

We are required to find the name of drax subkey within the `HKCU` registry hive.

Below is the command we shall use for this challenge.

```powershell
get-childitem -path hkcu:\ -recurse -erroraction
 silentlycontinue | select-string drax
```

Here is the breakdown of the command.

`Get-ChildItem -Path HKCU:\`

- Lists items in the HKEY_CURRENT_USER (HKCU) registry hive
- `HKCU:\` is the PowerShell drive representing the current user's registry settings

`Recurse`

- Searches through all subkeys recursively (entire registry tree under HKCU)

`ErrorAction SilentlyContinue`

- Suppresses permission errors when accessing protected registry keys
- Prevents the command from stopping if it hits keys it can't access

`| Select-String drax`

- Pipes the registry contents to search for the string "drax"
- Looks for "drax" in:
    - Key names
    - Property names
    - Property values (both names and data)

Below is the output of the command.

```powershell
PS C:\users\Groot4\desktop> get-childitem -path hkcu:\ -recurse -erroraction
 silentlycontinue | select-string drax

HKEY_CURRENT_USER\Software\Microsoft\Assistance\Drax
HKEY_CURRENT_USER\Software\Microsoft\Assistance\Drax\[REDACTED]

PS C:\users\Groot4\desktop>
```

We now have the password for the next level.

## Groot level 5-6

For this challenge, we are required to find the name of the workstation for the user `baby.groot` then combine it with the name of the file on the desktop of the user groot5.

```powershell
The password for groot6 is the name of the workstation that the user 
with a username of “baby.groot” can log into as depicted in Active 
Directory PLUS the name of the file on the desktop

NOTE:
– If the workstation is “system1” and the file on the desktop is named “_log”, the password would be “system1_log”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the target file on desktop.

```powershell
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\Groot5\desktop> ls

    Directory: C:\users\Groot5\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/20/2020   3:38 PM              0 _enterprise
```

To find the name of the workstation of the ad user `baby.groot` , we shall use the command below.

```powershell
 (get-aduser baby.groot -properties * ).userWorkstations
```

Below is the breakdown of the command.

`Get-ADUser baby.groot`

- Retrieves the Active Directory user object for account "baby.groot"
- Basic version only returns default properties (name, SAM account, SID, etc.)

`Properties *`

- Requests ALL properties of the user object (not just default ones)
- Required because `userWorkstations` isn't among default returned properties

`.userWorkstations`

- Accesses the specific `userWorkstations` attribute from the returned object
- This attribute specifies which computers the user can log into (by NetBIOS name)

Below is the output of the command.

```powershell
PS C:\users\Groot5\desktop> (get-aduser baby.groot -properties * ).userWorkstations
[REDACTED]
PS C:\users\Groot5\desktop>
```

## Groot level 6-7

For this challenge, we need to find the name of the program that is set to run when this user logs in, then join it with the name of the file on the desktop.

```powershell
The password for groot7 is the name of the program that is set to start when this user logs in PLUS the name of the file on the desktop.

NOTE:
– Omit the executable extension.
– If the program is “mspaint” and the file on the desktop is named “_log”, the password would be “mspaint_log”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the file on the desktop.

```powershell
PS C:\users\Groot6\desktop> ls

    Directory: C:\users\Groot6\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/21/2020   1:24 PM              0 _rules

PS C:\users\Groot6\desktop>
```

To find the name of the program that runs when the user logs in, we shall use the command shown below, which is very similar with the command we used on groot7.

```powershell
 get-WmiObject Win32_StartupCommand | Select-Obje
ct Name, command, Location, User  | Format-List
```

Below is the breakdown.

`Get-WmiObject Win32_StartupCommand`

- Queries WMI (Windows Management Instrumentation) for startup commands
- The `Win32_StartupCommand` class contains entries for:
    - Programs in Startup folders
    - Registry Run keys (HKLM\Software\Microsoft\Windows\CurrentVersion\Run)
    - Other auto-start locations

`| Select-Object Name, Command, Location, User`

- Filters the output to show only these specific properties:
    - Name: Display name of the startup item
    - Command: Actual executable/command that runs
    - Location: Where the startup entry is configured
    - User: Which user account the startup applies to (if user-specific)

`| Format-List`

- Displays the results in list format (one property per line)
- Alternative to table view (`Format-Table`), better for readability with long values

Below is the output of the command.

```powershell
PS C:\users\Groot6\desktop> get-WmiObject Win32_StartupCommand | Select-Obje
ct Name, command, Location, User  | Format-List

Name     : New Value #1
command  :
Location : HKU\S-1-5-21-758131494-606461608-3556270690-1169\SOFTWARE\Micros
           oft\Windows\CurrentVersion\Run
User     : underthewire\Groot6

Name     : New Value #2
command  :
Location : HKU\S-1-5-21-758131494-606461608-3556270690-1169\SOFTWARE\Micros
           oft\Windows\CurrentVersion\Run
User     : underthewire\Groot6

Name     : New Value #3
command  :
Location : HKU\S-1-5-21-758131494-606461608-3556270690-1169\SOFTWARE\Micros
           oft\Windows\CurrentVersion\Run
User     : underthewire\Groot6

Name     : New Value #4
command  :
Location : HKU\S-1-5-21-758131494-606461608-3556270690-1169\SOFTWARE\Micros
           oft\Windows\CurrentVersion\Run
User     : underthewire\Groot6

Name     : [REDACTED]
command  : C:\star-lord.exe
Location : HKU\S-1-5-21-758131494-606461608-3556270690-1169\SOFTWARE\Micros
           oft\Windows\CurrentVersion\Run
User     : underthewire\Groot6

PS C:\users\Groot6\desktop>
```

And with that, we have our password.

## Groot level 7-8

For this challenge, we are required to find the name of dll as depicted in the registry, associated with the `applockerfltr` service then join it with the name of the file on the desktop in order to get the password for the next level.

```powershell
The password for groot8 is the name of the dll, as depicted in the registry, associated with the “applockerfltr” service PLUS the name of the file on the desktop.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
– If the name of the dll is “abc.dll” and the file on the desktop is named “_1234”, the password would be “abc_1234”.
```

Below is the name of the files located on the desktop.

```powershell
PS C:\users\Groot7\desktop> ls

    Directory: C:\users\Groot7\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        5/31/2021   5:13 PM              0 _home

PS C:\users\Groot7\desktop>
```

We shall use the command below to find the name of the dll.

```powershell
(Get-ChildItem 'HKLM:\SYSTEM\CurrentControlSet\S
ervices' | Where-Object {$_.Name -like '*applocker*'} | Get-ItemProperty -Na
me DisplayName).DisplayName.Split('\\')[-1].Split('\.')[0]
```

`Get-ChildItem 'HKLM:\SYSTEM\CurrentControlSet\Services'`

- Lists all subkeys under the Windows services registry location
- This is where all system services are registered

`| Where-Object {$_.Name -like '*applocker*'}`

- Filters to only show services with "applocker" in their name (case-insensitive)
- The wildcards () mean it can appear anywhere in the name`| Get-ItemProperty -Name DisplayName`
- Retrieves the `DisplayName` property from each matching service
- This is the human-readable service name shown in services.msc

`.DisplayName.Split('\\')[-1]`

- Takes the DisplayName string and splits it by backslashes (`\`)
- `[-1]` selects the last element after splitting

`.Split('\.')[0]`

- Further splits the result by a period (`.`)
- `[0]` selects the first part before any period

In windows, registered services can be found under `HKLM:\SYSTEM\CurrentControlSet\Services`

Below is the output of the command.

```powershell
PS C:\users\Groot7\desktop> (Get-ChildItem 'HKLM:\SYSTEM\CurrentControlSet\S
ervices' | Where-Object {$_.Name -like '*applocker*'} | Get-ItemProperty -Na
me DisplayName).DisplayName.Split('\\')[-1].Split('\.')[0]
[REDACTED]
PS C:\users\Groot7\desktop>
```

## Groot level 8-9

For this level, we are required to find the description of the firewall rule that blocks `MYSQL` and join it with the name of the file on the desktop.

```powershell
The password for groot9 is the description of the firewall rule blocking MySQL PLUS the name of the file on the desktop.

NOTE:
– If the description of the rule is “blue” and the file on the desktop is named “_bob”, the password would be “blue_bob”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the file on the desktop.

```powershell
PS C:\users\Groot8\desktop> ls

    Directory: C:\users\Groot8\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 _starlord

PS C:\users\Groot8\desktop>
```

To find the description of the firewall rule, we shall use the following command.

```powershell
 Get-NetFirewallRule -Action Block | Where-Object {$_.DisplayName -like '*mysql*'}
```

Below is the breakdown of the command.

`Get-NetFirewallRule -Action Block`

- Retrieves all Windows Firewall rules that have an action of "Block"
- Part of the `NetSecurity` module (built into modern Windows)
- Without parameters, it would return all firewall rules (allow and block)

`|` (Pipeline)

- Sends the firewall rules to the next command for filtering

`Where-Object {$_.DisplayName -like '*mysql*'}`

- Filters the rules to only show those with "mysql" in their display name
- `like` with wildcards () makes the search case-insensitive
- `$_` represents each firewall rule as it comes through the pipeline

Below is the output of the command.

```powershell
PS C:\users\Groot8\desktop> Get-NetFirewallRule -Action Block | Where-Object
 {$_.DisplayName -like '*mysql*'}

Name                  : {8ce6b97d-5c1d-4347-a7fd-1792feb42355}
DisplayName           : MySQL
Description           :[REDACTED]
DisplayGroup          :
Group                 :
Enabled               : True
Profile               : Any
Platform              : {}
Direction             : Inbound
Action                : Block
EdgeTraversalPolicy   : Block
LooseSourceMapping    : False
LocalOnlyMapping      : False
Owner                 :
PrimaryStatus         : OK
Status                : The rule was parsed successfully from the store.
                        (65536)
EnforcementStatus     : NotApplicable
PolicyStoreSource     : PersistentStore
PolicyStoreSourceType : Local

PS C:\users\Groot8\desktop>
```

## Groot level 9-10

For this level we need to find the name of the OU that doesn’t have accidental deletion protection enabled, then join it with the name of the file on the desktop.

```powershell
The password for groot10 is the name of the OU that doesn’t have accidental deletion protection enabled PLUS the name of the file on the desktop.

NOTE:
– If the name of the OU is called “blue” and the file on the desktop is named “_bob”, the password would be “blue_bob”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the name of the file on the desktop.

```powershell
PS C:\users\Groot9\desktop> ls

    Directory: C:\users\Groot9\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 _tester

PS C:\users\Groot9\desktop>
```

To find the OU (Organizational unit), we shall use the command below.

```powershell
Get-ADOrganizationalUnit -Filter * -Properties ProtectedFromAccidentalDeletion | Where-Object {-not $_.ProtectedFromAccidentalDeletion}
```

Below is the breakdown of the command.

`Get-ADOrganizationalUnit -Filter *`

- Retrieves all Organizational Units (OUs) from Active Directory
- `Filter *` means "no filter" (get all OUs)

`Properties ProtectedFromAccidentalDeletion`

- Includes the `ProtectedFromAccidentalDeletion` property in the results
- This property isn't returned by default, so we explicitly request it

`| Where-Object {-not $_.ProtectedFromAccidentalDeletion}`

- Filters the OUs to only show those where protection is NOT enabled
- `not` inverts the boolean value (selects false values)
- `$_` represents each OU as it comes through the pipeline

Below is the output of the command.

```powershell
PS C:\users\Groot9\desktop> Get-ADOrganizationalUnit -Filter * -Properties P
rotectedFromAccidentalDeletion | Where-Object {-not $_.ProtectedFromAccident
alDeletion}

City                            :
Country                         :
DistinguishedName               : OU=T-25,OU=X-Wing,DC=underthewire,DC=tech
LinkedGroupPolicyObjects        : {cn={49401C32-4145-463F-B5E7-816926D4F78D
                                  },cn=policies,cn=system,DC=underthewire,D
                                  C=tech}
ManagedBy                       :
Name                            : [REDACTED]
ObjectClass                     : organizationalUnit
ObjectGUID                      : fc15c303-dd9a-4c44-a941-314cc6fdd394
PostalCode                      :
ProtectedFromAccidentalDeletion : False
State                           :
StreetAddress                   :

PS C:\users\Groot9\desktop>
```

We now have our password for level 10

## Groot level 10-11

For this level, we are required to find the word is different from the other file on the desktop. In short we are required to find a cmdlet that functions as the `diff` command in linux which in our case is `Compare-Object`.

```powershell
The password for groot11 is the one word that makes the two files on the desktop different.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

The files on the desktop are as shown below.

```powershell
PS C:\users\Groot10\desktop> ls

    Directory: C:\users\Groot10\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018   5:52 AM          17324 new.txt
-a----        8/30/2018   5:52 AM          17313 old.txt

PS C:\users\Groot10\desktop>
```

Here is how we shall solve the problem.

```powershell
compare-object (gc .\old.txt) (gc .\new.txt)
```

Below is the nreakdown of the command.

`Get-Content .\old.txt` (abbreviated as `gc .\old.txt`)

- Reads the content of `old.txt` line by line
- Returns an array of strings (each line becomes an array element)

`Get-Content .\new.txt` (abbreviated as `gc .\new.txt`)

- Reads the content of `new.txt` line by line
- Returns another array of strings

`Compare-Object`

- Compares the two collections (arrays of lines from each file)
- Shows which lines are unique to each file
- The output indicates whether each difference appears in the "Reference" set (first file) or "Difference" set (second file)

Below is the output of the command.

```powershell
PS C:\users\Groot10\desktop> compare-object (gc .\old.txt) (gc .\new.txt)

InputObject SideIndicator
----------- -------------
[REDACTED]   =>

PS C:\users\Groot10\desktop>
```

## Groot level 11-12

For this level, we are told the password is somewhere in the alternate data stream on the desktop, meaning we have to find a way of referencing all the data streams of the files on the desktop.

```powershell
The password for groot12 is within an alternate data stream (ADS) somewhere on the desktop.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

Below are the files on the desktop of the user.

```powershell
PS C:\users\Groot11\desktop> ls

    Directory: C:\users\Groot11\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018   5:52 AM             30 TPS_Reports01.txt
-a----        8/30/2018   5:52 AM             30 TPS_Reports02.doc
-a----        8/30/2018   5:52 AM              0 TPS_Reports03.txt
-a----        8/30/2018  10:51 AM             30 TPS_Reports04.pdf
-a----        8/30/2018   5:52 AM             30 TPS_Reports05.xlsx
-a----        8/30/2018   5:52 AM             30 TPS_Reports06.pptx

PS C:\users\Groot11\desktop>
```

To check the data streams of all the files, we shall use the command below.

```powershell
 gci -File | gi -Stream * | select-object FileNa
me,Stream
```

Below is the breakdown of the command.

`Get-ChildItem -File` (abbreviated as `gci -File`)

- Lists all files (not directories) in the current folder
- The `File` switch ensures only files are returned, not folders

`| Get-Item -Stream *` (abbreviated as `| gi -Stream *`)

- Pipes the files to examine their alternate data streams
- `Stream *` retrieves information about ALL streams (not just the main data stream)
- This includes hidden streams like `Zone.Identifier` (used for downloaded files)

`| Select-Object FileName, Stream`

- Filters the output to show only:
    - `FileName`: The name of the file
    - `Stream`: The name of the alternate data stream

Below is the output of the command.

```powershell
PS C:\users\Groot11\desktop> gci -File | gi -Stream * | select-object FileNa
me,Stream

FileName                                    Stream
--------                                    ------
C:\users\Groot11\desktop\TPS_Reports01.txt  :$DATA
C:\users\Groot11\desktop\TPS_Reports02.doc  :$DATA
C:\users\Groot11\desktop\TPS_Reports03.txt  :$DATA
C:\users\Groot11\desktop\TPS_Reports04.pdf  :$DATA
C:\users\Groot11\desktop\TPS_Reports04.pdf  secret
C:\users\Groot11\desktop\TPS_Reports05.xlsx :$DATA
C:\users\Groot11\desktop\TPS_Reports06.pptx :$DATA

PS C:\users\Groot11\desktop>
```

After getting the stream, we can now get the entire stream as a single stream as shown below.

```powershell
Get-Content C:\users\Groot11\desktop\TPS_Reports04.pdf -Raw -Stream secret
```

`Get-Content`

- PowerShell cmdlet for reading file contents

`C:\users\Groot11\desktop\TPS_Reports04.pdf`

- Specifies the path to a PDF file on user Groot11's desktop
- The file appears to be named "TPS_Reports04.pdf" (possibly a reference to Office Space movie)

`Raw`

- Reads the entire stream as a single string
- Without this, it would return line-by-line output

`Stream secret`

- Attempts to access an alternate data stream named "secret"
- This is a hidden stream attached to the file (NTFS feature)

Here is the output of the command

```powershell
PS C:\users\Groot11\desktop> Get-Content C:\users\Groot11\desktop\TPS_Report
s04.pdf -Raw -Stream secret
[REDACTED]

PS C:\users\Groot11\desktop>
```

## Groot level 12-13

For this challenge, we have to find the owner of the folder `Nine Realms` located on the desktop in order to find the password for the next level.

```powershell
The password for groot13 is the owner of the Nine Realms folder on the desktop.

NOTE:
– Exclude the Administrator, the Administrators group, and System.
–
 The password will be lowercase with no punctuation no matter how it 
appears on the screen. For example, if the owner is “john.doe”, it would
 be “johndoe”.
```

Below is the command we shall use to get the owner of the folder

```powershell
get-acl '.\Nine Realms'
```

`Get-Acl`

- The PowerShell cmdlet that retrieves security descriptors (permission information)
- Stands for "Get Access Control List"

`'.\Nine Realms'`

- Specifies the target path (a folder or file named "Nine Realms")
- `.\` indicates the current directory
- The single quotes allow for spaces in the name

Below is the output of the command

```powershell
PS C:\users\Groot12\desktop> get-acl '.\Nine Realms'

    Directory: C:\users\Groot12\desktop

Path        Owner                Access
----        -----                ------
Nine Realms underthewire\[REDACTED] NT AUTHORITY\SYSTEM Allow  FullControl...
```

## Groot level 13-14

For this level, we are required to find the name of the registered owner of this system, then combine with the name of the file on the desktop.

```powershell
The password for groot14 is the name of the Registered Owner of this system as depicted in the Registry PLUS the name of the file on the desktop.

NOTE:
– If the Registered Owner is “Elroy” and the file on the desktop is named “_bob”, the password would be “elroy_bob”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the file located on the desktop.

```powershell
PS C:\users\Groot13\desktop> ls

    Directory: C:\users\Groot13\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 _ned

PS C:\users\Groot13\desktop>
```

To find the name of the registered owner of this system, we shall check the registry `HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion` using the command below.

```powershell
Get-Item 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion'
```

What this command does is

`Get-Item`

- PowerShell cmdlet that retrieves information about items (files, folders, registry keys)
- When used with registry paths, it gets registry key information

`'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion'`

- Specifies the registry path to examine
- `HKLM:` = HKEY_LOCAL_MACHINE hive (physical machine settings)
- This specific path contains Windows version and installation information

Below is the output of the command.

```powershell
PS C:\users\Groot13\desktop> get-item 'HKLM:\SOFTWARE\Microsoft\Windows NT\C
urrentVersion'

    Hive: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT

Name                           Property
----                           --------
CurrentVersion                 SystemRoot                : C:\Windows
                               BuildBranch               : rs1_release
                               BuildGUID                 :
                               ffffffff-ffff-ffff-ffff-ffffffffffff
                               BuildLab                  :
                               14393.rs1_release.250210-1748
                               BuildLabEx                :
                               14393.7870.amd64fre.rs1_release.250210-1748
                               CompositionEditionID      : ServerStandard
                               CurrentBuild              : 14393
                               CurrentBuildNumber        : 14393
                               CurrentMajorVersionNumber : 10
                               CurrentMinorVersionNumber : 0
                               CurrentType               : Multiprocessor
                               Free
                               CurrentVersion            : 6.3
                               EditionID                 : ServerStandard
                               InstallationType          : Server
                               InstallDate               : 1533958991
                               ProductName               : Windows Server
                               2016 Standard
                               ReleaseId                 : 1607
                               SoftwareType              : System
                               UBR                       : 7876
                               PathName                  : C:\Windows
                               Customizations            : None
                               DigitalProductId          : {164, 0, 0,
                               0...}
                               DigitalProductId4         : {248, 4, 0,
                               0...}
                               ProductId                 :
                               00377-60000-00000-AA934
                               InstallTime               :
                               131784325915283879
                               RegisteredOwner           : [REDACTED]
                               RegisteredOrganization    : OVH SAS

PS C:\users\Groot13\desktop>
```

With that, we have our password.

## Groot level 14-15

We are required to find the description of the share whose name contains “task” in it, then combine it with the name of the file on the desktop.

```powershell
The password for groot15 is the description of the share whose name contains “task” in it PLUS the name of the file on the desktop.

NOTE:
–
 If the description is “frozen_pizza” and the file on the desktop is 
named “_sucks”, the password would be “frozen_pizza_sucks”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the name of the file on the desktop.

```powershell
PS C:\users\Groot14\desktop> ls

    Directory: C:\users\Groot14\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 _8

PS C:\users\Groot14\desktop>
```

To find the description of the share, we shall use the command below.

```powershell
Get-SmbShare -Name "*task*"
```

Here is what the command does.

`Get-SmbShare`

- PowerShell cmdlet that retrieves Server Message Block (SMB) shares
- Part of the `SmbShare` module (available on Windows Server and Windows 10/11)
- Without parameters, lists all SMB shares on the computer

`Name "*task*"`

- Filters shares to only those containing "task" in their name
- The asterisks () are wildcards meaning "any characters before/after 'task'"
- Case-insensitive matching (will find "Task", "TASK", "MyTasks", etc.)

Below is the output of the command.

```powershell
PS C:\users\Groot14\desktop> get-smbshare -name "*task*"

Name   ScopeName Path Description
----   --------- ---- -----------
Tasker *              [REDACTED]
PS C:\users\Groot14\desktop>
```

We now have the password for level 15.

## Groot level 15

😀 😊WE ARE DONE!!!

```powershell
Congratulations!

You have successfully made it to the end!

Try your luck with other games brought to you by the Under The Wire team.

Thanks for playing!

Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\Groot15\desktop> 

:O :)

```