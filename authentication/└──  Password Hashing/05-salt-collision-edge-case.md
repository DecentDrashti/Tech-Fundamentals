# What Happens If Two Bcrypt Salts Are The Same?

An interesting edge-case question is:

> What if bcrypt accidentally generates the same random salt twice?

---

## What Happens To The Hash?

Suppose:

```text
Student A:
Password = secret123
Salt = Salt_XYZ
```

and

```text
Student B:
Password = secret123
Salt = Salt_XYZ
```

Then:

```text
secret123 + Salt_XYZ
            ↓
       1024 Rounds
            ↓
        Hash_ABC
```

Both users would receive:

```text
Hash_ABC
```

---

## Does Login Break?

No.

Login continues to work perfectly.

Bcrypt still:

- Extracts salt
- Extracts cost factor
- Recomputes hash
- Compares results

Authentication works normally.

---

## Security Impact

If a hacker steals the database:

They may notice:

```text
Hash_A == Hash_B
```

which suggests:

```text
Both users likely share
the same password
```

If the hacker cracks one account:

```text
User A Password
        ↓
User B Password
```

becomes known as well.

---

## Why Don't We Worry About This?

Because the probability is unimaginably small.

---

## Salt Size

Bcrypt uses:

```text
16 bytes
```

A byte contains:

```text
8 bits
```

Therefore:

```text
16 × 8 = 128 bits
```

---

## Number Of Possible Salts

```text
2^128
```

Possible combinations.

Which is approximately:

```text
340,282,366,920,938,463,463,374,607,431,768,211,456
```

possible salts.

---

## Perspective

There are more possible bcrypt salts than there are atoms on Earth.

To experience a collision naturally, you would need to generate enormous numbers of passwords continuously for an absurd amount of time.

---

## Collision Resistance

This property is known as:

```text
Collision Resistance
```

Meaning:

Finding two inputs that generate the same result is extraordinarily difficult.

This concept is one of the foundations of modern cryptography and helps protect systems such as:

- Password Hashing
- SSL Certificates
- Cryptographic Protocols
- Blockchain Systems
