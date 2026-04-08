# 🚀 Frontend Testing Guide

## ✅ Setup Complete

All API URLs have been updated to use the local backend (`http://localhost:5000`).

---

## 📋 Step-by-Step Testing Instructions

### **Step 1: Start Backend Server**

Open a terminal in the `server` folder:

```bash
cd D:\FeedX-main\server
npm run dev
```

**Expected Output:**
```
App Listening To Port 5000
```

✅ **Keep this terminal running!**

---

### **Step 2: Start Frontend Server**

Open **another terminal** in the `client` folder:

```bash
cd D:\FeedX-main\client
npm run dev
```

**Expected Output:**
```
  VITE v6.x.x  ready in xxx ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

✅ **Keep this terminal running!**

---

### **Step 3: Test in Browser**

1. Open your browser and go to: **http://localhost:5173**
2. You should see the FeedX landing page

---

## 🧪 Testing Flow

### **Test 1: Signup & Authentication**

#### **1.1 Signup as HR**
1. Click **"Sign Up"** or go to `http://localhost:5173/signup`
2. Fill in the form:
   - **Name:** Test HR User
   - **Email:** adityak00927@gmail.com
   - **Password:** TestHR@123
   - **Organisation:** TestCorp
3. Click **Sign Up**
4. ✅ You should see toast: "OTP sent to email. Verify to complete signup."
5. You'll be redirected to `/verify-otp`

#### **1.2 Verify OTP**
1. Check your email for the 6-digit OTP
2. Enter the OTP in the verification page
3. Click **Verify**
4. ✅ You should see toast: "Signup successful. You can now log in."
5. You'll be redirected to `/login`

#### **1.3 Login**
1. Go to `http://localhost:5173/login`
2. Enter credentials:
   - **Email:** adityak00927@gmail.com
   - **Password:** TestHR@123
3. Click **Login**
4. ✅ You should see toast: "Login successful! 🎉"
5. You'll be redirected to **HR Dashboard**

---

### **Test 2: HR Dashboard**

After logging in as HR:

1. ✅ **Dashboard loads** with stats (Employees, Managers, Goals, Feedback)
2. ✅ **Charts display** (Team Overview, Goals Status, Feedback Sentiment)
3. ✅ **AI Insights** section shows (may say "unavailable at the moment")
4. ✅ **Top Performer** card shows (if data exists)

**Navigation Tests:**
- Click **"Employees"** in sidebar → Should show employee list
- Click **"Managers"** in sidebar → Should show manager list
- Click **"Profile"** in sidebar → Should show profile section

---

### **Test 3: Create Manager & Employee (from HR)**

#### **3.1 Create Manager**
1. Go to HR Dashboard → **Managers**
2. Click **"Add Manager"** button
3. Fill in:
   - **Name:** Test Manager
   - **Email:** testmanager@feedx.com
   - **Password:** TestManager@123
   - **Organisation:** TestCorp
   - **Organisation ID:** 894c1978-78d0-41a3-87e7-1960162c0926
4. Click **Create**
5. ✅ You should see toast: "User created successfully!"

#### **3.2 Create Employee**
1. Go to HR Dashboard → **Employees**
2. Click **"Add Employee"** button
3. Fill in:
   - **Name:** Test Employee
   - **Email:** employeetest@feedx.com
   - **Password:** TestEmployee@123
   - **Organisation:** TestCorp
   - **Organisation ID:** 894c1978-78d0-41a3-87e7-1960162c0926
4. Click **Create**
5. ✅ You should see toast: "User created successfully!"

---

### **Test 4: Manager Dashboard**

#### **4.1 Login as Manager**
1. **Logout** from HR (click logout button)
2. Go to `/login`
3. Login with:
   - **Email:** testmanager@feedx.com
   - **Password:** TestManager@123
4. ✅ You should be redirected to **Manager Dashboard**

#### **4.2 Test Manager Features**
1. ✅ **Dashboard loads** with team stats
2. Click **"Employees"** → Should show Test Employee
3. Click **"Goals"** → Should be empty initially
4. Click **"Feedbacks"** → Should be empty initially

---

### **Test 5: Employee Dashboard**

#### **5.1 Login as Employee**
1. **Logout** from Manager
2. Go to `/login`
3. Login with:
   - **Email:** employeetest@feedx.com
   - **Password:** TestEmployee@123
4. ✅ You should be redirected to **Employee Dashboard**

#### **5.2 Test Employee Features**
1. ✅ **Dashboard loads** with personal stats
2. Click **"Goals"** → Should be empty initially
3. Click **"Feedbacks"** → Should be empty initially

---

## 🎯 Complete User Journey Test

### **Journey 1: Goal Assignment & Completion**

1. **Login as Manager**
2. Go to **Employees** → Click on Test Employee
3. **Assign a Goal:**
   - Title: Complete Q1 Report
   - Description: Prepare Q1 performance analysis
   - Deadline: 2026-04-30
4. ✅ Toast: "Goal assigned successfully"

5. **Logout & Login as Employee**
6. Go to **Goals**
7. ✅ See the assigned goal with status "Pending"
8. Click **"Request Completion"**
9. ✅ Toast: "Goal completion request sent"
10. Goal status changes to "Pending Approval"

11. **Logout & Login as Manager**
12. Go to **Goals**
13. ✅ See goal with status "Pending Approval"
14. Click **"Approve"**
15. ✅ Toast: "Goal marked as completed"
16. Goal status changes to "Completed"

---

### **Journey 2: Feedback Cycle**

1. **Login as HR**
2. Go to **Employees** → Click on Test Employee
3. Click **"Trigger Feedback"**
4. Enter Session Name: "Q1 Performance Review"
5. Click **Request**
6. ✅ Toast: "Feedback session triggered successfully!"

7. **Logout & Login as Manager**
8. Go to **Feedbacks**
9. ✅ See pending feedback request
10. Click on the request
11. **Fill out the feedback form:**
    - Answer all objective questions (1-5 ratings)
    - Answer subjective questions (text)
12. Click **Submit**
13. ✅ Toast: "Feedback submitted successfully"

14. **Logout & Login as HR**
15. Go to **Employees** → Click Test Employee
16. ✅ See feedback responses and analytics

---

## 🐛 Common Issues & Fixes

### **Issue 1: CORS Error**
**Error:** `Access to XMLHttpRequest blocked by CORS policy`

**Fix:** Check `server/server.js` CORS configuration includes `http://localhost:5173`

---

### **Issue 2: Token Expired**
**Error:** "Session expired, please log in again"

**Fix:** Login again to get a fresh token

---

### **Issue 3: Network Error**
**Error:** "Network Error" or request fails

**Fix:** 
1. Make sure backend is running on port 5000
2. Check browser console for errors (F12 → Console)

---

### **Issue 4: 401 Unauthorized**
**Error:** API returns 401

**Fix:** 
1. Logout and login again
2. Clear browser cookies and localStorage
3. Check if token is being stored correctly

---

## 📊 Testing Checklist

Use this checklist to track your testing:

- [ ] Backend server started on port 5000
- [ ] Frontend server started on port 5173
- [ ] Landing page loads successfully
- [ ] Signup page loads and submits
- [ ] OTP email received
- [ ] OTP verification works
- [ ] Login redirects to correct dashboard based on role
- [ ] HR Dashboard loads with charts
- [ ] HR can create Manager
- [ ] HR can create Employee
- [ ] HR can view employee/manager lists
- [ ] Manager Dashboard loads
- [ ] Manager can view employees
- [ ] Manager can assign goals
- [ ] Manager can approve goals
- [ ] Manager can view/submit feedback
- [ ] Employee Dashboard loads
- [ ] Employee can view goals
- [ ] Employee can request goal completion
- [ ] Employee can view feedback requests
- [ ] Logout works and redirects to login
- [ ] Protected routes redirect to login when not authenticated

---

## 🔍 Debugging Tips

### **Browser Console (F12)**
- Check for JavaScript errors
- View network requests (Network tab)
- Check localStorage for auth token

### **Backend Terminal**
- Watch for incoming API requests
- Check for error messages
- Verify database connections

### **Frontend Terminal**
- Watch for Vite build errors
- Check for import errors

---

## 📸 What to Share If Issues Occur

1. **Screenshot of the error**
2. **Browser console errors** (F12 → Console tab)
3. **Network request details** (F12 → Network tab → click failed request)
4. **Backend terminal output**
5. **Frontend terminal output**

---

**Ready to test? Start both servers and let me know the results!** 🚀
