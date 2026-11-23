# UserLock

A Power BI custom visual that shows an email + password sign-in. After a correct sign-in it filters the bound Email column to that one person, so the rest of the page shows only their data.

> **This is a convenience lock, not security.** Password hashes are stored in the report, and anyone with edit access can bypass it. Never use it with "Publish to web". Use Row-Level Security for confidential data.

## Fields
- **Email**: your email column (for example `Access[Email]`)
- **Password hash**: the matching hash column
- **Full name** (optional): shown after sign-in

Hash = `SHA-256( salt + ":" + lowercase(email) + ":" + password )`. Set your own salt under Format, Lock settings and never change it afterwards.

## Use
1. Import `UserLock.pbiviz` into your report.
2. Make an Access table (Email, PasswordHash) related one-to-many to your people table.
3. Employees generate a code with "First time? Create password" and send it to the admin, who adds it to Access.
4. Add an `Unlocked` measure and use it as a visual-level filter ("is 1") on protected visuals:

```DAX
Unlocked =
IF ( ISFILTERED ( Access[Email] ) && HASONEVALUE ( Access[Email] ), 1, 0 )
```

## Build
```
npm install
pbiviz package
```

## License
MIT
