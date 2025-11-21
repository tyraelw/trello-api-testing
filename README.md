# 🔷 Trello API - Automated Testing Suite

![Postman](https://img.shields.io/badge/Postman-Collection-orange)
![Newman](https://img.shields.io/badge/Newman-CLI-blue)
![REST API](https://img.shields.io/badge/REST-API-green)

A comprehensive API testing suite for the Trello REST API, demonstrating QA automation fundamentals including CRUD operations, dynamic variable handling, and test assertions.

---

## 📋 Table of Contents

- [About](#about)
- [What I Learned](#what-i-learned)
- [Features](#features)
- [Test Coverage](#test-coverage)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Running with Newman](#running-with-newman)
- [Test Results](#test-results)
- [Project Structure](#project-structure)
- [Security](#security)
- [Author](#author)

---

## 🎯 About

This collection demonstrates end-to-end API testing for Trello's REST API. It covers the complete lifecycle of board management with validation at each step.

**Training Context:** Built during my 17-hour Postman course as a capstone project. This helped me understand REST API testing, dynamic variables, and test scripting.

### Testing Flow:

1. Get all existing boards
2. Create a new board (dynamic naming)
3. Retrieve the created board
4. Create TODO and DONE lists
5. Create a card in TODO
6. Move card to DONE
7. Delete the board
8. Verify deletion (404)

---

## 📚 What I Learned

This project taught me:

✅ **REST API Concepts** - HTTP methods, status codes, request/response structure  
✅ **CRUD Operations** - Create, Read, Update, Delete patterns  
✅ **Dynamic Variables** - Chaining requests with automatic ID propagation  
✅ **Test Assertions** - Multiple validation points per endpoint  
✅ **Postman Collections** - Organizing and documenting API tests  
✅ **Newman CLI** - Running collections from command line  
✅ **Environment Management** - Secure credential handling  

---

## ✨ Features

- ✅ **Complete CRUD Operations** - Full lifecycle testing
- ✅ **Dynamic Variables** - Automatic ID passing between requests
- ✅ **Smart Naming** - Incremental board names (TestBoard, TestBoard_1...)
- ✅ **27 Test Assertions** - Comprehensive validation
- ✅ **Newman Compatible** - Ready for CLI execution
- ✅ **Security First** - No hardcoded credentials

---

## 🧪 Test Coverage

| Endpoint | Method | Tests | Key Validations |
|----------|--------|-------|----------------|
| Get All Boards | GET | 1 | Status code |
| Create Board | POST | 7 | Name, URL, permissions, labels |
| Get Board | GET | 1 | Board retrieval |
| Create TODO List | POST | 4 | Name, state, board relationship |
| Create DONE List | POST | 4 | Name, state, board relationship |
| Create Card | POST | 5 | Name, list assignment, board link |
| Move Card | PUT | 3 | Name persistence, list update |
| Delete Board | DELETE | 1 | Deletion success |
| Get Deleted Board | GET | 1 | 404 error validation |

**Total Tests:** 27 automated assertions

---

## 📦 Prerequisites

- **Postman** (Desktop or Web)
- **Trello Account**
- **Trello API Key & Token**

---

## 🚀 Installation

### 1. Clone Repository
```bash
git clone https://github.com/tyraelw/trello-api-testing.git
cd trello-api-testing
```

### 2. Import to Postman

**Option A: Postman Desktop**
1. Open Postman
2. Click "Import"
3. Select `trello-api-collection.json`

**Option B: Postman Web**
1. Go to [Postman Web](https://web.postman.co)
2. Click "Import"
3. Drag and drop `trello-api-collection.json`

---

## 🔑 Configuration

### Step 1: Get Trello Credentials

1. Go to https://trello.com/app-key
2. Copy your **API Key**
3. Click "Token" link and generate a **Token**
4. Save both securely

### Step 2: Configure Variables

**Method A: Collection Variables (Recommended)**

1. Right-click collection → Edit
2. Go to "Variables" tab
3. Update **Current Value** column only:

| Variable | Initial Value | Current Value |
|----------|---------------|---------------|
| `trellokey` | `YOUR_KEY_HERE` | `your-actual-key` |
| `trellotoken` | `YOUR_TOKEN_HERE` | `your-actual-token` |
| `baseUrl` | `https://api.trello.com` | `https://api.trello.com` |

⚠️ **Important:** Only edit "Current Value" - not exported with collection

**Method B: Environment Variables**

1. Create Environment: "Trello - Local"
2. Add variables:
```json
{
  "trellokey": "your-key",
  "trellotoken": "your-token",
  "baseUrl": "https://api.trello.com"
}
```
3. Select environment from dropdown

---

## 💻 Usage

### Running in Postman

**Run Entire Collection:**
1. Click collection name
2. Click "Run" button
3. Select all requests
4. Click "Run Trello API"

**Run Individual Request:**
1. Open request
2. Click "Send"
3. Check "Test Results" tab

### Expected Results

All tests pass with `200 OK` (except "Get Deleted Board" expects `404`).

**Example output:**
```
✓ Status code is 200
✓ Board created successfully
✓ Board has a name
✓ Board has a valid URL
✓ Board is open (not closed)
```

---

## 🤖 Running with Newman

Newman runs Postman collections from command line.

### Install Newman
```bash
npm install -g newman
```

### Run Collection
```bash
newman run trello-api-collection.json \
  --env-var "trellokey=YOUR_KEY" \
  --env-var "trellotoken=YOUR_TOKEN" \
  --reporters cli,html
```

### Generate HTML Report
```bash
newman run trello-api-collection.json \
  --env-var "trellokey=YOUR_KEY" \
  --env-var "trellotoken=YOUR_TOKEN" \
  --reporters html \
  --reporter-html-export testResults.html
```

### Enhanced Reports (Optional)
```bash
npm install -g newman-reporter-htmlextra

newman run trello-api-collection.json \
  --env-var "trellokey=YOUR_KEY" \
  --env-var "trellotoken=YOUR_TOKEN" \
  --reporters htmlextra \
  --reporter-htmlextra-export report.html
```

---

## 📊 Test Results

**Example successful run:**
```
→ Create Board
  ✓ Board created successfully
  ✓ Board has a name
  ✓ Board has a valid URL
  ✓ Board is open
  
→ Create TODO List
  ✓ TODO list created
  ✓ List name is "TODO"
  ✓ List belongs to board
  
... (27 total assertions)

┌─────────────────────┬──────────┬────────┐
│                     │ executed │ failed │
├─────────────────────┼──────────┼────────┤
│ iterations          │        1 │      0 │
│ requests            │       10 │      0 │
│ test-scripts        │       20 │      0 │
│ assertions          │       27 │      0 │
└─────────────────────┴──────────┴────────┘
```

---

## 📁 Project Structure
```
trello-api-testing/
├── trello-api-collection.json         # Main collection
├── trello-environment-example.json    # Template
├── .gitignore                         # Protects credentials
└── README.md                          # This file
```

---

## 🛡️ Security Best Practices

✅ Never commit credentials to git  
✅ Use environment variables  
✅ Keep "Initial Value" empty in collection  
✅ Use `.gitignore` for environment files  
✅ Rotate API keys regularly  

---

## 📚 Learning Notes

### Challenges I Faced

1. **Variable Chaining:** Learned to extract and pass IDs between requests
2. **Dynamic Naming:** Implemented logic to avoid board name conflicts
3. **Test Assertions:** Practiced writing meaningful validations
4. **Newman Setup:** Learned CLI execution and reporting

### What I'd Improve

- Add more negative test cases
- Implement data-driven testing with CSV
- Add performance testing with Newman
- Create more detailed HTML reports

---

## 👤 Author

**Isrrael Andres Toro Alvarez**

- GitHub: [@tyraelw](https://github.com/tyraelw)
- LinkedIn: [Isrrael Toro Alvarez](https://www.linkedin.com/in/your-profile)
- Email: tyrael78w@gmail.com

---

## 📧 Contact

For questions or feedback: **tyrael78w@gmail.com**

---

## 🔗 Related Projects

- **[Cypress E-Commerce Testing](https://github.com/tyraelw/cypress-ecommerce-testing)** - UI automation with Page Object Model
- **[Grocery Store API](https://github.com/tyraelw/simple-grocery-store-api-testing)** - E-commerce API testing

---

**⭐ If this helps you learn API testing, please star it!**

---

### 📝 Honest Disclosure

This is a training project completed during my Postman course. It demonstrates API testing concepts I learned and practiced. While I'm comfortable with these fundamentals, I'm still growing as a QA engineer and excited to deepen my API testing skills in a professional setting.
