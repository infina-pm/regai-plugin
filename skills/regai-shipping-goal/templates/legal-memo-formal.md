# Template: Formal legal opinion / memo (Ý KIẾN PHÁP LÝ)

> For council-facing or regulator-facing output. Ground every statutory reference
> via regai (`check_in_force` + `get_article`) before relying on it. Header/footer
> and disclaimer come from `.regai/context.md`.

```
CÔNG TY [TÊN] — BỘ PHẬN PHÁP CHẾ & TUÂN THỦ

Ý KIẾN PHÁP LÝ
V/v: [tiêu đề vấn đề]
Ngày: [DD/MM/YYYY]

I.   CƠ SỞ PHÁP LÝ
     1. [Luật/Nghị định/Thông tư số... — tình trạng]
II.  VẤN ĐỀ CẦN TƯ VẤN
     [tình huống thực tế]
III. PHÂN TÍCH PHÁP LÝ
     1. [vấn đề 1] — Theo Điều X, khoản Y [văn bản...]: [áp dụng vào tình huống]
IV.  TÌNH HUỐNG TIỀN LỆ / THAM CHIẾU  (nếu có)
     [precedent — file mẫu hoặc thực tiễn thị trường]
V.   KẾT LUẬN & KHUYẾN NGHỊ
     1. Kết luận: 🟢 khả thi / 🟡 khả thi có điều kiện / 🔴 rủi ro cao / ⛔ không khuyến nghị
     2. Khuyến nghị + điều kiện thực hiện
     3. Rủi ro còn tồn tại
```

## Risk matrix (Bước IV of a proposal report)

| Vấn đề | Mức rủi ro | Xác suất | Tác động | Hành động cần thiết |
|--------|-----------|----------|----------|---------------------|
| ...    | 🔴/🟡/🟢  | Cao/TB/Thấp | Cao/TB/Thấp | ... |

## Compliance checklist (for Product)

- **Pre-dev** — [legal conditions to meet before building]; Legal sign-off obtained.
- **Dev** — [mandatory technical requirements: data not stored, encryption, UI disclosures].
- **Pre-launch** — [final Legal review]; consent flow correct per [số hiệu + Điều].
- **Ongoing** — [periodic reporting / monitoring obligations].

## SOP block

```
SOP — [tên quy trình]   |   Căn cứ: [Điều X, văn bản Y — tình trạng]
Mục đích | Phạm vi | Các bước (role + thời hạn) | Escalation (khi nào báo Legal / BGĐ) | Lưu hồ sơ
```
