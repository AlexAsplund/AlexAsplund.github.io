---
layout: post
title: Just released a Powershell module for todoist! PSTodoist
categories: project
project: PSTodoist
github: https://github.com/AlexAsplund/PSTodoist
projectdescription: A powershell module for todoist that uses the v8 api.
tags: [Todoist, Powershell, Productivity]
---
A while back I decided to create a powershell module for todoist.

First release of it wasn't too good. Lot's of bugs, no documentation and not to maintainable.
After that I created some plaster templates for work (with the help of a few tutorials) with PSake and platyPS.

A couple of weeks ago i saw that Todoist had released the V8 api in beta, and it's a lot more fun to deal with than the previous V7 API.

So I started making a new module from scratch so that i could learn more on how to create good modules.
It's not bug free, and I'm not a developer so It's not all according to best practice but it works fine and has a lot of features.

You can find it here on Github: [https://github.com/AlexAsplund/PSTodoist](https://github.com/AlexAsplund/PSTodoist)

Documentation of functions generated with platyPS: [https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/PSTodoist.md](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/PSTodoist.md)

Any input on how to make it better is appreciated.

I will make a follow-up post with how this was created.

# Some examples:

```Powershell
Set-TodoistToken -Token abcdef12345
Get-TodoistTask | Where-Object {$_.Content -eq "Do dishes"} | Update-TodoistTask -Content "Buy dishwasher"

Set-TodoistToken -Token abcdef12345
$Task = Get-TodoistTask | Where-Object {$_.Content -eq "Do dishes"}

# To close task
$Task | Close-TodoistTask

# To reopen Task
$Task | Open-Todoist Task

# To update task with class method
$Task.Content = 'Buy dishwasher'
$Task.Update()

# To close task with class method
$Task.Close()

# To update a class with function
Update-TodoistTask -Id $Task.Id -Content "Buy cheap dishwasher"

# To add comment to task
$Task | New-TodoistComment -Category Task -Content "Researched dishwashers online"

# To add comment with a file to task
$Task | New-TodoistComment -Category Task -Content "PDF of dishwasher" -AddAttachment -AttachmentResourceType "file" -AttachmentFileType "application/pdf" -AttachmentFileUrl "http://contoso.com/files/dishwasher.pdf" -AttachmentFileName "Dishwasher.pdf"

# To remove task
$Task | Remove-TodoistTask

# To create a new label
New-TodoistLabel -Name "At home"

# To create a new project
New-TodoistProject -Name "Repair dishwasher"
```


# Installing PSTodoist


## Install latest release

```Powershell
    [Net.ServicePointManager]::SecurityProtocol = "tls12, tls11, tls"

    $ModulePath = "C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\"

    $Version = "0.0.2"



    if((Test-Path ".\PStodoist-$Version")) {

        Remove-Item ".\PStodoist-$Version" -Force -Recurse

    }

    Invoke-RestMethod -Uri "https://github.com/AlexAsplund/PSTodoist/archive/v$Version.zip" -OutFile ".\PStodoist-$Version.zip"

    if((Test-Path ".\PStodoist-$Version")) {

        Remove-Item ".\PStodoist-$Version" -recurse -force

    }


    Expand-Archive ".\PStodoist-$Version.zip" -DestinationPath .\

    If(!(Test-Path "$ModulePath\PSTodoist")) {

        New-Item -ItemType Directory -Path $ModulePath -Name 'PSTodoist'

    }

    If(!(Test-Path "$ModulePath\PSTodoist")) {

        Remove-Item "$ModulePath\PSTodoist\*" -Recurse -Force 

    }

    cp ".\PSTodoist-$Version\Release\PSTodoist\*" -Destination "$ModulePath\PSTodoist\" -Recurse -Force

```

## Install latest build

```Powershell
    [Net.ServicePointManager]::SecurityProtocol = "tls12, tls11, tls"

    $ModulePath = "C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\"

    $Version = "master"



    if((Test-Path ".\PStodoist-$Version")) {

        Remove-Item ".\PStodoist-$Version" -Force -Recurse

    }

    Invoke-RestMethod -Uri "https://github.com/AlexAsplund/PSTodoist/archive/master.zip" -OutFile ".\PStodoist-$Version.zip"

    if((Test-Path ".\PStodoist-$Version")) {

        Remove-Item ".\PStodoist-$Version" -recurse -force

    }


    Expand-Archive ".\PStodoist-$Version.zip" -DestinationPath ".\"
    If(!(Test-Path "$ModulePath\PSTodoist")) {

        New-Item -ItemType Directory -Path $ModulePath -Name 'PSTodoist'

    }

    If(!(Test-Path "$ModulePath\PSTodoist")) {

        Remove-Item "$ModulePath\PSTodoist\*" -Recurse -Force 

    }

    cp ".\PSTodoist-$Version\Release\PSTodoist\*" -Destination "$ModulePath\PSTodoist\" -Recurse -Force
```

# Functions

### [Close-TodoistTask](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Close-TodoistTask.md)

    Close-TodoistTask -Id 123456789
    Close-TodoistTask -Id 098765432,123466721
    Get-TodoistTask | ? {$_.Content -match "Stop Procrastinating"} | Close-TodoistTask

### [Get-TodoistComment](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Get-TodoistComment.md)

    Get-TodoistComment -Id 1234 -Category "Task"
    Get-TodoistComment -Id 5678 -Category "Category"

### [Get-TodoistLabel](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Get-TodoistLabel.md)

    Get-Todoistlabel
    Get-Todoistlabel -Token 1q2w3e4r5t6y7u8i99o0p
    $labels = Get-Todoistlabel | ? {$_.Content -match "My label"}

### [Get-TodoistProject](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Get-TodoistProject.md)

    Get-Todoistproject
    Get-Todoistproject -Token 1q2w3e4r5t6y7u8i99o0p
    $projects = Get-Todoistproject | ? {$_.Content -match "Do dishes"} | Remove-TodoistProject

### [Get-TodoistTask](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Get-TodoistTask.md)

    Get-TodoistTask
    Get-TodoistTask -Token 1q2w3e4r5t6y7u8i99o0p
    $Tasks = Get-TodoistTask | ? {$_.Content -match "dishes"}

### [New-TodoistComment](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/New-TodoistComment.md)

    New-TodoistComment -Id 1234 -Content "Customer contacted"
    New-TodoistComment -Id 1234 -Content "New report" -Category Task -Verbose -AddAttachment -AttachmentResourceType "file" -AttachmentFileType "application/pdf" -AttachmentFileUrl "http://www.orimi.com/pdf-test.pdf" -AttachmentFileName "file.pdf"

### [New-TodoistLabel](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/New-TodoistLabel.md)

    New-TodoistLabel -Name "My cool label"

### [New-TodoistProject](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/New-TodoistProject.md)

    New-TodoistProject -Name "My cool project"

### [New-TodoistTask](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/New-TodoistTask.md)

    New-TodoistTask -Content "Put out forest fires" -DueString "Tomorrow 4am"

### [Open-TodoistTask](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Open-TodoistTask.md)

    Open-TodoistTask -Id 123456789
    Open-TodoistTask -Id 098765432,123466721
    Get-TodoistTask | ? {$_.Content -match "Start Procrastinating"} | Open-TodoistTask

### [Remove-TodoistComment](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Remove-TodoistComment.md)

    Remove-TodoistComment -Id 123456789
    Remove-TodoistComment -Id 098765432,123466721
    Get-TodoistComment | ? {$_.Content -match "Big"} | Remove-TodoistComment

### [Remove-TodoistLabel](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Remove-TodoistLabel.md)

    Remove-TodoistLabel -Id 123456789
    Remove-TodoistLabel -Id 098765432,123466721
    Get-TodoistLabel | ? {$_.Content -match "Big"} | Remove-TodoistLabel

### [Remove-TodoistProject](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Remove-TodoistProject.md)

    Remove-TodoistProject -Id 123456789
    Remove-TodoistProject -Id 098765432,123466721
    Get-TodoistProject | ? {$_.Content -match "Big"} | Remove-TodoistProject

### [Remove-TodoistTask](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Remove-TodoistTask.md)

    Remove-TodoistTask -Id 123456789
    Remove-TodoistTask -Id 098765432,123466721
    Get-TodoistTask | ? {$_.Content -match "Stop Procrastinating"} | Remove-TodoistTask

### [Set-TodoistToken](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Set-TodoistToken.md)

    Set-TodoistToken 1q2w3e434rt56y7ui8o09

### [Update-TodoistComment](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Update-TodoistComment.md)

    Update-TodoistComment -Id 1234 -Name "My updated comment"

### [Update-TodoistLabel](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Update-TodoistLabel.md)

    Update-TodoistLabel -Id 1234 -Name "My updated label"

### [Update-TodoistProject](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Update-TodoistProject.md)

    Update-TodoistProject -Id 1234 -Name "My updated project"

### [Update-TodoistTask](https://github.com/AlexAsplund/PSTodoist/blob/master/docs/en-us/Update-TodoistTask.md)

    Update-TodoistTask -Id 1234 -Content "Put out forest fires" -DueString "In 2 years"