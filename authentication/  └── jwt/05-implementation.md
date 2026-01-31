# 🛠️ JWT Implementation (Next.js)

This section explains the implementation of JWT authentication in Next.js, using a **clear separation of responsibilities**.  

---

## 🧩 1. The ID Card Maker (`lib/auth.ts`)

This file is pure logic. It doesn't talk to the database; it only handles the digital "paperwork."

```ts
import jwt from 'jsonwebtoken';

// 1. Pulling the 'Secret Stamp' from the vault (.env)
const JWT_SECRET = process.env.JWT_SECRET!; 

// 2. This function 'prints' the ID Card
export function createToken(payload: object) {
  // jwt.sign takes the user data, mixes it with the secret, 
  // and creates a long encrypted string.
  return jwt.sign(payload, JWT_SECRET, { expiresIn: '1h' });
}

// 3. This function 'checks' if the ID Card is fake
export function verifyToken(token: string) {
  // If someone changed a single letter in the token, 
  // this will throw an error.
  return jwt.verify(token, JWT_SECRET);
}
```
🧩 2. The Security Guard (api/auth/login/route.ts)

This is the "Brain." It handles the incoming request and makes the big decisions.

```ts
 export async function POST(req: Request) {
    try {
      // A. Reading the clipboard: Extract data sent from the Login Page
      const { identifier, password, role } = await req.json();
  
      let user: any = null;
  
      // B. Decision Tree: Which filing cabinet (Table) should I check?
      if (role === 'student') {
        // Look in the Student cabinet for an Enrollment Number
        user = await prisma.student.findFirst({
        where: { EnrollmentNo: identifier },
      });
    } else {
      // Look in the Staff cabinet for an Email AND a specific Role (Admin/Faculty)
      user = await prisma.staff.findFirst({
        where: {
          Email: identifier,
          Role: role,
        },
      });
    }

    // C. Validation Step 1: Does this person even exist?
    if (!user) {
      return NextResponse.json({ message: 'Invalid credentials' }, { status: 401 });
    }

    // D. Validation Step 2: Is the password correct?
    // user.Password comes from the DB, password comes from the user's input.
    if (password !== user.Password) {
      return NextResponse.json({ message: 'Invalid credentials' }, { status: 401 });
    }

    // E. Printing the Card: If they pass, we generate the JWT
    const token = createToken({
      id: user.id || user.EnrollmentNo,
      role, // We save the role inside the token so we remember who they are later
    });

    const response = NextResponse.json({ role });

    // F. Handing over the Card: We put the token in a "Cookie"
    response.cookies.set('token', token, {
      httpOnly: true,     // Security: JavaScript can't touch this cookie
      secure: false,       // Only use HTTPS (set to true for deployment!)
      sameSite: 'strict',  // Prevents other websites from stealing the session
      path: '/',           // The cookie is valid for the whole website
    });

    return response;

  } catch (error) {
    // If the database is down or the code crashes, return a 500
    return NextResponse.json({ message: 'Prisma failed' }, { status: 500 });
  }
}
```
🧩 3. The Reception Desk (login/page.tsx)

This is the code that runs in the user's browser.

```ts
const handleLogin = async (e: React.FormEvent) => {
    e.preventDefault(); // Don't refresh the page
    
    // 1. Sending the data to our Security Guard
    const res = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            identifier, // Our state variable (Enrollment or Email)
            password,   // Our state variable
            role,       // The role selected in the UI
        }),
    });

    const data = await res.json();

    // 2. Handling the Guard's answer
    if (!res.ok) {
        alert(data.message); // Show "Invalid credentials" or "Prisma failed"
        return;
    }

    // 3. Success! The Guard gave us a cookie. 
    // Now we push the user into their specific dashboard.
    router.push(`/dashboard/${data.role}`);
};
```
🧠 Why this structure is "Amazing"

Separation of Concerns: The Login Page only cares about UI. The API only cares about Database/Logic. The Auth Lib only cares about Security.

Statelessness: The server doesn't have to "remember" who is logged in. The user carries their own ID card (the cookie) every time they visit a new page.

Role-Based Access: Because we check the role inside the API, a student can't log into the staff table even if they know a staff member's email.
