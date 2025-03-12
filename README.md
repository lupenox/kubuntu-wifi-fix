# Kubuntu Wi-Fi Adapter Fix (TP-Link AC600)

This project documents the troubleshooting and resolution process for fixing the TP-Link AC600 (Realtek RTL8811AU) Wi-Fi adapter on a Kubuntu PC.

---

## 🚨 Issue Description

- **Problem:** Wi-Fi adapter was not detecting any networks.
- **Symptoms:**
  - `iwconfig` showed no active wireless extensions.
  - Wi-Fi device listed as disconnected in `nmcli device`.
  - Restarting the Network Manager had no effect.

---

## ⚙️ Fix Steps

### 1. Verify Adapter Presence

```bash
lsusb

    Confirmed the TP-Link AC600 was detected by the system.

2. Check Installed Drivers

dkms status

    Identified that existing drivers were present but not functioning correctly.

3. Remove Faulty Drivers

sudo rm -rf /var/lib/dkms/rtl8812au
sudo rm -rf /var/lib/dkms/rtl88x2bu

4. Install Drivers from Source

git clone https://github.com/aircrack-ng/rtl8812au.git
cd rtl8812au
sudo mv ~/rtl8812au /usr/src/rtl8812au-5.13.6
sudo dkms add -m rtl8812au -v 5.13.6
sudo dkms build -m rtl8812au -v 5.13.6
sudo dkms install -m rtl8812au -v 5.13.6

5. Load the Module

sudo modprobe 88XXau

6. Restart Network Manager

sudo systemctl restart NetworkManager

7. Connect to Wi-Fi

nmcli device wifi connect "YourNetworkName" password "YourPassword"

8. Verify Connection

nmcli device

📚 Lessons Learned

    Kernel Modules: Gained a deeper understanding of kernel module installation and troubleshooting with DKMS.
    Network Diagnostics: Utilized tools like nmcli and iwconfig for diagnosing network issues.
    Version Management: Learned the importance of aligning driver versions with Linux kernel versions.
    Persistence: Ensured the driver loaded correctly after reboots and was persistent across sessions.

## 📸 Screenshot

![SC_Bash_CL](https://github.com/user-attachments/assets/b9747475-98f4-4232-9472-aa00d36d3d1a)

*This screenshot shows the detailed process of identifying, removing, and reinstalling drivers to restore Wi-Fi functionality.*

✅ Result

    Wi-Fi adapter successfully detected and connected.
    Network functionality restored and persisted across reboots.
    Solidified expertise in Linux driver troubleshooting and module installation.

🛠️ License

MIT License

