## Problems

Feels like two separate problems somehow related to graphics drivers or power:

- Trying to install AMD Radeon drivers causes the PC to crash then restart back to the Windows login screen
  - Based on the different things we tried, seems like anything that tried to interact with the Radeon drivers caused the crash:
    - AMD Radeon driver software
    - AMD Radeon driver removal utility
    - Windows Update

- After 20 minutes or so of inactivity, the PC crashes then restarts back to the Windows login screen

## Things we tried

- Clean reinstall of Windows 10 Home from [USB installation media](https://support.microsoft.com/en-us/windows/create-installation-media-for-windows-99a58364-8c02-206f-aa6f-40c3b507420d)
  - Deleted the partitions on C:\ (128gb SSD) and E:\ (1tb HDD) drives and repartitioned as part of the install process
  - Kept D:\ (1tb SSD called `Games Data`) as it only contained Steam installs and some files we transferred across
  - Double check the drive mappings, they might have changed or I might have mis-remembered

- Ran Windows Update to bring everything up to date, including a couple small optional updates

- Downloaded and tried to install [the AMD Radeon drivers from their website](https://www.amd.com/en/support)
  - On the original Radeon RX580 graphics card, running the installer first popped up a error message:
    ```
    RadeonSoftware.exe - Bad Image c:\Windows\SYSTEM32\atiadlxx.dll is either not designed to run Windows or it contains an error. Try installing the program again [I did and failed] etc., etc. Error status 0xc000012f
    ```
  - After clicking ok, it then opened the Radeon installer, which started "Detecting device and Windows compatibility" or something similar
  - After 10s or so, PC would consistently crash then restart
  - I couldn't find any clear solutions when Googling for the DLL, error code or problems with installing AMD drivers on a clean Windows 10 install - just the generic "try removing old drivers first, run Windows Update, etc"

- Swapped out the graphics card to an alternative RX580 from partner's PC which works fine
  - Installing drivers this time **didn't** pop up with a DLL error, but still reliably crashed after 10s or so of the Radeon installer starting
  - Also tried installing the drivers in Safe Mode with Networking to see if it was a conflict with the default Windows 10 driver - no change, still got the same crash
  - We got the same crashing results with both the minimal driver installer which scans for then downloads the right drivers, as well as specifically selecting the RX580 installer from the website.

- Followed some steps from [this AMD support guide](https://www.amd.com/en/support/kb/faq/gpu-kb1603)
  - #1 - not relevant
    - installer never managed to get the point where it asked for factory reset
    - always crashed before then (and clean Windows install meant it had no existing settings)
  - #2 - didn't try disabling AV
    - no custom AV software installed beyond Windows Defender on default settings via Windows Update
    - could be worth trying but seemed unlikely to help
  - #3 - Windows was fully up to date
  - #4 - installed latest x64 Visual C++ Redistributable
    - no change in behaviour afterwards
    - maybe worth trying installing the x86 package as well?
    - although I didn't think that would help
  - #5 - ran both DISM and SFC scan / repair tools
    - no problems detected
    - no change in behaviour afterwards
  - Checked the AMD installer logs at `C:\Program Files\AMD\CIM\Log\Install.log`
    - couldn't see any obvious issues
    - log files appeared to be cut short after the start of the device scanning entry, presumably due to the crash

- Ran [AMD Cleanup Utility](https://www.amd.com/en/support/kb/faq/gpu-601)
  - Rebooted into Safe Mode when prompted
  - Running the cleanup tool sat there for a bit, then eventually crashed and restarted
  - Had similar behaviour to trying to install the drivers

- Checked Windows event viewer
  - Various errors reported about "system stopped unexpectedly" with error code 41
  - Couldn't find any clear indication about what caused the unexpected reboot though
  - Checking the log timestamps at the time of the reboot didn't have anything obviously wrong
  - Various info and warning messages about drivers and security permissions, but was hard to tell if they were related to the crash or just general noise
  - Googling error 41 and similar things just gave general "update your drivers / Windows / try a system restore" advice

- Ran Windows memory check
  - No errors detected

- Disabled "restart on crash" Windows setting
  - Thought maybe blue screen with error info was being hidden by auto-restart
  - No change after disabling though - crash still caused restart

- Tried manually uninstalling the display adapter via Device Manager
  - Interestingly caused a crash as well
  - But after reboot, display adapter was listed as a generic display adapter, not a Radeon RX580
  - Running Windows Update again then downloaded and installed Radeon drivers that way
  - A crash still happened, but after reboot Device Manager listed it as a Radeon RX580 again - so it seems like it had managed to install something?

- Tried running a couple of games
  - Was curious if performance was terrible on default Windows Radeon drivers
  - Luke said it seemed normal compared to before the reinstall, so perhaps the default Windows drivers were what he'd been using all along?
  - By that point we had swapped back to the original graphics card and left it at that

- Luke's reasonably confident the power supply is not the issue
  - Thought maybe it didn't have enough power for the new components and auto-shutoff
  - But it's new and should have enough wattage to power everything without problems
  - He had replaced some other components as well, but this crashing on idle / driver install problem appeared before that
  - Problem might have appeared after a Windows Update ages ago, but it's hard to pin it down exactly because it was so long ago

## Things to try

- Change Windows power settings
  - I forgot that Windows Balanced power settings try and put things into power saver mode after 20 mins of inactivity
  - Could try setting to High Performance, and disable the advanced setting that turns off the drives when idle
  - That would explain why the idle crash doesn't happen when a game or video is running, or when the mouse is wiggled
  - It only seems to idle crash when it's left alone for a bit with no game or video running

- Try a nVidia graphics card
  - Various forum threads mentioned similar crash issues with installing Radeon drivers, but no clear cause or solution
  - Would be interesting to see if a nVidia card installs drivers ok, and doesn't have the same idle crash issue
  - Really puzzled why a known-good alternative Radeon RX580 would still have the same issues though - so didn't feel like a hardware issue with the card
  - Clean Windows 10 install means it also shouldn't be a software issue though

- Swap out components to see if the problem still reoccurs
  - Memory check was fine but could try different modules?
  - Didn't try and alternate PSU or hard drives
  - By this point I was struggling for ideas... :slightly_smiling_face:
