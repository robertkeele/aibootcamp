# Supabase Setup Guide for To-Do List App

## Step-by-Step Instructions

### Step 1: Create Your Supabase Project

1. Go to https://supabase.com
2. Sign in with your account
3. Click **"New Project"**
4. Fill in the details:
   - **Name**: `todo-list` (or your preferred name)
   - **Database Password**: Create a strong password and **SAVE IT!**
   - **Region**: Choose the closest region to you
5. Click **"Create new project"**
6. Wait ~2 minutes for provisioning to complete

---

### Step 2: Create the Database Table

1. Once your project is ready, click **"SQL Editor"** in the left sidebar
2. Click **"New Query"**
3. Copy and paste this SQL code:

```sql
-- Create todos table
CREATE TABLE todos (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  task text NOT NULL,
  completed boolean NOT NULL DEFAULT false,
  created_at timestamptz DEFAULT now(),
  updated_at timestamptz DEFAULT now()
);

-- Enable Row Level Security (RLS)
ALTER TABLE todos ENABLE ROW LEVEL SECURITY;

-- Create policy to allow all operations (for now, without authentication)
CREATE POLICY "Allow all operations" ON todos
  FOR ALL
  USING (true)
  WITH CHECK (true);

-- Create function to auto-update updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
   NEW.updated_at = now();
   RETURN NEW;
END;
$$ language 'plpgsql';

-- Create trigger to auto-update updated_at
CREATE TRIGGER update_todos_updated_at
  BEFORE UPDATE ON todos
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();
```

4. Click **"Run"** (or press `Ctrl+Enter`)
5. You should see: "Success. No rows returned"

---

### Step 3: Get Your API Credentials

1. Click the **Settings** icon (gear icon) in the left sidebar
2. Click **"API"** in the Configuration section
3. You'll see two important values:

   **Project URL**
   - Looks like: `https://xxxxxxxxxxxxx.supabase.co`
   - Copy this value

   **anon public key**
   - Long string starting with `eyJ...`
   - Click the copy icon to copy it

---

### Step 4: Add Credentials to Your App

1. Open `index.html` in your editor
2. Find these lines near the top of the `<script>` section (around line 179-180):

```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL_HERE';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY_HERE';
```

3. Replace them with your actual credentials:

```javascript
const SUPABASE_URL = 'https://xxxxxxxxxxxxx.supabase.co';  // Your Project URL
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';  // Your anon key
```

4. Save the file

---

### Step 5: Test Your Application

1. If your local server is running, refresh the page
2. If not, start it with:
   ```bash
   npx http-server -p 8080
   ```
3. Open http://localhost:8080 in your browser
4. Try adding a task - it should now save to Supabase!
5. Open the browser console (F12) to check for any errors

---

## Verifying It Works

### Check Tasks in Supabase

1. Go back to your Supabase project
2. Click **"Table Editor"** in the left sidebar
3. Click on the **"todos"** table
4. You should see your tasks listed there!

### Test Features

- **Add a task** - Should appear immediately
- **Mark as complete** - Checkbox should persist
- **Delete a task** - Should remove from database
- **Refresh the page** - Tasks should remain (no localStorage anymore!)
- **Open in another browser** - Tasks should sync across devices!

---

## What Changed from localStorage?

| Feature | localStorage (Old) | Supabase (New) |
|---------|-------------------|----------------|
| **Storage** | Browser only | Cloud database |
| **Persistence** | Per browser/device | Across all devices |
| **Data sync** | None | Real-time capable |
| **Scalability** | ~5-10MB limit | Unlimited* |
| **Security** | Client-side only | Server-side RLS |

---

## Troubleshooting

### Error: "Failed to load tasks from database"

**Check:**
1. Are your credentials correct in `index.html`?
2. Did you run the SQL to create the table?
3. Check browser console (F12) for detailed error messages

### Error: "Failed to add task"

**Check:**
1. Is your Supabase project running? (Check dashboard)
2. Did you enable the RLS policy?
3. Check Network tab in browser DevTools for failed requests

### Tasks don't appear

**Check:**
1. Open Supabase Table Editor
2. Verify the table exists and has the correct columns
3. Try adding a task directly in Table Editor to test

---

## Database Schema Reference

```
Table: todos
├── id (uuid, primary key, auto-generated)
├── task (text, required)
├── completed (boolean, default: false)
├── created_at (timestamp, auto-set)
└── updated_at (timestamp, auto-updated)
```

---

## Next Steps

After basic functionality works:

1. **Add user authentication** - Use Supabase Auth to have per-user tasks
2. **Real-time subscriptions** - See changes instantly across devices
3. **Add categories/tags** - Create related tables
4. **Advanced queries** - Search, filter, sort tasks
5. **Deploy to Vercel** - Push to GitHub and redeploy

---

## Security Note

The current setup allows **anyone** with your URL to read/write tasks. This is fine for learning, but for production:

1. Enable Supabase Authentication
2. Update RLS policies to restrict access:
   ```sql
   -- Only allow users to see their own tasks
   CREATE POLICY "Users can only see own tasks" ON todos
     FOR SELECT
     USING (auth.uid() = user_id);
   ```
3. Add `user_id` column to todos table

---

**Questions?** Check the Supabase docs: https://supabase.com/docs
