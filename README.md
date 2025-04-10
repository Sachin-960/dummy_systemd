## 🛠️ Dummy Background Service using Systemd

This project demonstrates how to create a simple background service in Linux using a **Bash script** and **systemd**. The service simulates a dummy application that continuously logs messages every 10 seconds.

---

### 📁 Project Structure

```
dummy-service/
├── dummy.sh             # Bash script that simulates a running service
└── dummy.service        # systemd service file to manage dummy.sh
```

---

## 🔧 1. `dummy.sh` – The Background Script

```bash
#!/bin/bash

while true; do
  echo "Dummy service is running..." >> /var/log/dummy-service.log
  sleep 10
done
```

### 🔍 Explanation:
- **Infinite loop**: Keeps running forever using `while true`.
- **Logging**: Appends a message every 10 seconds to `/var/log/dummy-service.log`.
- **Simulates**: An application or service that performs a background task regularly.

### 📌 Make it executable:
```bash
sudo chmod +x /usr/local/bin/dummy.sh
```

---

## 🔧 2. `dummy.service` – The systemd Service File

```ini
[Unit]
Description=Dummy background service

[Service]
ExecStart=/usr/local/bin/dummy.sh
Restart=always
RestartSec=5
StandardOutput=append:/var/log/dummy-service.log
StandardError=append:/var/log/dummy-service.log

[Install]
WantedBy=multi-user.target
```

### 🔍 Explanation:
| Section       | Key                    | Meaning |
|---------------|------------------------|--------|
| `[Unit]`      | `Description`          | Human-readable name for the service |
| `[Service]`   | `ExecStart`            | Path to the script to run |
|               | `Restart=always`       | Restarts service if it crashes |
|               | `RestartSec=5`         | Waits 5 seconds before restarting |
|               | `StandardOutput`       | Appends normal output to log file |
|               | `StandardError`        | Appends errors to same log file |
| `[Install]`   | `WantedBy=multi-user.target` | Ensures the service starts at boot in multi-user mode |

---

## 🚀 Deployment Steps

1. **Copy the script:**
   ```bash
   sudo cp dummy.sh /usr/local/bin/dummy.sh
   sudo chmod +x /usr/local/bin/dummy.sh
   ```

2. **Copy the service file:**
   ```bash
   sudo cp dummy.service /etc/systemd/system/dummy.service
   ```

3. **Reload systemd & start service:**
   ```bash
   sudo systemctl daemon-reexec
   sudo systemctl daemon-reload
   sudo systemctl start dummy
   sudo systemctl enable dummy
   ```

---

## 🧪 How to Use

### 🎛️ Manage the Service

| Command | Purpose |
|--------|---------|
| `sudo systemctl start dummy`   | Start the service |
| `sudo systemctl stop dummy`    | Stop the service |
| `sudo systemctl restart dummy` | Restart the service |
| `sudo systemctl enable dummy`  | Start on boot |
| `sudo systemctl disable dummy` | Disable autostart |
| `sudo systemctl status dummy`  | Check service status |

---

### 📝 View Logs

```bash
sudo journalctl -u dummy -f
```

Or check the log file directly:
```bash
tail -f /var/log/dummy-service.log
```

---

## 🧼 Clean Up (Optional)

```bash
sudo systemctl stop dummy
sudo systemctl disable dummy
sudo rm /etc/systemd/system/dummy.service
sudo rm /usr/local/bin/dummy.sh
sudo systemctl daemon-reload
```

---

## ✅ Why Use `systemd`?

Systemd allows you to:
- Run background tasks as services
- Restart automatically on failure
- Enable services to run at boot
- Monitor logs easily via `journalctl`

This project is a part of [roadmap.sh](https://roadmap.sh/projects/dummy-systemd-service) Devops Project.
