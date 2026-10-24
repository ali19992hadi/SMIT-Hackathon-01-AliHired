# AliHired — HCM Lite

**Hire Smart. Grow Faster.**

AliHired is a small Human Capital Management web app built as a lightweight hiring & HR workspace. An **Admin** runs the company and HR work; **Employees** manage their own attendance, leave and payslips. It uses only **HTML, CSS and vanilla JavaScript**, with all data stored in the browser's **localStorage**. There is no backend and no cloud database.

**Look & feel:** light workspace with an ink top navigation bar, teal and saffron accents, and a logo of a briefcase with a check mark.

## Features
- First-run company setup and Admin account creation
- Login, logout, change password, role-based page protection
- Employee management: add (creates the login), edit, delete, search, filter by department
- Attendance: check-in / check-out with Present, Late, Half Day and Absent rules
- Leave: apply, approve / reject with a comment, working-day counting, overlap and quota checks, balance
- Company settings: office start time, grace minutes, yearly leave quota, working days
- Admin dashboard and Employee dashboard calculated from saved data
- Monthly payslip with print / save as PDF
- Smart Helper (keyword matching on real data, restricted by role)
- Announcements (Admin posts, Employees read)
- Responsive layout that works on phones

## Folder structure
```
AliHired/
├── index.html            Login + first-run setup + change password
├── pages/
│   ├── admin.html        Admin dashboard
│   ├── employees.html    Employee management
│   ├── leaves.html       Leave requests
│   ├── settings.html     Company settings
│   ├── employee.html     Employee dashboard
│   └── payslip.html      Monthly payslip
├── css/style.css
├── js/
│   ├── storage.js        localStorage helpers + small utilities
│   ├── auth.js           setup, login, logout, password, page guard
│   ├── employees.js      employee CRUD, search, filter
│   ├── attendance.js     check-in/out, status, hours, summaries
│   ├── leave.js          leave rules, approval, balance
│   └── helper.js         Smart Helper
├── assets/alihired_logo.svg   (briefcase + check logo)
└── README.md
```

## How to run
1. Download or clone the repository.
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox, Safari). No install or build step is needed.

Data lives in that one browser only. To start over, clear the site's localStorage (browser dev tools → Application → Local Storage).

## First-run setup
The app starts empty. On first open you will see **Set up your company**: enter the company name and create the Admin account (name, email, password). You are then taken to the Admin dashboard.
New companies start with these defaults, which the Admin can change in **Settings**: start time 09:00, 15 grace minutes, 20 leave days per year, working days Monday–Friday.

## Using the app
**Admin**
1. *Settings* — set start time, grace minutes, yearly leave quota and working days.
2. *Employees* — add employees (name, email, password, department, salary, role). The employee can log in straight away. Edit, delete, search or filter by department.
3. *Leave requests* — approve or reject with an optional comment.
4. *Dashboard* — totals, today's attendance, pending leave, announcements and the Smart Helper.
5. *Payslips* — pick an employee and month.

**Employee**
1. Log in with the email and password the Admin created.
2. Check in and check out from *My dashboard*, apply for leave, and see your attendance, leave status and balance.
3. *My payslip* shows only your own payslip. Use *Change password* in the sidebar to update your password.

## Rules
**Attendance**
- One check-in per employee per working day; check-out only after check-in.
- Late = check-in after office start + grace minutes (09:00 + 15 → 09:15 is Present, 09:16 is Late).
- Work hours = check-out − check-in. Under 5 hours = Half Day.
- No record on a past working day = Absent. Non-working days are never absent. Today is not counted absent until the day is over.
- Days covered by approved leave show as On Leave and are not counted absent.

**Leave**
- From date must be on or before To date. Only working days are counted (weekends / non-working days are skipped).
- A request cannot overlap another pending or approved request, and cannot exceed the remaining yearly quota.
- Balance = quota − approved leave days. Pending and rejected requests do not reduce it; the quota is checked again when the Admin approves.

**Payslip**
```
Per-day salary = monthly salary ÷ working days in the month
Deduction      = per-day salary × (absent days + 0.5 × half days)
Net salary     = monthly salary − deduction
```
No taxes, bonuses or other payroll items are included.

## Smart Helper
Plain JavaScript keyword matching over saved data, with no external AI service.
- Admin: "How many employees do we have?", "Who is on leave today?", "Who was late today?"
- Employee: "How many leaves do I have left?", "Was I late today?", "How many hours did I work this week?"

Employees only receive their own data; company-wide questions are declined.

## localStorage keys
| Key | Stores |
|---|---|
| `alh_company` | company name, start time, grace minutes, leave quota, working days (and announcements) |
| `alh_users` | id, name, email, password, role, department, salary (and joined date) |
| `alh_attendance` | employee id, date, check-in, check-out, status |
| `alh_leaves` | employee id, type, from, to, days, reason, status, comment |
| `alh_session` | who is logged in |

Passwords are stored as plain text in localStorage because this is a learning project with no server. Do not use real passwords.

## Hosting on GitHub Pages
1. Create a GitHub repository and push the `AliHired` folder contents (so `index.html` is at the repository root).
2. Repository **Settings → Pages**: choose the `main` branch and `/ (root)`, then save.
3. Open the published link (`https://<user>.github.io/<repo>/`). Netlify or Vercel also work by uploading the same folder.
