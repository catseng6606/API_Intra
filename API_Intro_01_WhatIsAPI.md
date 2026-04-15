# API 簡介：應用程式介面是什麼？ (What is an API?)

**API** (Application Programming Interface) 簡而言之，就是一套定義好如何溝通的**「介面」或「合約 (Contract)」**。它定義了不同軟體元件（例如，你的網站前端和後端伺服器）之間可以互相呼叫和交換資料的規則。

---

## 🍴 實例比喻：餐廳的服務生 (The Waiter Analogy)

想像你到一家餐廳吃飯：

*   **你 (Client)：** 你需要一份食物（資料）。你不知道廚房（Server）是如何烹飪的，也不知道厨房的複雜結構。
*   **廚房 (Server)：** 這是實際處理業務的地方，知道所有菜單（功能）和烹飪步驟（邏輯）。
*   **服務生 (API)：** 你不需要直接進去廚房。你只需要對服務生點餐，告訴他：「我要一份牛排，中等熟度」。服務生就是這個「合約」。他負責將你的需求（Request）轉譯成廚房能懂的指令，拿到成品（Response）後，再轉譯成你能理解的樣貌交給你。

**API 的核心價值就是「隔離 (Decoupling)」**：它讓你不用知道後端複雜的運作細節，只需知道「透過這個介面，我能拿到什麼」。

---

## 🧩 什麼是 API？ (The Technical Definition)

從技術層面看，API 規定了：

1.  **端點 (Endpoints)：** 某個明確的網路地址，代表一個資源或一個功能。例如：`/api/users`。
2.  **方法 (Methods)：** 用來指定你對該資源想要執行的動作（例如：取得、建立、更新）。
3.  **格式 (Format)：** 資料必須遵循的結構，最常見的是 JSON 或 XML。

### ⚙️ 參數傳遞的技術層面 (Technical Parameters)

在實際的網路通訊中，API 的「合約」不僅僅是端點和方法，還必須定義資料傳遞的**載體 (Carrier)** 和**結構 (Structure)**。

1.  **路徑參數 (Path Parameters):**
    *   **用途:** 用於在 URL 路徑中定位特定的資源實例。
    *   **範例:** `/api/users/{userId}`。這裡的 `{userId}` 就是一個路徑參數，它直接嵌入到 URL 結構中。

2.  **查詢參數 (Query Parameters):**
    *   **用途:** 用於傳遞可選的、用於過濾、分頁或排序的條件。
    *   **格式:** 附加在 URL 後方，以 `?` 開頭，後續參數之間用 `&` 連接。
    *   **範例:** `/api/users?status=active&page=2&limit=20`。

3.  **請求體 (Request Body):**
    *   **用途:** 用於 `POST`, `PUT`, `PATCH` 等需要傳送複雜資料結構的請求。
    *   **格式:** 最常見的是 **JSON** (JavaScript Object Notation)，它是一種輕量級的資料交換格式。
    *   **範例:** 建立使用者時，整個使用者物件的資料會放在 Body 中。

4.  **HTTP Header (標頭):**
    *   **用途:** 用於傳遞**元數據 (Metadata)**，這些資料不屬於資源本身，而是描述請求的「上下文」或「要求」。
    *   **範例:**
        *   `Authorization: Bearer <JWT_TOKEN>` (用於身份驗證)
        *   `Content-Type: application/json` (告訴伺服器 Body 的格式)
    *   **特性:** 這是 API 協定層面最底層的資訊交換，對於安全性和格式協商至關重要。

**總結來說，一個完整的 API 請求，是透過組合這些元素（Method + Endpoint + Parameters + Body/Header）來完成的。**