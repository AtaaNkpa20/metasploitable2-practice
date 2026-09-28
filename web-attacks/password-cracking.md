# Password Hash Cracking — John the Ripper

## Objective
Take the MD5 password hashes extracted via SQL injection and crack them to recover plaintext passwords.

---

## Tools Used
- **John the Ripper** — password cracking tool (pre-installed on Kali)
- **rockyou.txt** — wordlist from a real 2009 data breach, contains 14+ million common passwords (pre-installed on Kali, compressed)

---

## Steps

### Step 1 — Prepare the wordlist
rockyou.txt ships compressed on Kali. Extract it first:
```bash
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

Confirm it's available:
```bash
ls /usr/share/wordlists/rockyou.txt
```

---

### Step 2 — Save the hashes to a file
```bash
nano hashes.txt
```

Contents:
```
admin:5f4dcc3b5aa765d61d8327deb882cf99
gordonb:e99a18c428cb38d5f26085367892e03
1337:8d3533d75ae2c3966d7e0d4fcc69216b
pablo:0d107d09f5bbe40cade3de5c71e9e9b7
smithy:5f4dcc3b5aa765d61d8327deb882cf99
```

---

### Step 3 — Run John the Ripper
```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

---

### Step 4 — Display cracked passwords
```bash
john --show --format=raw-md5 hashes.txt
```

---

## Results

```
admin:password
gordonb:abc123
1337:charley
pablo:letmein
smithy:password

5 password hashes cracked, 0 left
```

All 5 hashes cracked in seconds.

---

## Analysis

| Username | Password | Strength |
|----------|----------|----------|
| admin | password | Very weak — #1 most common password |
| gordonb | abc123 | Very weak — top 5 most common |
| 1337 | charley | Weak — common name |
| pablo | letmein | Weak — top 10 most common |
| smithy | password | Very weak — same as admin |

- All passwords are in the top common password lists
- admin and smithy share the same password — a reuse vulnerability
- MD5 without salting means identical passwords produce identical hashes, making duplicates immediately visible

---

## How John the Ripper Works

John takes each word from the wordlist, hashes it using the specified algorithm (`raw-md5` here), and compares it to the target hashes. When a match is found, the plaintext password is revealed. This is called a **dictionary attack**.

---

## What I Learned

- Weak passwords are cracked in seconds even with basic tools
- MD5 is a fast hashing algorithm — never appropriate for passwords. Modern systems use bcrypt, scrypt, or Argon2 which are intentionally slow
- Password reuse (admin = smithy) means compromising one account compromises both
- Salting hashes (adding a random value before hashing) prevents identical passwords from producing identical hashes, and defeats precomputed rainbow table attacks
- Extracted hashes from SQL injection can lead directly to account takeover
