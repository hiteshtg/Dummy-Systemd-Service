
# 📦 Dummy‑Systemd‑Service

A minimal example of a custom **systemd** service that runs a Bash script in the background, writing periodic status messages to a log file—perfect for mastering service creation, management, and monitoring.

---

## 🧰 Project Structure

```
Dummy‑Systemd‑Service/
├── dummy.sh          # Background script (infinite loop logging to file)
└── dummy.service     # systemd unit configuration
```

---

## 📝 dummy.sh

A simple Bash script that indefinitely logs a heartbeat message every 10 seconds:

```bash
#!/bin/bash
while true; do
  echo "Dummy service is running..." >> /var/log/dummy-service.log
  sleep 10
done
```

- Ensures `/var/log/` is writable by the service user.
- Modify or replace with any script/command you want to run as a service.

---

## ⚙️ dummy.service Configuration

```ini
[Unit]
Description=Dummy Service
After=network.target

[Service]
ExecStart=/usr/local/bin/dummy.sh
Restart=always
User=nobody
StandardOutput=append:/var/log/dummy-service.log
StandardError=inherit

[Install]
WantedBy=multi-user.target
```

- **ExecStart**: full path to the script.
- **Restart=always**: restarts on failure.
- **User**: can be configured; adjust file permissions if not root.
- **Logging**: logs to file (and optionally journal).

---

## 🔧 Installation

1. Clone the repo:
   ```bash
   git clone https://github.com/hiteshtg/Dummy-Systemd-Service.git
   cd Dummy-Systemd-Service
   ```

2. Copy the script and service to system locations:
   ```bash
   sudo cp dummy.sh /usr/local/bin/
   sudo chmod +x /usr/local/bin/dummy.sh

   sudo cp dummy.service /etc/systemd/system/
   ```

3. Reload systemd to detect new service:
   ```bash
   sudo systemctl daemon-reload
   ```

4. Enable and start:
   ```bash
   sudo systemctl enable dummy.service
   sudo systemctl start dummy.service
   ```

---

## 🛠️ Usage

| Command | Description |
|--------|-------------|
| `sudo systemctl start dummy.service` | Starts the service |
| `sudo systemctl stop dummy.service` | Stops the service |
| `sudo systemctl enable dummy.service` | Enables on boot |
| `sudo systemctl disable dummy.service` | Disables auto-start |
| `sudo systemctl status dummy.service` | Checks service status |
| `sudo journalctl -u dummy.service -f` | Follows live journal logs |
| `tail -f /var/log/dummy-service.log` | Real-time log file output |

---

## ✅ Key Learnings

By building and managing this demo service, you’ll:

- Understand **systemd unit file structure**.
- Learn how to **install, enable, and run custom services**.
- Master **logging and monitoring** via systemd journal and log files.
- Practice **auto-restarting services** and handling failure.
- Manage service privileges via the **User** directive.

---

## 🛡️ Customize & Extend

- Change **`User=`** to run as `root` or a specific user—ensure proper directory permissions.
- Redirect logs to journal instead of file by using `StandardOutput=journal`.
- Swap out the dummy loop for any continuous task or real application.
- Add environment variables, dependencies, or timer units to schedule runs.

---

## 📚 References

- Inspired by the **roadmap.sh Dummy Systemd Service project**, aimed at teaching systemd basics.

---

## 🧪 Conclusion

This project serves as a solid introduction to Linux systemd services. Once confident, you can expand it with timers, socket activation, service dependencies, and more—turning this into a robust production‑grade template.

Happy system‑servicing!
