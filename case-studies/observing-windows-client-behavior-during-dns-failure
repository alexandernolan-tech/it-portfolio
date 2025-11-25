# Case Study: Observing Windows Client Behavior During DNS Failure

## Environment
- Windows 10 VM in Virtual Box
- Control Panel > IPv4 settings accessible
- Browser available

## Symptoms
- Web pages showed DNS_PROBE_FINISHED_BAD_CONFIG
- nslookup could not reach a DNS server and timed out
- Previously cached sites failed to load, indicating the issue wasn’t masked by local DNS cache

## Initial Troubleshooting
- Verified basic connectivity by pinging a known-good IP (8.8.8.8), which succeeded
- Checked multiple applications to rule out a browser-only problem
- Confirmed the VM still held a valid IP address and gateway
- Reviewed DNS servers in ipconfig /all to verify the system was using the expected resolvers

## Investigation
Once basic connectivity was confirmed, I began isolating whether the failure was DNS-specific or a broader network problem. Loading any website caused the browser to hang before returning a DNS configuration error. Other applications relying on hostname resolution showed the same behavior, suggesting the issue was not limited to HTTP.

Running nslookup resulted in a timeout and reported the DNS server as Unknown, which indicated the VM could reach the network but not a functioning resolver. Checking the local DNS cache with ipconfig /displaydns showed stale entries but nothing new resolving, reinforcing that no names were being translated.

To confirm the boundary between what worked and what didn’t, I tested raw IP connectivity again. Pinging an IP address succeeded immediately, but pinging any hostname failed, even before sending packets. This made it clear the network path was fine but name resolution was completely nonfunctional.

## Root Cause
The DNS servers configured in IPv4 settings were invalid, causing all hostname resolution attempts to fail.

## Resolution
I restored the DNS settings to valid resolvers (local server). After applying the correct DNS servers, the client resumed successful name resolution.

## Verification
- Browser loads websites normally
- nslookup returns valid authoritative or recursive answers
- ping hostname resolves to an IP
- ipconfig /displaydns shows updated entries
- No more timeouts

## Key Takeaways
- DNS failures often present as general ‘internet down’ reports.
- Clients will hang waiting for resolution rather than error instantly.
- Testing IP connectivity separately isolates DNS vs networking issues.
- Windows caches DNS locally, so cached records can mask misconfigurations.
- Changing DNS settings allows safe reproduction of real-world outages.
