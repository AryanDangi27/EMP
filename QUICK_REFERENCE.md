# ⚡ Quick Reference Guide

## 🚀 Start Here (Choose Your Path)

### I want to get running immediately
→ Read: **GETTING_STARTED.md** (5-minute overview)

### I want detailed setup instructions
→ Read: **SETUP.md** (Step-by-step installation)

### I want to understand the architecture
→ Read: **README.md** (Full technical documentation)

### I want to see what was built
→ Read: **PROJECT_INVENTORY.md** (Complete file structure)

---

## 🔑 Essential Commands

### Installation
```bash
npm install                    # Root dependencies
cd backend && npm install      # Backend dependencies
cd frontend && npm install     # Frontend dependencies
```

### Blockchain Deployment
```bash
npx hardhat compile          # Compile Solidity
npx hardhat run scripts/deploy.js --network sepolia
```

### Development Servers
```bash
cd backend && npm run dev     # Express on port 5000
cd frontend && npm run dev    # Next.js on port 3000
```

### Database
```bash
mongod                        # Start MongoDB locally
mongosh                       # Connect to MongoDB
```

---

## 📱 User Roles & What They Can Do

| Role | Can Do |
|------|--------|
| **PATIENT** | Own data, grant/revoke access, view records, generate emergency QR |
| **DOCTOR** | View records (with permission), upload diagnoses, check access requests |
| **HOSPITAL** | Upload lab reports, send test results to patients |
| **EMERGENCY** | View limited critical info (blood group, allergies, conditions) in emergencies |
| **ADMIN** | Manage user roles, deactivate accounts, view audit logs |

---

## 🛣️ Important API Routes

### Authentication (No login required)
```
POST /auth/register     - Create account with wallet
POST /auth/login        - Login with message signature
POST /auth/refresh      - Get new JWT token
```

### Medical Records
```
POST   /records/upload  - Upload file to IPFS
GET    /records/:addr   - Get patient's records
DELETE /records/:id     - Delete record
```

### Access Control
```
POST /access/grant      - Give someone access
POST /access/revoke     - Revoke someone's access
GET  /access/grants     - View your access grants
```

---

## 🔐 Security Quick Facts

- **Wallet-based authentication** - No passwords, use MetaMask
- **JWT tokens** - Expire after 7 days
- **Smart contract enforces access** - Blockchain verification
- **IPFS for files** - Medical data never stored on-chain
- **Audit logging** - Every action logged immutably
- **Role-based UI** - Unauthorized buttons hidden

---

## 🎯 Environment Variables You Need

### Backend (.env)
```
MONGODB_URI=your_mongodb_url
JWT_SECRET=secure_random_string
SEPOLIA_RPC_URL=your_alchemy_url
PINATA_API_KEY=your_pinata_key
CONTRACT_ADDRESS=0x...
```

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:5000
NEXT_PUBLIC_CONTRACT_ADDRESS=0x...
NEXT_PUBLIC_SEPOLIA_RPC_URL=your_alchemy_url
```

---

## 📂 File Locations (Quick Reference)

| What | Where |
|------|-------|
| Smart Contract | `contracts/MedicalPassport.sol` |
| Backend Server | `backend/server.js` |
| Frontend App | `frontend/app/page.tsx` |
| Database Models | `backend/models/` |
| API Controllers | `backend/controllers/` |
| Route Definitions | `backend/routes/` |
| RBAC Middleware | `backend/middleware/auth.js` |
| TypeScript Types | `frontend/lib/types.ts` |
| Hardhat Config | `hardhat.config.js` |

---

## 🧪 How to Test

### 1. Create Test Wallets
- Open MetaMask
- Create 3 test accounts (Patient, Doctor, Emergency)

### 2. Get Test ETH
- Go to https://www.alchemy.com/faucets/ethereum-sepolia
- Get ~0.5 test ETH in each wallet

### 3. Test Workflows
- **Patient**: Upload medical file
- **Doctor**: Grant yourself access (admin changes role)
- **Verify**: Doctor can see the file

---

## 🐛 Troubleshooting Fast

| Problem | Solution |
|---------|----------|
| "Cannot connect to MongoDB" | Ensure `mongod` is running |
| "RPC Error" in MetaMask | Update Sepolia RPC URL in MetaMask |
| "Contract address not found" | Run deployment script again, copy address |
| "CORS error" | Check `FRONTEND_URL` in backend/.env |
| "Port 3000 already in use" | Kill process: `lsof -i :3000` then `kill -9 <PID>` |

---

## 🚀 Deployment Quick Steps

### Frontend (Vercel)
```bash
vercel --prod
# Or connect GitHub repo for auto-deploy
```

### Backend (Railway/Render)
```bash
# Push to GitHub
# Connect repository to Railway/Render
# Set environment variables
# Deploy!
```

### Smart Contract (Ethereum Mainnet)
```bash
# Update hardhat.config.js with mainnet provider
npx hardhat run scripts/deploy.js --network mainnet
```

---

## 📊 Data Model Overview

### User
- walletAddress (unique)
- email (unique)
- role (PATIENT | DOCTOR | HOSPITAL | EMERGENCY | ADMIN)
- verified
- profile (name, DOB, specialization, etc.)

### MedicalRecord
- patientAddress
- ipfsHash (links to file on IPFS)
- recordType (lab, prescription, diagnosis, etc.)
- uploadedBy (wallet of uploader)
- timestamp

### AccessControl
- patientAddress (who owns the data)
- granteeAddress (who gets access)
- granteeRole (DOCTOR | HOSPITAL | EMERGENCY)
- permission (view, upload, download)
- expiresAt (when access expires, null = forever)

### AuditLog
- action (USER_REGISTERED, RECORD_UPLOADED, etc.)
- actorAddress (who did it)
- targetAddress (who was affected)
- timestamp
- details (what happened)

---

## 🔗 External Services You'll Need

| Service | What For | Sign Up |
|---------|----------|---------|
| Alchemy | Ethereum RPC URL | https://www.alchemy.com/ |
| Pinata | IPFS file hosting | https://www.pinata.cloud/ |
| MongoDB | Database | https://www.mongodb.com/ |
| Etherscan | Contract verification | https://etherscan.io/ |
| MetaMask | Wallet (browser extension) | https://metamask.io/ |

---

## 📖 Read These First

1. **COMPLETION_SUMMARY.md** - High-level overview (5 min)
2. **GETTING_STARTED.md** - Features and quick start (10 min)
3. **SETUP.md** - Detailed installation (30 min)
4. **README.md** - Full documentation (60 min)

---

## ✅ Pre-Launch Checklist

- [ ] All dependencies installed
- [ ] .env files configured with real API keys
- [ ] MongoDB running locally
- [ ] Smart contract deployed to Sepolia
- [ ] CONTRACT_ADDRESS updated in .env files
- [ ] Backend running on port 5000
- [ ] Frontend running on port 3000
- [ ] Can connect MetaMask wallet
- [ ] Can login with wallet signature
- [ ] Can create test users
- [ ] Can upload medical record
- [ ] Can grant/revoke access
- [ ] Can view records in dashboard

---

## 🎓 Learning Path

If new to any technology:

1. **Next.js** → Next.js Docs (5 hours)
2. **Express.js** → Express Docs (4 hours)
3. **Solidity** → CryptoZombies (6 hours)
4. **MongoDB** → MongoDB Docs (4 hours)
5. **Ethereum** → Ethereum.org (8 hours)

---

## 💡 Pro Tips

- Use Chrome DevTools for debugging frontend
- Use Postman to test API endpoints
- Check MongoDB with Compass GUI for easy data browsing
- Use Hardhat local node for contract testing: `npx hardhat node`
- Enable MetaMask console: `window.ethereum.request({method: 'eth_accounts'})`
- Save contract ABI after compilation for frontend integration

---

## 📞 Get Unblocked

**Stuck?** → Check **SETUP.md** Troubleshooting section

**Need architecture help?** → Read **README.md**

**Want to understand the code?** → Check inline comments

**Need details?** → See **PROJECT_INVENTORY.md**

---

**Good luck with your Electronic Medical Passport! 🏥💙**
