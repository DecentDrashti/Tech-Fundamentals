# Why Does The Same Password Generate Different Hashes?

A very common question is:

> If two users use the exact same password, why do they get different hashes?

Example:

```text
Password:
admin123
```

User A hashes it:

```text
$2b$10$ABC...
```

User B hashes it:

```text
$2b$10$XYZ...
```

Different hashes.

Why?

---

## The Secret Ingredient: Salt

Bcrypt automatically generates a random value called a Salt.

A salt is:

```text
A completely random 16-byte string
```

generated every time:

```javascript
bcrypt.hash()
```

is called.

---

## The Real Formula

Before hashing starts:

```text
Password
    +
Random Salt
```

are combined.

Then bcrypt performs all hashing rounds.

```text
Password + Random Salt
            ↓
      1024 Rounds
            ↓
       Final Hash
```

Because the salt changes every time, the final hash changes every time.

---

# When Is The Salt Generated?

The salt is generated BEFORE the hashing rounds begin.

Example:

```javascript
bcrypt.hash(password, 10)
```

---

## Step 1: Random Salt Generation

Bcrypt generates a completely random 16-byte salt.

---

## Step 2: Mixing Ingredients

```text
Password + Salt
```

are combined.

---

## Step 3: The 1024 Rounds

Because:

```text
2^10 = 1024
```

bcrypt performs 1024 hashing rounds.

```text
Round 1
 ↓
Round 2
 ↓
Round 3
 ↓
...
 ↓
Round 1024
```

---

## Step 4: Build Final Package

Bcrypt combines:

- Version
- Cost Factor
- Salt
- Final Hash

into a single string.

---

## Why This Order Matters

If salt were added after hashing:

```text
Same Password
      ↓
Same Calculations
      ↓
Same Result
```

Hackers could precompute attacks.

By generating the salt first:

```text
Password + Unique Salt
```

every calculation becomes unique from the very beginning.

This is one of the reasons bcrypt is so secure.
