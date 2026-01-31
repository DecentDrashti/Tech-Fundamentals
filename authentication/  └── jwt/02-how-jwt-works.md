# 🧠 Imagine our application as a high-security University Research Lab

> *Note: The core terminology stays the same—only the file names and syntax differ across languages.  
> Next.js is used as the reference.*

---

Every file we write has a **specific responsibility** —  
from the *receptionist at the front desk* to the *person in the basement guarding the safe*.

Below is a clear breakdown of each role 👇

---

 ## 1. The Hidden Vault (.env)
   Think of this as a physical safe hidden behind a painting in the Principal’s office.

   Purpose: It stores sensitive secrets like your database password and your JWT_SECRET.
   
  Why it matters: If you put your database password directly in your code, anyone who sees your code can steal your data. By putting it here, we keep the "keys to the kingdom" separate from the "instructions for the kingdom."

## 2. The Phone Line (prisma.ts)
  This is the dedicated, high-speed cable connecting our Next.js app to your MariaDB database.
  
  Purpose: It creates a "Singleton" instance. In plain English: it makes sure we don't pick up the phone and dial the database 1,000 times until the line gets busy (the "too many connections" error).
  
  The MariaDB Twist: Since you're using a specific MariaDB adapter, this file translates our modern code into a language the database understands.(only if you uses such things like in nextjs

## 3. The ID Card Maker (lib/auth.ts)
This is the Machine that prints official University ID cards.

Purpose: It uses your JWT_SECRET to "sign" a token.

The Magic: When a user logs in, we give them a digital ID card (the JWT). Because it’s signed with your secret, the user can’t change their name or role. If they try to edit the "Student" role to "Admin" on the card, the "signature" breaks, and our system knows they are cheating.

## 4. The Security Guard (api/auth/login/route.ts)
This is the Brain of the operation. It’s the guard standing at the gate.

The Process: 
  1. He takes the credentials (Email/Enrollment and Password) from the user.
  2. He looks at the Role the user claimed to be.
  3. If they say "Student," he checks the Student Table. If they say "Admin," he checks the Staff Table.
  4. He compares the password they gave with the one in the database.
  5. If everything matches, he calls the ID Card Maker (auth.ts), gets a token, and puts it in the user’s "pocket" (the Cookie).

## 5. The Reception Desk (login/page.tsx)
This is the Face of the app. It’s the beautiful glass desk where the user actually stands.

Purpose: It collects information. We used React State (useState) to remember what the user is typing in real-time.

The Hand-off: When the user clicks "Authenticate," this page bundles all that info up and sends it to the Security Guard (the API route) via a "Fetch" request.

## Summary: How it all works together
1. Student enters enrollment and password on the Reception Desk (page.tsx).

2. The desk sends that info to the Security Guard (route.ts).

3. The guard uses the Phone Line (prisma.ts) to check the database records (using the URL in the Vault .env).

4. If the student is real, the guard asks the ID Card Maker (auth.ts) for a token.

5. The guard hands the token back to the student as a Cookie.

6. The desk sees the "Success" message and pushes the student into the Student Dashboard.
