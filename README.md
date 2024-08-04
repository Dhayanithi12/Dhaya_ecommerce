# Dhaya_ecommerce

# Ethically Hacking an E-Commerce Website

This project documents the process of ethically hacking an e-commerce website running on a VM with the IP `192.168.0.21`. The project involves discovering open ports, enumerating services, and exploiting various vulnerabilities to gain unauthorized access and control.

## Prerequisites

- Virtual Machine setup with IP `192.168.0.21`
- Nmap
- Burp Suite
- Dirbuster
- OWASP Juice Shop

## Getting Started

1. **Boot the Box in VM and Get the IP**:
    ```sh
    IP: 192.168.0.21
    ```


2. **Nmap Scan**:
    ```sh
    nmap -sC -A -p- 192.168.0.21 -v
    ```
    Open ports: `20, 21, 80, 3000, 8080`

3. **Service Enumeration**:
    - **Port 21 (FTP)**: No luck with `ftp anonymous@192.168.0.21`
    - **Port 80 (HTTP)**: Apache default webpage at `http://192.168.0.21:80`
    - **Port 8080 (HTTP)**: No response from `http://192.168.0.21:8080`
    - **Port 3000 (HTTP)**: OWASP Juice Shop at `http://192.168.0.21:3000`

## Exploited Vulnerabilities

### 1. Zero Stars (Improper Input Validation)

**Description**: Improper input validation allowing setting a rating to 0, which is impossible as the least rating is 1.

**Steps to Reproduce**:
- Capture the request for customer feedback using Burp Suite.
- Change the rating to 0 in the request and forward it.

**Impact**:
- Unauthorized access to sensitive data.
- Ability to perform restricted actions.

### 2. Confidential Document (Sensitive Data Exposure)

**Description**: Sensitive data exposure through accessible documents in `/ftp` directory.

**Steps to Reproduce**:
- Navigate to `/ftp` directory found using Dirbuster.
- Download `acquisitions.md` containing company secrets.

**Impact**:
- Financial loss.
- Legal penalties.
- Damage to reputation.

### 3. DOM XSS (Cross-Site Scripting)

**Description**: DOM-based XSS executed in the search bar.

**Steps to Reproduce**:
- Inject the payload `<iframe src="javascript:alert('juice shop')">` in the search bar.

**Impact**:
- Stealing sensitive information.
- Redirecting users to malicious sites.

### 4. Error Handling (Security Misconfiguration)

**Description**: Generating internal server error by sending invalid file path request.

**Steps to Reproduce**:
- Change a valid request to an invalid filepath `/rest/Mahesh` using Burp Suite.

**Impact**:
- Unauthorized access.
- Ability to launch further attacks.

### 5. Missing Encoding (Improper Input Validation)

**Description**: URL encoding issue on the photo wall page.

**Steps to Reproduce**:
- Replace `#` with `%23` in the photo source path using CyberChef.

**Impact**:
- Unauthorized access.
- Ability to perform restricted actions.

### 6. Outdated Allowlist (Unvalidated Redirects)

**Description**: Unvalidated redirect to blockchain address.

**Steps to Reproduce**:
- Navigate to `http://192.168.1.10:3000/redirect?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm`

**Impact**:
- Stealing sensitive information.
- Redirecting users to phishing sites.

### 7. Privacy Policy (Privacy Policy Manipulation)

**Description**: Exploiting vulnerabilities in the privacy policy.

**Steps to Reproduce**:
- Read the privacy policy at `http://192.168.1.10:3000/#/privacy-security/privacy-policy`

**Impact**:
- Unauthorized access to sensitive information.

### 8. Repetitive Registration

**Description**: Creating multiple accounts with mismatched password lengths.

**Steps to Reproduce**:
- Register with different original and repeat password lengths.

**Impact**:
- Unauthorized access.
- Ability to perform restricted actions.

### 9. Login Admin (SQL Injection)

**Description**: SQL injection to login as admin.

**Steps to Reproduce**:
- Inject payload `admin' or 1=1--` in the username field.

**Impact**:
- Unauthorized access to sensitive data.

### 10. Admin Section (Broken Access Control)

**Description**: Accessing admin panel through broken access control.

**Steps to Reproduce**:
- Navigate to `http://192.168.1.10:3000/administration`

**Impact**:
- Unauthorized access to sensitive data.

### 11. Five Star Feedback (Broken Access Control)

**Description**: Deleting 5-star feedback as admin.

**Steps to Reproduce**:
- Delete the feedback with admin privileges.

**Impact**:
- Unauthorized access to sensitive data.

### 12. Password Strength (Broken Authentication)

**Description**: Brute-forcing admin password using Burp Suite Intruder.

**Steps to Reproduce**:
- Set payloads for the password field and intercept the request.

**Impact**:
- Unauthorized access to sensitive data.

### 13. Security Policy

**Description**: Manipulating security policy for unauthorized access.

**Steps to Reproduce**:
- Access `security.txt` at `http://192.168.1.3:3000/.well-known/security.txt`

**Impact**:
- Unauthorized access to sensitive information.

### 14. View Basket (Broken Authentication)

**Description**: Manipulating session storage to view other users' baskets.

**Steps to Reproduce**:
- Change `bid` value in session storage.

**Impact**:
- Unauthorized access to sensitive data.

### 15. Weird Crypto (Cryptographic Issues)

**Description**: Exploiting weak cryptography in customer feedback.

**Steps to Reproduce**:
- Comment with weak algorithm (e.g., MD5) in the feedback section.

**Impact**:
- Unauthorized access to sensitive data.

### 16. Admin Registration (Improper Input Validation)

**Description**: Registering a new user with admin privileges by modifying request.

**Steps to Reproduce**:
- Add `role: "admin"` in the registration request.

**Impact**:
- Unauthorized access to sensitive data.


## Conclusion

This project highlights the importance of securing web applications against various vulnerabilities. Proper security measures and regular audits can significantly reduce the risk of unauthorized access and data breaches.

---

**Disclaimer**: This project is for educational purposes only. Unauthorized access and exploitation of systems without permission is illegal and unethical.
