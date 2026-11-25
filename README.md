# 🎓 EduPortal – Student Management Portal

A single-file, front-end student portal with separate **Student** and **Admin** experiences. It runs as a static page (no build step) and can optionally connect to [Supabase](https://supabase.com) for authentication and data storage.

## Features

### Student
- **Dashboard** – welcome screen, fee progress, latest notices
- **My Profile** and **Academic Details** – overview, current-semester subjects, examination details
- **My Subjects** – subject list for the current semester
- **Attendance** – semester-wise (1st–7th) and subject-wise breakdowns
- **Fee Details** – payment progress, payment history, demo payment flow, downloadable/printable **fee receipt**
- **Examination** – exam schedule and semester examination status
- **Notice Board** – read notices posted by the admin
- **Settings** – light/dark theme, change password
- Global search across pages and subjects

### Admin
- Admin dashboard with a **students** list
- **Edit student profiles**
- **Post notices** to the notice board
- Review payments

### General
- Light and dark themes (remembered per browser)
- Responsive layout, safe-area aware for phones
- Password reset flow

## Tech stack

| Layer | Choice |
|---|---|
| UI | Plain HTML, CSS and vanilla JavaScript (one file) |
| Backend (optional) | Supabase (`@supabase/supabase-js` 2.45.4, loaded from jsDelivr) |
| Hosting | Any static host (Vercel, Netlify, GitHub Pages, …) |

## Project structure

```
.
├── index.html   # the whole app (rename s_t2.html to index.html)
└── README.md
```

## Run locally

No install needed. Either open `index.html` in a browser, or serve it:

```bash
# Python
python3 -m http.server 3000

# or Node
npx serve .
```

Then visit `http://localhost:3000`.

## Demo logins

When Supabase is not configured, the app runs in demo mode with built-in sample data:

| Role | Username | Password |
|---|---|---|
| Student | `ADM2026001` | `student123` |
| Admin | `admin` | `admin123` |

> ⚠️ Demo credentials are for testing only. Remove them before using the portal with real student data.

## Supabase setup (optional)

Near the top of the script, set your project values:

```js
const SUPABASE_URL = "https://YOUR-PROJECT.supabase.co";
const SUPABASE_ANON_KEY = "YOUR-ANON-PUBLIC-KEY";
```

The app uses Supabase Auth (email/password sign-in, sign-out, password update, session restore) and these tables:

- `profiles` – student/admin profile records
- `payments` – fee payment records
- `notices` – notice board posts

Checklist:
1. Create the three tables above (match the fields the app reads/writes).
2. **Enable Row Level Security** on every table and add policies. Students should only read their own `profiles` and `payments`; only admins should write `notices` and edit other profiles.
3. In **Authentication → URL Configuration**, add your deployed URL (Site URL and Redirect URLs) so login and password-reset links work.

> The anon key is designed to be public, but it is only safe when Row Level Security is properly configured. Never put the `service_role` key in this file.

## Deploy to Vercel

**Dashboard**
1. Rename `s_t2.html` to `index.html` and put it in a folder (or Git repo).
2. Go to [vercel.com/new](https://vercel.com/new) and import the repo or drag the folder in.
3. Framework preset: **Other**. Leave the build command empty and click **Deploy**.

**CLI**
```bash
npm i -g vercel
vercel          # preview
vercel --prod   # production
```

The same folder also works on Netlify, Cloudflare Pages and GitHub Pages.

## Notes and limitations

- Payments are a **demo**: no real money is charged and no payment gateway is connected.
- Receipts use the browser's print dialog (save as PDF from there).
- Theme and session preferences are stored in the browser's `localStorage`/`sessionStorage`.
- Depends on the jsDelivr CDN for the Supabase client; the page needs internet access for that script.

## License

Add a license of your choice (e.g. MIT) before publishing.
