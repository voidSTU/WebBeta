# WebBeta  
### BCTP Demonstration Version  
**Bullying Consequence Transparency Platform – Beta Demo**

A demonstration version of **BCTP** designed for faster system presentation.

Unlike the original version, this beta directly displays quiz answers to allow users to experience the complete platform workflow more quickly during demonstrations.

---

## Project Website

<a href="https://voidstu.github.io/WebBeta/">
  <img width="200" height="200" src="https://github.com/user-attachments/assets/4baa18b3-a930-4954-98ea-649a51a6e818" />
</a>

**Live Demo:**  
https://voidstu.github.io/WebBeta/

---

# Project Structure

## Front-end (GitHub Pages)

The front-end is built using static HTML, CSS, and JSON-driven content rendering.

---

## Data Files

### [LawJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/LawJson.json)
Contains legal regulations associated with different bullying behaviors.

### [CaseJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/CaseJson.json)
Contains real-world bullying-related case references.

### [QuizJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/QuizJson.json)
Stores quiz questions generated according to selected bullying behavior.

---

## Pages

### [index.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/index.html)
Introduces the platform purpose and explains the system workflow.

---

### [query.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/query.html)
Allows users to select bullying behavior and review:

- Related laws
- Legal consequences
- Real-world cases

---

### [quiz.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/quiz.html)

Knowledge assessment page.

Features:

- Questions generated dynamically from selected behavior
- Passing score requirement: **60 points or above**
- Immediate answer display after each question

### Demo Version Difference

Unlike the original BCTP version:

- Answers are displayed immediately
- No confirmation delay
- Faster access to the next stage

This design is intended only for demonstration and presentation purposes.

---

### [form.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/form.html)

Reflection submission page for recording:

- School name
- Student name
- Reflection content
- Selected behavior type

Submitted data is stored in the record system.

---

## Styling

### [style.css](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/style.css)

Controls:

- Layout structure
- Responsive interface behavior
- Visual consistency

---

## Platform Logo

<img width="200" height="200" src="https://github.com/user-attachments/assets/5079d688-579e-4c70-98d7-e8642c83255e" />

---

# Back-end (Google Apps Script)

The platform uses **Google Apps Script + Google Sheets** as a lightweight backend storage system.

Submitted data includes:

- Timestamp
- School Name
- Student Name
- Reflection Content
- Behavior Type

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

1. User enters the platform  
2. Selects bullying behavior  
3. Reviews legal information and cases  
4. Completes quiz assessment  
5. Receives immediate answer display  
6. Achieves passing score (60+)  
7. Writes reflection report  
8. Reflection is stored in Google Sheets

---

# Purpose

This beta version is intended for:

- System demonstrations
- Presentation use
- Faster walkthrough of platform features

It preserves the original educational workflow while reducing interaction time for easier demonstration.
