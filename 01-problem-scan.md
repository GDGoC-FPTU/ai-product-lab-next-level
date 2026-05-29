# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán AI cho Vingroup (quét qua 4 Lenses)

| #  | Subsidiary | Lens chính | Bài toán / Bottleneck | Vì sao đau / Tác động |
|----|------------|-----------|-----------------------|-----------------------|
| 1  | Vingroup | AI có thể tốt hơn | Chatbot hỏi–đáp tri thức ứng dụng AI nâng cao nhận thức về văn hóa, tư duy chất lượng và hệ sinh thái doanh nghiệp cho ứng viên **trước khi phỏng vấn** | Ứng viên vào phỏng vấn thiếu hiểu về văn hóa & hệ sinh thái → phỏng vấn kém hiệu quả, lệch kỳ vọng, tỷ lệ phù hợp thấp; HR tốn công giải thích lặp lại |
| 2  | VinUni | AI có thể tốt hơn | Chatbot tri thức về **đạo đức AI, sử dụng AI có trách nhiệm & khai thác AI an toàn** cho học sinh, sinh viên: tránh phụ thuộc công nghệ, hạn chế thông tin sai lệch, hình thành năng lực dùng AI minh bạch, có kiểm chứng, đúng chuẩn mực học thuật | Người học dùng AI tùy tiện → phụ thuộc, tiếp nhận thông tin sai, vi phạm liêm chính học thuật; thiếu công cụ định hướng dùng AI an toàn ở quy mô lớn |
| 3  | Vingroup | Tốn thời gian | Chatbot ứng dụng AI **lọc, phân loại & chấm điểm ý tưởng phát triển AI** được đề xuất, đối chiếu theo bộ framework để tìm ý tưởng phù hợp nhất với định hướng tập đoàn/doanh nghiệp | Sàng lọc ý tưởng thủ công tốn thời gian, thiếu nhất quán; ý tưởng tốt dễ bị bỏ sót, ý tưởng lệch định hướng vẫn lọt → lãng phí nguồn lực thẩm định |
| 4  | VinFast       | Tốn thời gian | Nhập & thẩm định claim bảo hành thủ công, chậm truy nguyên nhân gốc về dây chuyền | Chi phí bảo hành ~2–3% doanh thu; chậm phát hiện lỗi hệ thống → rủi ro recall |
| 5  | Xanh SM       | AI có thể tốt hơn | Dự báo & gợi ý khu vực/điểm đón tiềm năng theo thời gian thực (demand hotspot) cho tài xế rỗng: dự đoán nơi sắp có nhu cầu cao theo giờ–khu vực–thời tiết–sự kiện, để xe chủ động về đúng chỗ trước giờ cao điểm | Xe rỗng "đoán mò" → chạy rỗng tốn pin & thời gian; khách giờ cao điểm chờ lâu/không bắt được xe → hủy chuyến. Khớp cung–cầu kém làm giảm cả thu nhập tài xế lẫn tỷ lệ hoàn thành chuyến| |
---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                             │
│ Bài toán (1 câu): Nhập & thẩm định claim bảo hành thủ công, │
│   chậm truy nguyên nhân gốc về dây chuyền sản xuất.         │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? NV thẩm định claim ở trung tâm dịch vụ │
│   + kỹ sư chất lượng ở nhà máy (truy nguyên lỗi hệ thống).  │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đại lý nhập claim (VIN, mã lỗi, mô tả tự do, ảnh) ──>  │
│   2. NV đọc claim, đối chiếu điều kiện BH, duyệt/từ chối ──>│
│   3. Gom claim đã duyệt, phân loại theo cụm linh kiện ──>   │
│   4. Kỹ sư truy về lot / dây chuyền / nhà cung cấp          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (đọc & thẩm định mô │
│   tả tự do) (⏱ ~15 phút/lượt); bước 4 truy nguyên ~vài tuần │
│ AI có thể nhảy vào hỗ trợ ở bước nào? B2: LLM trích xuất &  │
│   chuẩn hóa, gợi ý mã lỗi, gắn cờ claim đáng ngờ. B4:       │
│   clustering triệu chứng + anomaly detection theo lot.      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thẩm định 1 claim từ 15 min ──> under 3 min;        │
│    phát hiện cụm lỗi hệ thống từ vài tuần ──> vài ngày"     │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [ ] LLM [x] Agent    │
└─────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                             │
│ Bài toán (1 câu): Tài xế rỗng "đoán mò" nơi có khách → chạy │
│   rỗng tốn pin/thời gian, khớp cung–cầu giờ cao điểm kém.   │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế đang ở trạng thái rỗng (idle);  │
│   gián tiếp: đội điều phối + khách giờ cao điểm hủy chuyến. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tài xế trả khách → trạng thái rỗng ──>                 │
│   2. Tự phán đoán theo kinh nghiệm nên đi đâu (đoán mò) ──> │
│   3. Di chuyển tới khu tự chọn → có thể sai hướng, rỗng ──> │
│   4. Chờ/dò chuyến, nhận được hoặc tiếp tục di chuyển       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (phán đoán sai +  │
│   di chuyển rỗng) (⏱ ~10–20 phút chạy rỗng/lượt)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? B2: mô hình dự báo    │
│   nhu cầu theo grid (khu vực × giờ) + thời tiết + sự kiện + │
│   cung hiện tại → đẩy gợi ý "về khu X trong 15' tới".       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian chạy rỗng từ 15 min ──> under 8 min/lượt; │
│    tăng tỷ lệ hoàn thành chuyến + thu nhập/giờ tài xế"      │
│                                                             │
│ Quick Architecture: [ ] No AI [x] Rule [ ] LLM [ ] Agent    │
│   (thực chất: ML dự báo geo-demand + Rule layer xuất gợi ý) │
└─────────────────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                             │
│ Bài toán (1 câu): Sàng lọc/chấm điểm ý tưởng AI thủ công,   │
│   tốn thời gian, thiếu nhất quán, dễ bỏ sót ý tưởng tốt.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│              [ ] Vinmec  [x] Khác: Vingroup (cấp tập đoàn)   │
│                                                             │
│ Ai đang đau (Actor)? Ban/hội đồng thẩm định ý tưởng — người │
│   gác cổng sàng lọc đề xuất từ các đơn vị.                  │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đơn vị/cá nhân gửi đề xuất ý tưởng AI (form/doc) ──>   │
│   2. Ban thẩm định đọc từng đề xuất ──>                     │
│   3. Đối chiếu thủ công với từng tiêu chí framework         │
│      (định hướng, khả thi, ROI, nguồn lực) ──>             │
│   4. Cho điểm, xếp hạng → chọn shortlist trình hội đồng     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (đọc + đối chiếu  │
│   đa tiêu chí, điểm lệch giữa người chấm) (⏱ ~20–30 ph/ý)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? B2–4: LLM đọc đề xuất,│
│   chấm từng tiêu chí kèm lý do, gắn cờ trùng lặp/lệch định  │
│   hướng, xuất bảng xếp hạng (human-in-the-loop quyết định). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm sàng lọc/ý tưởng từ 25 min ──> under 5 min; tăng    │
│    nhất quán điểm; giảm tỷ lệ bỏ sót ý tưởng tốt"           │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
│   (RAG nạp framework + scoring rubric)                       │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping

### Sơ đồ quy trình hiện tại

```
Actors:   [Đại lý / SC]        [NV Thẩm Định]           [NV Thẩm Định]           [NV Thẩm Định]
          ─────────────        ──────────────           ──────────────           ──────────────
          ┌────────────┐       ┌──────────────┐         ┌──────────────┐         ┌──────────────┐
          │  Bước 1    │       │   Bước 2     │         │   Bước 3     │         │   Bước 4     │
          │            │       │              │         │              │         │              │
          │ Đại lý     │  🔄   │ NV đọc claim │  🔴     │ NV chuẩn    │         │ Gom batch    │
          │ nhập claim │ ────→ │ & kiểm tra   │ ────→   │ hóa mã lỗi  │ ────→   │ claim đã duyệt│
          │ vào portal │       │ điều kiện BH │         │ + duyệt/từ  │         │ → phân cụm   │
          │            │       │              │         │ chối / YC   │         │ linh kiện    │
          │ Tool: App  │       │ Tool: ERP,   │         │ bổ sung      │         │              │
          │ Đại lý     │       │ Catalog BH   │         │              │         │ Tool: Excel  │
          │            │       │              │         │              │         │              │
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
                                                   │   Bước 5     │         │   Bước 6     │
                                                   │              │  🔴     │              │
                                                   │ KS nhận báo  │ ────→   │ KS phát hành │
                                                   │ cáo cụm claim│         │ ECN / SCAR   │
                                                   │ → truy lot,  │         │ (corrective  │
                                                   │ date code,   │         │ action)      │
                                                   │ nhà cung cấp │         │              │
                                                   │              │         │              │
                                                   │ Tool: MES,   │         │ Tool: PLM,   │
                                                   │ SAP, Excel   │         │ Email        │
                                                   │              │         │              │
                                                   │ ⏱ ~2–4 tuần  │         │ ⏱ ~1 tuần   │
                                                   │ /cụm lỗi 🔴  │         │ /cụm lỗi    │
                                                   └──────────────┘         └──────────────┘

🔴 = Bottleneck chính
🔄 = Handoff giữa các bộ phận / hệ thống

⏱ Tổng thẩm định 1 claim:     ~25 phút/lượt
⏱ Tổng phát hiện lỗi hệ thống: ~3–6 tuần/cụm
```

### Chi tiết từng bước

| Bước | Actor | Mô tả | Tool | Thời gian | Vấn đề |
|------|-------|-------|------|-----------|--------|
| 1 | Đại lý / Service Center | Nhập claim: VIN, mã OBD, mô tả tự do, ảnh chẩn đoán | Portal đại lý | ~5 min/claim | Mô tả tự do không nhất quán, thiếu chuẩn hóa |
| 2 🔴 | NV Thẩm Định | Đọc mô tả tự do, đối chiếu điều kiện BH (km/ngày), phán đoán phạm vi bảo hành | ERP, Catalog BH PDF | ~15 min/claim | Xử lý ngôn ngữ tự nhiên thủ công, dễ bỏ sót, không nhất quán giữa NV |
| 3 | NV Thẩm Định | Chuẩn hóa mã lỗi về catalog chuẩn, duyệt / từ chối / yêu cầu bổ sung | ERP | ~5 min/claim | Mã hóa sai → truy nguyên sai cụm linh kiện |
| 4 🔄 | NV Thẩm Định | Gom claim theo tuần, phân loại thủ công theo cụm: pin, động cơ, điện tử, thân vỏ | Excel | ~30 min/tuần | Gom theo lô → delay 1–2 ngày, pattern lỗi bị che khuất |
| 5 🔴 | Kỹ Sư Chất Lượng | Nhận báo cáo cụm, truy nguyên lot sản xuất, date code, nhà cung cấp linh kiện | MES, SAP, Excel | ~2–4 tuần/cụm | Query thủ công nhiều hệ thống rời rạc, mất nhiều tuần để xác nhận pattern |
| 6 | Kỹ Sư Chất Lượng | Phát hành Engineering Change Notice (ECN) hoặc Supplier Corrective Action Request | PLM, Email | ~1 tuần/cụm | Rủi ro recall nếu phát hiện quá trễ |

---

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|-------|-------------------|
| **1. Actor / Operator** | (a) NV Thẩm Định Bảo Hành tại Trung tâm Dịch vụ VinFast — người thẩm định từng claim. (b) Kỹ sư Chất Lượng tại nhà máy — người truy nguyên nhân gốc về dây chuyền. |
| **2. Current Workflow** | Đại lý nhập claim (VIN + mô tả tự do + ảnh) → NV đọc thủ công, đối chiếu điều kiện BH, chuẩn hóa mã lỗi, duyệt/từ chối → Gom batch cuối tuần, phân cụm linh kiện bằng Excel → KS truy nguyên lot/dây chuyền/NCC trên MES + SAP. Toàn bộ thủ công, rời rạc, không có tự động hóa. |
| **3. Bottleneck** | **Bước 2:** Đọc & thẩm định mô tả tự do (~15 min/claim) — xử lý ngôn ngữ tự nhiên, đánh giá điều kiện BH, gắn mã lỗi chuẩn. **Bước 5:** Truy nguyên cụm lỗi về lot/NCC (~2–4 tuần) — query thủ công nhiều hệ thống MES/SAP rời rạc, không có anomaly detection tự động. |
| **4. Business Impact** | Chi phí bảo hành ~2–3% doanh thu VinFast/năm. Mỗi NV xử lý ~40–60 claims/ngày × 15 min = 10–15 giờ công/NV/ngày bị đốt vào đọc mô tả. Phát hiện cụm lỗi hệ thống sau 2–4 tuần → rủi ro recall leo thang, chi phí cao hơn gấp nhiều lần nếu vấn đề đã lan rộng. |
| **5. Success Metric** | 1. Giảm thời gian thẩm định 1 claim từ 15 phút → dưới 3 phút (80% reduction). 2. Phát hiện cụm lỗi hệ thống bất thường từ 2–4 tuần → trong vòng 2–3 ngày kể từ khi có đủ 10+ claims cùng pattern. 3. Tỉ lệ gắn mã lỗi chuẩn chính xác ≥ 95% (so với baseline hiện tại do NV thực hiện). |
| **6. Operational Boundary** | **AI được phép:** Trích xuất thực thể từ mô tả tự do (triệu chứng, bộ phận, điều kiện), gợi ý mã lỗi chuẩn kèm confidence score, gắn cờ claim đáng ngờ (fraud risk), phát hiện anomaly clustering trên tập claim theo lot/ngày sản xuất. **TUYỆT ĐỐI CẤM:** AI không được tự động phê duyệt claim → toàn bộ quyết định duyệt/từ chối phải do NV (HITL). AI không được truy cập hoặc đề xuất hành động trên hệ thống MES/PLM sản xuất — chỉ được đọc và báo cáo. AI không được giao tiếp trực tiếp với khách hàng/đại lý. |

---

## 3.3. Future-State Flow & AI Fit

### AI Fit Matrix

| Tiêu chí | Đánh giá |
|----------|----------|
| **Mức AI Fit** | **Agent** (Agentic Loop) cho bước phát hiện cụm lỗi (tool use: query MES + clustering). **LLM Feature** cho bước thẩm định claim (trích xuất + gợi ý mã). |
| **Lý do không dùng Rule-based** | Mô tả claim là ngôn ngữ tự nhiên không có cấu trúc cố định — rule/regex không thể xử lý biến thể ngôn ngữ phong phú (tiếng Việt + tiếng Anh kỹ thuật + viết tắt đại lý). |
| **Lý do không dùng Agent toàn phần** | Bước duyệt claim có hậu quả tài chính và pháp lý → bắt buộc HITL. Agent chỉ được phép đề xuất, không được quyết định. |

### Sơ đồ Future-State

```
Actors:   [Đại lý / SC]     [🔵 AI Layer]             [🟢 NV Thẩm Định — HITL]    [🔵 AI Layer]
          ─────────────     ──────────────             ────────────────────────    ──────────────
          ┌───────────┐     ┌──────────────┐           ┌──────────────┐            ┌──────────────┐
          │  Bước 1   │     │   Bước 2     │           │   Bước 3     │            │   Bước 4     │
          │           │     │              │           │   🟢 HITL    │            │              │
          │ Đại lý    │ ──→ │ 🔵 LLM trích │ ────────→ │ NV xem gợi  │ ──────────→│ 🔵 Agent     │
          │ nhập claim│     │ xuất thực    │           │ ý AI, xác   │            │ clustering   │
          │           │     │ thể + gợi ý  │           │ nhận / sửa  │            │ claims theo  │
          │           │     │ mã lỗi +     │           │ mã lỗi +    │            │ lot/ngày/NCC │
          │           │     │ confidence + │           │ bấm DUYỆT / │            │ → anomaly    │
          │           │     │ fraud flag   │           │ TỪ CHỐI      │            │ detection    │
          │           │     │              │           │              │            │              │
          │ ⏱ ~5 min  │     │ ⏱ ~10 giây  │           │ ⏱ ~2 min    │            │ ⏱ real-time  │
          └───────────┘     └──────────────┘           └──────────────┘            └──────────────┘
                                                                                          │
                                                              ┌───────────────────────────┘
                                                              ▼
                                                   [🟢 KS Chất Lượng — HITL]
                                                   ──────────────────────────
                                                   ┌──────────────────────────┐
                                                   │         Bước 5           │
                                                   │         🟢 HITL          │
                                                   │ KS nhận alert: "Cụm bất  │
                                                   │ thường: 23 claims mã     │
                                                   │ B-0x1A3 từ lot Hải Phòng │
                                                   │ 2026-05-10 → 2026-05-20" │
                                                   │ → KS xác nhận, phát hành │
                                                   │ ECN / SCAR               │
                                                   │                          │
                                                   │ ⏱ 2–3 ngày (vs 2–4 tuần)│
                                                   └──────────────────────────┘

                                                   ↩️ Fallback:
                                                   Nếu LLM confidence < 70% →
                                                   gắn cờ "Cần NV xem xét thủ công"
                                                   → NV xử lý đầy đủ như quy trình cũ.
                                                   Nếu Agent clustering lỗi → KS nhận
                                                   raw data, truy nguyên thủ công.
```

### So sánh Rule vs LLM vs Agent

| Tiêu chí | Rule / State-Machine | LLM Feature | Agent (Agentic Loop) |
|----------|---------------------|-------------|----------------------|
| **Thẩm định claim** | ❌ Không xử lý được ngôn ngữ tự do | ✅ Trích xuất + gợi ý mã lỗi | Overkill |
| **Phát hiện cụm lỗi** | ❌ Cần cấu trúc cứng, không linh hoạt | ❌ Không đủ reasoning về hệ thống | ✅ Tool use: query MES + clustering |
| **Lý do loại Rule** | Mô tả claim không có cấu trúc cố định; biến thể ngôn ngữ quá nhiều | — | — |
| **Rủi ro** | Thấp (đơn giản) | Trung bình (hallucination mã lỗi) → dùng HITL | Cao hơn (tool use sai) → dùng HITL + audit log |

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
| # | Câu hỏi | Đánh giá |
|---|---------|----------|
| 1 | Có sẵn dữ liệu mẫu/logs sạch để test? | ✅ VinFast có lịch sử claims ERP → dùng để fine-tune/evaluate |
| 2 | Rủi ro khi AI sai có trong tầm kiểm soát? | ✅ HITL bắt buộc ở bước duyệt claim; Fallback về thủ công khi confidence < 70% |
| 3 | Stakeholders sẵn sàng thay đổi quy trình? | ⚠️ NV Thẩm Định cần training quy trình mới; KS Chất Lượng cần xác nhận accuracy của clustering trước khi tin tưởng |


### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
**Quyết định: ✅ GO — với scope hẹp**

**Lý giải:**

> Bài toán có đủ điều kiện để bắt đầu prototype vì: (1) Dữ liệu lịch sử claims trong ERP đã có — có thể xây dựng ground-truth dataset ngay. (2) LLM trích xuất thực thể từ văn bản không có cấu trúc là use case đã được kiểm chứng rộng rãi — rủi ro kỹ thuật thấp. (3) HITL ở bước duyệt claim đảm bảo AI không bao giờ tự động ra quyết định tài chính. (4) Metric đo lường rõ ràng và có baseline hiện tại (~15 min/claim, ~2–4 tuần phát hiện cụm lỗi).
>
> **Scope prototype Phase 1:** Chỉ xây LLM Feature cho Bước 2 (trích xuất + gợi ý mã lỗi) — không đụng đến Agent clustering ở Bước 4 trong giai đoạn đầu. Đo accuracy của mã lỗi gợi ý trên 200 claims lịch sử trước khi deploy. Ngưỡng tối thiểu để tiếp tục: accuracy ≥ 90% và thời gian xử lý < 3 phút/claim.
>
> **Không chọn NO-GO** vì rule-based không thể xử lý mô tả tự do đa dạng ngôn ngữ. **Không chọn NOT YET** vì đã có đủ dữ liệu và metric để bắt đầu — trì hoãn chỉ kéo dài rủi ro recall.


**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
