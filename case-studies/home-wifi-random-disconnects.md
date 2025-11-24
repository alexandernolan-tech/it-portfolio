# Case Study: Random WiFi Disconnects and “Connected but No Internet” Behavior

## Environment

- Residential home WiFi at family residence  
- Consumer-grade modem/router combination unit  
- Multiple wireless devices (laptops, phones, tablets)  
- Standard 2.4 GHz and/or 5 GHz network configuration  
- ISP-provided hardware, location-dependent signal strength  

## Symptoms

- Devices would **randomly disconnect** from WiFi throughout the day  
- Some devices showed **“Connected, no internet”** despite appearing linked to the network  
- Connection often returned *temporarily* after restarting the router  
- Issue affected **multiple devices**, not a single machine  
- Problems occurred most often in specific rooms of the house  
- Streaming and browsing would stall unpredictably

## Initial Troubleshooting Attempts

- Power-cycled the router frequently  
- Restarted individual devices  
- Temporarily forgot/re-added the WiFi network  
- Moved devices closer to the router  
- Checked for obvious service outages  
- Verified credentials and network name  

These steps provided temporary improvement but did not resolve the underlying problem.

## Investigation

1. **Compared device behavior**
   - Multiple devices (not just one) experienced identical symptoms.
   - This ruled out individual device problems and pointed toward the **router or WiFi environment**.

2. **Observed symptom pattern**
   - Random drops → inconsistent signal strength  
   - “Connected but no internet” → router was staying on, but connection to the ISP or internal routing was failing  
   - Recovery after reboot → router firmware instability or overheating

3. **Considered environmental factors**
   - Distance from router impacted reliability  
   - Certain rooms had worse performance, suggesting **weak signal zones**  
   - Potential interference from walls, appliances, or other networks

4. **Evaluated router behavior**
   - Frequent need to reboot pointed toward:
     - aging hardware  
     - firmware issues  
     - congested WiFi channels  
     - signal interference  
     - overheating  
     - failing ISP-provided gateway  

5. **Compared with known consumer networking issues**
   - ISP-provided gateways often struggle with:
     - multiple devices  
     - inconsistent firmware  
     - reduced throughput under load  
     - poor antenna design  
   - Symptoms were consistent with router hardware degradation or interference.

## Root Cause

The home WiFi network’s **router/gateway was unstable**, likely due to a combination of:

- Weak signal coverage in parts of the house  
- Possible channel congestion or interference  
- Router firmware or hardware instability  
- The device requiring frequent resets to re-establish a stable connection  

Because the issue affected multiple devices and responded temporarily to router restarts, the root problem was located in the **router itself**, not any specific client device.

## Resolution

- Performed repeated router power cycles to temporarily restore connectivity  
- Advised long-term resolution options:
  - replacing the ISP router with a more reliable model  
  - installing a WiFi extender or mesh system for better coverage  
  - relocating the router to a more central location  
  - updating router firmware if available  
  - separating 2.4 GHz and 5 GHz networks to reduce interference  

Ultimately, the recurring symptoms pointed to the need for new hardware or improved home WiFi design.

## Verification

- After router restarts, devices regained full connectivity temporarily  
- Consistency across devices confirmed the issue was network-level  
- Persistent recurrence in the same physical areas indicated coverage limitations  
- Hardware replacement or repositioning resolved the pattern of random disconnects

## Key Takeaways

- When multiple devices experience identical WiFi drops, the issue is almost always **router-side**.  
- “Connected but no internet” indicates the router is alive but its upstream connectivity or internal routing is unstable.  
- Consumer gateways commonly fail over time due to heat, firmware, or interference.  
- Power-cycling fixes symptoms, not root causes — environmental factors and hardware quality matter.  
- Long-term fixes usually require:
  - upgrading router hardware  
  - improving placement  
  - reducing interference  
  - or implementing mesh/extender solutions in larger homes.

