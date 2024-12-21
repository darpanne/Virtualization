# Virtualization Repository

## Objective
This repository demonstrates a comprehensive range of virtualization tasks and configurations, showcasing expertise in creating, managing, and optimizing virtual environments using Hyper-V and VMware. The goal is to build a strong foundation in virtualization and gain hands-on experience in advanced configurations.

---

## Skills Learned
- Setting up and configuring virtual machines
- Managing virtual networks (NAT, internal, and private switches)
- Remote Desktop configuration and secure access
- Checkpoint management for state recovery
- Advanced disk operations (expand, compact, merge)
- Failover clustering and high availability
- Automating VM deployment using PowerShell
- Nested virtualization setup and management
- Performance monitoring and resource optimization
- iSCSI shared storage configuration
- Security hardening for virtual environments

---

## Tools Used
- **Hyper-V Manager**
- **VMware Workstation**
- **PowerShell**
- **Remote Desktop Protocol (RDP)**
- **Disk2VHD**
- **Windows Server Manager**
- **Cluster Manager**

---

## Tasks

```powershell
### 1. Remote Desktop Configuration
# Enable Remote Desktop
Enable-PSRemoting -Force
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections" -Value 0

# Allow Remote Desktop through Firewall
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

### 2. Virtual Switch Setup
# Create NAT Switch
New-VMSwitch -SwitchName "dneupaneNATSwitch" -SwitchType NAT -NatSubnetAddress "192.168.100.0/24"
New-NetIPAddress -IPAddress 192.168.100.1 -PrefixLength 24 -InterfaceAlias "vEthernet (dneupaneNATSwitch)"

# Create Internal Switch
New-VMSwitch -SwitchName "dneupaneInternalSwitch" -SwitchType Internal

# Create Private Switch
New-VMSwitch -SwitchName "dneupanePrivateSwitch" -SwitchType Private

### 3. Checkpoint Management
# Create a Checkpoint
Checkpoint-VM -Name "dneupaneTestVM" -SnapshotName "Checkpoint1"

# Restore Checkpoint
Restore-VMCheckpoint -VMName "dneupaneTestVM" -Name "Checkpoint1"

# Remove Checkpoint
Remove-VMCheckpoint -VMName "dneupaneTestVM" -Name "Checkpoint1"

### 4. Disk Management
# Expand Virtual Hard Disk
Resize-VHD -Path "C:\VMs\dneupaneTestVM.vhdx" -SizeBytes 40GB

# Compact Virtual Hard Disk
Optimize-VHD -Path "C:\VMs\dneupaneTestVM.vhdx"

# Merge Virtual Hard Disk
Merge-VHD -Path "C:\VMs\dneupaneParentDisk.vhdx" -DestinationPath "C:\VMs\dneupaneMergedDisk.vhdx"

### 5. Automated Deployment
# Deploy Multiple Virtual Machines
for ($i=1; $i -le 3; $i++) {
    New-VM -Name "dneupaneTestVM$i" -MemoryStartupBytes 1GB -Generation 2 -NewVHDPath "C:\VMs\dneupaneTestVM$i.vhdx" -NewVHDSizeBytes 20GB
}

### 6. Nested Virtualization
# Enable Nested Virtualization
Set-VMProcessor -VMName "dneupaneNestedVM" -ExposeVirtualizationExtensions $true

### 7. Failover Clustering
# Configure Failover Clustering
New-Cluster -Name "dneupaneCluster" -Node "Node1", "Node2" -StaticAddress "192.168.1.100"
Add-ClusterSharedVolume -Name "dneupaneClusterDisk1"

### 8. Performance Monitoring
# Monitor Performance
Get-Counter -Counter "\Hyper-V Hypervisor Logical Processor(_Total)\% Total Run Time"

### 9. iSCSI Setup
# Configure iSCSI Initiator
New-IscsiTargetPortal -TargetPortalAddress "192.168.1.10"
Connect-IscsiTarget -NodeAddress "iqn.2023-01.com.dneupane:storage" -IsPersistent $true

### 10. P2V Migration
# Convert Physical Machine to Virtual Machine
disk2vhd C: D: E: C:\VMs\dneupaneConvertedMachine.vhdx

### 11. Secure VM Configuration
# Enable Secure Boot
Set-VMFirmware -VMName "dneupaneSecureVM" -EnableSecureBoot On

# Configure Shielded VM
Set-VMSecurity -VMName "dneupaneSecureVM" -Shielded $true
