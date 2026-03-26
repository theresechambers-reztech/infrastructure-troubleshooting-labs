# 🧪 Linux Network Interface Failure Lab

## 📌 Summary

Simulated a complete network failure by disabling a Linux network interface.  
Used systematic troubleshooting to isolate the issue from DNS and restore connectivity.


## 🎯 Objective
Simulate and troubleshoot a network interface failure in a Linux environment to understand how complete network loss differs from DNS-related issues.

---

## 🖥️ Environment
- VirtualBox
- Ubuntu Linux VM
- Network Interface: enp0s3

---

## ⚠️ Issue Description
A network interface failure was simulated by disabling the active network interface.  
This caused a complete loss of network connectivity.

This scenario represents real-world issues such as:
- Network adapter being disabled
- Cable unplugged
- Interface misconfiguration

---

## 🔍 Symptoms Observed
- No internet connectivity
- Ping to external IP (8.8.8.8) fails
- Interface shows `state DOWN`

---

## 🧪 Baseline Verification (Working State)

```bash
ip a
ping 8.8.8.8
```

### 📸 Figure 1 — Interface (UP State)
<img width="797" height="466" alt="01-interface-working" src="https://github.com/user-attachments/assets/628a3273-d3e3-4d20-a2cf-d1f5167ed822" />


### 📸 Figure 2 — Network Connectivity (Working)
<img width="799" height="461" alt="02-network-working" src="https://github.com/user-attachments/assets/d012667c-ab81-4975-9fa0-53864baa0a1e" />


## 💥 Fault Injection — Disable Interface

```bash
sudo ip link set enp0s3 down
```

### 📸 Figure 3 — Interface Down
<img width="794" height="460" alt="03-interface-down" src="https://github.com/user-attachments/assets/d6c25b52-63a0-4b7f-a339-3d328722ef12" />



## ❌ Connectivity Test After Failure

```bash
ping 8.8.8.8
```

Result:

Network is unreachable

### 📸 Figure 4 — Network Failure
<img width="796" height="458" alt="04-network-failed" src="https://github.com/user-attachments/assets/840b205e-e6e7-425a-bd39-9f85cbc8ceaf" />



## 🛠️ Resolution — Restore Interface

```bash
sudo ip link set enp0s3 up
```

### 📸 Figure 5 — Network Restored
<img width="795" height="454" alt="05-network-restored" src="https://github.com/user-attachments/assets/0ab93ef0-5369-447e-b55b-dfed97ebb459" />



## 🧠 Key Learning

This lab demonstrates that when the network interface is down, all connectivity is lost — both domain and IP-based communication fail.

---

## 🔄 DNS vs Interface Failure Comparison

| Test              | DNS Issue        | Interface Issue   |
|------------------|----------------|------------------|
| ping 8.8.8.8     | ✅ Works        | ❌ Fails          |
| ping google.com  | ❌ Fails        | ❌ Fails          |
| Interface State  | UP             | DOWN             |
| Root Cause Layer | DNS (Layer 7)  | Network (Layer 2/3) |
---

## 🧭 OSI Layer Analysis

- Layer 3 (Network): IP communication failed
- Layer 7 (Application): DNS resolution failed as a result
- Root cause: Layer 2/3 boundary (network interface down)

Conclusion: Lower-layer failure caused higher-layer symptoms

## 💡 Key Insight

When both DNS-based and direct IP connectivity fail, the issue is not name resolution but a lower-layer problem — typically the network interface, routing, or physical connectivity.

This reinforces the importance of isolating failures across OSI layers during troubleshooting.

## 🧱 Skills Demonstrated

- Linux network troubleshooting
- Interface state analysis (ip a)
- Connectivity validation (ICMP / ping)
- Failure isolation (DNS vs Network Layer)
- Root cause identification
- Structured troubleshooting methodology

---

## 🧾 Commands Used

```bash
# View network interfaces and their state
ip a

# Test connectivity to external IP (Google DNS)
ping 8.8.8.8

# Disable the network interface (simulate failure)
sudo ip link set enp0s3 down

# Verify interface is down
ip a

# Test connectivity after failure (should fail)
ping 8.8.8.8

# Restore the network interface
sudo ip link set enp0s3 up

# Verify interface is back up
ip a

# Test connectivity after restoration (should work)
ping 8.8.8.8
```

## 🚀 Outcome

Successfully simulated and resolved a complete network failure by identifying and restoring a disabled network interface.

## 🧠 Troubleshooting Approach

1. Verified baseline connectivity (IP + ping)
2. Introduced failure by disabling the interface
3. Observed total connectivity loss (IP + domain)
4. Tested against known DNS vs network patterns
5. Identified issue at Layer 2/3 (interface level)
6. Restored interface and confirmed recovery

---

## 🌍 Real-World Relevance

This scenario mirrors real-world incidents such as:
- Misconfigured or disabled network adapters
- Virtual machine interface failures
- Cloud instance network misconfiguration

Understanding these patterns helps quickly isolate whether issues originate from DNS or underlying network infrastructure.


