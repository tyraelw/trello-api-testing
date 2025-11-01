# Trello API - Automated Testing Suite

[![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)](https://www.postman.com/)
[![Trello](https://img.shields.io/badge/Trello-0052CC?style=for-the-badge&logo=trello&logoColor=white)](https://trello.com/)
[![Newman](https://img.shields.io/badge/Newman-Compatible-green?style=for-the-badge)](https://www.npmjs.com/package/newman)

A comprehensive API testing suite for the Trello REST API, demonstrating professional QA automation practices including CRUD operations, dynamic variable handling, and extensive test assertions.

## 📋 Table of Contents

- [About](#about)
- [Features](#features)
- [Test Coverage](#test-coverage)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Running with Newman](#running-with-newman)
- [CI/CD Integration](#cicd-integration)
- [Test Results](#test-results)
- [Project Structure](#project-structure)

## 🎯 About

This collection demonstrates end-to-end API testing for Trello's REST API. It covers the complete lifecycle of board management, from creation to deletion, with comprehensive validation at each step.

**Testing Flow:**
1. Get all existing boards
2. Create a new board (with dynamic naming)
3. Retrieve the created board
4. Create TODO and DONE lists
5. Create a card in TODO list
6. Move card to DONE list
7. Delete the board
8. Verify board deletion (404 response)

## ✨ Features

- ✅ **Complete CRUD Operations** - Create, Read, Update, Delete
- ✅ **Dynamic Variables** - Automatic ID propagation between requests
- ✅ **Smart Board Naming** - Incremental naming to avoid conflicts (TestBoard, TestBoard_1, TestBoard_2...)
- ✅ **Comprehensive Assertions** - Multiple validation points per endpoint
- ✅ **Clean Code** - Well-organized and documented tests
- ✅ **Newman Compatible** - Ready for CLI and CI/CD automation
- ✅ **Security First** - No hardcoded credentials

## 🧪 Test Coverage

| Endpoint | Method | Tests | Validations |
|----------|--------|-------|-------------|
| Get All Boards | GET | 1 | Status code validation |
| Create Board | POST | 7 | Status, name, URL, permissions, labels, organization |
| Get Board | GET | 1 | Status code validation |
| Create TODO List | POST | 4 | Status, name, state, board relationship |
| Create DONE List | POST | 4 | Status, name, state, board relationship |
| Create Card | POST | 5 | Status, name, list assignment, board relationship, attachments |
| Move Card | PUT | 3 | Status, name persistence, list update |
| Delete Board | DELETE | 1 | Status code validation |
| Get Deleted Board | GET | 1 | 404 error validation |

**Total Tests:** 27 automated assertions

## 📦 Prerequisites

- [Postman](https://www.postman.com/downloads/) (Desktop or Web)
- A Trello account
- Trello API Key and Token

## 🚀 Installation

### 1. Clone this repository

```bash
git clone https://github.com/tyraelw/trello-api-testing.git
cd trello-api-testing
```

### 2. Import the collection into Postman

**Option A: Using Postman Desktop**
1. Open Postman
2. Click "Import" button
3. Select `trello-api-collection.json`
4. Collection will appear in your workspace

**Option B: Using Postman Web**
1. Go to [Postman Web](https://web.postman.co/)
2. Click "Import"
3. Drag and drop `trello-api-collection.json`

## 🔑 Configuration

### Step 1: Get Trello Credentials

1. Go to [https://trello.com/app-key](https://trello.com/app-key)
2. Copy your **API Key**
3. Click on "Token" link and generate a **Token**
4. Save both credentials securely

### Step 2: Configure Variables in Postman

#### Method A: Collection Variables (Recommended for local testing)

1. Right-click on the collection → **Edit**
2. Go to **Variables** tab
3. Update only the **Current Value** column:

| Variable | Initial Value | Current Value |
|----------|---------------|---------------|
| trellokey | YOUR_TRELLO_API_KEY_HERE | *your-actual-api-key* |
| trellotoken | YOUR_TRELLO_TOKEN_HERE | *your-actual-token* |
| baseUrl | https://api.trello.com | https://api.trello.com |

4. Click **Save**

> ⚠️ **Important:** Never put credentials in "Initial Value" - they will be exported!

#### Method B: Environment Variables (Recommended for teams)

1. Create a new Environment: **Trello - Local**
2. Add variables:

```json
{
  "trellokey": "your-actual-api-key",
  "trellotoken": "your-actual-token",
  "baseUrl": "https://api.trello.com"
}
```

3. Select the environment from the dropdown

## 💻 Usage

### Running Tests in Postman

#### Run Entire Collection:
1. Click on the collection name
2. Click **Run** button
3. Select all requests
4. Click **Run Trello API**

#### Run Individual Request:
1. Open any request
2. Click **Send**
3. Check the **Test Results** tab

### Expected Results

All tests should pass with **200 OK** status (except "Get Deleted Board" which expects **404**).

Example output:
```
✓ Status code is 200
✓ Board created successfully
✓ Board has a name
✓ Board has a valid URL
✓ Board is open (not closed)
```

## 📝 Testing Data

This collection uses generic test data for demonstration purposes:
- **Board names:** TestBoard, TestBoard_1, TestBoard_2...
- **Card names:** Test Card
- **List names:** TODO, DONE

All test data is generated dynamically and cleaned up after execution.

## 🤖 Running with Newman

Newman allows you to run Postman collections from the command line.

### Install Newman

```bash
npm install -g newman
```

### Run Collection

```bash
newman run trello-api-collection.json \
  --env-var "trellokey=YOUR_API_KEY" \
  --env-var "trellotoken=YOUR_TOKEN" \
  --reporters cli,html
```

### Generate HTML Report

```bash
newman run trello-api-collection.json \
  --env-var "trellokey=YOUR_API_KEY" \
  --env-var "trellotoken=YOUR_TOKEN" \
  --reporters html \
  --reporter-html-export testResults.html
```

## 🔄 CI/CD Integration

### GitHub Actions Example

Create `.github/workflows/api-tests.yml`:

```yaml
name: Trello API Tests

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight

jobs:
  api-tests:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install Newman
        run: npm install -g newman newman-reporter-htmlextra
      
      - name: Run API Tests
        env:
          TRELLO_KEY: ${{ secrets.TRELLO_API_KEY }}
          TRELLO_TOKEN: ${{ secrets.TRELLO_TOKEN }}
        run: |
          newman run trello-api-collection.json \
            --env-var "trellokey=$TRELLO_KEY" \
            --env-var "trellotoken=$TRELLO_TOKEN" \
            --reporters cli,htmlextra \
            --reporter-htmlextra-export testResults.html
      
      - name: Upload Test Results
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: test-results
          path: testResults.html
```

### Setting up Secrets

1. Go to your GitHub repository
2. Settings → Secrets and variables → Actions
3. Add secrets:
   - `TRELLO_API_KEY`: Your Trello API Key
   - `TRELLO_TOKEN`: Your Trello Token

## 📊 Test Results

Example successful run:

```
→ Get all boards
  ✓ Status code is 200

→ Create Board
  ✓ Board created successfully
  ✓ Board has a name
  ✓ Board has a valid URL
  ✓ Board is open (not closed)
  ✓ Board has prefs object with default settings
  ✓ Board has labelNames object
  ✓ Board belongs to an organization

→ Create TODO List
  ✓ TODO list created successfully
  ✓ List name is "TODO"
  ✓ List is open
  ✓ List belongs to the correct board

... (and so on)

┌─────────────────────────┬───────────────────┬───────────────────┐
│                         │          executed │            failed │
├─────────────────────────┼───────────────────┼───────────────────┤
│              iterations │                 1 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│                requests │                10 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│            test-scripts │                20 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│      prerequest-scripts │                11 │                 0 │
├─────────────────────────┼───────────────────┼───────────────────┤
│              assertions │                27 │                 0 │
└─────────────────────────┴───────────────────┴───────────────────┘
```

## 📁 Project Structure

```
trello-api-testing/
├── trello-api-collection.json      # Main test collection
├── trello-environment-example.json # Environment template
├── .gitignore                      # Protects credentials
├── README.md                       # This file
└── .github/
    └── workflows/
        └── api-tests.yml          # CI/CD configuration
```

## 🛡️ Security Best Practices

- ✅ Never commit credentials to Git
- ✅ Use environment variables for sensitive data
- ✅ Keep "Initial Value" empty in collection variables
- ✅ Use GitHub Secrets for CI/CD
- ✅ Rotate API keys regularly
- ✅ Use `.gitignore` to exclude environment files

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

**Isrrael Andres Toro Alvarez**
- GitHub: [@tyraelw](https://github.com/tyraelw)
- LinkedIn: [Isrrael Toro Alvarez](https://www.linkedin.com/in/isrrael-toro-alvarez-1019a4119/)
- Email: tyrael78w@gmail.com

## 📧 Contact

For questions or feedback, please reach out via [tyrael78w@gmail.com](mailto:tyrael78w@gmail.com)

---

⭐ **If you found this helpful, please consider giving it a star!** ⭐