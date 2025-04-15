# 🧩 WordPress Deployment with Ansible AWX

Welcome to my AWX-based WordPress deployment project!  
This repo uses **Ansible** and **AWX** to set up a full WordPress environment — with one host running **MySQL/MariaDB** (database) and the other running **Apache + PHP + WordPress** (web server).

---

## 📦 What This Project Does

This project:
- Sets up a **database server** with MariaDB/MySQL
- Sets up a **web server** with Apache, PHP, and WordPress
- Configures WordPress to talk to the database across hosts
- Uses **AWX** to run everything automatically from Git

---

## 🌍 Server Setup

This uses **two remote hosts**:

- 🗄️ `db` group — runs MySQL/MariaDB
- 🌐 `web` group — runs Apache, PHP, and WordPress

You define the IPs in the inventory file.

---

## 📁 Repo Structure

```bash
.
├── inventory/                # Host inventory (define IPs & groups)
│   └── hosts.ini
├── group_vars/              # Group variables for db and web
│   ├── db.yml
│   └── web.yml
├── roles/                   # Ansible roles for installing services
│   ├── mysql/               # Installs & configures MariaDB
│   ├── apache/              # Installs Apache web server
│   ├── php/                 # Installs PHP and modules
│   └── wordpress/           # Downloads, configures WordPress
├── playbook.yml             # Main playbook that runs all roles
└── README.md                # This file
```

---

## 🚀 How to Use It with AWX

> Prerequisite: AWX is installed and configured (with Git project support enabled)

### 1. 🔗 Connect This Git Repo to AWX

- In AWX, go to **Projects**
- Create a new project:
  - Name: `wordpress-project`
  - SCM Type: `Git`
  - URL: `https://github.com/saravanakumaran2/awx-git-project.git`

### 2. 🧩 Add an Inventory

- Create a new **Inventory** in AWX
- Define two groups:
  - `db` with your database server IP
  - `web` with your web server IP

Example:
```ini
[db]
35.183.34.226

[web]
3.99.141.39
```

### 3. 🔐 Add Credentials

- SSH private key for remote access
- Optional: Vault password (if you encrypt variables)

### 4. 🧾 Create a Job Template

- Choose the project and inventory you added
- Select `playbook.yml`
- Launch the job!

---

## 📌 Variable Configuration

### group_vars/db.yml

```yaml
mysql_root_password: rootpass
mysql_db: wordpress
mysql_user: wp_user
mysql_password: wp_pass
```

### group_vars/web.yml

```yaml
wordpress_db_host: 35.183.34.226  # IP of DB server
wordpress_db_name: wordpress
wordpress_db_user: wp_user
wordpress_db_password: wp_pass
```

---

## 🛠 What Each Role Does

### 🔹 `roles/mysql/`
- Installs MariaDB
- Creates a database and a user
- Sets `bind-address = 0.0.0.0` for remote access

### 🔹 `roles/apache/`
- Installs Apache2
- Ensures it's running and enabled

### 🔹 `roles/php/`
- Installs PHP and required extensions

### 🔹 `roles/wordpress/`
- Downloads latest WordPress
- Moves it to `/var/www/html`
- Generates `wp-config.php` using Jinja2 template

---

