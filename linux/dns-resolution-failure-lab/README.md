# DNS Resolution Failure Lab

**Lab Type:** Linux Troubleshooting  
**Difficulty:** Beginner–Intermediate  
**Focus Area:** DNS Resolution / Network Troubleshooting

This lab documents a simulated DNS resolution failure in an Ubuntu VirtualBox environment and shows how the issue was identified, verified, and resolved through structured troubleshooting.

## 🧩 Issue Description
A DNS resolution failure was simulated on a Linux virtual machine by configuring an invalid DNS server. The system was unable to resolve domain names while maintaining network connectivity.

---

## 🖥️ Environment Setup
- Ubuntu Linux (VirtualBox VM)
- Network Adapter: NAT
- Interface: enp0s3

📸 **Figure 1 — DNS Configuration (Initial State)**  
<img width="778" height="468" alt="01 - DNS-Configuration" src="https://github.com/user-attachments/assets/8057b0ba-eb49-4a3a-bc31-60c62e71deb3" />

📸 **Figure 2 — Environment Setup and Network Configuration**  
<img width="804" height="465" alt="02 - Environment-Setup" src="https://github.com/user-attachments/assets/92044688-668d-4d54-95aa-663dabf96f87" />

---

## ⚙️ Fault Injection (Breaking DNS)
An invalid DNS server (`1.2.3.4`) was manually configured to simulate a DNS failure scenario.

📸 **Figure 3 — DNS Misconfiguration Applied**  
<img width="798" height="462" alt="03 - DNS-Break" src="https://github.com/user-attachments/assets/3449fc43-9a58-4786-bcf3-5d9863deba63" />

---

## ⚠️ Symptoms Observed
- Unable to resolve domain names
- Error: `Temporary failure in name resolution`

📸 **Figure 4 — Domain Resolution Failure (google.com)**  
<img width="798" height="470" alt="04 - Domain-Failure" src="https://github.com/user-attachments/assets/dc75146e-376f-4c60-b54e-789f433d404d" />

---

## 🔍 Investigation Steps

### Step 1 — Test Domain Resolution

```
ping google.com
```

❌ Result: Failed (DNS issue)

---

### Step 2 — Test Network Connectivity

```
ping 8.8.8.8
```

✔ Result: Success (Network is working)

### 📸 Figure 5 — Successful ICMP Test
<img width="804" height="465" alt="05 - IP-Success" src="https://github.com/user-attachments/assets/754cc57d-62ed-4e7c-8084-2e5e1017346f" />

👉 Insight: Network works, DNS does not.

---

### Step 3 — Check DNS Configuration

```
resolvectl status
```

### 📸 Figure 6 — DNS Misconfiguration Confirmed

<img width="803" height="460" alt="06 - DNS-Broken-Confirmed" src="https://github.com/user-attachments/assets/e3a6df29-8b65-4ae7-9f9d-08ead27ed536" />

---

DNS resolution failed because the active interface (`enp0s3`) was manually configured with an invalid DNS server address (`1.2.3.4`). This prevented the system from resolving domain names while leaving raw IP connectivity intact.

A second troubleshooting discovery occurred during recovery: after running `sudo resolvectl revert enp0s3`, the invalid DNS setting was removed, but the interface was left with no active DNS scope. This meant DNS resolution still did not work until a valid DNS server (`8.8.8.8`) was manually assigned.

---

## 🛠️ Resolution Steps

### Step 1 — Clear DNS Configuration

```
sudo resolvectl revert enp0s3
```


### 📸 Figure 7 — DNS Configuration Cleared
<img width="801" height="458" alt="07 - DNS-Cleared" src="https://github.com/user-attachments/assets/f3897d8e-2b79-48f0-91bd-ac211bda2570" />

👉 Observation: The invalid DNS value was removed, but the interface no longer had an active DNS scope. This showed that clearing a bad configuration did not automatically restore working DNS.

---

### Step 2 — Apply Valid DNS

Because the interface no longer had an active DNS scope after the revert, a valid DNS server had to be assigned manually.

```
sudo resolvectl dns enp0s3 8.8.8.8
```


---

### Step 3 — Verify Resolution

```
ping google.com
```


### 📸 Figure 8 — DNS Resolution Restored
<img width="800" height="456" alt="08 - DNS-Restored" src="https://github.com/user-attachments/assets/3b76a137-36c0-4c12-bbf2-093e82085923" />

✔ Result: DNS working again

---

## 📘 Lessons Learned
- DNS failure does not necessarily mean full network failure
- Testing both domain names and raw IP addresses helps isolate DNS issues quickly
- `resolvectl` is useful for checking and modifying interface-level DNS settings in Linux
- Removing a bad configuration does not always restore a working state automatically
- Every attempted fix should be verified before assuming the problem is resolved

---

## 🔧 Commands Used

```bash
resolvectl status
resolvectl dns enp0s3 1.2.3.4
ping google.com
ping 8.8.8.8
sudo resolvectl revert enp0s3
sudo resolvectl dns enp0s3 8.8.8.8
```

## 🧱 Key Skills Demonstrated
- Network troubleshooting  
- DNS analysis  
- Root cause identification  
- Linux command-line diagnostics  
- System recovery

  ---

## ✅ Conclusion
This lab demonstrated how to isolate DNS failure from general network failure, validate findings with command-line testing, and recover the system by manually restoring a valid DNS server. It also showed that troubleshooting often uncovers secondary issues during recovery, making verification after each change essential.

