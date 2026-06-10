# Electronic Medical Passport (EMP)

A full-stack decentralized healthcare application where patients own their medical data with blockchain-enforced access control.

## 🎯 Features

- **Patient-Controlled Data**: Patients own and manage medical records
- **Role-Based Access Control (RBAC)**: Strict access permissions for doctors, hospitals, and emergency responders
- **Blockchain-Secured**: Immutable audit trail on Ethereum Sepolia
- **IPFS Storage**: Medical files stored decentralized via Pinata
- **Emergency Access**: Time-limited access for emergency situations
- **Wallet Authentication**: Web3 login with MetaMask
- **Production-Grade**: Type-safe, modular, fully documented

## 🏗️ Architecture

```
EMP/
├── frontend/              # Next.js React app
│   ├── app/              # App Router pages
│   ├── components/       # Reusable components
│   ├── hooks/           # Custom React hooks
│   ├── lib/             # Utilities and API client
│   └── package.json
├── backend/              # Express.js API server
│   ├── controllers/     # Business logic
│   ├── routes/          # API endpoints
│   ├── middleware/      # Auth & RBAC
│   ├── models/          # MongoDB schemas
│   ├── server.js
│   └── package.json
├── contracts/            # Solidity smart contracts
│   └── MedicalPassport.sol
├── scripts/              # Deployment scripts
│   └── deploy.js
├── hardhat.config.js    # Hardhat configuration
└── README.md            # This file
```

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Frontend | Next.js 14, React 18, TypeScript, Tailwind CSS, Framer Motion |
| Backend | Node.js, Express.js, MongoDB, Mongoose, JWT |
| Blockchain | Solidity, Hardhat, Ethereum Sepolia |
| Storage | IPFS (Pinata) |
| Authentication | MetaMask, Wallet Signatures |
| Web3 Integration | Ethers.js, Wagmi, RainbowKit |

## 📋 User Roles

| Role | Permissions |
|------|------------|
| **PATIENT** | Own data, grant/revoke access, view records |
| **DOCTOR** | View patient records (with permission), upload diagnoses |
| **HOSPITAL** | Upload lab reports and test results |
| **EMERGENCY** | Limited read-only access in emergencies (time-limited) |
| **ADMIN** | Manage user roles, system settings |

## 🚀 Quick Start

### Prerequisites

- Node.js 16+ 
- MongoDB (local or Atlas)
- MetaMask browser extension
- Pinata account (for IPFS)
- Alchemy account (for Sepolia RPC)

### 1. Clone and Setup

```bash
cd EMP

# Install dependencies
npm install

# Frontend setup
cd frontend
npm install
cd ..

# Backend setup
cd backend
npm install
cd ..

# Blockchain setup
npm install
```

### 2. Environment Configuration

**Backend** (`.env`):
```bash
cp backend/.env.example backend/.env
# Edit backend/.env with your credentials
```

**Frontend** (`.env.local`):
```bash
cp frontend/.env.example frontend/.env.local
# Edit frontend/.env.local with your contract address
```

### 3. MongoDB Setup

```bash
# Local MongoDB (if installed)
mongod

# Or use MongoDB Atlas cloud database
# Update MONGODB_URI in backend/.env
```

### 4. Deploy Smart Contract

```bash
# Compile
npx hardhat compile

# Deploy to Sepolia
npx hardhat run scripts/deploy.js --network sepolia

# Copy CONTRACT_ADDRESS from output and update .env files
```

### 5. Start Services

**Terminal 1 - Backend:**
```bash
cd backend
npm run dev
# Runs on http://localhost:5000
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
# Runs on http://localhost:3000
```

### 6. Access Application

- Open http://localhost:3000
- Connect MetaMask wallet
- Sign message to authenticate
- Explore role-based features

## 📚 API Endpoints

### Authentication
```
POST   /auth/register         - Register new user
POST   /auth/login            - Login with signature
POST   /auth/verify-email     - Verify email
POST   /auth/refresh          - Refresh JWT token
```

### User Profile
```
GET    /user/profile          - Get user profile
PUT    /user/profile          - Update profile
GET    /user/:walletAddress   - Get user by address (admin)
GET    /user/role/:role       - Get users by role (admin)
PUT    /user/:walletAddress/role - Update user role (admin)
```

### Medical Records
```
POST   /records/upload        - Upload medical record
GET    /records/:patientAddress - Get patient's records
GET    /records/detail/:recordId - Get single record
DELETE /records/:recordId     - Delete record
```

### Access Control
```
POST   /access/grant          - Grant access to provider
POST   /access/revoke         - Revoke access
GET    /access/grants         - Get patient's access grants
GET    /access/check/:patientAddress/:granteeAddress - Check access
GET    /access/requests       - Get provider's access requests
```

## 🔐 Smart Contract Functions

```solidity
// Role Management
assignRole(address, role)              // Assign role to user
getRole(address)                       // Get user's role

// Access Control
grantAccess(provider, role, duration)  // Grant time-limited access
revokeAccess(provider)                 // Revoke access
hasAccess(patient, accessor)           // Check if accessor has permission

// Medical Records
addRecord(patient, ipfsHash, type)     // Add medical record
getRecords(patient)                    // Retrieve patient's records
getEmergencyData(patient)              // Emergency-only data

// Emergency Access
activateEmergencyAccess(patient, expiry) // Activate emergency access
getEmergencyAccess(patient)            // Get active emergency access
```

## 🔑 Authentication Flow

1. **Wallet Connection**: User connects MetaMask
2. **Message Signing**: User signs unique message to prove ownership
3. **Backend Verification**: Backend verifies signature using ethers.js
4. **JWT Issuance**: Backend issues JWT token (7-day expiration)
5. **Protected Requests**: All API calls include Authorization header with JWT

## 💾 Data Flow

### Medical Record Upload
```
Patient/Doctor → Backend (Express) → Pinata (IPFS) → IPFS Hash
                    ↓
            MongoDB (record metadata)
                    ↓
            Smart Contract (hash log for immutability)
```

### Access Control
```
Patient → Smart Contract (assignRole, grantAccess) → Immutable Log
            ↓
        MongoDB (metadata & expiration)
            ↓
        Backend RBAC (enforces on API level)
            ↓
        Frontend (role-based UI)
```

## 🧪 Testing Smart Contract

```bash
# Run tests
npx hardhat test

# Run with coverage
npx hardhat coverage

# Run on local network
npx hardhat node
# In another terminal:
npx hardhat run scripts/deploy.js --network localhost
```

## 📊 Database Schemas

### User Collection
```javascript
{
  walletAddress: String (unique),
  email: String (unique),
  role: String (enum: PATIENT, DOCTOR, HOSPITAL, EMERGENCY, ADMIN),
  verified: Boolean,
  profile: {
    fullName: String,
    dateOfBirth: Date,
    specialization: String (for doctors)
  },
  createdAt: Date,
  updatedAt: Date
}
```

### MedicalRecord Collection
```javascript
{
  patientAddress: String,
  ipfsHash: String (unique),
  recordType: String,
  uploadedBy: String,
  fileName: String,
  metadata: {
    description: String,
    uploadedToBlockchain: Boolean
  },
  timestamp: Date
}
```

### AccessControl Collection
```javascript
{
  patientAddress: String,
  granteeAddress: String,
  granteeRole: String,
  permission: { view, upload, download },
  expiresAt: Date,
  isActive: Boolean,
  timestamp: Date
}
```

## 🔒 Security Features

- ✅ Role-Based Access Control (RBAC) on UI, API, and smart contract
- ✅ JWT authentication with 7-day expiration
- ✅ Wallet signature verification for Web3 authenticity
- ✅ No medical data stored on-chain (only IPFS hashes)
- ✅ Audit logs for all operations
- ✅ HTTPS ready (Vercel frontend, secure backend)
- ✅ MongoDB indexes for performance
- ✅ Helmet.js for HTTP security headers
- ✅ CORS configuration for frontend isolation

## 🚨 Error Handling

All endpoints return consistent error responses:

```javascript
{
  error: "Error message",
  details: "Additional details (dev only)"
}
```

## 📈 Production Deployment

### Frontend (Vercel)
```bash
npm install -g vercel
vercel --prod
```

### Backend (Render/Railway)
```bash
# Push to GitHub, connect service, set env vars
```

### Smart Contract (Mainnet)
```bash
# Update hardhat.config.js with mainnet provider
npx hardhat run scripts/deploy.js --network mainnet
```

## 🤝 Contributing

1. Create feature branch: `git checkout -b feature/name`
2. Commit changes: `git commit -m 'Add feature'`
3. Push to branch: `git push origin feature/name`
4. Open Pull Request

## 📝 License

MIT License - See LICENSE file

## 📞 Support

For issues or questions:
1. Check existing issues on GitHub
2. Create detailed issue report
3. Include logs and reproduction steps

---

**Built with ❤️ for decentralized healthcare**
