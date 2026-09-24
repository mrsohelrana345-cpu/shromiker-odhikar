# শ্রমিকের অধিকার — বাস্তব সিস্টেমের Starter

## কী আছে
- শ্রমিক অভিযোগ ফর্ম
- Complaint ID
- SQLite database
- JPG/PNG/PDF evidence upload (5MB)
- অভিযোগ tracking
- JWT protected admin login
- Admin status update
- Basic security headers + rate limiting

## চালানো
1. Node.js 18+ ইনস্টল করুন।
2. Terminal খুলে project folder-এ যান।
3. `npm install`
4. Production-এর আগে environment variables দিন:
   - `JWT_SECRET` = দীর্ঘ random secret
   - `ADMIN_USER`
   - `ADMIN_PASSWORD`
5. `npm start`
6. Browser: `http://localhost:3000`

## গুরুত্বপূর্ণ
এটি production-ready legal case-management system নয়। Public launch-এর আগে:
- HTTPS
- strong admin password + MFA
- encrypted backups
- access logging
- secure file scanning
- privacy/consent policy
- data retention/deletion policy
- abuse/spam protection
- legal review
- authorized complaint-handling workflow
যোগ করুন।

প্রকাশ্যে evidence files serve করা হয়নি; এই starter-এ uploads private server directory-তে থাকে।
