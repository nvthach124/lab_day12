# Day 12 Lab - Mission Answers

## Part 1: Localhost vs Production

### Exercise 1.1: Anti-patterns found in monolithic code
1. **Hardcoded Secrets:** API Keys và các thông số nhạy cảm để trực tiếp trong code thay vì dùng biến môi trường.
2. **Lack of Security:** Không có cơ chế xác thực (Authentication), bất kỳ ai cũng có thể gọi API.
3. **No Rate Limiting:** Không giới hạn số lượng request, dễ bị tấn công DDoS hoặc làm cạn kiệt tài nguyên/chi phí.
4. **Stateful Design:** Lưu trữ session trong bộ nhớ của instance đơn lẻ, không thể scale ngang (Horizontal Scaling).
5. **No Structured Logging:** Log dạng text đơn giản, khó cho việc giám sát và phân tích lỗi trên quy mô lớn.

### Exercise 1.3: Comparison table
| Feature | Develop | Production | Why Important? |
|---------|---------|------------|----------------|
| Config  | `.env` đơn giản | Environment Variables/Vault | Bảo mật thông tin nhạy cảm |
| Logging | Console/Text | JSON Structured Logging | Giúp hệ thống monitor (ELK/Datadog) dễ xử lý |
| Errors  | Hiện đầy đủ Traceback | Ẩn chi tiết, chỉ hiện mã lỗi | Ngăn chặn rò rỉ thông tin hạ tầng cho Hacker |
| Scaling | Chạy 1 instance | Load Balancer + N instances | Đảm bảo tính sẵn sàng cao (High Availability) |

## Part 2: Docker

### Exercise 2.1: Dockerfile questions
1. **Base image:** `python:3.11-slim` (Lý do: Nhẹ và đủ thư viện cần thiết, bảo mật hơn bản full).
2. **Working directory:** `/app`.
3. **User:** `agent` (Non-root user - để đảm bảo nếu container bị xâm nhập, Hacker không có quyền root hệ thống).

### Exercise 2.3: Image size comparison
- **Build Stage:** ~800 MB (do có các bộ công cụ gcc, build-essential).
- **Runtime Stage:** 247 MB.
- **Difference:** Giảm ~70% so với bản build ban đầu.

## Part 4: API Security & Cost Guard
- **Authentication:** Triển khai qua `X-API-Key` header cho mọi request.
- **Cost Guard:** Sử dụng module `cost_guard.py` để theo dõi lượng token và chi phí USD hàng ngày, tự động ngắt nếu vượt ngưỡng $10/tháng.

## Part 5: Scaling & Reliability
- **Statelessness:** Sử dụng Redis để quản lý session và rate limiting, cho phép 3 Agent instance (scaling) hoạt động đồng bộ mà không mất dữ liệu lịch sử chat.
- **Load Balancer:** Sử dụng Nginx (cổng 8080) để phân phối tải đều cho các instance.
