# Odoo_Installation_Guide_on_Ubuntu_24
Odoo_Installation_Guide_on_Ubuntu_24

Certainly! Below is a professional documentation format for installing Odoo on Ubuntu 24, which you can present on GitHub. This documentation includes setup steps, installation, and configuration instructions in a structured and clear manner.

---

# Odoo Installation Guide on Ubuntu 24

This document provides a step-by-step guide to install and configure Odoo on Ubuntu 24. The instructions include setting up the system dependencies, installing PostgreSQL, configuring the Odoo service, and setting up a virtual environment for Python dependencies.

## Prerequisites

Before proceeding with the installation, ensure that you have:

- Ubuntu 24 or a compatible version installed.
- A user with `sudo` privileges to execute commands.
- A stable internet connection for downloading packages.

---

## 1. Update the System

Update the system to ensure all packages are up-to-date:

```bash
sudo apt-get update
sudo apt-get upgrade
```

---

## 2. Install System Dependencies

Install the required system packages for Odoo:

```bash
sudo apt install python3-minimal python3-dev python3-pip python3-venv python3-setuptools build-essential libzip-dev libxslt1-dev libldap2-dev python3-wheel libsasl2-dev node-less libjpeg-dev xfonts-utils libpq-dev libffi-dev fontconfig git wget
```

---

## 3. Install PostgreSQL Database

Install PostgreSQL, which is required for Odoo's database backend:

```bash
sudo apt-get install postgresql
```

Start and enable the PostgreSQL service:

```bash
systemctl start postgresql
systemctl enable postgresql
```

Check the status of PostgreSQL:

```bash
systemctl status postgresql
```

Create a PostgreSQL user for Odoo:

```bash
su - postgres -c "createuser -s odoo"
```

---

## 4. Install Node.js and npm

Install Node.js and npm to handle JavaScript dependencies:

```bash
apt install nodejs npm
npm install -g rtlcss
```

Install additional fonts for Odoo:

```bash
apt-get install xfonts-75dpi xfonts-base
```

---

## 5. Install wkhtmltopdf

Odoo requires `wkhtmltopdf` to generate PDF reports. Download and install the `wkhtmltopdf` package:

```bash
wget http://security.ubuntu.com/ubuntu/pool/universe/w/wkhtmltopdf/wkhtmltopdf_0.12.6-2build2_amd64.deb
dpkg -i wkhtmltopdf_0.12.6-2build2_amd64.deb
apt-get install -f
dpkg -i wkhtmltopdf_0.12.6-2build2_amd64.deb
```

Verify the installation:

```bash
wkhtmltopdf --version
```

---

## 6. Create Odoo User

Create a system user for Odoo:

```bash
adduser --system --group --home=/opt/odoo --shell=/bin/bash odoo
```

Switch to the Odoo user:

```bash
su - odoo
```

---

## 7. Install Odoo

Clone the Odoo repository from GitHub:

```bash
git clone https://www.github.com/odoo/odoo --depth 1 --branch 18.0 /opt/odoo/odoo
```

Create a virtual environment for Python:

```bash
python3 -m venv odoo-env
```

Activate the virtual environment:

```bash
source odoo-env/bin/activate
```

Install required Python dependencies:

```bash
pip3 install wheel
pip3 install -r /opt/odoo/odoo/requirements.txt
```

Deactivate the virtual environment after installation:

```bash
clear
deactivate
```

---

## 8. Configure Custom Addons

Create a directory for custom addons:

```bash
mkdir /opt/odoo/custom-addons
exit
```

---

## 9. Set Up Log Directory

Create and set proper ownership for Odoo log files:

```bash
mkdir /var/log/odoo18
chown odoo:odoo /var/log/odoo18
```

---

## 10. Configure Odoo

Create and configure the Odoo configuration file:

```bash
nano /etc/odoo.conf
```

Add the following configuration:

```
[options]
admin_passwd = P@ssw0rd
db_host = False
db_port = False
db_user = odoo
db_password = False
logfile = /var/log/odoo18/odoo-server.log
addons_path = /opt/odoo/odoo/addons,/opt/odoo/custom-addons
xmlrpc_port = 8069
```

---

## 11. Create Odoo Systemd Service

Create a systemd service to manage Odoo:

```bash
nano /etc/systemd/system/odoo.service
```

Add the following content:

```
[Unit]
Description=Odoo
Requires=postgresql.service
After=network.target postgresql.service

[Service]
Type=simple
SyslogIdentifier=odoo
PermissionsStartOnly=true
User=odoo
Group=odoo
ExecStart=/opt/odoo/odoo-env/bin/python3 /opt/odoo/odoo/odoo-bin -c /etc/odoo.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target
```

---

## 12. Start and Enable Odoo Service

Reload the systemd daemon, start Odoo, and enable it to start on boot:

```bash
systemctl daemon-reload
systemctl start odoo
systemctl enable odoo
```

Check the status of the Odoo service:

```bash
systemctl status odoo
```

---

## 13. Access Odoo

Once the Odoo service is running, you can access the Odoo web interface by navigating to the following URL in your web browser:

```
http://<your-server-ip>:8069
OR
http://localhost:8069/
```

Use the admin password (`P@ssw0rd`) for initial setup.

---

## Conclusion

You have successfully installed and configured Odoo on Ubuntu 24. If you encounter any issues during the installation, check the Odoo logs located in `/var/log/odoo18/odoo-server.log` for troubleshooting.

For further customization, you can refer to the official [Odoo documentation](https://www.odoo.com/documentation) for more information.

---


