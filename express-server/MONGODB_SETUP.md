# MongoDB Setup Guide

## Overview

The Express server has been migrated from SQLite to MongoDB for better data persistence and reliability.

## Prerequisites

1. **MongoDB installed** on your system, OR
2. **MongoDB Atlas account** (cloud MongoDB - free tier available)

## Installation

### 1. Install Dependencies

```bash
cd express-server
npm install
```

This will install `mongoose` (MongoDB driver for Node.js).

### 2. MongoDB Options

#### Option A: Local MongoDB

1. **Install MongoDB:**
   - Windows: Download from [mongodb.com](https://www.mongodb.com/try/download/community)
   - Mac: `brew install mongodb-community`
   - Linux: `sudo apt-get install mongodb`

2. **Start MongoDB:**
   ```bash
   # Windows
   net start MongoDB

   # Mac/Linux
   mongod
   ```

3. **Default connection:** The server will use `mongodb://localhost:27017/ids_insights` by default.

#### Option B: MongoDB Atlas (Cloud - Recommended)

1. **Create free account:** [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)

2. **Create a cluster** (free tier M0)

3. **Get connection string:**
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database password

4. **Set in .env file:**
   ```env
   MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/ids_insights?retryWrites=true&w=majority
   ```

### 3. Configure Environment Variables

Create or update `.env` file in `express-server/`:

```env
PORT=3000
MONGODB_URI=mongodb://localhost:27017/ids_insights
```

Or for MongoDB Atlas:
```env
PORT=3000
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/ids_insights?retryWrites=true&w=majority
```

## Start the Server

```bash
npm start
```

You should see:
```
✅ Connected to MongoDB
   Database: mongodb://localhost:27017/ids_insights
Server is running on port 3000
```

## Database Structure

### Collection: `insights`

Each document structure:
```json
{
  "_id": "ObjectId",
  "attack_session_id": "attack_20251104_101530",
  "insights": [
    "https://drive.google.com/uc?export=view&id=chart1",
    "https://drive.google.com/uc?export=view&id=chart2"
  ],
  "created_at": "2025-11-04T10:15:30.000Z"
}
```

## API Endpoints

All endpoints work the same as before:

- `GET /` - Welcome message
- `POST /data` - Store insights
- `GET /data` - Get all insights
- `GET /data/session/:sessionId` - Get insights by session

Also available with `/api/data` prefix for consistency.

## Benefits of MongoDB

✅ **Persistent storage** - Data doesn't disappear
✅ **Scalable** - Can handle large amounts of data
✅ **Flexible schema** - Easy to add new fields
✅ **Cloud option** - MongoDB Atlas for free hosting
✅ **Better performance** - Indexed queries
✅ **No file management** - No .db files to manage

## Troubleshooting

### Error: "MongoDB connection error"

**Solution:**
1. Make sure MongoDB is running (if using local)
2. Check MONGODB_URI in .env file
3. For Atlas: Check IP whitelist (add 0.0.0.0/0 for testing)

### Error: "Authentication failed"

**Solution:**
- Check username/password in connection string
- Make sure database user has read/write permissions

### Data not persisting

**Solution:**
- MongoDB should persist data automatically
- Check MongoDB logs for errors
- Verify connection is successful (check server startup logs)

## Migration from SQLite

If you had data in SQLite:
1. Export data from SQLite (if needed)
2. MongoDB will start fresh
3. Data from notebook will be stored going forward

## Testing

Test the connection:
```bash
node test-endpoints.js
```

All tests should pass with MongoDB!

---

**MongoDB is now configured and ready to use!** 🎉
