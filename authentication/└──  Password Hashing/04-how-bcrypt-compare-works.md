# How Does bcrypt.compare() Work?

If the same password produces different hashes every time, a natural question appears:

> How does login work?

---

## Example Stored Hash

```text
$2b$10$KaxtGOOnXJjDaiEUZiq7HeOITDd0DMOFBM8eF8pSQtPvYoV11C/CO
```

This single string contains everything bcrypt needs.

---

## Breaking It Down

### Part 1

```text
$2b$
```

Bcrypt version.

---

### Part 2

```text
$10$
```

Cost Factor.

Meaning:

```text
2^10 = 1024 rounds
```

---

### Part 3

```text
KaxtGOOnXJjDai...
```

Contains:

- Original Salt
- Final Hash

The first portion stores the salt that was originally used.

---

# What Happens During Login?

Suppose the user enters:

```text
admin123
```

---

## Step 1

Bcrypt reads the stored hash from the database.

---

## Step 2

Bcrypt extracts:

- Salt
- Cost Factor

from the stored string.

---

## Step 3

It combines:

```text
Entered Password
      +
Stored Salt
```

---

## Step 4

It performs the same number of rounds.

Example:

```text
1024 rounds
```

---

## Step 5

It compares the newly generated hash with the stored hash.

If they match:

```text
Password Correct
```

Otherwise:

```text
Password Incorrect
```

---

## Why This Is Brilliant

Suppose:

Student Aarav:

```text
password123
```

Student Drashti:

```text
password123
```

Because bcrypt generates different salts:

```text
Different Salt
        ↓
Different Hash
```

A hacker cannot easily identify users sharing the same password.

This protects against Rainbow Table attacks and significantly improves security.
