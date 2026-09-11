# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> *Câu trả lời của bạn*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Với chatbot hỗ trợ khách hàng, mình sẽ đặt temperature càng thấp càng tốt, thường là 0.0-0.2. Sở dĩ là nếu để temperature ở mức 1 như nhiều mô hình ngôn ngữ khác, hiện tượng hallucination (ảo giác) sẽ đưa ra những thông tin sai lệch, gây ảnh hưởng đến vận hành và uy tín của doanh nghiệp. Khi temperature là rất thấp, dù các khách hàng khác nhau hỏi cùng một câu hỏi theo các cách diễn đạt khác nhau, chatbot vẫn sẽ đưa ra một phương án giải quyết chuẩn chỉnh duy nhất.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Dựa trên số liệu cung cấp từ solution.py, giá cho mỗi 1K token của GPT-4o là 0.01 USD, còn GPT-40-mini là 0.0006. Xét số token output là bằng nhau ở cả hai bên - như vậy, GPT-4o đắt hơn 0.01/0.0006 = xấp xỉ 16.67 lần GPT-4o-mini. Một trường hợp GPT-4o xứng đáng với ci phí là trong trợ lý phân tích pháp lý hoặc chẩn đoán y tế phức tạp. Đây là những công việc đòi hỏi suy luận logic nhiều bước, tuân thủ định dạng ngặt nghèo, hiểu ngữ cảnh chuyên sâu và độ chính xác cao tuyệt đối, nếu hiện tượng hallucination xảy ra, thiệt hại sẽ lớn hơn nhiều so với chênh lệch giá API. GPT-4o-mini lại phù hợp trong trường hợp yêu cầu không quá chi tiết như dịch thuật, phân loại phản hồi khách hàng hoặc tóm tắt đoạn văn ngắn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi 1 dùng những từ ngữ đơn giản, ví dụ trực quan để trẻ em có thể hiểu, còn phản hồi 2 dài hơn, sử dụng nhiều thuật ngữ chuyên ngành, cùng nhiều ví dụ chuyên sâu. System prompt đóng vai trò như một bộ lọc định hình: đặt ngữ cảnh và áp đặt các ràng buộc về văn phong, độ dài, góc nhìn cho mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Câu trả lời của bạn*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các hệ thống xử lý thời gian thực (chatbot, trợ lý giọng nói...) vì việc giảm latency là yêu cầu cốt lõi để nâng cao trải nghiệm người dùng, như thể đang giao tiếp giữa người với người. Non-streaming lại phù hợp với các tác vụ ngầm như xuất dữ liệu có cấu trúc (.json), gọi API hệ thống,... đảm bảo tính chính xác trước khi gửi về phía người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff có tác dụng tăng thời gian chờ theo cấp số nhân (ví dụ 0.1, 0.2, 0.4... s) giúp giảm áp lực dồn dập lên API, tạo khoảng nghỉ đủ dài cho hệ thống xử lý queue tắc nghẽn. Khi hàng nghìn client cùng retry và delay cố định giống nhau, thay vì giảm tải, API sẽ liên tục bị đập mạnh bởi từng đợt sóng request đồng loạt. Server không thể xử lý xong các tác vụ dở dang, dẫn đến trạng thái quá tải kéo dài mãi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Câu trả lời của bạn*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Câu trả lời của bạn*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
