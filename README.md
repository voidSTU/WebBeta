# BCTP  
**Bullying Consequence Transparency Platform**

## GitHub Project / Front-end

### Project Website
[![image](https://hackmd.io/_uploads/ry1SGNS4Wx.png)
](https://voidstu.github.io/BCTP/)

---

### JSON Files

[LawJson.json](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/LawJson.json)  
Contains laws related to different bullying behaviors.

[CaseJson.json](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/CaseJson.json)  
Contains real cases related to different bullying behaviors.

[QuizJson.json](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/QuizJson.json)  
Quiz data related to bullying laws and cases, used to test user understanding.

---

### HTML Files

[index.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/index.html)  
Introduces the purpose of the platform and explains what the system aims to do.

[query.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/query.html)  
Allows the user (bully) to select the behavior they have committed, then read the related laws and view corresponding cases.

[quiz.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/quiz.html)  
Tests whether the user has studied the content carefully.  
The quiz questions are generated based on the data shown on the `query.html` page.  
The user must score **over 60 points** to proceed to `form.html`.

[form.html](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/form.html)  
Allows the user (bully) to write a reflection on their bullying behavior and upload it to the record system.

---

### CSS Files

[style.css](https://raw.githubusercontent.com/voidSTU/BCTP/refs/heads/main/style.css)  
Defines the layout and visual style of the HTML pages.

---

### Logo
(Platform logo design used in the website)
![LogoJPG](https://hackmd.io/_uploads/B1HGMVS4Zg.jpg)

---

## Google Sheets Apps Script / Back-end

Receives data sent from `form.html`, including:
- Timestamp  
- School Name  
- Student Name  
- Reflection Content  
- Behavior Type  

All data is stored and managed using Google Sheets via Apps Script.

```javascript=
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
