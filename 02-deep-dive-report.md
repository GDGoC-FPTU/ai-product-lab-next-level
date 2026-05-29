# 📄 02 — Deep-Dive Report (Bài nhóm) — Vin Smart Future

> **Báo cáo phân tích sâu (Phase 3 — DEEP-DIVE & Phase 5 — EVALUATE)**
> Vai trò: **AI Product Engineer @ Vin Smart Future (Vingroup)**

---

## 👥 Khai báo thành viên nhóm

| Trường | Nội dung |
|--------|----------|
| **Tên nhóm** | _<điền tên nhóm>_ |

| # | Họ và tên | MSSV | Vai trò trong dự án |
|---|-----------|------|---------------------|
| 1 | Nguyễn Quang Hoà | 2A202600986 | Tổng hợp báo cáo |
| 2 | Tiền Anh Kiệt | 2A202600961 | Xác định vấn đề và đánh giá |
| 3 | Nguyễn Văn Phúc | 2A202600539 | Vẽ sơ đồ, xác định vấn đề |

---

# 🗳️ Quyết định lựa chọn bài toán

Nhóm thống nhất chọn bài toán **Card #1 — VinFast: Tự động hóa thẩm định claim bảo hành & truy nguyên nhân gốc lỗi về dây chuyền sản xuất** để thực hiện Deep-Dive.

### Lý do lựa chọn và loại bỏ các thẻ khác

* **✅ Card #1 (VinFast — Thẩm định claim bảo hành):** Bài toán có **tác động tài chính rõ ràng** (chi phí bảo hành ~2–3% doanh thu), có **dữ liệu lịch sử claims sẵn trong ERP** để xây ground-truth, và bottleneck (đọc mô tả tự do + truy nguyên cụm lỗi) là use case **LLM/Agent đã được kiểm chứng**. Đây là bài toán "Problem-first" với metric đo lường được.
* **❌ Card #2 (Xanh SM — Geo-demand hotspot):** Bản chất là bài toán **dự báo ML chuỗi thời gian + tối ưu không gian**, phụ thuộc nặng vào hạ tầng dữ liệu GPS real-time và mô hình dự báo nhu cầu — vượt scope một prototype LLM trong buổi lab. Rủi ro: gợi ý sai làm tài xế chạy rỗng nhiều hơn.
* **❌ Card #3 (Vingroup — Sàng lọc ý tưởng AI):** Tác vụ **back-office, tần suất thấp**, không phải bottleneck vận hành real-time. Giá trị tạo ra/ngày thấp hơn nhiều so với bài toán claim bảo hành chạy hàng nghìn lượt/ngày.

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow Mapping

> 📌 Sơ đồ vẽ tay chi tiết (có Handoff 🔄 và Bottleneck 🔴) được nộp tại file **`04-workflow-diagram.png`**. Phần dưới là bản mô tả text đồng bộ với sơ đồ đó.

```text
Actors:   [Đại lý / SC]        [NV Thẩm Định]           [NV Thẩm Định]           [NV Thẩm Định]
          ─────────────        ──────────────           ──────────────           ──────────────
          ┌────────────┐       ┌──────────────┐         ┌──────────────┐         ┌──────────────┐
          │  Bước 1    │       │   Bước 2     │         │   Bước 3     │         │   Bước 4     │
          │ Đại lý     │  🔄   │ NV đọc claim │  🔴     │ NV chuẩn    │         │ Gom batch    │
          │ nhập claim │ ────→ │ & kiểm tra   │ ────→   │ hóa mã lỗi  │ ────→   │ claim đã duyệt│
          │ vào portal │       │ điều kiện BH │         │ + duyệt/từ  │         │ → phân cụm   │
          │            │       │              │         │ chối / YC   │         │ linh kiện    │
          │ Tool: App  │       │ Tool: ERP,   │         │ bổ sung      │         │ Tool: Excel  │
          │ Đại lý     │       │ Catalog BH   │         │              │         │              │
          │ ⏱ ~5 min   │       │ ⏱ ~15 min 🔴│         │ ⏱ ~5 min    │         │ ⏱ ~30 min   │
          │ /claim     │       │ /claim       │         │ /claim       │         │ /tuần (batch)│
          └────────────┘       └──────────────┘         └──────────────┘         └──────────────┘
                                                                                        │
                                                                                        │ 🔄 Handoff
                                                                                        │ (delay 1–2 ngày)
                                                                                        ▼
Actors:                                            [Kỹ Sư Chất Lượng — Nhà Máy]
                                                   ─────────────────────────────
                                                   ┌──────────────┐         ┌──────────────┐
                                                   │   Bước 5     │  🔴     │   Bước 6     │
                                                   │ KS truy lot, │ ────→   │ KS phát hành │
                                                   │ date code,   │         │ ECN / SCAR   │
                                                   │ nhà cung cấp │         │ (corrective) │
                                                   │ Tool: MES,   │         │ Tool: PLM,   │
                                                   │ SAP, Excel   │         │ Email        │
                                                   │ ⏱ ~2–4 tuần │         │ ⏱ ~1 tuần   │
                                                   │ /cụm lỗi 🔴  │         │ /cụm lỗi    │
                                                   └──────────────┘         └──────────────┘

🔴 = Bottleneck chính     🔄 = Handoff giữa bộ phận / hệ thống

⏱ Tổng thẩm định 1 claim:      ~25 phút/lượt
⏱ Tổng phát hiện lỗi hệ thống:  ~3–6 tuần/cụm
```

### Chi tiết từng bước

| Bước | Actor | Mô tả | Tool | Thời gian | Vấn đề |
|------|-------|-------|------|-----------|--------|
| 1 | Đại lý / Service Center | Nhập claim: VIN, mã OBD, mô tả tự do, ảnh chẩn đoán | Portal đại lý | ~5 min/claim | Mô tả tự do không nhất quán, thiếu chuẩn hóa |
| 2 🔴 | NV Thẩm Định | Đọc mô tả tự do, đối chiếu điều kiện BH (km/ngày), phán đoán phạm vi bảo hành | ERP, Catalog BH PDF | ~15 min/claim | Xử lý ngôn ngữ tự nhiên thủ công, dễ bỏ sót, không nhất quán giữa NV |
| 3 | NV Thẩm Định | Chuẩn hóa mã lỗi về catalog chuẩn, duyệt / từ chối / yêu cầu bổ sung | ERP | ~5 min/claim | Mã hóa sai → truy nguyên sai cụm linh kiện |
| 4 🔄 | NV Thẩm Định | Gom claim theo tuần, phân loại thủ công theo cụm: pin, động cơ, điện tử, thân vỏ | Excel | ~30 min/tuần | Gom theo lô → delay 1–2 ngày, pattern lỗi bị che khuất |
| 5 🔴 | Kỹ Sư Chất Lượng | Nhận báo cáo cụm, truy nguyên lot sản xuất, date code, nhà cung cấp linh kiện | MES, SAP, Excel | ~2–4 tuần/cụm | Query thủ công nhiều hệ thống rời rạc, mất nhiều tuần xác nhận pattern |
| 6 | Kỹ Sư Chất Lượng | Phát hành Engineering Change Notice (ECN) / Supplier Corrective Action Request | PLM, Email | ~1 tuần/cụm | Rủi ro recall nếu phát hiện quá trễ |

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|-------|-------------------|
| **1. Actor / Operator** | (a) **NV Thẩm Định Bảo Hành** tại Trung tâm Dịch vụ VinFast — người thẩm định từng claim. (b) **Kỹ sư Chất Lượng** tại nhà máy — người truy nguyên nhân gốc về dây chuyền. |
| **2. Current Workflow** | Đại lý nhập claim (VIN + mô tả tự do + ảnh) → NV đọc thủ công, đối chiếu điều kiện BH, chuẩn hóa mã lỗi, duyệt/từ chối → Gom batch cuối tuần, phân cụm linh kiện bằng Excel → KS truy nguyên lot/dây chuyền/NCC trên MES + SAP. Toàn bộ thủ công, rời rạc, không tự động hóa. |
| **3. Bottleneck** | **Bước 2** — Đọc & thẩm định mô tả tự do (~15 min/claim): xử lý ngôn ngữ tự nhiên, đánh giá điều kiện BH, gắn mã lỗi chuẩn. **Bước 5** — Truy nguyên cụm lỗi về lot/NCC (~2–4 tuần): query thủ công nhiều hệ thống MES/SAP rời rạc, không có anomaly detection. |
| **4. Business Impact** | Chi phí bảo hành ~**2–3% doanh thu/năm**. Mỗi NV xử lý ~40–60 claims/ngày × 15 min = **10–15 giờ công/NV/ngày** bị đốt vào đọc mô tả. Phát hiện cụm lỗi hệ thống sau 2–4 tuần → **rủi ro recall leo thang**, chi phí khắc phục cao hơn gấp nhiều lần khi lỗi đã lan rộng trên thị trường. |
| **5. Success Metric** | 1. Giảm thời gian thẩm định 1 claim từ **15 phút → dưới 3 phút** (≈80% reduction). 2. Phát hiện cụm lỗi bất thường từ **2–4 tuần → trong 2–3 ngày** kể từ khi đủ 10+ claims cùng pattern. 3. Tỉ lệ gắn mã lỗi chuẩn chính xác **≥ 95%** so với baseline NV hiện tại. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** trích xuất thực thể từ mô tả tự do (triệu chứng, bộ phận, điều kiện), gợi ý mã lỗi chuẩn **kèm confidence score**, gắn cờ claim đáng ngờ (fraud risk), phát hiện anomaly clustering theo lot/ngày sản xuất. **TUYỆT ĐỐI CẤM:** ❌ tự động phê duyệt/từ chối claim (quyết định tài chính phải do NV — **HITL**); ❌ thực thi hành động trên hệ thống MES/PLM sản xuất (chỉ được **đọc & báo cáo**); ❌ giao tiếp trực tiếp với khách hàng/đại lý. |

---

## 3.3. Future-State Flow & AI Fit

### AI-Fit Matrix

| Tiêu chí | Đánh giá |
|----------|----------|
| **Mức AI Fit** | **LLM Feature** cho Bước 2 (trích xuất thực thể + gợi ý mã lỗi). **Agent (Agentic Loop)** cho Bước 5 (tool use: query MES/SAP + clustering + anomaly detection). |
| **Lý do không dùng Rule-based** | Mô tả claim là ngôn ngữ tự nhiên không cấu trúc cố định — rule/regex không xử lý được biến thể ngôn ngữ (tiếng Việt + thuật ngữ kỹ thuật + viết tắt đại lý). |
| **Lý do không dùng Agent toàn phần** | Bước duyệt claim có hậu quả tài chính & pháp lý → **bắt buộc HITL**. Agent chỉ đề xuất, không được tự quyết định. |

### So sánh Rule vs LLM vs Agent

| Tiêu chí | Rule / State-Machine | LLM Feature | Agent (Agentic Loop) |
|----------|---------------------|-------------|----------------------|
| **Thẩm định claim (B2)** | ❌ Không xử lý ngôn ngữ tự do | ✅ Trích xuất + gợi ý mã lỗi | ⚠️ Overkill |
| **Phát hiện cụm lỗi (B5)** | ❌ Cứng nhắc, không linh hoạt | ❌ Thiếu reasoning đa hệ thống | ✅ Tool use: query MES + clustering |
| **Rủi ro** | Thấp | Trung bình (hallucination mã lỗi) → HITL | Cao hơn (tool use sai) → HITL + audit log |

### Sơ đồ Future-State (text-diagram)

```text
Actors:   [Đại lý/SC]     [🔵 AI Layer]          [🟢 NV Thẩm Định — HITL]   [🔵 AI Agent]
          ───────────     ──────────────          ───────────────────────    ──────────────
          ┌───────────┐   ┌──────────────┐        ┌──────────────┐           ┌──────────────┐
          │  Bước 1   │   │   Bước 2     │        │   Bước 3     │           │   Bước 4     │
          │ Đại lý    │──→│ 🔵 LLM trích │ ─────→ │ 🟢 NV xem    │ ────────→ │ 🔵 Agent     │
          │ nhập claim│   │ xuất thực    │        │ gợi ý AI,    │           │ clustering   │
          │           │   │ thể + gợi ý  │        │ xác nhận/sửa │           │ theo lot/ngày│
          │           │   │ mã lỗi +     │        │ + bấm DUYỆT/ │           │ → anomaly    │
          │           │   │ confidence + │        │ TỪ CHỐI      │           │ detection    │
          │           │   │ fraud flag   │        │              │           │              │
          │ ⏱ ~5 min  │   │ ⏱ ~10 giây  │        │ ⏱ ~2 min    │           │ ⏱ real-time  │
          └───────────┘   └──────────────┘        └──────────────┘           └──────────────┘
                                                                                    │
                                              ┌─────────────────────────────────────┘
                                              ▼
                                   [🟢 KS Chất Lượng — HITL]
                                   ┌──────────────────────────┐
                                   │         Bước 5           │
                                   │ KS nhận alert: "Cụm bất  │
                                   │ thường: 23 claims mã     │
                                   │ B-0x1A3 từ lot Hải Phòng │
                                   │ 2026-05-10 → 2026-05-20" │
                                   │ → xác nhận, phát hành    │
                                   │ ECN / SCAR               │
                                   │ ⏱ 2–3 ngày (vs 2–4 tuần)│
                                   └──────────────────────────┘

🔵 AI Step   🟢 Human Step (HITL)   ↩️ Fallback

↩️ Fallback:
  • Nếu LLM confidence < 70% → gắn cờ "Cần NV xem xét thủ công" → NV xử lý
    đầy đủ như quy trình cũ (không để AI đoán bừa).
  • Nếu Agent clustering lỗi/timeout → KS nhận raw data, truy nguyên thủ công.
```

---

# 🏁 Phase 5 — EVALUATE

## 5.1. AI Readiness Checklist

| # | Câu hỏi | Đánh giá |
|---|---------|----------|
| 1 | Có sẵn dữ liệu mẫu/logs sạch để test? | ✅ VinFast có lịch sử claims trong ERP → dùng để xây ground-truth & evaluate. |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát? | ✅ HITL bắt buộc ở bước duyệt claim; Fallback về thủ công khi confidence < 70%; AI chỉ đọc, không ghi lên MES/PLM. |
| 3 | Stakeholders sẵn sàng thay đổi quy trình? | ⚠️ NV Thẩm Định cần training quy trình mới; KS Chất Lượng cần xác nhận accuracy clustering trước khi tin tưởng alert. |

## 5.2. Ước lượng chi phí (Cost Estimate)

> Giả định scope **Phase 1 prototype**: chỉ LLM Feature cho Bước 2, dùng **Gemini 2.5 Flash**, volume ~**3.000 claims/ngày** (≈90.000/tháng), mỗi claim ~1.5K token input + 0.5K token output.

| Hạng mục | Ước lượng | Ghi chú |
|----------|-----------|---------|
| **Chi phí LLM API** | ~**$15–25/tháng** | 90K claims × ~2K token ≈ 180M token/tháng × đơn giá Flash (rất rẻ). Chi phí biến đổi gần như không đáng kể so với giá trị tạo ra. |
| **Chi phí xây dựng (one-off)** | ~**2–3 kỹ sư × 4–6 tuần** | Tích hợp ERP, prompt engineering, dataset 200 claims để đánh giá accuracy. |
| **Chi phí vận hành/giám sát** | ~**0.2 FTE** | Theo dõi accuracy, cập nhật catalog mã lỗi, xử lý các case fallback. |
| **Giá trị tiết kiệm (ROI)** | ~**12 phút × 3.000 claims/ngày ≈ 600 giờ công/ngày** | Quy đổi: giải phóng phần lớn thời gian đọc mô tả thủ công của đội thẩm định → ROI dương ngay trong tháng đầu. |

**Kết luận chi phí:** Chi phí API không đáng kể; chi phí chính là tích hợp & thay đổi quy trình (one-off). Giá trị tiết kiệm thời gian công + giảm rủi ro recall vượt xa chi phí → **ROI dương rõ rệt**.

## 5.3. Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

### ✅ **GO — với scope hẹp (Phase 1)**

**Lý giải dựa trên bằng chứng kỹ thuật & chi phí:**

> 1. **Dữ liệu sẵn sàng:** Lịch sử claims trong ERP cho phép xây ground-truth dataset ngay, đánh giá accuracy trước khi deploy.
> 2. **Rủi ro kỹ thuật thấp:** Trích xuất thực thể từ văn bản không cấu trúc là use case LLM đã kiểm chứng rộng rãi.
> 3. **An toàn được kiểm soát:** HITL bắt buộc ở bước duyệt claim → AI không bao giờ tự ra quyết định tài chính; AI chỉ đọc, không ghi lên hệ thống sản xuất.
> 4. **Metric rõ ràng & có baseline:** ~15 min/claim và 2–4 tuần phát hiện cụm lỗi là baseline đo được; chi phí API không đáng kể, ROI dương.
>
> **Scope prototype Phase 1:** Chỉ xây **LLM Feature cho Bước 2** (trích xuất + gợi ý mã lỗi) — chưa đụng Agent clustering (Bước 4–5). Đo accuracy mã lỗi gợi ý trên **200 claims lịch sử**. **Ngưỡng tối thiểu để tiếp tục Phase 2:** accuracy ≥ 90% **và** thời gian xử lý < 3 phút/claim.
>
> **Không chọn NO-GO** vì rule-based không xử lý được mô tả tự do đa dạng ngôn ngữ. **Không chọn NOT YET** vì đã có đủ dữ liệu và metric để bắt đầu — trì hoãn chỉ kéo dài rủi ro recall.

---

## 📎 Liên kết deliverable liên quan

* `01-problem-scan.md` — Phase 1 (SCAN) & Phase 2 (3 Quick Cards) — bài cá nhân.
* `04-workflow-diagram.png` — Sơ đồ Current-State Workflow vẽ tay (Phase 3.1).
* `starter-code/prompt_prototype.py` — Bản mẫu prompt + adversarial test (Phase 4).
* `03-ai-log.md` — Nhật ký chiêm nghiệm dùng AI (Phase 6) — bài cá nhân.
