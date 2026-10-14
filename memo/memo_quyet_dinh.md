# Memo quyết định: GOV.UK Chat

**Ngày:** 14/10/2026  
**Quyết định:** Sửa rồi tiếp tục pilot có kiểm soát

## 1. Vấn đề và nguyên nhân gốc

Pilot GOV.UK Chat giúp người dùng hỏi dịch vụ công bằng ngôn ngữ tự nhiên, nhưng baseline ban đầu chưa đủ để rollout rộng. Accuracy ban đầu là 76%, có hallucination, và nghiên cứu cho thấy một số người dùng đánh giá thấp rủi ro sai vì họ tin vào thương hiệu GOV.UK.

Nguyên nhân gốc thứ nhất là readiness và kiến trúc tin cậy chưa hoàn chỉnh: retrieval/chunking, eval, regression và kiểm soát nguồn chưa đủ mạnh. Nguyên nhân thứ hai là workflow chưa xử lý trọn vẹn câu hỏi mơ hồ, ngoài phạm vi hoặc cần hỗ trợ có dữ liệu cá nhân; chưa có handoff đo được.

## 2. Framework và bằng chứng

- **Gartner-Lite:** Direction đạt; Readiness thiếu ở retrieval/eval; Absorption thiếu ở owner vận hành, feedback và handoff.
- **Mollick:** AI phù hợp với tìm kiếm, tổng hợp và tạo nháp câu trả lời. Con người giữ quyền với nội dung chính thức, ngoại lệ, safety và trường hợp cá nhân.
- **ADKAR:** nghẽn chính ở Desire và Reinforcement. Cần tạo niềm tin bằng nguồn, QA và feedback; đào tạo riêng lẻ không giải quyết được.

Bằng chứng công khai từ GDS cho thấy gần 70% người dùng ban đầu thấy câu trả lời hữu ích và gần 65% hài lòng, nhưng accuracy chưa đạt chuẩn và có hallucination. Báo cáo pilot sau đó cho thấy accuracy tăng từ 76% lên 90%, usefulness 73%, satisfaction 64%, answer rate 88% và latency trung bình 10,7 giây.

## 3. Thay đổi sau tự kiểm theo rubric

Không có nhóm phản biện chéo. Các thay đổi dưới đây được thực hiện sau khi tự kiểm theo bốn trục phạm vi, framework, chỉ số và hành động:

1. Thay metric activity “số người/số câu hỏi” bằng accuracy, usefulness, satisfaction, latency, answer rate và handoff success. V2 phân biệt product metric và workflow metric.
2. Bổ sung nguồn dữ liệu, owner và hành động khi chỉ số xấu cho từng dòng. Metric thiếu baseline được gắn gate “cần đo”, không được coi là đạt.
3. Thêm clarification và handoff vào TO-BE, cùng regression suite và review failure set hằng tuần.

## 4. Quyết định

**SỬA rồi tiếp tục pilot có kiểm soát.** Không rollout rộng chỉ vì hết 90 ngày hoặc volume tăng. Gate mở rộng yêu cầu accuracy ≥90%, usefulness ≥73%, satisfaction ≥70%, latency ≤10 giây, handoff ≥80% và không có sự cố an toàn nghiêm trọng.

## 5. Lý do, bước tiếp theo và owner

Accuracy 90% chứng minh hướng tiếp cận có thể đạt chuẩn mục tiêu. Tuy nhiên satisfaction mới nhất 64% và latency 10,7 giây vẫn chưa đạt gate, còn handoff chưa có baseline công khai. Vì vậy nhóm tiếp tục pilot để sửa UX/tốc độ và hoàn thiện bàn giao.

- **0–30 ngày:** Product Manager và Content Lead khóa scope, owner và baseline.
- **31–60 ngày:** AI Quality Lead và Service Owner sửa retrieval, citations, clarification, handoff và chạy QA/regression.
- **61–90 ngày:** Service Owner đánh giá dashboard v2 và quyết định mở rộng, sửa tiếp hoặc dừng.

## Nguồn

- https://insidegovuk.blog.gov.uk/2024/01/18/the-findings-of-our-first-generative-ai-experiment-gov-uk-chat/
- https://www.gov.uk/government/publications/ai-playbook-for-the-uk-government/artificial-intelligence-playbook-for-the-uk-government-html
- https://insidegovuk.blog.gov.uk/2026/03/16/5-things-we-learned-testing-gov-uk-chat-an-ai-assistant-for-government/
- https://www.gov.uk/algorithmic-transparency-records/dsit-gov-dot-uk-chat
- https://docs.publishing.service.gov.uk/repos/govuk-chat-evaluation.html
