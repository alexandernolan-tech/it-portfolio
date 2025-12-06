# Case Study: Android Connectivity Failure After ADB Debloating (Samsung S22)

## Summary
After aggressively debloating Google and Samsung system components via ADB, a Samsung Galaxy S22 lost the ability to connect to Wi-Fi or mobile data. Network Reset, Airplane Mode toggling, and standard troubleshooting procedures failed. Investigation revealed that disabling certain core Android networking/system packages prevented the Wi-Fi supplicant from initializing, resulting in complete radio failure. Re-enabling system packages (except Knox) restored full connectivity.

## Environment
- Device: Samsung Galaxy S22
- OS: Android 13
- Network: Wi-Fi + LTE
- Tools:
  - ADB
  - Windows PC
  - Open Camera
- Status before issue:
  - ADB debugging enabled
  - Multiple system packages disabled
  - Google Play Services partially disabled/throttled

## Symptoms
- Wi-Fi would not connect to any network, including new networks and hotspots
- Mobile data remained stuck on “Searching…”
- Entering **Settings → Connections** caused Settings app to freeze or become unresponsive
- Wi-Fi scans detected SSIDs but instantly failed to authenticate
- Camera and storage temporarily malfunctioned due to related media/storage components being disabled (later resolved)

## Investigation

### Initial suspicion
- Incorrect Wi-Fi password
- Router-side issue
- eSIM/carrier provisioning
- DAM/Knox restrictions

### Evidence collected
- `adb shell dumpsys wifi` showed:
  - Supplicant state: **UNINITIALIZED**
  - MAC address: **02:00:00:00:00:00**
- Device could scan networks but instantly rejected attempts to connect
- Network reset did not resolve the issue
- Mobile data and Wi-Fi failed simultaneously
- IMEI present (modem alive)

### Key insight
Wi-Fi hardware was functional, but the Wi-Fi identity/supplicant layer never initialized due to disabled system components. MAC address fallback and supplicant state indicated system components required for radio identity were unavailable.

## Root Cause
Critical Android networking/system packages were disabled using ADB. Even though core Wi-Fi components seemed unrelated, hidden dependencies prevented the Wi-Fi supplicant and radio registration layers from initializing. Android’s network stack depends on multiple system services, and disabling certain packages results in the entire radio subsystem failing.

## Resolution
Re-enabled all previously disabled **system** packages (except Knox) using:
```bash
adb shell cmd package install-existing <package>
```
followed by a reboot.

After re-enabling system apps:
- Wi-Fi authenticated normally
- Mobile network registration resumed
- Settings → Connections stopped crashing
- Camera and storage returned to normal operation

## Verification
- Tested on multiple Wi-Fi networks
- Confirmed mobile data connectivity
- `dumpsys wifi` showed valid supplicant state and valid MAC
- No further radio failures after reboot

## Lessons Learned
- Android networking relies on multiple invisible framework dependencies; disabling packages via ADB can silently break radios.
- Removing or disabling Google Play Services is not equivalent to privacy; core Google/Samsung frameworks assist with device identity.
- Safe de-Googling requires:
  - Restricting permissions
  - Disabling background data
  - Using firewalls
  - Avoiding removal of system frameworks
- Radios breaking after debloating is not a user error—modern Samsung/Android integration assumes certain components exist.

## Preventative Actions
- Avoid disabling unknown system packages with ADB unless explicitly documented safe
- Favor permission controls instead of app removal:
  - Background data off
  - Restrict location
  - Limit Google Play Services only via permissions and network rules
- Keep a list of disabled packages for rollback

## Key Takeaways
- If **Supplicant = UNINITIALIZED** and MAC = 02:00:00:00:00:00, Wi-Fi identity layers are failing
- IMEI present + no Wi-Fi means radio infrastructure is running but uninitialized
- Re-enabling system components restores identity and connectivity without factory reset
- Modern Samsung firmware expects certain components to exist even if the user doesn’t use them

## Status
- Resolved
- Device functioning normally
- Connectivity fully restored
- Google Play Services currently throttled using safer methods (permissions + network restrictions instead of system removal)
