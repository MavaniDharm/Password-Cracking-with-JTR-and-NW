# 🔓 PDF Password Recovery — Two Methods Compared

A hands-on cybersecurity lab, completed as part of the **Networkwalks Cybersecurity Program (Batch B084)**, demonstrating **two different ways** to recover the password of a locked PDF — one using a locally installed tool, and one using free browser-based tools. Both follow the exact same underlying idea: convert the PDF's encryption into a crackable hash, then run a dictionary attack against it.

> ⚠️ **Disclaimer:** Performed entirely on self-created test files (`My Locked PDF1.pdf`, `My Locked PDF2.pdf`) for educational/authorized practice only. Only attempt password recovery on files you own or are explicitly permitted to test — never on third-party documents.

---

## 🧰 Tools Used

| Method | Tool | Role |
|---|---|---|
| **Method 1 — Local** | [John the Ripper (Jumbo)](https://www.openwall.com/john/) | Core password-cracking engine (installed locally) |
| **Method 1 — Local** | [Johnny](https://openwall.info/wiki/john/johnny) | GUI front-end for John the Ripper |
| **Method 1 — Local** | Online PDF Hash Extractor (pdf2john-style) | Converts the PDF into an attackable hash |
| **Method 2 — Browser** | [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/) | Extracts the `$pdf$...` hash directly in-browser, nothing uploaded |
| **Method 2 — Browser** | [Networkwalks Password Cracker](https://networkwalks.com/password-cracker/) | Dictionary-attack lab that cracks the hash, same idea as John the Ripper |

---

## ⚙️ The Core Idea (Applies to Both Methods)

A password-protected PDF can't be attacked directly — its encryption metadata first has to be converted into a hash string (`$pdf$...`). Once that hash exists, a cracking tool hashes candidate passwords from a wordlist one by one (using the same algorithm, e.g. MD5 + RC4 key derivation) and compares each to the target hash. A match reveals the original password.

```
Locked PDF → extract $pdf$ hash → run dictionary attack → recovered password → unlock PDF
```

John the Ripper does this from the command line (via Johnny's GUI); the Networkwalks tools do the exact same thing, entirely inside the browser, with no install required.

---

## 🪜 Method 1 — Local: John the Ripper (Jumbo) + Johnny GUI

| # | Step | Screenshot |
|---|---|---|
| 1 | Downloaded the latest **jumbo** Windows build of John the Ripper from Openwall — jumbo supports far more hash/file types than core John, including PDFs. | `01_download-john-the-ripper.png` |
| 2 | Downloaded **Johnny**, the GUI front-end for John the Ripper, from the Openwall wiki. | `02_download-johnny-gui.png` |
| 3 | On first launch, browsed to the extracted John the Ripper folder and selected the core `john` executable for Johnny to use. | `03_johnny-browse-john-executable.png` |
| 4 | Confirmed in Johnny's **Settings** tab that John the Ripper was correctly detected (`1.9.0-jumbo-1 OMP`) — an important check before attacking. | `04_johnny-settings-john-detected.png` |
| 5 | Uploaded the locked `My Locked PDF1.pdf` to an online pdf2john-style hash extractor. | `05_upload-pdf-to-hash-extractor.png` |
| 6 | Copied the extracted `$pdf$...` hash returned by the extractor. | `06_extracted-pdf-hash.png` |
| 7 | Loaded the hash into Johnny's Passwords tab — status showed `0% (0/1 cracked)`. | `07_johnny-hash-loaded-0-percent.png` |
| 8 | Ran **Start new attack** — John the Ripper recovered the password **`good-luck`**, status updated to `100% cracked`. | `08_password-cracked-100-percent.png` |
| 9 | Entered the recovered password into the PDF's password prompt. | `09_enter-cracked-password-in-pdf.png` |
| 10 | PDF opened successfully, confirming the password was correct. 🎉 | `10_pdf-unlocked-result.png` |

**Recovered password:** `good-luck`

---

## 🪜 Method 2 — Browser-Based: Networkwalks Hash Calculator + Password Cracker

| # | Step | Screenshot |
|---|---|---|
| 1 | Opened the **Networkwalks Hash Calculator** (`networkwalks.com/hash-calculator/`) — supports MD5/SHA hashing or extracting a crackable hash from an encrypted PDF, all computed **locally in the browser** (nothing uploaded). | `m2-01_hash-calculator-tool.png` |
| 2 | Switched to the **PDF** tab and uploaded `My Locked PDF2.pdf`. The tool detected it was encrypted and extracted a pdf2john/hashcat-compatible `$pdf$...` hash (Revision R4, 128-bit). | `m2-02_pdf-hash-extracted.png` |
| 3 | Opened the **Networkwalks Password Cracker** (`networkwalks.com/password-cracker/`) — a Dictionary Attack Lab that "hashes every word in a wordlist and matches it against a PDF password hash, the same idea John the Ripper uses." Pasted the hash and used the built-in 100-password list. | `m2-03_password-cracker-tool.png` |
| 4 | Clicked **Start Cracking** — the live log tried each candidate password (`123456`, `password`, `qwerty`, etc.) until, at 91/100 tried, it displayed **PASSWORD CRACKED SUCCESSFULLY: `password1`**. | `m2-04_password-cracked-result.png` |
| 5 | Opened `My Locked PDF2.pdf`, entered `password1`, and confirmed the file unlocked correctly. ✅ | — |

**Recovered password:** `password1`

---

## 🔍 Method Comparison

| | Method 1: John the Ripper + Johnny | Method 2: Networkwalks Browser Tools |
|---|---|---|
| **Install required** | Yes (John the Ripper + Johnny) | No — runs entirely in-browser |
| **Where it runs** | Locally on your machine | Client-side in the browser |
| **Hash extraction** | Online pdf2john-style extractor | Networkwalks Hash Calculator (local to browser) |
| **Attack engine** | John the Ripper (via Johnny GUI) | Networkwalks Password Cracker (JS dictionary attack) |
| **Wordlist used** | John's default handling | Built-in 100-password list (or custom `.txt` upload) |
| **Best for** | Realistic, industry-standard pentesting workflow | Quick demos, learning the concept with zero setup |

---

## 💡 Key Takeaways

- 🔑 Both methods rely on the same core trick: convert a PDF's encryption into a `$pdf$...` hash, then run a dictionary attack against it — the raw PDF itself is never directly "crackable."
- 🐏 **John the Ripper (jumbo)** is the industry-standard, highly optimized engine for this; **Johnny** just adds a GUI on top.
- 🌐 The **Networkwalks** browser tools demonstrate the exact same concept with zero installation — useful for quick learning or demos, and everything is processed locally without uploading the file.
- 🪶 Both test passwords (`good-luck`, `password1`) were cracked almost instantly from small wordlists — a strong reminder of how little protection weak/common passwords actually provide.
- ⚙️ Always confirm your cracking tool is correctly configured (e.g. Johnny's "Detected John the Ripper..." message) before running an attack, to avoid silent failures.
- 🚫 Both methods are strictly for recovering passwords on **files you own or are authorized to test** — never use them against someone else's documents without permission.

---

## 🙋 About This Practice

Completed as part of the **Networkwalks Cybersecurity Program (Batch B084)**, focused on building practical, hands-on skills in password security, password-auditing tools, and ethical-hacking fundamentals.

🔗 Connect: [linkedin.com/in/dharm-mavani-075525344](https://www.linkedin.com/in/dharm-mavani-075525344)

---

## 📁 Repository Structure

```
.
├── README.md
│
├── Method 1 — John the Ripper + Johnny
│   ├── 01_download-john-the-ripper.png
│   ├── 02_download-johnny-gui.png
│   ├── 03_johnny-browse-john-executable.png
│   ├── 04_johnny-settings-john-detected.png
│   ├── 05_upload-pdf-to-hash-extractor.png
│   ├── 06_extracted-pdf-hash.png
│   ├── 07_johnny-hash-loaded-0-percent.png
│   ├── 08_password-cracked-100-percent.png
│   ├── 09_enter-cracked-password-in-pdf.png
│   └── 10_pdf-unlocked-result.png
│
└── Method 2 — Networkwalks Browser Tools
    ├── m2-01_hash-calculator-tool.png
    ├── m2-02_pdf-hash-extracted.png
    ├── m2-03_password-cracker-tool.png
    └── m2-04_password-cracked-result.png
```
