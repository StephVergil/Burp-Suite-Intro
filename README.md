# Homework 10: Burp Suite Intro

[Burp Suite Project](https://github.com/StephVergil/Burp-Suite-Intro/blob/main/Homework%2010.docx) 

## Step 1: Configure Burp Suite

### Check for Existing Proxies
1. Open a terminal and type:
   ```bash
   env | grep -i proxy
   ```
2. Take note of the output and close the terminal.
3. Start Burp Suite by double-clicking the icon or finding it in your start menu.
4. Accept the user agreement and proceed with a "Temporary project" setup using default settings. Click "Start Burp."
5. Skip updating Kali software if prompted.
6. The main Burp Dashboard should now be visible.

### Configure Upstream Proxy
1. Navigate to `User options > Upstream Proxy Servers > Add` and set:
   - Destination Host: `*`
   - Proxy Host: `proxy`
   - Proxy Port: `8080`
   - Authentication Type: `None`
   - Click `OK`.
2. Go to `Proxy > Options` and set:
   - Bind to Port: `8088`
   - Bind to Address: `127.0.0.1`
3. Go to `Proxy > Intercept` and set it to "Intercept is off."
4. Go to `Proxy > HTTP history`.

---

## Step 2: Set Up Firefox ESR

### Update Firefox ESR
1. Open a terminal and run:
   ```bash
   sudo apt update
   sudo apt install firefox-esr
   ```
2. Follow prompts and type `Y` to continue.
3. If errors occur:
   - Run:
     ```bash
     sudo apt --fix-broken install
     sudo dpkg --configure -a
     ```
   - Reinstall Firefox ESR:
     ```bash
     sudo apt install firefox-esr
     ```

### Configure Proxy
1. Open Firefox ESR, go to `Settings > Network Settings`, and choose "Manual proxy configuration."
2. Set:
   - HTTP Proxy: `127.0.0.1`
   - Port: `8088`

### Install Burp’s CA Certificate
1. Open Firefox and visit `http://burp`.
2. Download the CA Certificate.
3. Import it via `Settings > Privacy & Security > Certificates`.
4. Check both trust boxes and confirm.

---

## Step 3: Set Up Juice Shop Account

1. Open Juice Shop in Firefox at [Juice Shop](https://juice-shop.herokuapp.com/#/).
2. Dismiss pop-ups and click `Account > Login > Not yet a customer?`
3. Create a fake account (e.g., `test@example.com` with `testpassword`) and log in.
4. Observe requests in `Burp Suite > HTTP history`.
5. Add the Juice Shop URL to Burp's scope via `Target > Site map`.

---

## Step 4: Juice Shop Shenanigans

### Modify Requests
1. Enable `Proxy > Intercept` in Burp.
2. Add an item to the cart and modify the `POST /api/BasketItems/` request:
   - Change the quantity from `1` to `-1`.
   - Forward the modified response and observe changes in the browser.

### Test for SQL Injection
1. Toggle `Intercept` on and capture the `POST /rest/user/login` request.
2. Send it to `Repeater` and test:
   - Enter:
     ```sql
     ;' OR 1=1--
     ```
   - Send the request and observe the admin authentication token.
3. Use this token in the browser to log in as Admin.

---

## Step 5: Report Summary

This lab demonstrated the security risks of weak input validation using the OWASP Juice Shop. Exploits included:
- **Parameter Tampering:** Manipulated cart item quantities.
- **SQL Injection:** Bypassed authentication.

The exercise emphasized secure coding practices to protect against vulnerabilities like SQL injection and parameter tampering, which can lead to significant security breaches.

---

### Disclaimer
This project was conducted in a controlled environment. Unauthorized use of these techniques or tools outside such an environment may violate ethical guidelines and legal regulations.
