# 常用 API 工具介紹

## 1. Postman
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

## 2. Swagger/OpenAPI
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

## 3. Insomnia
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

## 4. cURL
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

## 5. JMeter
- **功能**：用於負載測試和性能分析的工具。
- **特色**：
	- 支援 HTTP、HTTPS、JDBC、LDAP 等協議。
	- 可視化報告與分析。
	- 支援分佈式負載測試。
- **範例**：
	- 在 JMeter 中新增 HTTP Request 並設定負載測試。

## 6. Charles Proxy
- **功能**：用於監控 HTTP/HTTPS 通訊的代理工具。
- **特色**：
	- 監控 API 通訊細節（請求、回應、頭資訊）。
	- 支援 HTTPS 解密。
	- 支援自動化測試與錄製。
- **範例**：
	- 在 Charles Proxy 中錄製 API 請求並檢查回應。

## 7. API 文檔工具（如 ReadMe, Postman 文檔）
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

## 總結
- **Postman/Swagger/OpenAPI**：用於 API 開發與文檔化。
- **Insomnia/cURL**：用於 API 測試。
- **JMeter/Charles Proxy**：用於負載測試與監控。
