# What is Bcrypt?

Bcrypt is a password hashing algorithm specifically designed for storing passwords securely.

Instead of storing the original password inside the database, bcrypt converts the password into a hashed value.

Example:

```text
Password:
admin123

Stored in Database:
$2b$10$KaxtGOOnXJjDaiEUZiq7HeOITDd0DMOFBM8eF8pSQtPvYoV11C/CO
```

The original password cannot be directly obtained from the hash.

This helps protect users even if the database is stolen.

---

## Why Not Store Passwords Directly?

Bad:

```text
Database
---------
admin123
password123
welcome123
```

If a hacker gains access to the database, every password becomes visible.

---

Good:

```text
Database
---------
$2b$10$......
$2b$10$......
$2b$10$......
```

Passwords are stored as hashes instead of plain text.

---

## Why Bcrypt?

Bcrypt is designed to:

- Hash passwords securely
- Slow down brute-force attacks
- Use random salts automatically
- Prevent rainbow table attacks

This is why bcrypt is one of the most commonly used password hashing algorithms in modern applications.
