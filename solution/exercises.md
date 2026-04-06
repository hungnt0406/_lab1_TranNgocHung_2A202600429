# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, các phản hồi gần như giống hệt nhau qua nhiều lần gọi vì mô hình chọn token có xác suất cao nhất (deterministic). Khi tăng temperature lên 0.5 và 1.0, các phản hồi trở nên đa dạng hơn, sáng tạo hơn và có thể đề cập đến các chủ đề khác nhau. Tại temperature = 1.5, phản hồi rất ngẫu nhiên, đôi khi mất mạch lạc hoặc chứa thông tin không liên quan — mô hình "mạo hiểm" hơn trong việc chọn token ít xác suất.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Đối với chatbot hỗ trợ khách hàng, nên đặt temperature khoảng 0.0–0.3. Lý do là chatbot cần trả lời chính xác, nhất quán và đáng tin cậy — không nên "sáng tạo" khi cung cấp thông tin về sản phẩm, chính sách hay xử lý khiếu nại. Temperature thấp đảm bảo câu trả lời ổn định, giảm thiểu rủi ro cung cấp thông tin sai lệch.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> Tổng token/ngày = 10,000 × 3 × 350 = 10,500,000 token = 10,500 × 1K token.
> - Chi phí GPT-4o: 10,500 × $0.010 = **$105/ngày**
> - Chi phí GPT-4o-mini: 10,500 × $0.0006 = **$6.30/ngày**
> - Tỷ lệ: $105 / $6.30 ≈ **16.67 lần** — GPT-4o đắt hơn GPT-4o-mini khoảng 16.7 lần.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> **GPT-4o xứng đáng:** Khi cần phân tích tài liệu pháp lý hoặc y tế — các lĩnh vực đòi hỏi độ chính xác cao, khả năng suy luận phức tạp và hiểu ngữ cảnh sâu. Sai sót ở đây có thể gây hậu quả nghiêm trọng, nên chi phí cao hơn là chấp nhận được.
>
> **GPT-4o-mini phù hợp hơn:** Khi dùng để phân loại email, tạo tag tự động, hoặc trả lời FAQ đơn giản — các tác vụ không yêu cầu suy luận phức tạp. Với volume lớn (hàng triệu request/ngày), tiết kiệm ~94% chi phí là rất đáng kể mà chất lượng vẫn đủ tốt.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi người dùng trực tiếp chờ đợi phản hồi dài — ví dụ chatbot, trợ lý viết bài, hoặc giải thích code. Thay vì chờ 10–30 giây để nhận toàn bộ phản hồi (gây cảm giác "treo"), streaming cho phép hiển thị token ngay khi được sinh ra, tạo trải nghiệm tương tác mượt mà giống cuộc trò chuyện thật. Ngược lại, non-streaming phù hợp hơn cho các pipeline backend tự động — ví dụ batch processing, phân loại dữ liệu, hoặc khi output cần được xử lý nguyên khối trước khi dùng (như parse JSON response). Trong các trường hợp này, streaming thêm độ phức tạp code không cần thiết mà không mang lại lợi ích UX.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả tests pass: `pytest tests/ -v`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
