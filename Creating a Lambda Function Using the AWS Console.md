# AWS Lambda Lab: URL Status Checker

## 📌 Overview
This lab demonstrates how to create and deploy an **AWS Lambda function** using the **AWS Management Console**. The function checks the status of a given URL and returns its HTTP status code, indicating whether the website is up or down.

## ✅ Learning Objectives
1. **Create a Lambda function** in the AWS Management Console using Node.js.
2. **Configure a test event** to invoke the Lambda function.
3. **Review logs in CloudWatch** to analyze execution details.

## 🛠 Steps Performed

### 1. **Create Lambda Function**
- Navigate to **AWS Lambda Console**.
- Click **Create function** → Select **Author from scratch**.
- **Function Name:** `holURLChecker`
- **Runtime:** Node.js 20.x
- **Execution Role:** Create a new role with basic Lambda permissions.
- Click **Create function**.

### 2. **Update Function Code**
Rename `index.mjs` to `index.js` and replace the code with:

```javascript
const https = require('https');
let url = "https://www.amazon.com";

exports.handler = async function(event) {
    let statusCode;
    await new Promise(function(resolve, reject) {
        https.get(url, (res) => {
            statusCode = res.statusCode;
            resolve(statusCode);
        }).on("error", (e) => {
            reject(Error(e));
        });
    });
    console.log(statusCode);
    return statusCode;
};
```

Click **Deploy** to apply changes.

### 3. **Configure Test Event**
- Click **Test** → **Configure test event**.
- **Event Name:** `myTestEvent`
- Save and run the test.
- Expected Output: `200` (Website is up).

### 4. **Monitor Logs in CloudWatch**
- Navigate to **Monitor** → **View logs in CloudWatch**.
- Review START and END Request IDs, Billed Duration, and Status Code.

## ✅ Completion Status
✔ Lambda function created  
✔ Test event configured  
✔ Logs reviewed in CloudWatch  

### 📂 Repository Structure Suggestion
```
aws-lambda-url-checker/
│
├── README.md        # This file
└── screenshots/     # Optional: Add screenshots of steps
```
