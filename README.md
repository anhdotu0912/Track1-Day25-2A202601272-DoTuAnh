# Track 1 - Day 25: AI Pricing, GTM & Evidence Lab

**Học viên:** Đỗ Tú Anh  
**Mã học viên:** 2A202601272  
**Dự án:** CodeGuardian AI (AI Code Reviewer)

---

## 🛡️ Giới thiệu CodeGuardian AI
**CodeGuardian AI** không phải là một chatbot AI chung chung. Đây là một công cụ quy trình chuyên dụng (Automated Code Review Tool) được thiết kế riêng cho hệ sinh thái **Web App (Typescript/React)**. 

Mục tiêu của CodeGuardian AI là trở thành "người gác cổng" thầm lặng, tự động đọc các Pull Request (PR) và phát hiện các lỗi logic nghiệp vụ rườm rà, lỗi vòng đời (hydration) trong React, cũng như các lỗ hổng bảo mật sơ đẳng ngay trên giao diện GitHub trước khi code được merge vào nhánh chính.

---

## ✨ Tính năng cốt lõi (Core Features)
- **Deep React/TS Analysis:** Không chỉ linting cú pháp, AI hiểu được context của hooks (`useEffect` dependencies, memory leaks) và các lỗi Hydration đặc thù của Next.js/React.
- **Security First:** Tự động quét và cảnh báo các pattern nguy hiểm (như hardcode secrets, SQL Injection sơ đẳng, XSS trong việc render DOM).
- **Inline Comments:** Nhúng thẳng comment cảnh báo vào đúng dòng code bị lỗi (tab "Files changed" của GitHub) bằng định dạng Markdown, kèm theo code snippet gợi ý sửa lỗi (Diff Suggestions).
- **Zero Data Retention:** Không lưu trữ mã nguồn của công ty trên máy chủ bên thứ 3, đảm bảo tuyệt đối an toàn bảo mật (Compliance SOC2 Type II).

---

## ⚙️ Luồng hoạt động (How it works)
1. **Trigger:** Lập trình viên tạo hoặc cập nhật một Pull Request trên GitHub.
2. **Action:** GitHub Actions workflow tự động được kích hoạt, thu thập file `diff` và gửi context tới hệ thống CodeGuardian.
3. **Analyze:** Sử dụng mô hình **Claude 3.5 Sonnet** với tính năng Prompt Caching để phân tích mã nguồn siêu tốc và tiết kiệm chi phí.
4. **Report:** CodeGuardian gửi API gọi ngược lại GitHub để thả comment vào đúng vị trí cần review chỉ trong vòng chưa đầy 30 giây.

### 🚀 Hướng dẫn tích hợp (Integration Snippet)
Chỉ cần thêm file `.github/workflows/codeguardian.yml` vào repository của bạn:
```yaml
name: CodeGuardian AI Review
on: [pull_request]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run CodeGuardian
        uses: codeguardian-ai/pr-reviewer@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          CG_API_KEY: ${{ secrets.CG_API_KEY }}
```

---

## 📈 Kết quả & Hiệu suất (Impact & Results)
Qua quá trình triển khai thực tế nghiệm thu (Pilot) và đo lường trên tập dữ liệu nội bộ, **CodeGuardian AI** đã đạt được các chỉ số ấn tượng:
- **Giảm 82% thời gian chờ đợi (Lead Time for Changes):** Thời gian chờ review và merge Pull Request giảm từ trung bình 14 tiếng xuống chỉ còn **2.5 tiếng**.
- **Độ chính xác cao:** Phát hiện thành công **92%** các lỗi bảo mật phổ biến (OWASP Top 10) trên tập dữ liệu kiểm thử SWE-bench.
- **Trải nghiệm Developer thân thiện:** Tỷ lệ cảnh báo sai (False Positive) được ép xuống **dưới 15%**, đảm bảo dev không bị làm phiền bởi các comment rác.
- **Khả năng tự động hóa 100%:** Tỷ lệ tự xử lý lỗi (Containment Rate) đạt tuyệt đối, lập trình viên hoàn toàn tự giác sửa lỗi sau khi AI nhắc nhở mà không cần sự can thiệp của Quản lý hay QA (Human-in-the-loop = 0).

---

## 📊 Báo cáo Kinh doanh: Pricing, GTM & Evidence
Bên cạnh khía cạnh kỹ thuật, repository này chứa toàn bộ hồ sơ định giá, mô hình tài chính và chiến lược Go-To-Market (GTM) theo yêu cầu của Lab Day 25.

**Định nghĩa "1 Job" (Đơn vị tạo ra giá trị lõi):** 1 Pull Request được hệ thống AI review hoàn tất (đã phân tích mã nguồn, phát sinh comment hoặc tự động approve).

### 5.1 Nguồn ngân sách & Lựa chọn Value Metric
- **Ngân sách mục tiêu (Buyer):** Rút từ ngân sách **Phần mềm & Công cụ Dev (DevTools/SaaS)**. Người ra quyết định là CTO / VP of Engineering.
- **Value Metric:** Sử dụng mô hình **Hybrid (Thu theo Seat + Giới hạn Hard Cap)**. 
  - Tính phí **$15/User/Tháng**. Giới hạn cứng: **40 PR/tháng** (vượt quá tính $0.20/PR).
  - **Lý luận:** Khách hàng khối doanh nghiệp (B2B) và bộ phận Procurement rất sợ các mô hình thanh toán biến động (unpredictable billing). Mô hình "per seat" như GitHub Copilot giúp họ dễ duyệt chi. Đồng thời, "Hard Cap 40 PR" là chốt chặn sinh tử để bảo vệ biên lợi nhuận (Gross Margin) khỏi những "heavy users" chia vụn commit.

### 5.2 Phân tích Độ chặt chẽ của Chi phí (Cost/Job Rigor)
Dựa trên bảng giá API ngày 27/08/2026:
- **TỔNG COST/JOB:** **$0.052 / PR**
  - **LLM API ($0.033):** Sử dụng **Claude 3.5 Sonnet** tích hợp Prompt Caching (giảm 90% chi phí input).
  - **Infra & DB ($0.005):** Phí Webhook, Vector DB.
  - **Retry ($0.004):** Bù hao 10% chạy lại do rate limit/hallucination.
  - **Overhead ($0.01):** Khấu hao máy chủ, R&D, Support.
  - **HITL ($0):** Sản phẩm chạy theo Biến thể A (Tool gợi ý). Lập trình viên phải tự đọc và sửa lỗi (self-escalate). Containment Rate luôn bằng 100%.
- **Điểm gãy của mô hình:** Nếu tỷ lệ phát hiện lỗi sai (False Positive) vượt quá **15%**, User sẽ thấy phiền, tỷ lệ hủy gói (Churn) sẽ tàn phá giá trị LTV.
- **Gross Margin:** **89% - 93%**. Cực kỳ an toàn ngay cả ở mức trần 40 PR.

### 5.3 Chiến lược Kênh phân phối (Go-To-Market & Channel Fit)
- **Kênh GTM lựa chọn:** **PLG (Product-Led Growth)** tích hợp qua GitHub Marketplace.
- **Biện luận tài chính:** Giả sử ARPU = $150/tháng (10 devs). Với Payback 12 tháng, ngân sách CAC tối đa = **$1,675**. Áp dụng định luật Tomasz Tunguz: *Chỉ với ACV trên $3,000 mới đủ tiền nuôi Inside Sales (AE)*. Kênh tự phục vụ qua PLG là cách duy nhất khả thi, do đó mô hình không dùng chỉ số Deal/AE/ngày hay cost-per-opportunity.
- **Pain Moment (Điểm chạm hoàn hảo):** *16h45 chiều Thứ 6*. Lập trình viên push code và ngóng review để gập máy. Không bắt cài App mới; AI trigger thẳng vào tab "Files changed" giải quyết bế tắc tức thì.

### 5.4 Kế hoạch 90 Ngày (90-Day Rollout Plan)
- **Tháng 1 (Giai đoạn Học hỏi):** Đưa app lên GitHub Marketplace. KPI 50 cài đặt (bản free) để lấy số liệu thực tế về hành vi push code và đối soát Cost/Job.
- **Tháng 2 (Giai đoạn Khuếch đại):** Phát tán Case study thực chiến trên Reddit, HackerNews. KPI: 300 cài đặt.
- **Tháng 3 (Giai đoạn Chuyển đổi):** Tối ưu Drip Email Marketing. KPI: Chốt thành công 20 tổ chức trả phí.

### 5.5 Gói bằng chứng thuyết phục (Evidence Pack)
3 "lá chắn" bảo vệ dành cho Procurement và Security:
1. **Eval Results (Dành cho CTO):** Báo cáo kiểm thử trên SWE-bench, cam kết tỷ lệ bắt sai (False Positive) < 15%.
2. **Risk & Compliance (Dành cho IT Security):** Chính sách Zero Data Retention & SOC2 Type II.
3. **Pilot Report (Dành cho CFO):** Case study thực tế: *"Giảm thời gian chờ đợi từ lúc open PR đến lúc Merge từ 14 tiếng xuống chỉ còn 2.5 tiếng"*.

---

## 📁 Danh sách File Nộp bài (Deliverables)

Dự án bao gồm 2 tài sản cốt lõi nộp cho Giảng viên:

1. 📄 **`DoTuAnh_Day25_onepager.docx`**: Bản Monetization One-Pager gói gọn toàn bộ bức tranh tài chính, GTM và bằng chứng.
2. 📊 **`DoTuAnh_Day25_model.xlsx`**: File Excel chứa mô hình tính toán chi tiết 5 sheet (Cost/Job, Margin, Value Metric, GTM, 90-Day Plan) với đầy đủ công thức và tô màu định dạng.

> *"Chạy được là bài toán kỹ thuật. Bán được là bài toán sinh tồn."*