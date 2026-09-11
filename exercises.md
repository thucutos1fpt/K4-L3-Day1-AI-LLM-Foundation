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
> Khi temperature tăng từ 0.0 lên 1.5, câu trả lời thường trở nên đa dạng và sáng tạo hơn. Ở temperature thấp, model có xu hướng trả lời ổn định và ít thay đổi, còn ở temperature cao, cách diễn đạt và nội dung có thể thay đổi nhiều hơn.
### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời ổn định, chính xác và ít lan man, đồng thời vẫn đủ tự nhiên khi giao tiếp với khách hàng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với workload này, GPT-4o đắt hơn GPT-4o-mini khoảng 6–7 lần về chi phí token đầu ra (tùy bảng giá được dùng trong bài). GPT-4o xứng đáng khi cần câu trả lời phức tạp, chất lượng cao hoặc cần khả năng suy luận tốt. Với các câu hỏi đơn giản như FAQ, phân loại hoặc hỗ trợ thông thường, nên dùng GPT-4o-mini để tiết kiệm chi phí.
---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với system prompt dành cho giáo viên tiểu học, model thường trả lời ngắn gọn, dùng từ đơn giản và đưa ra những ví dụ gần gũi với trẻ em. Với system prompt dành cho chuyên gia tài chính, câu trả lời thường chuyên sâu hơn, dài hơn và sử dụng nhiều thuật ngữ kỹ thuật. Hai system prompt tạo ra cách trình bày và mức độ giải thích khác nhau dù câu hỏi của người dùng giống nhau. Điều này cho thấy system prompt có ảnh hưởng lớn đến vai trò, phong cách và hành vi của model.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài?**
> Với một đoạn văn tiếng Việt khoảng 100 từ, số token thực tế bằng tiktoken thường cao hơn cách ước lượng số từ / 0.75. Mức chênh lệch phụ thuộc vào chính đoạn văn được chọn, nhưng có thể khá lớn. Nguyên nhân là token không tương đương với một từ; tiếng Việt có nhiều dấu, ký tự và cách tách từ khiến tokenizer có thể chia một từ thành nhiều token. Vì vậy, cách đếm bằng tiktoken chính xác hơn cho việc ước tính token thực tế.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng khi model tạo ra câu trả lời dài vì người dùng có thể nhìn thấy kết quả ngay khi model đang tạo nội dung, giúp giảm cảm giác phải chờ đợi. Streaming phù hợp với chatbot, trợ lý AI và các ứng dụng tương tác trực tiếp. Ngược lại, non-streaming phù hợp hơn khi câu trả lời ngắn hoặc khi ứng dụng cần nhận toàn bộ kết quả rồi mới xử lý tiếp.
### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry với delay cố định giống nhau?**
> Exponential backoff giúp các client không gửi request lại cùng một lúc khi API đang quá tải. Thời gian chờ tăng dần sau mỗi lần retry, ví dụ 0.1 giây, 0.2 giây, 0.4 giây, giúp hệ thống có thời gian phục hồi. Nếu hàng nghìn client cùng retry với delay cố định 1 giây, chúng có thể gửi request lại đồng thời, tạo thêm một đợt quá tải mới và làm tình trạng lỗi nghiêm trọng hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu "trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Bạn là một trợ lý AI học tập thân thiện. Hãy giải thích các khái niệm AI và lập trình bằng tiếng Việt, sử dụng ngôn ngữ đơn giản, dễ hiểu và đưa ra ví dụ khi cần. Trả lời ngắn gọn, tập trung vào câu hỏi của người dùng và không sử dụng thuật ngữ phức tạp nếu không cần thiết.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt, không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý hiện tại là history chỉ lưu tối đa 3 lượt hội thoại, nên trợ lý có thể quên những thông tin được nói từ lâu. Một cải thiện cụ thể là sử dụng bộ nhớ dài hạn hoặc lưu lịch sử hội thoại vào cơ sở dữ liệu. Khi người dùng bắt đầu cuộc trò chuyện mới, hệ thống có thể lấy lại những thông tin liên quan từ bộ nhớ và đưa chúng vào context trước khi gọi API, giúp trợ lý duy trì ngữ cảnh tốt hơn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
