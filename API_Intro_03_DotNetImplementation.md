# API 實作：使用 ASP.NET Core 實作 CRUD (ASP.NET Core Implementation)

**目標：** 將前一個章節學到的 HTTP 方法與 CRUD 概念，具體化為一個可運行的程式碼範例，使用業界主流的 ASP.NET Core 框架。

---

## 💻 實作環境準備

在開始之前，請確保您的開發環境已安裝 .NET SDK，並且了解以下核心概念：

1.  **Controller:** 負責處理 HTTP 請求的類別。
2.  **Action Method:** Controller 內的方法，實際執行業務邏輯。
3.  **Model/DTO:** 用來定義資料結構的類別。

---

## 📂 範例：使用者 (User) 資源的 Controller

我們假設我們要建立一個管理使用者資料的 API，其資源路徑為 `/api/users`。

### 1. 定義資料模型 (Model)

首先，定義一個代表使用者資料的類別。

```csharp
// Models/User.cs
public class User
{
    public int Id { get; set; }
    public string Username { get; set; }
    public string Email { get; set; }
    public string CreatedDate { get; set; }
}
```

### 2. 建立 Controller (Controller)

接下來，我們在 `UsersController.cs` 中使用 `[ApiController]` 屬性，並使用 HTTP 屬性（如 `[HttpGet]`, `[HttpPost]`）來綁定不同的 HTTP 方法。

```csharp
// Controllers/UsersController.cs
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;

[ApiController]
[Route("api/[controller]")] // 設置基礎路由為 /api/users
public class UsersController : ControllerBase
{
    // 模擬資料庫操作的資料集合
    private static List<User> _users = new List<User>
    {
        new User { Id = 1, Username = "alice", Email = "alice@example.com", CreatedDate = DateTime.Now.ToString() }
    };

    // -----------------------------------------------------------------
    // 📚 GET: 讀取 (Read) - 取得所有使用者
    // -----------------------------------------------------------------
    [HttpGet]
    public ActionResult<IEnumerable<User>> GetUsers()
    {
        // 模擬資料庫查詢所有資料
        return Ok(_users); // 返回 200 OK
    }

    // -----------------------------------------------------------------
    // 📚 GET: 讀取 (Read) - 取得特定使用者
    // -----------------------------------------------------------------
    [HttpGet("{id}")]
    public ActionResult<User> GetUserById(int id)
    {
        var user = _users.FirstOrDefault(u => u.Id == id);
        if (user == null)
        {
            return NotFound(); // 返回 404 Not Found
        }
        return Ok(user); // 返回 200 OK
    }

    // -----------------------------------------------------------------
    // 📚 POST: 建立 (Create) - 新增使用者
    // -----------------------------------------------------------------
    [HttpPost]
    public ActionResult<User> CreateUser([FromBody] User newUser)
    {
        // 1. 業務邏輯：生成 ID 並加入資料
        newUser.Id = _users.Any() ? _users.Max(u => u.Id) + 1 : 1;
        newUser.CreatedDate = DateTime.Now.ToString();
        _users.Add(newUser);

        // 2. 最佳實踐：返回 201 Created，並在 Header 中提供新資源的 URI
        return CreatedAtAction(nameof(GetUserById), new { id = newUser.Id }, newUser);
    }

    // -----------------------------------------------------------------
    // 📚 PATCH: 更新 (Update) - 部分更新使用者 (推薦使用 PATCH)
    // -----------------------------------------------------------------
    [HttpPatch("{id}")]
    public ActionResult<User> UpdateUser(int id, [FromQuery] string? status, [FromBody] UserUpdateDto updateData)
    {
        var user = _users.FirstOrDefault(u => u.Id == id);
        if (user == null)
        {
            return NotFound();
        }

        // 模擬部分更新邏輯
        // 範例：如果從 Query 傳入 status，則更新狀態欄位
        if (!string.IsNullOrEmpty(status))
        {
            // 假設我們新增一個 Status 欄位來接收 Query 參數
            // user.Status = status; 
        }
        // 範例：如果從 Body 傳入 Email，則更新 Email
        if (!string.IsNullOrEmpty(updateData.Email))
        {
            user.Email = updateData.Email;
        }
        // 這裡可以加入更多欄位的更新邏輯...

        return Ok(user); // 返回 200 OK
    }

    // -----------------------------------------------------------------
    // 📚 DELETE: 刪除 (Delete) - 刪除使用者
    // -----------------------------------------------------------------
    [HttpDelete("{id}")]
    public StatusCodeResult DeleteUser(int id)
    {
// ...existing code...

    // -----------------------------------------------------------------
    // 📚 DELETE: 刪除 (Delete) - 刪除使用者
    // -----------------------------------------------------------------
    [HttpDelete("{id}")]
    public StatusCodeResult DeleteUser(int id)
    {
        var userToRemove = _users.FirstOrDefault(u => u.Id == id);
        if (userToRemove == null)
        {
            return NotFound();
        }

        _users.Remove(userToRemove);
        return StatusCode(204); // 返回 204 No Content
    }
}

// 輔助 DTO 類別，用於接收部分更新的資料
public class UserUpdateDto
{
    public string? Email { get; set; }
    // 可以在此加入其他可選的欄位
}
```

### 💡 程式碼重點解析

1.  **路由綁定 (`[Route]` / `[HttpGet("{id}")]`):** 屬性讓框架知道哪個方法對應哪個 URL 結構。
2.  **HTTP 狀態碼 (Status Codes):** 這是 API 的「語言」。
    *   `200 OK`: 請求成功。
    *   `201 Created`: 成功建立新資源。
    *   `204 No Content`: 成功執行，但沒有內容回傳（常用於 DELETE）。
    *   `400 Bad Request`: 請求的資料格式錯誤（例如：缺少必要欄位）。
    *   `401 Unauthorized`: 未登入或未提供憑證。
    *   `403 Forbidden`: 已登入，但沒有權限執行此操作。
    *   `404 Not Found`: 找不到指定的資源。
3.  **`CreatedAtAction`:** 在 `POST` 成功後，使用此方法不僅返回 201，還會自動在響應頭部設置 `Location` 標頭，指引客戶端下一步可以訪問的資源 URI，這是 RESTful 設計的標準做法。

---



## 📌 [FromQuery] 與 [FromBody] 屬性說明

在 ASP.NET Core 中，我們可以使用以下屬性來綁定不同的資料來源：

| 屬性       | 說明                         | 使用情境                     |
|------------|------------------------------|------------------------------|
| [FromQuery]  | 綁定查詢字串參數（Query String） | 當請求 URL 帶有 `?key=value` 時使用 |
| [FromBody]   | 綁定請求主體（Request Body）   | 當傳送 JSON 或 XML 資料時使用     |
| [FromRoute]  | 綁定路由參數（Route Parameter）| 當 URL 中包含 `{id}` 類似的動態路由時使用 |

### ✅ 範例：使用 [FromQuery] 獲取查詢參數
```csharp
[HttpGet("/api/users")]
public IActionResult GetUsers([FromQuery] string name, [FromQuery] int page = 1)
{
    // 這裡的 `name` 會對應到 URL 中的 ?name=xxx
    // `page` 有預設值 1
    return Ok();
}
```

### ✅ 範例：使用 [FromBody] 接收 JSON 資料
```csharp
[HttpPost("/api/users")]
public IActionResult CreateUser([FromBody] User user)
{
    // 這裡的 `user` 物件會自動解析來自請求主體的 JSON 資料
    return Ok();
}
```

### ⚠️ 注意事項
- 使用 [FromBody] 時，請求標頭需要包含 `Content-Type: application/json`
- 若同一個參數需要來自多個來源，可以使用 `[FromQuery]`, `[FromBody]`, `[FromRoute]` 等組合
- 可以搭配 `[Bind]` 屬性來指定要綁定的屬性名稱（特別是在處理複雜物件時）

**總結：** 透過 ASP.NET Core 的屬性路由和標準的 HTTP 狀態碼，我們將理論上的 CRUD 操作，轉化為一個結構清晰、符合業界規範的後端服務。

請審閱此草稿。如果內容符合預期，我們將進入下一個主題：安全性議題。