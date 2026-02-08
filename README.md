# Cybersecurity Home Lab Setup: Kali Purple & Windows 10

## Lab Requirements
- VMware Workstation Pro 25H2
- 8GB RAM total system memory
- 100GB free disk space
- Windows 10 ISO
- Kali Linux Purple ISO

## Lab Setup Instructions

### Step 1: Install Kali Linux Purple VM
1. Open VMware Workstation Pro 25H2
2. Click "Create a New Virtual Machine"
3. Select "Typical" configuration
4. Choose "Installer disc image file (iso)" and browse to Kali Purple ISO
5. Guest OS: Linux, Version: Debian 11.x 64-bit
6. Virtual machine name: `Kali-Purple-Attacker`
7. Maximum disk size: 40.0 GB, select "Split virtual disk"
8. Click "Customize Hardware" before finishing:
   - Memory: 4096 MB
   - Processors: 2
   - Network Adapter: NAT
   - Remove unnecessary devices (USB, printer, sound)
9. Click "Finish" and power on VM
10. Install Kali with default settings

### Step 2: Install Windows 10 VM
1. In VMware, click "Create a New Virtual Machine"
2. Select "Typical" configuration
3. Choose "Installer disc image file (iso)" and browse to Windows 10 ISO
4. Guest OS: Windows, Version: Windows 10 x64
5. Virtual machine name: `Windows10-Target`
6. Maximum disk size: 60.0 GB, select "Split virtual disk"
7. Click "Customize Hardware" before finishing:
   - Memory: 4096 MB
   - Processors: 2
   - Network Adapter: NAT
   - Remove unnecessary devices
8. Click "Finish" and power on VM
9. Install Windows 10 with default settings
10. Once installed, disable Windows Defender Firewall for lab exercises

### Step 3: Network Configuration
Both VMs use NAT networking automatically. VMware NAT service assigns IP addresses:
- Kali VM: Typically 192.168.xxx.xxx
- Windows VM: Typically 192.168.xxx.xxx (different from Kali)
- Both VMs can communicate with each other
- Host can access both VMs

**To verify connectivity:**
1. Start both VMs
2. In Kali terminal: `ip a` (note the IP address)
3. In Windows command prompt: `ipconfig` (note the IP address)
4. From Kali: `ping [Windows-IP]` (should receive replies)

## 📸 Required Screenshots for Setup Verification

1. **Screenshot 1: VMware Resource Verification**
   - VMware Workstation Pro 25H2 main window
   - Show host system has 8GB+ RAM available
   - Show 100GB+ free disk space

2. **Screenshot 2: Both VMs in VMware Library**
   - VMware library panel showing:
     - Kali-Purple-Attacker (powered off)
     - Windows10-Target (powered off)
   - Both VMs visible and created

3. **Screenshot 3: Kali VM Hardware Settings**
   - Hardware tab showing configuration:
     - Memory: 4096 MB
     - Processors: 2
     - Hard Disk: 40GB
     - Network Adapter: NAT
   - Take screenshot before powering on VM

4. **Screenshot 4: Windows VM Hardware Settings**
   - Hardware tab showing configuration:
     - Memory: 4096 MB
     - Processors: 2
     - Hard Disk: 60GB
     - Network Adapter: NAT
   - Take screenshot before powering on VM

5. **Screenshot 5: Both VMs Running**
   - VMware showing both VMs powered on
   - Kali Purple desktop visible
   - Windows 10 desktop visible
   - Both consoles active in single screenshot

6. **Screenshot 6: Network Verification**
   - **Split screen showing:**
     - Left: Kali terminal with `ip a` command results
     - Right: Windows cmd with `ipconfig` results
   - Both showing NAT IP addresses (192.168.xxx.xxx range)
   - IPs should be different but in same subnet

## Quick Connectivity Test

After setup, verify the lab works:

**Important Note About Windows Firewall:**  
When you try to ping the Windows 10 machine from Kali Linux immediately after setup, the ping will fail because Windows Firewall blocks ICMP (ping) requests by default. Here's how to test and fix:

### Test 1: Initial Ping Test (Will Fail)

```bash
# From Kali terminal - find Windows IP first
ip a                         # Note Kali's IP
ping 192.168.xxx.xxx         # Replace with Windows IP
# Expected: "Destination Host Unreachable" or timeout
```

### Test 2: Ping from Windows to Kali (Will Succeed)

```bash
# From Windows Command Prompt
ipconfig                    # Note Windows IP
ping 192.168.xxx.xxx       # Replace with Kali's IP
# Expected: Successful replies (4 packets received)
```

### Why This Happens:
Windows → Kali: Succeeds because Kali Linux allows ICMP by default <br>
Kali → Windows: Fails because Windows Firewall blocks incoming ICMP

### Solution: Disable Windows Firewall for Lab
```bash
# From Windows as Administrator
# Temporarily disable firewall for lab exercises
netsh advfirewall set allprofiles state off

# Verify firewall is off
netsh advfirewall show allprofiles
```

### Test 3: Ping Test After Firewall Disabled
```bash
# From Kali terminal
ping 192.168.xxx.xxx        # Windows IP
# Expected: Successful replies
```

### Optional: Allow Only ICMP Instead of Disabling Firewall
If you prefer to keep some protection:
```bash
# From Windows as Administrator
# Allow ICMPv4-In (ping) through firewall
netsh advfirewall firewall add rule name="Allow ICMPv4-In" dir=in action=allow protocol=icmpv4
```

### Verify Complete Connectivity:
```bash
# From Kali - test both ways
ping [Windows-IP]           # Should succeed after firewall adjustment

# From Windows
ping [Kali-IP]              # Should succeed immediately
```

Remember: Disabling firewall is for lab purposes only. Re-enable when not using the lab:


