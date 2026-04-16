# API 入門總結（01-06）

---

## **01. API 是什麼？（API_Intro_01_WhatIsAPI.md）**
### **定義與功能**
- **API（Application Programming Interface）**：應用程式之間的溝通協議，讓不同系統或服務能夠互相調用功能。
- **目的**：
  - 提供資料存取（如查詢、新增、修改、刪除）。
  - 隱藏複雜的實現細節，簡化使用者開發。
- **例子**：
  - 使用 **Google Maps API** 顯示地圖。
  - 使用 **Twitter API** 發佈推文。

---

## **02. HTTP 方法（API_Intro_02_HTTPMethods.md）**
HTTP 方法用於定義對資源的操作，主要有以下幾種：

| 方法  | 描述                          | 例子用途                     |
|-------|-------------------------------|-----------------------------|
| `GET` | 取得資源（不修改資料）         | 查詢用戶資料：`GET /users/1`  |
| `POST`| 創建新資源（提交資料）         | 新增用戶：`POST /users`       |
| `PUT` | 更新整個資源（或創建）         | 更新用戶資料：`PUT /users/1`  |
| `PATCH`| 部分更新資源（局部修改）       | 修改用戶姓名：`PATCH /users/1`|
| `DELETE`| 刪除資源                     | 刪除用戶：`DELETE /users/1`   |

---

## **03. .NET 實作 API（API_Intro_03_DotNetImplementation.md）**
### **ASP.NET Core Web API**
- **框架**：用於建立 RESTful API 的框架。
- **基本步驟**：
  1. 建立新的 **ASP.NET Core Web API** 專案。
  2. 定義 `Controller` 類別，處理 HTTP 方法。
  3. 使用 `HttpGet`, `HttpPost` 等屬性標記方法。
  4. 返回 JSON 或 XML 數據。

### **範例代碼**
```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetUser(int id)
    {
        return Ok(new { Id = id, Name = "John Doe" });
    }

    [HttpPost]
    public IActionResult CreateUser([FromBody] User user)
    {
        return CreatedAtAction(nameof(GetUser), new { id = 1 }, user);
    }
}
```

---

## **04. 安全性（API_Intro_04_Security.md）**
### **常見安全風險**
- SQL 注入、跨站腳本（XSS）、跨站請求偽造（CSRF）。

### **安全措施**
- **認證（Authentication）**：使用 **JWT（JSON Web Token）** 或 **OAuth**。
- **授權（Authorization）**：角色驗證（如 `[Authorize(Roles = "Admin")]`）。
- **HTTPS**：加密傳輸資料。
- **輸入驗證**：防止 SQL 注入、XSS。

---

## **05. RESTful API 設計原則（API_Intro_05_RESTful.md）**
### **RESTful API 設計原則**
- **資源化**：每個 URL 代表一個資源（如 `/users`）。
- **狀態無關**：每次請求都包含所有必要資訊。
- **標準 HTTP 方法**：使用 `GET`, `POST`, `PUT`, `DELETE`。
- **資源層級**：URL 反映資源的層級關係（如 `/users/{id}/orders`）。
- **HATEOAS**：回應包含連結，指引客戶端下一步操作。

---

## **06. cURL 範例（API_Intro_06_cURL_Examples.md）**
### **cURL 命令**
- **cURL** 是一個命令行工具，用於測試 API。

#### **範例**
```bash
# GET 請求
curl -X GET https://api.example.com/users/1

# POST 請求（帶有 JSON 資料）
curl -X POST https://api.example.com/users \
       -H "Content-Type: application/json" \
       -d '{"name": "Alice", "email": "alice@example.com"}'

# 帶認證的請求
curl -X GET https://api.example.com/protected \
       -H "Authorization: Bearer YOUR_TOKEN"
```

---

## **07. 常用工具介紹**

### **1. Postman**
- **功能**：用於測試、開發和監控 API 的強大工具。
- **特色**：
  - 支援 HTTP 方法（GET, POST, PUT, DELETE 等）。
  - 收藏夾與環境變數管理。
  - 自動化測試與報告。
  - 集成 RESTful API 文檔生成。
- **範例**：
  ```bash
  # 在 Postman 中新增一個 GET 請求
  GET https://api.example.com/users/1
  ```

---

### **2. Swagger/OpenAPI**
- **功能**：用於文檔化和測試 RESTful API 的工具。
- **特色**：
  - 自動生成 API 文檔。
  - 支援多種語言（如 Python, Java, Node.js）。
  - 集成在開發環境中，方便開發者使用。
- **範例**：
  ```yaml
  # Swagger 文檔範例
  paths:
    /users:
      get:
        summary: Get all users
        responses:
          200:
            description: A list of users
  ```

---

### **3. Insomnia**
- **功能**：類似 Postman 的 API 測試工具，支援 RESTful API 和 GraphQL。
- **特色**：
  - 支援多種協議（HTTP, WebSocket, GraphQL）。
  - 環境變數與資料庫同步。
  - 支援自動化測試與報告。
- **範例**：
  ```bash
  # 在 Insomnia 中新增一個 POST 請求
  POST https://api.example.com/users
  ```

---

### **4. cURL**
- **功能**：命令行工具，用於測試 API。
- **特色**：
  - 支援 HTTP 方法（GET, POST, PUT, DELETE 等）。
  - 簡單快速，適合自動化測試。
- **範例**：
  ```bash
  # 帶認證的 POST 請求
  curl -X POST https://api.example.com/users \
       -H "Authorization: Bearer YOUR_TOKEN" \
       -H "Content-Type: application/json" \
       -d '{"name": "Alice", "email": "alice@example.com"}'
  ```

---

### **5. JMeter**
- **功能**：用於負載測試和性能分析的工具。
- **特色**：
  - 支援 HTTP、HTTPS、JDBC、LDAP 等協議。
  - 可視化報告與分析。
  - 支援分佈式負載測試。
- **範例**：
  - 在 JMeter 中新增 HTTP Request 並設定負載測試。

---

### **6. Charles Proxy**
- **功能**：用於監控 HTTP/HTTPS 通訊的代理工具。
- **特色**：
  - 監控 API 通訊細節（請求、回應、頭資訊）。
  - 支援 HTTPS 解密。
  - 支援自動化測試與錄製。
- **範例**：
  - 在 Charles Proxy 中錄製 API 請求並檢查回應。

---

### **7. API 文檔工具（如 ReadMe, Postman 文檔）**
- **功能**：用於生成和管理 API 文檔。
- **特色**：
  - 支援 Markdown 或 JSON 格式。
  - 支援自動化文檔更新。
  - 支援多語言文檔。
- **範例**：
  ```markdown
  # API 文檔範例
  ## 用戶資源
  - **GET /users/{id}**：取得特定用戶資料。
  - **POST /users**：新增用戶。
  ```

---

### **總結**
- **Postman/Swagger/OpenAPI**：用於 API 開發與文檔化。
- **Insomnia/cURL**：用於 API 測試。
- **JMeter/Charles Proxy**：用於負載測試與監控。