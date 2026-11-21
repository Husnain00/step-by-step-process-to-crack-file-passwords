# 🔐 Week 3: PDF Hash Extraction & Password Cracking Lab

![Focus](https://img.shields.io/badge/Focus-Password_Cracking_%26_Hash_Analysis-red)
![Tool](https://img.shields.io/badge/Tool-John_the_Ripper-blue)
![Academy](https://img.shields.io/badge/Lab-Networkwalks_Academy-green)

## 📌 Overview
This repository documents the step-by-step methodology used to complete the **Week 3 Cybersecurity Challenge** provided by **Networkwalks Academy**. The objective of this lab was to extract cryptographic hashes from password-protected PDF files, perform an offline dictionary attack using **John the Ripper (JtR)**, recover the plaintext passwords, and capture the challenge flags.

---

## 🛠️ Tools & Prerequisites
* **John the Ripper (JtR)** (CLI & GUI / Johnny)
* **pdf2john** (Utility to extract hash strings from encrypted PDF documents)
* **Wordlist** (e.g., `rockyou.txt` or a custom targeted dictionary)
* **PDF Viewer** (to verify the unlocked document and retrieve the flag)

---

## 🚀 Step-by-Step Walkthrough

### Step 1: Extract the PDF Hash
To crack a password-protected PDF offline, we first need to extract its cryptographic hash in a format that John the Ripper can understand.

Using `pdf2john` (or the provided lab extractor), extract the hash from the target file and save it to a text file:
```bash
pdf2john target_document.pdf > pdf_hash.txt
```

**Extracted Hash Sample:**
```text
$pdf$4*4*128*-1028*1*16*ca7f72f11459cba469f1005a8765ed51*32*f32d8fa1bfbe2648226dffc39f7909ea0021446990b9e4114071a4d9104984c1*32*9322f50c29569712067a775264635e4954ccb1b99e209d664984054ffad30a6a
```

---

### Step 2: Configure & Run John the Ripper
Once the hash is isolated, load `pdf_hash.txt` into John the Ripper using a wordlist to initiate a dictionary attack.

**Via Command Line:**
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt pdf_hash.txt
```

**Via GUI (Johnny):**
1. Click **Open password file** and select `pdf_hash.txt`.
2. Ensure the detected format is set to `PDF`.
3. Click **Start new attack** to begin testing candidate passwords against the hash.

---

### Step 3: Analyze Output & Recover Plaintext Password
John the Ripper iterates through the wordlist, hashing each candidate and comparing it against the target PDF hash.

```text
[-] Trying: service
[-] Trying: canada
[-] Trying: hockey
[-] Trying: killer
[-] Trying: george
[-] Trying: asdfgh
[-] Trying: zxcvbn
[-] Trying: qwertyuiop
[-] Trying: 111222
[+] MATCH password1 ✓
```

To display cracked passwords stored in John's pot file at any time, run:
```bash
john --show pdf_hash.txt
```
* **Recovered Password:** `password1`

---

### Step 4: Unlock the PDF & Capture the Flags
Using the recovered password (`password1`), open the protected PDF documents to access their contents and retrieve the challenge flags.

* **Flag 1:** `nw{networkwalks_flag1_jtr_270521_1}`
* **Flag 2 (Persistence):** `nw{networkwalks_persistence_jtr_270521}`

---

## 🛡️ Security Takeaways & Mitigation
* **Avoid Weak Passwords:** Common dictionary words and predictable patterns (like `password1`) can be cracked in seconds during an offline attack.
* **Use High-Entropy Passphrases:** Enforce long, complex passwords combining uppercase/lowercase letters, numbers, and symbols to drastically increase brute-force resistance.
* **Modern Encryption Standards:** Ensure sensitive documents use strong encryption algorithms (such as AES-256) rather than legacy RC4 or weak 128-bit configurations.

---

## ⚠️ Disclaimer
*This documentation is strictly for educational purposes and authorized lab training. Always ensure you have explicit permission before performing security testing or password auditing on any system or file.*