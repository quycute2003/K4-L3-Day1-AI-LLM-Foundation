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
> Khi temperature tăng từ 0.0 lên 1.5, phản hồi có xu hướng đa dạng và sáng tạo hơn về cách diễn đạt. Temperature 0.0 cho câu trả lời ổn định, trực tiếp; mức 1.0–1.5 dễ tạo thông tin thú vị hơn nhưng cũng có thể lan man hoặc kém nhất quán.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature khoảng 0.2 cho chatbot hỗ trợ khách hàng. Mức thấp giúp câu trả lời nhất quán, chính xác và bám sát chính sách, nhưng vẫn tự nhiên hơn so với temperature 0.0.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với 10,5 triệu output token mỗi ngày, GPT-4o tốn khoảng 105 USD/ngày, còn GPT-4o-mini khoảng 6,30 USD/ngày; vì vậy GPT-4o đắt hơn khoảng 16,7 lần nếu chỉ xét output token. GPT-4o xứng đáng cho yêu cầu phức tạp cần suy luận và xử lý ngữ cảnh tinh tế, còn mini phù hợp với FAQ, phân loại yêu cầu hoặc các tác vụ đơn giản có lưu lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
>Persona giáo viên tiểu học sử dụng câu ngắn, từ ngữ đơn giản và thường ví blockchain như một cuốn sổ được nhiều người cùng giữ. Persona chuyên gia tài chính trả lời dài và chi tiết hơn, sử dụng các thuật ngữ như sổ cái phân tán, cơ chế đồng thuận, mật mã học và tính bất biến. System prompt không thay đổi câu hỏi, nhưng định hướng rõ độ sâu, từ vựng, ví dụ và đối tượng người đọc của phản hồi.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn văn tiếng Việt 116 từ, tiktoken đếm được 143 token, trong khi công thức số từ/0,75 ước lượng khoảng 154,67 token; hai kết quả chênh nhau khoảng 7,54%. Trong trường hợp này, công thức thô đã ước lượng cao hơn kết quả thực tế. Tiếng Việt thường tốn nhiều token hơn tiếng Anh có nội dung tương đương vì một từ có dấu hoặc từ ít phổ biến có thể bị tokenizer tách thành nhiều đơn vị nhỏ, dù mức chênh lệch còn phụ thuộc vào bộ mã hóa và nội dung cụ thể.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi model tạo phản hồi dài hoặc có độ trễ cao, chẳng hạn chatbot, trợ lý viết nội dung và giải thích tài liệu, vì người dùng có thể đọc phần đầu ngay thay vì chờ toàn bộ kết quả. Non-streaming phù hợp hơn với phản hồi ngắn, xử lý nền, hoặc khi ứng dụng cần nhận và kiểm tra toàn bộ kết quả—ví dụ JSON có cấu trúc—trước khi hiển thị hay chuyển sang bước tiếp theo.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tăng dần thời gian chờ sau mỗi lần thất bại, giúp giảm tần suất request và cho API thêm thời gian phục hồi khi quá tải. Nếu hàng nghìn client cùng retry sau một khoảng cố định như 1 giây, chúng có thể đồng loạt gửi request trở lại và tạo ra một đợt quá tải mới.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona: “Bạn là trợ giảng thân thiện của khóa AI, giải thích rõ ràng, trả lời ngắn gọn bằng tiếng Việt và đưa ví dụ đơn giản khi cần.” Cụm “trợ giảng thân thiện” giúp model giữ giọng điệu hỗ trợ, dễ tiếp cận; yêu cầu “ngắn gọn bằng tiếng Việt” giúp phản hồi phù hợp với người học Việt Nam và tránh nội dung dài dòng, tốn token. Việc yêu cầu ví dụ đơn giản giúp các khái niệm AI trừu tượng dễ hiểu hơn.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
>Hạn chế lớn nhất là trợ lý chỉ giữ ba lượt hội thoại gần nhất, nên có thể quên thông tin quan trọng được nhắc đến từ đầu phiên. Tôi sẽ cải thiện bằng cách tóm tắt các message cũ trước khi cắt history, sau đó lưu bản tóm tắt này như một message ngữ cảnh và gửi kèm trong các request tiếp theo. Cách này duy trì được thông tin dài hạn quan trọng mà không làm số lượng input token tăng liên tục.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
