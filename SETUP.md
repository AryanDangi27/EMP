# EMP Setup Guide

## 🔧 Installation & Configuration

### Step 1: Prerequisites

Ensure you have installed:
- **Node.js 16+** - https://nodejs.org/
- **MongoDB** - https://www.mongodb.com/try/download/community
- **Git** - https://git-scm.com/
- **MetaMask** browser extension

### Step 2: Clone Repository

```bash
git clone <repo_url> EMP
cd EMP
```

### Step 3: Install Root Dependencies

```bash
npm install
```

### Step 4: Configure Environment Variables

#### Backend Configuration

```bash
cp backend/.env.example backend/.env
```

Edit `backend/.env`:

```env
# Server
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/emp
# Or use MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/emp

# JWT
JWT_SECRET=your_secret_key_here_change_in_production

# Blockchain (Ethereum Sepolia)
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR_INFURA_KEY
PRIVATE_KEY=your_wallet_private_key
CONTRACT_ADDRESS=0x... # Will be set after contract deployment
ETHERSCAN_API_KEY=your_etherscan_key

# IPFS Storage (Pinata)
PINATA_API_KEY=your_pinata_key
PINATA_SECRET_API_KEY=your_pinata_secret

# Frontend
FRONTEND_URL=http://localhost:3000
```

**Getting the Keys:**

1. **Sepolia RPC URL**: Register at Alchemy (https://www.alchemy.com/)
2. **Private Key**: Export from MetaMask
3. **Pinata Keys**: Sign up at Pinata (https://www.pinata.cloud/)
4. **Etherscan API**: Get from https://etherscan.io/apis

#### Frontend Configuration

```bash
cp frontend/.env.example frontend/.env.local
```

Edit `frontend/.env.local`:

```env
NEXT_PUBLIC_API_URL=http://localhost:5000
NEXT_PUBLIC_CONTRACT_ADDRESS=0x... # After contract deployment
NEXT_PUBLIC_SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR_INFURA_KEY
```

### Step 5: Install Project Dependencies

```bash
# Backend
cd backend
npm install
cd ..

# Frontend
cd frontend
npm install
cd ..

# Contracts (already at root)
npm install
```

### Step 6: Deploy Smart Contract

```bash
# Compile Solidity
npx hardhat compile

# Deploy to Sepolia Testnet
npx hardhat run scripts/deploy.js --network sepolia
```

**Output will show:**
```
✅ MedicalPassport deployed to: 0x...
```

Copy this address and update:
- `backend/.env` → `CONTRACT_ADDRESS`
- `frontend/.env.local` → `NEXT_PUBLIC_CONTRACT_ADDRESS`

### Step 7: Start MongoDB

**Option A: Local MongoDB**
```bash
# macOS/Linux
mongod

# Windows
"C:\Program Files\MongoDB\Server\6.0\bin\mongod.exe"
```

**Option B: MongoDB Atlas (Cloud)**
- No action needed; connection via `MONGODB_URI`

### Step 8: Start Services

**Terminal 1 - Backend API:**
```bash
cd backend
npm run dev
# Listening on http://localhost:5000
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
# Listening on http://localhost:3000
```

### Step 9: Test Application

1. Open http://localhost:3000 in browser
2. Click "Login"
3. Connect MetaMask wallet
4. Sign message with your wallet
5. Complete registration
6. Explore dashboard based on your role

## 🧪 Testing the System

### Test Data Flow

1. **Register as Patient**: Create account, get assigned PATIENT role
2. **Register as Doctor**: Create separate wallet, switch role to DOCTOR
3. **Upload Medical Record** (as Patient):
   - Upload a test file (PDF/Image)
   - Record automatically uploaded to IPFS via Pinata
   - Hash saved to MongoDB and smart contract
4. **Grant Access** (as Patient):
   - Grant DOCTOR access to your records
   - Doctor can now view and upload records
5. **Emergency Access** (as Emergency responder):
   - Activate emergency access with limited duration
   - View only critical health information

### Test MetaMask Setup

1. Create 2-3 test accounts in MetaMask
2. Request test ETH from Sepolia Faucet:
   - https://www.alchemy.com/faucets/ethereum-sepolia
3. Switch between accounts to test role-based features

## 📊 Database Inspection

### Using MongoDB CLI

```bash
# Connect to local MongoDB
mongosh

# Switch to EMP database
use emp

# View users
db.users.find().pretty()

# View medical records
db.medicalrecords.find().pretty()

# View access control
db.accesscontrols.find().pretty()

# View audit logs
db.auditlogs.find().pretty()
```

### Using MongoDB Compass (GUI)

1. Download MongoDB Compass: https://www.mongodb.com/products/compass
2. Connect to `mongodb://localhost:27017`
3. Browse collections and data visually

## 🔍 Smart Contract Verification

### Check Contract on Etherscan

1. Go to https://sepolia.etherscan.io/
2. Search for your contract address
3. Verify it shows:
   - Constructor arguments
   - Transaction history
   - Function calls

### Verify Source Code on Etherscan

```bash
npx hardhat verify --network sepolia <CONTRACT_ADDRESS>
```

## 🐛 Troubleshooting

### Connection Errors

**MongoDB Connection Failed:**
```bash
# Check if MongoDB is running
mongosh --version

# Start MongoDB service
sudo systemctl start mongod  # Linux
brew services start mongodb-community  # macOS
```

**Backend can't connect to frontend:**
```bash
# Check CORS in backend/server.js
# Ensure FRONTEND_URL env var is correct
```

### Smart Contract Issues

**Contract deployment fails:**
```bash
# Check Sepolia RPC URL
npx hardhat networks  # Lists configured networks

# Check private key has test ETH
# Get from: https://www.alchemy.com/faucets/ethereum-sepolia
```

**Contract address not found:**
```bash
# Search transaction hash on etherscan.io
# Verify deployment successful before copying address
```

### MetaMask Issues

**"RPC Error" when connecting:**
1. Open MetaMask settings
2. Networks → Sepolia
3. Set RPC URL to your Alchemy endpoint

**Can't sign message:**
1. Ensure wallet is unlocked
2. Try reconnecting wallet
3. Clear browser cache and reload

## 📈 Performance Optimization

### Frontend
- Images optimized with Next.js Image component
- Code splitting for faster initial load
- Tailwind CSS purged unused styles

### Backend
- MongoDB indexes created on frequently queried fields
- JWT token caching with Redis (optional)
- File uploads limited to 50MB

### Smart Contract
- Gas optimization via compiler settings
- Functions use minimal storage operations
- Batch operations for efficiency

## 🚀 Deployment Checklist

Before going to production:

- [ ] Change `JWT_SECRET` to strong random value
- [ ] Update `NODE_ENV` to "production"
- [ ] Use production MongoDB Atlas instance
- [ ] Deploy contract to Ethereum Mainnet
- [ ] Set `HTTPS` for all URLs
- [ ] Configure proper CORS origins
- [ ] Enable rate limiting on API
- [ ] Set up monitoring and logging
- [ ] Regular backups of MongoDB
- [ ] Audit smart contract on security

## 📚 Additional Resources

- [Ethereum Sepolia Docs](https://ethereum.org/en/developers/docs/networks/)
- [Pinata IPFS Docs](https://docs.pinata.cloud/)
- [MongoDB Docs](https://docs.mongodb.com/)
- [Hardhat Docs](https://hardhat.org/docs)
- [Next.js Docs](https://nextjs.org/docs)

## 🆘 Getting Help

1. Check existing issues on GitHub
2. Review backend logs: `npm run dev 2>&1 | tee backend.log`
3. Enable debug logging: `DEBUG=* npm run dev`
4. Post detailed error messages with context

---

**You're all set!** 🎉 Start exploring the EMP application.
