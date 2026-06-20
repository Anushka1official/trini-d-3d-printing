TRINI-D ADMIN FIREBASE CLOUD SYNC
=================================

This version connects the /admin panel to Firebase Authentication + Cloud Firestore.

What it does:
- Admin login uses Firebase Email/Password.
- Items, Orders, Quotations, Bills/Invoices, Budget, and Custom records are saved to Firestore.
- Other opened admin pages update automatically in realtime.
- A local browser cache is kept as a fallback.
- JSON export/import still works. Imported JSON or Desktop SQLite data is uploaded to Firebase automatically after import.

Firebase project used:
- projectId: trini-d-3d-printing
- Cloud document path: trinid/default

Required Firebase setup:
1. Authentication > Sign-in method > Email/Password must be enabled.
2. Authentication > Users must contain your admin email/password.
3. Authentication > Settings > Authorized domains must include:
   anushka1official.github.io
4. Firestore Rules should be:

rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /trinid/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}

How to update GitHub Pages:
1. Upload/replace all files from this folder to your repository root.
2. Make sure /admin/firebase-config.js, /admin/index.html, /admin/admin.js, and /admin/admin.css are uploaded.
3. Wait 2-5 minutes for GitHub Pages.
4. Open:
   https://anushka1official.github.io/trini-d-3d-printing/admin/

Important:
- The first signed-in admin page will create the Firestore cloud document if it does not exist.
- If you import the desktop SQLite database from the admin page, it will sync to Firebase cloud.
- If many years of records grow too large, the database should later be split into separate Firestore collections. This version stores the admin database as one Firestore document for simplicity and easy migration from the current localStorage version.
