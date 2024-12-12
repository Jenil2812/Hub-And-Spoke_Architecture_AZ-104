# Hub and Spoke Network Architecture Project

## Overview
This document outlines the steps to create a Hub and Spoke network architecture using Azure. The project involves setting up resource groups, virtual networks (VNets), subnets, virtual machines (VMs), and configuring network security and routing.

##Architecture Of Hub and Spoke 
![image alt](https://github.com/Jenil2812/Hub-And-Spoke_Architecture_AZ-104/blob/e884e0ade876780793b1b9601fe908aabff64d1c/Hub-And-Spoke_Architecture.png)

## Resource Groups
- **Hub-RG**: Located in Canada Central.
- **Spoke1-RG**: Located in Canada Central.
- **Spoke2-RG**: Located in Central India.

## Virtual Networks and Subnets

### Hub-VNet
- **Resource Group**: Hub-RG
- **Location**: Canada Central
- **Address Space**: 10.0.0.0/16
- **Subnets**:
  - **AzureFirewallSubnet**: 10.0.0.0/26
  - **AzureFirewallManagementSubnet**: 10.0.0.64/26

### Spoke1-VNet
- **Resource Group**: Spoke1-RG
- **Location**: Canada Central
- **Address Space**: 192.0.0.0/16
- **Subnets**:
  - **Spoke1-RDP-Subnet**: 192.0.0.0/24
  - **Spoke1-Work-Subnet**: 192.0.1.0/24

### Spoke2-VNet
- **Resource Group**: Spoke2-RG
- **Location**: Central India
- **Address Space**: 172.0.0.0/16
- **Subnets**:
  - **Spoke2-RDP-Subnet**: 172.0.0.0/24
  - **Spoke2-Work-Subnet**: 172.0.1.0/24

## Virtual Machines

### Spoke1-RG
1. **Spoke1-RDP-Vm**
   - **Private IP**: 192.0.0.4
   - **Public IP**: Assigned
   - **OS**: Windows Server 2019
2. **Spoke1-Work-Vm**
   - **Private IP**: 192.0.1.4
   - **OS**: Windows Server 2019

### Spoke2-RG
1. **Spoke2-RDP-Vm**
   - **Private IP**: 172.0.0.4
   - **Public IP**: Assigned
   - **OS**: Windows Server 2019
2. **Spoke2-Work-Vm**
   - **Private IP**: 172.0.1.4
   - **OS**: Windows Server 2019

## Network Peering
- **Hub-to-Spoke1**: Peering between Hub-VNet and Spoke1-VNet.
- **Spoke2-to-Hub**: Peering between Spoke2-VNet and Hub-VNet.

## Route Tables
- **Spoke1-RT**: Route table for Spoke1-VNet with next hop set to the firewall.
- **Spoke2-RT**: Route table for Spoke2-VNet with next hop set to the firewall.

## Firewall Configuration
- **Public IP**: Assigned to the firewall.
- **Private IP**: 10.0.0.4
- **Application Rule Collection**:
  - **Allow**: Access to `www.google.com`
  - **Deny**: All other websites

## Jumpbox Configuration
In both Spoke1-RG and Spoke2-RG, a jumpbox VM is created to provide secure access to internal VMs without exposing them to the public internet.

### Spoke1-RG
- **Spoke1-RDP-Vm**: Acts as a jumpbox with both public and private IP addresses.
  - **Private IP**: 192.0.0.4
  - **Public IP**: Assigned

### Spoke2-RG
- **Spoke2-RDP-Vm**: Acts as a jumpbox with both public and private IP addresses.
  - **Private IP**: 172.0.0.4
  - **Public IP**: Assigned

### Advantages of Using a Jumpbox
- **Enhanced Security**: Reduces the attack surface by limiting public IP exposure to a single VM.
- **Cost-Effective**: More economical than using Azure Bastion, which can be expensive.
- **Controlled Access**: Provides a controlled entry point to access internal VMs, ensuring secure and managed connectivity.

## Additional Notes
- Ensure that all VNets and subnets are correctly associated with their respective route tables.
- Verify that the firewall rules are correctly applied and tested for the desired traffic flow.
- Regularly monitor and update the network security configurations to adhere to best practices.

## Benefits of Hub and Spoke Architecture
1. **Centralized Management**
   - **Simplified Administration**: By centralizing services like firewalls and VPN gateways in the hub, you can manage and monitor network traffic more efficiently.
   - **Cost Efficiency**: Centralizing these services reduces the need for redundant resources in each spoke, lowering overall costs.
2. **Improved Security**
   - **Controlled Access**: The hub can act as a central point for implementing security policies, ensuring consistent security across all spokes.
   - **Isolation**: Spokes can be isolated from each other, reducing the risk of lateral movement in case of a security breach.
3. **Scalability**
   - **Flexible Expansion**: You can easily add new spokes without significant changes to the existing network architecture.
   - **Resource Optimization**: Resources can be allocated and scaled independently in each spoke, optimizing performance and cost.
4. **Enhanced Performance**
   - **Optimized Traffic Flow**: By routing traffic through the hub, you can optimize the flow of data between spokes and external networks.
   - **Reduced Latency**: Localizing traffic within the hub and spoke regions can reduce latency, improving application performance.
5. **Simplified Connectivity**
   - **Easy Peering**: Establishing peering connections between the hub and spokes simplifies network connectivity and management.
   - **Centralized Routing**: Centralized route tables in the hub streamline routing configurations and updates.


