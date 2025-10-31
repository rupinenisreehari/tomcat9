---

````markdown
# 🚀 Apache Tomcat 9 Installation Guide on Ubuntu (with systemd)

This guide explains how to install **Apache Tomcat 9** on **Ubuntu** with **OpenJDK 17**, configure it as a **systemd service**, and manage it easily using `systemctl`.

---

## 🧩 Step 1: Create a Dedicated Tomcat User

Create a system user and group named **tomcat** without a login shell for security purposes.

```bash
sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat
````

* `-m`: Creates the home directory `/opt/tomcat`
* `-U`: Creates a group with the same name as the user
* `-d`: Sets the home directory
* `-s /bin/false`: Prevents login access for the Tomcat user

---

## ☕ Step 2: Install Java (OpenJDK 17)

Tomcat 9 requires Java to run. Install **OpenJDK 17**:

```bash
sudo apt install openjdk-17-jdk -y
```

---

## 📦 Step 3: Download and Extract Apache Tomcat 9

Move to the `/opt` directory and download Tomcat 9.0.91 from the Apache archive:

```bash
cd /opt
curl -O -L https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz
```

Extract the downloaded tarball:

```bash
sudo tar xzf apache-tomcat-9.0.91.tar.gz
```

Create a symbolic link for easier version management:

```bash
sudo ln -s apache-tomcat-9.0.91 tomcat9
```

Set proper ownership:

```bash
sudo chown -R tomcat:tomcat /opt/apache-tomcat-9.0.91
sudo chown -h tomcat:tomcat /opt/tomcat9
```

---

## ⚙️ Step 4: Create a systemd Service File

Create and edit the systemd service configuration file:

```bash
sudo vim /etc/systemd/system/tomcat9.service
```

Add the following content:

```ini
[Unit]
Description=Apache Tomcat 9 Servlet Container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64"
Environment="CATALINA_HOME=/opt/tomcat9"
Environment="CATALINA_BASE=/opt/tomcat9"
Environment="CATALINA_PID=/opt/tomcat9/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"

ExecStart=/opt/tomcat9/bin/startup.sh
ExecStop=/opt/tomcat9/bin/shutdown.sh

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

---

## 🔁 Step 5: Enable and Start Tomcat Service

Reload systemd to recognize the new service:

```bash
sudo systemctl daemon-reload
```

Enable Tomcat to start on boot:

```bash
sudo systemctl enable tomcat9
```

Start the Tomcat service:

```bash
sudo systemctl start tomcat9
```

---

## ✅ Step 6: Verify Tomcat Status

Check if Tomcat is running properly:

```bash
sudo systemctl status tomcat9
```

You should see an **“active (running)”** status.

---

## 🌐 Step 7: Access Tomcat Web Interface

Open your web browser and go to:

```
http://<your-server-ip>:8080
```

You should see the **Apache Tomcat Welcome Page** 🎉

---

## 🧰 Useful Commands

| Action         | Command                          |
| -------------- | -------------------------------- |
| Start Tomcat   | `sudo systemctl start tomcat9`   |
| Stop Tomcat    | `sudo systemctl stop tomcat9`    |
| Restart Tomcat | `sudo systemctl restart tomcat9` |
| Check Status   | `sudo systemctl status tomcat9`  |
| View Logs      | `sudo journalctl -u tomcat9`     |

---

### 📝 Notes

* Configuration files are located in: `/opt/tomcat9/conf`
* Web applications are deployed in: `/opt/tomcat9/webapps`
* Logs are stored in: `/opt/tomcat9/logs`

---

**Author:** RUPINENI SREEHARI
**Version:** 1.0
**Last Updated:** October 2025

---


