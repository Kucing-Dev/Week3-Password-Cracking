## Password Cracking & Hash Extraction Phases

**W3- PM1 & PM2 | CYBERSECURITY | NETWORKWALKS**

| Field | Detail |
| :--- | :--- |
| **Pentester Name** | **Khairunnisya Lubis** |
| **Program/Batch** | B084-Networkwalks |
| **Date** | 17 November 2026 |
| **Modules completed** | W3-PM1 (John the Ripper / Johnny GUI)<br>W3-PM2 (Password Cracking with Networkwalks Tools) |
| **Client/Target** | 1. Networkwalks (secured written permission already)<br>2. Authorized Lab Sample Files (`My Locked PDF1.pdf`) |
| **Permission secured from client?** | Yes |
| **Phases covered** | **Phase 1:** Hash Extraction & Format Conversion<br>**Phase 2:** Local Dictionary Attack (John the Ripper / Johnny)<br>**Phase 3:** Web-Based Dictionary Attack (Networkwalks Tools)<br>**Phase 4:** Document Decryption & Flag Verification |

---

# 1. Liability Disclaimer

I have performed these activities only on the files & systems where I had secured written permission or the devices/files that I own myself. All these materials are for education and research purposes only. Do not use these techniques on unauthorized files or systems without explicit permission.

---

# 2. Theoretical Background & Tool Overview

### What is Password Cracking?
Password cracking is the process of recovering a plaintext password from stored data or a password-protected file. Security professionals perform password auditing to evaluate password strength and demonstrate the risks of using weak, short, or predictable passwords.

### Encryption vs. Hashing
* **Encryption:** A two-way cryptographic function where plaintext is converted into ciphertext using a key and can be decrypted back to plaintext with the proper key.
* **Hashing:** A one-way cryptographic function that transforms plaintext into a fixed-length scrambled string (message digest/hash). Protected files like PDFs do not store passwords in plaintext; instead, they store cryptographic hashes (e.g., `$pdf$...`).

### What is John the Ripper (JTR)?
**John the Ripper** is a widely used, open-source offline password-cracking tool. It supports hundreds of hash formats and utilizes attack modes such as **Dictionary Attacks** and **Brute-Force Attacks** by hashing candidate words from a wordlist and comparing them against the target hash. **Johnny** serves as the cross-platform graphical user interface (GUI) frontend for the John the Ripper CLI engine.

---

# 3. Methodology 1: Local Password Cracking (John the Ripper & Johnny GUI)

### Step-by-Step Execution:
1. **Engine Setup & Configuration:**
   * Installed **Johnny GUI** alongside the **John the Ripper (Jumbo CLI)** distribution.
<img width="1365" height="763" alt="00_Johnny" src="https://github.com/user-attachments/assets/7b9d84ef-9c91-4439-9b6f-bcfe4cb65a2c" />

   * Linked Johnny to the core executable binary (`john.exe` inside the `/run` directory) to enable local hash processing.
     <img width="1365" height="763" alt="02_john_exe" src="https://github.com/user-attachments/assets/ede9e3a5-e63f-43cb-b8b2-772aa6a14a12" />


1. **PDF Hash Extraction:**
   * Extracted the cryptographic hash string from the encrypted target document (`My Locked PDF1.pdf`) using a `pdf2john`-compatible extractor, generating a string starting with `$pdf$4*4*128*...`.
     <img width="1365" height="763" alt="04_locked_pdf1" src="https://github.com/user-attachments/assets/116ed26d-2264-47a6-b77d-1d517d957f57" />
<img width="1365" height="763" alt="05_hash" src="https://github.com/user-attachments/assets/2407b24d-608e-4388-9c59-393f27ef372f" />

   * Saved the extracted hash string into a plain text file named `hash1.txt`.
     
     <img width="1365" height="763" alt="07_file_hash" src="https://github.com/user-attachments/assets/fa630802-469b-4c00-b60f-578b3d62cec1" />


2. **Executing the Attack in Johnny:**
   * Loaded `hash1.txt` into Johnny via **Open password file** (`PASSWD` format).
     <img width="1365" height="763" alt="06_passwd" src="https://github.com/user-attachments/assets/71c972db-d1f6-439c-825a-71d873b2cccd" />

   * Initiated the cracking session by clicking **Start new attack**.
3. **Result & Verification:**
   * John the Ripper successfully matched the hash against its wordlist and recovered the plaintext password: `good-luck`.
     <img width="1365" height="763" alt="08_password_result" src="https://github.com/user-attachments/assets/8f55bbac-ad0d-44b3-9659-e7c53a806e9d" />
     <img width="1365" height="763" alt="09_congratulations" src="https://github.com/user-attachments/assets/f0c89e44-44e2-4837-bc9a-b4d005eaa721" />



---

# 4. Methodology 2: Web-Based Cracking (Networkwalks Tools)

To demonstrate browser-based offline/local parsing workflows without local CLI installations, the second part of the module utilized Networkwalks' browser tools:

### Step-by-Step Execution:
1. **Hash Extraction via Networkwalks Hash Calculator:**
   * Accessed the [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/).
   * Uploaded the encrypted file `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf`, which parsed the file locally in the browser and generated the `pdf2john`/`hashcat`-compatible `$pdf$...` hash string.
   * Copied the complete hash value.
2. **Dictionary Attack via Networkwalks Password Cracker:**
   * Opened the [Networkwalks Password Cracker](https://networkwalks.com/password-cracker/).
   * Pasted the extracted `$pdf$...` hash into the **PDF HASH** input field.
   * Selected the **Built-in list** wordlist and clicked **START CRACKING**.
3. **Document Unlocking & Flag Capture:**
   * Once the dictionary attack succeeded, used the recovered password to unlock the protected PDF document.
   * Verified access by capturing the lab completion flag inside the document:
     ```text
     nw{networkwalks_flag1_jtr_270521_1}
     ```
    

---

# 5. Key Takeaways & Remediation

* **Vulnerability:** Weak or dictionary-based passwords protecting sensitive documents can be recovered within seconds once the cryptographic hash is extracted.
* **Remediation:** Enforce strong password policies (minimum 12+ characters combining uppercase, lowercase, numbers, and special symbols) and utilize modern AES-256 encryption standards for sensitive documents.

