## 🔐 How the JWT Signature Is Verified

When a user sends a JWT to the server, the server does **not blindly trust it**.  
Instead, it performs a verification process to ensure the token was not tampered with.

### 🔁 Verification Steps

1. The server extracts the **Header** and **Payload** from the token sent by the user.
2. The server retrieves the **JWT_SECRET** stored securely in the `.env` file.
3. Using the same algorithm mentioned in the Header, the server **re-calculates** what the Signature *should* be based on:
   - Header  
   - Payload  
   - JWT_SECRET
4. **Comparison Step**:
   - ✅ **If the signatures match** → The token is authentic and verified.
   - ❌ **If they don’t match** →  
     Either the Payload was modified **or** the token was signed using a fake secret.

---

## 👀 Visibility of JWT Components

| Part | Where is it? | Can the user see it? |
|----|----|----|
| Header / Payload | Inside the token | ✅ Yes (Base64 encoded, not encrypted) |
| Signature | Inside the token | ✅ Yes (but unreadable gibberish) |
| JWT_SECRET | Only on the server | ❌ **Never exposed** |

---

## 🚨 Crucial Security Rule

> **If a user ever gains access to your `JWT_SECRET`, your entire authentication system is compromised.**

With the secret, an attacker can:
- Forge their own tokens
- Modify roles or permissions
- Pretend to be an **Admin** or any other user

Think of the `JWT_SECRET` as the **official stamp of the research lab** —  
once stolen, anyone can print fake ID cards.

---

## 🧩 What a JWT Looks Like

A JWT always consists of **three Base64-encoded strings** separated by dots:

  xxxxx.yyyyy.zzzzz

### Breakdown

1. **xxxxx (Header)**  
   Tells the server which signing algorithm was used (e.g., HS256)

2. **yyyyy (Payload)**  
   Contains user-related data such as:
   - User ID
   - Role
   - Expiration time

3. **zzzzz (Signature)**  
   The cryptographic proof that:
   - The token was issued by the server
   - The data has not been altered

---

## ⚠️ Important Note

JWT verification only confirms that the token is **authentic and untampered**.  
It does **not** automatically grant permission to access resources.

> **Authentication ≠ Authorization**

Authorization checks (roles, permissions, access rules) must still be handled
separately by the application.
