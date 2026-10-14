# Day26 Track 01: AI Adoption & Change Management

Case thực tế: **GOV.UK Chat**. Bài phân tích dùng trạng thái pilot 2023–2024 làm baseline và số liệu công khai mới hơn để kiểm tra logic ra quyết định. Không có dữ liệu nội bộ của doanh nghiệp trong repo.

## 1. Thành viên

| Họ tên | MSSV | Phần phụ trách | Góp ý đã đưa cho nhóm bạn |
|---|---|---|---|
| Trần Thị Thuý | 2A202602960 | Chẩn đoán Gartner-Lite/ADKAR/Mollick, workflow, dashboard và memo | Không có nhóm phản biện; tự kiểm theo rubric: thay metric activity bằng accuracy và task completion; thêm handoff, owner và action khi chỉ số xấu. |

## 2. Phạm vi

- **Sản phẩm AI:** GOV.UK Chat, trợ lý hỏi đáp dựa trên nội dung chính thức của GOV.UK.
- **Người dùng chính:** người dân và người dùng doanh nghiệp cần tìm hướng dẫn dịch vụ công.
- **Ba quy trình:** đặt câu hỏi; kiểm chứng nguồn; chuyển sang hỗ trợ con người khi câu hỏi cần dữ liệu cá nhân hoặc nằm ngoài phạm vi.
- **Vấn đề quan sát:** accuracy ban đầu chưa đủ cho rollout rộng, xuất hiện hallucination, và uy tín thương hiệu GOV.UK có thể khiến người dùng đánh giá thấp rủi ro sai.

## 3. Nguyên nhân gốc

1. **Readiness và kiến trúc tin cậy chưa đủ.** Gartner-Lite cho thấy Direction đạt nhưng Readiness còn thiếu: retrieval/chunking làm mất ngữ cảnh, eval chưa đủ để chặn regressions. GDS ghi nhận accuracy ban đầu 76%, có câu trả lời hallucinated và các trang dài gây khó cho retrieval.
2. **Workflow chưa có clarification và handoff hoàn chỉnh.** Gartner-Lite Absorption và Mollick cho thấy AI đã trả lời nhưng chưa có quy trình trọn vẹn cho câu hỏi mơ hồ, ngoài phạm vi hoặc cần tư vấn viên có dữ liệu cá nhân. ADKAR cho thấy nghẽn ở Desire/Reinforcement: người dùng cần cơ chế kiểm chứng, phản hồi và học từ lỗi, không chỉ hướng dẫn sử dụng.

Nguồn chính: [Inside GOV.UK, 18/01/2024](https://insidegovuk.blog.gov.uk/2024/01/18/the-findings-of-our-first-generative-ai-experiment-gov-uk-chat/) và [Inside GOV.UK, 16/03/2026](https://insidegovuk.blog.gov.uk/2026/03/16/5-things-we-learned-testing-gov-uk-chat-an-ai-assistant-for-government/).

## 4. Cách làm mới

**AS-IS:** hỏi tự do → hệ thống tìm nguồn → LLM trả lời → người dùng tự quyết định.

**TO-BE:** phân loại phạm vi → hỏi lại khi mơ hồ → retrieval có benchmark → trả lời kèm nguồn → kiểm tra groundedness/QA mẫu → dùng hoặc handoff sang người → feedback vào failure set và regression suite.

Ba thay đổi bắt buộc trong bản v2:

- Nguồn kiểm chứng hiển thị cùng câu trả lời và dùng trong QA.
- AI Quality Lead, Content Lead và Service Owner chịu trách nhiệm cho chất lượng, nội dung và bàn giao.
- Nếu AI không chắc chắn, hệ thống hỏi lại, từ chối đúng phạm vi hoặc chuyển người thay vì đoán.

## 5. Chỉ số

- **Product metric:** accuracy do chuyên gia đánh giá. Baseline 76%, target 90%, nguồn là bộ eval và QA mẫu, owner là AI Quality Lead. Nếu xấu: phân tích failure set, sửa retrieval/prompt và chặn release.
- **Workflow metric:** usefulness, satisfaction, latency và handoff success. Mỗi metric có baseline/target, nguồn log hoặc khảo sát, owner và hành động khi xấu trong `dashboard/dashboard_hanh_dong_v2.xlsx`.
- Baseline chưa được công bố được ghi rõ “đo tuần 1” hoặc “chưa có quy trình”; không thay bằng số giả.

## 6. Quyết định

**SỬA rồi tiếp tục pilot có kiểm soát.** Accuracy đã chứng minh có thể đạt 90%, nhưng satisfaction mới nhất 64% thấp hơn target 70%, latency 10,7 giây cao hơn target 10 giây, và handoff chưa có baseline công khai.

Hai thay đổi chính so với v1:

1. Thay dashboard thiên về volume/usage bằng product metric về accuracy và workflow metric về chất lượng, năng suất, giá trị và bàn giao.
2. Bổ sung owner, nguồn dữ liệu, action khi metric xấu và gate 30–60–90 có điều kiện chuyển giai đoạn.

## Cấu trúc repo

```text
Day26_Track01_GOVUK_Chat/
├── README.md
├── dashboard/
│   └── dashboard_hanh_dong_v2.xlsx
├── memo/
│   └── memo_quyet_dinh.md
└── v1/
    └── dashboard_hanh_dong_v1.xlsx
```

## Nguồn công khai

- [GOV.UK Chat: first generative AI experiment](https://insidegovuk.blog.gov.uk/2024/01/18/the-findings-of-our-first-generative-ai-experiment-gov-uk-chat/)
- [UK Government AI Playbook](https://www.gov.uk/government/publications/ai-playbook-for-the-uk-government/artificial-intelligence-playbook-for-the-uk-government-html)
- [GOV.UK Chat: five findings from the pilots](https://insidegovuk.blog.gov.uk/2026/03/16/5-things-we-learned-testing-gov-uk-chat-an-ai-assistant-for-government/)
- [GOV.UK Algorithmic Transparency Record](https://www.gov.uk/algorithmic-transparency-records/dsit-gov-dot-uk-chat)
- [GOV.UK Chat evaluation repository documentation](https://docs.publishing.service.gov.uk/repos/govuk-chat-evaluation.html)
