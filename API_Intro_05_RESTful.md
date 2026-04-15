# API 設計原則：RESTful (Representational State Transfer)

**目標：** 了解 API 設計的「黃金標準」，從功能層面提升到架構層面的思維度。

---

## 🏛️ 什麼是 RESTful？

REST 是一種**架構風格 (Architectural Style)**，而不是一個具體的協定 (Protocol)。它提供了一組設計指南，指導我們如何設計出「可擴展、易於理解、且一致性高」的 Web API。

一個 API 遵循 RESTful 原則，通常意味著它將**「資源 (Resource)」**作為核心概念，並使用標準的 HTTP 方法來操作這些資源。

---

## 🧱 核心概念：資源 (Resource) 與 URI

在 RESTful 設計中，一切皆是**資源**。資源是任何可以被操作的實體（例如：使用者、文章、訂單）。

*   **資源的識別:** 使用 **URI (Uniform Resource Identifier)** 來唯一標識一個資源或資源的集合。
    *   **好範例 (Collection):** `/api/users` (代表所有使用者資源的集合)
    *   **好範例 (Specific Resource):** `/api/users/123` (代表 ID 為 123 的單一使用者資源)
    *   **壞範例 (非 RESTful):** `/api/getUser?id=123` (將資源和操作混在查詢參數中)

---

## 🔄 資源與 HTTP 方法的組合 (The Combination)

RESTful 的精髓在於將 **資源 (URI)** 和 **動作 (HTTP Method)** 完美結合。

| 目的 (CRUD) | HTTP 方法 | 資源 URI 範例 | 說明 |
| :---: | :---: | :---: | :--- |
| **讀取所有** | `GET` | `/api/products` | 獲取產品列表。 |
| **讀取單一** | `GET` | `/api/products/456` | 獲取 ID 為 456 的產品詳細資料。 |
| **建立** | `POST` | `/api/products` | 將新的產品資料發送到此端點，讓伺服器建立資源。 |
| **更新** | `PATCH` | `/api/products/456` | 部分更新產品的某些屬性（例如：只更新價格）。 |
| **刪除** | `DELETE` | `/api/products/456` | 刪除 ID 為 456 的產品。 |

### 💡 為什麼要這樣設計？

1.  **可預測性 (Predictability):** 任何熟悉 REST 的開發者，看到 `/api/users/123` 搭配 `GET`，都能立刻猜到這是用來讀取使用者資料。
2.  **可擴展性 (Scalability):** 當您需要新增一個功能（例如：評論），您只需要新增一個資源 `/api/comments`，而不是修改現有的 API 結構。

---

## 🌟 總結與最佳實踐

*   **使用名詞 (Nouns) 作為資源名稱:** 永遠用複數名詞來代表資源集合（如 `users`, `products`），而不是動詞（如 `getUsers`, `createProduct`）。
*   **使用動詞 (Verbs) 作為 HTTP 方法:** 讓 HTTP 方法來承擔「動作」的責任。
*   **保持一致性:** 整個 API 應遵循一套統一的命名和操作規則。

遵循 RESTful 原則，能讓您的 API 結構更健壯、更易於維護，並且更符合業界的共識。