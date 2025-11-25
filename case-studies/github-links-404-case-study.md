# Case Study: GitHub Relative Path Links Returning 404

## Environment
- GitHub repository (`it-portfolio`)
- Markdown documentation stored in `/case-studies`
- README located in the repo root
- LinkedIn used for external link previews
- Mix of relative links and incorrectly named paths

---

## Symptoms
- Clicking case study links in the README resulted in **404 Not Found**.
- LinkedIn project links also produced 404 errors.
- All `.md` files existed in `case-studies`, but could not be accessed via the README.
- Relative links worked internally but failed when viewed outside GitHub.

---

## Initial Troubleshooting
- Verified that each `.md` file existed in the correct folder.
- Compared expected filenames vs. actual filenames (checking for typos and mismatches).
- Tested relative links directly in GitHub.
- Opened README from external viewers to replicate the issue.
- Attempted manual navigation through GitHub’s file tree.

---

## Investigation

### 1. Relative Link Behavior
Relative paths like:
(case-studies/home-wifi-case-study.md)
worked **inside GitHub**, but broke when the README was viewed on platforms like LinkedIn or external renderers.

These systems do not inherit GitHub’s repository path context.

---

### 2. Filename Mismatches
Some README entries referenced filenames that did not exist (e.g., `windows-file-picker-case-study.md` vs. `file-picker-case-study.md`).

Even one character difference results in a 404.

---

### 3. External Viewer Behavior
LinkedIn treats relative paths as if they are local to LinkedIn, not GitHub.

This guarantees a 404.

Example:
case-studies/script-mod-conflict-case-study.md
LinkedIn tried to open:
linkedin.com/.../case-studies/script-mod...
→ Instant 404.

---

### 4. Absolute URL Testing
Manually copying the full GitHub link worked everywhere:
https://github.com/alexandernolan-tech/it-portfolio/blob/main/case-studies/home-wifi-case-study.md

Switching all links to the absolute path solved the issue universally.

---

## Root Cause
A combination of:

1. **Relative links** that external platforms cannot resolve  
2. **Incorrect filenames** referenced in the README

Both contributed to the 404 failures.

---

## Resolution
- Replaced *all* relative Markdown links with **absolute GitHub URLs**.
- Corrected incorrect filenames in the README.
- Added descriptive text-based Markdown links for better readability:
📄 View Case Study
- Tested links:
- inside GitHub  
- logged-out GitHub sessions  
- LinkedIn previews  
- external markdown renderers  

All links resolved correctly after updates.

---

## Verification
- Opened every case study link across multiple platforms.
- Confirmed that GitHub rendered each file without 404.
- Checked LinkedIn cards after replacing links.
- Verified that GitHub’s internal navigation still functioned normally.

---

## Key Takeaways
- Relative links are **context-dependent** and break easily when shared externally.
- Absolute URLs guarantee cross-platform reliability.
- Small discrepancies in filenames cause path failures—consistent naming matters.
- This mirrors real-world IT troubleshooting involving:
- wrong file paths  
- misrouted references  
- dependency failures  
- environment assumptions  
- Documentation systems benefit from:
- standardized structure  
- naming conventions  
- link testing  
