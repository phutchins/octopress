---
layout: post
title: "Reverse Scrolling in Windows 7"
date: 2013-11-15 19:15
comments: true
published: true
categories: [windows]
---
There is no straight forward way built into Windows 7 to reverse the scrolling direction for your mouse. If you're like me, you primarily use a Mac with OSX. Once you get used to the natural scrolling, its kind of nice. I got tired of trying to wrap my brain around switching back and forth between different scrolling directions when at work or home and decided to find a way to switch my Windows 7 machine to match my Mac.

There are two real ways to do this...

Option One
---
This is the way I chose. I chose this becuase finding the registry key for my house was not straight forward. The logitech driver overrides the hardware tab in the windows control panel which you would use to find the key.

* Open power Windows Power Shell by browsing to Start Menu > All Applications > Accessories > Windows PowerShell

* Wait until you see a prompt that looks something like this...
``` bash
PS C:\Users\flip>
```

* Then enter...
``` bash
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Enum\HID\*\*\Device` Parameters FlipFlopWheel -EA 0 | ForEach-Object { Set-ItemProperty $_.PSPath FlipFlopWheel 1 }
```

* All done. You may need to unplug your mouse and plug it back in or reboot to see your changes.

Option Two
---
This is the manual way of doing things. You will change a flag in the Windows registry to get the desired effect.

* Open the "Mouse Properties" dialog in your control panel and click on the "Hardware" tab.
* Open the device properties window by clicking on the "Properties" button in the bottom right corner just above the "Apply" and "Help" buttons.
* In the device properties dialogue window, click on the "Details" tab and select "Hardwire Ids" from the "Property" drop-down list.  You should see a list of entries starting with "HID...".  Write down the values that start with "HID\VID_???...".
* Close device properties by clicking "Cancel".
* Close "Mouse Properties" by clicking "Cancel".
* Find and open the application "regedt32.exe" as an administrator, if you are not already logged in as one.
* Navigate to "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\HID", and look for an entry that resembles the "VID_???..." number that you wrote down earlier in step 3.
* Open each child entry under your specified "VID_???..." key, and open the "Device Parameters" key. In my case, there were two child entries with a long "serial number"-like alphanumeric text string.
* There should be a key named "FlipFlopWheel" under each of the "Device Parameters" key.  Reverse the entry value for the "FlipFlopWheel".  i.e. if default is "0" (zero) change it to "1", and vice-versa.
* Do the same step 9, for each alphanumeric text string key entry, and their "Device Parameters", "FlipFlopWheel".
* Close "regedt32.exe".
* Unplug your USB mouse, and listen or watch for Windows 7's acknowledgement and confirmation that it has recognized the event of the mouse being unplugged.  In my case, it was simply a "USB device unplugged" system sound. Re-connect the mouse and the new settings should have taken effect.  If not, simply shutdown and restart Windows 7.

Enjoy life a little bit more now...

