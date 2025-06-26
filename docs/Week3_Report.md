Week 3 Advanced Security & Final Reporting
Project: User Management System
Local Instance: http://localhost:3000

1. Basic Penetration Testing
•	Ran Nmap: only expected ports (3000 app, Windows/VMware services)
•	Closed OWASP ZAP proxy after testing
•	Tried bad URLs and form fuzzing—no new crashes
•	Verified no stack traces leak
2. Basic Logging with Winston
•	Installed winston
•	Created security.log and console logger
•	Logged app start, successful logins, registration errors, and uncaught errors
Simple Security Best-Practices Checklist
1.	Check and clean all inputs
o	Make sure emails are real (e.g. validator.isEmail)
o	Strip out or escape harmful characters from names, comments, etc.
2.	Hash and salt passwords
o	Always run passwords through bcrypt before saving them.
3.	Protect sensitive routes
o	Only allow logged-in users (or admins) to access their pages.
4.	Use CSRF tokens on forms
o	Add a hidden token to each form so other sites can’t trick your users.
5.	Send security headers
o	Use a library like Helmet to set headers (CSP, clickjacking protection, HSTS, nosniff).
6.	Harden cookies
o	Mark cookies as HttpOnly (no JavaScript), Secure (HTTPS only), SameSite=Strict.
7.	Hide framework details
o	Turn off the header that tells everyone you’re using Express (or your framework).
8.	Log key events
o	Keep a log of starts, logins, errors, and unusual actions.
9.	Scan your app locally
o	Use a tool like Nmap to see what ports are open; close anything you don’t need.
10.	Enforce HTTPS
o	Only serve the site over TLS so data in-transit is encrypted.
11.	Keep a handy checklist
o	Review this list anytime you add new features or deploy updates.

Part 1: Results by Week
Week 1: Security Assessment
•	Visiting /admin without logging in let you see the admin dashboard (authorization bypass).
•	None of the forms included a CSRF token (CSRF risk).
•	OWASP ZAP reported that key headers were missing: CSP, X-Frame-Options, HSTS, X-Content-Type-Options.
•	Responses revealed X-Powered-By: Express.
•	EJS templates still had debug comments.
•	The password-length check (&& instead of ||) let bad passwords through.
•	The isAdmin field was a String, even though the controller treated it as a Boolean.
•	Flash messages were overly specific (e.g. “User already exists”).
Week 2: Feature Enhancements
•	Short passwords were correctly rejected in both user and admin signup.
•	Emails were validated server-side (validator.isEmail).
•	All admin routes are now guarded by isAdminLoggedIn.
•	On login, the app now returns a JSON Web Token instead of or in addition to a redirect.
•	Helmet was added and now sends CSP, frameguard, HSTS, nosniff, and other security headers.
•	Session cookies are marked HttpOnly, Secure (when HTTPS), and SameSite=Strict.
•	X-Powered-By was disabled.
•	isAdmin in the database schema is now a Boolean.
•	CSRF tokens were added to every form.
•	Winston logging captures app start, user logins, registration failures, and uncaught errors.
Week 3: Basic Penetration & Logging
•	Nmap scans showed only expected ports—3000 (Node/Express), plus standard Windows/VMware services. ZAP proxy (8080) was closed when not in use.
•	Browser fuzzing of URLs and form inputs produced no new crashes or leaked stack traces.
•	A security.log file now records critical events for later review.
•	A concise security checklist was drafted to keep best practices front of mind.
________________________________________
Part 2: Fixes Implemented
1.	Route Protection: Added isAdminLoggedIn middleware to all admin routes.
2.	CSRF Mitigation: Installed and configured csurf to include tokens in forms.
3.	Security Headers: Added app.use(helmet()) to send CSP, frameguard, HSTS, and nosniff.
4.	Hide Framework Info: Disabled the X-Powered-By header via app.disable('x-powered-by').
5.	Clean Up HTML (Planned): Remove all debug comments from EJS templates.
6.	Password-Length Logic: Changed the check to pwd.length < 8 || pwdConf.length < 8.
7.	Email Validation: Added validator.isEmail(email) before any database lookup.
8.	Password Hashing: Ensured every password is hashed with bcrypt before saving.
9.	Schema Correction: Changed isAdmin to a Boolean field with default false.
10.	Error-Message Generalization (Planned): Will replace specific flash text with “Invalid credentials” or “Operation failed.”
11.	Token-Based Auth: On user login, generate and return a JWT (jwt.sign({ id }, JWT_SECRET)).
12.	Basic Logging: Integrated Winston to record startup, logins, registration errors, and unexpected exceptions.
