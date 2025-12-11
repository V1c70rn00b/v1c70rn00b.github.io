---
title: "Underthewire oracle war games"
date: 2025-06-11
description: This is a writeup on oracle from underthewire wargames.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "underthewire wargames oracle series" # Here you can write a small summary of the post if needed
tags: [powershell, SSH, event logs]
categories: [Underthewire, AD]
---

## Oracle level 0-1

The goal for this level is to log in to the first level via ssh using the creds provided to us in the Slack channel.

```powershell
The goal of this level is to log into the game. Do the following in order to achieve this goal.

1. Obtain the initial credentials via the #StartHere channel on our Slack (link). Once you are in the channel, scroll to the top to see the credentials.

2.
 After obtaining the credentials, connect to the server via SSH. You 
will need an SSH client such as Putty. The host that you will be 
connecting to is oracle.underthewire.tech, on port 22.

3. When prompted, use the credentials for the applicable game found in the #StartHere Slack channel.

4. You have successfully connected to the game server when your path changes to “PS C:\Users\Oracle1\desktop>”.
```

![image.png](image.png)

With the credentials above, we are able to now log in as shown below.

```powershell
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\Oracle1\desktop> 
```

## Oracle level 1-2

For this challenge, we are required to find the timezone in which this system is set to.

```powershell
The password for oracle2 is the timezone in which this system is set to.

NOTE:
–
 The password is the abbreviation of the timezone. For example, if it is
 listed as being in the Eastern timezone, the answer is est.
– The password will be lowercase no matter how it appears on the screen.
```

To check the time zone, we shall use the following command.

```powershell
Get-TimeZone
```

This command retrives timezone details of the system. Below is the output of the command.

```powershell
PS C:\users\Oracle1\desktop> get-timezone

Id                         : [REDACTED]
DisplayName                : (UTC) Coordinated Universal Time
StandardName               : Coordinated Universal Time
DaylightName               : Coordinated Universal Time
BaseUtcOffset              : 00:00:00
SupportsDaylightSavingTime : False

PS C:\users\Oracle1\desktop>
```

## Oracle level 2-3

For this level, we are required to get the md5 hash of the files on the desktop that have the same hash, then get the last 5 alphanumeric digits to use as the password for the next level.

```powershell
The password for oracle3 is the last five digits of the MD5 hash, from the hashes of files on the desktop that appears twice.

NOTE:
– The password will be lowercase no matter how it appears on the screen. 
```

We shall use the command below to get the md5hash of the files then look which hashes are the same 

```powershell
et-ChildItem -File | Get-FileHash -Algorithm MD5 | Sort-Object Hash
```

`Get-ChildItem -File`

- Lists all items in the current directory
- `File` switch limits results to only files (excludes folders)
- Without parameters, would show both files and directories

`| Get-FileHash -Algorithm MD5`

- Pipes the files to the `Get-FileHash` cmdlet
- Calculates MD5 hash for each file
- `Algorithm MD5` specifies the hash algorithm (alternatives include SHA1, SHA256, etc.)

`| Sort-Object Hash`

- Pipes the results to sort by the Hash property
- Arranges output in alphabetical/numerical order based on MD5 hash values
- Makes it easier to identify duplicate files (which will have identical hashes)

Here is the output of the command.

```powershell
PS C:\users\Oracle2\desktop> Get-ChildItem -File | Get-FileHash -Algorithm MD5 | Sort-Object Hash

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
MD5             0E848B375C2871DBFE0AD405B58BF4E2                                       C:\users\Oracle2\desktop\file19.txt
MD5             1463397F8FDB4B99CDE3DB0B1E37EA6E                                       C:\users\Oracle2\desktop\file11.txt
MD5             1C579B4F21EB236E0CC7ABBB0313AE4C                                       C:\users\Oracle2\desktop\file15.txt
MD5             1E0054A7A7C6D8C820C7307F94513E50                                       C:\users\Oracle2\desktop\file21.txt
MD5             226F590B023BF532FBEEB46154288644                                       C:\users\Oracle2\desktop\file10.txt
MD5             3444EAB3BB3F80522031104151DAADA5                                       C:\users\Oracle2\desktop\file7.txt
MD5             3705066150D7BF296F1E0F0EDC0DB9FA                                       C:\users\Oracle2\desktop\file6.txt
MD5             39A77E07A13922A8C971EB0BEEFFCEE3                                       C:\users\Oracle2\desktop\file8.txt
MD5             3E00A2BF4B49C7D18DBA79DD39C4B19C                                       C:\users\Oracle2\desktop\file18.txt
MD5             41E65125606DE228B94CC2C97B401C1A                                       C:\users\Oracle2\desktop\file20.txt
MD5             4A22F5027B6E3C09C9743DB955B6878A                                       C:\users\Oracle2\desktop\file2.txt
MD5             4CEB4AAE0231B53834280CC5314FB932                                       C:\users\Oracle2\desktop\file1.txt
MD5             5BE11FF0037EED156F77213658C2F5C4                                       C:\users\Oracle2\desktop\file16.txt
MD5             5BE11FF0037EED156F77213658C2F5C4                                       C:\users\Oracle2\desktop\file.txt
MD5             67CE823F3BCC2BCA22ADEB066160CF54                                       C:\users\Oracle2\desktop\file5.txt
MD5             BA5DACAC4B8791D0A10C606C7DCCD10C                                       C:\users\Oracle2\desktop\file14.txt
MD5             C6DDC40861E16E60E924C34ED00F787B                                       C:\users\Oracle2\desktop\file3.txt
MD5             CABBDEAF260BF1FE922A8605D4DDD2BE                                       C:\users\Oracle2\desktop\file9.txt
MD5             F0483A1F2E8A20412DBBFC12F99C1193                                       C:\users\Oracle2\desktop\file12.txt
MD5             FAB743AE9D2C15F5B84975A891133CCB                                       C:\users\Oracle2\desktop\file13.txt
MD5             FD74053193C5E5984E905FBDAD1D61BF                                       C:\users\Oracle2\desktop\file17.txt
MD5             FEB6246F13187BFBCA82EB9105B04B61                                       C:\users\Oracle2\desktop\file4.txt

PS C:\users\Oracle2\desktop>
```

As seen above, we can now be able to identify the files with the same hash by looking through the hashes.

## Oracle level 3-4

The goal for this level is to go through the event logs and get the date which the event logs were last wiped.

```powershell
The password for oracle4 is the date that the system logs were last wiped as depicted in the event logs on the desktop.

NOTE:
– The format for the password is 2 digit month, 2 digit day, 4 digit year. Ex: 5 Jan 2015 would be 01/05/2015.
```

For easier identification, we can search for the [event id](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-1102) that signifies when audit logs were cleared.

![image.png](image%201.png)

With this information, we can now search for the event ID and get the date using the command below.

```powershell
Get-WinEvent -Path .\Oracle3_Security.evtx
| where-object {$_.Id -Eq 1102}
```

`Get-WinEvent -Path .\Oracle3_Security.evtx`

- Reads events from an exported Windows Event Log file named "Oracle3_Security.evtx"
- `.evtx` is the standard extension for Windows Event Log files
- The file is located in the current working directory (`.\`)

`| Where-Object {$_.Id -Eq 1102}`

- Filters the events to only show those with Event ID 1102
- `$_.Id` refers to the EventID property of each log entry
- `Eq` is the equality comparison operator

Here is the output of the command

```powershell
PS C:\users\Oracle3\desktop> Get-WinEvent -Path .\Oracle3_Security.evtx
| where-object {$_.Id -Eq 1102}

   ProviderName: Microsoft-Windows-Eventlog

TimeCreated                     Id LevelDisplayName Message
-----------                     -- ---------------- -------
[REDACTED] 11:36:05 PM          1102 Information      The audit log w...

PS C:\users\Oracle3\desktop>
```

With that, we now have the password for our newt level.

## Oracle level 4-5

For this challenge, we need to find the name of the `GPO(Group policy object)` that was last created then we combine it with the name of the file on the desktop to get the password for level 5

```powershell
The password for oracle5 is the name of the GPO that was last created PLUS the name of the file on the user’s desktop.

NOTE:
– If the GPO name is “blob” and the file on the desktop is named “1234”, the password would be “blob1234”.
– The password will be lowercase no matter how it appears on the scree
```

Below is the listing of the file on the desktop.

```powershell
PS C:\users\Oracle4\desktop> ls

    Directory: C:\users\Oracle4\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:49 AM              0 83

PS C:\users\Oracle4\desktop>
```

To find the name of the GPO that was last created, we shall use the following commands.

```powershell
 get-gpo -all | sort-object CreationTime -descending | select-object -first 1
```

`Get-GPO -All`

- Retrieves all Group Policy Objects (GPOs) in the Active Directory domain
- The `All` parameter ensures all GPOs are returned, including unlinked ones
- Without parameters, it would only return GPOs in the current domain

`| Sort-Object CreationTime -Descending`

- Pipes the GPOs to be sorted by their creation timestamp
- `Descending` sorts from newest to oldest
- This puts the most recently created GPO at the top of the list

`| Select-Object -First 1`

- Selects only the first (top) item from the sorted list
- This gives us the single most recently created GPO

Here is the output of the command.

```powershell
PS C:\users\Oracle4\desktop> get-gpo -all | sort-object CreationTime -d
escending | select-object -first 1

DisplayName      : [REDACTED]
DomainName       : underthewire.tech
Owner            : underthewire\Domain Admins
Id               : 49401c32-4145-463f-b5e7-816926d4f78d
GpoStatus        : AllSettingsEnabled
Description      : Are you there?
CreationTime     : 1/13/2019 9:40:20 PM
ModificationTime : 1/13/2019 9:40:20 PM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        :

PS C:\users\Oracle4\desktop>
```

With that, we now have the password for our next level.

## Oracle level 5-6

For this level, we are required to find the GPO with the description `I_AM_oracle` and combine it with the name of the file on the desktop

```powershell
The password for oracle6 is the name of the GPO that contains a description of “I_AM_GROOT” PLUS the name of the file on the user’s desktop.

NOTE:
–
 If you are using SSH, you MUST do a Help on the cmdlet needed to solve 
this. For example, if the cmdlet is “get-something” type “help 
get-something” first, this will make the cmdlet available for you to 
use. This is a bug in the SSH software used.
– If the GPO description is “blob” and the file on the desktop is named “1234”, the password would be “blob1234”.
– The password will be lowercase no matter how it appears on the screen.
```

The file on the desktop is as shown below.

```powershell
PS C:\users\Oracle5\desktop> ls

    Directory: C:\users\Oracle5\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:50 AM              0 1337

PS C:\users\Oracle5\desktop>
```

To get the GPO, we shall use the following command as shown below.

```powershell
get-gpo -all | where-object {$_.Description -eq "I_AM_GROOT"}
```

`Get-GPO -All`

- Retrieves all Group Policy Objects in the Active Directory domain
- The `All` parameter ensures it returns every GPO, including:
    - Linked and unlinked GPOs
    - Enabled and disabled GPOs
    - GPOs in all domains (if run in a forest with multiple domains)

`| Where-Object {$_.Description -eq "I_AM_GROOT"}`

- Filters the GPOs to only those with an exact description match to "I_AM_GROOT"
- `$_.Description` accesses the Description property of each GPO
- `eq` performs an exact case-sensitive comparison

Here is the command output.

```powershell
PS C:\users\Oracle5\desktop> get-gpo -all | where-object {$_.Descriptio
n -eq "I_AM_GROOT"}

DisplayName      :[REDACTED]
DomainName       : underthewire.tech
Owner            : underthewire\Domain Admins
Id               : 44080cf1-1053-467d-b000-2ea3f27dbbfa
GpoStatus        : AllSettingsEnabled
Description      : I_am_Groot
CreationTime     : 11/20/2018 12:18:09 AM
ModificationTime : 11/20/2018 12:18:08 AM
UserVersion      : AD Version: 0, SysVol Version: 0
ComputerVersion  : AD Version: 0, SysVol Version: 0
WmiFilter        :

PS C:\users\Oracle5\desktop>
```

## Oracle level 6-7

Below is the description of the challenge.

```powershell
The password for oracle7 is the name of the OU that doesn’t have a GPO linked to it PLUS the name of the file on the user’s desktop.

NOTE:
– The password will be lowercase no matter how it appears on the screen. 
– Exclude the “Groups” OU.
```

Below is the file on the desktop.

```powershell
PS C:\users\Oracle6\desktop> ls

    Directory: C:\users\Oracle6\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:50 AM              0 _97

PS C:\users\Oracle6\desktop>
```

To get the name of the OU that does not have a GPO linked to it, we shall use the command below.

```powershell
get-adorganizationalunit -filter * | where
-object {-not $_.LinkedGroupPolicyObjects}
```

`Get-ADOrganizationalUnit -Filter *`

- Retrieves all Organizational Units from Active Directory
- `Filter *` means "no filter" (get all OUs)
- Without this, it would only return a default set of properties

`| Where-Object {-not $_.LinkedGroupPolicyObjects}`

- Filters the OUs to only show those without linked GPOs
- `$_.LinkedGroupPolicyObjects` checks the LinkedGroupPolicyObjects property
- `not` returns OUs where this property is empty/null
- The property normally contains Distinguished Names of linked GPOs

Here is the output of the command

```powershell
PS C:\users\Oracle6\desktop> get-adorganizationalunit -filter * | where
-object {-not $_.LinkedGroupPolicyObjects}

City                     :
Country                  :
DistinguishedName        : OU=T-50,OU=X-Wing,DC=underthewire,DC=tech
LinkedGroupPolicyObjects : {}
ManagedBy                :
Name                     : [REDACTED]
ObjectClass              : organizationalUnit
ObjectGUID               : 5ace8bef-c00e-4f58-a543-3fd45436f1d4
PostalCode               :
State                    :
StreetAddress            :

City                     :
Country                  :
DistinguishedName        : OU=Groups,DC=underthewire,DC=tech
LinkedGroupPolicyObjects : {}
ManagedBy                :
Name                     : Groups
ObjectClass              : organizationalUnit
ObjectGUID               : bf366f71-f291-43ca-8334-cdb18890e332
PostalCode               :
State                    :
StreetAddress            :

PS C:\users\Oracle6\desktop>
```

With that, we have the password to the next level.

## Oracle level 7-8

To find the password for the next level, we need to find the name of the domain, where trust is built, then combine it with the name of the file that is on the desktop.

```powershell
The password for oracle8 is the name of the domain that a trust is built with PLUS the name of the file on the user’s desktop. 
NOTE:
– The password will be lowercase no matter how it appears on the screen.
– If the name of the trust is “blob” and the file on the desktop is named “1234”, the password would be “blob1234”.
```

Below is the name of the file that is on the desktop.

```powershell
PS C:\users\Oracle7\desktop> ls

    Directory: C:\users\Oracle7\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:50 AM              0 111

PS C:\users\Oracle7\desktop>
```

To find the name of the domain that has trust built in it, we shall use the following command.

```powershell
get-adtrust -filter *
```

`Get-ADTrust`

- The cmdlet that retrieves Active Directory trust objects
- Part of the Active Directory module (requires RSAT tools on workstations)

`-Filter *`

- Returns all trust relationships (no filtering)
- The asterisk () is a wildcard meaning "match everything"

Here is the output of the command.

```powershell
PS C:\users\Oracle7\desktop> get-adtrust -filter *

Direction               : Outbound
DisallowTransivity      : True
DistinguishedName       : CN=multiverse,CN=System,DC=underthewire,DC=t
                          ech
ForestTransitive        : False
IntraForest             : False
IsTreeParent            : False
IsTreeRoot              : False
Name                    : [REDACTED]
ObjectClass             : trustedDomain
ObjectGUID              : bbfcc0ca-e586-4058-9aef-c6b4a6b32708
SelectiveAuthentication : False
SIDFilteringForestAware : False
SIDFilteringQuarantined : False
Source                  : DC=underthewire,DC=tech
Target                  : multiverse
TGTDelegation           : False
TrustAttributes         : 1
TrustedPolicy           :
TrustingPolicy          :
TrustType               : MIT
UplevelOnly             : False
UsesAESKeys             : False
UsesRC4Encryption       : False

PS C:\users\Oracle7\desktop>
```

## Oracle level 8-9

Below is the objective of the challenge.

```powershell
The password for oracle9 is the name of the file in the GET Request from
 www.guardian.galaxy.com within the log file on the desktop.

NOTE:
– Don’t include the extension.
– The password will be lowercase no matter how it appears on the screen.
```

As described above, we are required to get the name of the file in the GET request form `www.guardian.galaxy.com` within the log file on the desktop.

To get the file, we shall use the following command.

```powershell
 type .\logs.txt | out-string -stream | select-string -patern "guardian"
```

`type .\logs.txt`

- This displays the contents of the `logs.txt` file located in the current directory (`.\`).
- Equivalent to `Get-Content .\logs.txt`.

`| out-string -stream`

- This takes the output of `type` and converts each line to a string object without merging the lines into one block (thanks to `stream`).
- Useful for ensuring that `Select-String` receives clean line-by-line string input, especially when previous commands might output objects.

`| select-string -pattern "guardian"`

- This searches each line of the text for the pattern "guardian" (case-insensitive by default unless `CaseSensitive` is added).
- Returns the line(s) that match, highlighting the matching word.

Here is the output of the command

```powershell
PS C:\users\Oracle8\desktop> type .\logs.txt | out-string -stream | sele
ct-string -pattern "guardian"

guardian.galaxy.com - - [28/Jul/1995:13:03:55 -0400] "GET
/images/[REDACTED].gif HTTP/1.0" 200 786

PS C:\users\Oracle8\desktop>
```

## Oracle level 9-10

Below is the objective and description of the challenge.

```powershell
The password for oracle10 is the computer name of the DNS record of the mail server listed in the UnderTheWire.tech zone PLUS the name of the file on the user’s desktop.

NOTE:
– If the server name is “some_blob” and the file on the desktop is named “1234”, the password would be “some_blob1234”.
– Only submit the computer name and not the fully qualified domain name.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the file located on the desktop of the user oracle9

```powershell
PS C:\users\Oracle9\desktop> ls

    Directory: C:\users\Oracle9\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 9229

```

To find the computer name of the DNS record of the mail server listed in the `Underthewire,tech` zone, we shall use the command below.

```powershell
get-dnsserverresourcerecord -zonename 'und
erthewire.tech'
```

`Get-DnsServerResourceRecord`:

- This PowerShell cmdlet is part of the DnsServer module and is used to get all or specific DNS records from a DNS server for a given zone.

`ZoneName 'underthewire.tech'`:

- Specifies the DNS zone from which to retrieve the records. In this case, it's querying the `underthewire.tech` DNS zone.

Here is the command output.

```powershell
PS C:\users\Oracle9\desktop> get-dnsserverresourcerecord -zonename 'und
erthewire.tech'

HostName                  RecordType Type       Timestamp            T
                                                                     i
                                                                     m
                                                                     e
                                                                     T
                                                                     o
                                                                     L
                                                                     i
                                                                     v
                                                                     e
--------                  ---------- ----       ---------            -
@                         A          1          4/25/2025 4:00:00 AM 0
@                         NS         2          0                    0
@                         SOA        6          0                    0
_msdcs                    NS         2          0                    0
_gc._tcp.Default-First... SRV        33         4/25/2025 2:00:00 AM 0
_kerberos._tcp.Default... SRV        33         4/25/2025 2:00:00 AM 0
_ldap._tcp.Default-Fir... SRV        33         4/25/2025 3:00:00 AM 0
_gc._tcp                  SRV        33         4/25/2025 2:00:00 AM 0
_kerberos._tcp            SRV        33         4/25/2025 4:00:00 AM 0
_kpasswd._tcp             SRV        33         4/25/2025 2:00:00 AM 0
_ldap._tcp                SRV        33         4/25/2025 3:00:00 AM 0
_kerberos._udp            SRV        33         4/25/2025 2:00:00 AM 0
_kpasswd._udp             SRV        33         4/25/2025 2:00:00 AM 0
CYBORG718W100N            A          1          0                    0
DomainDnsZones            A          1          4/25/2025 2:00:00 AM 0
_ldap._tcp.Default-Fir... SRV        33         4/25/2025 2:00:00 AM 0
_ldap._tcp.DomainDnsZones SRV        33         4/25/2025 2:00:00 AM 0
ForestDnsZones            A          1          4/25/2025 2:00:00 AM 0
_ldap._tcp.Default-Fir... SRV        33         4/25/2025 2:00:00 AM 0
_ldap._tcp.ForestDnsZones SRV        33         4/25/2025 2:00:00 AM 0
utw                       A          1          0                    0
[REDACTED]                  MX         15         0                    0

PS C:\users\Oracle9\desktop>
```

From the above output, we need to check the `mx` record to find the computer name of the mail server.

## Oracle level 10-11

For this level, we are required to find the site that the user last visited that ends with `.biz` 

```powershell
The password for oracle11 is the .biz site the user has previously navigated to.

NOTE: 
– Don’t include the extension. 
– The password will be lowercase no matter how it appears on the screen.
```

For this case, we need to check the registry key for [TypedURLs](http://sketchymoose.blogspot.co.uk/2014/02/typedurls-registry-key.html) since this registry stores Internet Explorer’s cached history and is very valuable in forensics investigations.

We shall use the following command.

```powershell
 get-itemproperty -Path "HKCU:\Software\Mi
crosoft\Internet Explorer\TypedURLs"
```

`Get-ItemProperty`:

- A PowerShell cmdlet used to get the properties (i.e., name–value pairs) of a registry key or file system item.

`-Path "HKCU:\Software\Microsoft\Internet Explorer\TypedURLs"`:

- Specifies the registry path to the TypedURLs key, which is found under:

Here is the output of the command.

```powershell
PS C:\users\Oracle10\desktop> get-itemproperty -Path "HKCU:\Software\Mi
crosoft\Internet Explorer\TypedURLs"

url1         : http://go.microsoft.com/fwlink/p/?LinkId=255141
url2         : http://google.com
url3         : http://underthewire.tech
url4         : http://bimmerfest.com
url5         : http://nba.com
url6         : http://[REDACTED].biz
url7         : http://hardknocks.edu
url8         : http://installation.org
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\S
               oftware\Microsoft\Internet Explorer\TypedURLs
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\S
               oftware\Microsoft\Internet Explorer
PSChildName  : TypedURLs
PSDrive      : HKCU
PSProvider   : Microsoft.PowerShell.Core\Registry

PS C:\users\Oracle10\desktop>
```

## Oracle level 11-12

The password for the next level, we are told that the letter associated with the mapped driver that user oracle11 has.

```powershell
The password for oracle12 is the drive letter associated with the mapped drive that this user has. 
 
NOTE: 
– Submission should be one letter and lowercase. 
```

To get the drive letter, we shall use the command below.

```powershell
get-smbmapping
```

`Get-SmbMapping` displays active SMB (Server Message Block) network share mappings on the local machine.

In simpler terms, it shows which network folders (shared drives) are currently connected/mapped via SMB — the protocol used for Windows file sharing.

Here is the output of the command.

```powershell
PS C:\users\Oracle11\desktop> get-smbmapping

Status      Local Path Remote Path
------      ---------- -----------
Unavailable [REDACTED]         \\127.0.0.1\WsusContent

PS C:\users\Oracle11\desktop>
```

## Oracle level 12-13

For this level, we are required to find the IP of the system that user oracle12 has established remote desktop with.

```powershell
The password for oracle13 is the IP of the system that this user has previously established a remote desktop with.
```

For this challenge, we need to check the [terminal server client registry](http://woshub.com/how-to-clear-rdp-connections-history) to find the IP. We shall use the command below.

```powershell
 get-childitem -Path "HKCU:\Software\Micro
soft\Terminal Server Client"
```

`Get-ChildItem`:

- This cmdlet is like `ls` or `dir` — it lists items (in this case, registry keys) at a specified path.

`Path "HKCU:\Software\Microsoft\Terminal Server Client"`:

Specifies the registry location in the HKEY_CURRENT_USER (HKCU) hive.

This particular path stores information related to the Remote Desktop Client (mstsc.exe), such as:

- Recently connected IPs or hostnames
- Connection bar settings
- Display preferences

Here is the command output.

```powershell
PS C:\users\oracle12\desktop> get-childitem -Path "HKCU:\Software\Micro
soft\Terminal Server Client"

    Hive: HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client

Name                           Property
----                           --------
[REDACTED]                    UsernameHint : MyServer\raccoon

PS C:\users\oracle12\desktop>
```

## Oracle level 13-14

As described below, is the objective of the challenge.

```powershell
The
 password for oracle14 is the name of the user who created the Galaxy 
security group as depicted in the event logs on the desktop PLUS the name of the text file on the user’s desktop. 
NOTE: 
– If the user’s name is “randy” and the file on the desktop is named “1234”, the password would be “randy1234”. 
– The password will be lowercase no matter how it appears on the screen.
```

We need to find the name of the user who created the galaxy security group then combine it with the name of the text file on the user’s desktop.

Below is the name of the file.

```powershell
PS C:\users\Oracle13\desktop> ls

    Directory: C:\users\Oracle13\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 88
-a----        8/30/2018   5:52 AM        2166784 security.evtx

PS C:\users\Oracle13\desktop>
```

To find the name of the user, we shall use the following command.

```powershell
Get-WinEvent -Path .\security.evtx | Where-Object {$_.Message -like '*group*created*'} | Select-Object -ExpandProperty Message
```

`Get-WinEvent -Path .\security.evtx`

- Reads a saved event log file named `security.evtx` from the current directory (`.\`).
- `Get-WinEvent` is used for reading event logs — either from live system logs or `.evtx` log files.
- This pulls in all events from that log file.

`| Where-Object {$_.Message -like '*group*created*'}`

- Filters the events where the `Message` property (the human-readable explanation of the event) contains both "group" and "created" in that order.
- The `like` operator uses wildcards (), so this matches any string that includes "group", followed at some point by "created".
- Example match: `"A security-enabled local group was created."`

`| Select-Object -ExpandProperty Message`

- Instead of returning the full event object, this pulls out just the `Message` field.
- `ExpandProperty` unwraps it to plain strings (not objects).

Here is the output of the command.

```powershell
PS C:\users\Oracle13\desktop> Get-WinEvent -Path .\security.evtx | Wher
e-Object {$_.Message -like '*group*created*'} | Select-Object -ExpandPr
operty Message
A security-enabled global group was created.

Subject:
        Security ID:            S-1-5-21-2268727836-2773903800-29522480
01-1621
        Account Name:           [REDACTED]
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0xBC24FF

New Group:
        Security ID:            S-1-5-21-2268727836-2773903800-29522480
01-1626
        Group Name:             Galaxy
        Group Domain:           UNDERTHEWIRE

Attributes:
        SAM Account Name:       Galaxy
        SID History:            -

Additional Information:
        Privileges:             -
A security-enabled global group was created.

Subject:
        Security ID:            S-1-5-21-2268727836-2773903800-29522480
01-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0xB6B6FE

New Group:
        Security ID:            S-1-5-21-2268727836-2773903800-29522480
01-1625
        Group Name:             Guardian
        Group Domain:           UNDERTHEWIRE

Attributes:
        SAM Account Name:       Guardian
        SID History:            -

Additional Information:
        Privileges:             -
PS C:\users\Oracle13\desktop>
```

## Oracle level 14-15

Below is a description and objective of the challenge.

```powershell
The password for oracle15 is the name of the user who added the user Bereet to the Galaxy security group as depicted in the event logs on the 
desktop PLUS the name of the text file on the user’s desktop. 

NOTE: 
– If the script name is “randy” and the file on the desktop is named “1234”, the password would be “randy1234”. 
– The password will be lowercase no matter how it appears on the screen.
```

For this challenge, we need to find the name of the user who added the user `Bereet` to the Galaxy security group then combine it with the name of the file on the desktop.

```powershell
The password for oracle15 is the name of the user who added the user Bereet to the Galaxy security group as depicted in the event logs on the 
desktop PLUS the name of the text file on the user’s desktop. 

NOTE: 
– If the script name is “randy” and the file on the desktop is named “1234”, the password would be “randy1234”. 
– The password will be lowercase no matter how it appears on the sc
```

Below is the file located on the desktop.

```powershell
PS C:\users\Oracle14\desktop> ls

    Directory: C:\users\Oracle14\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:51 AM              0 2112
-a----        8/30/2018   5:52 AM        2166784 security.evtx

PS C:\users\Oracle14\desktop>
```

To find the name of the user who added the user, we have two event IDs that are of interest to us [632](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=632) and [4728](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4728). With this two events in mind, we shall use the command below to find our user.

```powershell
Get-WinEvent -Path ..\Desktop\Security.evtx | where {$_.Id -Eq 632 -OR $_.Id -Eq 4728} | Format-List -Property Message | Out-String -Stream | Select-String -Pattern "Bereet" -Context 8,1
```

`Get-WinEvent -Path ..\Desktop\Security.evtx` 

- Loads a saved Windows Security Event Log file named `Security.evtx` located on the desktop of the parent directory (`..`).
- Pulls all events from the file.

`where {$*.Id -eq 632 -or $*.Id -eq 4728}`

- Filters events to include only those with Event ID 632 or 4728.
    - 632 (Legacy): A user was added to a group.
    - 4728: A user was added to a privileged group (e.g., Administrators, Domain Admins).
- This step focuses on group membership modifications.

`Format-List -Property Message` 

- Formats each event to only show the `Message` field in list format (each property on its own line).
- This is useful for readable, line-by-line text processing.

`Out-String -Stream` 

- Converts the formatted output to a stream of plain strings (one per line).
- This makes it possible to process it with `Select-String`.

`Select-String -Pattern "Bereet" -Context 8,1`

- Searches for lines that contain the word `"Bereet"`.
- `Context 8,1`:
    - Shows 8 lines before and 1 line after each match, giving you useful context around where `"Bereet"` appears.

Here is the output of the command.

```powershell
PS C:\users\Oracle14\desktop> Get-WinEvent -Path ..\Desktop\Security.ev
tx | where {$_.Id -Eq 632 -OR $_.Id -Eq 4728} | Format-List -Property M
essage | Out-String -Stream | Select-String -Pattern "Bereet" -Context
8,1

            S-1-5-21-2268727836-2773903800-2952248001-1622
                Account Name:           [REDACTED]
                Account Domain:         UNDERTHEWIRE
                Logon ID:               0xBD8CC7

            Member:
                Security ID:
            S-1-5-21-2268727836-2773903800-2952248001-1623
>               Account Name:           CN=Bereet,OU=Morag,DC=UNDERTHEW
IRE,DC=TECH

PS C:\users\Oracle14\desktop>
```

With that we have our password for level 15.

## Oracle level 15

We are done.

```powershell
Congratulations! 

You have successfully made it to the end! 
Try your luck with other games brought to you by the Under The Wire team. 

Thanks for playing!
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\Oracle15\desktop>

:) :o
 
```