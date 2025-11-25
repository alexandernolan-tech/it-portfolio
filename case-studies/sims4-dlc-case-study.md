# Case Study: Sims 4 DLC Not Appearing Despite Ownership

## Environment

- Windows 11  
- EA App (latest version)  
- The Sims 4 installed on a secondary drive  
- Large DLC library (1000+ USD of content purchased over time)  
- Custom content and mods previously installed  
- Game files partially managed through File Explorer during modding

## Symptoms

- The Sims 4 launched normally but **did not recognize any owned DLC**.  
- Game acted as if only the base game was installed.  
- EA App showed all content as owned.  
- Attempting to repair or reload DLC through the EA App had no effect.  
- Reinstalling the game, restarting the EA App, and relogging did not resolve the issue.

## Initial Troubleshooting Attempts

- Restarted PC  
- Restarted EA App  
- Logged out and back in  
- Verified DLC ownership in EA App  
- Fully reinstalled The Sims 4  
- Checked mods and CC folders  
- Ensured EA App was running with correct account

None of these corrected the missing DLC.

## Investigation

1. **Confirmed EA App recognized DLC ownership**  
   - Not an account issue.  
   - Not a cloud licensing problem.

2. **Checked install directory integrity**  
   - Verified The Sims 4 folder structure.  
   - Noted presence of an unexpected folder inside game directory.

3. **Reviewed game folder path consistency**  
   - The Sims 4 relies on a predictable file tree.  
   - Any foreign or malformed folders can cause the DLC loader to fail silently.

4. **Identified a stray folder** inside the main game directory that did not belong to The Sims 4 or EA’s expected structure.

This stray folder appeared to interfere with the game’s ability to enumerate installed DLC.

## Root Cause

A **misplaced stray folder** inside The Sims 4 installation directory caused the DLC loader to fail.  
Because Sims 4 scans directories to determine installed packs, the unexpected folder broke that scan, resulting in the game believing no DLC was present.

This is a known quirk of The Sims 4: nonstandard files or folders inside the main installation path can disrupt how DLC manifests in-game.

## Resolution

- Removed the stray folder from The Sims 4 install directory.  
- Relaunched the game.  
- All purchased DLC immediately appeared and loaded correctly.

No reinstall or further EA App troubleshooting was required once the directory was clean.

## Verification

- Sims 4 recognized all DLC on next launch.  
- EA App no longer attempted to redownload packs.  
- Saved games using DLC loaded without warnings.  
- Game functioned normally with full DLC access restored.

## Key Takeaways

- Sims 4 uses a strict directory structure; foreign files or folders can prevent DLC discovery.  
- Reinstalling the game does not always remove user-created or leftover folders.  
- When troubleshooting missing DLC, folder hygiene in the install path is essential.  
- A simple directory scan can resolve issues that appear on the surface to be account or licensing problems.

