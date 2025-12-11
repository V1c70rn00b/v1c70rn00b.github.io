---
title: "Underthewire trebek war games"
date: 2025-06-11
description: This is a writeup on trebek from underthewire wargames.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "underthewire wargames trebek series" # Here you can write a small summary of the post if needed
tags: [powershell, SSH, event logs]
categories: [Underthewire, AD]
---

## Trebek level 0-1

The goal for this level is to log in to the first level via ssh using the creds provided to us in the Slack channel.

```powershell
The goal of this level is to log into the game. Do the following in order to achieve this goal.

1. Obtain the initial credentials via the #StartHere channel on our Slack (Link). Once you are in the channel, scroll to the top to see the credentials.

2.
 After obtaining the credentials, connect to the server via SSH. You 
will need an SSH client such as Putty. The host that you will be 
connecting to is trebek.underthewire.tech, on port 22.

3. When prompted, use the credentials for the applicable game found in the #StartHere Slack channel.

4. You have successfully connected to the game server when your path changes to “PS C:\Users\Trebek1\desktop>”.
```

![image.png](image.png)

With the credentials above, we are able to now log in as shown below.

```powershell
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\trebek1\desktop> 
```

## Trebek level 1-2

For this challenge, we need to find the name of the deleted script referenced in a deleted task.

```powershell
The password for trebek2 is the name of the script referenced in a deleted task as depicted in the event logs on the desktop. 
NOTE: 
– Don’t include the file extension (i.e.- .vbs)
– The password will be lowercase no matter how it appears on the screen.
```

For this task, we need to find the event ID that is generated when deleting a scheduled task, from this [link](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4699?ref=benheater.com), we find our event ID to be `4699.` 

To achieve this, we shall first start by doing the following.

```powershell
$evt = Get-WinEvent -Path .\security.evtx -FilterXPath "*[System[EventID=4699]]"
```

 `$evt =`

- Assigns the output of the command to a PowerShell variable named `$evt`.
- You can use `$evt` later to inspect or manipulate the filtered event data.

 `Get-WinEvent -Path .\security.evtx`

- Reads the `security.evtx` file from the current directory.
- This file contains exported Windows Security logs.

 `FilterXPath "*[System[EventID=4699]]"`

- Filters the logs using XPath, a powerful query language for selecting nodes in XML documents (Windows event logs are XML under the hood).
- the filter means look for any event (`*`) whose `<System>` block contains `<EventID>4699`.

After assigning the output of the command to the `$evt` variable, we then display the message content of the event(s) stored in the variable as shown below.

```powershell
$evt.Message
```

We shall then the most recent event from the variable `$evt` list, extract its `Message` text and slits that message into an array of lines, using the newline character as the separator as shown below.

```powershell
$evt[-1].Message.Split("`n")
```

Below is the output of the commands above.

```powershell
PS C:\users\trebek1\desktop> $evt = Get-WinEvent -Path .\security.evtx
-FilterXPath "*[System[EventID=4699]]"
PS C:\users\trebek1\desktop> $evt.Message
A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x338C9

Task Information:
        Task Name:              \Bitvise\Persistent BvSshServer Control
 Panel\S-1-5-21-3968311752-1263969649-2303472966-500
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
      <LogonType>InteractiveToken</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>Parallel</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>false</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>6</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Bitvise SSH Server\BssCtrl.exe</Command
>
      <Arguments>-startMinimized</Arguments>
    </Exec>
  </Actions>
</Task>

A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x74703

Task Information:
        Task Name:              \Bitvise\Persistent BvSshServer Control
 Panel\S-1-5-21-3968311752-1263969649-2303472966-500
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
      <LogonType>InteractiveToken</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>Parallel</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>false</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>6</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Bitvise SSH Server\BssCtrl.exe</Command
>
      <Arguments>-startMinimized</Arguments>
    </Exec>
  </Actions>
</Task>

A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x13069DD

Task Information:
        Task Name:              \Bitvise\Persistent BvSshServer Control
 Panel\S-1-5-21-3968311752-1263969649-2303472966-500
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
      <LogonType>InteractiveToken</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>Parallel</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>false</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>6</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Bitvise SSH Server\BssCtrl.exe</Command
>
      <Arguments>-startMinimized</Arguments>
    </Exec>
  </Actions>
</Task>

A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x12B5275

Task Information:
        Task Name:              \Bitvise\Persistent BvSshServer Control
 Panel\S-1-5-21-3968311752-1263969649-2303472966-500
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
      <LogonType>InteractiveToken</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>Parallel</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>false</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>6</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Bitvise SSH Server\BssCtrl.exe</Command
>
      <Arguments>-startMinimized</Arguments>
    </Exec>
  </Actions>
</Task>

A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x128CC45

Task Information:
        Task Name:              \Bitvise\Persistent BvSshServer Control
 Panel\S-1-5-21-3968311752-1263969649-2303472966-500
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
      <LogonType>InteractiveToken</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>Parallel</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>false</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>6</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Bitvise SSH Server\BssCtrl.exe</Command
>
      <Arguments>-startMinimized</Arguments>
    </Exec>
  </Actions>
</Task>

A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-500
        Account Name:           Administrator
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x12053FB

Task Information:
        Task Name:              \Bitvise\Persistent BvSshServer Control
 Panel\S-1-5-21-3968311752-1263969649-2303472966-500
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <LogonTrigger>
      <Enabled>true</Enabled>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
    </LogonTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>UNDERTHEWIRE\Administrator</UserId>
      <LogonType>InteractiveToken</LogonType>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>Parallel</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>false</StopIfGoingOnBatteries>
    <AllowHardTerminate>false</AllowHardTerminate>
    <StartWhenAvailable>true</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>false</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT0S</ExecutionTimeLimit>
    <Priority>6</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Program Files\Bitvise SSH Server\BssCtrl.exe</Command
>
      <Arguments>-startMinimized</Arguments>
    </Exec>
  </Actions>
</Task>

A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-1135
        Account Name:           trebek1
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x644A01

Task Information:
        Task Name:              \cleanup mess
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.3" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2017-05-10T01:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <RandomDelay>P0DT0H0M0S</RandomDelay>
      <ScheduleByDay>
        <DaysInterval>1</DaysInterval>
      </ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>true</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>false</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <DisallowStartOnRemoteAppSession>false</DisallowStartOnRemoteAppSes
sion>
    <UseUnifiedSchedulingEngine>true</UseUnifiedSchedulingEngine>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT72H</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Windows\System32\WindowsPowerShell\v1.0\powershell.ex
e</Command>
      <Arguments>-NonInteractive -NoLogo -NoProfile -File 'c:\users\tre
bek1\mess_cleaner.ps1'</Arguments>
    </Exec>
  </Actions>
  <Principals>
    <Principal id="Author">
      <UserId>UNDERTHEWIRE\trebek1</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>LeastPrivilege</RunLevel>
    </Principal>
  </Principals>
</Task>

PS C:\users\trebek1\desktop> $evt[-1].Message.Split("`n")
A scheduled task was deleted.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-1135
        Account Name:           trebek1
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x644A01

Task Information:
        Task Name:              \cleanup mess
        Task Content:           <?xml version="1.0" encoding="UTF-16"?>

<Task version="1.3" xmlns="http://schemas.microsoft.com/windows/2004/02
/mit/task">
  <RegistrationInfo />
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2017-05-10T01:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <RandomDelay>P0DT0H0M0S</RandomDelay>
      <ScheduleByDay>
        <DaysInterval>1</DaysInterval>
      </ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>true</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>false</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <Duration>PT10M</Duration>
      <WaitTimeout>PT1H</WaitTimeout>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <DisallowStartOnRemoteAppSession>false</DisallowStartOnRemoteAppSes
sion>
    <UseUnifiedSchedulingEngine>true</UseUnifiedSchedulingEngine>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT72H</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>C:\Windows\System32\WindowsPowerShell\v1.0\powershell.ex
e</Command>
      <Arguments>-NonInteractive -NoLogo -NoProfile -File 'c:\users\tre
bek1\mess_cleaner.ps1'</Arguments>
    </Exec>
  </Actions>
  <Principals>
    <Principal id="Author">
      <UserId>UNDERTHEWIRE\trebek1</UserId>
      <LogonType>InteractiveToken</LogonType>
      <RunLevel>LeastPrivilege</RunLevel>
    </Principal>
  </Principals>
</Task>
```

As seen above, the content is a lot to go through at once. We could get the last event in the variable array, splits its message into an array of lines, then get the 12th line from the end of that message as shown below. As I had gone through the long, boring output to get the file.

```powershell
$evt[-1].Message.Split("`n")[-12]
```

Here is the output of the command above.

```powershell
PS C:\users\trebek1\desktop> $evt[-1].Message.Split("`n")[-12]
      <Arguments>-NonInteractive -NoLogo -NoProfile -File 'c:\users\tre
bek1\mess_cleaner.ps1'</Arguments>
PS C:\users\trebek1\desktop>
```

We have our script and password. 

## Trebek level 2-3

Below is the description of the challenge.

```powershell
The password for trebek3 is the name of the executable associated with the C-3PO service PLUS the name of the file on the user’s desktop.

NOTE:
–
 Don’t include the file extension (i.e.- .exe). If the executable name 
is “binary.exe” and the file on the desktop is named “1234”, the 
password would be “binary1234”.
– The password will be lowercase no matter how it appears on the screen.
```

As seen, we need to find the name of the executable associated with the `C-3PO` service then combine it with the name of the file on the desktop.

Below is the file on the desktop directory.

```powershell
PS C:\users\trebek2\desktop> ls

    Directory: C:\users\trebek2\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:45 AM              0 823

PS C:\users\trebek2\desktop>

```

To find the name of the executable, we shall use the following command.

```powershell
Get-CimInstance Win32_Service -Filter 'Name like "C-3PO"' | Select-Object *
```

 `Get-CimInstance`

- Retrieves information from the Common Information Model (CIM).
- It's a modern alternative to `Get-WmiObject`.
- Used to query system information like services, processes, network configs, etc.

 `Win32_Service`

- This is the CIM class representing Windows services.
- Each object returned represents one Windows service, with properties like:
    - `Name`
    - `DisplayName`
    - `StartMode`
    - `State`
    - `Status`
    - `ProcessId`
    - and many more...
    
     `Filter 'Name like "C-3PO"'`
    
    - Applies a filter: it looks for services with a Name exactly equal to `"C-3PO"`.
        - Technically `LIKE` allows for wildcards (`%`), but since none are used here, it behaves like an exact match.
        - Example: `'Name like "C-3PO"'` only matches a service named `C-3PO`.
    
     `| Select-Object *`
    
    - Pipes the result into `Select-Object *` which means:
        - Show all properties of the service object (not just the default summary).
        - Gives you full details about the service.

Here is the output of the command

```powershell
PS C:\users\trebek2\desktop> Get-CimInstance Win32_Service -Filter 'Nam
e like "C-3PO"' | Select-Object *

Name                    : C-3PO
Status                  : OK
ExitCode                : 0
DesktopInteract         : False
ErrorControl            : Normal
PathName                : [REDACTED]
ServiceType             : Own Process
StartMode               : Auto
Caption                 : C-3PO
Description             :
InstallDate             :
CreationClassName       : Win32_Service
Started                 : False
SystemCreationClassName : Win32_ComputerSystem
SystemName              : UTW
AcceptPause             : False
AcceptStop              : False
DisplayName             : C-3PO
ServiceSpecificExitCode : 0
StartName               : LocalSystem
State                   : Stopped
TagId                   : 0
CheckPoint              : 0
DelayedAutoStart        : False
ProcessId               : 0
WaitHint                : 0
PSComputerName          :
CimClass                : root/cimv2:Win32_Service
CimInstanceProperties   : {Caption, Description, InstallDate, Name...}
CimSystemProperties     : Microsoft.Management.Infrastructure.CimSyste
                          mProperties

```

From the above output, the name of the executable will be at the `PathName` property.

We now have our password.

## Trebek level 3-4

To get the password for the next level, we are required to find the IP of the user `Yoda` which was last logged in from, as depicted  in the event logs.

```powershell
The password for trebek4 is the IP that the user Yoda last logged in from as depicted in the event logs on the desktop PLUS the name of the text file on the user’s desktop. 
NOTE: 
– If the IP address is “2.3.3.2” and the file on the desktop is named “bobby”, the password would be “2.3.3.2bobby”.
```

Below is the file on the desktop.

```powershell
PS C:\users\trebek3\desktop> ls

    Directory: C:\users\trebek3\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:46 AM              0 address
-a----        8/30/2018   5:55 AM       99684352 security.evtx

PS C:\users\trebek3\desktop>
```

To find the IP from the event logs, we need to look for the event ID [4624](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4624&ref=benheater.com) which indicates an account has successfully logged on. We shall use the command below.

```powershell
 ( Get-WinEvent -Path .\security.evtx -Filt
erXPath "*[System[(EventID=4624)]] and *[EventData[Data[@Name='TargetUs
erName'] and (Data='Yoda')]]").Message.Split("`n")
```

 `Get-WinEvent -Path .\security.evtx`

- Reads an exported Windows Security log file (`security.evtx`) from the current folder.

 `FilterXPath "*[System[(EventID=4624)]] and *[EventData[Data[@Name='TargetUserName'] and (Data='Yoda')]]"`

- This is an XPath filter for the event log.
- `EventID=4624`: Look for successful logon events.
- `TargetUserName='Yoda'`: Only select logon events where the account being logged into is named "Yoda".

 `( ... ).Message`

- Extracts the `Message` property from the event.
- This is the formatted description of the event, like:

Here is a deprecated output of the command.

```powershell
PS C:\users\trebek3\desktop> ( Get-WinEvent -Path .\security.evtx -Filt
erXPath "*[System[(EventID=4624)]] and *[EventData[Data[@Name='TargetUs
erName'] and (Data='Yoda')]]").Message.Split("`n")
An account was successfully logged on.

Subject:
        Security ID:            S-1-5-18
        Account Name:           TREBEK$
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x3E7

Logon Type:                     10

Impersonation Level:            Impersonation

New Logon:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-1155
        Account Name:           yoda
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0xEEB1C
        Logon GUID:             {00000000-0000-0000-0000-000000000000}

Process Information:
        Process ID:             0xf10
        Process Name:           C:\Windows\System32\winlogon.exe

Network Information:
        Workstation Name:       TREBEK
        Source Network Address: [REDACTED]
        Source Port:            0

Detailed Authentication Information:
        Logon Process:          User32
        Authentication Package: Negotiate
        Transited Services:     -
        Package Name (NTLM only):       -
        Key Length:             0

This event is generated when a logon session is crea
```

We have a our passowrd for level 4

## Trebek level 4-5

The password for the next level is the last execution date of Microsoft access plus the name of the text file on the user’s desktop.

```powershell
The password for trebek5 is the last execution date of Microsoft Access PLUS the name of the text file on the user’s desktop.

Note: 
– Format for the date is 2 digit month, 2 digit day, 4 digit year. Ex: 9 feb 2009 would be 02/09/2009.
– If the date is “02/09/2009” and the file on the desktop is named “_bob”, the password would be “02/09/2009_bob”.
```

Below is the name of the file ion the user’s desktop.

```powershell
PS C:\users\trebek4\desktop> ls

    Directory: C:\users\trebek4\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:46 AM              0 _red

PS C:\users\trebek4\desktop>
```

To get the last execution date for Microsoft access, we shall use the following command.

```powershell
get-childitem "C:\Windows\prefetch" -filter "*access*"
```

 `Get-ChildItem`

- Alias: `gci` or `dir`
- Lists files and folders (i.e. "children") in a specified directory.

 `"C:\Windows\Prefetch"`

- This is the Windows Prefetch folder, used by the operating system to speed up the loading of frequently accessed programs.
- Each file here typically represents a record of an application that has been run.

 `Filter "*access*"`

- Filters results to only include files that have `"access"` in their name.
    - This is case-insensitive.
    - Matches files like:
        - `ACCESS.EXE-XXXXX.pf`
        - `MSACCESS.EXE-YYYYY.pf`

Here is the output of the command.

```powershell
PS C:\users\trebek4\desktop> get-childitem "C:\Windows\prefetch" -filte
r "*access*"

    Directory: C:\Windows\prefetch

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         [REDACTED]   6:04 PM          65058 MSACCESS.EXE-EF45328A
                                                 .pf

PS C:\users\trebek4\desktop>
```

With that, we have our password for the next level.

## Trebek level 5-6

Below is the description and objective of the challenge.

```powershell
The password for trebek6 is the name of the executable that is starting at 
3/23/2017 8:08:53 PM via the Software Protection service as depicted in 
the event log on the desktop. 
NOTE: 
– Don’t include the file extension (i.e.- .exe). 
– The password will be lowercase no matter how it appears on the screen.
```

We need to look through the event log file to find the name of the executable that is starting at the stated dates and times.

For this challenge, the event IDs we shall be targeting are [900](https://learn.microsoft.com/en-us/troubleshoot/windows-server/licensing-and-activation/event-id-8208-8200-900?ref=benheater.com), [16384 and 16394](https://answers.microsoft.com/en-us/windows/forum/all/event-id-16394-and-16384-getting-spammed/b730c7aa-5f17-4455-a93b-2ae43c85608c?ref=benheater.com)

The command we shall be using is as follows.

```powershell
(Get-WinEvent -Path .\application.evtx -FilterXPath "*[System[(EventID=900)]] or *[System[(EventID=16384)]] or *[System[(EventID=16394)]]").Where({$_.TimeCreated -eq (Get-Date '3/23/2017 8:08:53 PM')}).Message.Split("`n")[-1].Split('=')[1].Split('\.')[0]
```

 `Get-WinEvent -Path .\application.evtx`

- Loads an exported Windows event log file from the current directory.

 `FilterXPath "*[System[(EventID=900)]] or *[System[(EventID=16384)]] or *[System[(EventID=16394)]]"`

- Uses XPath to filter for three specific Event IDs:
    - `900` – Often related to application lifecycle or info messages.
    - `16384`, `16394` – These may relate to software installations or service initializations, depending on the source.
    
     `.Where({$_.TimeCreated -eq (Get-Date '3/23/2017 8:08:53 PM')})`
    
    - Filters the result to only the event that was created at that exact date and time.
    
     `.Message`
    
    - Extracts the full, human-readable text of the event.
    
     `.Split("`n")[-1]`
    
    - Splits the message into lines by newline (`\n`), then picks the last line.
    
     `.Split('=')[1]`
    
    - Splits that line at the `=` character and grabs the second part (index `[1]`), which is likely a value.
    
     `.Split('\.')[0]`
    
    - Splits that value on the dot character (`.`) and takes the first part.
    - This is commonly done to extract a NetBIOS name, hostname, or prefix from a fully qualified domain name.

Here is the command output.

```powershell
PS C:\users\trebek5\desktop> (Get-WinEvent -Path .\application.evtx -FilterXPath "*[System[(EventID=900)]] or *[System[(EventID=16384)]] or *[System[(EventID=16394)]]").Where({$_.TimeCreated -eq (Get-Date '3/23/2017 8:08:53 PM')}).Message.Split("`n")[-1].Split('=')[1].Split('\.')[0]
[REDACTED]

```

## Trebek level 6-7

To find the password for level 7, we need to get the total number of dlls within the directory `C:\program files\adobe\` including those in the subfolders found in the directory then join it with the name of the file on the user’s desktop.

```powershell
The password for trebek7 is the total number of DLLs within the “C:\program files\adobe\” folder and it’s subfolders PLUS the name of the file on the desktop. 
NOTE: 
– If the count is “9999” and the file on the desktop is named “_abc”, then the password would be “9999_abc”. 
– The password will be lowercase no matter how it appears on the screen.
```

The file on the desktop is as shown below.

```powershell
PS C:\Users\trebek6\desktop> ls

    Directory: C:\Users\trebek6\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:47 AM              0 _reader

PS C:\Users\trebek6\desktop>
```

The command we shall use to find the number of DLL(dynamic link library) in the said directory is as shown below.

```powershell
(Get-ChildItem -Recurse -Filter '*.dll' "C
:\program files\adobe\").Count
```

 `Get-ChildItem`

- Lists files and folders in a specified directory.
- Alias: `gci` or `dir`.

 `Recurse`

- Tells PowerShell to search all subdirectories as well — not just the top-level folder.

 `Filter '*.dll'`

- Limits the search to files with the `.dll` extension (Dynamic Link Libraries).
- These files typically contain code libraries used by software applications (like Adobe apps).

 `"C:\Program Files\Adobe\"`

- This is the target directory to search in — where Adobe software is installed.

 Wrapping the command in parentheses `( ... ).Count`

- The `(...)` ensures the entire result of `Get-ChildItem` is treated as one object.
- `.Count` returns the number of `.dll` files found.

Here is the output of the command.

```powershell
PS C:\Users\trebek6\desktop> (Get-ChildItem -Recurse -Filter '*.dll' "C
:\program files\adobe\").Count
[REDACTED]
PS C:\Users\trebek6\desktop>
```

## Trebek level 7-8

Below is the challenge objective, to get the name of the program set to run prior to login if sticky keys are activated, then join it with the name of the file on the user’s desktop.

```powershell
The password for trebek8 is the name of the program set to run prior to login if sticky keys are activated PLUS the name of the file on the desktop. 
NOTE: 
– Don’t include the file extension (i.e.- 
.vbs). If the name is “script.vbs” and the file on the desktop is named 
“1234”, then the password would be “script1234”. 
– The password will be lowercase no matter how it appears on the screen.
```

The file on the desktop is as shown below.

```powershell
PS C:\users\trebek7\desktop> ls

    Directory: C:\users\trebek7\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:47 AM              0 99

PS C:\users\trebek7\desktop>
```

To check if sticky keys is enabled, a flag of 510 is usually set as shown below.

```powershell
PS C:\users\trebek7\desktop> Get-Item 'HKCU:\Control Panel\Accessibilit
y\StickyKeys'

    Hive: HKEY_CURRENT_USER\Control Panel\Accessibility

Name                           Property
----                           --------
StickyKeys                     Flags : 510

PS C:\users\trebek7\desktop>
```

When sticky keys are enabled, the `sethc.exe` binary is activated. To get a brief understanding of how analyst and malware authors use image file execution options, read this [article](https://blog.malwarebytes.com/101/2015/12/an-introduction-to-image-file-execution-options).

We shall use the command below to get the name of the name of the program set.

```powershell
Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe'
```

 `HKLM:\...sethc.exe`

- This is a registry path under HKEY_LOCAL_MACHINE.
- It refers to the execution settings for the `sethc.exe` binary.
- `sethc.exe` is the Sticky Keys program that can be launched on the login screen by pressing `Shift` 5 times.

 `| Get-ItemProperty`

- Retrieves the properties (values) set under that registry key — especially looking for a value named `Debugger`.

Below is the output of the command.

```powershell
PS C:\users\trebek7\desktop> Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft
\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe'

Debugger     : han_solo.exe
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\
               Microsoft\Windows NT\CurrentVersion\Image File Execution
               Options\sethc.exe
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\
               Microsoft\Windows NT\CurrentVersion\Image File Execution Options
PSChildName  : sethc.exe
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
```

## Trebek level 8-9

The passowrd for level9 is the first 8 bytes of the file located on the user’s desktop.

```powershell
The password for trebek9 the first 8 bytes of the file located on the desktop. Combine the answer together with NO spaces.
```

For this scenario, we shall use the command below.

```powershell
 (Get-Content -Path ..\Desktop\Clone_Trooper_data.pdf -Encoding Byte)[0..7] -join ""
```

 `Get-Content -Path ..\Desktop\Clone_Trooper_data.pdf -Encoding Byte`

- Reads the binary content of the file, byte by byte.
- `Encoding Byte` ensures you're reading the file in raw binary mode, not as text.

 `[0..7]`

- This extracts the first 8 bytes of the file.

 `join ""`

- This joins the extracted bytes into a single string, with no separators.

Here is the output of the command.

```powershell
PS C:\users\trebek8\desktop> (Get-Content -Path ..\Desktop\Clone_Trooper_data.pdf -Encoding Byte)[0..7] -join ""
[REDACTED]
PS C:\users\trebek8\desktop>
```

## Trebek level 9-10

For this level, we are required to find the name of the potentially rogue share on the system then combine it with the name of the file on the desktop.

```powershell
The password for trebek10 is the name of the potentially rogue share on the system PLUS the name of the file on the desktop. 
Note: 
– If the share name is “share$” and the file on the desktop is named “_today”, the password would be “share$_today”. 
– Exclude any default shares typically found on a Domain Controller. 
– The password will be lowercase no matter how it appears on the screen.
```

The file on the user’s desktop is.

```powershell
PS C:\users\trebek9\desktop> ls

    Directory: C:\users\trebek9\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:48 AM              0 _hiding

PS C:\users\trebek9\desktop>
```

To find the rogue share, we shall use the following command.

```powershell
get-smbshare
```

This command lists all server message block shares with their descriptions.

Below is the output of the command.

```powershell
PS C:\users\trebek9\desktop> get-smbshare

Name           ScopeName Path Description
----           --------- ---- -----------
ADMIN$         *              Remote Admin
C$             *              Default share
IPC$           *              Remote IPC
NETLOGON       *              Logon server share
[REDACTED]     *              Nothing to see here
SYSVOL         *              Logon server share
Tasker         *              scheduled_things

PS C:\users\trebek9\desktop>
```

From the above share we have our rogue share

## Trebek level 10-11

The objective for ths level is as stated below.

```powershell
The password for trebek11 is the last name of the user who enabled Obi-Wan 
Kenobi’s account as depicted in the event logs on the desktop PLUS the name of the file on the desktop. 
NOTE: 
– If the last name of the user is “johnson” and the file on the desktop is named “1234”, the password would be “johnson1234”. 
– The password will be lowercase no matter how it appears on the screen.
```

File on the user’s desktop is.

```powershell
PS C:\users\trebek10\desktop> ls

    Directory: C:\users\trebek10\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:48 AM              0 2121
-a----        8/30/2018   5:54 AM       99684352 security.evtx

PS C:\users\trebek10\desktop>
```

To find the last name of the user, we need to search for event ID [4722](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4722?ref=benheater.com) which indicates user account  enablement.

We shall use the command below to get the user and pick the last name alone.

```powershell
(Get-WinEvent -Path .\security.evtx -FilterXPath "*[System[EventID=4722]]" | Where-Object { $_.Message -like '*obi*' }).Message.Split("`n")[4]

```

- `Get-WinEvent ...` — Reads `security.evtx` and filters for Event ID 4722 (user account enabled).
- `Where-Object { $_.Message -like '*obi*' }` — Filters only those events that mention `"obi"` in the message.
- `.Message.Split("`n")[4]` — Takes the matching event’s message, splits it into lines, and shows line 5, which usually contains useful info like who enabled the account.

Here is the command output.

```powershell
PS C:\users\trebek10\desktop> (Get-WinEvent -Path .\security.evtx -FilterXPath "*[System[EventID=4722]]" | Where-Object { $_.Message -like '*obi*' }).Message.Split("`n")[4]
        Account Name:           admiral.[REDACTED]
PS C:\users\trebek10\desktop>
```

## Trebek level 11-12

To find the password for the next level, we need to find the user who was created at the stated time, then combine with the name of the file on the user’s desktop.

```powershell
The password for trebek12 is the username of the user who was created on 11 May 17 at 26 minutes after the hour, as depicted in the event logs on 
the desktop PLUS the name of the file on the desktop. 
NOTE:
– If the username is “john.doe” and the file on the desktop is named “1234”, the password would be “john.doe1234” 
– The password will be lowercase no matter how it appears on the screen.
```

Below is the file on the user’s desktop.

```powershell
PS C:\users\trebek11\desktop> ls

    Directory: C:\users\trebek11\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:49 AM              0 100
-a----        8/30/2018   5:55 AM       99684352 security.evtx

PS C:\users\trebek11\desktop>
```

To find the user, we need to keep in mind of the event ID for user account creation which is [4720](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4720?ref=benheater.com).

From the challenge description, we need to find the user who was created at the 26th minute of the 11th day, from the description, we shall use the command below.

```powershell
 (Get-WinEvent -Path .\security.evtx -FilterXPath "*[System[EventID=4720]]").Where({$_.TimeCreated.Day -eq 11 -and $_.TimeCreated.Minute -eq 26}).Message

```

 `(Get-WinEvent -Path .\security.evtx -FilterXPath "*[System[EventID=4720]]")`

- Loads the `.evtx` event log file from the current directory.
- Filters it to only return Event ID 4720 events.
    - Event ID 4720 = A user account was created (part of Windows Security logs).

 `.Where({$_.TimeCreated.Day -eq 11 -and $_.TimeCreated.Minute -eq 26})`

- Filters the events further by their timestamp:
    - `Day -eq 11` means the event happened on the 11th day of the month.
    - `Minute -eq 26` means the event occurred at XX:26 (e.g., 14:26, 07:26).

 `.Message`

- Extracts and displays the Message property from each matching event.
- This message contains details such as:
    - Who created the account,
    - What account was created,
    - Security IDs (SIDs),
    - Domain and privileges info.

Here is the command output.

```powershell
PS C:\users\trebek11\desktop> (Get-WinEvent -Path .\security.evtx -FilterXPath "*[System[EventID=4720]]").Where({$_.TimeCreated.Day -eq 11 -and $_.TimeCreated.Minute -eq 26}).Message
A user account was created.

Subject:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-1150
        Account Name:           poe.dameron
        Account Domain:         UNDERTHEWIRE
        Logon ID:               0x1235812

New Account:
        Security ID:            S-1-5-21-3968311752-1263969649-23034729
66-1152
        Account Name:           [REDACTED]
        Account Domain:         UNDERTHEWIRE

Attributes:
        SAM Account Name:       general.hux
        Display Name:           Hux, General
        User Principal Name:    general.hux@underthewire.tech
        Home Directory:         -
        Home Drive:             -
        Script Path:            -
        Profile Path:           -
        User Workstations:      -
        Password Last Set:      <never>
        Account Expires:                <never>
        Primary Group ID:       513
        Allowed To Delegate To: -
        Old UAC Value:          0x0
        New UAC Value:          0x15
        User Account Control:
                Account Disabled
                'Password Not Required' - Enabled
                'Normal Account' - Enabled
        User Parameters:        -
        SID History:            -
        Logon Hours:            <value not set>

Additional Information:
        Privileges              -
PS C:\users\trebek11\desktop>
```

## Trebek level 12-13

This level is almost similar to level 12, but the difference is that, for this level, we are searching for the user who created the user `Lor San Tekka` then join it with the name of the file on the desktop.

```powershell
The password for trebek13 is the username of the user who created the user 
Lor San Tekka as depicted in the event logs on the desktop PLUS the name of the file on the desktop. 
NOTE: 
– If the username is “john.doe” and the file on the desktop is named “1234”, the password would be “john.doe1234” 
– The password will be lowercase no matter how it appears on the screen.
```

File on the user’s desktop is as shown below.

```powershell
PS C:\users\trebek12\desktop> ls

    Directory: C:\users\trebek12\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:49 AM              0 53
-a----        8/30/2018   5:55 AM       99684352 security.evtx

PS C:\users\trebek12\desktop>
```

Just like the previous challenge, we will use the event ID 4720. Below is the command we shall use.

```powershell
Get-WinEvent -Path .\Security.evtx | where {$_.Id -Eq 4720} | Format-List -Property Message | Out-String -Stream | Select-String "tekka" -Context 8,1

```

Here is the output of the command.

```powershell
PS C:\users\trebek12\desktop> Get-WinEvent -Path .\Security.evtx | where {$_.Id -Eq 4720} | Format-List -Property Message | Out-String -Stream | Select-String "tekka" -Context 8,1

            S-1-5-21-3968311752-1263969649-2303472966-1150
                Account Name:           [REDACTED]
                Account Domain:         UNDERTHEWIRE
                Logon ID:               0x1235812

            New Account:
                Security ID:
            S-1-5-21-3968311752-1263969649-2303472966-1151
               Account Name:           lor.tekka
               Account Domain:         UNDERTHEWIRE

            Attributes:
               SAM Account Name:       lor.tekka
               Display Name:           Tekka, Lor San
               User Principal Name:    lor.tekka@underthewire.tech
               Home Directory:         -

```

With that, we have the password for our next level.

## Trebek level 13-14

For this level, we are required to find the last name of the user who has an encoded PowerShell command in their City property then combine it with the name of the file on the desktop.

```powershell
The password for trebek14 is the last name of the user who has an encoded PowerShell command in their City property PLUS the name of the file on the desktop. 
NOTE: 
– If the last name is “doe” and the file on the desktop is named “1234”, the password would be “doe1234”. 
– The password will be lowercase no matter how it appears on the screen.
```

You should note of [LDAP city attribute](http://www.computerperformance.co.uk/Logon/LDAP_attributes_active_directory.htm) which is represented in the field named l (lowercase ‘L’). We shall use the command below.

```powershell
Get-ADUser -Filter 'l -like "*"'
```

Here is the command output.

```powershell
PS C:\users\trebek13\desktop> Get-ADUser -Filter 'l -like "*"'

DistinguishedName : CN=Prindel,OU=T-65,OU=X-Wing,DC=underthewire,DC=te
                    ch
Enabled           : False
GivenName         : Prindel
Name              : Prindel
ObjectClass       : user
ObjectGUID        : 8edb88c2-c69b-4d78-957e-6c1d1b08f2a5
SamAccountName    : Prindel
SID               : S-1-5-21-758131494-606461608-3556270690-2178
Surname           : Prindel
UserPrincipalName : Prindel
```

With that, we have the password.

## Trebek level 14-15

To find the password for the next level, we are required to decode the PowerShell code found in the account properties of the user account from the previous level, then combine with the name of the file on the desktop.

```powershell
The password for trebek15 is the output from decoding the PowerShell code 
found in the account properties of the user account from the previous 
level PLUS the name of the file on the desktop. 

NOTE: 
– If the output is “blue_and_red” and the 
file on the desktop is named “_are_great”, the password would be 
“blue_and_red_are_great”.
– The password will be lowercase no matter how it appears on the screen.
```

Below is the file on the desktop.

```powershell
PS C:\users\trebek14\desktop> ls

    Directory: C:\users\trebek14\desktop

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/30/2018  10:49 AM              0 _today

PS C:\users\trebek14\desktop>
```

To find the encoded string, we shall use the command below.

```powershell
 Get-ADUser -Filter 'l -like "*"' -Properties l
```

 `Get-ADUser`

- Retrieves user accounts from Active Directory (AD).

 `Filter 'l -like "*"'`

- This filters users to include only those who have a non-empty value for the `l` attribute.
- `l` stands for "locality" — it's typically used to store the city or location of the user.
- `like "*"` matches any value, meaning this returns users where locality is set.

Here is the output from the command

```powershell
PS C:\users\trebek1\desktop> Get-ADUser -Filter 'l -like "*"' -Properties l

DistinguishedName : CN=Prindel,OU=T-65,OU=X-Wing,DC=underthewire,DC=te
                    ch
Enabled           : False
GivenName         : Prindel
l                 : [REDACTED]
ObjectClass       : user
ObjectGUID        : 8edb88c2-c69b-4d78-957e-6c1d1b08f2a5
SamAccountName    : Prindel
SID               : S-1-5-21-758131494-606461608-3556270690-2178
Surname           : Prindel
UserPrincipalName : Prindel

PS C:\users\trebek14\desktop> 
```

the hash found is base64 encoded, we can decode it as follows.

```powershell
[System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String("agBvAGkAbgBfAHQAaABlAF8AcgBlAGIAZQBsAHMA"))
```

 `[System.Convert]::FromBase64String(...)`

- This decodes a Base64-encoded string into raw bytes.
- The input Base64 string is

 `[System.Text.Encoding]::Unicode.GetString(...)`

- Interprets the raw bytes as Unicode (UTF-16LE) and converts them to a readable string.

Here is the output from the command.

```powershell
PS C:\users\trebek14\desktop> [System.Text.Encoding]::Unicode.GetString
([System.Convert]::FromBase64String([REDACTED]))
[REDACTED]
PS C:\users\trebek14\desktop>
```

## Trebek level 15

We are done :) :) :) :)

```powershell
Congratulations! 
You have successfully made it to the end! 
Try your luck with other games brought to you by the Under The Wire team. 
Thanks for playing!

Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

Under the Wire... PowerShell Training for the People!
PS C:\users\trebek15\desktop> 

:o :o :o :) :) 
```