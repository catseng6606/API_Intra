## 📁 API_Intro_06_cURL_Examples.md

### 📌 來自 Query String 的範例
```bash
curl "https://api.example.com/search?query=GitHub&sort=date" 
  -H "X-API-Key: your-secret-api-key"
```

### 📌 來自 POST Body 的範例
```bash
curl -X POST "https://api.example.com/submit" 
  -H "X-API-Key: your-secret-api-key" 
  -H "Content-Type: application/json" 
  -d '{"username":"john_doe", "action":"login"}'
```

### 📌 來自 JSON 檔案的範例
```bash
curl -X PATCH "https://api.example.com/users/123" 
  -H "X-API-Key: your-secret-api-key" 
  -H "Content-Type: application/json" 
  -d @"./user_data.json"
```

---

### 📌 來自傳統 HTML 表單 (Form Data) 的範例
這是模擬 HTML `<form>` 的 `application/x-www-form-urlencoded` 格式。

```bash
curl -X POST "https://api.example.com/login" \
  -H "X-API-Key: your-secret-api-key" \
  -d "username=john_doe" \
  -d "password=my_password"
```

### 📌 來自檔案上傳 (Multipart Form Data) 的範例
這是模擬帶有檔案上傳的表單 `multipart/form-data`。

```bash
curl -X POST "https://api.example.com/upload" \
  -H "X-API-Key: your-secret-api-key" \
  -F "user_id=123" \
  -F "profile_pic=@/path/to/image.jpg"
```

