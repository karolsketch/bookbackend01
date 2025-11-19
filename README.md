# bookbackend01
# 📚 Online Bookstore Backend 
## 📌 1. 專案簡介
本專案是一套「線上書店後端系統」，提供：
書籍查詢
會員註冊 / 登入（含正規化驗證、密碼加密、Email 寄送）
付款方式管理
建立訂單（含小計、運費、總金額運算）
查詢訂單列表、訂單明細
後端使用 RESTful API，前端可自由串接
---
## 📌 2. 目錄（Table of Contents）
你可以按住 ⌘ + F 或 Ctrl + F 搜尋：
1. 專案簡介
2. 使用技術與工具
3. 系統架構圖
4. 專案資料夾結構
5. 資料庫 Schema
6. 功能流程
7. API 一覽
8. Eclipse 開發步驟
9. GitHub 使用說明（新手版）
10. Markdown 排版教學（標題/字體/連結/圖片）
---
## 📌 3. 系統架構圖（分層架構）
Controller (API 出入口)  
↓ 接收 Request/回傳 Response

Service（商業邏輯層）
↓ 核心流程：註冊/登入/訂單流程/金額計算

Repository（資料庫操作層）
↓ JPA 實作查詢 / 新增 / 更新

Entity（資料表對應模型）

DTO（資料傳輸物件：Request / Response）
---
📌 4. 專案資料夾結構
src/main/java/com/demo
├── Bookbackend01Application.java     # Spring Boot 啟動主程式
│
├── entity                            # JPA 實體（對應資料表）
│   ├── Book.java
│   ├── BookCategory.java
│   ├── Member.java
│   ├── PaymentMethod.java
│   ├── Order.java
│   ├── OrderDetail.java
│   └── OrderAmount.java
│
├── repository                        # JPA Repository（DAO）
│   ├── BookRepository.java
│   ├── MemberRepository.java
│   ├── PaymentMethodRepository.java
│   ├── OrderRepository.java
│   ├── OrderDetailRepository.java
│   └── OrderAmountRepository.java
│
├── service                           # Service 層（商業邏輯）
│   ├── MemberService.java
│   ├── BookService.java
│   ├── EmailService.java
│   └── OrderService.java
│
├── controller                        # API 控制器
│   ├── MemberController.java
│   ├── BookController.java
│   ├── PaymentMethodController.java
│   └── OrderController.java
│
├── dto                               # Request / Response DTO
│   ├── MemberRegisterRequest.java
│   ├── MemberLoginRequest.java
│   ├── CreateOrderRequest.java
│   ├── CreateOrderResponse.java
│   ├── OrderSummaryResponse.java
│   └── OrderDetailViewResponse.java
│
└── config                            # Config 設定
    └── SecurityConfig.java           # PasswordEncoder 設定
## 📌 6. 功能流程
✔️ 6.1 會員註冊流程
1. Controller 接收 JSON
2. DTO 驗證（帳號/Email/手機不可重複、密碼正規化）
3. 密碼使用 BCrypt 雜湊
4. 寫入資料庫
5. 寄送註冊成功 Email
6. 回傳成功 JSON 給前端
✔️ 6.2 登入流程
1. 使用者輸入帳號或 Email
2. 從 DB 找會員
3. 使用 BCrypt matches() 比對密碼
4. 回傳 MemberResponse
✔️ 6.3 書籍查詢流程
/api/books
/api/books/{id}
/api/books/search?keyword=xxx
✔️ 6.4 建立訂單流程
1. 檢查會員存在
2. 檢查付款方式存在
3. 檢查每本書是否存在、庫存足夠
4. 計算小計 subtotal
5. 運費判斷（滿 500 免運，不滿收 60）
6. 建立 Orders
7. 建立 OrderDetail（多筆）
8. 庫存扣除（Book.StockQty -= 購買數量）
9. 建立 OrderAmount
10. 回傳訂單資訊 JSON
✔️ 6.5 查詢訂單列表
GET /api/orders/member/{memberId}

OrderSummaryResponse：
- 訂單編號
- 日期
- 付款方式
- 小計
- 總金額
- 明細數量
✔️ 6.6 查詢單一訂單明細
GET /api/orders/{orderId}

OrderDetailViewResponse：
- 訂單資訊（會員、地址、備註）
- 金額資訊
- 明細列表（書名 / 價格 / 數量）


📌 7. API 一覽表
| Method | URL                            | 說明   |
| ------ | ------------------------------ | ---- |
| GET    | `/api/books`                   | 全部書籍 |
| GET    | `/api/books/{id}`              | 單一本書 |
| GET    | `/api/books/search?keyword=xx` | 書名搜尋 |

👤 會員 Member API
| Method | URL                     | 說明 |
| ------ | ----------------------- | -- |
| POST   | `/api/members/register` | 註冊 |
| POST   | `/api/members/login`    | 登入 |

💳 付款方式 PaymentMethod API
| Method | URL                         | 說明     |
| ------ | --------------------------- | ------ |
| GET    | `/api/payment-methods`      | 全部付款方式 |
| GET    | `/api/payment-methods/{id}` | 單一付款方式 |

🧾 訂單 Order API
| Method | URL                             | 說明     |
| ------ | ------------------------------- | ------ |
| POST   | `/api/orders`                   | 建立訂單   |
| GET    | `/api/orders/member/{memberId}` | 會員訂單列表 |
| GET    | `/api/orders/{orderId}`         | 單一訂單明細 |

📌 8. Eclipse 開發步驟（從零開始）
1. 建立 Spring Boot 專案
2. 加入依賴：web, jpa, validation, mail, mysql-connector-j, security
3. 建立 application.properties 設定 DB / Mail
4. 建立 entity（資料庫模型）
5. 建立 repository（資料查詢）
6. 建立 service（商業邏輯）
7. 建立 DTO（前後端交換資料格式）
8. 建立 controller（提供 API）
9. 用 Postman 測試每個 API
10. 推到 GitHub
