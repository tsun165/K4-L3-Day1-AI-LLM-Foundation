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
> *Khi temperature tăng từ 0.0 đến 1.5, phản hồi sẽ ngày càng đa dạng, tự nhiên hơn nhưng cũng dễ mất tính nhất quán. Ở temperature thấp, câu trả lời ổn định và tập trung; ở temperature cao, nó có thể thay đổi nhiều hơn và sáng tạo hơn.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi chọn temperature khoảng 0.3–0.5 cho chatbot hỗ trợ khách hàng. Vì ở mức này, phản hồi vẫn rõ ràng, nhất quán và đáng tin cậy, nhưng vẫn còn chút độ tự nhiên.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *10.000 người dùng × 3 lần/ngày × 350 token = 10,5 triệu token/ngày. GPT-4o ước tính tốn 105 USD/ngày, GPT-4o-mini tốn 6,3 USD/ngày, nên GPT-4o đắt hơn khoảng 16,7 lần. Nên dùng GPT-4o cho các nhiệm vụ phức tạp, cần độ chính xác cao; dùng mini cho FAQ, triage, hoặc khối lượng lớn.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Hai phản hồi khác nhau rõ rệt về độ dài, từ vựng và ví dụ. System prompt làm model “đổi vai trò”: giáo viên tiểu học sẽ đơn giản, thân thiện, ví dụ gần gũi; chuyên gia tài chính sẽ chuyên sâu, dùng thuật ngữ kỹ thuật. Vì vậy, system prompt ảnh hưởng trực tiếp đến cách model diễn giải và mức độ chi tiết.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Nếu đoạn văn khoảng 100 từ, ước lượng “số từ / 0.75” khoảng 133 token. count_tokens thường cho ra cao hơn một chút, khoảng 10–30% tùy văn bản. Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì tokenizer chia token khác và tiếng Việt có nhiều từ ghép, cấu trúc biểu đạt khác nhau.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng khi người dùng cần thấy phản hồi ngay lập tức, như chatbot hoặc trợ lý trực tiếp. Nó tạo cảm giác nhanh và tương tác tốt hơn. Non-streaming phù hợp hơn khi cần chờ toàn bộ kết quả hoàn chỉnh trước khi hiển thị, ví dụ báo cáo hoặc văn bản có cấu trúc chặt chẽ.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff giúp giảm tải khi API quá tải vì các client retry theo khoảng cách tăng dần, tránh bùng phát cùng lúc. Nếu nhiều client retry với delay cố định giống nhau, họ sẽ gửi lại request đồng loạt, khiến hệ thống càng quá tải và dễ bị sụp đổ.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Tôi chọn persona “trợ lý học tập tiếng Việt, ngắn gọn, rõ ràng và thân thiện”. System prompt ví dụ: “Bạn là trợ lý học tập tiếng Việt, trả lời ngắn gọn, rõ ràng, dễ hiểu và luôn ưu tiên độ chính xác.” “Trả lời ngắn gọn” giúp dễ đọc, “bằng tiếng Việt” đảm bảo sự phù hợp với người dùng.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất là thiếu bộ nhớ dài hạn, nên trợ lý dễ quên ngữ cảnh cũ giữa các lượt chat. Một cải thiện cụ thể là lưu tóm tắt cuộc trò chuyện và đưa vào prompt mỗi lần gọi model, giúp trợ lý duy trì ngữ cảnh tốt hơn.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
