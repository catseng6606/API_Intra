# API 介紹：HTTP 方法與 CRUD 關聯 (HTTP Methods & CRUD)

**目標：** 了解在 Web API 中，我們如何使用標準化的 HTTP 方法來對資源（Resource）執行標準的資料操作（CRUD）。

---

## 📚 什麼是 CRUD？

CRUD 是資料庫操作中最基礎的四個動作的縮寫：

*   **C**reate (建立)：新增資料。
*   **R**ead (讀取)：查詢或取得資料。
*   **U**pdate (更新)：修改現有資料。
*   **D**elete (刪除)：移除資料。

---

## 🌐 HTTP 方法與 CRUD 的對應關係

HTTP 協議本身定義了一組標準的「動詞」（Methods），這些動詞完美地對應了 CRUD 的概念。當我們說一個 API 支援 `GET` 方法，我們實際上就是在說它支援「讀取」功能。

| CRUD 操作 | 目的 | 標準 HTTP 方法 | 說明 |
| :---: | :---: | :---: | :--- |
| **Read (讀取)** | 取得資源列表或單一資源 | `GET` | 最常用的方法，用於安全地檢索資料。**不應**在 `GET` 請求中傳送敏感資料。 |
| **Create (建立)** | 在伺服器上新增一個資源 | `POST` | 將資料發送到伺服器，讓伺服器根據資料建立一個新的資源實例。 |
| **Update (更新)** | 修改現有資源的資料 | `PUT` 或 `PATCH` | **`PUT`**: 用來完全替換一個資源的內容。**`PATCH`**: 用來部分修改一個資源的內容（更常用）。 |
| **Delete (刪除)** | 移除一個資源 | `DELETE` | 永久刪除指定的資源。 |

### 💡 關鍵區別點：PUT vs PATCH

*   **PUT**: 假設您要更新一個使用者，您必須傳送**完整的**使用者資料（包含所有欄位），即使您只修改了其中一個欄位。
*   **PATCH**: 您只需要傳送您**想要修改的欄位**及其新值，伺服器會只更新這些欄位。

---

## 🔗 實戰範例：使用者資源 (`/api/users`)

假設我們有一個使用者資源，其 API 端點為 `/api/users`。

1.  **取得所有使用者 (Read All):**
    *   **方法:** `GET`
    *   **URL:** `/api/users`
    *   **範例:** 瀏覽器直接訪問或使用工具呼叫。

2.  **取得特定使用者 (Read One):**
    *   **方法:** `GET`
    *   **URL:** `/api/users/{userId}` (例如：`/api/users/123`)
    *   **範例:** 查詢 ID 為 123 的使用者資料。

3.  **新增使用者 (Create):**
    *   **方法:** `POST`
    *   **URL:** `/api/users`
    *   **Body (請求體):** 包含新使用者的資料 (JSON 格式)。
    *   **回應 (Response):** 通常會回傳新建立的資源，以及一個 `201 Created` 的狀態碼。

4.  **更新使用者 (Update):**
    *   **方法:** `PATCH`
    *   **URL:** `/api/users/{userId}`
    *   **Body (請求體):** 僅包含要修改的欄位 (例如：`{"email": "new_email@example.com"}`)。
    *   **回應 (Response):** 成功後通常回傳更新後的完整資源，以及 `200 OK` 的狀態碼。

5.  **刪除使用者 (Delete):**
    *   **方法:** `DELETE`
    *   **URL:** `/api/users/{userId}`
    *   **回應 (Response):** 成功後通常回傳 `204 No Content`，表示操作成功，但沒有內容需要回傳。

6.  **使用 Query Parameters 篩選和分頁 (GET):**
    *   **方法:** `GET`
    *   **URL:** `/api/users?status=active&page=2&limit=20`
    *   **範例:** 查詢所有狀態為 'active'，且在第 2 頁，每頁顯示 20 筆資料的資料。

---

## 🌐 參數傳遞的類型與差異 (Parameter Passing Types)

在實際的 API 呼叫中，除了 URL 路徑參數 (Path Parameters) 和查詢參數 (Query Parameters) 之外，資料還可以透過以下幾種方式傳遞，理解它們的差異至關重要：

### 1. 路徑參數 (Path Parameters)
*   **用途:** 用於識別資源的唯一 ID 或特定區塊。
*   **範例:** `/api/users/{userId}` 中的 `{userId}`。
*   **特性:** 這是 URL 結構的一部分，通常用於定位單一資源。

### 2. 查詢參數 (Query Parameters)
*   **用途:** 用於過濾 (Filtering)、排序 (Sorting)、分頁 (Pagination) 或傳遞非核心的、可選的篩選條件。
*   **格式:** 附加在 URL 後方，以問號 `?` 開頭，後續參數之間用 `&` 連接。
*   **範例:** `/api/users?status=active&page=2&limit=20`
*   **特性:** 參數是可選的，且不改變資源的本質結構。

### 3. 請求體 (Request Body)
*   **用途:** 用於 `POST`, `PUT`, `PATCH` 等需要傳送複雜資料結構（如建立或更新整個資源）的請求。
*   **格式:** 最常見的是 **JSON** 格式，但也可以是 XML 或 Form Data。
*   **範例:** 建立使用者時，整個使用者物件的資料會放在 Body 中。
*   **特性:** 這是傳遞**資料內容**的主要載體。

### 4. HTTP Header (標頭)
*   **用途:** 用於傳遞**元數據 (Metadata)**，這些資料不屬於資源本身，而是描述請求的「上下文」或「要求」。
*   **特性:** 這是 API 協定層面最底層的資訊交換。**這是放置 API Key 或身份驗證 Token 最常見且安全的地方**。
*   **範例:**
    *   `Authorization: Bearer <JWT_TOKEN>` (用於身份驗證)
    *   `X-API-Key: <YOUR_API_KEY>` (用於傳遞 API Key)
    *   `Content-Type: application/json` (告訴伺服器 Body 內容的格式)
    *   `Accept: application/xml` (告訴伺服器客戶端期望接收的資料格式)


### 5. Form Data (表單資料) vs. JSON Body
初學者常會疑惑「傳統表單」與「現代 JSON API」之間的不同。它們其實分別對應不同的 `Content-Type` 與應用場景：

*   **傳統格式 (Form Data):** 早期網頁最常使用的資料傳遞方式，這也是 HTML `<form>` 標籤的預設行為。
    *   **HTML 範例:**
        ```html
        <!-- 這是傳統 HTML 表單 -->
        <form action="/api/users" method="POST">
          <input type="text" name="username" value="Tom">
          <input type="password" name="password" value="123456">
          <button type="submit">註冊</button>
        </form>
        ```
    *   `application/x-www-form-urlencoded`: 格式為 `username=Tom&password=123456`（格式與 Query 參數相同，但放在 Body 裡發送）。
    *   `multipart/form-data`: 當你需要**「上傳檔案」**(如圖片) 時必須使用的格式。
*   **現代 API 格式 (JSON Body):** 即 `application/json`。
    *   **格式:** 使用 `{ "username": "Tom", "password": "123456" }`。
    *   **優勢:** 能輕鬆表達「複雜 / 巢狀」的資料結構（例如使用者底下有一組標籤 `["student", "vip"]`），這是傳統 Form Data 難以做到的。

*   **💡 給初學者的實戰建議：**
    *   **一般 API 資料傳遞**：請一律優先選擇 **JSON Body**。
    *   **牽涉到「檔案上傳」**：請切換為傳統的 **Form Data (`multipart/form-data`)**。


---

**總結參數傳遞的優先級與用途：**

| 參數類型 | 傳遞內容 | 適用 HTTP 方法 | 核心用途 |
| :---: | :---: | :---: | :--- |
| **Path** | 資源的唯一 ID | GET, PUT, DELETE | 定位資源的**位置**。 |
| **Query** | 篩選、分頁、排序條件 | GET | 描述資源的**範圍**。 |
| **Header** | 認證 Token、內容類型等元數據 | 所有方法 | 描述請求的**上下文**和**要求**。 |
| **Body** | 資源的實際資料內容 | POST, PUT, PATCH | 傳遞資源的**內容**。 |

---

**總結：** 掌握 HTTP 方法與 CRUD 的對應關係，是理解 RESTful API 設計的基石。記住，**動詞 (Method)** 決定了**動作 (Action)**，**資源 (Resource)** 決定了**目標 (Target)**。