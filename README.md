# Track 1 - Day 25: AI Pricing, GTM & Evidence Lab

**Học viên:** Đỗ Tú Anh  
**Mã học viên:** 2A202601272
**Khóa học:** VLearn - Học AI thực chiến qua Lab  
**Lab:** Từ sản phẩm chạy được đến sản phẩm bán được

---

## 🎯 Giới thiệu Dự án: AI Code Reviewer

Repository này chứa toàn bộ hồ sơ định giá, mô hình tài chính và chiến lược Go-To-Market (GTM) cho sản phẩm **AI Code Reviewer**. Đây không phải là một chatbot AI chung chung, mà là một công cụ quy trình chuyên dụng được thiết kế riêng cho các dự án Web App (Typescript/React). Sản phẩm tự động đọc Pull Request (PR) và phát hiện các lỗi logic nghiệp vụ, lỗi hydration trong React, cũng như các lỗ hổng bảo mật sơ đẳng ngay trên giao diện GitHub.

**Định nghĩa "1 Job" (Đơn vị tạo ra giá trị lõi):** 1 Pull Request được hệ thống AI review hoàn tất (đã phân tích mã nguồn, phát sinh comment cảnh báo lỗi hoặc tự động approve).

---

## 📊 Tóm tắt Mô hình Kinh doanh & Chiến lược (Executive Summary)

Dự án đã phân tích và đáp ứng toàn bộ các tiêu chuẩn khắt khe nhất của Lab Day 25. Dưới đây là bức tranh toàn cảnh về mặt tài chính và chiến lược thâm nhập thị trường:

### 1. Nguồn ngân sách & Lựa chọn Value Metric
- **Ngân sách mục tiêu (Buyer):** Sản phẩm nhắm vào túi tiền của danh mục **Phần mềm & Công cụ Dev (DevTools/SaaS)**. Người ra quyết định (Buyer) là CTO hoặc VP of Engineering. Người dùng cuối (User) là Software Engineers.
- **Value Metric:** Quyết định sử dụng mô hình **Hybrid (Thu theo Seat + Giới hạn Hard Cap)**. 
  - Tính phí **$15/User/Tháng**.
  - Giới hạn cứng: **40 PR/tháng**. Nếu sử dụng vượt mức, tính phí $0.20/PR.
  - **Lý do lựa chọn:** Khách hàng khối doanh nghiệp (B2B) và bộ phận Procurement rất sợ các mô hình thanh toán biến động (unpredictable billing). Họ đã quen thuộc và dễ dàng "xuống tiền" với mô hình trả phí cố định trên đầu người (Seat) như GitHub Copilot. Thiết lập "Hard Cap 40 PR" là chốt chặn sinh tử để bảo vệ biên lợi nhuận (Gross Margin) khỏi các "heavy users" (những lập trình viên chia vụn commit và push liên tục).

### 2. Phân tích Độ chặt chẽ của Chi phí (Cost/Job Rigor)
Mô hình đã bóc tách rõ ràng 5 thành phần chi phí cấu thành cho 1 Job (lấy giá API cập nhật ngày 27/08/2026):
- **TỔNG COST/JOB:** **$0.052 / PR**
  - **LLM API ($0.033):** Sử dụng **Claude 3.5 Sonnet** ($2/$10) tích hợp Prompt Caching (giúp tiết kiệm 90% chi phí input). Một PR tiêu tốn khoảng 25k token input và 1k token output.
  - **Infra & DB ($0.005):** Phí duy trì Webhook lắng nghe sự kiện GitHub và chi phí Vector DB.
  - **Retry ($0.004):** Bù hao 10% tỷ lệ chạy lại do dính rate limit hoặc hallucination.
  - **Overhead ($0.01):** Khấu hao máy chủ và chia bổ chi phí R&D, Support.
  - **HITL - Human In The Loop ($0):** Sản phẩm chạy theo Biến thể A, định vị là một "Tool gợi ý". Lập trình viên phải tự đọc comment và đưa ra quyết định sửa code (self-escalate). Do đó, Containment Rate luôn bằng 100% (công ty không tốn chi phí nhân sự ngồi sửa lỗi cho khách).
- **Điểm đứt gãy của mô hình:** Mô hình không bị sập do chi phí HITL (vì bằng 0), nhưng sẽ sập nếu tỷ lệ phát hiện lỗi sai (False Positive) vượt quá **15%**. Khi đó User sẽ cảm thấy phiền vì comment rác, dẫn đến tỷ lệ hủy gói (Churn) tăng đột biến và tàn phá giá trị LTV.
- **Biên lợi nhuận gộp (Gross Margin):** **89% - 93%**. Ngay cả khi người dùng chạm trần 40 PR, biên lợi nhuận vẫn an toàn ở mức 86%.

### 3. Chiến lược Kênh phân phối (Go-To-Market & Channel Fit)
- **Kênh GTM lựa chọn:** **PLG (Product-Led Growth)** tích hợp chặt chẽ qua GitHub Marketplace.
- **Biện luận tài chính (Affordability):** Giả sử một công ty mua gói cho 10 devs (ARPU = $150/tháng, $1,800/năm). Với biên lợi nhuận 93% và Payback 12 tháng, ngân sách CAC tối đa có thể chi trả chỉ rơi vào khoảng **$1,675**. Áp dụng định luật Tomasz Tunguz: *Chỉ với ACV (Annual Contract Value) trên $3,000, startup mới đủ tiền nuôi đội ngũ Inside Sales (AE)*. Do đó, kênh tự phục vụ (Self-serve) qua PLG là cách duy nhất khả thi về mặt tài chính. Mô hình này không dùng đội Sales, không cần tính các chỉ số Deal/AE/ngày hay cost-per-opportunity.
- **Pain Moment (Điểm chạm hoàn hảo):** *16h45 chiều Thứ 6*. Lập trình viên vừa push nhánh code cuối cùng lên và mòn mỏi chờ đồng nghiệp review PR để có thể gập máy. Không bắt khách hàng cài thêm App mới; hệ thống tự động trigger qua `.github/workflows/ai-review.yml` và comment trực tiếp vào tab "Files changed" chỉ 30 giây sau khi mở PR.

### 4. Kế hoạch 90 Ngày (90-Day Rollout Plan)
Kế hoạch tung sản phẩm thực tế với nguồn lực hạn chế, tập trung đánh sâu vào 1 kênh duy nhất:
- **Tháng 1 (Giai đoạn Bám rễ & Học hỏi):** Code xong workflow, đưa app lên GitHub Marketplace. Đặt KPI 50 lượt cài đặt (bản free) để lấy số liệu thực tế về tần suất push code và đối soát lại mô hình Cost/Job.
- **Tháng 2 (Giai đoạn Khuếch đại):** Phát tán Case study thực chiến trên Reddit, HackerNews. KPI: Đạt 300 lượt cài đặt.
- **Tháng 3 (Giai đoạn Chuyển đổi):** Tối ưu hóa chuỗi Drip Email Marketing tự động cho các tài khoản hết hạn Trial. KPI: Chốt thành công 20 tổ chức trả phí.

### 5. Gói bằng chứng thuyết phục (Evidence Pack Readiness)
Để đánh gục sự hoài nghi của hội đồng Mua hàng (Procurement) và IT Security, sản phẩm chuẩn bị sẵn 3 "lá chắn" bằng chứng:
1. **Eval Results (Dành cho CTO):** Báo cáo kiểm thử tự động trên tập SWE-bench, cam kết tỷ lệ bắt sai (False Positive) < 15%. Chứng minh hệ thống giúp dọn rác, không tạo thêm việc rác cho dev.
2. **Risk & Compliance Checklist (Dành cho IT Security):** Công bố chính sách **Zero Data Retention** (Tuyệt đối không lưu mã nguồn để train AI), đính kèm chứng nhận SOC2 Type II để đối phó với Security Team.
3. **Pilot Report (Dành cho CFO):** Case study thực tế đo đếm được: *"Sản phẩm đã giúp giảm thời gian chờ đợi từ lúc open PR đến lúc Merge từ 14 tiếng xuống chỉ còn 2.5 tiếng"*.

---

## 📁 Danh sách File Nộp bài (Deliverables)

Dự án này bao gồm 2 tài sản cốt lõi nộp cho Giảng viên (đã được dọn dẹp và làm chuẩn hóa theo đúng cấu trúc yêu cầu):

1. 📄 **`DoTuAnh_Day25_onepager.docx`**: Bản Monetization One-Pager gói gọn toàn bộ bức tranh tài chính, GTM và bằng chứng của dự án (Lưu ý: Hệ thống chấp nhận cả PDF và DOCX. File này được định dạng chuẩn Times New Roman cỡ 13).
2. 📊 **`DoTuAnh_Day25_model.xlsx`**: File Excel chứa mô hình tính toán chi tiết 5 sheet (Cost/Job, Margin, Value Metric, GTM, 90-Day Plan) với đầy đủ công thức tham chiếu nội bộ, được tô màu định dạng rõ ràng các thông số sinh tử.

> *"Chạy được là bài toán kỹ thuật. Bán được là bài toán sinh tồn."*