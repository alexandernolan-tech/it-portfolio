# Case Study: Automating AO3 Reading Analytics (2025)

## Environment
- Windows 11
- Node.js LTS (npm included)
- Firefox (for cookie access and testing)
- AO3-wrapped (updated JavaScript repo)

## Problem
I wanted to generate year-over-year statistics for my AO3 reading history (fandoms, ships, tags, characters, etc.). The original “AO3 Wrapped” Python script I found was outdated and repeatedly failed at authentication due to changes in AO3’s login flow, SSL requirements, and bot protections.

## Investigation
1. Attempted to run the legacy Python repo.
2. Encountered OpenSSL version errors and login failures.
3. Tried modernizing login by adding headers, CSRF handling, and cookies.
4. Attempted to reuse AO3 browser session cookies.
5. Determined that AO3 security changes made the old script unreliable.
6. Looked for updated tools and discovered a recently maintained JavaScript-based AO3 Wrapped project.

## Root Cause
The original Python script depended on older login methods that no longer work reliably with AO3’s updated security and session handling. It also required older OpenSSL compatibility, which conflicted with current Python environments.

## Solution
- Installed Node.js (LTS) to access npm.
- Downloaded and extracted the updated JavaS
