# Reflection — Lab 19

**Tên:** Nguyễn Nhật Quang\\
**Cohort:** A20-K3\\
**Path đã chạy:** lite\\

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

**Kết quả Precision@10 trên golden set 50 queries:**

- **`exact` (15 queries):** BM25 và hybrid ngang bằng nhau (96.7%), trong khi đó semantic kém hơn hai bản kia (88.7%). Với các query chứa từ kỹ thuật verbatim (VD: "Kubernetes auto-scaling") thì BM25 khớp trực tiếp term-frequency, không cần semantic understanding. Hybrid không cải thiện vì signal BM25 đã đủ mạnh.
- **`paraphrase` (15 queries):** Cả ba mode đều yếu với lần lượt là BM25 (33.3%), hybrid (32.0%), semantic (24.0%). Nguyên nhân dẫn đến kết quả thấp này là do model `bge-small-en-v1.5` được train cho tiếng Anh, không capture tốt semantic tiếng Việt paraphrase. Nếu đổi sang model `bge-m3 (multilingual) thì semantic sẽ thắng rõ ở slice này.
- **`mixed` (20 queries):** Trong trường hợp này, hybrid đúng hoàn toàn (100%), sau đó đến semantic (98.5%) và keyword (97%). Giải thích cho kết quả này thì là do tập query có các query vừa có exact term vừa có ý paraphrased nên RRF fusion kết hợp signal từ cả hai retriever, bù đắp điểm yếu của nhau.

**Khi nào KHÔNG dùng hybrid?**
- **Pure BM25**: khi query là exact keyword/mã lỗi/tên sản phẩm cụ thể/làm việc với con số — BM25 đã đủ tốt, thêm vector chỉ tốn latency.
- **Pure vector**: khi có embedding model multilingual mạnh (bge-m3) và query hoàn toàn là paraphrase/semantic, không chứa keyword đặc thù nào.

---

## Điều ngạc nhiên nhất khi làm lab này

_(Optional, 1–2 câu)_

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
