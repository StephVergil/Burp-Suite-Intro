# Burp Suite Intro

[Burp Suite Project](https://github.com/StephVergil/Burp-Suite-Intro/blob/main/Homework%2010.docx)

## Overview

This project provides a practical introduction to **Burp Suite**, a leading tool for web application security testing. By following this guide, you will learn to configure Burp Suite, set up a proxy with Firefox ESR, and exploit vulnerabilities in the **OWASP Juice Shop**. The lab demonstrates real-world exploitation techniques such as SQL injection and parameter tampering while emphasizing secure coding practices to prevent these vulnerabilities.

---

## Step 1: Configure Burp Suite

### Verify Existing Proxies
1. Open a terminal and check for existing proxy configurations:
   ```bash
   env | grep -i proxy
   ```
2. Note the output and close the terminal.

### Launch Burp Suite
1. Start Burp Suite by double-clicking its icon or finding it in your start menu.
2. Accept the user agreement and select "Temporary project" with default settings.
3. Click **Start Burp**.
4. Skip updating Kali software if prompted.
5. The main Burp Dashboard should now be visible.

### Configure Upstream Proxy
1. Navigate to **User options > Upstream Proxy Servers > Add**:
   - Destination Host: `*`
   - Proxy Host: `proxy`
   - Proxy Port: `8080`
   - Authentication Type: `None`
   - Click **OK**.
2. Go to **Proxy > Options** and configure:
   - Bind to Port: `8088`
   - Bind to Address: `127.0.0.1`
3. Navigate to **Proxy > Intercept** and set it to "Intercept is off."
4. Open **Proxy > HTTP history** to monitor captured requests.

---

## Step 2: Set Up Firefox ESR

### Update Firefox ESR
1. Open a terminal and run:
   ```bash
   sudo apt update
   sudo apt install firefox-esr
   ```
2. Follow prompts and type `Y` to continue.
3. If errors occur, run the following commands:
   ```bash
   sudo apt --fix-broken install
   sudo dpkg --configure -a
   sudo apt install firefox-esr
   ```

### Configure Proxy Settings
1. Open Firefox ESR and navigate to **Settings > Network Settings**.
2. Select "Manual proxy configuration" and set:
   - HTTP Proxy: `127.0.0.1`
   - Port: `8088`

### Install Burp’s CA Certificate
1. Visit `http://burp` in Firefox ESR.
2. Download the CA Certificate.
3. Import the certificate via **Settings > Privacy & Security > Certificates**.
4. Check both trust boxes and confirm.

---

## Step 3: Set Up Juice Shop Account

1. Open the Juice Shop application in Firefox ESR at [Juice Shop](https://juice-shop.herokuapp.com/#/).
2. Dismiss pop-ups and navigate to **Account > Login > Not yet a customer?**
3. Create a fake account (e.g., `test@example.com` with `testpassword`) and log in.
4. Observe HTTP requests in **Burp Suite > HTTP history**.
5. Add the Juice Shop URL to Burp's scope via **Target > Site map**.

---

## Step 4: Juice Shop Shenanigans

### Modify Requests
1. Enable **Proxy > Intercept** in Burp.
2. Add an item to the cart in Juice Shop and capture the `POST /api/BasketItems/` request.
3. Modify the request:
   - Change the `quantity` parameter from `1` to `-1`.
   - Forward the modified request and observe the changes in the browser.

### Test for SQL Injection
1. Capture the `POST /rest/user/login` request using **Proxy > Intercept**.
2. Send the request to **Repeater** and test for SQL injection:
   ```sql
   ;' OR 1=1--
   ```
3. Send the modified request and observe if an admin authentication token is returned.
4. Use the token to log in as Admin in the Juice Shop application.

---

## Step 5: Report Summary

This lab highlighted the risks associated with poor input validation and weak security practices, including:

1. **Parameter Tampering**: Demonstrated by manipulating cart quantities.
2. **SQL Injection**: Bypassed authentication by exploiting a vulnerable login form.

### Key Takeaways
- Secure coding practices, such as input validation and prepared statements, are essential to defend against parameter tampering and SQL injection.
- Regular security testing with tools like Burp Suite helps identify vulnerabilities before attackers can exploit them.
- Applications must implement robust logging and monitoring to detect suspicious activities.

---

## Resources

- [Burp Suite Documentation](https://portswigger.net/burp/documentation)
- [Firefox ESR Proxy Configuration Guide](https://support.mozilla.org/en-US/kb/connection-settings-firefox)
- [OWASP Juice Shop Project](https://owasp.org/www-project-juice-shop/)
- [SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

---

## Disclaimer

This project was conducted in a controlled lab environment for educational purposes. Unauthorized use of these tools or techniques outside such an environment may violate ethical guidelines and legal regulations.
