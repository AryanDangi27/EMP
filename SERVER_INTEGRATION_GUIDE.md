# Server Integration - Step by Step

## How to Add Versioning to Your Backend

### Step 1: Find Your server.js File

Location: `backend/server.js`

### Step 2: Add the Import

Find where you have other route imports like:
```javascript
const authRoutes = require('./routes/authRoutes');
const recordsRoutes = require('./routes/recordsRoutes');
// ... other routes
```

**Add this line:**
```javascript
const recordsRoutesV2 = require('./routes/recordsRoutesV2');
```

### Step 3: Register the Routes

Find where you have route registrations like:
```javascript
app.use('/api/auth', authRoutes);
app.use('/api/records', recordsRoutes);
// ... other routes
```

**Add this line (after other routes):**
```javascript
app.use('/api/v2/records', recordsRoutesV2);
```

---

## Complete Example

### Before:
```javascript
// backend/server.js

const express = require('express');
const app = express();
const mongoose = require('mongoose');
const cors = require('cors');

// Routes
const authRoutes = require('./routes/authRoutes');
const recordsRoutes = require('./routes/recordsRoutes');
const accessRoutes = require('./routes/accessRoutes');
const userRoutes = require('./routes/userRoutes');

// Middleware
app.use(cors());
app.use(express.json());

// API Routes
app.use('/api/auth', authRoutes);
app.use('/api/records', recordsRoutes);
app.use('/api/access', accessRoutes);
app.use('/api/users', userRoutes);

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### After:
```javascript
// backend/server.js

const express = require('express');
const app = express();
const mongoose = require('mongoose');
const cors = require('cors');

// Routes
const authRoutes = require('./routes/authRoutes');
const recordsRoutes = require('./routes/recordsRoutes');
const recordsRoutesV2 = require('./routes/recordsRoutesV2');  // NEW LINE
const accessRoutes = require('./routes/accessRoutes');
const userRoutes = require('./routes/userRoutes');

// Middleware
app.use(cors());
app.use(express.json());

// API Routes
app.use('/api/auth', authRoutes);
app.use('/api/records', recordsRoutes);
app.use('/api/v2/records', recordsRoutesV2);  // NEW LINE
app.use('/api/access', accessRoutes);
app.use('/api/users', userRoutes);

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

---

## Environment Variables (if needed)

These should already be in your `.env`:

```env
# IPFS
PINATA_API_KEY=your_key
PINATA_SECRET_KEY=your_secret

# MongoDB
MONGODB_URI=your_connection_string

# JWT
JWT_SECRET=your_secret_key
```

No new environment variables needed for versioning!

---

## Verification

### After making changes:

1. **Restart server**
   ```bash
   npm start
   # or
   npm run dev
   ```

2. **Check console output**
   ```
   Server running on port 3000
   ```

3. **Test endpoint**
   ```bash
   curl http://localhost:3000/api/v2/records
   # Should return 401 Unauthorized (auth required)
   ```

4. **Test with token**
   ```bash
   curl -H "Authorization: Bearer YOUR_TOKEN" \
        http://localhost:3000/api/v2/records/123/versions
   ```

---

## Available Endpoints After Integration

Once server.js is updated, you'll have these endpoints:

```
POST   /api/v2/records/upload                    - Upload/create version
GET    /api/v2/records/:recordId/versions         - List all versions
GET    /api/v2/records/:recordId/versions/:v      - Get specific version
GET    /api/v2/records/:recordId/audit-trail      - Get audit trail
GET    /api/v2/records/:recordId/versions/:v/download - Download version
GET    /api/v2/records/:recordId/versions/compare - Compare versions
POST   /api/v2/records/:recordId/retract          - Retract record
```

---

## Troubleshooting

### If you get "Module not found" error:

**Check:**
```bash
ls backend/routes/recordsRoutesV2.js
# Should exist

ls backend/controllers/recordsControllerV2.js
# Should exist

ls backend/models/RecordVersion.js
# Should exist
```

### If routes not working:

**Check server.js:**
```bash
grep "recordsRoutesV2" backend/server.js
# Should show 2 lines (require and app.use)
```

### If 404 errors:

**Verify endpoint:**
- Check path spelling (case sensitive)
- Ensure JWT token is valid
- Check authentication middleware is applied

### If upload fails:

**Check:**
- PINATA_API_KEY in .env
- File size < 50MB
- File type is allowed (PDF, images, docs)

---

## That's All!

Just:
1. Add the require statement
2. Add the app.use() line
3. Restart server
4. Start using `/api/v2/records` endpoints

**Done!** ✅

---

## Next Steps

### After server.js is updated:

1. **Test with curl**
   ```bash
   # Upload a test file
   curl -X POST http://localhost:3000/api/v2/records/upload \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -F "patientAddress=0x..." \
     -F "recordType=lab_report" \
     -F "recordName=Test" \
     -F "file=@testfile.pdf"
   ```

2. **Add component to frontend**
   ```tsx
   import RecordVersionHistory from '@/components/records/RecordVersionHistory';
   
   export default function Records() {
     return <RecordVersionHistory recordId="from-db" />;
   }
   ```

3. **Test in browser**
   - Upload a file
   - Upload same record with changes
   - View version history
   - Compare versions

---

## Support

If issues:
1. Read VERSIONING_QUICK_SETUP.md
2. Check VERSIONING_IMPLEMENTATION_GUIDE.md
3. Verify all files created
4. Check server.js has both lines
5. Restart server

**Ready to deploy!** 🚀
