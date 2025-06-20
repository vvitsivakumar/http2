
# Enabling HTTP/2 on Apache (SSL)

## Step 1: Modify Virtual Host

Add the following to your SSL virtual host configuration file:

```apache
<VirtualHost *:443>
    ServerName test.example.org
    Protocols h2 h2c http/1.1

    # Other SSL settings...
    SSLEngine on
    SSLCertificateFile /path/to/ssl.crt
    SSLCertificateKeyFile /path/to/ssl.key
</VirtualHost>
```

## Step 2: Enable HTTP/2 Apache Module

Run the following command to enable the `http2` module:

```bash
sudo a2enmod http2
```

## Step 3: Check if HTTP/2 Module is Enabled

```bash
apachectl -M | grep http2
```

## Step 4: Check the Apache MPM Mode

```bash
apachectl -V | grep -i mpm
```

If the output shows:

```
Server MPM:     prefork
```

You must switch to the `event` MPM. Proceed to the next step.

## Step 5: Switch from `prefork` to `event` MPM

Run the following commands:

```bash
sudo a2dismod php8.2
sudo a2dismod mpm_prefork
sudo a2enmod mpm_event
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php8.2-fpm
```

Then, restart Apache:

```bash
sudo systemctl restart apache2
```

## Step 6: Verify HTTP/2 is Working

Use `curl` to check:

```bash
curl -I --http2 -k https://your-domain
```

If successful, the output should include:

```
HTTP/2 200
```

---
✅ You have now successfully enabled **HTTP/2** on Apache!
