# Reflection - Top 5 Lakehouse Anti-Patterns

Trong số các Lakehouse Anti-Patterns, lỗi mà team mình cảm thấy dễ vướng phải nhất là **"Small File Problem" (Vấn đề quá nhiều file nhỏ)**.

**Lý do:**
1. **Thiếu tự động hóa bảo trì:** Khi thiết kế pipeline đẩy dữ liệu liên tục vào lớp Bronze (dạng streaming hoặc micro-batch), hệ thống rất dễ sinh ra hàng ngàn file dữ liệu nhỏ. Nếu quên thiết lập các job tự động chạy lệnh `OPTIMIZE` định kỳ, số lượng metadata khổng lồ sẽ làm chậm đáng kể hiệu suất đọc của engine (như đã thấy sự khác biệt tốc độ đọc ở Notebook 2).
2. **Quá tập trung vào logic:** Trong giai đoạn đầu xây dựng Data Lakehouse, các kĩ sư thường chỉ chú trọng việc lấy được dữ liệu vào hệ thống và biến đổi logic (ETL) cho đúng, mà hay bỏ qua khâu tối ưu hoá lưu trữ vật lý (physical optimization) ngay từ đầu.
3. **Quên đánh chỉ mục:** Việc không sử dụng `Z-ORDER` trên các cột thường xuyên được filter (như `user_id` hay `date`) sẽ khiến hệ thống phải quét toàn bộ các file không cần thiết, làm tốn tài nguyên và tăng chi phí.

Bài học rút ra là luôn phải tích hợp quy trình maintenance (Optimize, Z-order, Vacuum) ngay khi thiết kế data pipeline.
