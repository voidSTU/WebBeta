# WebBeta  
### BCTP Performance Demonstration Version

**WebBeta** is the demonstration version of **BCTP (Bullying Consequence Transparency Platform)**.

This version is designed for **presentation and performance demonstration purposes**, providing a faster interaction flow than the standard BCTP platform.

The primary difference is the **quiz system**:  
correct answers are displayed immediately after each question, allowing direct feedback and smoother demonstrations.

---

## Project Website

<a href="https://voidstu.github.io/WebBeta/">
  <img width="200" height="200" src="https://github.com/user-attachments/assets/4baa18b3-a930-4954-98ea-649a51a6e818" />
</a>

**Live Demo:**  
https://voidstu.github.io/WebBeta/

---

# Key Difference from Original BCTP

### Standard BCTP
- Requires users to complete quizzes independently
- Measures actual comprehension
- Focuses on educational assessment

### WebBeta
- Displays correct answers immediately
- Provides instant feedback
- Optimized for demonstrations and performance presentations

---

# Project Structure

## Front-end (GitHub Pages)

Built using static **HTML, CSS, and JSON-based content files**.

---

## Data Files

### [LawJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/LawJson.json)
Contains legal regulations and consequences linked to bullying behaviors.

### [CaseJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/CaseJson.json)
Contains real-world bullying cases for educational reference.

### [QuizJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/QuizJson.json)
Stores quiz questions generated according to selected behaviors.

---

## Pages

### [index.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/index.html)
Introduces the platform purpose and explains the system workflow.

---

### [query.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/query.html)
Allows users to select bullying behaviors and review:

- Related laws
- Legal consequences
- Real-life cases

---

### [quiz.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/quiz.html)

Knowledge assessment page.

Features:

- Dynamically generated questions
- Based on selected behavior data
- Passing score: **60 points or above**

**Beta Demonstration Feature:**  
Correct answers are revealed immediately after each question without confirmation, enabling faster system presentation.

---

### [form.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/form.html)

Reflection submission page where users provide:

- School name
- Student name
- Reflection content
- Selected bullying behavior

The submission is then recorded in the backend system.

---

## Styling

### [style.css](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/style.css)

Controls:

- Page layout
- Typography
- Responsive design
- Visual consistency

---

## Platform Logo

<img width="200" height="200" src="https://github.com/user-attachments/assets/5079d688-579e-4c70-98d7-e8642c83255e" />

---

# Back-end (Google Apps Script)

The backend receives form data submitted from `form.html` and records:

- Timestamp
- School Name
- Student Name
- Reflection Content
- Behavior Type

All records are stored using **Google Sheets + Apps Script**.

---

## Apps Script Code

```javascript
const SHEET_NAME = "record";  // 設定要操作的試算表工作表名稱

function doPost(e) {
  const responseJson = (data) => 
    ContentService.createTextOutput(JSON.stringify(data))
                  .setMimeType(ContentService.MimeType.JSON);

  // 檢查是否收到有效的 POST 請求數據，若沒有則返回錯誤
  if (!e || !e.postData || !e.postData.contents) {
    return responseJson({result: 'error', message: '未收到任何數據'});
  }
  
  let data;
  try {
    // 解析收到的 JSON 數據
    data = JSON.parse(e.postData.contents);
  } catch (error) {
    return responseJson({result: 'error', message: '無效的 JSON 格式'});
  }

  try {
    const ss = SpreadsheetApp.getActiveSpreadsheet();
    const sheet = ss.getSheetByName(SHEET_NAME);
    
    // 檢查工作表是否存在
    if (!sheet) {
        return responseJson({result: 'error', message: `找不到名為 "${SHEET_NAME}" 的工作表。`});
    }
    
    // 準備數據並將其寫入工作表
    const rowData = [
      new Date(),
      data.school,
      data.studentName,
      data.reflection,
      data.behaviors
    ];

    sheet.appendRow(rowData);
    return responseJson({result: 'success', status: '數據已成功記錄'});

  } catch (error) {
    return responseJson({result: 'error', message: `伺服器錯誤: ${error.toString()}`});
  }
}
```

---

# Workflow Overview

1. User enters platform  
2. Selects bullying behavior  
3. Reviews laws and real cases  
4. Completes quiz with instant-answer feedback  
5. Achieves passing score (60+)  
6. Writes reflection report  
7. Reflection is stored in Google Sheets

---

# Purpose

WebBeta demonstrates the core BCTP workflow in a faster, presentation-friendly format.

It is designed for:

- Live demonstrations
- Project showcases
- Performance evaluation
- System testing

while preserving the educational structure of the original platform.
