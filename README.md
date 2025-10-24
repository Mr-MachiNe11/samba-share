# 🗂️ Samba File Sharing Server (AlmaLinux 9)

This project demonstrates how to set up a **secure, multi-user Samba file sharing server** on AlmaLinux 9.  
The goal is to configure a local shared folder accessible by authenticated users from Linux or Windows systems.

---

## 🧩 Project Overview

- **OS:** AlmaLinux 9  
- **Service:** Samba (SMB/CIFS)  
- **Purpose:** Create and manage a secure shared directory `/srv/samba/shared`  
- **Users:** `alice` and `bob` with individual Samba credentials

---

## ⚙️ Step-by-Step Setup

### 🧱 1. Update System and Install Samba
```bash
sudo dnf update -y
sudo dnf install samba samba-client samba-common -y
📁 2. Create the Shared Directory
bash
Copy code
sudo mkdir -p /srv/samba/shared
sudo groupadd sambashare
sudo chown :sambashare /srv/samba/shared
sudo chmod 2770 /srv/samba/shared
👥 3. Create and Configure Samba Users
bash
Copy code
sudo useradd alice
sudo passwd alice
sudo smbpasswd -a alice

sudo useradd bob
sudo passwd bob
sudo smbpasswd -a bob

sudo usermod -aG sambashare alice
sudo usermod -aG sambashare bob
⚙️ 4. Configure Samba
Open the Samba configuration file:

bash
Copy code
sudo nano /etc/samba/smb.conf
Add this section at the end:

ini
Copy code
[shared]
    path = /srv/samba/shared
    valid users = @sambashare
    writable = yes
    browseable = yes
Save and exit, then restart Samba:

bash
Copy code
sudo systemctl restart smb
🧪 5. Test the Share
List available shares:

bash
Copy code
smbclient -L localhost -U alice
Expected output:

pgsql
Copy code
Sharename       Type      Comment
---------       ----      -------
shared          Disk
IPC$            IPC
💻 6. Access from Windows
In File Explorer, go to:

php-template
Copy code
\\<server-ip>\shared
Use the username and password for alice or bob.

🔒 7. Adjust Firewall (if required)
bash
Copy code
sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload
✅ Setup Complete!
You now have a fully working Samba file-sharing server on AlmaLinux 9, ready to use for local or small-office testing.
