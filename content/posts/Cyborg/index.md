---
title: "Underthewire cyborg war games"
date: 2025-06-10
description: This is a writeup on cyborg from underthewire wargames.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "underthewire wargames cyborg series" # Here you can write a small summary of the post if needed
tags: [powershell, SSH]
categories: [Underthewire, AD]
---

## Cyborg level 0-1

For this challenge, we are required to just log in into the platform with creds from the Slack channel.

```powershell
The goal of this level is to log into the game. Do the following in order to achieve this goal.

1. Obtain the initial credentials via the #StartHere channel on our Slack (link). Once you are in the channel, scroll to the top to see the credentials.

2.
 After obtaining the credentials, connect to the server via SSH. You 
will need an SSH client such as Putty. The host that you will be 
connecting to is cyborg.underthewire.tech, on port 22.

3. When prompted, use the credentials for the applicable game found in the #StartHere Slack channel.

4. You have successfully connected to the game server when your path changes to “PS C:\Users\Cyborg1\desktop>”.
```

Below is a screenshot showing the credentials from the slack channel.

![image.png](image.png)

With the creds obtained, we can ssh to our instance. Below is a shell to the instance from my computer.

```powershell
C:\Users\Victo\Desktop\underthewire>ssh cyborg1@cyborg.underthewire.tech
cyborg1@cyborg.underthewire.tech's password:

Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\cyborg1\desktop>

```

## Cyborg level 1-2

Below is the description and objectives of the challenge.

```powershell
The password for cyborg2 is the state that the user Chris Rogers is from as stated within Active Directory.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
– “State” refers to the location within the country and NOT the “state” of the account (enabled/ disabled).

IMPORTANT:
Once
 you feel you have completed the Cyborg1 challenge, start a new 
connection to the server, and log in with the username of Cyborg2 and 
this password will be the answer from Cyborg1. If successful, close out 
the Cyborg1 connection and begin to solve the Cyborg2 challenge. This 
concept is repeated over and over until you reach the end of the game.
 

     ▼ HINT:List the available modules, there may be a useful one available… 
```

As seen above, we are required to know the state in which `Chris Rogers` is from in order to find our password.

We shall use the following commands.

```powershell
 Get-ADUser -Filter * -Properties State | Where-Object { $_.surname -eq "Rogers" } | ft Name, State
```

Below is a breakdown of what the commands above do.

`Get-ADUser -Filter *`

- Gets all user objects from Active Directory
- `Filter *` means "no filter" (get all users)

`Properties State`

- By default, Get-ADUser only returns basic properties
- This includes the 'State' property in the results (which isn't returned by default)

`| Where-Object { $_.surname -eq "Rogers" }`

- Pipes the results to filter for users where the surname equals "Rogers"
- `$_` represents the current object in the pipeline

`| ft Name, State`

- Formats the output as a table (`ft` is an alias for `Format-Table`)
- Only displays the Name and State columns

Below is the output of the command.

```powershell
PS C:\users\cyborg1\desktop> Get-ADUser -Filter * -Properties State | W
here-Object { $_.surname -eq "Rogers" } | ft Name, State

Name           State
----           -----
Rogers, Chris  [REDACTED]
```

## Cyborg level 2-3

From the objective of this challenge, we are required to find the IP address of host A for CYBORG718W100N and join it with the name of the file on the desktop to get the password for level 3 as stated below.

```powershell
The password for cyborg3 is the host A record IP address for CYBORG718W100N PLUS the name of the file on the desktop.

NOTE:
– If the IP is “10.10.1.5” and the file on the desktop is called “_address”, then the password is “10.10.1.5_address”.
– The password will be lowercase no matter how it appears on the screen.
```

We shall use the following command.

```powershell
$($(Resolve-DnsName CYBORG718W100N).IPAddress + $(ls).name)
```

Below is an explanation of what is happening.

 `$(Resolve-DnsName CYBORG718W100N).IPAddress`

- `Resolve-DnsName CYBORG718W100N` - Performs a DNS lookup for the hostname
- `.IPAddress` - Extracts just the IP address from the DNS results
- Wrapped in `$()` - This is a subexpression that gets evaluated first

 `$(ls).name`

- `ls` - Alias for `Get-ChildItem` (lists files in current directory)
- `.name` - Extracts just the filename(s) from the results
- Wrapped in `$()` - Another subexpression evaluated separately

 The `+` operator

- Concatenates (combines) the IP address string with the filename string

 The outer `$()`

- Combines everything into a single expression that outputs the result

Below is the output of the  command.

```powershell
PS C:\users\cyborg2\desktop> $($(Resolve-DnsName CYBORG718W100N).IPAddress + $(ls).name)
[REDACTED]
```

## Cyborg level 3-4

For this level we are required to find the number of users in the cyborg group within active directory and then link it with the name of the file on the desktop.

```powershell
The password for cyborg4 is the number of users in the Cyborg group within Active Directory PLUS the name of the file on the desktop.

NOTE:
– If the number of users is “20” and the file on the desktop is called “_users”, then the password is “20_users”.
– The password will be lowercase no matter how it appears on the screen.
```

Here is the command we shall use to find the number of users in the cyborg group of the active directory.

```powershell
(Get-ADGroupMember -Identity Cyborg).Count
```

Below is a breakdown of the commnad.

`Get-ADGroupMember`

- A cmdlet from the ActiveDirectory module that retrieves members of an AD group
- Requires the AD module (typically installed via RSAT tools)

`Identity Cyborg`

- Specifies which group to examine (in this case, a group named "Cyborg")

`.Count` property

- Returns the number of objects in the collection
- This counts all direct members of the group (not nested group members)

Below is the output of the command

```powershell
PS C:\users\cyborg3\desktop> (Get-ADGroupMember -Identity Cyborg).Count

[REDACTED]
PS C:\users\cyborg3\desktop>
```

We can now check the file on the desktop directory using the command `ls` as shown below.

```powershell
PS C:\users\cyborg3\desktop> ls

    Directory: C:\users\cyborg3\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/26/2022   2:14 PM              0 [REDACTED]

PS C:\users\cyborg3\desktop>
```

And with that we have the password for the next level.

## Cyborg level 4-5

For this challenge, we are required to find the module name with the version number of `8.9.8.9` and then link it with the name of the file on the desktop.

```powershell
The password for cyborg5 is the PowerShell module name with a version number of 8.9.8.9 PLUS the name of the file on the desktop.

NOTE:
– If the module name is “bob” and the file on the desktop is called “_settings”, then the password is “bob_settings”.
– The password will be lowercase no matter how it appears on the screen.
```

We shall be using the following command to find the module with the version number.

```powershell
Get-Module -ListAvailable | Where-Object { $_.Version -eq "8.9.8.9" }
```

Below is a break-down of the command.

`Get-Module -ListAvailable`

- `Get-Module` - Retrieves PowerShell modules
- `ListAvailable` - Shows all modules installed on the system (not just currently imported ones)
- Without this switch, it would only show currently loaded modules

`|` (Pipeline)

- Takes the output from `Get-Module` and passes it to the next command

`Where-Object { $_.Version -eq "8.9.8.9" }`

- Filters the modules to find only those with exact version 8.9.8.9
- `$_` represents each module object as it comes through the pipeline
- `eq` performs an exact match comparison

Below is the output of the command, followed by the listing command `ls` to get the file on the desktop directory.

```powershell
PS C:\users\cyborg4\desktop> get-module -listavailable | where-object { $_.Ver
sion -eq "8.9.8.9"}

    Directory: C:\Windows\system32\WindowsPowerShell\v1.0\Modules

ModuleType Version    Name                                ExportedComm
                                                          ands
---------- -------    ----                                ------------
Manifest   8.9.8.9    [REDACTED]                               Get-bacon

PS C:\users\cyborg4\desktop> ls

    Directory: C:\users\cyborg4\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:45 AM              0 _eggs
```

## Cyborg level 5-6

For this level, we are required to find the user who has their logon hours set on their account, with that data, we then use it to link it with the name of the file on the desktop of the user cyborg5 in order to get the password for level 6.

```powershell
The password for cyborg6 is the last name of the user who has logon hours set on their account PLUS the name of the file on the desktop.

NOTE:
– If the last name is “fields” and the file on the desktop is called “_address”, then the password is “fields_address”.
– The password will be lowercase no matter how it appears on the screen.
```

Here is the command I used for this case.

```powershell
 Get-ADUser -Filter * -Properties logonhour
s | Where-Object { ($null -ne $_.logonhours) -and ($null -ne $_.surname
) }
```

Below is the breakdown of the command.

 `Get-ADUser -Filter * -Properties logonhours`

- Retrieves all Active Directory user accounts (`Filter *`)
- Includes the `logonhours` property (which isn't returned by default)
- The `logonhours` property contains the permitted logon hours for each user

 `Where-Object` Filter

The pipeline filters users to only those who meet both conditions:

1. `$null -ne $_.logonhours` - User has logon hours restrictions configured
2. `$null -ne $_.surname` - User has a surname (last name) value populated

Below is the output of the command.

```powershell
PS C:\users\cyborg5\desktop> Get-ADUser -Filter * -Properties logonhour
s | Where-Object { ($null -ne $_.logonhours) -and ($null -ne $_.surname
) }

DistinguishedName : CN=Rowray\, Benny  \
                    ,OU=T-85,OU=X-Wing,DC=underthewire,DC=tech
Enabled           : False
GivenName         : Benny
logonhours        : {0, 0, 0, 0...}
Name              : Rowray, Benny
ObjectClass       : user
ObjectGUID        : c9aad4f3-3e4f-46b5-84db-2bb7105796dd
SamAccountName    : Benny.Rowray
SID               : S-1-5-21-758131494-606461608-3556270690-1647
Surname           : [REDACTED]
UserPrincipalName : Benny.Rowray

PS C:\users\cyborg5\desktop>

```

We can then use the `ls` command to list the contents of the desktop directory.

```powershell
PS C:\users\cyborg5\desktop> ls

    Directory: C:\users\cyborg5\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:45 AM              0 [REDACTED]

PS C:\users\cyborg5\desktop>
```

With that, we have the password for the next level.

## Cyborg level 6-7

For this challenge, we are required to find the decoded text of the string within the file on the desktop in order to get the password for level 7.

```powershell
The password for cyborg7 is the decoded text of the string within the file on the desktop.

NOTE:
– The password is the last word of the string. For example, if it is “I like PowerShell”, the password would be “powershell”.
– The password will be lowercase no matter how it appears on the screen.
– There are no spaces in the answer.
```

The contents of the file are encoded using base64 as shown below.

```powershell

PS C:\users\cyborg6\desktop> type ".\cypher.txt"
[REDACTED]
PS C:\users\cyborg6\desktop>
```

We are required to decode this text to get the password for the next level. Below is the command I used to decode the text.

```powershell
[System.Text.Encoding]::UTF8.GetString([Sy
stem.Convert]::FromBase64String($(gc .\cypher.txt)))
```

Below is a breakdown of the command.

`gc .\cypher.txt`

- `gc` is the alias for `Get-Content`
- Reads the content of the file `cypher.txt` in the current directory

`[System.Convert]::FromBase64String()`

- Takes the Base64-encoded string from the file
- Converts it to a byte array (the raw binary data)

`[System.Text.Encoding]::UTF8.GetString()`

- Takes the byte array from the previous step
- Decodes it into a UTF-8 string (human-readable text)

Below is the output of the command.

```powershell
PS C:\users\cyborg6\desktop> [System.Text.Encoding]::UTF8.GetString([Sy
stem.Convert]::FromBase64String($(gc .\cypher.txt)))
[REDACTED]
PS C:\users\cyborg6\desktop>
```

With the hash we got, we can use online tools too to decrypt the hash to get the password as well.

## Cyborg level 7-8

For this challenge, we are required to find the binary that automatically starts when cyborg7 logs in to the system. The binary name is the password for level 8.

```powershell
The password for cyborg8 is the executable name of a program that will start automatically when cyborg7 logs in.
NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

We shall use this command as shown below.

```powershell
Get-WmiObject Win32_StartupCommand | Select-Object Name, command, Location, User  | Format-List
```

What this command does is as explained below.

`Get-WmiObject Win32_StartupCommand`

- Uses the WMI (Windows Management Instrumentation) class `Win32_StartupCommand`
- Retrieves all programs configured to run at system startup or user login

`| Select-Object Name, command, Location, User`

- Filters the output to show only these specific properties:
    - `Name`: The display name of the startup item
    - `Command`: The actual command/program that runs
    - `Location`: Where the startup entry is configured (registry, startup folder, etc.)
    - `User`: Which user account the startup item applies to

`| Format-List`

- Displays the results in a vertical list format (one property per line)
- Alternative to the default table view (`Format-Table`)

Below is the output of the command.

```powershell
PS C:\users\cyborg7\desktop> Get-WmiObject Win32_StartupCommand | Selec
t-Object Name, command, Location, User  | Format-List

Name     : [REDACTED]
command  : C:\program files\SkyNet\skynet.exe
Location : HKU\S-1-5-21-758131494-606461608-3556270690-1140\SOFTWARE\M
           icrosoft\Windows\CurrentVersion\Run
User     : underthewire\cyborg7

```

## Cyborg level 8-9

For this level, we are required to get the internet zone from which the image on the desktop was downloaded from. This is the password for level 9.

```powershell
The password for cyborg9 is the Internet zone that the picture on the desktop was downloaded from.
NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

Zone information is usually recorded in the `Zone.Identifier` data stream. We can now easily view the stream using the command below.

```powershell
 get-content .\1_qs5nwlcl7f_-SwNlQvOrAw.png
 -stream Zone.Identifier
```

Below is a breakdown of the command.

`Get-Content`

- PowerShell cmdlet for reading file contents

`.\1_qs5nwlcl7f_-SwNlQvOrAw.png`

- Specifies the PNG image file in the current directory

`Stream Zone.Identifier`

- Accesses the alternate data stream (ADS) named "Zone.Identifier"
- This is a special hidden metadata stream that Windows adds to downloaded files

Below is the output of the command.

```powershell
PS C:\users\cyborg8\desktop> get-content .\1_qs5nwlcl7f_-SwNlQvOrAw.png
 -stream Zone.Identifier
[ZoneTransfer]
ZoneId=[REDACTED]
```

## Cyborg level 9-10

For this level, we need to find the active directory user with a phone number `876-5309` to find their first name and then concatenate it with the name of the file on the desktop of the user cyborg9.

```powershell
The password for cyborg10 is the first name of the user with the phone number of 876-5309 listed in Active Directory PLUS the name of the file on the desktop.

NOTE:
– If the first name “chris” and the file on the desktop is called “23”, then the password is “chris23”.
– The password will be lowercase no matter how it appears on the screen.
```

I used the following command to get the name of the user.

```powershell
 (Get-ADUser -Properties officephone -Filte
r * | ? {$_.officephone -eq "876-5309"}).GivenName
```

Below is the breakdown of the command.

`Get-ADUser -Properties officephone -Filter *`

- `Get-ADUser`: Retrieves user accounts from Active Directory
- `Properties officephone`: Includes the office phone number in results (not returned by default)
- `Filter *`: Returns all user accounts (no filtering at the initial query level)

`| ? {$_.officephone -eq "876-5309"}`

- `|`: Pipes the results to the next command
- `?`: Alias for `Where-Object` (filters the results)
- `$_.officephone -eq "876-5309"`: Only keeps users whose office phone exactly matches "876-5309"

`.GivenName`

- Extracts just the first name (GivenName) property from the filtered results

Parentheses `( )`

- Ensures the entire pipeline completes before selecting GivenName

Below is the output of the command followed by the contents on the desktop listed using the command `ls` 

```powershell
PS C:\users\cyborg9\desktop> (Get-ADUser -Properties officephone -Filte
r * | ? {$_.officephone -eq "876-5309"}).GivenName
[REDACTED]
PS C:\users\cyborg9\desktop> ls

    Directory: C:\users\cyborg9\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:45 AM              0 99
```

## Cyborg level 10-11

For this level, we need to find the description for the applocker executable deny policy for `ill_be_back.exe` and then concatenate it with the name of the file on the desktop of the user cyborg10.

```powershell
The password for cyborg11 is the description of the Applocker Executable deny policy for ill_be_back.exe PLUS the name of the file on the desktop.

NOTE:
– If the description is “green$” and the file on the desktop is called “28”, then the password is “green$28”.
– The password will be lowercase no matter how it appears on the screen.
```

We shall use the following the command.

```powershell
$($(Get-AppLockerPolicy -Effective).RuleCollections.Description + $(ls).name)
```

Below is the breakdown of the commands used.

`$(Get-AppLockerPolicy -Effective).RuleCollections.Description`

- `Get-AppLockerPolicy -Effective`: Retrieves the currently applied AppLocker policies
- `.RuleCollections.Description`: Extracts the description fields from all rule collections
- Wrapped in `$()` to evaluate as a subexpression

`$(ls).name`

- `ls`: Alias for `Get-ChildItem` (lists files/folders in current directory)
- `.name`: Extracts just the names of the items
- Wrapped in `$()` to evaluate as a subexpression

The `+` operator

- Concatenates (combines) the two collections of strings

Outer `$()`

- Combines everything into a single output expression

Below is the output of the command.

```powershell
PS C:\users\cyborg10\desktop> $($(Get-AppLockerPolicy -Effective).RuleC
ollections.Description + $(ls).name)
[REDACTED]
PS C:\users\cyborg10\desktop>
```

## Cyborg level 11-12

For this challenge, the password is located in IIS log and we are told it is `Mozilla` or `Opera` 

```powershell
The password for cyborg12 is located in the IIS log. The password is not Mozilla or Opera.

NOTE:
– The password will be lowercase no matter how it appears on the screen.
```

We shall use the following command.

```powershell
Get-Content -Path ..\..\..\inetpub\logs\LogFiles\W3SVC1\u_ex160413.log | Select-String -NotMatch -Pattern "Mozilla|Opera"
```

Below is a breakdown of the command.

`Get-Content -Path ..\..\..\inetpub\logs\LogFiles\W3SVC1/u_ex160413.log`

- Reads the IIS log file from the specified path
- The `..\..\..` navigates up three directory levels from the current location
- `u_ex160413.log` is an IIS log file (April 13, 2016) in W3SVC1 (typically the Default Web Site)

`|` (Pipeline)

- Sends the log file contents to the next command

`Select-String -NotMatch -Pattern "Mozilla|Opera"`

- Filters the log entries to show only lines that DON'T contain:
    - "Mozilla" (used by Firefox, Chrome, and many other browsers)
    - "Opera" (Opera browser)
- `NotMatch` inverts the matching logic

Below is the output of the command.

```powershell
PS C:\users\cyborg11\desktop> Get-Content -Path ..\..\..\inetpub\logs\L
ogFiles\W3SVC1\u_ex160413.log | Select-String -NotMatch -Pattern "Mozil
la|Opera"

#Software: Microsoft Internet Information Services 8.5
#Version: 1.0
#Date: 2016-04-13 04:14:01
#Fields: date time s-sitename s-computername s-ip cs-method
cs-uri-stem cs-uri-query s-port cs-username c-ip cs-version
cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus
sc-win32-status sc-bytes cs-bytes time-taken
2016-04-13 04:14:12 W3SVC1 Century 172.31.45.65 GET / - 80 -
172.31.45.65 HTTP/1.1
LordHelmet/5.0+(CombTheDesert)+Password+is:[REDACTED] - -
century.underthewire.tech 200 0 0 925 118 0

PS C:\users\cyborg11\desktop>
```

## Cyborg level 12-13

The password for the next level is the first four characters of the base64 encoded full path of the file that started the i_heart_robots service, plus the name of the file on the desktop.

```powershell
The password for cyborg13 is the first four characters of the base64 
encoded full path to the file that started the i_heart_robots service PLUS the name of the file on the desktop.

NOTE:
– An example of a full path would be ‘c:\some_folder\test.exe’.
– Be sure to use ‘unicode’ in your encoding.
–
 If the encoded base64 is “rwmed2fdreewrt34t” and the file on the 
desktop is called “_address”, then the password is “rwme_address”.
– The password will be lowercase no matter how it appears on the screen.
```

We shall the following one-liner command.

```powershell
$((([Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($(Get-WmiObject win32_service | where {$_.Name -eq "i_heart_robots"}).PathName))[0..3]) -join "") + $(ls).name)
```

Below is a breakdown of the command.

`Get-WmiObject win32_service | where {$_.Name -eq "i_heart_robots"}`

- Queries WMI for a service named "i_heart_robots"
- Returns the service object if found

`[System.Text.Encoding]::UTF8.GetBytes($(...).PathName)`

- Takes the service's executable path
- Converts it to UTF-8 encoded bytes

`[Convert]::ToBase64String(...)` 

- Converts the byte array to a Base64 string

`(...)[0..3]` 

- Takes only the first 4 characters of the Base64 string

`(...) -join ""` 

- Ensures the characters are combined as a single string

`$(ls).name` 

- Gets names of all files in current directory (`ls` = `Get-ChildItem`)

Below is the output of the command.

```powershell

PS C:\users\cyborg12\desktop> $((([Convert]::ToBase64String([System.Tex
t.Encoding]::UTF8.GetBytes($(Get-WmiObject win32_service | where {$_.Na
me -eq "i_heart_robots"}).PathName))[0..3]) -join "") + $(ls).name)
[REDACTED]
```

## Cyborg level 13-14

Below is the objectives and description of the challenge.

```powershell
The password cyborg14 is the number of days the refresh interval is set to for DNS aging for the underthewire.tech zone PLUS the name of the file on the desktop.

NOTE:
– If the days are set to “08:00:00:00” and the file on the desktop is called “_tuesday”, then the password is “8_tuesday”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the command I used.

```powershell
$($($(Get-DnsServerZoneAging -Name underthewire.tech).RefreshInterval).ToString().Split(".")[0] + $(ls).name)
```

Here is the breakdown of the command.

`Get-DnsServerZoneAging -Name underthewire.tech` 

- Retrieves aging/scavenging properties for the DNS zone "underthewire.tech"
- Requires DNS Server module and administrative privileges

`).RefreshInterval` 

- Gets the refresh interval value (how often aging/scavenging occurs)
- Typically returns a TimeSpan object

`).ToString().Split(".")[0]` 

- Converts TimeSpan to string (format "days.hours:minutes:seconds")
- Splits at the decimal point and takes the first part (days)

`$(ls).name` 

- Gets names of all files in current directory (`ls` = `Get-ChildItem`)

`+ $(ls).name`

- Concatenates the day value with all filenames

 Below is the output of the command.

```powershell

PS C:\Users\cyborg13\desktop>$($($(Get-DnsServerZoneAging -Name underthewire.tech).RefreshInterval).ToString().Split(".")[0] + $(ls).name)
[REDACTED]
```

## Cyborg level 14-15

Below is the objective of the challenge.

```powershell
The password for cyborg15 is the caption for the DCOM application 
setting for application ID {59B8AFA0-229E-46D9-B980-DDA2C817EC7E} PLUS the name of the file on the desktop.

NOTE:
– If the caption is “dcom” and the file on the desktop is called “_address”, then the password is “dcom_address”.
– The password will be lowercase no matter how it appears on screen.
```

To get information about DCOM applicatios we can use again the `Get-WmiObject` cmdlet and filter for `win32_DCOMApplication` class, we shall use the following command.

```powershell
(Get-WmiObject -Class "Win32_DCOMApplication" -Filter "AppId='{59B8AFA0-229E-46D9-B980-DDA2C817EC7E}'" ).Caption
```

Here is the breakdown of the command.

`Get-WmiObject`

- The PowerShell cmdlet that retrieves WMI class instances

`Class "Win32_DCOMApplication"`

- Specifies the WMI class that represents DCOM applications
- Contains information about all registered DCOM applications on the system

`Filter "AppId='{59B8AFA0-229E-46D9-B980-DDA2C817EC7E}'"`

- Filters to find only the DCOM application with this specific AppID (GUID)
- The GUID `{59B8AFA0-229E-46D9-B980-DDA2C817EC7E}` identifies a particular DCOM application

`.Caption`

- Retrieves the "Caption" property of the found DCOM application
- This typically contains the human-readable name of the application

The output of the command is as follows and the file on the desktop directory.

```powershell
PS C:\users\cyborg14\desktop> (Get-WmiObject -Class "Win32_DCOMApplicat
ion" -Filter "AppId='{59B8AFA0-229E-46D9-B980-DDA2C817EC7E}'" ).Caption

[REDACTED]
PS C:\users\cyborg14\desktop> ls

    Directory: C:\users\cyborg14\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:45 AM              0 _objects

PS C:\users\cyborg14\desktop>
```

## Cyborg level 15

We are done with the entire cyborg challenge series.

```powershell
Congratulations!

You have successfully made it to the end!

Try your luck with other games brought to you by the Under The Wire team.

Thanks for playing!

Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\cyborg15\desktop>

:) :) :o

```