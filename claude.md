# Claude Code Session - BYU AI Bootcamp

## Session Overview
This document summarizes the work completed during the BYU AI Bootcamp workshop session using Claude Code. We built a simple to-do list web application and deployed it to Vercel via GitHub.

## Project Built: Simple To-Do List Web Application

A clean, modern to-do list application built with vanilla HTML, CSS, and JavaScript.

### Features
- Add new tasks
- Mark tasks as complete/incomplete with checkboxes
- Delete tasks
- Persistent storage using browser localStorage
- Task statistics (total, active, completed)
- Responsive design with purple gradient background
- Clean, modern UI

### Tech Stack
- HTML5
- CSS3
- Vanilla JavaScript
- localStorage API (for data persistence)

## Setup Attempts

### Gemini CLI Installation (Failed)
**Attempted**: Install Google's Gemini CLI tool
```bash
npm install -g @google/gemini-cli
```

**Issue**: The Gemini CLI requires Node.js version 20 or higher. Current system has Node.js v18.12.1.

**Error**: SyntaxError due to unsupported regex flags in Node.js 18

**Resolution Required**:
- Upgrade Node.js to version 20+ from https://nodejs.org
- Or use a Node version manager like `nvm-windows`

**Status**: Postponed until Node.js upgrade

## Key Steps Taken

### 1. Built To-Do List Application
Created `todo-list.html` (later renamed to `index.html`) with:
- Input field for adding tasks
- Add button and Enter key support
- Task list with checkboxes and delete buttons
- Statistics footer
- Object-oriented JavaScript using a TodoApp class
- localStorage integration for persistence

### 2. Set Up Git Repository
```bash
git init
```

### 3. Created Supporting Files

**`.gitignore`**
- Excluded node_modules, OS files, IDE configs, and Vercel files

**`README.md`**
- Project documentation
- Features list
- Tech stack information
- Deployment instructions

### 4. Committed to Git
```bash
git add .
git commit -m "Add simple to-do list web application"
```

### 5. Pushed to GitHub
```bash
git remote add origin https://github.com/robertkeele/aibootcamp.git
git push -u origin master
```

### 6. Deployed to Vercel
Connected the GitHub repository to Vercel for automatic deployment.

## Troubleshooting

### Issue 1: Gemini CLI Won't Run
**Problem**: After installing Gemini CLI, running `gemini --version` produced a SyntaxError

**Root Cause**: Node.js v18.12.1 is too old. Gemini CLI requires Node.js 20+

**Solution**: Upgrade Node.js to version 20 or higher

### Issue 2: Vercel 404 Error
**Problem**: After deploying to Vercel, received error:
```
404: NOT_FOUND
Code: NOT_FOUND
ID: cle1::kmx27-1762897299665-0f030fa0728d
```

**Root Cause**: Vercel looks for `index.html` as the default entry point, but our file was named `todo-list.html`

**Solution**: Renamed the file to `index.html`
```bash
git mv todo-list.html index.html
git commit -m "Rename todo-list.html to index.html for Vercel deployment"
git push
```

**Result**: Vercel automatically redeployed with the new commit and the app now works correctly

## File Structure

```
BYUWorkshop/
├── .git/                      # Git repository
├── .gitignore                 # Git ignore rules
├── README.md                  # Project documentation
├── index.html                 # Main to-do list application
├── notes.txt                  # Workshop setup notes
└── claude.md                  # This session summary
```

## How to Run Locally

### Method 1: Open Directly in Browser
Simply double-click `index.html` or run:
```bash
start index.html
```
The app will open using the `file://` protocol and work perfectly.

### Method 2: Local HTTP Server (Optional)
If you prefer running a local server:
```bash
npx http-server -p 8080
```
Then visit `http://localhost:8080`

## How to Deploy to Vercel

### Initial Deployment
1. Go to https://vercel.com
2. Sign in with your GitHub account
3. Click "Add New Project"
4. Import the `robertkeele/aibootcamp` repository
5. Vercel auto-detects it's a static site
6. Click "Deploy"

### Continuous Deployment
After initial setup, every push to the GitHub repository automatically triggers a new deployment on Vercel.

### Important: Entry Point
Vercel requires an `index.html` file in the root directory as the default entry point.

## Repository Information

**GitHub Repository**: https://github.com/robertkeele/aibootcamp.git
**Branch**: master

## Next Steps & Enhancements

### Potential Features to Add
- Edit existing tasks (double-click to edit)
- Due dates for tasks
- Priority levels (high, medium, low)
- Categories or tags
- Filter tasks (all, active, completed)
- "Clear completed" button
- Dark mode toggle
- Export/import tasks
- Multiple lists/projects

### Technical Improvements
- Add unit tests
- Implement drag-and-drop reordering
- Add animations for task additions/deletions
- Progressive Web App (PWA) capabilities
- Sync across devices (using a backend)

## Tools & Accounts Set Up

From `notes.txt`:
- ✅ Claude Pro account
- ✅ GitHub account
- ✅ Supabase account
- ✅ Vercel account
- ✅ VS Code installed
- ✅ Claude Code CLI installed
- ⏳ Gemini CLI (pending Node.js upgrade)
- ⏳ GitHub Copilot (optional)
- ⏳ Codex (optional)

## Session Stats

**Files Created**: 4 (index.html, README.md, .gitignore, claude.md)
**Git Commits**: 2
**Issues Resolved**: 1 (Vercel 404)
**Issues Identified**: 1 (Gemini CLI Node.js version)

---

**Generated with Claude Code**
BYU AI Bootcamp Workshop - 2025-11-11
