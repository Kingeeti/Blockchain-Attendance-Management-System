# Blockchain-Attendance-Management-System
# Blockchain-Based Attendance Management System (BAMS)

## Project Overview

This is a comprehensive **Multi-Layered Hierarchical Blockchain** attendance management system built with Node.js backend and vanilla JavaScript frontend. The system implements three interconnected blockchain layers:

1. **Layer 1 - Department Blockchain**: Independent chains for each department
2. **Layer 2 - Class Blockchain**: Child chains linked to department chains
3. **Layer 3 - Student Blockchain**: Personal ledgers linked to class chains

## Features

### Core Functionality
- ✅ **Complete CRUD Operations** for Departments, Classes, and Students
- ✅ **Immutable Blockchain** - No blocks are deleted, only new blocks added
- ✅ **Proof of Work (PoW)** - Hash must start with "0000"
- ✅ **SHA-256 Hashing** - Secure cryptographic hashing
- ✅ **Hierarchical Chain Linking** - Each child chain genesis block links to parent
- ✅ **Attendance Tracking** - Present, Absent, Leave status
- ✅ **Multi-Level Validation** - Validates entire blockchain hierarchy
- ✅ **Search Functionality** - Search departments, classes, and students
- ✅ **Real-time Updates** - Dynamic blockchain visualization

### Update/Delete Mechanism
- Updates and deletions don't modify existing blocks
- New blocks are added with update/delete status
- System reads the most recent block for current state
- Complete audit trail maintained

## Project Structure

```
blockchain-attendance-system/
├── backend/
│   ├── models/
│   │   ├── Block.js                 # Block structure with PoW
│   │   ├── Blockchain.js            # Blockchain management
│   │   ├── Department.js            # Department model
│   │   ├── Class.js                 # Class model
│   │   └── Student.js               # Student model
│   ├── controllers/
│   │   ├── departmentController.js  # Department CRUD operations
│   │   ├── classController.js       # Class CRUD operations
│   │   ├── studentController.js     # Student CRUD operations
│   │   └── attendanceController.js  # Attendance operations
│   ├── routes/
│   │   ├── departmentRoutes.js      # Department API routes
│   │   ├── classRoutes.js           # Class API routes
│   │   ├── studentRoutes.js         # Student API routes
│   │   └── attendanceRoutes.js      # Attendance API routes
│   ├── utils/
│   │   ├── hash.js                  # SHA-256 hashing utility
│   │   └── validation.js            # Multi-level validation
│   ├── server.js                    # Express server
│   └── package.json                 # Dependencies
├── frontend/
│   ├── index.html                   # Main HTML file
│   ├── styles.css                   # Styling
│   └── app.js                       # Frontend JavaScript
└── README.md                        # This file
```

## Installation & Setup

### Prerequisites
- Node.js (v14 or higher)
- npm (Node Package Manager)

### Step 1: Clone/Create Project
```bash
mkdir blockchain-attendance-system
cd blockchain-attendance-system
```

### Step 2: Setup Backend
```bash
cd backend
npm init -y
npm install express cors body-parser
```

### Step 3: Create All Files
Create the directory structure as shown above and copy the provided code into each file.

### Step 4: Start the Server
```bash
# In the backend directory
node server.js
```

You should see:
```
==================================================
Blockchain Attendance Management System
==================================================
Server running on port 3000
API Base URL: http://localhost:3000
Health Check: http://localhost:3000/health
==================================================
```

### Step 5: Open Frontend
Open `frontend/index.html` in your web browser, or use a local server:
```bash
# In the frontend directory (optional)
npx http-server -p 8080
```

Then navigate to `http://localhost:8080`

## API Endpoints

### Departments
- `POST /api/departments` - Create department
- `GET /api/departments` - Get all departments
- `GET /api/departments/:id` - Get department by ID
- `GET /api/departments/search?name=` - Search departments
- `PUT /api/departments/:id` - Update department
- `DELETE /api/departments/:id` - Delete department
- `GET /api/departments/:id/validate` - Validate department chain

### Classes
- `POST /api/classes` - Create class
- `GET /api/classes` - Get all classes
- `GET /api/classes/department/:departmentId` - Get classes by department
- `GET /api/classes/:departmentId/:classId` - Get class by ID
- `GET /api/classes/search?name=` - Search classes
- `PUT /api/classes/:departmentId/:classId` - Update class
- `DELETE /api/classes/:departmentId/:classId` - Delete class

### Students
- `POST /api/students` - Create student
- `GET /api/students` - Get all students
- `GET /api/students/class/:departmentId/:classId` - Get students by class
- `GET /api/students/:departmentId/:classId/:studentId` - Get student by ID
- `GET /api/students/search?query=` - Search students
- `PUT /api/students/:departmentId/:classId/:studentId` - Update student
- `DELETE /api/students/:departmentId/:classId/:studentId` - Delete student
- `GET /api/students/:departmentId/:classId/:studentId/blockchain` - Get student blockchain

### Attendance
- `POST /api/attendance/:departmentId/:classId/:studentId` - Mark attendance
- `GET /api/attendance/:departmentId/:classId/:studentId` - Get student attendance
- `GET /api/attendance/class/:departmentId/:classId/today` - Today's class attendance
- `GET /api/attendance/department/:departmentId/today` - Today's department attendance
- `POST /api/attendance/class/:departmentId/:classId/bulk` - Bulk mark attendance
- `GET /api/attendance/validate/system` - Validate complete system

## How It Works

### 1. Blockchain Structure

Each block contains:
- **index**: Block number
- **timestamp**: Creation time
- **transactions**: Data (attendance, metadata, etc.)
- **prevHash**: Previous block hash
- **nonce**: Proof of Work nonce
- **hash**: SHA-256 hash of block contents

### 2. Proof of Work

When mining a block:
```javascript
while (hash does not start with "0000") {
    nonce++;
    hash = SHA256(timestamp + transactions + prevHash + nonce);
}
```

### 3. Hierarchical Linking

```
Department Chain (Genesis: prevHash = "0")
    ↓
Class Chain (Genesis: prevHash = Department's latest hash)
    ↓
Student Chain (Genesis: prevHash = Class's latest hash)
    ↓
Attendance Blocks (prevHash = Previous student block hash)
```

### 4. Update/Delete Mechanism

**Traditional Database:**
```sql
UPDATE students SET name = 'New Name' WHERE id = 1;  -- Overwrites data
```

**Blockchain Approach:**
```javascript
// Old block remains unchanged
Block 0: { type: 'create', name: 'Old Name' }

// New block added
Block 1: { type: 'update', name: 'New Name', previousName: 'Old Name' }

// System reads most recent block for current state
```

### 5. Validation Process

1. Validate each block's hash
2. Verify PoW (hash starts with "0000")
3. Check prevHash links to previous block
4. Verify genesis blocks link to parent chains
5. Validate entire hierarchy (Department → Class → Student → Attendance)

## Usage Guide

### Adding a Department
1. Navigate to "Departments" tab
2. Enter department name
3. Click "Create Department"
4. System creates a department chain with genesis block

### Adding a Class
1. Navigate to "Classes" tab
2. Select a department
3. Enter class name
4. Click "Create Class"
5. Class genesis block links to department's latest hash

### Adding a Student
1. Navigate to "Students" tab
2. Select department and class
3. Enter student name and roll number
4. Click "Create Student"
5. Student genesis block links to class's latest hash

### Marking Attendance
1. Navigate to "Attendance" tab
2. Select department and class
3. Click "Load Today's Attendance"
4. Mark each student as Present, Absent, or Leave
5. Each attendance creates a new block in student's chain

### Viewing Blockchain
1. Navigate to "Validation" tab
2. Enter student details
3. Click "View Blockchain"
4. See complete blockchain with all blocks

### Validating System
1. Navigate to "Validation" tab
2. Click "Run Complete Validation"
3. System checks all chains for integrity
4. Reports any tampering or invalid chains

## Security Features

1. **Immutability**: Once created, blocks cannot be modified
2. **Cryptographic Hashing**: SHA-256 ensures data integrity
3. **Proof of Work**: Computational cost prevents tampering
4. **Chain Linking**: Modifying one block invalidates all subsequent blocks
5. **Hierarchical Dependencies**: Tampering with parent chain invalidates all child chains

## Example Blockchain

```javascript
// Department Block
{
  index: 0,
  timestamp: 1699456789000,
  transactions: {
    type: 'genesis',
    chainType: 'department',
    metadata: { departmentId: 'dept_001', departmentName: 'School of Computing' }
  },
  prevHash: '0',
  nonce: 45632,
  hash: '0000a1b2c3d4e5f6...'
}

// Class Block (links to department)
{
  index: 0,
  timestamp: 1699456790000,
  transactions: {
    type: 'genesis',
    chainType: 'class',
    metadata: { classId: 'class_001', className: 'CS-101' }
  },
  prevHash: '0000a1b2c3d4e5f6...',  // Department's hash
  nonce: 78901,
  hash: '0000f6e5d4c3b2a1...'
}

// Student Block (links to class)
{
  index: 0,
  timestamp: 1699456791000,
  transactions: {
    type: 'genesis',
    chainType: 'student',
    metadata: { studentId: 'student_001', studentName: 'John Doe', rollNumber: 'CS001' }
  },
  prevHash: '0000f6e5d4c3b2a1...',  // Class's hash
  nonce: 23456,
  hash: '00001a2b3c4d5e6f...'
}

// Attendance Block (links to previous student block)
{
  index: 1,
  timestamp: 1699456792000,
  transactions: {
    type: 'attendance',
    data: {
      studentId: 'student_001',
      status: 'Present',
      date: '2024-11-16'
    }
  },
  prevHash: '00001a2b3c4d5e6f...',  // Previous student block
  nonce: 89012,
  hash: '00006f5e4d3c2b1a...'
}
```

##  Testing

### Test Blockchain Validation
1. Create a department, class, and student
2. Mark some attendance
3. Run system validation - should show "VALID"
4. Manually tamper with data in code
5. Re-run validation - should show "INVALID"

### Test Update Mechanism
1. Create a student
2. Update the student's name
3. View blockchain - you'll see both old and new blocks
4. System displays new name (most recent block)

### Test Hierarchical Dependencies
1. Create department → class → student → attendance
2. View each blockchain showing proper linking
3. Verify genesis blocks contain parent hashes

## Key Concepts Demonstrated

1. **Blockchain Fundamentals**: Blocks, hashing, chaining
2. **Proof of Work**: Mining with difficulty
3. **Immutability**: No data modification
4. **Hierarchical Blockchain**: Parent-child chain relationships
5. **RESTful API**: Complete CRUD operations
6. **Data Integrity**: Validation and tampering detection
7. **Audit Trail**: Complete history of all changes

## Troubleshooting

**Server won't start:**
- Check if port 3000 is available
- Ensure all npm packages are installed
- Verify Node.js version (v14+)

**Frontend not connecting:**
- Check API_URL in app.js matches server URL
- Enable CORS in browser if needed
- Check browser console for errors

**Blockchain validation fails:**
- Ensure PoW is running (blocks mining can be slow)
- Check that all chains are properly linked
- Verify no manual data modifications


**Happy Coding!**
