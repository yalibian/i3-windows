# i3-windows

Forked from [pmb6tz/windows-desktop-switcher](https://github.com/pmb6tz/windows-desktop-switcher) and [Depau/swin](https://github.com/Depau/swin)[jnoynaert/window-mover](https://github.com/jnoynaert/window-mover), to minimally reproduce some desirable i3/sway-like behavior on Windows.

- Switch to virtual desktops 1-9 using `⊞-#` and backfill # of desktops if needed
- Move active windows to virtual desktops 1-9 using `⊞-shift-#` and backfill desktops if needed
- Close windows with `⊞-shift-q`
- Open WSL with `⊞-enter` (need to create and edit link in user_config.ahk)
- Open PowerShell with `⊞-shift-enter`

# Dependencies

- Windows 10
- [AutoHotkey](https://autohotkey.com/download/) v1.1+
- [VirtualDesktopAccessor.dll](https://github.com/Ciantic/VirtualDesktopAccessor)


## Running on boot

You can make the script run on every boot with either of these methods.

### Simple (Non-administrator method)

1. Press `Win + R`, enter `shell:startup`, press `OK`
2. Create a shortcut to the `desktop_switcher.ahk` file here

### Advanced (Administrator method)

Windows prevents hotkeys from working in windows that were launched with higher elevation than the AutoHotKey script (such as CMD or Powershell terminals that were launched as Administrator). As a result, Windows Desktop Switcher hotkeys will only work within these windows if the script itself is `Run as Administrator`, due to the way Windows is designed. 

You can do this by creating a scheduled task to invoke the script at logon. You may use 'Task Scheduler', or create the task in powershell as demonstrated.
```
# Run the following commands in an Administrator powershell prompt. 
# Be sure to specify the correct path to your desktop_switcher.ahk file. 

$A = New-ScheduledTaskAction -Execute "PATH\TO\desktop_switcher.ahk"
$T = New-ScheduledTaskTrigger -AtLogon
$P = New-ScheduledTaskPrincipal -GroupId "BUILTIN\Administrators" -RunLevel Highest
$S = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries -ExecutionTimeLimit 0
$D = New-ScheduledTask -Action $A -Principal $P -Trigger $T -Settings $S
Register-ScheduledTask WindowsDesktopSwitcher -InputObject $D
```

The task is now registered and will run on the next logon, and can be viewed or modified in 'Task Scheduler'. 

## Original credits

- Thanks to [Ciantic/VirtualDesktopAccessor](https://github.com/Ciantic/VirtualDesktopAccessor) (for the DLL) and [sdias/win-10-virtual-desktop-enhancer](https://github.com/sdias/win-10-virtual-desktop-enhancer) (for the DLL usage samples) our code can move windows between desktops.
