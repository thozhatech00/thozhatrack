# ThozhaTrack

Internal daily work tracking portal for Thozha Tech.

This app allows:

- Admin login
- Admin can create many users/interns
- Admin can assign tasks
- Users/interns can view their tasks
- Users/interns can submit daily work updates
- Admin can review and add comments
- Admin can mark updates as approved or rework required

## Tech Stack

- Frontend: Next.js
- Authentication: Supabase Auth
- Database: Supabase PostgreSQL
- Backend APIs: Next.js API routes
- Hosting: Vercel or any Node hosting
- Domain suggestion: `app.thozhatech.in`

## Important Security Note

The `SUPABASE_SERVICE_ROLE_KEY` is used only in server-side API routes for admin actions like creating users. Never expose this key in the browser and never commit `.env.local` to GitHub.

## 1. Create Supabase Project

1. Go to Supabase.
2. Create a new project.
3. Project name suggestion: `thozhatrack`.
4. Save the database password safely.

## 2. Create Database Tables

Open:

```text
Supabase Dashboard → SQL Editor
```

Run the SQL from:

```text
supabase/schema.sql
```

## 3. Enable Email Authentication

Open:

```text
Supabase Dashboard → Authentication → Providers → Email
```

Enable Email provider.

For simple internal setup, you can disable email confirmation during testing. For production, enable confirmation if required.

## 4. Create First Admin Manually

Only the first admin must be created manually. After that, admin can create all users from the app.

### Step 4.1 Create auth user

Open:

```text
Supabase Dashboard → Authentication → Users → Add User
```

Create:

```text
Email: admin@thozhatech.in
Password: your secure password
Auto confirm user: Yes
```

Copy the new user's UUID.

### Step 4.2 Insert admin profile

Open SQL Editor and run this after replacing the UUID:

```sql
insert into public.profiles (id, full_name, email, role, status)
values (
  'PASTE_ADMIN_AUTH_USER_UUID_HERE',
  'Sathish Kumar',
  'admin@thozhatech.in',
  'admin',
  'active'
);
```

## 5. Configure Environment Variables

Copy:

```bash
cp .env.example .env.local
```

Fill the values:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-public-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key-server-only
NEXT_PUBLIC_APP_NAME=ThozhaTrack
NEXT_PUBLIC_ALLOWED_EMAIL_DOMAIN=thozhatech.in
```

Where to get values:

```text
Supabase Dashboard → Project Settings → API
```

Use:

- Project URL
- anon public key
- service_role key

## 6. Run Locally

```bash
npm install
npm run dev
```

Open:

```text
http://localhost:3000
```

Login using the first admin account.

## 7. Admin Flow

After login as admin:

```text
Admin Dashboard → Users → Create User
```

Create intern/user:

```text
Full Name
Email
Temporary Password
Role: User / Intern
Status: Active
```

Then:

```text
Admin Dashboard → Tasks → Assign Task
```

## 8. User / Intern Flow

Intern logs in and can:

```text
View assigned tasks
Change task status
Submit daily update
View admin feedback
```

## 9. Deploy to Vercel

1. Push this project to GitHub.
2. Import GitHub repo in Vercel.
3. Add all environment variables in Vercel Project Settings.
4. Deploy.
5. Add custom domain:

```text
app.thozhatech.in
```

## 10. Suggested DNS

In your DNS provider, create:

```text
Type: CNAME
Name: app
Value: cname.vercel-dns.com
```

Follow the exact DNS value shown by Vercel if it gives a different value.

## Current MVP Scope

Included now:

- Login
- Role based admin/user pages
- Admin user creation
- Task assignment
- Daily work update
- Admin review/comments

Can be added later:

- File attachments
- Email notifications
- Weekly PDF report
- Leave/attendance
- Password change page
- Zoho OAuth login
