# Packet Tracer - Implement VLANs and Trunking

## 📌 Project Overview

This repository contains the configuration guidelines and project files for setting up VLANs and trunking on Cisco Catalyst 2960 switches in a lab environment simulating a branch office deployment.

## 📋 Objectives

* **Part 1:** Configure VLANs

* **Part 2:** Assign Ports to VLANs

* **Part 3:** Configure Static Trunking

* **Part 4:** Configure Dynamic Trunking

## 📊 Reference Tables

### 1. VLAN Table

| VLAN Number | VLAN Name | Purpose / Notes | 
| ----- | ----- | ----- | 
| **10** | `Admin` | Administration subnet | 
| **20** | `Accounts` | Accounting subnet | 
| **30** | `HR` | Human Resources subnet | 
| **40** | `Voice` | IP Telephony subnet | 
| **99** | `Management` | Switch Virtual Interface (SVI) Management | 
| **100** | `Native` | Native VLAN for Trunk links | 

> **Note:** VLAN names are case-sensitive and must match the table exactly.

### 2. Addressing & Port Assignment Table

| Device | Interface | IP Address | Subnet Mask | Switchport | Assigned VLAN | 
| ----- | ----- | ----- | ----- | ----- | ----- | 
| **PC1** | NIC | `192.168.10.10` | `255.255.255.0` | SWB F0/1 | VLAN 10 | 
| **PC2** | NIC | `192.168.20.20` | `255.255.255.0` | SWB F0/2 | VLAN 20 | 
| **PC3** | NIC | `192.168.30.30` | `255.255.255.0` | SWB F0/3 | VLAN 30 | 
| **PC4** | NIC | `192.168.10.11` | `255.255.255.0` | SWC F0/1 | VLAN 10 | 
| **PC5** | NIC | `192.168.20.21` | `255.255.255.0` | SWC F0/2 | VLAN 20 | 
| **PC6** | NIC | `192.168.30.31` | `255.255.255.0` | SWC F0/3 | VLAN 30 | 
| **PC7** | NIC | `192.168.10.12` | `255.255.255.0` | SWC F0/4 | VLAN 10 / VLAN 40 (Voice) | 
| **SWA** | SVI | `192.168.99.252` | `255.255.255.0` | N/A | VLAN 99 | 
| **SWB** | SVI | `192.168.99.253` | `255.255.255.0` | N/A | VLAN 99 | 
| **SWC** | SVI | `192.168.99.254` | `255.255.255.0` | N/A | VLAN 99 | 

## 📝 Configuration Steps & Instructions

### Part 1: Configure VLANs

Configure all required VLANs (`10`, `20`, `30`, `40`, `99`, `100`) on switches **SWA**, **SWB**, and **SWC**.

### Part 2: Assign Ports to VLANs

1. **Assign Access Ports:**

   * Map access ports on **SWB** and **SWC** according to the Addressing Table.

2. **Configure Voice VLAN Port:**

   * On **SWC Port F0/4**, configure access mode for data on VLAN 10 and voice traffic on VLAN 40 (`Voice`).

3. **Configure Virtual Management Interfaces (SVI):**

   * Create interface `VLAN 99` on all three switches (**SWA**, **SWB**, **SWC**).

   * Assign IP addresses according to the Addressing Table and activate the interface (`no shutdown`).

   * *Note:* Until trunk links are properly configured with matching native/allowed settings, cross-switch ping reachability should remain restricted.

### Part 3: Configure Static Trunking

1. **Static Trunk Link (SWA <-> SWB):**

   * Configure the link between **SWA** and **SWB** as a static trunk (`switchport mode trunk`).

   * Disable Dynamic Trunking Protocol (DTP) on both ends using `switchport nonegotiate`.

2. **Native VLAN Alignment:**

   * Change the default native VLAN from VLAN 1 to **VLAN 100** on both switch ports to prevent native VLAN misconfigurations.

### Part 4: Configure Dynamic Trunking

1. **Dynamic Trunk Negotiation (SWA <-> SWC):**

   * Assuming **SWC** is using default Cisco 2960 DTP settings (`dynamic auto`), set port **G0/2** on **SWA** to `dynamic desirable` to allow successful trunk negotiation.

2. **Native VLAN Alignment:**

   * Configure **VLAN 100** as the native VLAN across the trunk between **SWA** and **SWC** to ensure end-to-end consistency and clear native VLAN mismatch errors.

## 🧪 Verification Commands

```
# Verify VLAN database configuration
show vlan brief

# Verify interface switchport status and mode
show interfaces switchport

# Verify active trunk ports and native VLAN settings
show interfaces trunk

```