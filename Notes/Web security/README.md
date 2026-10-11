# Web & Security Fundamentals — Quick Reference

**Big picture analogy:** Think of a web app like a **hotel**.
- HTTP = the front desk conversation (request/reply)
- Cookies/Sessions = your room key/wristband
- Authentication = showing ID at check-in
- Authorization = whether your key opens the gym, or only your room
- TLS = the locked, sealed envelope your key card details travel in
- CORS/CSRF/XSS/SQLi = different ways a scammer tries to trick the front desk or other guests

Keep that hotel picture in your head — it'll anchor everything below.

---

## 1. The Conversation Layer

### HTTP
- The protocol browsers and servers use to talk: **request → response**.
- Stateless — each request is a fresh conversation, the server forgets you the moment it replies.
- Example: your browser sends `GET /profile`, server replies with HTML + a `200 OK` status.
- Because it's stateless, we needed cookies/sessions to "remember" you.

### Headers
- Metadata attached to every HTTP request/response — like the label on a package (sender, content type, instructions).
- Examples: `Content-Type: application/json`, `Authorization: Bearer <token>`, `Set-Cookie: session=abc123`.
- Security-relevant headers: `Content-Security-Policy` (blocks XSS), `Strict-Transport-Security` (forces HTTPS), `X-Frame-Options` (blocks clickjacking).

### TLS
- Encrypts the connection between browser and server (the `S` in `HTTPS`).
- Analogy: sending a letter in a **locked box** instead of a postcard — anyone intercepting it sees gibberish.
- Prevents eavesdropping and tampering (e.g., someone on public Wi-Fi reading your password as it travels).
- **When to use:** always, everywhere, no exceptions in 2026 — it's free (Let's Encrypt) and mandatory for cookies marked `Secure`.

### Cookies
- Small key-value data the server asks the browser to store and resend on every request.
- Analogy: a **wristband** from a theme park — you don't re-show your ticket every ride, you just show the wristband.
- Example: `Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Strict`
  - `HttpOnly` → JavaScript can't read it (blocks theft via XSS)
  - `Secure` → only sent over HTTPS
  - `SameSite` → limits it being sent from other sites (blocks CSRF)

### Sessions
- Server-side memory of "who this cookie belongs to." The cookie is just a pointer/ID; the actual data (user ID, permissions) lives on the server.
- Analogy: the wristband (cookie) has a number; the **guest list at the front desk** (server session store) maps that number to your identity.
- **Secure sessions** = short expiry, regenerate session ID after login (prevents "session fixation"), invalidate on logout, store server-side (not sensitive data in the cookie itself).

---

## 2. Proving & Controlling Identity

### Authentication ("Who are you?")
- Verifying identity — username/password, fingerprint, OTP, Google login.
- Analogy: showing your **ID at hotel check-in**.
- Example: login form checks your password against a stored hash.

### Authorization ("What are you allowed to do?")
- Deciding what an *authenticated* user can access. Authentication and authorization are often confused — you can be logged in (authenticated) but still blocked from an admin page (not authorized).
- Analogy: your room key (you're a verified guest) doesn't open the manager's office.
- Example: a regular user hitting `/admin/delete-user` should get a `403 Forbidden` even though they're logged in.

### Password hashing
- Never store plaintext passwords. Hash them with a slow, salted algorithm (bcrypt, argon2, scrypt) — not MD5/SHA1, which are fast and crackable.
- Analogy: instead of keeping a copy of your house key, the hotel keeps a **one-way plaster mold** — useless to reverse, but they can check if a new key matches the mold shape.
- "Salt" = random data added per user so two people with the same password don't get the same hash (defeats precomputed "rainbow table" attacks).

### JWT (JSON Web Token)
- A self-contained, signed token carrying claims (e.g., `{userId: 5, role: "admin"}`), used instead of server-side sessions in many APIs.
- Analogy: a **sealed, tamper-evident wax-stamped letter** — anyone can read the contents (it's not encrypted, just signed), but they can't alter it without breaking the seal (signature).
- Common mistake: storing JWTs in `localStorage` (readable by JS → stealable via XSS). Prefer `HttpOnly` cookies for web apps.
- **When to use:** stateless APIs, mobile apps, microservices talking to each other. Not ideal when you need instant revocation (a stolen JWT is valid until it expires).

### Least privilege
- Every user, service, or process should have the *minimum* access needed — nothing more.
- Analogy: a hotel cleaner's key opens guest rooms on their floor, not the safe or every floor in the building.
- Example: your app's database user for the web server should not have `DROP TABLE` rights — only what the app actually needs (SELECT/INSERT/UPDATE on specific tables).

---

## 3. Moving Data

### APIs
- A defined contract for how programs talk to each other (endpoints, inputs, outputs).
- Analogy: a **restaurant menu** — you don't go into the kitchen, you order from defined options and get a defined result.
- Example: `POST /api/orders` with a JSON body creates an order.

### JSON
- The data format APIs mostly speak — lightweight, human-readable key-value/array structure.
- Example: `{"name": "Sam", "age": 27}`.
- Not inherently secure or insecure — but never trust JSON coming from the client without validating it server-side.

### SQL
- The language for querying relational databases (Postgres, MySQL, etc.).
- Example: `SELECT * FROM users WHERE id = 5;`
- Danger zone when user input gets pasted directly into a query string → SQL Injection (below).

---

## 4. The Attacks (and how each maps to a defense)

### SQL Injection
- Attacker sneaks SQL code into an input field so the database executes it.
- Example: login form takes `username = admin' --`. If the query is built as a string:
  `SELECT * FROM users WHERE username = '` + input + `'`
  the `--` comments out the password check → instant login bypass.
- **Prevention: parameterized queries / prepared statements** — the input is always treated as *data*, never as *code*.
  ```
  db.query("SELECT * FROM users WHERE username = ?", [input])
  ```
- Analogy: instead of letting a guest scribble anything on the guest-list form (which staff then read as instructions), you make them fill in a fixed-format box that can only ever mean "a name."

### XSS (Cross-Site Scripting)
- Attacker injects malicious JavaScript into a page that other users view.
- Example: a comment field that isn't escaped lets someone post `<script>stealCookies()</script>`, which runs in every visitor's browser.
- Types: **Stored** (saved in DB, hits everyone), **Reflected** (in a URL, hits whoever clicks the link), **DOM-based** (client-side JS mishandling).
- **Prevention: output encoding** (escape `<`, `>`, `&`, etc. before rendering user content) + `Content-Security-Policy` header + `HttpOnly` cookies so even if XSS happens, session cookies can't be stolen.

### CSRF (Cross-Site Request Forgery)
- Tricks a *logged-in* user's browser into submitting a request they didn't intend, using their existing session.
- Example: you're logged into your bank; a malicious site has `<img src="https://bank.com/transfer?to=attacker&amount=1000">` — your browser auto-sends your bank cookies with that request.
- Analogy: someone forges your **signature** and mails it using your own return-address envelope, which the recipient trusts because it "came from you."
- **Prevention:** CSRF tokens (a random value the form must include, unguessable by attackers), `SameSite=Strict/Lax` cookies, and checking the `Origin`/`Referer` header for sensitive actions.

### CORS (Cross-Origin Resource Sharing)
- Not an attack — it's a **browser security policy** that blocks a webpage from `site-A.com` from calling `api.site-B.com` unless site-B explicitly allows it.
- Analogy: a **bouncer's guest list** at api.site-B.com's door — "only requests from these approved origins get in."
- Example: your frontend at `app.mysite.com` calls your API at `api.mysite.com`. The API responds with `Access-Control-Allow-Origin: https://app.mysite.com` to permit it.
- Common mistake: setting `Access-Control-Allow-Origin: *` on an API that also uses cookies for auth — this can open the door too wide.
- **When you hit it:** you'll see it as a browser console error when building any frontend that calls a separate API domain.

---

## 5. Defensive Practices (the checklist side)

### Input validation
- Check that data is the *shape/type/range* you expect **before** using it — on the server, not just the client (client-side validation is just UX, trivially bypassed).
- Example: an "age" field should reject `"DROP TABLE users"` or `-5` before it ever touches your logic.
- Analogy: airport security checking your bag *before* you board, not trusting you to self-declare.

### Output encoding
- The counterpart to input validation: when you *display* data, encode it for the context it's going into (HTML, JS, URL, SQL).
- This is the actual fix for XSS — validation alone doesn't stop it if you later render raw HTML.

### Rate limiting
- Cap how many requests a user/IP can make in a time window.
- Analogy: a nightclub bouncer only letting people in one at a time, not the whole street rushing the door.
- Example: allow 5 login attempts per 15 minutes per IP → stops brute-force password guessing.

### Secure file uploads
- Never trust file type from the extension or `Content-Type` header (both attacker-controlled) — validate actual file content (magic bytes), restrict allowed types, rename files, store outside the web root or in object storage (S3), scan for malware, cap file size.
- Example: an "upload profile picture" feature that accepts a `.php` file renamed `photo.jpg.php` could let an attacker run code on your server if misconfigured.

### Error handling
- Don't leak internals in error messages shown to users. `Invalid username or password` (generic) — not `User not found` vs `Wrong password` (tells attacker which part was correct) and never a raw stack trace with file paths/DB schema.
- Analogy: front desk saying "that key doesn't work" rather than "that room exists but you typed the wrong PIN."

### Logging without sensitive information
- Log enough to debug/audit, but never log passwords, full card numbers, tokens, or personal data in plaintext.
- Example: log `user 4521 failed login attempt` not `user tried password "hunter2"`.
- Why it matters: logs are often less protected than the main DB and get shipped to third-party tools (Datadog, Sentry) — a leak there is still a leak.

### Dependency updates
- Most real-world breaches exploit **known** vulnerabilities in outdated libraries, not exotic zero-days (e.g., Log4Shell in 2021 hit millions of servers running an old logging library).
- Practice: automated tools like `npm audit`, Dependabot, or Snyk flag and patch vulnerable packages.
- Analogy: leaving a known-faulty lock on your door because "no one's broken in yet" — attackers scan the internet specifically for outdated software.

---

## How it all connects (mental model)

1. **HTTP/TLS** — the pipe data travels through (make it encrypted).
2. **Cookies/Sessions/JWT** — how the server remembers you across requests.
3. **Authentication** — proving who you are once.
4. **Authorization + Least privilege** — controlling what you can touch, every time after.
5. **Input validation + Output encoding** — the two-sided gate: don't trust data coming in, don't trust data going back out.
6. **SQLi / XSS / CSRF / CORS** — specific attacks that exploit gaps in #5 and #2, each with a matching, well-known defense.
7. **Rate limiting / secure uploads / error handling / logging / dependency updates** — the operational hygiene that keeps the whole system resilient over time, not just secure on day one.

If you remember nothing else: **never trust client-side data, encrypt everything in transit, hash passwords, and give every part of your system the least access it needs to do its job.** That handful of habits stops the vast majority of real-world breaches.
