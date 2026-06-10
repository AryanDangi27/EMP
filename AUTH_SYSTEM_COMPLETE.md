# Complete Authentication System - All Roles

## ✅ What's Been Built

### 1. **Signup System** (`/signup`)
- Role selection (Patient, Doctor, Hospital, Emergency, Admin)
- MetaMask wallet connection
- Email registration
- Blockchain-verified signup (wallet signature)

### 2. **Login System** (`/login`)
- Role-based login for all 5 roles
- MetaMask wallet connection
- Message signing for authentication
- JWT token generation

### 3. **Role-Based Dashboards**
- **Patient** → `/patient-dashboard`
- **Doctor** → `/doctor-dashboard`
- **Hospital** → `/hospital` (already exists)
- **Emergency** → `/emergency-dashboard`
- **Admin** → `/admin-dashboard`

### 4. **Backend Updates**
- `/auth/me` endpoint to fetch current user data
- Support for all 5 roles in database
- JWT authentication middleware

## 🚀 Quick Start

### Step 1: Ensure MongoDB is Running
```powershell
# If not already running, start MongoDB
mongod
```

### Step 2: Start Backend
```powershell
cd c:\Users\hp\Desktop\EMP\backend
npm run dev
```
Backend runs on: `http://localhost:5000`

### Step 3: Start Frontend (in new terminal)
```powershell
cd c:\Users\hp\Desktop\EMP\frontend
npm run dev
```
Frontend runs on: `http://localhost:3001`

## 🔄 Complete User Flow

### Signup Flow:
1. Go to `http://localhost:3001/signup`
2. Select your role (Patient/Doctor/Hospital/Emergency/Admin)
3. Enter email and full name
4. Click "Connect" → MetaMask connects
5. Click "Create Account" → Sign message in MetaMask
6. Auto-redirects to role-specific dashboard

### Login Flow:
1. Go to `http://localhost:3001/login?role=patient` (or doctor/hospital/emergency/admin)
2. Click "Connect" → MetaMask connects
3. Click "Sign In with Wallet" → Sign message
4. Auto-redirects to role-specific dashboard

## 📊 Available Roles

| Role | Dashboard | Key Features |
|------|-----------|--------------|
| Patient | `/patient-dashboard` | View records, QR codes, access control |
| Doctor | `/doctor-dashboard` | Manage patients, appointments, verification |
| Hospital | `/hospital` | Upload reports, verify records, management |
| Emergency | `/emergency-dashboard` | Quick access, emergency cases, critical data |
| Admin | `/admin-dashboard` | User management, role assignment, audit logs |

## 🔐 Authentication Details

- **Method**: MetaMask wallet + message signing
- **Storage**: JWT tokens in localStorage
- **Backend**: Express.js with MongoDB
- **Protected Routes**: All dashboards check for valid token + matching role

## 🎯 For Hospital Dashboard

The hospital dashboard is already working at `/hospital` with:
- Issue EMP certificates
- Upload medical records
- Verify documents
- Activity logs

## 📝 API Endpoints

```
POST   /auth/register          - Create new account
POST   /auth/login            - Login with wallet
POST   /auth/refresh          - Refresh JWT token
GET    /auth/me              - Get current user (requires auth)
POST   /auth/verify-email    - Verify email address
```

## ✨ What's Next

To enhance further:
1. Add role-specific features for each dashboard
2. Implement record upload/sharing for patients
3. Add appointment booking for doctors
4. Implement emergency access controls
5. Add admin management panels

## 🏆 Ready for Hackathon!

All 5 roles now have:
✅ Signup with wallet verification
✅ Login with MetaMask
✅ Separate dashboards
✅ Protected routes
✅ Role-based access control
✅ Clean UI with Tailwind CSS
✅ Real authentication system

Good luck winning the hackathon! 🚀
