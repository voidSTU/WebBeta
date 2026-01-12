# WebBeta: BCTP for Performance

(Bullying Consequence Transparency Platform – Beta Version)

The **Beta version** quiz page is different from the original version.  
This version focuses on faster and more direct performance feedback.

---

## GitHub Project / Front-end

### Project Website
<a href="https://voidstu.github.io/WebBeta/">
  <img width="200" height="200" alt="upload_f86deddc586149676dc228396aff69b0" src="https://github.com/user-attachments/assets/4baa18b3-a930-4954-98ea-649a51a6e818" />
</a>

---

### JSON Files

[LawJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/LawJson.json)  
This file stores laws that are linked to different bullying behaviors.

[CaseJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/CaseJson.json)  
This file stores real cases that are linked to different bullying behaviors.

[QuizJson.json](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/QuizJson.json)  
This file stores quiz questions related to bullying laws and cases.

---

### HTML Files

[index.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/index.html)  
This page explains the purpose of the platform and tells the user (the bully) what the system is designed to do.

[query.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/query.html)  
On this page, the user (the bully) chooses the bullying behavior they have committed.  
The system then displays related laws and cases based on the selected behavior.

[quiz.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/quiz.html)  
This page tests whether the user has carefully studied the content shown on the `query.html` page.  
The quiz content is generated from the selected behavior.  
The user must score **over 60 points** to proceed to `form.html`.

In this **Beta version**, the correct answer is shown immediately after each question without confirmation.  
This design allows the system to demonstrate performance more directly and efficiently.

[form.html](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/form.html)  
On this page, the user (the bully) writes a reflection on their bullying behavior.  
The reflection is then uploaded and saved as a record.

---

### CSS File

[style.css](https://raw.githubusercontent.com/voidSTU/WebBeta/refs/heads/main/style.css)  
This file controls the layout and visual design of the HTML pages.

---

### Logo

<img width="200" height="200" alt="WebBetaLogo" src="https://github.com/user-attachments/assets/5079d688-579e-4c70-98d7-e8642c83255e" />

---

## Google Sheets Apps Script / Back-end

~~~javascript
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

~~~

This back-end script receives the **timestamp, school name, student name, reflection content, and behavior type** sent from `form.html`, and records them into Google Sheets.
