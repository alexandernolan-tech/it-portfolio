# Case Study Outline: Windows VM Networking Failure

## Goal
Break networking inside a Windows 10 VM and restore it.

## Planned Tests
- Disable network adapter
- Incorrect DNS server
- Wrong default gateway
- Test connectivity with `ipconfig` and `ping`

## Expected Learning
Understanding how Windows handles:
- network stack initialization
- DNS resolution
- gateway routing
