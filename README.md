# N8N

- Đây là 1 số Workflow cơ bản để mọi người có thể import và nghiên cứu cách thức hoạt động chuyển đổi các Sub Workflow thành 1 MCP Server để call từ các Workflow chính cho gọn gàng sạch sẽ, và đặc biệt là có thể thêm vào MCP trên Home Assistant để làm cho Assistant Hass thông minh hơn, đỡ phải code nhiều
- Đặc biệt là các Node Http Request cơ bản, các bạn có thể kham khảo để hiểu về cách thức lấy dữ liệu, by pass captcha, và các Node Code cơ bản để lọc dữ liệu trả về kết quả sạch sẽ gọn gàng theo chuẩn cấu trúc JSON dể giúp AI đọc hiểu dễ dàng hơn, và đỡ tốn TOKEN hơn

---

# Workflow Zalo Chat AI - N8N

## Giới thiệu

Workflow "Zalo Chat AI" là một hệ thống trí tuệ nhân tạo hội thoại được xây dựng trên nền tảng n8n, tích hợp với Zalo Bot để cung cấp trải nghiệm trò chuyện thông minh. Workflow này hoạt động như một trợ lý ảo cá nhân, có khả năng xử lý tin nhắn văn bản, hình ảnh và giọng nói, đồng thời tích hợp với nhiều công cụ và nguồn dữ liệu khác nhau.

## Tính năng chính

Workflow Zalo Chat AI cung cấp các tính năng sau:

1. **Xử lý đa dạng định dạng tin nhắn**:
   - Tin nhắn văn bản (text)
   - Tin nhắn hình ảnh (photo)
   - Tin nhắn giọng nói (voice)

2. **Tích hợp nhiều mô hình AI**:
   - Google Gemini: Mô hình chính để xử lý tin nhắn
   - DeepSeek: Mô hình dự phòng
   - Gemini Speech-to-Text: Chuyển đổi tin nhắn giọng nói thành văn bản

3. **Tích hợp công cụ và dịch vụ**:
   - Truy cập Google (tìm kiếm, Gmail, Calendar, Drive)
   - Tra cứu thông tin (phạt nguội, xổ số, đăng kiểm, lịch âm dương)
   - Tải xuống và chia sẻ tài liệu từ Google Drive
   - Truy vấn cơ sở dữ liệu tin tức và tin nhắn nhóm

4. **Bộ nhớ hội thoại**:
   - Lưu trữ lịch sử trò chuyện trong cơ sở dữ liệu PostgreSQL
   - Duy trì ngữ cảnh giữa các lần trò chuyện

## Cấu trúc workflow

Workflow bao gồm các thành phần chính sau:

1. **Webhook**: Điểm khởi đầu, nhận tin nhắn từ Zalo Bot
2. **to json**: Chuyển đổi dữ liệu từ webhook thành định dạng JSON chuẩn
3. **Các node If**: Phân loại loại tin nhắn (text, image, voice)
4. **stt**: Chuyển đổi giọng nói thành văn bản (khi nhận tin nhắn voice)
5. **AI Agent**: Xử lý yêu cầu và tạo phản hồi thông minh
6. **maxLen**: Xử lý tin nhắn dài, chia thành nhiều phần nếu cần
7. **Call a service**: Gửi phản hồi về Zalo

## Quy trình xử lý

### 1. Nhận và phân tích tin nhắn

- **Webhook** nhận dữ liệu từ Zalo Bot khi có tin nhắn mới
- **to json** chuyển đổi dữ liệu thành cấu trúc JSON chuẩn
- Các node **If** phân loại tin nhắn dựa trên loại (text, image, voice)
- Chỉ xử lý tin nhắn có chứa "@n8n" và từ người dùng cụ thể (ID: 8527626492165203115)

### 2. Xử lý tin nhắn theo loại

#### Tin nhắn văn bản
- Chuyển trực tiếp đến AI Agent để xử lý

#### Tin nhắn hình ảnh
- Chuyển URL hình ảnh và caption đến AI Agent để phân tích

#### Tin nhắn giọng nói
- Sử dụng **stt** (Gemini Speech-to-Text) để chuyển đổi thành văn bản
- Chuyển văn bản đã chuyển đổi đến AI Agent

### 3. Xử lý yêu cầu với AI

- **AI Agent** nhận tin nhắn đã xử lý
- Sử dụng **Google Gemini** làm mô hình chính và **DeepSeek** làm mô hình dự phòng
- Tích hợp **memory** để duy trì ngữ cảnh hội thoại
- Sử dụng các công cụ tích hợp:
  - **google**: Tìm kiếm và truy cập dịch vụ Google
  - **tracuu**: Tra cứu thông tin đặc biệt
  - **download**: Tải và chia sẻ tài liệu

### 4. Tạo và gửi phản hồi

- **maxLen** xử lý phản hồi dài, chia thành nhiều phần nếu cần
- **Call a service** gửi phản hồi về Zalo Bot
- Phản hồi được gửi đến người dùng trong cuộc trò chuyện Zalo

## Cấu hình hệ thống

- **Mô hình AI**: Google Gemini và DeepSeek
- **Bộ nhớ**: PostgreSQL lưu trữ lịch sử hội thoại trong bảng `histories_private`
- **Tích hợp Home Assistant**: Để giao tiếp với Zalo Bot
- **Cơ sở dữ liệu**: Truy vấn bảng `vnnew` cho tin tức và `chatnhom` cho tin nhắn nhóm

## Hướng dẫn AI

AI được hướng dẫn hoạt động như một trợ lý thông minh với các quy tắc:
- Phân tích yêu cầu và chọn công cụ phù hợp
- Tạo phản hồi ngắn gọn, dễ đọc
- Cung cấp nguồn thông tin
- Được phép trả lời các câu hỏi về nhiều chủ đề khác nhau
- Kết hợp xu hướng hiện tại và hài hước để tạo trải nghiệm thoải mái

## Lưu ý

- Workflow chỉ phản hồi tin nhắn có chứa "@n8n" và từ người dùng cụ thể
- Phản hồi được giới hạn ở 3000 ký tự mỗi tin nhắn
- Không hỗ trợ định dạng markdown hoặc URL trong phản hồi
- Workflow sử dụng nhiều API bên ngoài nên cần đảm bảo các kết nối luôn hoạt động

---

# MCP Server - N8N

## Giới thiệu

MCP Server là một workflow n8n được thiết kế để cung cấp các công cụ AI và tích hợp nhiều dịch vụ khác nhau thông qua giao diện MCP (Model Control Protocol). Workflow này hoạt động như một trung tâm điều khiển, cho phép tương tác với nhiều dịch vụ Google và các công cụ tra cứu thông qua giao diện AI.

## Tính năng chính

MCP Server cung cấp hai endpoint chính:

1. **Google** (`/google`): Tích hợp các dịch vụ Google:
   - Gmail: Gửi, nhận, tìm kiếm, trả lời và xóa email
   - Google Calendar: Tìm, tạo, cập nhật và xóa sự kiện
   - Google Docs/Drive: Tạo, tìm kiếm, đọc, cập nhật và xóa tài liệu
   - Google Gemini: Phân tích hình ảnh

2. **Tra Cứu** (`/tracuu`): Cung cấp các công cụ tra cứu thông tin:
   - Tra cứu phạt nguội
   - Tra cứu xổ số
   - Tra cứu đăng kiểm
   - Tra cứu lịch âm dương

## Cấu trúc workflow

Workflow được tổ chức thành các node chính:
- **Trigger Nodes**: `Google` và `Tra Cuu` - điểm khởi đầu cho các yêu cầu
- **Tool Nodes**: Các công cụ cụ thể cho từng dịch vụ (Gmail, Calendar, Drive, v.v.)
- **Integration Nodes**: `tracuu`, `weather`, `search-tool` - kết nối với các dịch vụ bên ngoài

## Cách sử dụng

Workflow này được thiết kế để hoạt động với giao diện AI thông qua MCP, cho phép người dùng tương tác bằng ngôn ngữ tự nhiên để sử dụng các công cụ khác nhau.

---

# Sub Workflow Tra Cứu - N8N

## Giới thiệu

Workflow "Tra Cứu" là một workflow đa năng được phát triển trên nền tảng n8n, cho phép tra cứu nhiều loại thông tin khác nhau thông qua một giao diện thống nhất. Workflow này có thể được sử dụng độc lập hoặc được tích hợp vào các hệ thống khác như Home Assistant, Zalo Bot, hoặc thông qua MCP Server.

## Tích hợp với MCP Server

Workflow Tra Cứu được tích hợp vào MCP Server thông qua node `tracuu`, cho phép:
- Sử dụng thông qua giao diện AI của MCP Server
- Truy cập thông qua endpoint `/tracuu`
- Tương tác bằng ngôn ngữ tự nhiên thay vì cần nhớ cú pháp chính xác
- Kết hợp với các công cụ khác trong MCP Server

### Tham số đầu vào khi sử dụng qua MCP Server

Khi sử dụng thông qua MCP Server, người dùng chỉ cần mô tả yêu cầu bằng ngôn ngữ tự nhiên, và MCP Server sẽ tự động điền các tham số cần thiết:
- `loaixe`: Loại phương tiện (1: Ô tô, 2: Xe máy, 3: Xe điện)
- `bienso`: Biển số xe
- `khuvuc`: Khu vực xổ số (xsmb, xsmt, xsmn)
- `date`: Ngày tra cứu xổ số
- `temdk`: Số tem đăng kiểm
- `dateal`: Ngày tra cứu lịch âm dương

## Tính năng chính

Workflow này hỗ trợ bốn loại tra cứu khác nhau:

1. **Tra cứu phạt nguội**:
   - Tra cứu thông tin vi phạm giao thông qua biển số xe
   - Hiển thị chi tiết các lỗi vi phạm, thời gian, địa điểm
   - Sử dụng API từ phatnguoi.vn

2. **Tra cứu đăng kiểm**:
   - Tra cứu thông tin đăng kiểm xe thông qua biển số và số tem đăng kiểm
   - Hiển thị thông tin về xe (nhãn hiệu, loại xe, số khung, số máy)
   - Hiển thị thông tin đăng kiểm (ngày kiểm định, hạn hiệu lực)
   - Hiển thị thông tin phí đường bộ
   - Sử dụng dữ liệu từ Cục Đăng kiểm Việt Nam (app.vr.org.vn)

3. **Tra cứu kết quả xổ số**:
   - Xổ số miền Bắc (XSMB)
   - Xổ số miền Trung (XSMT)
   - Xổ số miền Nam (XSMN)
   - Hiển thị kết quả các giải và bảng lô tô

4. **Tra cứu lịch âm dương**:
   - Thông tin ngày âm lịch
   - Giờ hoàng đạo
   - Tuổi xung khắc

## Cách thức hoạt động

### Cấu trúc workflow

Workflow được thiết kế theo mô hình luồng dữ liệu, với các thành phần chính:

- **Node Start**: Điểm bắt đầu, nhận các tham số đầu vào
- **Node Switch1**: Phân luồng dựa trên loại tra cứu
- **Các Node HTTP Request**: Gọi API từ các nguồn khác nhau
- **Các Node Code**: Xử lý và định dạng dữ liệu trả về

### Tham số đầu vào khi sử dụng độc lập

Khi sử dụng workflow này độc lập, cần cung cấp các tham số sau:

- `loaixe`: Loại phương tiện khi tra cứu thông tin xe (1: Ô tô, 2: Xe máy)
- `bienso`: Biển số xe khi tra cứu thông tin xe hoặc phạt nguội
- `khuvuc`: Khu vực xổ số (xsmb, xsmt, xsmn)
- `date`: Ngày tra cứu xổ số (định dạng: ngày-tháng-năm)
- `temdk`: Số tem đăng kiểm khi tra cứu thông tin đăng kiểm
- `dateal`: Ngày tra cứu lịch âm dương

### Quy trình xử lý

#### 1. Tra cứu phạt nguội

- Gọi API từ phatnguoi.vn với thông tin biển số và loại xe
- Phân tích dữ liệu HTML trả về để trích xuất thông tin vi phạm
- Định dạng kết quả thành JSON với các trường thông tin:
  - Biển kiểm soát
  - Loại phương tiện
  - Danh sách vi phạm
  - Thời gian cập nhật
  - Nguồn dữ liệu

#### 2. Tra cứu đăng kiểm

- Quy trình phức tạp hơn do cần xử lý CAPTCHA:
  1. Gọi trang web đăng kiểm để lấy session và CAPTCHA
  2. Sử dụng OpenAI để giải mã CAPTCHA
  3. Gửi form với thông tin biển số, số tem và mã CAPTCHA
  4. Phân tích kết quả trả về để trích xuất thông tin đăng kiểm
- Kết quả bao gồm:
  - Thông tin chung về xe (biển đăng ký, nhãn hiệu, loại phương tiện)
  - Thông số kỹ thuật (kích thước, khối lượng, số chỗ ngồi)
  - Thông tin kiểm định (ngày kiểm định, đơn vị kiểm định, hạn hiệu lực)
  - Thông tin phí đường bộ (ngày nộp, đơn vị thu, hạn nộp)
  - Tình trạng hiện tại (còn hạn/hết hạn, số ngày còn lại)

#### 3. Tra cứu xổ số

- Gọi API từ xosodaiphat.com với thông tin khu vực và ngày
- Phân tích HTML để trích xuất kết quả xổ số
- Định dạng kết quả thành JSON với cấu trúc phân chia theo giải thưởng và lô tô

#### 4. Tra cứu lịch âm dương

- Gọi API từ 24h.com.vn với thông tin ngày cần tra cứu
- Phân tích HTML để trích xuất thông tin ngày âm lịch, giờ hoàng đạo, tuổi xung khắc
- Định dạng kết quả thành JSON với đầy đủ thông tin

## Cách sử dụng độc lập

### Gọi workflow từ bên ngoài

Workflow này có thể được gọi thông qua webhook với các tham số tương ứng:

```
POST /webhook/tra-cuu
Content-Type: application/json

{
  "loaixe": 1,
  "bienso": "51A-12345",
  "khuvuc": "xsmb",
  "date": "25-07-2023",
  "temdk": "12345678",
  "dateal": "2023-07-25"
}
```

### Kết quả trả về

Kết quả trả về sẽ là JSON với cấu trúc phụ thuộc vào loại tra cứu:

- **Tra cứu phạt nguội**: Danh sách các vi phạm giao thông, thông tin biển số, loại xe
- **Tra cứu đăng kiểm**: Thông tin chi tiết về xe, đăng kiểm, phí đường bộ
- **Tra cứu xổ số**: Kết quả xổ số theo từng giải và bảng lô tô
- **Tra cứu lịch âm dương**: Thông tin ngày âm lịch, giờ hoàng đạo, tuổi xung khắc

## Xử lý lỗi

Workflow có cơ chế xử lý lỗi:
- Tự động thử lại khi gặp lỗi mạng
- Xử lý trường hợp CAPTCHA không chính xác
- Kiểm tra và báo lỗi khi không tìm thấy dữ liệu

## Tích hợp

Workflow này có thể được tích hợp với:
- MCP Server thông qua node `tracuu`
- Home Assistant thông qua RESTful Command
- Zalo Bot thông qua webhook
- Các hệ thống khác có khả năng gọi API

## Lưu ý

- Một số nguồn dữ liệu có thể thay đổi cấu trúc HTML, cần cập nhật mã phân tích khi cần thiết
- Xử lý CAPTCHA có thể không hoạt động nếu trang web thay đổi cơ chế bảo mật
- Nên kiểm tra định kỳ các nguồn dữ liệu để đảm bảo workflow hoạt động chính xác

---

# Sub Workflow Download - N8N

## Giới thiệu

Workflow "Download" là một công cụ tự động hóa được phát triển trên nền tảng n8n, được thiết kế để tải file từ Google Drive và gửi đến người dùng qua Zalo Bot. Workflow này hoạt động như một cầu nối giữa dịch vụ lưu trữ đám mây và ứng dụng nhắn tin, giúp chia sẻ tài liệu một cách nhanh chóng và hiệu quả.

## Tính năng chính

Workflow Download thực hiện các chức năng sau:

1. **Tải file từ Google Drive**: Lấy file từ Google Drive dựa vào ID file
2. **Xử lý tên file**: Thêm ngày tháng vào tên file để dễ quản lý
3. **Lưu trữ tạm thời**: Lưu file vào thư mục cấu hình của n8n
4. **Gửi file qua Zalo**: Gửi file đã tải về cho người dùng thông qua Zalo Bot

## Cấu trúc workflow

Workflow bao gồm 5 node chính, mỗi node thực hiện một nhiệm vụ cụ thể:

1. **Start**: Node khởi đầu, nhận các tham số đầu vào
2. **get file**: Kết nối với Google Drive và tải file theo ID
3. **Code**: Xử lý thông tin file và tạo tên file mới với định dạng ngày-tháng
4. **save file**: Lưu file đã tải về vào thư mục cấu hình của n8n
5. **sent file**: Gọi dịch vụ Home Assistant để gửi file qua Zalo Bot

## Tham số đầu vào

Workflow nhận hai tham số quan trọng:

- `idfile`: ID của file cần tải từ Google Drive
- `idzalo`: ID của cuộc trò chuyện Zalo để gửi file đến

## Quy trình hoạt động

### 1. Tải file từ Google Drive

- Node "get file" sử dụng Google Drive API để tải file theo ID được cung cấp
- Kết quả trả về bao gồm nội dung file dưới dạng binary data và metadata

### 2. Xử lý thông tin file

- Node "Code" thực hiện các nhiệm vụ:
  - Trích xuất thông tin file (tên, phần mở rộng, kiểu MIME)
  - Tạo định dạng ngày tháng hiện tại (DD-MM-YYYY)
  - Tạo tên file mới với định dạng: `[ngày-tháng-năm]-[tên file gốc]`
  - Xác định đường dẫn lưu trữ trong thư mục cấu hình n8n

### 3. Lưu trữ file tạm thời

- Node "save file" lưu file đã tải về vào đường dẫn `/config/n8n/[tên file mới]`
- Giữ nguyên định dạng và nội dung file

### 4. Gửi file qua Zalo Bot

- Node "sent file" gọi dịch vụ Home Assistant để gửi file đến người dùng Zalo
- Sử dụng tham số:
  - `thread_id`: ID cuộc trò chuyện Zalo (từ tham số đầu vào)
  - `account_selection`: Số điện thoại của tài khoản Zalo Bot (+84764343466)
  - `type`: Loại tin nhắn (1 - file)
  - `file_path_or_url`: Đường dẫn đến file đã lưu

## Cách sử dụng

### Gọi workflow từ bên ngoài

Workflow này có thể được gọi thông qua webhook hoặc từ workflow khác với các tham số tương ứng:

```
POST /webhook/download
Content-Type: application/json

{
  "idfile": "1AbCdEfGhIjKlMnOpQrStUvWxYz",
  "idzalo": "123456789"
}
```

### Tích hợp với các hệ thống khác

Workflow này được thiết kế để hoạt động với:
- Google Drive: Làm nguồn tài liệu
- Home Assistant: Làm trung gian để gửi file
- Zalo Bot: Làm kênh phân phối file đến người dùng cuối

## Lưu ý

- Cần cấu hình đúng quyền truy cập cho Google Drive API
- Tài khoản Zalo Bot cần được cấu hình trong Home Assistant
- Thư mục `/config/n8n/` cần có quyền ghi để lưu trữ file tạm thời
- File sẽ được lưu với tên mới bao gồm ngày tháng hiện tại

---

# Workflow Automatically - N8N

## Giới thiệu

Workflow "Automatically" là một hệ thống tự động hóa đa chức năng được xây dựng trên nền tảng n8n, tích hợp nhiều tính năng tự động chạy theo lịch trình. Workflow này kết hợp ba chức năng chính: phân loại email tự động bằng AI, kiểm tra phạt nguội định kỳ, và thu thập tin tức từ các nguồn RSS.

## Tính năng chính

Workflow Automatically bao gồm ba nhóm chức năng chính:

1. **Phân loại email tự động bằng AI**:
   - Lấy email từ Gmail
   - Phân tích nội dung email bằng AI (OpenAI)
   - Tự động gắn nhãn (label) phù hợp
   - Tạo nhãn mới nếu cần thiết

2. **Kiểm tra phạt nguội định kỳ**:
   - Kiểm tra phạt nguội cho xe ô tô và xe máy
   - Phân tích dữ liệu vi phạm
   - Gửi thông báo qua Zalo khi phát hiện vi phạm mới

3. **Thu thập và lưu trữ tin tức**:
   - Thu thập tin tức từ nhiều nguồn (VnExpress, Thanh Niên, Dân Trí)
   - Lọc và xử lý dữ liệu tin tức
   - Lưu trữ vào cơ sở dữ liệu PostgreSQL

## Cấu trúc workflow

Workflow được tổ chức thành ba phần chính, mỗi phần hoạt động độc lập:

### 1. Phân loại email tự động

- **Schedule Gmail**: Trigger định kỳ (2 phút/lần)
- **Gmail - Get All Messages**: Lấy email mới
- **Filter Emails Without Excluded Labels**: Lọc email chưa được phân loại
- **Loop Over Items**: Xử lý từng email
- **Gmail - Single Message**: Lấy nội dung chi tiết của email
- **Extract Email Data**: Trích xuất thông tin từ email
- **Categorize Email with AI**: Sử dụng AI để phân loại email
- **Store AI Category**: Lưu kết quả phân loại
- **Check if Label Exists**: Kiểm tra nhãn đã tồn tại chưa
- **Apply Label to Email/Create New Label**: Gắn nhãn hoặc tạo nhãn mới

### 2. Kiểm tra phạt nguội

- **Schedule Phat nguoi**: Trigger định kỳ
- **phat-nguoi-oto/phat-nguoi-xemay**: Gọi API kiểm tra phạt nguội
- **Code**: Phân tích dữ liệu phạt nguội
- **If co vi pham**: Kiểm tra có vi phạm không
- **sent-zalo-my-home**: Gửi thông báo qua Zalo nếu có vi phạm

### 3. Thu thập tin tức

- **vnexpress/thannien/dantri**: Trigger RSS định kỳ (10 phút/lần)
- **lọc**: Xử lý và lọc dữ liệu tin tức
- **lưu tin tức**: Lưu tin tức vào cơ sở dữ liệu PostgreSQL

## Cách thức hoạt động

### 1. Phân loại email tự động

Workflow sử dụng AI để phân loại email vào các nhóm như:
- Newsletter
- Inquiry
- Invoice
- Proposal
- Action Required
- Follow-up Reminder
- Task
- Personal
- Urgent
- Bank
- Job Update
- Spam / Junk
- Social / Networking
- Receipt
- Event Invite
- Subscription Renewal
- System Notification

Quy trình hoạt động:
1. Lấy email mới từ Gmail
2. Lọc các email chưa được phân loại
3. Trích xuất thông tin từ email (người gửi, chủ đề, nội dung)
4. Sử dụng OpenAI để phân tích và phân loại email
5. Kiểm tra xem nhãn đã tồn tại chưa
6. Nếu nhãn đã tồn tại, gắn nhãn cho email
7. Nếu nhãn chưa tồn tại, tạo nhãn mới và gắn cho email

### 2. Kiểm tra phạt nguội

Workflow tự động kiểm tra phạt nguội cho hai biển số xe:
- Ô tô: 34B87922
- Xe máy: 34B53015

Quy trình hoạt động:
1. Gọi API từ phatnguoi.vn với thông tin biển số
2. Phân tích dữ liệu HTML trả về
3. Trích xuất thông tin vi phạm (nếu có)
4. Nếu phát hiện vi phạm, gửi thông báo qua Zalo với các thông tin:
   - Số lượng vi phạm
   - Loại phương tiện
   - Thời gian vi phạm
   - Địa điểm vi phạm
   - Trạng thái
   - Đơn vị phát hiện
   - Nơi giải quyết vụ việc

### 3. Thu thập tin tức

Workflow tự động thu thập tin tức từ ba nguồn:
- VnExpress
- Thanh Niên
- Dân Trí

Quy trình hoạt động:
1. Đọc tin tức từ các nguồn RSS (10 phút/lần)
2. Lọc và trích xuất thông tin quan trọng (tiêu đề, link, nội dung tóm tắt, ngày)
3. Lưu trữ tin tức vào bảng `vnnew` trong cơ sở dữ liệu PostgreSQL

## Cấu hình và tích hợp

Workflow này tích hợp với nhiều dịch vụ:

1. **Gmail**: Sử dụng OAuth2 để truy cập và quản lý email
2. **OpenAI**: Sử dụng model GPT-4.1-nano để phân tích và phân loại email
3. **Home Assistant**: Kết nối với Zalo Bot để gửi thông báo
4. **PostgreSQL**: Lưu trữ tin tức thu thập được

## Lưu ý

- Workflow chạy theo lịch trình định kỳ, không cần can thiệp thủ công
- Phân loại email sử dụng AI nên có thể cần điều chỉnh để phù hợp với nhu cầu cụ thể
- Biển số xe được cố định trong code, cần cập nhật nếu muốn kiểm tra biển số khác
- Các nguồn tin tức có thể thay đổi cấu trúc RSS, cần kiểm tra định kỳ
- Cần đảm bảo các kết nối API (Gmail, OpenAI, Home Assistant) luôn hoạt động
