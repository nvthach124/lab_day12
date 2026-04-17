# Deployment Information - Day 12 AI Agent

## Public URL
🚀 **[https://upbeat-strength-production-7db5.up.railway.app](https://upbeat-strength-production-7db5.up.railway.app)**

## Platform
- **Hosting:** Railway (PaaS)
- **Database:** Railway Redis Managed Service
- **Runtime:** Docker (Multi-stage Build)

## Test Commands

### 1. Health Check
Kiểm tra tình trạng sống của hệ thống qua Nginx/Railway Proxy.
```bash
curl https://upbeat-strength-production-7db5.up.railway.app/health
# Expected Output: {"status": "ok", "uptime_seconds": ...}
```

### 2. API Test (Có mã bảo mật)
Sử dụng mã API Key đã được mã hóa an toàn.
```bash
curl -X POST https://upbeat-strength-production-7db5.up.railway.app/ask \
  -H "X-API-Key: agent_b32527a23dfa7adef494a5a9b779872b" \
  -H "Content-Type: application/json" \
  -d '{"question": "Hello Agent! Bạn đã sẵn sàng nộp bài chưa?"}'
```

### 3. Rate Limiting Test
Dùng vòng lặp gọi 30 lần liên tiếp để kích hoạt lỗi 429 (Too Many Requests).
```bash
for i in {1..30}; do 
  curl -s -o /dev/null -w "%{http_code}\n" -X POST https://upbeat-strength-production-7db5.up.railway.app/ask \
    -H "X-API-Key: agent_b32527a23dfa7adef494a5a9b779872b" \
    -H "Content-Type: application/json" \
    -d '{"question": "spam"}'; 
done
```

## Infrastructure Settings
- **Horizontal Scaling:** Cấu hình sẵn sàng cho 3 replica.
- **Stateless Session:** Lưu trữ hội thoại qua Redis.
- **Monitoring:** Nhật ký JSON tích hợp sẵn.
