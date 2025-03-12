# Kubuntu Wi-Fi Adapter Fix (TP-Link AC600)

This project documents the troubleshooting and resolution process for fixing the TP-Link AC600 (Realtek RTL8811AU) Wi-Fi adapter on a Kubuntu PC.

---

## 🔄 Issue Description
- **Problem:** Wi-Fi adapter not detecting any networks.
- **Symptoms:**
  - `iwconfig` showed no active wireless extensions.
  - Wi-Fi device listed as disconnected in `nmcli device`.
  - Network Manager restart had no effect.

---

## ⚙️ Fix Steps

### 1. **Verify Adapter Presence**
```bash
lsusb
```
- Confirmed the TP-Link AC600 was detected by the system.

### 2. **Check Installed Drivers**
```bash
dkms status
```
- Identified existing drivers but found module issues.

### 3. **Remove Faulty Drivers**
```bash
sudo rm -rf /var/lib/dkms/rtl8812au
sudo rm -rf /var/lib/dkms/rtl88x2bu
```

### 4. **Install Drivers from Source**
```bash
git clone https://github.com/aircrack-ng/rtl8812au.git
cd rtl8812au
sudo mv ~/rtl8812au /usr/src/rtl8812au-5.13.6
sudo dkms add -m rtl8812au -v 5.13.6
sudo dkms build -m rtl8812au -v 5.13.6
sudo dkms install -m rtl8812au -v 5.13.6
```

### 5. **Load the Module**
```bash
sudo modprobe 88XXau
```

### 6. **Restart Network Manager**
```bash
sudo systemctl restart NetworkManager
```

### 7. **Connect to Wi-Fi**
```bash
nmcli device wifi connect "YourNetworkName" password "YourPassword"
```

### 8. **Verify Connection**
```bash
nmcli device
```

---

## 🛠️ Lessons Learned
- Understanding kernel modules and troubleshooting DKMS.
- Using `nmcli` and `iwconfig` for deeper network diagnostics.
- Importance of modular builds and dependency management on Linux.

---

## 📸 Screenshots
1. Driver installation process.
2. Wi-Fi adapter detected via `lsusb`.
3. Successful network connection using `nmcli`.

---

## ✅ Result
- Wi-Fi adapter successfully detected and connected.
- Network functionality restored after comprehensive troubleshooting.

---

## 📜 License
MIT License

---

This documentation showcases my problem-solving skills, Linux expertise, and determination in resolving complex system-level issues.
