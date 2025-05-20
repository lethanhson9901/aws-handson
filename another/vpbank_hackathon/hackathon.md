**VPBANK TECHNOLOGY HACKATHON 2025**
**Chinh phục công nghệ, Kiến tạo tương lai cùng VPBank**

---

**TỔNG QUAN VỀ TUYÊN BỐ THỬ THÁCH 2025 (OVERVIEW OF CHALLENGE STATEMENT 2025)**

Tổng cộng có **26 Tuyên bố Thử thách** thuộc hai khối chính: IT (Công nghệ Thông tin) và EDA (Enterprise Data & Analytics - Dữ liệu & Phân tích Doanh nghiệp).

*   **Khối IT (21 Thử thách):**
    *   Trung tâm Ứng dụng Core Banking (IT Core Banking Applications Center): 4 thử thách
    *   Hỗ trợ Dịch vụ Doanh nghiệp CNTT (IT Enterprise Services Support): 7 thử thách
    *   Trung tâm Dịch vụ Vận hành CNTT (IT Services for Operations Center): 5 thử thách
    *   Phòng Bảo mật CNTT (IT Security Department): 1 thử thách
    *   Phòng Chiến lược CNTT & Kiến trúc Doanh nghiệp (IT Strategy & Enterprise Architect Department): 2 thử thách
    *   Phòng Nền tảng Công nghệ (Technology Platform Department): 2 thử thách
*   **Khối EDA (05 Thử thách):**
    *   Trung tâm Nền tảng Dữ liệu (Data Platform Center): 1 thử thách
    *   Trung tâm Kinh doanh Thông minh (Business Intelligence Center): 1 thử thách
    *   Trung tâm Trí tuệ Nhân tạo (Artificial Intelligence Center): 1 thử thách
    *   Phòng Quản trị Dữ liệu (Data Governance Department): 2 thử thách

*(Lưu ý: Thông tin chi tiết về người sở hữu và người hướng dẫn cho từng thử thách được liệt kê trong các slide "CHALLENGE STATEMENT OWNER" từ trang 3-5 của tài liệu gốc, nhưng theo yêu cầu, phần này sẽ tập trung vào nội dung chi tiết của từng thử thách từ trang 6 trở đi.)*

---

## Thử thách #1: BỘ PHÁT HIỆN BẤT THƯỜNG TRONG METRIC KẾT HỢP AI PHÂN TÍCH NGUYÊN NHÂN GỐC (METRIC ANOMALY DETECTOR WITH ROOT CAUSE AI)

### 1. Tổng quan Thử thách

Việc giám sát các chỉ số (metric) như mức sử dụng CPU, bộ nhớ, mã phản hồi (response codes) và số lượng yêu cầu (request counts) là cực kỳ quan trọng trong DevSecOps. Tuy nhiên, việc xác định các điểm bất thường và tìm ra nguyên nhân gốc rễ có thể rất tốn thời gian. Thử thách này tập trung vào việc xây dựng một hệ thống phát hiện bất thường thông minh, không chỉ cảnh báo các vấn đề mà còn giải thích lý do tại sao chúng có thể đã xảy ra bằng cách sử dụng AI.

### 2. Yêu cầu Kỹ thuật

*   **Ngôn ngữ Lập trình:** Python (ưu tiên), hoặc bất kỳ ngôn ngữ nào phù hợp cho xử lý dữ liệu.
*   **Công cụ Dữ liệu:** Pandas, Matplotlib, v.v.
*   **Tích hợp AI:** Bất kỳ API Mô hình Ngôn ngữ Lớn (LLM) nào (ví dụ: OpenAI, Claude).
*   **Nguồn dữ liệu:** Nhật ký (logs) các chỉ số được mô phỏng hoặc thực tế (định dạng CSV, JSON, Prometheus).

### 3. Đánh giá và Đo lường

*   **Độ chính xác Phát hiện Bất thường (Anomaly Detection Accuracy) - 30%:**
    *   Phát hiện chính xác các điểm bất thường trong dữ liệu metric.
    *   Sử dụng một phương pháp xác định (ví dụ: z-score, ngưỡng).
*   **Giải thích Nguyên nhân Gốc rễ bằng AI (AI Root Cause Explanations) - 25%:**
    *   Đưa ra các giả thuyết hữu ích kèm theo ngữ cảnh.
    *   Dựa trên các mẫu hoặc nhật ký được căn chỉnh theo thời gian.
*   **Trực quan hóa & Rõ ràng Kết quả (Visualization & Result Clarity) - 15%:**
    *   Biểu đồ hiển thị rõ ràng các xu hướng và điểm bất thường.
    *   Chú giải/nhãn (legends/labels) giúp diễn giải dữ liệu dễ dàng.
*   **Cấu trúc Code & Khả năng Tái sử dụng (Code Structure & Reusability) - 15%:**
    *   Code được module hóa với sự tách biệt logic rõ ràng.
    *   Phù hợp để sử dụng trong notebook hoặc script.
*   **Tài liệu Hướng dẫn (Documentation - README.md) - 10%:**
    *   Giải thích phương pháp phát hiện bất thường, cách sử dụng, cài đặt.
    *   Bao gồm các ví dụ đầu vào/đầu ra.
*   **Mô phỏng Cảnh báo CI/CD (CI/CD Alert Simulation) - 10%**

### 4. Sản phẩm Bàn giao (Deliverables)

*   Mô tả phát triển Script hoặc Notebook phân tích Metric.
*   Giải thích logic Phát hiện Bất thường.
*   Giải thích Phân tích Nguyên nhân Gốc rễ thông qua Module AI.
*   Các tệp Đầu vào và Đầu ra Mẫu.
*   Tài liệu về Triển khai (Deploy).
*   Trình bày Mô phỏng Bảng điều khiển Giám sát (Monitoring Dashboard).
*   Trình bày Mô phỏng Kích hoạt CI/CD (CI/CD Trigger).

---

## Thử thách #2: BỘ PHÂN TÍCH TRÔI DẠT IAC DỰA TRÊN LLM (LLM-BASED IAC DRIFT ANALYZER)

### 1. Tổng quan Thử thách

Trôi dạt cơ sở hạ tầng (Infrastructure drift) xảy ra khi cơ sở hạ tầng đã triển khai không còn khớp với phiên bản được định nghĩa trong Infrastructure-as-Code (IaC). Điều này có thể dẫn đến hành vi không mong muốn, pipeline thất bại hoặc rủi ro bảo mật. Mục tiêu là xây dựng một công cụ giúp các nhóm tự động xác định và hiểu rõ về trôi dạt cơ sở hạ tầng, sử dụng AI để giải thích điều gì đã xảy ra và tại sao.

### 2. Yêu cầu Kỹ thuật

*   **Ngôn ngữ Lập trình:** Python, Bash, hoặc ngôn ngữ kịch bản tương tự.
*   **Công cụ Cơ sở hạ tầng (Infrastructure Tooling):** Terraform, Terragrunt hoặc bất kỳ ngôn ngữ kịch bản IaC nào (thậm chí Pulumi hoặc Crossplane).
*   **Tích hợp AI:** Bất kỳ API LLM nào.
*   **(Tùy chọn) Kiến thức về kịch bản CI/CD hoặc tự động hóa.**

### 3. Đánh giá và Đo lường

*   **Tổng: 100%**
*   **Độ chính xác Phát hiện Trôi dạt (Drift Detection Accuracy) - 40%:**
    *   Xác định chính xác các tài nguyên bị trôi dạt (mới, đã xóa, đã thay đổi).
    *   Phân tích và diễn giải đầu ra `terraform show -json` một cách đáng tin cậy.
    *   Bao quát nhiều tình huống trôi dạt, bao gồm cả các trường hợp biên (edge cases).
*   **Giải thích bằng AI (AI-Powered Explanations) - 20%:**
    *   Sử dụng mô hình ngôn ngữ để tạo ra các mô tả bằng ngôn ngữ tự nhiên.
    *   Cung cấp nguyên nhân gốc rễ liên quan và các khuyến nghị có thể hành động.
*   **Chất lượng Kịch bản Mẫu (Sample Scenarios Quality) - 15%:**
    *   Ít nhất hai ví dụ đầu vào và đầu ra được xây dựng tốt.
*   **Chất lượng Code và Tính Module hóa (Code Quality and Modularity) - 15%:**
    *   Code sạch sẽ, module hóa và được chú thích tốt.
    *   Dễ dàng chạy, mở rộng hoặc tích hợp vào quy trình làm việc.
*   **Tài liệu Hướng dẫn (Documentation - README.md) - 10%:**
    *   Hướng dẫn cài đặt và sử dụng cơ bản.
    *   Giải thích rõ ràng logic phân loại.
*   **Ví dụ Tích hợp CI/CD (CI/CD Integration Example) - 10%:**
    *   GitHub Action hoặc script shell cho Gitlab runner để tích hợp pipeline.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Script Phát hiện Trôi dạt có thể thực thi trong mã nguồn.
*   Trình bày Hệ thống Phân loại Trôi dạt.
*   Mô tả Module Giải thích bằng AI.
*   Các tệp Đầu vào và Đầu ra Mẫu.
*   Tài liệu Hướng dẫn (README.md).
*   Giải thích Ví dụ Tích hợp CI/CD.

---

## Thử thách #3: KHUNG KIỂM TOÁN TỰ ĐỘNG BẰNG AI (AUTO AUDIT FRAMEWORK BY AI)

### 1. Tổng quan Thử thách

Ngân hàng phải tuân thủ các tiêu chuẩn bảo mật nổi tiếng như PCI DSS, ISO27001 và Luật An ninh mạng Việt Nam... Việc kiểm toán và đảm bảo hệ thống CNTT tuân thủ các yêu cầu bảo mật của tiêu chuẩn và luật pháp tốn rất nhiều thời gian và công sức. Do đó, ngân hàng muốn xây dựng một khung kiểm toán tự động bằng cách tận dụng sức mạnh của GenAI để tăng tốc độ kiểm tra và xác minh bảo mật.

*   **Mục tiêu cần kiểm toán:** Hàng trăm tài khoản AWS (hiện tại VPBank có ~200 tài khoản).
*   **Lớp kiểm toán:** Cấu hình cơ sở hạ tầng AWS.
*   **Tiêu chuẩn bảo mật:** PCI DSS.

### 2. Yêu cầu Kỹ thuật

Sản phẩm thử nghiệm (prototype) nên có:
*   **Ngôn ngữ Lập trình:** Bất kỳ ngôn ngữ lập trình nào.
*   **Dịch vụ Đám mây:** AWS.
*   **Mô hình AI:** Bất kỳ LLM nào.
*   **Tự động hóa Quy trình (Workflow automation):** Bất kỳ công cụ quy trình nào (khuyến nghị sử dụng n8n).
*   **Cơ sở dữ liệu (DB):** Bất kỳ DB nào (khuyến nghị sử dụng Postgresql).
*   **Khác:** Docker, Kubernetes hoặc ứng dụng dựa trên máy chủ (hosted-base app).

### 3. Đánh giá và Đo lường

*   **Tự động hóa quy trình kiểm toán và báo cáo:** 100%.
*   **Độ chính xác kiểm toán:** > 99,9%.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Bài trình bày (Presentation).
*   Bản thử nghiệm Demo (Prototype Demo).

---

## Thử thách #4: TÁI THIẾT KẾ QUY TRÌNH NGÂN HÀNG BẰNG CÔNG NGHỆ AI (AI-POWERED BANKING PROCESS REDESIGN)

### 1. Tổng quan Thử thách

Các hoạt động ngân hàng thường liên quan đến các quy trình làm việc phức tạp, nhiều bước, có thể không hiệu quả, khó hình dung và khó tối ưu hóa. Việc phân tích sơ đồ quy trình, xác định các điểm nghẽn và thiết kế lại quy trình làm việc theo truyền thống đòi hỏi nỗ lực thủ công đáng kể và chuyên môn cao. Thử thách này nhằm mục đích tận dụng sức mạnh của Trí tuệ Nhân tạo Tạo sinh (Generative AI - GenAI) để tăng cường và hợp lý hóa đáng kể việc phân tích và thiết kế lại các quy trình hoạt động ngân hàng.

Mục tiêu là phát triển một sản phẩm thử nghiệm (prototype) sử dụng GenAI để giúp người dùng hiểu, hình dung, phân tích và có khả năng tối ưu hóa các quy trình kinh doanh thông qua các tương tác trực quan, chẳng hạn như gợi ý bằng ngôn ngữ tự nhiên hoặc trợ lý đàm thoại.

### 2. Yêu cầu Kỹ thuật

Sản phẩm thử nghiệm nên có:
*   **Ngôn ngữ Lập trình:** Python, Nodejs, Typescript...
*   **Dịch vụ Đám mây:** AWS.
*   **Mô hình AI:** Bất kỳ LLM nào (OpenAI, Gemini, Claude...).
*   **Khác:** Docker, Kubernetes.

### 3. Đánh giá và Đo lường

*   **Xử lý Mô tả Quy trình (Process Description Handling):** Khả năng nhận và hiểu mô tả quy trình thông qua ngôn ngữ tự nhiên hoặc đầu vào có cấu trúc.
*   **Tạo/Trực quan hóa Quy trình (Workflow Generation/Visualization):** Tự động tạo ra một biểu diễn (văn bản có cấu trúc hoặc hình ảnh cơ bản) của quy trình được mô tả.
*   **Phân tích Quy trình bằng AI (AI-driven Process Analysis):** Sử dụng AI để trả lời các câu hỏi cơ bản về quy trình (ví dụ: các bước chính, các bên liên quan được đề cập).
*   **Đề xuất Cải tiến bằng AI (AI-driven Improvement Suggestion):** Sử dụng AI để cung cấp ít nhất một đề xuất cải tiến liên quan hoặc xác định một vấn đề tiềm ẩn trong quy trình được mô tả.
*   **Mức độ liên quan của Đầu ra AI (Relevance of AI Output):** Kết quả do AI tạo ra (sơ đồ, câu trả lời, đề xuất) phải liên quan và logic dựa trên đầu vào.
*   **Demo Chức năng (Functional Demo):** Sản phẩm thử nghiệm hoạt động ổn định và thể hiện các tính năng cốt lõi trong quá trình demo.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Phát triển giải pháp sản phẩm thử nghiệm hoạt động.
*   Thiết kế/biểu diễn quy trình làm việc được tạo ra (hình ảnh hoặc cấu trúc) bằng cách sử dụng gợi ý (prompts).
*   Giao diện cho tương tác demo.

---

## Thử thách #5: CHẤM ĐIỂM TÍN DỤNG NÂNG CAO BẰNG AI/ML (AI/ML ENHANCED CREDIT SCORING)

### 1. Tổng quan Thử thách

Các hệ thống chấm điểm tín dụng truyền thống phụ thuộc nhiều vào dữ liệu tài chính có cấu trúc, thường bỏ qua các nguồn dữ liệu thay thế có thể phản ánh tốt hơn khả năng tín dụng của khách hàng mới (new-to-bank) hoặc khách hàng có ít thông tin tín dụng (thin-file). Thử thách này mời gọi các đội tham gia phát triển một sản phẩm thử nghiệm chấm điểm tín dụng nâng cao bằng AI, tận dụng các mô hình Trí tuệ Nhân tạo Tạo sinh (Generative AI) và/hoặc Học máy (ML) để cải thiện độ chính xác, tính minh bạch và tính toàn diện của việc chấm điểm.

Hệ thống nên mô phỏng việc chấm điểm dựa trên cả đầu vào dữ liệu truyền thống và phi truyền thống (ví dụ: hành vi giao dịch, dữ liệu xã hội, hóa đơn tiện ích, hoạt động thương mại điện tử) và cung cấp các kết quả đầu ra có thể diễn giải được cho nhân viên kinh doanh hoặc nhân viên quản lý rủi ro tín dụng.

### 2. Yêu cầu Kỹ thuật

Sản phẩm thử nghiệm nên có:
*   **Ngôn ngữ Lập trình:** Python, Node.js, TypeScript...
*   **Dịch vụ Đám mây:** AWS.
*   **Mô hình AI:** OpenAI, Claude, hoặc bất kỳ mô hình dựa trên ML/LLM nào (ví dụ: XGBoost, LightGBM, LangChain...).
*   **Khác:** Docker, Kubernetes.

### 3. Đánh giá và Đo lường

*   **Xử lý Dữ liệu (Data Handling):** Khả năng nhận dữ liệu đầu vào giả lập có cấu trúc và phi cấu trúc (ví dụ: thu nhập, giao dịch, việc sử dụng tiện ích).
*   **Logic Chấm điểm (Scoring Logic):** Áp dụng mô hình AI/ML để tạo ra điểm tín dụng hoặc phân loại rủi ro.
*   **Khả năng Giải thích (Explainability):** Đưa ra một giải thích đơn giản (bằng ngôn ngữ tự nhiên hoặc biểu đồ) cho thấy lý do tại sao điểm số được ấn định.
*   **Khả năng Thích ứng (Adaptability):** Hỗ trợ chấm điểm cho các loại khách hàng khác nhau (ví dụ: người làm công ăn lương, người làm nghề tự do...).
*   **Mức độ liên quan của Đầu ra (Relevance of Output):** Đầu ra phải logic, có thể giải thích được và phản ánh các đặc điểm đầu vào.
*   **Demo Chức năng (Functional Demo):** Sản phẩm thử nghiệm phải chạy ổn định và mô phỏng việc chấm điểm từ đầu đến cuối trên các trường hợp mẫu.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Sản phẩm thử nghiệm hoạt động của hệ thống chấm điểm tín dụng.
*   Các mẫu dữ liệu đầu vào giả lập và điểm số do AI tạo ra.
*   Giao diện hoặc báo cáo đầu ra có thể giải thích được.
*   Luồng demo cho thấy các loại khách hàng và kết quả chấm điểm.

---

## Thử thách #6: DỊCH VỤ THANH TOÁN MODULE HÓA VỚI PHÂN TÍCH KHÁCH HÀNG VÀ THỊ TRƯỜNG BẰNG AI (CÓ AGENT HỖ TRỢ) (MODULAR PAYMENT SERVICE WITH AI-POWERED CUSTOMER INSIGHTS AND MARKET ANALYTICS (AGENT-ENABLED))

### 1. Tổng quan Thử thách

VPBank đang phát triển một Dịch vụ Thanh toán hiện đại để xử lý các giao dịch theo thời gian thực và tăng cường sự tương tác của khách hàng thông qua các dịch vụ thông minh.

Thử thách của bạn là thiết kế và triển khai một Dịch vụ Thanh toán Module hóa có khả năng:
*   Xử lý và định tuyến các giao dịch (ví dụ: IBFT, QR, ví điện tử - eWallet).
*   Cung cấp báo cáo hành vi thị trường cho mục đích sử dụng nội bộ.
*   Tạo ra các phân tích chuyên sâu được cá nhân hóa cho khách hàng dựa trên lịch sử giao dịch của họ bằng cách sử dụng một Tác nhân AI (AI Agent).

Hệ thống này phải có tính module hóa và khả năng mở rộng, thể hiện cách AI có thể biến đổi ngân hàng giao dịch thành một trải nghiệm khách hàng thông minh hơn.

### 2. Yêu cầu Kỹ thuật

*   **Lập trình:** Python,...
*   **Logic định tuyến giao dịch.**
*   **Thiết kế và phát triển API (REST/Async).**
*   **Kiến thức cơ bản về AI/ML (mô hình hóa + đánh giá).**
*   **Docker/containerization.**
*   **Trực quan hóa (BI/dashboard).**
*   **Tùy chọn:** Thiết kế gợi ý (Prompt design) và triển khai tác nhân AI.

### 3. Đánh giá và Đo lường

*   **Tính đúng đắn về chức năng (xác thực, định tuyến, phản hồi).**
*   **Tính module hóa và sự tách biệt rõ ràng các mối quan tâm (separation of concerns).**
*   **Khả năng phục hồi lỗi (mô phỏng thời gian chờ, thiếu trường dữ liệu).**
*   **Tài liệu tốt (README, Swagger/OpenAPI spec).**
*   **Thưởng (Bonus):** Sử dụng các công cụ AI một cách có suy nghĩ trong việc phân tích khách hàng.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Ứng dụng demo (có thể chạy cục bộ hoặc triển khai trên AWS).
*   Bài thuyết trình slide (5-7 phút).
*   **Tùy chọn:** Sơ đồ kiến trúc.
*   **Thưởng (Bonus):** Triển khai AWS với các dịch vụ Free Tier.

---

## Thử thách #7: HỆ THỐNG PHÁT HIỆN VÀ CẢNH BÁO BẤT THƯỜNG GIAO DỊCH BẰNG AI (AI-POWERED TRANSACTION ANOMALY DETECTION & ALERT SYSTEM)

### 1. Tổng quan Thử thách

VPBank hiện đang xử lý hàng triệu giao dịch hàng ngày: chuyển khoản liên ngân hàng (IBFT), thanh toán QR, nạp tiền vào ví, v.v. Với khối lượng này, việc xác định các mẫu bất thường là chìa khóa để ngăn chặn gian lận và đảm bảo trải nghiệm khách hàng liền mạch.

Thử thách là xây dựng một hệ thống thông minh có khả năng:
*   Tiếp nhận dữ liệu giao dịch (thời gian thực hoặc mô phỏng).
*   Phát hiện các mẫu đáng ngờ bằng AI và các quy tắc nghiệp vụ.
*   Tự động gửi cảnh báo đến các nhóm nội bộ.

Hệ thống phải có khả năng mở rộng, hoạt động trên nền tảng đám mây (ưu tiên AWS) và có thể mở rộng cho các loại giao dịch khác nhau.

### 2. Yêu cầu Kỹ thuật

*   **Lập trình:** Python...
*   **Kiến thức cơ bản về AI/ML (mô hình hóa + đánh giá).**
*   **Phát triển API.**
*   **Điện toán đám mây (ưu tiên AWS - khuyến khích sử dụng dịch vụ AWS Free Tier).**
*   **Trực quan hóa (BI/dashboard).**
*   **Cảnh báo và tự động hóa dựa trên sự kiện.**

### 3. Đánh giá và Đo lường

*   Phát hiện bất thường một cách có thể mở rộng.
*   Phản hồi theo thời gian thực với các cảnh báo có thể hành động.
*   Hoạt động trên nhiều loại giao dịch.
*   Có thể triển khai trên đám mây, ưu tiên trên AWS.
*   Bao gồm sơ đồ hệ thống và demo được trình bày rõ ràng.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Ứng dụng demo (có thể chạy cục bộ hoặc triển khai trên AWS).
*   Bài thuyết trình slide (5-7 phút).
*   **Tùy chọn:** Sơ đồ kiến trúc đám mây.
*   **Thưởng (Bonus):** Triển khai AWS với các dịch vụ Free Tier.

---

## Thử thách #8: PIPELINE CI/CD TỰ ĐỘNG HÓA VIỆC VÁ LỖ HỔNG BẢO MẬT WINDOWS TỪ BÁO CÁO LỖ HỔNG (CI/CD PIPELINE FOR AUTOMATING WINDOWS SECURITY PATCHING FROM VULNERABILITY REPORT)

### 1. Tổng quan Thử thách

Hàng quý, đội ngũ Bảo mật cung cấp một Báo cáo Lỗ hổng được tạo ra từ các lần quét tự động. Báo cáo liệt kê các CVE (Lỗ hổng và Phơi nhiễm Chung), mức độ nghiêm trọng, các gói HĐH bị ảnh hưởng và yêu cầu vá lỗi - chủ yếu cho các hệ thống dựa trên Windows Server.

Thử thách là xây dựng một pipeline CI/CD tự động có khả năng:
*   Tiếp nhận các tệp báo cáo lỗ hổng (định dạng CSV/Excel).
*   Phân tích các CVE ở mức độ cao/nghiêm trọng cho Windows.
*   Tự động kích hoạt cập nhật bản vá Windows thông qua kịch bản (scripting).
*   Ghi nhật ký/báo cáo lại thành công hoặc thất bại cho mỗi bản vá.

Pipeline này sẽ giảm thời gian khắc phục, thực thi chính sách vá lỗi nhất quán và phù hợp với các kỳ vọng về tuân thủ.

### 2. Yêu cầu Kỹ thuật

*   **Lập trình.**
*   **Xác định CVE theo mức độ ưu tiên (lọc Nghiêm trọng/Cao).**
*   **Kịch bản vá lỗi Windows thông qua PowerShell.**
*   **GitOps/Công cụ CI: GitLab CI, Jenkins...**
*   **Tùy chọn:**
    *   Tạo công việc YAML cho pipeline vá lỗi.
    *   Bảng điều khiển cho kết quả vá lỗi.

### 3. Đánh giá và Đo lường

*   Khả năng tự động tiếp nhận báo cáo lỗ hổng Excel.
*   Lọc và ưu tiên CVE (Nghiêm trọng/Cao).
*   Kích hoạt thành công việc vá lỗi Windows dựa trên PowerShell.
*   Ghi nhật ký các CVE đã vá so với các CVE thất bại.
*   **Thưởng (Bonus):** Thử lại các bản vá lỗi thất bại & Email/báo cáo tóm tắt sau công việc vá lỗi.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Pipeline CI/CD YAML / Jenkinsfile.
*   Các kịch bản PowerShell. Tệp Excel mẫu về lỗ hổng.
*   Tệp README để mô phỏng quá trình chạy đầy đủ.
*   Bài thuyết trình slide (5-7 phút).

---

## Thử thách #9: HUẤN LUYỆN VIÊN TÀI CHÍNH AI – QUẢN LÝ TIỀN BẠC THEO 6 CHIẾC HŨ (AI FINANCIAL COACH – 6-JAR MONEY MANAGEMENT)

### 1. Tổng quan Thử thách

Hầu hết người Việt Nam - đặc biệt là thế hệ trẻ - không có kế hoạch tài chính cá nhân rõ ràng. Thói quen chi tiêu kém và thiếu mục tiêu tài chính dài hạn là một trong những lý do chính khiến nhiều người phải vật lộn để đạt được sự ổn định tài chính.
Trong bối cảnh đó, VPBank đang nỗ lực trở thành một ngân hàng số hóa và cá nhân hóa, có ảnh hưởng tích cực đến hành vi tài chính của người Việt Nam. Tham vọng này phù hợp với một trong bốn trụ cột chính của sự Thịnh vượng do VPBank đề xuất: Thịnh vượng Tài chính.

**Mục tiêu Thử thách:**
Xây dựng một sản phẩm thử nghiệm hoạt động cho phép người dùng:
*   Phân bổ thu nhập hàng tháng vào 6 chiếc hũ.
*   Tự động phân loại các giao dịch theo thời gian thực vào chiếc hũ tương ứng.
*   Theo dõi chi tiêu so với giới hạn của hũ và nhận cảnh báo.
*   Nhận huấn luyện bằng AI để điều chỉnh hành vi và khuyến khích thói quen tài chính tốt hơn.

### 2. Yêu cầu Kỹ thuật

Sản phẩm thử nghiệm nên có:
*   **Frontend:** ReactJS / Next.js hoặc Ứng dụng Web Di động.
*   **Backend:** Node.js / Python (Flask, FastAPI), API RESTful.
*   **Cơ sở dữ liệu:** PostgreSQL / DynamoDB (cho phép giả lập), dữ liệu giả lập JSON.
*   **AI/ML:** OpenAI API, LangChain, hoặc bất kỳ LLM nguồn mở nào (Claude, Mistral, Llama2...).
*   **Trường hợp sử dụng AI:** Phân loại NLP, dự đoán chi tiêu, RAG (Retrieval-Augmented Generation - Tạo sinh Tăng cường bằng Truy xuất).
*   **Dịch vụ Đám mây:** AWS.

### 3. Đánh giá và Đo lường

*   **Ý tưởng & Đổi mới (Idea & Innovation):** Tính sáng tạo và mức độ liên quan trong việc thúc đẩy giáo dục tài chính.
*   **Ứng dụng AI (AI Application):** Sử dụng hiệu quả LLM để phân loại, đề xuất và cá nhân hóa.
*   **Chức năng Tính năng (Feature Functionality):** Sản phẩm thử nghiệm từ đầu đến cuối: tạo hũ, phân loại chi tiêu, cung cấp huấn luyện.
*   **Khả năng Mở rộng Tác động Kinh doanh cho VPBank (Business Impact Scalability for VPBank):** Tăng trưởng CASA, bán chéo, độ gắn kết của sản phẩm.
*   **Trình bày & Demo (Presentation & Demo):** Sự rõ ràng, cách kể chuyện, demo trực tiếp và lộ trình sản phẩm.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Sản phẩm thử nghiệm ứng dụng Web hoặc web di động với các tính năng cốt lõi: 6 hũ, công cụ phân loại, đề xuất AI.
*   Mã nguồn: Kho lưu trữ (GitHub/GitLab) với tài liệu rõ ràng và hướng dẫn cài đặt.
*   Tổng quan Kỹ thuật Tài liệu: sơ đồ kiến trúc, API, logic AI, phương pháp tích hợp.
*   Bảng điều khiển: Chi tiêu theo hũ, tiến độ theo dõi mục tiêu (nếu có).
*   Slide Thuyết trình: Sự phù hợp giữa vấn đề và giải pháp, kiến trúc, luồng demo, tác động tiềm năng và mở rộng trong tương lai.
*   Video Demo (Tùy chọn): Video hướng dẫn demo 1-3 phút để hỗ trợ ban giám khảo.
*   Hỗ trợ Tùy chọn Có sẵn.

---

## Thử thách #10: XÂY DỰNG CÔNG CỤ TỔNG HỢP VÀ TÍNH TOÁN DỮ LIỆU (BUILDING A DATA CONSOLIDATION AND CALCULATION TOOL)

### 1. Tổng quan Thử thách

Ngân hàng hiện đang đối mặt với những thách thức trong việc tổng hợp dữ liệu từ nhiều nguồn và thực hiện các phép tính chính xác dựa trên dữ liệu này. Mục tiêu của thử thách này là xây dựng một công cụ có khả năng trích xuất và tổng hợp dữ liệu từ hai nguồn: dữ liệu lịch sử từ S3 (T-1) và dữ liệu gần thời gian thực từ luồng Kafka (T-0). Nguồn S3 chứa dữ liệu được phân mảnh theo ngày, với dữ liệu giao dịch của khách hàng trong một năm, khoảng 1 triệu bản ghi mỗi ngày. Công cụ này phải thực hiện các phép tính ở cấp độ khách hàng với thời gian xử lý dưới 1 giây.

Đầu ra của ứng dụng phải là một API hoặc bất kỳ phương thức nào khác mà các hệ thống khác có thể tích hợp để truy xuất dữ liệu cấp độ khách hàng đã được tổng hợp và tính toán. Công cụ này sẽ giúp ngân hàng thực hiện các phép tính khác nhau, bao gồm nhưng không giới hạn ở việc tính phí, một cách chính xác và hiệu quả hơn, đồng thời cải thiện khả năng phân tích dữ liệu và ra quyết định.

### 2. Yêu cầu Kỹ thuật

*   **Ngôn ngữ Lập trình:** Python, Java, .Net Core.
*   **Dịch vụ Đám mây:** AWS.
*   **Công cụ Xử lý Dữ liệu:** Apache Kafka, Apache Spark, AWS Glue.
*   **Containerization và Điều phối (Orchestration):** Docker, Kubernetes.
*   **Quản lý Cơ sở dữ liệu:** AWS RDS, PostgreSQL, Redis, bất kỳ NoSQL DB nào.
*   **Phát triển API:** API RESTful, GraphQL.
*   **Giám sát và Ghi nhật ký:** AWS CloudWatch, ELK Stack (Elasticsearch, Logstash, Kibana).
*   **Bảo mật:** AWS IAM, VPC, KMS.

### 3. Đánh giá và Đo lường

*   **Khả năng Tích hợp Dữ liệu:** Công cụ phải tích hợp hiệu quả dữ liệu từ S3 và Kafka.
*   **Độ chính xác của Phép tính:** Độ chính xác của các phép tính khác nhau phải trên 95%.
*   **Thời gian Xử lý:** Thời gian xử lý và tính toán dữ liệu phải dưới 1 giây.
*   **Khả năng Mở rộng:** Công cụ phải có khả năng mở rộng để xử lý khối lượng lớn dữ liệu.
*   **Hiệu suất API:** API phải phản hồi nhanh và có khả năng xử lý nhiều yêu cầu một cách hiệu quả.
*   **Bảo mật:** Công cụ phải đảm bảo an ninh dữ liệu và tuân thủ các tiêu chuẩn ngành.
*   **Trình bày và Demo:** Các ứng viên phải trình bày và demo công cụ một cách rõ ràng và thuyết phục.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Bài trình bày chi tiết về giải pháp.
*   Demo công cụ tổng hợp và tính toán dữ liệu.
*   Mã nguồn và tài liệu.
*   Hướng dẫn triển khai và cài đặt.
*   Tài liệu API và hướng dẫn sử dụng.

---

## Thử thách #11: KHUNG LÀM VIỆC CHO NHIỀU TÁC NHÂN AI (MULTI-AI AGENTS FRAMEWORK)

### 1. Tổng quan Thử thách

Bối cảnh phát triển AI hiện tại đang nhanh chóng hướng tới sự hợp tác đa tác nhân (multi-agent collaboration), nơi nhiều tác nhân AI làm việc cùng nhau để giải quyết các nhiệm vụ phức tạp. Tuy nhiên, hiện đang thiếu các khung làm việc nhẹ, có khả năng mở rộng, cho phép người dùng – cả kỹ thuật và bán kỹ thuật – dễ dàng tạo, quản lý và điều phối nhiều tác nhân AI với các công cụ và khả năng khác nhau.

Mục tiêu là phát triển một khung quản lý đa tác nhân AI cho phép người dùng:
*   Tạo các tác nhân AI với các gợi ý (prompts) và kỹ năng có thể cấu hình.
*   Tích hợp các công cụ (ví dụ: web scraping, máy tính, API).
*   Quản lý và giám sát hành vi và nhiệm vụ của các tác nhân.
*   Cho phép các tác nhân hợp tác hoặc được gọi như các tác nhân con (sub-agents).

Điều này sẽ giảm chi phí phát triển, khuyến khích thiết kế module hóa và cho phép tạo mẫu nhanh các hệ thống dựa trên tác nhân – một cơ hội để dân chủ hóa quyền truy cập vào việc điều phối AI tiên tiến.

### 2. Yêu cầu Kỹ thuật

Nhân sự lý tưởng nên có kinh nghiệm thực tế trong các lĩnh vực sau:
*   **Ngôn ngữ Lập trình:** Python (cốt lõi), JavaScript/TypeScript (tùy chọn cho UI).
*   **Dịch vụ Đám mây:** AWS (để triển khai).
*   **Mô hình AI:** Tích hợp với các LLM như GPT-4, Claude, hoặc các giải pháp thay thế nguồn mở (ví dụ: LLaMA, Mistral).
*   **Khung làm việc & Công cụ (Frameworks & Tools):**
    *   FastAPI hoặc Flask cho API backend.
    *   Google Agent Development Kit, LangChain hoặc tương tự cho công cụ và chuỗi tác nhân.
    *   Docker cho containerization & Kubernetes.
*   **Khác:** Git, REST API, JSON, WebSockets (để cập nhật tác nhân theo thời gian thực).

### 3. Đánh giá và Đo lường

Sản phẩm thử nghiệm sẽ được đánh giá dựa trên các tiêu chí có thể đo lường sau:
*   **Chức năng (Functionality):**
    *   Khả năng tạo nhiều tác nhân với vai trò và bộ nhớ riêng biệt.
    *   Tích hợp công cụ với ít nhất 2 công cụ hoạt động (ví dụ: máy tính, tìm kiếm web).
    *   Gọi tác nhân-đến-tác nhân (agent-to-agent calling) và gọi công cụ thành công.
*   **Hiệu suất (Performance):**
    *   Độ trễ trung bình khi gọi công cụ: < 5 giây.
    *   Thời gian phản hồi trung bình của tác nhân: < 12 giây.
*   **Trải nghiệm Người dùng (User Experience):**
    *   Giao diện trực quan, đơn giản hoặc tài liệu API để quản lý tác nhân.
    *   Giám sát trực quan hoặc dựa trên nhật ký (log-based) các quy trình làm việc của tác nhân.
*   **Chất lượng Code (Code Quality):**
    *   Kiến trúc sạch sẽ và codebase module hóa.
    *   Tuân theo các thực hành SDLC (yêu cầu, thiết kế, phát triển, kiểm thử, tài liệu).
*   **Đổi mới & Khả năng Mở rộng (Innovation & Scalability):**
    *   Sự chu đáo trong kiến trúc để mở rộng độ phức tạp của tác nhân.
    *   Các trường hợp sử dụng độc đáo hoặc các phần mở rộng vượt ra ngoài các yêu cầu cơ bản.

### 4. Sản phẩm Bàn giao (Deliverables)

Các sản phẩm bàn giao sau:
*   **Demo Sản phẩm thử nghiệm:** Một hệ thống hoạt động thể hiện việc quản lý và công cụ đa tác nhân.
*   **Bộ Slide Thuyết trình:** Tổng quan về kiến trúc, các quyết định chính, những thách thức gặp phải, kế hoạch tương lai và các chức năng để tiếp tục phát triển.
*   **Tài liệu Kỹ thuật:** Hướng dẫn cài đặt, hướng dẫn cấu hình tác nhân/công cụ, cách sử dụng API (nếu có).
*   **Mã nguồn:** Được lưu trữ trên GitHub hoặc nền tảng tương tự hoặc tệp zip, được cấu trúc rõ ràng với README.

---

## Thử thách #12: TÁC NHÂN FACEBOOK (FACEBOOK AGENT)

### 1. Tổng quan Thử thách

VPBank hiện đang vận hành một trang Facebook với hàng triệu người theo dõi để tương tác và hỗ trợ khách hàng. Tuy nhiên, các yêu cầu của khách hàng thường được xử lý thủ công, dẫn đến sự chậm trễ, phản hồi không nhất quán và bỏ lỡ cơ hội cung cấp dịch vụ chủ động.

Mục tiêu là phát triển một tác nhân AI có khả năng:
*   Theo dõi và trích xuất các yêu cầu từ trang Facebook của VPBank.
*   Phân loại và tóm tắt ý định của khách hàng.
*   Nếu điểm tin cậy cao (>90%) và câu trả lời có thể được xác minh từ các nguồn chính thức của VPBank (website, FAQs), tự động phản hồi cho khách hàng.
*   Đối với các câu hỏi có độ tin cậy thấp hoặc trung bình, gắn cờ chúng để xem xét thủ công và ghi chú chúng trong một bảng điều khiển hoặc nhật ký tóm tắt cho quản trị viên trang.

Giải pháp này sẽ tăng cường khả năng phản hồi, giảm khối lượng công việc cho các nhân viên hỗ trợ con người và cải thiện trải nghiệm tổng thể của khách hàng trong khi vẫn duy trì việc xác thực của con người đối với các yêu cầu quan trọng hoặc không rõ ràng.

### 2. Yêu cầu Kỹ thuật

Đội ngũ lý tưởng nên có kinh nghiệm trong các lĩnh vực kỹ thuật sau:
*   **Ngôn ngữ Lập trình:** Python (chính), JavaScript (tùy chọn cho frontend).
*   **Dịch vụ Đám mây:** AWS (để lưu trữ và tích hợp).
*   **Mô hình & Thư viện AI:**
    *   Các Mô hình Ngôn ngữ Lớn (OpenAI GPT-4, Azure OpenAI, hoặc Claude, Gemini, Mistral ...) có thể được sử dụng để phân loại văn bản, tóm tắt.
    *   Logic chấm điểm tin cậy và phân loại ý định.
*   **Công cụ & Tích hợp:**
    *   Facebook Graph API (để trích xuất nội dung trang và bình luận/tin nhắn của khách hàng).
    *   BeautifulSoup/Scrapy hoặc công cụ khác để trích xuất nội dung từ trang web chính thức của VPBank.
    *   LangChain, Google Agent Development Kit hoặc các khung điều phối tương tự.
    *   Streamlit, Flask, hoặc FastAPI (cho bảng điều khiển nội bộ/điểm cuối API).

### 3. Đánh giá và Đo lường

Sản phẩm thử nghiệm sẽ được đánh giá dựa trên các tiêu chí sau:
*   **Chức năng (Functionality):**
    *   Trích xuất tin nhắn/bình luận Facebook: tích hợp hoạt động.
    *   Phân loại các yêu cầu thành độ tin cậy cao/thấp với độ chính xác >90%.
    *   Tỷ lệ tự động phản hồi: Ít nhất 70% yêu cầu nhận được phản hồi tự động chính xác (được xác thực qua bộ thử nghiệm).
    *   Gắn cờ đúng các yêu cầu không chắc chắn với thẻ để xem xét.
    *   Phản hồi bằng ngôn ngữ tự nhiên.
    *   Báo cáo cho phản hồi tự động và phản hồi thủ công.
*   **Chỉ số Hiệu suất (Performance Metrics):**
    *   Thời gian phản hồi trung bình: <12 giây.
    *   Độ chính xác phân loại ý định: >85% mức độ liên quan.
*   **Trải nghiệm Người dùng/Quản trị viên (User/Admin Experience):**
    *   Giao diện người dùng hoặc bảng điều khiển đơn giản cho quản trị viên xem xét các mục được gắn cờ.
    *   Tóm tắt có thể xuất (CSV hoặc JSON) cho các nhóm hỗ trợ khách hàng.
*   **Tài liệu và Chất lượng Code (Documentation and Code Quality):**
    *   Tuân theo SDLC: bao gồm yêu cầu, thiết kế, triển khai, kế hoạch kiểm thử và hướng dẫn sử dụng.
    *   Cấu trúc code sạch sẽ, module hóa với các bình luận phù hợp.

### 4. Sản phẩm Bàn giao (Deliverables)

Các sản phẩm bàn giao dự kiến bao gồm:
*   **Demo Sản phẩm thử nghiệm:** Một hệ thống tác nhân AI hoạt động với tích hợp Facebook, tự động phản hồi và gắn cờ xem xét.
*   **Slide Thuyết trình:** Kiến trúc Giải pháp, thiết kế hệ thống, hướng dẫn demo, kế hoạch tương lai để cải thiện ý tưởng.
*   **Tài liệu Kỹ thuật:** Hướng dẫn cài đặt, thông số kỹ thuật.
*   **Mã nguồn:** Codebase được cấu trúc tốt với README rõ ràng và các bước cài đặt. Được lưu trữ trên GitHub hoặc nền tảng tương tự hoặc tệp zip.
*   **Kế hoạch & Kết quả Kiểm thử:** Các mẫu đầu vào và đầu ra, xử lý trường hợp biên và tóm tắt các chỉ số đánh giá.

---

## Thử thách #13: BỘ PHÂN TÍCH RỦI RO THÔNG MINH CHO VIỆC XEM XÉT TRƯỜNG HỢP AML (INTELLIGENT RISK ANALYZER FOR AML CASE REVIEW)

### 1. Tổng quan Thử thách

Hiện tại, các trường hợp được tạo ra bởi hệ thống Chống rửa tiền (AML) cần được điều tra và xác minh để đánh giá mức độ rủi ro của khách hàng liên quan. Tuy nhiên, việc sàng lọc thông tin bất lợi (adverse media) trên internet vẫn đang được thực hiện thủ công, dẫn đến tốn thời gian và nguy cơ bỏ sót cao.

**Mục tiêu:**
Thiết kế một giải pháp thông minh có khả năng:
*   Tự động tìm kiếm, thu thập và phân tích thông tin bất lợi về khách hàng từ internet (tin tức, mạng xã hội, nguồn mở, v.v.).
*   Thông tin đầu vào bao gồm: Tên khách hàng, số nhận dạng (Số đăng ký kinh doanh, Mã số thuế đối với doanh nghiệp; CMND/Hộ chiếu đối với cá nhân), quốc tịch và địa chỉ.
*   Cung cấp các khuyến nghị liên quan đến khách hàng (ví dụ: nghi ngờ tội phạm tài chính, tiền án tiền sự, vi phạm pháp luật trước đó, lịch sử bị phạt tiền hoặc hình phạt, v.v.).

### 2. Yêu cầu Kỹ thuật

Giải pháp có thể được triển khai bằng các công nghệ sau:
*   **Ngôn ngữ Lập trình:** Python, JavaScript (Node.js).
*   **AI/ML:** Sử dụng các mô hình ngôn ngữ lớn (LLM) hoặc các mô hình phân loại văn bản.
*   **Trích xuất dữ liệu từ internet:** Scraping API (Google Search, Bing News, Social API).
*   **Xử lý Ngôn ngữ Tự nhiên (NLP):** NER (Nhận dạng Thực thể Có tên - Named Entity Recognition), Phân tích Cảm xúc (Sentiment Analysis), Phân loại Văn bản (Text Classification).
*   **Cơ sở hạ tầng Triển khai:** AWS / Docker Cục bộ.
*   **Frontend (nếu cần):** ReactJS / Streamlit.

### 3. Đánh giá và Đo lường

Hệ thống sẽ được đánh giá dựa trên các tiêu chí sau:
*   **Độ chính xác phát hiện thông tin bất lợi:** ≥ 85%.
*   **Thời gian phản hồi:** Thời gian xử lý < 5 giây mỗi trường hợp (có thể điều chỉnh lên đến < 1 phút tùy thuộc vào lượng thông tin thu thập được).
*   **Mức độ tự động hóa:** > 80% quy trình sàng lọc được hoàn thành mà không cần sự can thiệp thủ công.
*   **Khả năng giải thích:** Các khuyến nghị phải đi kèm với các giải thích rõ ràng.
*   **Demo thuyết phục và trình bày logic.**

### 4. Sản phẩm Bàn giao (Deliverables)

*   **Demo Sản phẩm thử nghiệm:** Có thể là một ứng dụng web hoặc một notebook demo.
*   **Slide Thuyết trình Giải pháp:** Nên bao gồm kiến trúc hệ thống, thuật toán và kết quả.
*   **Tài liệu Kỹ thuật:** Mô tả các phương pháp thu thập dữ liệu (data crawling), xử lý NLP và các mô hình phân loại.
*   **Video Demo Ngắn (tùy chọn).**

---

## Thử thách #14: PHÁT HIỆN BẤT THƯỜNG ĐỘNG TRÊN HỆ THỐNG AML (DYNAMIC ANOMALY DETECTION ON AML SYSTEM)

### 1. Tổng quan Thử thách

Hệ thống Chống rửa tiền (AML) nhận dữ liệu hàng ngày (T+1) từ Trung tâm Tích hợp Dữ liệu (DIH) làm đầu vào, sau đó phân tích, tổng hợp và tính toán dựa trên các kịch bản và thuật toán giám sát được xác định trước. Quá trình này tạo ra các Trường hợp/Cảnh báo (đầu ra) để hỗ trợ PIC (Người phụ trách) thực hiện các hoạt động tuân thủ AML tiếp theo.
Khi giá trị Đầu vào/Đầu ra dao động, điều này có thể cho thấy:
*   **Đối với Đầu vào:** Các vấn đề trong quy trình xử lý dữ liệu (ví dụ: lỗi ETL, thiếu dữ liệu, v.v.).
*   **Đối với Đầu ra:** Điều này có thể cho thấy lỗi hệ thống (ví dụ: lỗi xử lý cuối ngày - EOD batch failures) hoặc các cảnh báo thực tế được kích hoạt bởi các mẫu giao dịch thực sự bất thường.

**Bảng Đầu vào Điển hình:** Thông tin Khách hàng/Tài khoản/Bảng Giao dịch.
**Ví dụ Đầu ra Điển hình:** Các trường hợp giao dịch đáng ngờ được tạo ra dựa trên các kịch bản giám sát (ví dụ: Tài khoản có khối lượng giao dịch đến và đi bất thường).

**Mục tiêu:**
Phát triển một hệ thống để phát hiện và cảnh báo các điểm bất thường trong:
*   Số lượng bản ghi được nhập hàng ngày cho mỗi bảng dữ liệu.
*   Số lượng trường hợp AML được tạo ra cho mỗi kịch bản trên cơ sở hàng ngày/hàng tháng.

Theo đó, hệ thống nên:
*   Phân biệt giữa ngày làm việc thông thường và cuối tuần/ngày lễ.
*   Thực hiện phân tích cho mỗi bảng dữ liệu và mỗi kịch bản (hàng ngày/hàng tháng).
*   Tự động học và xác định các ngưỡng dao động hợp lý cho mỗi loại ngày và mỗi kịch bản.
*   Kích hoạt cảnh báo khi số lượng bản ghi hoặc trường hợp AML vượt quá hoặc giảm xuống dưới các ngưỡng đã xác định.
*   Cung cấp một bảng điều khiển trực quan và thân thiện với người dùng để trực quan hóa.

### 2. Yêu cầu Kỹ thuật

*   **Ngôn ngữ Lập trình:** Python, SQL.
*   **Kỹ thuật:** Phát hiện bất thường trong chuỗi thời gian, ngưỡng thống kê, hoặc học máy cơ bản.
*   **Yêu cầu Đặc biệt:**
    *   Nhận dạng ngày lễ, ánh xạ với ngày giao dịch.
    *   Nhóm theo scenario_id, và phân tích theo ngày/tháng.
*   **Công cụ/Thư viện Đề xuất:** Pandas, Scikit-learn, Prophet, PyOD, Grafana, Isolation Forest, Streamlit.
*   **Dữ liệu:**
    *   Dữ liệu giả lập về số lượng bản ghi hàng ngày cho mỗi bảng (được cung cấp).
    *   Tệp giả lập bao gồm: SCENARIO_ID, SCENARIO_NAME, SCENARIO_TYPE (hàng ngày/hàng tháng), RUN_DATE, và CASE_COUNT.

### 3. Đánh giá và Đo lường

| No. | Tiêu chí                                  | Yêu cầu Cụ thể                                                                  | Điểm Tối đa (Scoring) |
|-----|-------------------------------------------|---------------------------------------------------------------------------------|-----------------------|
| 1   | Độ chính xác Phát hiện Bất thường        | Phát hiện chính xác các điểm bất thường đáng kể vượt ngưỡng cho mỗi bảng/kịch bản | 20                    |
| 2   | Xử lý Ngày lễ/Ngày giao dịch             | Áp dụng logic và ngưỡng riêng cho ngày lễ/cuối tuần                               | 5                     |
| 3   | Học Ngưỡng Tự động                       | Ngưỡng nên được học từ dữ liệu lịch sử, không phải mã hóa cứng                   | 15                    |
| 4   | Hỗ trợ phân tích hàng ngày và hàng tháng   | Xử lý xu hướng hàng ngày và hàng tháng cho mỗi kịch bản                            | 10                    |
| 5   | Khả năng Mở rộng                          | Dễ dàng thêm bảng hoặc kịch bản mới                                               | 10                    |
| 6   | Bảng điều khiển/Biểu đồ                   | Cung cấp trực quan hóa trực quan, bao gồm chế độ xem dòng thời gian và điểm nổi bật bất thường | 10                    |
| 7   | Giải thích Cảnh báo                       | Lý do rõ ràng, dễ hiểu và có thể kiểm chứng & bằng chứng                          | 10                    |
| 8   | Trình bày & Demo                          | Giao diện người dùng, biểu đồ hoặc báo cáo dễ hiểu cho người dùng doanh nghiệp    | 10                    |
| 9   | Tích hợp Hệ thống                         | Hỗ trợ tích hợp với các công việc batch hiện tại hoặc pipeline notebook           | 10                    |

*(Lưu ý: Slide 47 lặp lại bảng đánh giá với mô tả chi tiết hơn cho từng tiêu chí, nội dung cơ bản đã được tích hợp vào bảng trên.)*

### 4. Sản phẩm Bàn giao (Deliverables)

*   **Công cụ Sản phẩm thử nghiệm:** Script/Notebook hoặc Ứng dụng Web với các chức năng:
    *   Tải dữ liệu/số lượng bản ghi hàng ngày.
    *   Hiển thị biểu đồ và cảnh báo cho các điểm bất thường.
*   **Slide Thuyết trình:** Bao gồm kiến trúc hệ thống và thuật toán.
*   **Tài liệu Kỹ thuật:**
    *   Logic xử lý ngày lễ.
    *   Phương pháp học ngưỡng.
    *   Mô tả pipeline xử lý.
*   **(Tùy chọn):** Demo bảng điều khiển sử dụng Streamlit, Grafana, hoặc Matplotlib.

---

## Thử thách #15: HIỆN ĐẠI HÓA KIẾN TRÚC DỊCH VỤ AML SANG KIẾN TRÚC MICROSERVICE (MODERNIZE AML SERVICE ARCHITECTURE TO MICROSERVICE ARCHITECTURE)

### 1. Tổng quan Thử thách

Hệ thống Chống rửa tiền (AML) hiện tại, một sản phẩm của Oracle, được tích hợp với một số hệ thống kinh doanh, bao gồm:
*   **T24:** Sàng lọc giao dịch cho khách hàng vãng lai.
*   **T24/SWIFT:** Sàng lọc giao dịch chuyển tiền quốc tế (SWIFT).
*   **ECMBPM:** Sàng lọc thông tin khách hàng onboarding tại các chi nhánh thực.
*   **BizConnect/ECMBPM:** Sàng lọc trong quá trình thẩm định và phê duyệt cho khách hàng doanh nghiệp lớn.
*   **eKYC:** Sàng lọc khách hàng cá nhân trong quá trình onboarding/mở tài khoản.
*   **MSA:** Sàng lọc các giao dịch liên quan đến chuyển tiền và thu hộ cho các đối tác như TerraPay, Ezy Remit, JP Remit, v.v.

Hiện tại, tất cả các dịch vụ được triển khai tập trung trên WebLogic, gây khó khăn cho việc xây dựng mô hình Tính sẵn sàng cao (HA) và ngăn cản việc mở rộng theo chiều ngang, khiến việc nâng cấp cơ sở hạ tầng trở thành lựa chọn duy nhất để mở rộng.

Do nhu cầu kinh doanh ngày càng tăng và sự cần thiết phải tuân thủ các quy định AML, hệ thống AML phải áp dụng một phương pháp cho phép phát triển nhanh chóng, khả năng mở rộng mạnh mẽ, khả năng tích hợp đa dạng và hiệu suất được tối ưu hóa. Các mục tiêu tích hợp tiềm năng bao gồm:
*   Trung gian thanh toán: Western Union, v.v.
*   Nền tảng thương mại điện tử: PingPong, Alibaba, v.v.
*   Nhà cung cấp ví điện tử.
*   Hệ thống nội bộ: eKYC doanh nghiệp, quy trình dự án tối ưu hóa hoạt động, v.v.
*   Các hệ thống trong hệ sinh thái doanh nghiệp rộng lớn hơn.

**Mục tiêu:**
Thiết kế một mô hình triển khai dựa trên Microservices để cho phép khả năng mở rộng hệ thống và HA.
Áp dụng mô hình để di chuyển các dịch vụ được chọn hiện đang triển khai trên Oracle WebLogic sang một kiến trúc microservices mới.

### 2. Yêu cầu Kỹ thuật

*   **Ngôn ngữ Lập trình:** Python, SQL, Java Spring Boot (embedded Tomcat).
*   **Kỹ thuật:** Docker, Docker Compose, CI/CD sử dụng Jenkins, Git, Linux, AWS ECS hoặc EKS.
*   **Yêu cầu Đặc biệt:** Xây dựng và kiểm thử khả năng Tự động Mở rộng (Auto Scaling) và HA cho các dịch vụ mới khi máy chủ chính gặp sự cố.
*   **Công cụ Đề xuất:** Docker, Docker Compose, CI/CD với Jenkins, Git, Linux, AWS ECS hoặc EKS.
*   **Môi trường:** AWS Non-production (môi trường SIT) hoặc một tài khoản lab (phù hợp với chính sách của ngân hàng).

### 3. Đánh giá và Đo lường

| No. | Tiêu chí                     | Yêu cầu Cụ thể                                                                      | Điểm Tối đa (Scoring) |
|-----|------------------------------|-------------------------------------------------------------------------------------|-----------------------|
| 1   | Thực tiễn (Practical)        | Khả năng phát triển và cấu hình dịch vụ theo yêu cầu cho mỗi tác vụ                  | 10                    |
| 2   | Đánh giá Vấn đề              | Hiểu rõ tuyên bố vấn đề và liên hệ nó với các thực tiễn tốt nhất                     | 15                    |
| 3   | Tư duy Kiến trúc             | Đề xuất một kiến trúc hệ thống sơ bộ dựa trên các vấn đề đã cho và thực tiễn tốt nhất | 15                    |
| 4   | Tối ưu hóa Kiến trúc         | Đề xuất ít nhất hai tùy chọn triển khai, phân tích ưu nhược điểm của mỗi tùy chọn    | 20                    |
| 5   | Coding & Phát triển         | Hoàn thành mỗi tác vụ đúng hạn; tối ưu hóa code về hiệu suất và chi phí             | 25                    |
| 6   | Trình bày & Demo             | Tài liệu rõ ràng, chi tiết và trình bày giải pháp một cách thuyết phục               | 15                    |

*(Lưu ý: Slide 53 lặp lại bảng đánh giá với mô tả chi tiết hơn cho từng tiêu chí, nội dung cơ bản đã được tích hợp vào bảng trên.)*

### 4. Sản phẩm Bàn giao (Deliverables)

*   **Công cụ Sản phẩm thử nghiệm:** Các dịch vụ mới được kích hoạt theo mô hình microservices, hoạt động độc lập và song song với các dịch vụ Oracle WebLogic hiện có.
*   **Slide Kiến trúc Giải pháp.**
*   **Mã nguồn:** Được lưu trữ trong kho GitLab của VPBank.
*   **Tài liệu Giải pháp.**
*   **Checklist Yêu cầu Thay đổi (CR) Giả lập.**
*   **Kết quả Kiểm thử Tự động Mở rộng và HA:** Bao gồm kết quả kiểm thử hiệu suất.

---

## Thử thách #16: ÁP DỤNG AI/ML VÀO DỰ BÁO VÀ PHÂN TÍCH CHI PHÍ NGÂN SÁCH NHÂN SỰ (APPLYING AI/ML TO FORECASTING AND ANALYZING HUMAN RESOURCES BUDGET COSTS)

### 1. Tổng quan Thử thách

Xây dựng một nền tảng tận dụng AI/ML để dự báo chi phí ngân sách nhân sự dựa trên các kịch bản kinh doanh khác nhau và phân tích dữ liệu nhân sự bằng GenAI trong hệ sinh thái SAP ERP tại VPBank. Nền tảng này sẽ được phát triển bằng các công nghệ hiện đại, tận dụng các dịch vụ đám mây của AWS và khai thác sức mạnh của AI/ML để hỗ trợ phân tích và ra quyết định trong việc lập kế hoạch nguồn nhân lực và quản lý số lượng nhân viên.

### 2. Yêu cầu Kỹ thuật

Giải pháp có thể được thực hiện bằng các công nghệ sau:
*   **Ngôn ngữ Lập trình:** Fiori/TypeScript, Node.js, dịch vụ API Restful, SAP ABAP (tùy chọn), R, Python hoặc Java.
*   **AI/ML:**
    *   **Mô hình AI:** Dự báo dựa trên chuỗi thời gian & hồi quy sử dụng: Prophet /XGBoost/ LSTM.
    *   **Mô hình Ngôn ngữ Lớn (LLM):** OpenAI, Gemma, hoặc Open LLM.
*   **Cơ sở hạ tầng Triển khai:** AWS.
*   **Frontend:** Fiori / TypeScript.
*   **Backend:** Node.js/Python.

### 3. Đánh giá và Đo lường

*   **Tích hợp:** Tự động hóa ETL dữ liệu nhân sự từ SAP ERP để hợp lý hóa và phân loại dữ liệu, đồng thời thực hiện phân cụm dữ liệu để cung cấp cho các mô hình đào tạo AI/ML thông qua Dịch vụ OData API Restful.
*   **Giao diện Người dùng:** Cung cấp một giao diện SAP Fiori đơn giản hóa cho người dùng cuối (HR) để xem màn hình dự báo chi phí ngân sách nhân sự theo thời gian, trực quan hóa kết quả dự báo thông qua bảng điều khiển hoặc báo cáo, thực hiện phân tích What-if, và xuất báo cáo để hội đồng quản trị/ban điều hành xem xét.
*   **Thông tin Đầu vào, Dữ liệu, Mô hình AI/ML và Thông tin Đầu ra:**
    *   Dữ liệu Đầu vào.
    *   Mô hình AI/ML.
    *   Dữ liệu Đầu ra.
    *   Các tùy chọn Cấu hình Gợi ý (Prompt Configuration Options) và lựa chọn các mô hình LLM (GPT-3.5, Claude).
*   **Độ chính xác lớn hơn 90%.**
*   **Đảm bảo thời gian phản hồi dưới 10 giây.**
*   **Sử dụng các dịch vụ đám mây gốc của AWS, bao gồm ECS, EKS, Lambda, ALB, RDS database.**
*   **Frontend được xây dựng bằng Fiori và Typescript.**
*   **Backend được triển khai bằng Node.js/Python.**

### 4. Sản phẩm Bàn giao (Deliverables)

*   Bài thuyết trình bao gồm thiết kế cấp cao và cấp thấp, chi tiết thiết kế giải pháp kỹ thuật và demo ứng dụng.

---

## Thử thách #17: HIỆN ĐẠI HÓA CƠ SỞ DỮ LIỆU ORACLE & DI CHUYỂN KIẾN TRÚC SẠCH (ORACLE DATABASE MODERNIZATION & CLEAN ARCHITECTURE MIGRATION)

### 1. Tổng quan Thử thách

Hiện đại hóa một hệ thống Cơ sở dữ liệu Oracle cũ, bao gồm schemas, packages, stored procedures, bằng cách:
*   Di chuyển toàn bộ cơ sở dữ liệu sang PostgreSQL.
*   Tái cấu trúc (Refactoring) tất cả logic PL/SQL thành các microservices Java theo các nguyên tắc Kiến trúc Sạch (Clean Architecture).

Mục tiêu là tách rời logic nghiệp vụ khỏi cơ sở dữ liệu, cải thiện khả năng bảo trì và cho phép mở rộng tốt hơn bằng cách sử dụng các thực hành DevOps và kiến trúc hiện đại.
Hệ thống phải có khả năng:
*   Phân tích và di chuyển các schema Oracle sang các schema tương đương trong PostgreSQL.
*   Chuyển đổi các stored procedures và packages phức tạp thành mã Java.
*   Tổ chức mã theo các lớp: Controller → Use Case → Service → Repository.
*   Duy trì tính nhất quán của dữ liệu và sự tương đương về logic.
*   Viết các bài kiểm thử đơn vị/tích hợp toàn diện.
*   Hỗ trợ xử lý hàng loạt/không đồng bộ (batch/async) khi áp dụng.

### 2. Yêu cầu Kỹ thuật

| Thành phần             | Công nghệ / Kỹ năng Mong đợi                                  |
|------------------------|---------------------------------------------------------------|
| Ngôn ngữ Lập trình    | Java (Spring Boot), PL/SQL, Bash                              |
| Cơ sở dữ liệu           | Oracle → PostgreSQL                                           |
| Công cụ Di chuyển       | Ora2Pg, OpenRewrite, Flyway, Liquibase                        |
| Kiến trúc              | Clean Architecture, Hexagonal/DDD concepts                    |
| Tích hợp               | RESTful APIs, Kafka/RabbitMQ (cho các công việc không đồng bộ) |
| Cơ sở hạ tầng          | Docker, Kubernetes, GitLab CI/CD                              |
| Kiểm thử                | JUnit, Mockito, Testcontainers                                |
| Khả năng Quan sát (Bonus)| ELK, Prometheus, Grafana                                      |

### 3. Đánh giá và Đo lường

| Tiêu chí                               | Yêu cầu Tối thiểu                                                    |
|----------------------------------------|----------------------------------------------------------------------|
| Độ chính xác Di chuyển Schema          | ≥ 95% khớp cấu trúc + ràng buộc chính xác                            |
| Độ bao phủ Tái cấu trúc PL/SQL → Java | ≥ 95% được chuyển đổi với độ chính xác logic                          |
| Sự sạch sẽ của Code & Kiến trúc       | Phải tuân theo các hướng dẫn SOLID, DRY, và Clean Arch                 |
| Độ bao phủ Kiểm thử                    | ≥ 70% độ bao phủ kiểm thử đơn vị                                      |
| Hiệu suất                              | Bằng hoặc cải thiện so với luồng Oracle gốc                            |
| Khả năng Xử lý Hàng loạt               | ≥ 10,000 bản ghi trong dưới 5 phút                                   |
| Khả năng Phục hồi / Logic Thử lại      | Được triển khai cho các lỗi và thời gian chờ                          |
| Tài liệu                               | Báo cáo di chuyển rõ ràng và sơ đồ kiến trúc                          |

### 4. Sản phẩm Bàn giao (Deliverables)

| Sản phẩm Bàn giao        | Mô tả                                                                    |
|--------------------------|--------------------------------------------------------------------------|
| Oracle Source Dump       | DDL schema Oracle cũ + packages + procedures                             |
| Java Microservices       | Logic nghiệp vụ được tái cấu trúc bằng Java (Spring Boot + Clean Arch)    |
| PostgreSQL Schema        | Schema được di chuyển ở định dạng tương thích Flyway/Liquibase             |
| Bảng Ánh xạ (Mapping Matrix)| Bảng ánh xạ: Oracle → PostgreSQL + PL/SQL → các hàm Java                 |
| Báo cáo Kỹ thuật         | Kiến trúc, phương pháp tiếp cận, sự đánh đổi, những thách thức             |
| Báo cáo Hiệu suất        | Điểm chuẩn trước/sau, bao gồm thông lượng & độ trễ                        |
| Bộ Slide (Slide Deck)    | Tóm tắt 5 phút: vấn đề → kiến trúc → triển khai → kết quả                 |
| Kết quả Kiểm thử         | Nhật ký kiểm thử đơn vị + tích hợp + ảnh chụp màn hình độ bao phủ          |
| (Bonus) Admin UI         | Giao diện Web UI để xem/tái cấu trúc/quản lý các stored procedures trực quan (tùy chọn) |

---

## Thử thách #18: XÂY DỰNG TÁC NHÂN AI DỮ LIỆU CHO CÁC CHI NHÁNH (BUILD DATA AI AGENT FOR BRANCHES)

### 1. Tổng quan Thử thách

Trong lĩnh vực ngân hàng, các nhà lãnh đạo đơn vị kinh doanh (như giám đốc khu vực và giám đốc chi nhánh) và Quản lý Quan hệ Khách hàng (RM) gặp phải những trở ngại khi họ truy xuất và phân tích thông tin khách hàng:
*   **Dữ liệu khách hàng bị phân mảnh và khó truy cập:**
    *   Thông tin nằm rải rác trên nhiều hệ thống: T24, W4, LOS, Internet Banking, và thậm chí cả các tệp ngoại tuyến được duy trì thủ công.
    *   Cần nhiều thời gian và nỗ lực đáng kể để tổng hợp dữ liệu này và có được hiểu biết toàn diện, đa chiều về từng khách hàng.
*   **Thiếu công cụ truy vấn bằng ngôn ngữ tự nhiên:**
    *   Người dùng thường cần đặt câu hỏi như:
        *   "Những khách hàng bán lẻ nào có tài khoản tiết kiệm đáo hạn trong tháng này?"
        *   "Những công ty nào trong danh mục của tôi đã có dòng tiền giảm trong ba tháng liên tiếp?"

Hiện tại, các truy vấn như vậy chỉ có thể được thực hiện bằng cách:
*   Viết truy vấn SQL.
*   Sử dụng các bảng điều khiển được xây dựng sẵn cứng nhắc.
*   Hoặc dựa vào sự hỗ trợ của bộ phận IT.

**Thiếu phân tích hành vi và các khuyến nghị chủ động:**
*   Hệ thống không thể phát hiện các dấu hiệu rời bỏ của khách hàng hoặc cơ hội bán chéo.
*   Các RM không được hỗ trợ bằng các khuyến nghị về việc nên tương tác với ai, khi nào, và với nội dung gì.
*   Người dùng phải tự lọc và phân khúc khách hàng dựa trên ngành nghề, ngân sách, hoặc hành vi giao dịch.

**Giải pháp Đề xuất Dựa trên GenAI:** Phát triển một Trợ lý được hỗ trợ bởi GenAI có khả năng phân tích hồ sơ khách hàng hợp nhất và dữ liệu hành vi trên cả phân khúc Bán lẻ và Doanh nghiệp. Trợ lý này sẽ trao quyền cho người dùng:
*   Truy vấn thông tin chi tiết của khách hàng bằng ngôn ngữ tự nhiên.
*   Đưa ra quyết định sáng suốt nhanh hơn.
*   Và chủ động quản lý mối quan hệ khách hàng bằng các hành động phù hợp, kịp thời.

### 2. Yêu cầu Kỹ thuật

Sản phẩm thử nghiệm nên có:
*   Giao diện Trò chuyện (Chatbox).
*   Ngôn ngữ lập trình: Python/Go/Nodejs.
*   Dịch vụ đám mây: AWS.
*   Cơ sở dữ liệu: Redshift, Glue Catalog.
*   Ghi nhật ký, theo dõi tác vụ.

### 3. Đánh giá và Đo lường

Sản phẩm thử nghiệm nên có:
*   Theo dõi tác vụ, ghi nhật ký cho bảng điều khiển giám sát.
*   CI/CD.
*   Độ chính xác câu trả lời: > 95%.
*   Thời gian phản hồi: < 20 giây cho luồng từ đầu đến cuối (end2end flow).
*   Đo lường hiệu suất cơ sở dữ liệu:
    *   Thời gian phản hồi thực thi SQL: <5 giây.
    *   Sử dụng CPU: < 80%.

### 4. Sản phẩm Bàn giao (Deliverables)

*   Các trường hợp thử nghiệm, kết quả thử nghiệm.
*   Thiết kế hệ thống, triển khai, và hướng dẫn vận hành.
*   Bài thuyết trình, mã nguồn, Demo Sản phẩm thử nghiệm.

---

## Thử thách #19: THIẾT KẾ ỨNG DỤNG THẨM ĐỊNH CHO VAY SỬ DỤNG BPM VỚI AI (DESIGN A LENDING APPRAISAL APPLICATION USING BPM WITH AI)

### 1. Tổng quan Thử thách

**Ngân hàng Số Thông minh – InstaLend**
Ngân hàng đặt mục tiêu phát triển một nền tảng BPM thông minh tích hợp với AI để tự động hóa quy trình phê duyệt khoản vay, đồng thời phân tích hành vi khách hàng để cá nhân hóa các đề xuất sản phẩm tài chính cho từng phân khúc.

Hệ thống dự kiến sẽ hỗ trợ số hóa toàn bộ quy trình phê duyệt khoản vay bằng nền tảng BPM. Quy trình bao gồm nhập liệu bởi người tạo (maker), phê duyệt bởi người kiểm tra (checker), và điều phối thông qua một công cụ BPM (BPM engine). AI được tích hợp để hỗ trợ ra quyết định, phân tích nội dung đơn xin vay, và cung cấp các khuyến nghị thông minh. Sau khi khoản vay được phê duyệt, hệ thống tiếp tục phân tích dữ liệu hành vi khách hàng để đề xuất các sản phẩm tài chính liên quan phù hợp với các phân khúc khách hàng cụ thể. Ngoài ra, AI có thể giúp xác định các điểm nghẽn trong quy trình và đề xuất các tối ưu hóa. Giải pháp được thiết kế như một kiến trúc từ đầu đến cuối được triển khai trên AWS, đảm bảo khả năng mở rộng, tích hợp linh hoạt, và sử dụng hiệu quả các dịch vụ AI/ML hiện đại.

### 2. Yêu cầu Kỹ thuật

| Thành phần             | Mô tả                                                                                             |
|------------------------|---------------------------------------------------------------------------------------------------|
| BPM Engine             | Sử dụng Camunda hoặc Flowable để mô hình hóa và điều phối quy trình phê duyệt khoản vay (BPMN 2.0).   |
| Tích hợp AI           | Tích hợp các gợi ý AI sử dụng LLM API (OpenAI, Bedrock, Claude) hoặc một mô hình ML nhẹ.            |
| Gọi AI (AI Invocation)| AI được gọi thông qua REST API hoặc tác vụ dịch vụ bên ngoài trong quy trình BPM.                   |
| Frontend               | Được xây dựng bằng ReactJS hoặc Next.js cho giao diện nhập liệu và giám sát quy trình UI.            |
| Backend                | Được phát triển bằng Node.js (hoặc Spring Boot) để xử lý tích hợp BPM và AI.                        |
| Backend Frameworks     | Express / Fastify (Node.js) hoặc Spring Web (Java).                                               |
| Cơ sở dữ liệu           | Sử dụng PostgreSQL để lưu trữ dữ liệu giao dịch và trạng thái quy trình.                            |
| Triển khai             | Sử dụng Docker Compose cho phát triển cục bộ hoặc AWS ECS cho triển khai trên đám mây.              |
| Môi trường Demo        | Hỗ trợ Docker cục bộ hoặc AWS EC2 free tier cho mục đích demo.                                     |
| Xác thực (Authentication)| Tùy chọn: Vai trò người dùng được mã hóa cứng (Maker/Checker), hoặc xác thực nhẹ qua Firebase Auth / Keycloak. |

### 3. Đánh giá và Đo lường

| Tiêu chí                                            | Chi tiết                                                                                                                                                                                                                                                                                                                      |
|-----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1. Hoàn thành Quy trình BPM (35%)**                 | - Quy trình được xác định rõ ràng được mô hình hóa bằng BPMN 2.0. <br> - Bao gồm ít nhất ba bước chính: Maker, Checker, và Tác vụ AI/Dịch vụ. <br> - Được triển khai và thực thi thành công trên nền tảng BPM (ví dụ: Camunda, Flowable). <br> - Quy trình từ đầu đến cuối chạy trơn tru không có lỗi chuyển tiếp hoặc logic. |
| **2. Tích hợp AI (30%)**                             | - AI được tích hợp vào luồng BPM thông qua REST API hoặc Tác vụ Bên ngoài. <br> - Các lệnh gọi AI thực tế được thực hiện, với kết quả rõ ràng, có thể diễn giải được. <br> - Đầu ra AI ảnh hưởng trực tiếp đến logic phê duyệt (ví dụ: phê duyệt/từ chối, đề xuất sản phẩm). <br> - (Tùy chọn) Xử lý dự phòng hoặc lỗi được triển khai cho các lỗi hoặc bất thường của AI. |
| **3. Kiến trúc Kỹ thuật & Khả năng Triển khai (25%)** | - Codebase được cấu trúc tốt và sẵn sàng cho demo (cục bộ hoặc trên đám mây). <br> - Bao gồm Dockerfile, kịch bản cài đặt, và README rõ ràng. <br> - Thời gian cài đặt demo trong vòng 10-15 phút. <br> - Kết quả có thể được xác thực qua nhật ký, phản hồi API, hoặc chỉ báo UI. |
| **4. Khả năng Mở rộng & Đổi mới (10%)**              | - Bao gồm các tính năng mở rộng như phân tích, bảng điều khiển, hoặc cảnh báo trạng thái. <br> - Thể hiện việc sử dụng AI một cách sáng tạo (ví dụ: phân tích hành vi, đề xuất sản phẩm động). <br> - Cho thấy tiềm năng mở rộng trong thế giới thực, phát triển MVP, hoặc tích hợp với các hệ thống hiện có. |

### 4. Sản phẩm Bàn giao (Deliverables)

| Sản phẩm Bàn giao     | Mô tả                                                                         |
|-----------------------|-------------------------------------------------------------------------------|
| Mô hình Quy trình BPMN | Mô hình quy trình BPMN được triển khai trên Camunda/Flowable.                    |
| Demo Từ đầu đến cuối  | Ứng dụng demo bao gồm toàn bộ quy trình: Maker → AI → Checker.                 |
| Tích hợp AI          | AI thực tế được tích hợp vào quy trình (qua API hoặc tác vụ dịch vụ).            |
| Mã nguồn              | Mã nguồn hoàn chỉnh (frontend, backend, BPM) với hướng dẫn cài đặt.             |
| Slide Thuyết trình    | Bộ slide trình bày giải pháp và các thành phần của nó.                          |
| Video Demo / Kiến trúc| Video demo ngắn hoặc sơ đồ kiến trúc hệ thống.                                  |

---

## Thử thách #20: PHÂN ĐOẠN NGỮ NGHĨA CHO TÀI LIỆU ĐA ĐỊNH DẠNG (SEMANTIC CHUNKING FOR MULTI-FORMAT DOCUMENTS)

### 1. Tổng quan Thử thách

Xây dựng một Hệ thống Phân tích Văn bản có khả năng xử lý nhiều định dạng tài liệu (PDF, Word, Excel, Hình ảnh được Quét) và trích xuất các phân đoạn ngữ nghĩa có cấu trúc, cho phép truy xuất thông minh, tìm kiếm ngữ nghĩa, và các kịch bản RAG (Retrieval-Augmented Generation - Tạo sinh Tăng cường bằng Truy xuất).

Hệ thống phải có khả năng:
*   Xử lý nhiều định dạng tài liệu, bao gồm:
    *   a. Tài liệu PDF.
    *   b. Tệp Word (.docx).
    *   c. Bảng tính Excel (.xlsx).
    *   d. Hình ảnh được quét (yêu cầu OCR).
*   Trích xuất các phần (chunks) có ý nghĩa bao gồm:
    *   a. Tiêu đề phần.
    *   b. Nội dung phần.
    *   c. Tùy chọn: Thẻ/nhãn (ví dụ: "Thông tin Khách hàng", "Điều khoản Khoản vay").
*   Hỗ trợ các trường hợp sử dụng tìm kiếm & truy xuất thông minh, bao gồm:
    *   a. Tìm kiếm Ngữ nghĩa (dựa trên ngữ cảnh, không phải từ khóa).
    *   b. Tích hợp với trả lời câu hỏi dựa trên RAG/LLM.

### 2. Yêu cầu Kỹ thuật

| Thành phần             | Công nghệ / Kỹ năng Mong đợi                                     |
|------------------------|------------------------------------------------------------------|
| Ngôn ngữ Lập trình    | Python, Java, TypeScript, PHP, Go                                |
| OCR / Phân tích Bố cục | Giải pháp nguồn mở                                                |
| Xử lý Tài liệu         | Giải pháp nguồn mở                                                |
| Mô hình AI             | Bất kỳ LLM nào (GPT-4, Claude, LLaMA, v.v.)                       |
| Cơ sở hạ tầng          | Docker, Kubernetes, Redis, Kafka hoặc RabbitMQ (cho hàng đợi)     |
| Cơ sở dữ liệu           | Postgresql; MongoDB                                              |

### 3. Đánh giá và Đo lường

| Tiêu chí                                  | Yêu cầu Tối thiểu                                                              |
|-------------------------------------------|-------------------------------------------------------------------------------|
| Độ chính xác phân đoạn ngữ nghĩa          | ≥ 90% tính đúng đắn của các chunks                                            |
| Độ chính xác tạo nhãn/thẻ                 | ≥ 85% liên quan và được gắn nhãn chính xác                                   |
| Định dạng đầu ra JSON                     | Có cấu trúc tốt và sạch sẽ, có thể sử dụng trong các hệ thống khác              |
| Tốc độ xử lý                              | ≤ 30 giây mỗi tài liệu (10 trang, trung bình 50 dòng/trang)                   |
| Khả năng mở rộng: Thông lượng hàng loạt    | ≥ 100 tài liệu/phút trong xử lý hàng loạt (đa luồng hoặc không đồng bộ)       |
| Hỗ trợ đồng thời (CCU)                    | Xử lý ≥ 100 người dùng hoặc yêu cầu đồng thời                                 |
| Kiến trúc hàng đợi                        | Hỗ trợ xử lý không đồng bộ qua RabbitMQ, Kafka                                |
| Cơ chế thử lại / Chịu lỗi                 | Phải xử lý lỗi OCR, thời gian chờ, hoặc sự cố một phần                         |
| Khả năng kiểm thử                         | ≥ 70% độ bao phủ kiểm thử đơn vị (Bonus: kiểm thử tích hợp toàn bộ luồng)      |
| Thiết kế gợi ý cho LLM (nếu sử dụng)       | Đầu ra JSON ổn định, ảo giác tối thiểu (minimal hallucination)                 |

### 4. Sản phẩm Bàn giao (Deliverables)

| Sản phẩm Bàn giao      | Mô tả                                                                              |
|------------------------|------------------------------------------------------------------------------------|
| Mẫu Tài liệu           | Ít nhất 2-3 tệp đa định dạng (.pdf, .docx, .jpg, .xlsx)                            |
| Mã nguồn / API Hoạt động| Script hoặc REST API tiếp nhận tài liệu & trả về các chunks ngữ nghĩa               |
| Mẫu Đầu ra JSON        | Định dạng có cấu trúc với tiêu đề, nội dung, thẻ, thông tin trang tùy chọn          |
| Báo cáo Kỹ thuật       | Mô tả thiết kế pipeline, kiến trúc, những thách thức                               |
| Bộ Slide (Slide Deck)  | Bài thuyết trình 5 phút bao gồm vấn đề, kiến trúc, kết quả                         |
| Bằng chứng Kiểm thử (Bonus)| Kết quả điểm chuẩn cho hiệu suất hàng loạt / CCU cao                              |
| (Bonus) Giao diện Đánh giá UI| Frontend (ví dụ: Streamlit) để trực quan hóa tài liệu & các chunks được trích xuất |

---

**THỬ THÁCH CÔNG NGHỆ #21: GIẢI PHÁP DI CHUYỂN ETL TỪ SSIS ON-PREMISE LÊN AWS CLOUD**

**1. Tổng quan Thách thức**

*   **Hiện trạng:**
    VPBank hiện đang vận hành quy trình ETL (Extract, Load and Transform - Trích xuất, Tải và Chuyển đổi) sử dụng SQL Server Integration Services (SSIS) và Microsoft SQL Server trên hạ tầng tại chỗ (On-premise).
    Dòng dữ liệu cơ bản được trình bày như sau: Các nguồn dữ liệu (T24, W4, OCB, B2B) được trích xuất và tải (Extract and Load) vào Data Integration Hub (DIH). Sau đó, dữ liệu thô (RAW DB) được xử lý qua SSIS-ETL và truy vấn (Query) bởi DB App để phục vụ các ứng dụng (BOF, EBU, ACH).

*   **Mục tiêu:**
    Hệ thống cần được tái kiến trúc và tận dụng các công nghệ AWS để giảm thiểu việc sao chép dữ liệu giữa các hệ thống, cắt giảm chi phí lưu trữ và tăng cường khả năng giám sát vận hành thông qua các dịch vụ của AWS.
    Dòng dữ liệu mới cơ bản được trình bày như sau: Các nguồn dữ liệu (T24, W4, OCB, B2B) được trích xuất và tải vào EOD (End of Day) trên Cloud. Sau đó, dữ liệu được xử lý qua AWS-ETL và truy vấn bởi DB App để phục vụ các ứng dụng (BOF, EBU, ACH).

    Các ứng viên được yêu cầu xây dựng một giải pháp để di chuyển hệ thống SSIS-ETL hiện tại lên AWS Cloud/nền tảng AWS-ETL, với các tiêu chí sau:
    *   Sử dụng bộ công nghệ AWS để triển khai các quy trình nghiệp vụ hiện đang chạy trên nền tảng SSIS, đảm bảo hiệu suất tương đương hoặc tốt hơn so với hệ thống on-premise (tham khảo ví dụ trong phụ lục).
    *   Thiết kế giải pháp serverless với khả năng mở rộng linh hoạt và tối ưu hóa chi phí.
    *   Áp dụng các dịch vụ CI/CD, Infrastructure as Code (IaC) và bảo mật theo tiêu chuẩn của AWS.
    *   Đảm bảo tuân thủ SLA và giảm thiểu gián đoạn hệ thống trong quá trình vận hành.

**2. Yêu cầu Kỹ thuật**

*   Ngôn ngữ lập trình: Python, Scala, Java.
*   Dịch vụ đám mây: AWS (bao gồm nhưng không giới hạn AWS, IAM, S3, Glue).
*   Quản lý mã nguồn: Git.
*   Tài liệu hóa luồng dữ liệu và logic nghiệp vụ.
*   Tận dụng các dịch vụ serverless và managed services của AWS.
*   Triển khai các job ETL trên AWS (chi tiết trong phụ lục).
*   Triển khai logging, giám sát trạng thái và xử lý lỗi.
*   Triển khai CI/CD.
*   Thiết kế giải pháp có khả năng đáp ứng tăng trưởng dữ liệu.
*   Thời gian xử lý tương đương hoặc nhanh hơn giải pháp hiện tại.
*   Phân tích và tối ưu hóa chi phí vận hành trên AWS.

**3. Tiêu chí Đánh giá và Đo lường**

*   **Giải pháp (Trọng số 50%):**
    *   *Điểm 1:* Ý tưởng không giải quyết được tiêu chí 1 hoặc 2 hoặc không thể hiện các tiêu chí như đã nêu trong yêu cầu.
    *   *Điểm 2:* Ý tưởng giải quyết được tiêu chí 1 hoặc 2.
    *   *Điểm 3:* Ý tưởng giải quyết được tiêu chí 1 và 2.
    *   *Điểm 4:* Ý tưởng giải quyết được tiêu chí 1, 2 và một trong hai tiêu chí 3 hoặc 4.
    *   *Điểm 5:* Ý tưởng giải quyết được tất cả 4 tiêu chí.
        1.  Sử dụng bộ công nghệ AWS để triển khai các quy trình nghiệp vụ hiện đang chạy trên nền tảng SSIS, đảm bảo hiệu suất tương đương hoặc tốt hơn so với on-premise.
        2.  Thiết kế giải pháp serverless với khả năng mở rộng linh hoạt và tối ưu hóa chi phí.
        3.  Áp dụng các dịch vụ CI/CD, Infrastructure as Code (IaC) và bảo mật theo tiêu chuẩn của AWS.
        4.  Đảm bảo tuân thủ SLA và giảm thiểu gián đoạn hệ thống trong quá trình vận hành.

*   **Hiệu suất (Trọng số 25%):**
    *   Thời gian hoàn thành job ETL chậm hơn 100% so với chạy trên nền tảng tương đương (theo phụ lục).
    *   Thời gian hoàn thành job ETL chậm hơn >30% so với chạy trên nền tảng tương đương (theo phụ lục).
    *   Thời gian hoàn thành job ETL tương đương với chạy trên nền tảng tương tự (theo phụ lục).
    *   Thời gian hoàn thành job ETL nhanh hơn 30% so với chạy trên nền tảng tương đương (theo phụ lục).
    *   Thời gian hoàn thành job ETL nhanh hơn 100% so với chạy trên nền tảng tương đương (theo phụ lục).

*   **Sản phẩm bàn giao và Thuyết trình (Trọng số 25%):**
    *   Thiếu 2 trong số 4 tài liệu bàn giao nhưng tài liệu kiến trúc hệ thống là bắt buộc (theo Mục 7. Sản phẩm bàn giao).
    *   Thiếu 1 trong số 4 tài liệu bàn giao nhưng tài liệu kiến trúc hệ thống và mã nguồn là bắt buộc.
    *   Bàn giao đầy đủ theo yêu cầu trong Mục 7.
    *   Bàn giao đầy đủ theo yêu cầu trong Mục 7. Tài liệu được trình bày khoa học/có phương pháp. Mã nguồn được tổ chức tốt và tối ưu hóa.
    *   Bàn giao đầy đủ theo yêu cầu trong Mục 7. Tài liệu được trình bày khoa học/có phương pháp. Mã nguồn được tổ chức tốt và tối ưu hóa. Trình bày tốt, dễ hiểu.

**4. Sản phẩm Bàn giao**

*   **Kiến trúc Hệ thống:**
    *   Sơ đồ kiến trúc tổng thể trên AWS (S3, IAM, Redshift,...).
    *   Mô tả luồng dữ liệu và tổ chức pipeline.
    *   Các mẫu hình (patterns) và thực tiễn tốt nhất (best practices) đã áp dụng.
*   **Triển khai Mã nguồn:**
    *   Script AWS (Python hoặc Scala).
    *   File cấu hình CloudFormation hoặc Terraform.
*   **Báo cáo Hiệu suất:**
    *   So sánh hiệu suất dựa trên thời gian xử lý với dữ liệu tương tự như trong phụ lục dưới đây.
*   **Video Demo:**
    *   Video ghi lại quá trình chương trình chạy với dữ liệu tương tự như trong phụ lục dưới đây.
    *   Giải thích rõ ràng các thành phần chính của giải pháp và luồng vận hành.

---

**THỬ THÁCH CÔNG NGHỆ #22 (KHỐI EDA): HỆ THỐNG ĐA TÁC NHÂN GENAI CHO TỰ ĐỘNG HÓA QUY TRÌNH**

**1. Tổng quan Thách thức**

Thử thách này tập trung vào việc khai thác tài năng trẻ để giải quyết nhu cầu ngày càng tăng về phát triển nhanh các giải pháp kinh doanh dựa trên AI. Cơ hội chủ yếu nằm ở việc đóng góp vào nền tảng GenAI Agent Studio, cho phép phát triển và chia sẻ các Tác nhân GenAI có thể tái sử dụng, chẳng hạn như các tác nhân cho OCR, trích xuất thông tin, xử lý tài liệu, v.v.

Mục tiêu chính là tận dụng studio này và thư viện tác nhân của nó để tăng tốc việc tạo ra các ứng dụng đầu cuối phù hợp cho các Đơn vị Kinh doanh khác nhau, cho phép các giải pháp như Trợ lý RM, soạn thảo tài liệu tự động, xác thực cam kết CASA, Phong tỏa Tài khoản, v.v., qua đó thúc đẩy đổi mới và tự động hóa quy trình kinh doanh.

**2. Yêu cầu Kỹ thuật**

*   Hiểu biết về các nguyên tắc học máy, kiến trúc học sâu và các khái niệm cốt lõi đằng sau các mô hình Generative AI, đặc biệt là các Mô hình Ngôn ngữ Lớn (LLMs).
*   Quen thuộc với việc sử dụng và có khả năng tinh chỉnh (fine-tuning) các mô hình GenAI. Quen thuộc với các khái niệm tác nhân AI, bao gồm luồng công việc của tác nhân (agentic workflows), tích hợp công cụ, lập kế hoạch và có khả năng là các hệ thống đa tác nhân sử dụng các framework như LangChain, LlamaIndex, hoặc tương tự.
*   Kỹ năng viết mã tốt bằng Python và các thư viện liên quan cho Học sâu và các tác nhân GenAI.
*   Kỹ năng xử lý văn bản, trích xuất thông tin, phân tích tình cảm và hiểu cấu trúc tài liệu là rất quan trọng đối với các tác nhân tương tác với dữ liệu văn bản.
*   Kiến thức về xử lý hình ảnh cơ bản và các kỹ thuật thị giác máy tính như phát hiện đối tượng, OCR, v.v., là cần thiết để phát triển các tác nhân dựa trên thị giác.
*   Quen thuộc với việc xây dựng các thành phần phần mềm module hóa và có thể tái sử dụng, thiết kế API và hiểu biết về các nguyên tắc kiến trúc microservices cho phát triển và triển khai nền tảng.
*   Quen thuộc với việc triển khai và quản lý ứng dụng cũng như các mô hình AI trên các nền tảng đám mây, cụ thể là AWS.
*   Quen thuộc với Git để phát triển mã nguồn cộng tác.

**3. Tiêu chí Đánh giá và Đo lường**

*   **Chức năng Prototype và Triển khai Kỹ thuật (Trọng số: 35%)**
    *   Tính đúng đắn & Độ chính xác: Đo lường được: Tỷ lệ thành công (%), Điểm chính xác.
    *   Độ bền & Xử lý lỗi: Đo lường được: Số lỗi không được xử lý gặp phải trong quá trình kiểm thử, Điểm đánh giá mã nguồn (đặc biệt đối với xử lý lỗi).
    *   Hiệu suất & Hiệu quả: Đo lường được: Thời gian thực thi trung bình cho mỗi tác vụ, các chỉ số sử dụng tài nguyên (nếu cơ sở hạ tầng cho phép).
    *   Chất lượng mã nguồn & Khả năng bảo trì: Đo lường được: Điểm đánh giá mã nguồn (trên thang điểm, ví dụ: 1-5), Chất lượng lịch sử commit Git.
    *   Tính phù hợp Kỹ thuật: Đo lường được: Điểm đánh giá của giám khảo dựa trên đánh giá mã nguồn và giải thích demo.
*   **Đổi mới và Sáng tạo (Trọng số: 25%)**
    *   Tính mới của Phương pháp tiếp cận: Đo lường được: Điểm đánh giá của giám khảo.
    *   Giải quyết Vấn đề một cách Sáng tạo: Đo lường được: Điểm đánh giá của giám khảo.
    *   Các Tính năng Giá trị Gia tăng: Đo lường được: Điểm đánh giá của giám khảo, Danh sách các Tính năng Giá trị Gia tăng.
*   **Khả năng Tái sử dụng và Tính Module hóa (Trọng số: 20%)**
    *   Thiết kế Tác nhân & Tính Module hóa: Đo lường được: Điểm đánh giá mã nguồn (đặc biệt đối với tính module hóa), Điểm đánh giá Kiến trúc.
    *   Tiềm năng Tái sử dụng: Đo lường được: Điểm đánh giá của giám khảo dựa trên thiết kế và các trường hợp sử dụng tiềm năng.
*   **Tác động Kinh doanh và Mức độ Liên quan (Trọng số: 10%)**
    *   Sự phù hợp với Nhu cầu Kinh doanh: Đo lường được: Điểm đánh giá của giám khảo dựa trên định nghĩa vấn đề và giải pháp đề xuất.
    *   Giá trị Kinh doanh Tiềm năng: Đo lường được: Điểm đánh giá của giám khảo dựa trên giá trị được trình bày rõ ràng.
*   **Thuyết trình và Giao tiếp (Trọng số: 10%)**
    *   Sự rõ ràng và Cấu trúc: Đo lường được: Điểm đánh giá của giám khảo.
    *   Hiệu quả Trình diễn: Đo lường được: Điểm đánh giá của giám khảo, Thành công của Demo trực tiếp.
    *   Hiểu biết và Hỏi & Đáp: Đo lường được: Điểm đánh giá của giám khảo trong quá trình Hỏi & Đáp.
*   **Thiết kế GUI và Trải nghiệm Người dùng (Trọng số: 10%)**
    *   Tính trực quan và Dễ sử dụng: Đo lường được: Điểm phản hồi của người dùng (định tính/định lượng), Điểm đánh giá của giám khảo.
    *   Khả năng Phản hồi và Tiếp cận: Điểm đánh giá của giám khảo.
    *   Luồng công việc và Điều hướng: Đo lường được: Điểm đánh giá của giám khảo.

**4. Sản phẩm Bàn giao**

Các sản phẩm sau đây được mong đợi từ mỗi đội tham gia để đánh giá:
*   **Prototype hoạt động:** Một bản triển khai chức năng của (các) Tác nhân GenAI và/hoặc ứng dụng đầu cuối giải quyết vấn đề kinh doanh đã chọn. Sản phẩm này phải có thể chạy và trình diễn được.
*   **Mã nguồn:** Toàn bộ mã nguồn cho prototype đã phát triển, lý tưởng nhất là được lưu trữ trên một kho lưu trữ có kiểm soát phiên bản (ví dụ: Git). Mã nguồn phải được chú thích rõ ràng và tổ chức tốt.
*   **Tài liệu Kỹ thuật:** Tài liệu giải thích kiến trúc của giải pháp, thiết kế của (các) Tác nhân GenAI, các công nghệ được sử dụng, hướng dẫn cài đặt và chạy prototype, và bất kỳ sự phụ thuộc nào.
*   **Báo cáo Dự án:** Một báo cáo bằng văn bản tóm tắt vấn đề được giải quyết, giải pháp đề xuất, chi tiết triển khai kỹ thuật, những thách thức gặp phải và cách chúng được khắc phục, các khía cạnh đổi mới, tác động kinh doanh tiềm năng và những cân nhắc về khả năng tái sử dụng trong GenAI Agent Studio.
*   **Slide Thuyết trình:** Các phương tiện trực quan được sử dụng trong quá trình thuyết trình, tóm tắt các điểm chính của báo cáo dự án và trình diễn.
*   **Demo (Tùy chọn nhưng Rất khuyến khích):** Một bản demo trực tiếp hoặc một video ngắn giới thiệu chức năng của prototype, có thể được sử dụng để sàng lọc hoặc đánh giá ban đầu.

---

**THỬ THÁCH CÔNG NGHỆ #23 (KHỐI EDA): XÂY DỰNG HỢP ĐỒNG DỮ LIỆU THÔNG MINH VỚI SỨC MẠNH TỪ GENAI**

**1. Tổng quan Thách thức**

Khi dữ liệu trở thành một tài sản quan trọng cho toàn bộ tổ chức, các nhóm ứng dụng và kinh doanh ngày càng phụ thuộc vào việc truy cập dữ liệu nhất quán và đáng tin cậy. Tuy nhiên, quy trình cung cấp dữ liệu hiện tại còn rời rạc, thường mang tính đặc thù (ad-hoc) và thiếu các giao diện chuẩn hóa giữa nhà sản xuất và người tiêu dùng dữ liệu. Hợp đồng Dữ liệu (Data Contracts), một khái niệm mới nổi gần đây, cho phép định nghĩa các thỏa thuận giữa nhà sản xuất và người tiêu dùng, bao gồm lược đồ (schema), SLA, quyền sở hữu và các chính sách quản trị cho các sản phẩm Dữ liệu. Tại VPBank, chúng tôi nhận thấy tiềm năng của Hợp đồng Dữ liệu trong việc thiết lập chia sẻ sản phẩm Dữ liệu hiệu quả và đáng tin cậy giữa các đơn vị.

Thử thách này yêu cầu các đội tận dụng Generative AI trong việc quản lý vòng đời của Hợp đồng Dữ liệu, bao gồm:
*   Cho phép tạo hợp đồng dữ liệu thông qua các yêu cầu bằng ngôn ngữ tự nhiên, chỉ định các loại dữ liệu, trường dữ liệu và tần suất sử dụng... với việc tạo tự động dựa trên một định dạng được xác định trước. Các đội có thể đề xuất các định dạng hợp đồng dữ liệu phù hợp.
*   Cung cấp hỗ trợ truy vấn dữ liệu dựa trên các Hợp đồng Dữ liệu có sẵn.
*   Thể hiện khả năng tích hợp mạnh mẽ với các công cụ giám sát việc thực thi hợp đồng dữ liệu trong quá trình chia sẻ dữ liệu qua các pipeline xử lý theo lô (batch) và xử lý luồng (streaming).
*   Cung cấp thông tin chi tiết và báo cáo về hiệu suất thực thi hợp đồng dữ liệu trong suốt vòng đời của nó.
*   Các tính năng được cung cấp, chẳng hạn như tạo, truy vấn và báo cáo, được triển khai thông qua các giao diện API (GraphQL hoặc REST).

**Yêu cầu Nâng cao:** Giải pháp nên tính đến khả năng hỗ trợ cả Sản phẩm Dữ liệu truyền thống và Sản phẩm Dữ liệu Mô phỏng (Simulated Data Products - cung cấp dữ liệu gần với chất lượng sản xuất, dành cho các trường hợp sử dụng ML hoặc BI nơi độ chính xác tuyệt đối không phải là thiết yếu).

**2. Yêu cầu Kỹ thuật**

Không có giới hạn về công nghệ. Tuy nhiên, chúng tôi quan tâm đến việc tận dụng các công nghệ sau:
*   API Frameworks: GraphQL, REST (Express.js, FastAPI)
*   Dịch vụ đám mây: AWS
*   Mô hình AI: Bất kỳ LLM nào (OpenAI, Claude, Cohere, hoặc mã nguồn mở như LLaMA)
*   Khác: Docker, Kubernetes

**3. Tiêu chí Đánh giá và Đo lường**

Các tiêu chí đánh giá chính bao gồm:
*   **Mức độ Liên quan và Đổi mới:** Giải pháp thể hiện sự hiểu biết rõ ràng về thách thức và áp dụng GenAI một cách sáng tạo để giải quyết các vấn đề thực tế liên quan đến Hợp đồng Dữ liệu.
*   **Triển khai Đầu cuối:** Bài dự thi bao gồm toàn bộ vòng đời của một hợp đồng dữ liệu, bao gồm tạo, xác thực và giám sát, trong một môi trường dữ liệu phân tán.
*   **Tính Khả thi Kỹ thuật và Khả năng Mở rộng:** Giải pháp cho thấy kiến trúc vững chắc, khả thi về mặt kỹ thuật và có thể mở rộng trong môi trường cấp doanh nghiệp. Các tiêu chí như dễ triển khai và tích hợp, đảm bảo hiệu suất và bảo mật sẽ được đánh giá cao.
*   **Thuyết trình và Trình diễn:** Bài dự thi bao gồm một bài thuyết trình rõ ràng và chuyên nghiệp, kèm theo một bản demo hoặc hướng dẫn chi tiết.

**4. Sản phẩm Bàn giao**

*   **Mã nguồn & Tài liệu:** Toàn bộ mã nguồn giải pháp với hướng dẫn cài đặt rõ ràng và tài liệu làm nổi bật các thành phần GenAI và Hợp đồng Dữ liệu. Kiến trúc hệ thống hiển thị các thành phần chính và tương tác.
*   **Slide thuyết trình:** Bài thuyết trình tóm tắt bao gồm vấn đề, giải pháp, bộ công nghệ (tech stack) và tác động.
*   **Demo:** Một video ngắn trình diễn luồng công việc của giải pháp và các tính năng chính.
*   **Các Trường hợp Sử dụng Mẫu:** Các kịch bản ví dụ hoặc các trường hợp kiểm thử minh họa việc tạo, xác thực và giám sát hợp đồng.

---

**THỬ THÁCH CÔNG NGHỆ #24 (KHỐI EDA): CÔNG CỤ QUY TẮC KIỂM TRA ĐỘ CHÍNH XÁC DỮ LIỆU (DATA ACCURACY TEST RULE ENGINE)**

**1. Tổng quan Thách thức**

Đây là một thử thách được thiết kế cho Kỹ sư Dữ liệu (Data Engineer). Thông qua thử thách này, chúng tôi dự định tìm kiếm một ứng viên tài năng trẻ với các kỹ năng được đề cập trong mục 5 (Yêu cầu Kỹ thuật) và khả năng phân tích, thiết kế một giải pháp có thể triển khai để đáp ứng các yêu cầu được đề cập trong Phần này.

**Quy trình Phát triển & Kiểm thử Kỹ thuật Dữ liệu:**
Quy tắc nghiệp vụ (Business Rule) → Phát triển Data job(s) và pipeline(s) (Thiết kế + code) để xử lý dữ liệu theo quy tắc nghiệp vụ → Script kiểm thử để kiểm tra quy tắc nghiệp vụ → Tổng hợp thành báo cáo kiểm thử dữ liệu.

**Nhu cầu về Công cụ Quy tắc Kiểm tra Độ chính xác Dữ liệu:**
Hiện tại, các script kiểm thử ở bước 3 đang được phát triển một cách đặc thù/riêng lẻ/riêng biệt để xác minh các quy tắc nghiệp vụ cụ thể và phức tạp.

Chúng tôi cần một Công cụ Quy tắc Kiểm tra Độ chính xác Dữ liệu sẽ giúp:
*   Hỗ trợ các toán tử Boolean để tạo các quy tắc phức tạp bằng cách kết hợp các quy tắc được phân loại và đơn giản hóa, tức là một quy tắc phức tạp sẽ được hình thành từ các quy tắc được phân loại và đơn giản hóa bằng một biểu thức Boolean, ví dụ: `<Quy tắc được phân loại và đơn giản hóa 1> <TOÁN TỬ BOOLEAN A> <Quy tắc được phân loại và đơn giản hóa 2> <TOÁN TỬ BOOLEAN B> <Quy tắc được phân loại và đơn giản hóa 3>`. Kết quả cuối cùng của một quy tắc phức tạp sẽ là TRUE hoặc FALSE để thể hiện việc kiểm tra logic là ĐẠT hoặc KHÔNG ĐẠT.
*   Hỗ trợ danh sách các quy tắc được phân loại và đơn giản hóa sau:
    1.  **Kiểu quy tắc:** Phạm vi giá trị (Value range)
        *   **Áp dụng trên:** Một cột
        *   **Mô tả:** Xác thực dữ liệu theo các phạm vi dự kiến.
        *   **Ví dụ:** Từ... đến... (Tham khảo 8. Phụ lục)
    2.  **Kiểu quy tắc:** Mẫu giá trị (Value template)
        *   **Áp dụng trên:** Một cột
        *   **Mô tả:** Xác thực các mẫu biểu thức chính quy (regex).
        *   **Ví dụ:** Số điện thoại, định dạng email... (Tham khảo 8. Phụ lục)
    3.  **Kiểu quy tắc:** Tính liên tục/toàn vẹn dữ liệu (Data continuity/integrity)
        *   **Áp dụng trên:** Một cột
        *   **Mô tả:** Xác thực tính liên tục/toàn vẹn của dữ liệu.
        *   **Ví dụ:** Dấu thời gian (Timestamp) hoặc ID theo thứ tự (Tham khảo 8. Phụ lục)
    4.  **Kiểu quy tắc:** So sánh 2 nhóm cột Thống kê và Số học giống nhau (trong các bảng khác nhau)
        *   **Áp dụng trên:** Các cột
        *   **Mô tả:** Hỗ trợ cùng một phép tính thống kê và số học trên 2 nhóm cột, sau đó so sánh kết quả (=, !=, >, <).
        *   **Ví dụ:** Cùng min, max, average (Tham khảo 8. Phụ lục)
    5.  **Kiểu quy tắc:** So sánh 2 nhóm cột Thống kê và Số học khác nhau (trong các bảng khác nhau)
        *   **Áp dụng trên:** Các cột
        *   **Mô tả:** Hỗ trợ các phép tính thống kê và số học khác nhau trên 2 nhóm cột, sau đó so sánh kết quả (=, !=, >, <).
        *   **Ví dụ:** Tổng của một nhóm cột = một cột khác (Tham khảo 8. Phụ lục)

*   Tạo các script SQL được tham số hóa để xác thực mọi quy tắc được phân loại và đơn giản hóa trên nhiều bảng và cột dữ liệu. Các loại tham số chính bao gồm:
    1.  Các TỪ KHÓA (KEYWORDS) tham chiếu đến ví dụ: tên của các hàm để tính toán Thống kê và Số học (danh sách các hàm sẽ bao gồm các hàm dựng sẵn được hỗ trợ bởi công cụ cơ sở dữ liệu (tham khảo 5. Yêu cầu Kỹ thuật) hoặc các hàm do người dùng định nghĩa được tải vào công cụ cơ sở dữ liệu, script của các hàm do người dùng định nghĩa sẽ nằm ngoài phạm vi của công cụ quy tắc này, các script SQL được tạo trong công cụ quy tắc này là để gọi các hàm).
    2.  Các TÊN (NAMES) tham chiếu đến các đối tượng dữ liệu cụ thể như schema, bảng và cột và tuân theo không gian tên của công cụ cơ sở dữ liệu, ví dụ: tham chiếu đến một cột cụ thể bằng `<schema>.<table>.<column>`.
    3.  Các GIÁ TRỊ (VALUES) tham chiếu đến các giá trị hằng số được định dạng cho các kiểu dữ liệu khác nhau, bao gồm danh sách, dữ liệu liệt kê hoặc tập hợp tương thích với công cụ cơ sở dữ liệu.

**2. Yêu cầu Kỹ thuật**

Prototype cần tương thích với bộ công nghệ sau:
*   Ngôn ngữ lập trình: SQL, Python.
*   Hỗ trợ ít nhất 1 công cụ cơ sở dữ liệu: DB2, PostgreSQL, AWS Redshift, MSSQL, Oracle.

**3. Tiêu chí Đánh giá và Đo lường**

*   **Hiệu suất:**
    *   Tốc độ xử lý: Thời gian cần thiết để thực thi một bộ quy tắc.
    *   Độ trễ: Độ trễ trong việc thực thi các quy tắc trong thời gian thực.
*   **Tính linh hoạt:**
    *   Tùy chỉnh quy tắc dễ dàng: các quy tắc được cấu hình bằng tham số (tức là không thay đổi mã nguồn).
    *   Định nghĩa quy tắc bằng JSON, XML, thủ tục/hàm SQL lưu trữ hoặc ngôn ngữ mô tả.
*   **Khả năng mở rộng:** Công cụ có thể xử lý một số lượng lớn các quy tắc.

**4. Sản phẩm Bàn giao**

*   Tài liệu thiết kế nguyên lý.
*   Demo prototype (bao gồm mã nguồn và demo đang chạy).

---

**THỬ THÁCH CÔNG NGHỆ #25 (KHỐI EDA): TỰ ĐỘNG HÓA DÒNG DỮ LIỆU (DATA LINEAGE AUTOMATION)**

**1. Tổng quan Thách thức**

Tổ chức quản lý các mối quan hệ bảng nghiệp vụ thông qua các tài liệu quy trình và yêu cầu, nhưng các mối quan hệ này không được ghi lại chính thức trong các mô hình dữ liệu. Kết quả là, các nhóm kỹ thuật & kinh doanh thường dựa vào việc diễn giải thủ công, dẫn đến sự không nhất quán. Ngân hàng muốn tự động hóa việc tạo ra các mối quan hệ logic nghiệp vụ của bảng, bao gồm các khóa kỹ thuật/nghiệp vụ, dựa trên logic nghiệp vụ được mô tả trong tài liệu để cải thiện độ chính xác và điều chỉnh thiết kế dữ liệu với nhu cầu kinh doanh thực tế. Nói tóm lại, chúng tôi muốn có công cụ hỗ trợ tự động (AI) tạo mối quan hệ bảng, mối quan hệ nghiệp vụ, với các khóa từ quy trình/tài liệu.

**2. Yêu cầu Kỹ thuật**

Prototype nên:
*   Ngôn ngữ lập trình: Nodejs, Python, Typescript.
*   Dịch vụ đám mây: AWS.
*   Mô hình AI: bất kỳ LLM nào.

**3. Tiêu chí Đánh giá và Đo lường**

Prototype nên có các khả năng sau:
*   Tự động phát hiện các thực thể kinh doanh và mối quan hệ từ tài liệu.
*   Tạo khóa logic dựa trên quy tắc nghiệp vụ.
*   Trực quan hóa các mối quan hệ thực thể một cách rõ ràng.
*   Độ chính xác của mối quan hệ: >90%.
*   Độ chính xác của khóa: >85%.
*   Thời gian xử lý tài liệu: <10 giây.
*   Phạm vi bao phủ thực thể kinh doanh: ≥90%.
*   Sự hài lòng của người dùng: ≥80% từ góc độ quản trị dữ liệu.

**4. Sản phẩm Bàn giao**

*   Bài thuyết trình.
*   Demo Prototype.

---

**THỬ THÁCH CÔNG NGHỆ #26 (KHỐI EDA): TỰ ĐỘNG HÓA TỪ ĐIỂN DỮ LIỆU NGHIỆP VỤ (AUTO BUSINESS DATA DICTIONARY)**

**1. Tổng quan Thách thức**

Tổ chức có thuật ngữ nghiệp vụ được lưu trữ trong tài liệu (ví dụ: BRDs và SRSs), nhưng thông tin này tĩnh, rời rạc và khó truy cập. Kết quả là, các nhóm gặp khó khăn trong việc hiểu các quy trình và định nghĩa nghiệp vụ. Mục tiêu là xây dựng một công cụ tự động trích xuất và cấu trúc các thuật ngữ nghiệp vụ từ các tài liệu này thành một từ điển dữ liệu tập trung, cải thiện sự rõ ràng và sự đồng nhất giữa các nhóm.

**2. Yêu cầu Kỹ thuật**

Prototype nên:
*   Ngôn ngữ lập trình: Nodejs, Python, Typescript...
*   Dịch vụ đám mây: AWS.
*   Mô hình AI: bất kỳ LLM nào.

**3. Tiêu chí Đánh giá và Đo lường**

Prototype sẽ được đánh giá dựa trên khả năng trích xuất và quản lý các thuật ngữ và quy trình nghiệp vụ từ tài liệu (BRD/SRS). Các tiêu chí đo lường chính bao gồm:
*   Độ chính xác trích xuất: ≥ 90% đối với thuật ngữ nghiệp vụ, ≥ 85% đối với quy trình.
*   Hiệu quả loại bỏ trùng lặp: ≥ 90% các thuật ngữ tương tự được chuẩn hóa.
*   Điểm khả dụng: ≥ 80% sự hài lòng của người dùng từ khảo sát phản hồi.
*   Hiệu suất tìm kiếm: < 2 giây để truy xuất một thuật ngữ.
*   Kiểm soát phiên bản: Hỗ trợ theo dõi ≥ 3 phiên bản của mỗi thuật ngữ.
*   Khả năng xuất: Xuất từ điển sang Excel hoặc JSON.
*   Phạm vi bao phủ nghiệp vụ: ≥ 90% các lĩnh vực nghiệp vụ liên quan được nắm bắt.
Việc đánh giá sẽ bao gồm kiểm thử chức năng và xác thực người dùng dựa trên một bộ tiêu chuẩn được quản lý.

**4. Sản phẩm Bàn giao**

*   Bài thuyết trình.
*   Demo Prototype.
