# Case Study: Windows File Picker Freezing in Chrome and Firefox

## Environment

- Windows 11 client  
- Previously had OneDrive folder redirection enabled and later removed  
- OEM software installed in the past: MSI Center, Nahimic, audio enhancement tools, Xbox/Game overlay components  
- Multiple modern (WinUI) shell components and legacy shell extensions present  

## Symptoms

- Native file picker froze when trying to interact with it in **Chrome** and **Firefox** (e.g., clicking Desktop, Documents, or other folders)
- File Explorer worked normally when browsing folders directly
- HTML-based upload dialogs (such as ChatGPT’s file selector) worked without issues
- Quick Access contained items that behaved strangely or didn’t respond
- `shell:` shortcuts for Documents/Pictures/Music did not open correctly
- Some Explorer thumbnail/icon cache files appeared in Explorer but not from the command line
- `DISM /Online /Cleanup-Image /RestoreHealth` hung at ~62.3% and did not complete
- System had a history of OEM audio and shell-related software (MSI/Nahimic) previously installed and partially removed

## Investigation

1. **Confirmed scope of the issue**
   - Reproduced the freeze in both Chrome and Firefox using native file-upload dialogs.
   - Verified that normal File Explorer windows behaved correctly.
   - Verified that ChatGPT’s file picker (HTML-based) worked, which suggested the core filesystem was fine and the problem was specific to the **WinUI Common File Dialog**.

2. **Checked shell folders and Quick Access**
   - Used `shell:` commands and the GUI to inspect Desktop, Documents, Pictures, and Music.
   - Observed that some known folders did not open correctly or routed to unexpected locations.
   - Quick Access behavior was inconsistent and appeared to reference stale or invalid paths, likely left over from earlier OneDrive redirection.

3. **Looked for shell extension conflicts**
   - Used tools (like Autoruns) to review non-Microsoft Explorer/shell extensions (MSI, Nahimic, LibreOffice, etc.).
   - Disabled/uninstalled several OEM-added shell hooks and audio enhancers.
   - Confirmed that, while these improved overall stability, the **file picker freeze persisted**, which pointed to a deeper shell/WinUI issue rather than just third-party shell extensions.

4. **System integrity checks**
   - Ran `sfc /scannow`: no integrity violations reported.
   - Ran `DISM /Online /Cleanup-Image /RestoreHealth`: consistently hung around 62.3%, suggesting the **component store was readable but the repair step was getting stuck**, likely due to higher-level shell/WinUI package issues rather than core OS files.

5. **Narrowing it down to WinUI shell and KnownFolders**
   - The pattern suggested:
     - Explorer (classic shell) handled broken folder mappings gracefully.
     - The modern WinUI file picker (used by Chrome/Firefox) was freezing when resolving **Known Folder GUIDs** (Documents, Desktop, etc.) and Quick Access/Home data.
   - Historical improper OneDrive removal likely left **broken KnownFolder mappings** and corrupted Quick Access/Home databases.

## Root Cause

The freeze was caused by corruption in the **modern Windows shell / WinUI layer**, not in the core OS:

- Stale or broken **Known Folder GUID mappings** after OneDrive detachment.
- Corrupted **Home/Quick Access** and jump-list databases (Recent/AutomaticDestinations/CustomDestinations).
- Issues with modern **WinUI shell packages** (ShellExperienceHost, StartMenuExperienceHost, File Explorer’s AppX component, and related cloud/store bindings).
- Legacy OEM shell-related software (MSI/Nahimic) had contributed to instability but was not the final root cause once removed.

The component store itself was healthy; the problem sat in the **shell state and WinUI packages** that the file picker depends on.

## Resolution

1. **Removed lingering OEM/shell components**
   - Uninstalled MSI Center, Nahimic-related components, and other OEM utilities that hooked into Explorer and audio.
   - Disabled or removed related services and tasks where appropriate.

2. **Reset key WinUI shell packages**
   - In elevated PowerShell, ran `Reset-AppxPackage` on:
     - `Microsoft.Windows.ShellExperienceHost`
     - `Microsoft.Windows.StartMenuExperienceHost`
     - `Microsoft.Windows.FileExplorer`
     - `Microsoft.Windows.Search`
     - `Microsoft.Windows.CloudStore` (and related where present)
   - This forced Windows to rebuild the modern shell components used by the file picker and Home/Quick Access.

3. **Repaired Known Folder mappings**
   - Explicitly reset registry entries under:
     - `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders`
   - Ensured key folders pointed to local paths:
     - Documents → `%USERPROFILE%\Documents`
     - Pictures  → `%USERPROFILE%\Pictures`
     - Music     → `%USERPROFILE%\Music`
     - Desktop   → `%USERPROFILE%\Desktop`

4. **Cleared Quick Access and recent-item caches**
   - Deleted contents of:
     - `%AppData%\Microsoft\Windows\Recent\`
     - `%AppData%\Microsoft\Windows\Recent\AutomaticDestinations\` (when present)
     - `%AppData%\Microsoft\Windows\Recent\CustomDestinations\`
   - Removed Explorer `.db` cache files in:
     - `%LocalAppData%\Microsoft\Windows\Explorer\` (icon/thumbnail and nav/home databases), where possible.

5. **Restarted shell**
   - Restarted `explorer.exe` to force the shell to reload with a clean, rebuilt state.

## Verification

- Tested native file pickers in Chrome and Firefox using simple upload sites (e.g., file.io) that are known to use the WinUI Common File Dialog.
- File pickers opened and allowed navigation to Desktop, Documents, and other folders **without freezing**.
- Normal File Explorer behavior remained stable.
- Known folders resolved correctly.
- HTML-based upload dialogs (e.g., ChatGPT) continued to work as expected.
- No further DISM hangs or shell-related freezes appeared.

## Key Takeaways

- A healthy component store (SFC/DISM clean) does **not** guarantee a healthy WinUI shell or file picker; those can be corrupted independently.
- Incomplete OneDrive detachment can leave behind broken KnownFolder GUIDs and corrupt Quick Access/Home databases, which modern file pickers rely on heavily.
- HTML-based file selectors can be used as a diagnostic tool to differentiate between **browser issues**, **filesystem issues**, and **WinUI file picker issues**.
- Many “reinstall Windows” scenarios can be avoided by:
  - Resetting WinUI AppX shell packages,
  - Repairing KnownFolder mappings,
  - Clearing Quick Access / Recent / Explorer DB caches,
  - And restarting the shell cleanly.
- Careful, layered troubleshooting (ruling out browser, hardware, drivers, core OS, then shell) leads to a precise fix instead of a full wipe-and-reinstall.
