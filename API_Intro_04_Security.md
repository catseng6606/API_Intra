# API 安全性議題 (Security Concerns)

**目標：** 了解在實際部署的 API 中，除了功能性之外，還必須考慮哪些安全層面。

---

## 🔒 1. 身份驗證 (Authentication) vs. 授權 (Authorization)

這是初學者最容易混淆的概念，但卻是安全的核心。

*   **身份驗證 (Authentication - AuthN):** **「你是誰？」** 驗證使用者或服務的身份。
    *   **常見機制:** API Key, Basic Auth, **JWT (JSON Web Token)**。
    *   **實作點:** 伺服器必須在接收到請求時，檢查傳入的 Token 是否有效、是否過期，並從中解析出使用者身份。

*   **授權 (Authorization - AuthZ):** **「你有權限做什麼？」** 驗證已驗證的身份是否有權限執行該操作。
    *   **範例:** 只有「管理員 (Admin)」角色才能呼叫 `DELETE /api/users/{id}` 這個端點。
    *   **實作點:** 在 Controller 層級或 Middleware 中，根據使用者角色（Role）來判斷是否允許存取資源。

---

## 🌐 2. 跨來源資源共享 (CORS: Cross-Origin Resource Sharing)

**這是部署時最常遇到的跨域問題。**

*   **問題描述:** 瀏覽器預設有「同源政策 (Same-Origin Policy)」，目的是防止惡意網站竊取資料。如果您的前端網址是 `http://localhost:3000`，而您的 API 伺服器在 `http://localhost:5000`，瀏覽器會阻止前端直接呼叫後端，除非後端明確允許。
*   **解決方案:** 後端 API 伺服器必須在回應的 HTTP Header 中加入 `Access-Control-Allow-Origin` 等標頭，明確告訴瀏覽器：「我允許來自 `http://localhost:3000` 的請求。」
*   **實作建議:** 在 ASP.NET Core 中，通常是透過配置 `Startup.cs` 或 `Program.cs` 來啟用和配置 CORS 中間件。

---

## 🛡️ 3. 輸入驗證 (Input Validation)

**永遠不要相信客戶端傳來的資料。**

*   **風險:** 如果前端沒有做驗證，惡意使用者可以直接發送格式錯誤、過長或包含惡意代碼的資料。
*   **實作:** 必須在後端（Controller/Service 層）進行嚴格的資料類型、長度、格式（如 Email 格式）檢查。
*   **範例:** 確保 `Email` 欄位確實符合標準的電子郵件正規表達式。

---

## 🔑 2. API Key 驗證實作

### 🛠️ 實作步驟

1. **新增 API Key 中間件 (Middleware)**
```csharp
// Startup.cs 或 Program.cs
app.Use(async (context, next) =>
{
    var apiKey = context.Request.Headers["X-API-Key"];
    
    if (apiKey != "your-secret-api-key")
    {
        context.Response.StatusCode = 401;
        await context.Response.WriteAsync("Invalid API Key");
        return;
    }

    await next();
});
```

2. **使用 [ApiKey] 屬性驗證**
```csharp
[ApiController]
[Route("[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet]
    [ApiKey]
    public IActionResult GetUsers()
    {
        return Ok(new[] { new { Id = 1, Name = "John" }, new { Id = 2, Name = "Jane" } });
    }
}
```

### ⚠️ 注意事項
- API Key 應該儲存在伺服器端，**不要硬碼在客戶端程式碼中**
- 建議搭配 **JWT** 使用，API Key 可作為第一層防禦，JWT 作為第二層驗證
- 定期輪換 API Key 以降低風險
- 可將 API Key 存於 **環境變數** 或 **配置檔** 中（如 `appsettings.json`）

---

## 🌀 3. 使用 cURL 測試 API 的基本語法

### 📌 常見指令格式
```bash
# GET 請求示例
curl -X GET \n  "https://api.example.com/users" \n  -H "X-API-Key: your-secret-api-key"

# POST 請求示例
curl -X POST \n  "https://api.example.com/users" \n  -H "X-API-Key: your-secret-api-key" \n  -H "Content-Type: application/json" \n  -d '{"name":"John", "email":"john@example.com"}'
```

### 📌 常用參數說明
| 參數 | 說明 |
|------|------|
| `-X` | 指定 HTTP 方法 (GET/POST/PUT/DELETE) |
| `-H` | 添加 HTTP Headers (如 API Key、Content-Type) |
| `-d` | 設定 POST/PUT 請求的 Body 內容 |
| `--verbose` | 顯示詳細的請求/回應資訊 (用於除錯) |

### ⚠️ 安全注意事項
- **不要在公開的腳本中硬碼 API Key**，應使用環境變數（如 `export API_KEY=your-secret-key`）
- 使用 `--insecure` 時要特別小心（會忽略 SSL 憑證驗證）
- 敏感資料建議使用 `--data-binary` 來傳輸二進位資料

---

## 🔑 總結安全流程

當一個請求到達 API 時，安全檢查的順序通常是：

1.  **CORS 檢查:** 檢查來源網域是否被允許。
2.  **身份驗證 (AuthN):** 檢查 Token 是否有效，確定「你是誰」。
3.  **授權 (AuthZ):** 檢查角色權限，確定「你有權限做什麼」。
4.  **輸入驗證:** 檢查傳入的資料是否符合業務規則。
5.  **業務邏輯執行:** 執行核心的 CRUD 操作。