# What Does 10 Salt Rounds Mean in Bcrypt?

One of the most common questions is:

> If we bcrypt a password with only 10 salt rounds, does bcrypt simply add the number 10 to the password?

The answer is:

**No.**

---

## What does the "10" actually mean?

When you write:

```javascript
bcrypt.hash(password, 10)
```

the number `10` is called the:

- Work Factor
- Cost Factor

It represents:

```text
2^10
```

which equals:

```text
1024 rounds
```

---

## Examples

### Salt Rounds = 10

```text
2^10 = 1024 rounds
```

### Salt Rounds = 11

```text
2^11 = 2048 rounds
```

### Salt Rounds = 12

```text
2^12 = 4096 rounds
```

---

## What Happens Internally?

Bcrypt does not append the number 10.

Instead, it repeatedly runs the password through its hashing process.

Example:

```text
Password
    ↓
Hash Round 1
    ↓
Hash Round 2
    ↓
Hash Round 3
    ↓
...
    ↓
Hash Round 1024
    ↓
Final Hash
```

---

## Why Do We Do This?

This is intentionally designed to slow down the hashing process.

If a hacker steals your database:

Without bcrypt:

```text
Millions of guesses per second
```

With bcrypt:

```text
Every guess requires
1024 expensive calculations
```

This makes brute-force attacks significantly slower and more difficult.
