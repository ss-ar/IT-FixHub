# phpMyAdmin Setup on Arch Linux with Apache & PHP

> A professional guide to installing and configuring phpMyAdmin on Arch Linux using Apache and PHP, including full configuration storage setup.

---

## 🧰 Step 1: Download and Extract phpMyAdmin

1. Visit [https://www.phpmyadmin.net](https://www.phpmyadmin.net) and download the latest `.zip` archive.
2. Move and extract the downloaded archive to your Apache web root:

```bash
cd /srv/http
sudo unzip ~/Downloads/phpMyAdmin-*-all-languages.zip
sudo mv phpMyAdmin-*-all-languages phpmyadmin
```

phpMyAdmin will now be available at `http://localhost/phpmyadmin`.

---

## ⚙️ Step 2: Configure PHP

Edit your PHP configuration file to enable required extensions:

```bash
sudo vim /etc/php/php.ini
```

Uncomment or add the following lines:

```ini
extension=mysqli
extension=pdo_mysql
extension=mbstring
```

**Tip:** Use Vim visual block mode to uncomment multiple lines:

* `Ctrl+V` → Arrow keys to select block → `Shift+I` → Add `#` or `;` → `Esc Esc`

---

## 🔐 Step 3: Configure `config.inc.php`

1. Copy the sample config file:

```bash
cd /srv/http/phpmyadmin
sudo cp config.sample.inc.php config.inc.php
sudo nano config.inc.php
```

2. Add a Blowfish secret for cookie authentication:

```php
$cfg['blowfish_secret'] = 'gFh#7kd!28Djl9sN38hf8sKd930dLsPQ';
```

Generate a secure secret:

```bash
openssl rand -base64 32
```

3. Basic server configuration:

```php
$i = 0;
$i++;
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['host'] = 'localhost';
$cfg['Servers'][$i]['AllowNoPassword'] = false;
```

---

## 🛠️ Step 4: Set Up phpMyAdmin Configuration Storage

This enables advanced features like bookmarks, saved queries, and the designer.

1. Create the phpMyAdmin database:

```bash
mysql -u root -p
CREATE DATABASE phpmyadmin;
```

2. Import the phpMyAdmin table structure:

```bash
mysql -u root -p phpmyadmin < /srv/http/phpmyadmin/sql/create_tables.sql
```

---

## 👤 Step 5: Create Control User

Create the internal user that phpMyAdmin uses for managing configuration storage:

```sql
CREATE USER 'pma'@'localhost' IDENTIFIED BY 'YourStrongPassword';
GRANT SELECT, INSERT, UPDATE, DELETE ON phpmyadmin.* TO 'pma'@'localhost';
FLUSH PRIVILEGES;
```

---

## 📊 Step 6: Enable Advanced Configuration in `config.inc.php`

Add the following to configure storage tables:

```php
$cfg['Servers'][$i]['controluser'] = 'pma';
$cfg['Servers'][$i]['controlpass'] = 'YourStrongPassword';
$cfg['Servers'][$i]['pmadb'] = 'phpmyadmin';

$cfg['Servers'][$i]['bookmarktable'] = 'pma__bookmark';
$cfg['Servers'][$i]['relation'] = 'pma__relation';
$cfg['Servers'][$i]['table_info'] = 'pma__table_info';
$cfg['Servers'][$i]['table_coords'] = 'pma__table_coords';
$cfg['Servers'][$i]['pdf_pages'] = 'pma__pdf_pages';
$cfg['Servers'][$i]['column_info'] = 'pma__column_info';
$cfg['Servers'][$i]['history'] = 'pma__history';
$cfg['Servers'][$i]['table_uiprefs'] = 'pma__table_uiprefs';
$cfg['Servers'][$i]['tracking'] = 'pma__tracking';
$cfg['Servers'][$i]['userconfig'] = 'pma__userconfig';
$cfg['Servers'][$i]['recent'] = 'pma__recent';
$cfg['Servers'][$i]['favorite'] = 'pma__favorite';
$cfg['Servers'][$i]['users'] = 'pma__users';
$cfg['Servers'][$i]['usergroups'] = 'pma__usergroups';
$cfg['Servers'][$i]['navigationhiding'] = 'pma__navigationhiding';
$cfg['Servers'][$i]['savedsearches'] = 'pma__savedsearches';
$cfg['Servers'][$i]['central_columns'] = 'pma__central_columns';
$cfg['Servers'][$i]['designer_settings'] = 'pma__designer_settings';
$cfg['Servers'][$i]['export_templates'] = 'pma__export_templates';
```

---

## 🌐 Step 7: Restart Apache

```bash
sudo systemctl restart httpd
```

---

## 🔧 Step 8: Verify Setup

Open your browser and go to:

```
http://localhost/phpmyadmin
```

Confirm:

* No cookie encryption or config errors
* Advanced features like bookmarks and designer work

---

## 🔒 Optional Security Enhancements

* Rename the phpMyAdmin directory:

  ```bash
  sudo mv /srv/http/phpmyadmin /srv/http/mydbpanel
  ```
* Set up HTTPS using self-signed or Let's Encrypt certificates
* Protect the panel with Apache `.htpasswd` authentication

---

**You're now running a fully configured phpMyAdmin instance on Arch Linux!**

