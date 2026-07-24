# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature tăng từ 0.0 lên 1.5, câu trả lời trở nên đa dạng và sáng tạo hơn nhưng cũng kém ổn định hơn. Temperature thấp tạo ra phản hồi nhất quán, trong khi temperature cao dễ xuất hiện cách diễn đạt mới, đôi khi lan man hoặc ít chính xác hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature = 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời nhất quán, chính xác và hạn chế việc mô hình tự suy diễn, trong khi vẫn giữ được cách diễn đạt tự nhiên

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Khối lượng sử dụng mỗi ngày:10.000 người dùng3 request/người350 output token/request→ Tổng output ≈ 10,5 triệu token/ngày.Theo bảng giá OpenAI, GPT-4o thường đắt hơn GPT-4o-mini khoảng 5–10 lần (tùy input/output token).Nên dùng GPT-4o: phân tích tài liệu phức tạp, lập kế hoạch, suy luận nhiều bước hoặc các tác vụ yêu cầu chất lượng cao.Nên dùng GPT-4o-mini: chatbot CSKH, FAQ, tóm tắt văn bản, phân loại dữ liệu hoặc các tác vụ đơn giản với lưu lượng lớn để tiết kiệm chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Hai system prompt tạo ra hai phong cách trả lời hoàn toàn khác nhau. Persona "giáo viên tiểu học" sử dụng câu ngắn, từ ngữ đơn giản và nhiều ví dụ gần gũi, còn persona "chuyên gia tài chính" dùng thuật ngữ chuyên môn, giải thích chi tiết và có chiều sâu hơn. Điều này cho thấy system prompt định hướng cách diễn đạt, mức độ chi tiết, đối tượng người đọc và phong cách của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Đoạn văn khoảng 100 từ được tiktoken đếm khoảng 145 token, trong khi cách ước lượng 100 / 0.75 ≈ 133 token. Sai lệch khoảng 9%.Tiếng Việt thường tốn nhiều token hơn tiếng Anh vì nhiều từ có dấu, nhiều âm tiết và cách tokenizer phải chia nhỏ thành nhiều token hơn so với các từ tiếng Anh phổ biến đã được học đầy đủ trong vocabulary.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt hữu ích khi mô hình tạo câu trả lời dài hoặc cần vài giây để hoàn thành, vì người dùng có thể đọc ngay từ những token đầu tiên thay vì phải chờ toàn bộ kết quả. Điều này giúp giảm cảm giác chờ đợi và cải thiện trải nghiệm. Ngược lại, non-streaming phù hợp với các phản hồi ngắn, tác vụ backend hoặc khi cần nhận toàn bộ kết quả trước khi xử lý tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm số lượng request gửi lại khi hệ thống đang quá tải, tạo thời gian để server phục hồi. Nếu tất cả client đều retry sau đúng 1 giây thì chúng sẽ đồng loạt gửi request cùng lúc, gây ra hiện tượng retry storm và khiến hệ thống tiếp tục quá tải. Backoff theo cấp số nhân (thường kết hợp jitter) giúp phân tán các lần retry và tăng khả năng thành công.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Bạn là trợ lý AI chuyên về Data Science và AI. Luôn trả lời bằng tiếng Việt rõ ràng, chính xác, ưu tiên giải thích theo từng bước và đưa ví dụ thực tế khi phù hợp. Nếu không chắc chắn, hãy nói rõ giới hạn thay vì suy đoán. Trả lời ngắn gọn nhưng đầy đủ ý.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là trợ lý chưa có bộ nhớ dài hạn, nên chỉ nhớ được một số lượt hội thoại gần đây và không thể ghi nhớ sở thích hoặc thông tin của người dùng giữa các phiên làm việc.Một cải thiện là bổ sung Long-term Memory bằng cách lưu các thông tin quan trọng vào cơ sở dữ liệu vector hoặc database (ví dụ FAISS hoặc PostgreSQL). Mỗi khi người dùng đặt câu hỏi, hệ thống sẽ truy xuất các thông tin liên quan rồi đưa vào prompt theo cơ chế RAG trước khi gọi mô hình, giúp trợ lý trả lời nhất quán và mang tính cá nhân hóa hơn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
