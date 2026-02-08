
---

**#DTP (Dynamic Trunking Protocol)**  
- Cisco-proprietary, now **deprecated**.  
- Auto-negotiates **trunking mode** between switches.  

**Modes:**  
1. `switchport mode dynamic desirable` → **actively requests** trunk.  
2. `switchport mode dynamic auto` → **responds only if asked** (passive).  

**Defaults:**  
- Older switches: `dynamic desirable`  
- Newer switches: `dynamic auto`  

**Disabling DTP:**  
- `switchport nonegotiate` → disables DTP (must pair with `switchport mode trunk` or `access`).  
- `switchport mode access` → **automatically disables DTP**.  

**Encapsulation:**  
- DTP can negotiate **ISL** or **802.1Q** if both supported.  
- Default: `switchport trunk encapsulation negotiate`  

**Verification:**  
- Use: `show interfaces [int] switchport`  
- **"Negotiation of Trunking: On"** → DTP active (`desirable`, `auto`, or `trunk`).  
- **"Negotiation of Trunking: Off"** → DTP disabled (`access` mode or `nonegotiate`).

---

**#VTP (VLAN Trunking Protocol)**  
- Centralized VLAN management:  
  - **Server**: creates/modifies VLANs.  
  - **Clients**: sync VLAN DB from server.  
- All switches in same **VTP domain** share VLAN info.  

*(Note: VTP also deprecated in modern networks due to risks—use with caution.)*
- On a switch by default a VTP will have no domain name. 
- In a network for VTP to synchronize across all the switches, they need to have same VTP domain name. 
- When a VTP server with no domain name receives a VTP advertisement it automatically joins that domain. 
- Commands: 
    - `show vtp status`
    - `vtp domain [domain name]` 
    <!-- after using this above command the revision will increment by 1 -->
    - `vtp mode [mode type]` : used to set the VTP mode (server, client or transparent)
- scenerio: What if there is non-transparent VTP switch right next to a transparent VTP mode switch, then how will the non-transparent will receive the advertisement? 
We can add the transparent switch to the domain, it will not update its database based on the advertiment but will forward it. 


