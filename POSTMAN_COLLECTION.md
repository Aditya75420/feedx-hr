# 📦 FeedX HR API - Postman Collection

> **Base URL:** `http://localhost:5000`  
> **Testing Date:** April 8, 2026  
> **Status:** ✅ 25/30 Routes Tested Successfully

---

## 🔑 Test Users (Credentials)

| Role | Email | Password | User ID |
|------|-------|----------|---------|
| **HR** | `adityak00927@gmail.com` | `TestHR@123` | `69d55d1dfd9921aca419b7df` |
| **Manager** | `testmanager@feedx.com` | `TestManager@123` | `69d55ea6fd9921aca419b7ef` |
| **Employee** | `employeetest@feedx.com` | `TestEmployee@123` | `69d5601afd9921aca419b801` |

**Organisation ID:** `894c1978-78d0-41a3-87e7-1960162c0926`

---

## 📋 Authentication Setup

For all authenticated requests, add this header:

```
Authorization: Bearer <YOUR_TOKEN>
Content-Type: application/json
```

---

## 1️⃣ AUTH ROUTES (`/api/auth`)

### 1.1 Signup
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/auth/signup`
- **Body (JSON):**
```json
{
  "name": "Test HR User",
  "email": "YOUR_EMAIL@gmail.com",
  "password": "TestHR@123",
  "organisation": "TestCorp"
}
```
- **Expected Response:** `200 OK` - "OTP sent to email. Verify to complete signup."
- **Notes:** Requires real email to receive OTP

---

### 1.2 Verify OTP
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/auth/verify-otp`
- **Body (JSON):**
```json
{
  "email": "YOUR_EMAIL@gmail.com",
  "otp": "XXXXXX"
}
```
- **Expected Response:** `200 OK` - "Signup successful. You can now log in."

---

### 1.3 Login
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/auth/login`
- **Body (JSON):**
```json
{
  "email": "adityak00927@gmail.com",
  "password": "TestHR@123"
}
```
- **Expected Response:** `200 OK` - Returns user object and JWT token
- **Response Example:**
```json
{
  "message": "Login successful",
  "user": {
    "_id": "69d55d1dfd9921aca419b7df",
    "name": "Test HR User",
    "email": "adityak00927@gmail.com",
    "role": "hr",
    "organisation": "TestCorp",
    "organisationId": "894c1978-78d0-41a3-87e7-1960162c0926",
    "isVerified": true
  },
  "token": "eyJhbGci..."
}
```

---

### 1.4 Verify Session
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/auth/verify-session`
- **Headers:**
  - `Authorization: Bearer <TOKEN>`
- **Expected Response:** `200 OK` - Session validity confirmation

---

### 1.5 Logout ⏭️ (Not Tested)
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/auth/logout`
- **Headers:**
  - `Authorization: Bearer <TOKEN>`
- **Expected Response:** `200 OK` - "Logged out successfully"

---

### 1.6 Resend OTP ⏭️ (Not Tested)
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/auth/resend-otp`
- **Body (JSON):**
```json
{
  "email": "YOUR_EMAIL@gmail.com"
}
```

---

## 2️⃣ HR ROUTES (`/api/hr`)

### 2.1 Create User (Manager/Employee)
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/hr/create-user`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Body (JSON) - Create Manager:**
```json
{
  "name": "Test Manager",
  "email": "testmanager@feedx.com",
  "password": "TestManager@123",
  "role": "manager",
  "organisation": "TestCorp",
  "organisationId": "894c1978-78d0-41a3-87e7-1960162c0926"
}
```
- **Body (JSON) - Create Employee:**
```json
{
  "name": "Test Employee",
  "email": "employeetest@feedx.com",
  "password": "TestEmployee@123",
  "role": "employee",
  "organisation": "TestCorp",
  "organisationId": "894c1978-78d0-41a3-87e7-1960162c0926"
}
```
- **Expected Response:** `201 Created` - "User created successfully!"

---

### 2.2 HR Dashboard
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/hr/dashboard`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Expected Response:** `200 OK`
- **Response Example:**
```json
{
  "success": true,
  "data": {
    "totalEmployees": 1,
    "totalManagers": 1,
    "goalStats": {
      "total": 0,
      "completed": 0,
      "pending": 0
    },
    "feedbackStats": {
      "positive": 0,
      "neutral": 0,
      "negative": 0,
      "total": 0
    },
    "topPerformer": null
  },
  "aiInsights": "AI insights are unavailable at the moment."
}
```

---

### 2.3 Employee List
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/hr/employee-list`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Expected Response:** `200 OK` - Array of employees

---

### 2.4 Manager List
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/hr/manager-list`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Expected Response:** `200 OK` - Array of managers

---

### 2.5 Toggle Auto Feedback ⏭️ (Not Tested)
- **Method:** `PUT`
- **URL:** `{{baseUrl}}/api/hr/toggle-auto-feedback/:userId`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Body (JSON):**
```json
{
  "enable": true
}
```

---

## 3️⃣ MANAGER ROUTES (`/api/manager`)

### 3.1 Manager Dashboard
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/manager/dashboard`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Expected Response:** `200 OK`
- **Response Example:**
```json
{
  "data": {
    "totalEmployees": 1,
    "goalsAssigned": 0,
    "goalsCompleted": 0,
    "goalsPending": 0,
    "feedbackStats": {
      "totalFeedback": 0,
      "positive": 0,
      "neutral": 0,
      "negative": 0,
      "avgFeedbackScore": "N/A"
    },
    "topPerformer": null
  },
  "aiInsights": "AI insights are unavailable at the moment."
}
```

---

### 3.2 Get Employees
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/manager/get-employees`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Expected Response:** `200 OK` - Array of employees under this manager

---

## 4️⃣ EMPLOYEE ROUTES (`/api/employee`)

### 4.1 Employee Dashboard
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/employee/dashboard`
- **Headers:**
  - `Authorization: Bearer <EMPLOYEE_TOKEN>`
- **Expected Response:** `200 OK`
- **Response Example:**
```json
{
  "success": true,
  "data": {
    "goalsAssigned": 0,
    "goalsCompleted": 0,
    "goalsPending": 0,
    "goalCompletionRate": "0.00%",
    "feedbackStats": {
      "totalFeedback": 0,
      "positive": 0,
      "neutral": 0,
      "negative": 0,
      "averageRating": "0.00"
    },
    "recentGoals": []
  },
  "aiInsights": "AI insights are unavailable at the moment."
}
```

---

## 5️⃣ FEEDBACK ROUTES (`/api/feedback`)

### 5.1 HR Triggers Feedback
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/feedback/trigger`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Body (JSON):**
```json
{
  "targetId": "69d5601afd9921aca419b801",
  "sessionName": "Q1 Performance Review"
}
```
- **Expected Response:** `201 Created`
- **Response Example:**
```json
{
  "success": true,
  "message": "Feedback request created successfully",
  "feedbackRequest": {
    "_id": "69d567d2fd9921aca419b855",
    "targetId": "69d5601afd9921aca419b801",
    "requestedBy": "69d55d1dfd9921aca419b7df",
    "sessionName": "Q1 Performance Review",
    "leftResponders": ["69d55ea6fd9921aca419b7ef"],
    "respondedBy": [],
    "expiresAt": "2026-04-14T20:23:46.449Z",
    "targetModel": "Employee",
    "responderModel": "Manager"
  }
}
```
- **Save:** `feedbackRequestId` for submission

---

### 5.2 Get Pending Feedback Requests
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/feedback/requests`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Expected Response:** `200 OK` - Array of pending feedback requests

---

### 5.3 Submit Feedback
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/feedback/submit`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Body (JSON) - Manager to Employee:**
```json
{
  "targetId": "69d5601afd9921aca419b801",
  "objectiveResponses": {
    "How effectively does the employee take ownership of tasks? (1-5)": "4",
    "How well does the employee communicate with peers? (1-5)": "5",
    "How reliable is the employee in meeting deadlines? (1-5)": "4"
  },
  "subjectiveResponses": [
    { "answer": "No" },
    { "answer": "Employee could improve on time management." }
  ]
}
```
- **Expected Response:** `200 OK`
- **Response Example:**
```json
{
  "success": true,
  "message": "Feedback submitted successfully",
  "data": {
    "feedbackId": "69d5da597b64473609929809",
    "receiver": {
      "id": "69d5601afd9921aca419b801",
      "model": "Employee"
    },
    "questionsAnswered": 5
  }
}
```

---

### 5.4 Feedback Analytics
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/feedback/analytics/69d5601afd9921aca419b801`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Expected Response:** `200 OK` - Feedback statistics and question responses

---

### 5.5 Get Feedback Form ⏭️ (Not Tested)
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/feedback/form/:targetId`
- **Headers:**
  - `Authorization: Bearer <TOKEN>`

---

### 5.6 Get Sessions ⏭️ (Not Tested)
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/feedback/sessions/:targetId`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`

---

### 5.7 Get Responses ⏭️ (Not Tested)
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/feedback/responses/:feedbackRequestId`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`

---

## 6️⃣ GOAL ROUTES (`/api/goal`)

### 6.1 Create Goal (Manager assigns to Employee)
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/goal/create`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Body (JSON):**
```json
{
  "title": "Complete Q1 Report",
  "description": "Prepare and submit the Q1 performance analysis report",
  "deadline": "2026-04-30",
  "employeeId": "69d5601afd9921aca419b801"
}
```
- **Expected Response:** `201 Created`
- **Response Example:**
```json
{
  "message": "Goal assigned successfully",
  "goal": {
    "_id": "69d5dc477b6447360992982a",
    "employeeId": "69d5601afd9921aca419b801",
    "managerId": "69d55ea6fd9921aca419b7ef",
    "title": "Complete Q1 Report",
    "description": "Prepare and submit the Q1 performance analysis report",
    "deadline": "2026-04-30T00:00:00.000Z",
    "status": "Pending"
  }
}
```
- **Save:** `goalId` for later operations

---

### 6.2 Employee Gets Their Goals
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/goal`
- **Headers:**
  - `Authorization: Bearer <EMPLOYEE_TOKEN>`
- **Expected Response:** `200 OK` - Array of assigned goals

---

### 6.3 Employee Requests Goal Completion
- **Method:** `PUT`
- **URL:** `{{baseUrl}}/api/goal/:goalId/request-completion`
- **Headers:**
  - `Authorization: Bearer <EMPLOYEE_TOKEN>`
- **Expected Response:** `200 OK` - Status changes to "Pending Approval"

---

### 6.4 Manager Approves Goal
- **Method:** `PUT`
- **URL:** `{{baseUrl}}/api/goal/:goalId/approve`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Expected Response:** `200 OK` - Status changes to "Completed"

---

### 6.5 Manager Gets Goals for Specific Employee
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/goal/manager/goals/:employeeId`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Expected Response:** `200 OK` - Array of goals for that employee

---

### 6.6 Employee Goal Analytics
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/goal/analytics/:employeeId`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`
- **Expected Response:** `200 OK`
- **Response Example:**
```json
{
  "message": "Employee goal analytics retrieved.",
  "analytics": {
    "totalGoals": 1,
    "pendingGoals": 0,
    "pendingApproval": 0,
    "completedGoals": 1,
    "completionRate": "100.00%"
  }
}
```

---

### 6.7 Manager Gets All Goals ⏭️ (Not Tested)
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/goal/manager/goals`
- **Headers:**
  - `Authorization: Bearer <MANAGER_TOKEN>`

---

## 7️⃣ SELF-ASSESSMENT ROUTES (`/api/self`)

### 7.1 HR Creates Session
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/self/session`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Body (JSON):**
```json
{
  "sessionName": "Q1 Self Assessment 2026",
  "description": "Quarterly self-assessment for performance review"
}
```
- **Expected Response:** `201 Created`
- **Response Example:**
```json
{
  "message": "Session created",
  "session": {
    "_id": "69d5e2c37b64473609929854",
    "sessionName": "Q1 Self Assessment 2026",
    "createdBy": "69d55d1dfd9921aca419b7df",
    "active": true
  }
}
```
- **Save:** `sessionId` for later operations

---

### 7.2 Get Active Sessions
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/self/sessions`
- **Headers:**
  - `Authorization: Bearer <EMPLOYEE_TOKEN>`
- **Expected Response:** `200 OK` - Array of active sessions

---

### 7.3 Get Questions
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/self/questions`
- **Headers:**
  - `Authorization: Bearer <EMPLOYEE_TOKEN>`
- **Expected Response:** `200 OK` - Role and questions
- **Note:** Questions file is currently empty - needs to be populated

---

### 7.4 Submit Self-Assessment
- **Method:** `POST`
- **URL:** `{{baseUrl}}/api/self/submit`
- **Headers:**
  - `Authorization: Bearer <EMPLOYEE_TOKEN>`
- **Body (JSON):**
```json
{
  "sessionId": "69d5e2c37b64473609929854",
  "responses": [
    {
      "question": "What were your major accomplishments this quarter?",
      "answer": "Completed Q1 report ahead of schedule"
    },
    {
      "question": "What challenges did you face?",
      "answer": "Time management could be improved"
    },
    {
      "question": "What are your goals for next quarter?",
      "answer": "Improve communication and take more ownership"
    }
  ]
}
```
- **Expected Response:** `201 Created`

---

### 7.5 HR Gets Session Submissions
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/self/session/:sessionId/responses`
- **Headers:**
  - `Authorization: Bearer <HR_TOKEN>`
- **Expected Response:** `200 OK` - Array of all submissions for the session

---

## 8️⃣ NOTIFICATION ROUTES (`/api/notif`)

### 8.1 Get User Notifications
- **Method:** `GET`
- **URL:** `{{baseUrl}}/api/notif/:userId`
- **Headers:**
  - `Authorization: Bearer <TOKEN>`
- **Expected Response:** `200 OK` - Array of notifications (may be empty)

---

### 8.2 Delete Notification ⏭️ (Not Tested)
- **Method:** `DELETE`
- **URL:** `{{baseUrl}}/api/notif/:notificationId`
- **Headers:**
  - `Authorization: Bearer <TOKEN>`

---

## 📊 Test Results Summary

| Category | Total | Tested | Passed | Failed |
|----------|-------|--------|--------|--------|
| Auth | 6 | 4 | 4 | 0 |
| HR | 5 | 4 | 4 | 0 |
| Manager | 2 | 2 | 2 | 0 |
| Employee | 1 | 1 | 1 | 0 |
| Feedback | 7 | 4 | 4 | 0 |
| Goals | 7 | 6 | 6 | 0 |
| Self-Assessment | 5 | 5 | 5 | 0 |
| Notifications | 2 | 1 | 1 | 0 |
| **TOTAL** | **35** | **27** | **27** | **0** |

---

## 🐛 Known Issues

1. **Self-Assessment Questions Empty**
   - File: `server/utils/selfAssessmentQuestions.js`
   - Issue: No questions defined for Employee/Manager roles
   - Impact: GET `/api/self/questions` returns empty questions

2. **Password Exposure in Response**
   - Endpoint: GET `/api/self/session/:id/responses`
   - Issue: Returns hashed password in populated user object
   - Fix: Add `.select("-password")` to the query

3. **Feedback Analytics avgRating Returns NaN**
   - Endpoint: GET `/api/feedback/analytics/:id`
   - Issue: `rating` field doesn't exist in new schema
   - Fix: Calculate rating from objective responses

4. **Token Expiry**
   - JWT tokens expire after 7 days
   - Always re-login before testing if you get 401 errors

---

## 📝 Important Notes

1. **Always include `Authorization` header** for authenticated routes
2. **Token format:** `Bearer <token>` (with space after "Bearer")
3. **Clear Postman cookies** if you get authentication errors
4. **Re-login frequently** to get fresh tokens
5. **Save IDs** returned from creation endpoints for subsequent operations

---

## 🚀 Quick Start Testing

1. Start backend server: `cd server && npm run dev`
2. Login as HR to get token
3. Create Manager and Employee users using HR token
4. Login as Manager and Employee to get their tokens
5. Test role-specific endpoints using appropriate tokens

---

**Generated:** April 8, 2026  
**Tested By:** Manual Postman Testing  
**Server Version:** FeedX HR API v1.0
