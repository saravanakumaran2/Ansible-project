# My AWX + Ansible WordPress Setup (Simple Notes Style)

This repo is part of my project where I used **Ansible and AWX** to fully automate a WordPress setup using two different servers — one for the **database** and another for the **web server**.

---

## What I Did in This Project

- Created a full **Ansible playbook** that sets up:
  - `MariaDB` (MySQL) on the DB host
  - `Apache`, `PHP`, and `WordPress` on the web host

- Set up proper **roles** for:
  - `mysql`: to install MariaDB and set the database
  - `apache`: to install Apache server
  - `php`: to install PHP and needed extensions
  - `wordpress`: to download and configure WordPress

- Organized inventory and group variables:
  - Group `db` = DB host (MariaDB)
  - Group `web` = Web host (Apache + WordPress)
  - `group_vars/db.yml` contains MySQL info
  - `group_vars/web.yml` connects WordPress to DB

- Connected the Git repo to **AWX**:
  - So that all playbooks run directly from this GitHub repo


## Manual Step You Still Need To Do

After the playbook runs, **you still need to manually log in to the DB server** and do the following once:

```sql
CREATE USER 'wp_user'@'%' IDENTIFIED BY 'wp_pass';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wp_user'@'%';
FLUSH PRIVILEGES;
```
- This gives WordPress access to your database.  
- Without this step, you’ll see “Error establishing a database connection” in the browser.


##  How to Run It in AWX

1. Connect this Git repo in AWX (as a Project)
2. Set up an Inventory with `db` and `web` groups
3. Add your SSH Credentials
4. Launch a Job Template using `playbook.yml`
5. Then manually run the SQL commands above 👆

## About This Repo

This is a personal and learning project for using:
- AWX (Ansible Tower)
- GitOps-style automation
- Two-tier WordPress deployment

