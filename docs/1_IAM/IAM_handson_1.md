# **Lộ Trình Hands-On Lab Chuyên Sâu Để Trở Thành Chuyên Gia AWS IAM Hàng Đầu**

**Giới thiệu: Con Đường Chinh Phục Đỉnh Cao AWS IAM**

* **Dấn Thân Vào Hành Trình Trở Thành Chuyên Gia Đẳng Cấp Thế Giới:**  
  * Chào mừng đến với hành trình nâng cao này. Lộ trình này không dành cho những người yếu tim; nó dành cho những ai khao khát trở thành một trong những chuyên gia AWS IAM hàng đầu thế giới. Chúng ta sẽ đi sâu, thách thức các giả định và xây dựng các kỹ năng thực tế, đã được tôi luyện qua thử thách.  
  * Bối cảnh mối đe dọa không ngừng phát triển đòi hỏi các chuyên gia IAM có khả năng kiến trúc, triển khai và bảo vệ các môi trường đám mây phức tạp. Chương trình này được thiết kế để rèn giũa những chuyên gia như vậy.  
* **Vai Trò Không Thể Thiếu Của Các Bài Thực Hành Hands-On Đầy Thách Thức:**  
  * Kiến thức lý thuyết là nền tảng, nhưng sự thành thạo thực sự đến từ việc áp dụng. Các bài thực hành trong lộ trình này được thiết kế phức tạp, phản ánh sự phức tạp của các kịch bản doanh nghiệp thực tế. 1  
  * Mỗi bài thực hành sẽ thúc đẩy người học giải quyết vấn đề, khắc phục sự cố và tư duy phản biện về các tác động bảo mật, vượt ra ngoài các bài tập "click-through" đơn giản.  
* **Triết Lý Chỉ Đạo: Đặc Quyền Tối Thiểu, Zero Trust và Bảo Mật Chủ Động:**  
  * **Đặc Quyền Tối Thiểu (Least Privilege):** Đây là nền tảng của bảo mật IAM. Chúng ta sẽ khám phá cách chỉ cấp các quyền cần thiết cho mọi danh tính và tác vụ, giảm thiểu bán kính ảnh hưởng tiềm ẩn của bất kỳ sự kiện bảo mật nào. 3  
  * **Zero Trust:** Giả định vi phạm. Mặc định không tin tưởng bất kỳ ai. Chúng ta sẽ khám phá cách áp dụng các nguyên tắc Zero Trust vào IAM, xác minh mọi yêu cầu truy cập và đảm bảo xác thực liên tục. 7  
  * **Bảo Mật Chủ Động:** Vượt qua các biện pháp phản ứng để dự đoán và giảm thiểu các mối đe dọa thông qua thiết kế mạnh mẽ, giám sát liên tục và tuân thủ tự động hóa.

**Module 1: Kiến Trúc và Đánh Giá Chính Sách IAM Nâng Cao**

* **Vượt Ra Ngoài Kiến Thức Cơ Bản: Tìm Hiểu Sâu Sắc về Danh Tính IAM và Cấu Trúc Chính Sách:**  
  * **Xem Xét Lại Danh Tính IAM:** Mặc dù người học đã biết về Users, Groups và Roles, chúng ta sẽ khám phá ứng dụng tinh tế của chúng trong các kiến trúc phức tạp. Users dành cho các cá nhân (lý tưởng nhất là được liên kết), Groups để quản lý quyền của người dùng, và Roles dành cho các thực thể đáng tin cậy (dịch vụ, ứng dụng, truy cập liên tài khoản) tận dụng thông tin xác thực tạm thời. 12  
    * Việc chuyển hướng sang sử dụng IAM roles hoặc người dùng liên kết để truy cập thay cho thông tin xác thực IAM user dài hạn là một xu hướng quan trọng, được thúc đẩy bởi các thực hành bảo mật tốt nhất. 12 Điều này làm giảm bề mặt tấn công liên quan đến các khóa tĩnh, tồn tại lâu dài. Thông tin xác thực tĩnh (khóa truy cập người dùng) là mục tiêu chính của những kẻ tấn công. Nếu bị xâm phạm, chúng cung cấp quyền truy cập liên tục. Thông tin xác thực tạm thời, có được bằng cách đảm nhận vai trò, có thời gian tồn tại giới hạn, giảm đáng kể rủi ro này. Do đó, các mẫu kiến trúc nên ưu tiên vai trò cho tất cả các loại truy cập nếu khả thi.  
  * **Tìm Hiểu Sâu Sắc Cấu Trúc Chính Sách:** Hiểu rõ các yếu tố Version, Statement, Sid, Effect (Allow, Deny), Principal, Action, Resource, và Condition ở cấp độ chuyên gia. 13  
    * Yếu tố Condition là nơi triển khai phần lớn khả năng kiểm soát chi tiết và bảo mật theo ngữ cảnh trong các chính sách IAM. Việc thành thạo các khóa điều kiện là tối quan trọng đối với IAM nâng cao. Đơn giản chỉ cho phép hoặc từ chối hành động trên tài nguyên thường quá thô sơ. Các điều kiện cho phép chính sách trở nên linh hoạt, phản ứng với các yếu tố như IP nguồn, thời gian trong ngày, trạng thái MFA hoặc các thẻ cụ thể. Điều này cho phép các chính sách vừa an toàn vừa linh hoạt.  
  * **Managed Policies vs. Inline Policies:** Các lựa chọn chiến lược cho khả năng tái sử dụng, sự rõ ràng và bảo trì. AWS khuyến nghị managed policies để tái sử dụng, nhưng inline policies rất quan trọng đối với các quyền được liên kết chặt chẽ, không thể tái sử dụng. 12  
    * Mặc dù các AWS-managed policies mang lại sự tiện lợi và được AWS cập nhật, đôi khi chúng có thể quá rộng rãi. Để đạt được đặc quyền tối thiểu thực sự, các customer-managed policies được điều chỉnh cho các nhu cầu cụ thể thường vượt trội hơn, đặc biệt đối với các khối lượng công việc quan trọng. 12 Các AWS-managed policies như AdministratorAccess cấp các quyền sâu rộng. Mặc dù hữu ích cho việc thiết lập ban đầu hoặc các vai trò rộng, chúng vi phạm nguyên tắc đặc quyền tối thiểu trong hầu hết các kịch bản hoạt động. Việc tạo các customer-managed policies buộc phải xem xét kỹ lưỡng các quyền cần thiết.  
* **Làm Chủ Logic Đánh Giá Chính Sách: Deny Rõ Ràng, Điều Kiện và Khóa Ngữ Cảnh:**  
  * Logic đánh giá chính sách IAM rất quan trọng: một Deny rõ ràng luôn ghi đè một Allow. Nếu không có Deny rõ ràng và có ít nhất một Allow áp dụng, yêu cầu sẽ được cho phép. Nếu không, nó sẽ bị từ chối ngầm định (mặc định từ chối). 15  
  * Hiểu cách nhiều chính sách (identity-based, resource-based, permissions boundaries, session policies, SCPs) tương tác và được đánh giá.  
  * Tìm hiểu sâu về các khóa điều kiện toàn cục (ví dụ: aws:SourceIp, aws:PrincipalTag, aws:MultiFactorAuthPresent) và các khóa điều kiện dành riêng cho dịch vụ. 14  
    * Sự tương tác của các loại chính sách khác nhau và thứ tự đánh giá nghiêm ngặt (đặc biệt là sức mạnh của một Deny rõ ràng) thường là nguồn gốc của sự nhầm lẫn và cấu hình sai. Một sự hiểu biết rõ ràng ở đây là rất quan trọng để khắc phục sự cố. Một người dùng có thể có Allow trong identity-based policy, nhưng nếu resource-based policy, SCP hoặc session policy chứa Deny rõ ràng cho cùng một hành động, hành động đó sẽ bị từ chối. Hệ thống phân cấp này là nền tảng để thiết kế bảo mật theo lớp.  
* **Ứng Dụng Chiến Lược của Identity-based Policies vs. Resource-based Policies:**  
  * Identity-based policies được đính kèm với các danh tính IAM (users, groups, roles) và xác định *những gì chúng có thể làm*. 12  
  * Resource-based policies được đính kèm với các tài nguyên (ví dụ: S3 buckets, SQS queues, KMS keys) và xác định *ai có thể truy cập tài nguyên này và họ có thể làm gì với nó*. 14  
  * Các trường hợp sử dụng phổ biến cho resource-based policies: truy cập liên tài khoản, cấp quyền truy cập ẩn danh (cẩn trọng), xác định quyền truy cập dịch vụ vào tài nguyên. 19  
    * Resource-based policies là công cụ mạnh mẽ để kiểm soát chi tiết các tài nguyên riêng lẻ và rất cần thiết cho các kịch bản chia sẻ liên tài khoản. Chúng có thể đơn giản hóa việc quản lý truy cập bằng cách cho phép chủ sở hữu tài nguyên trực tiếp cấp quyền. 19 Thay vì một nhóm IAM trung tâm quản lý tất cả các quyền cho một tài nguyên, chủ sở hữu tài nguyên (ví dụ: chủ sở hữu S3 bucket) có thể xác định ai (ngay cả từ các tài khoản khác) có thể truy cập tài nguyên cụ thể của họ. Việc ủy quyền này có thể linh hoạt hơn nhưng đòi hỏi kiểm tra cẩn thận để ngăn chặn các resource-based policies quá rộng rãi.  
  * **Bảng: Các Loại Chính Sách IAM \- Phân Tích So Sánh**

| Loại Chính Sách | Đặc Điểm Chính | Trường Hợp Sử Dụng Chính | Điểm Nổi Bật Logic Đánh Giá |
| :---- | :---- | :---- | :---- |
| Identity-based Policy | Gắn với user, group, role. Xác định những gì danh tính có thể làm. | Cấp quyền cho người dùng, vai trò thực hiện các tác vụ cụ thể trong tài khoản. | Được đánh giá cùng với các loại chính sách khác. Allow có thể bị Deny từ các chính sách khác ghi đè. |
| Resource-based Policy | Gắn với tài nguyên (S3 bucket, SQS queue, KMS key). Xác định ai có thể truy cập tài nguyên và làm gì với nó. | Chia sẻ tài nguyên liên tài khoản, cấp quyền truy cập ẩn danh (cẩn trọng), truy cập dịch vụ. | Hoạt động song song với identity-based policies. Một Allow ở một trong hai là đủ (trừ khi có Deny rõ ràng). |
| Permissions Boundary | Gắn với user hoặc role. Đặt giới hạn quyền tối đa mà identity-based policy có thể cấp. Không tự cấp quyền. | Ủy quyền quản lý vai trò cho developer, ngăn chặn leo thang đặc quyền. | Giới hạn hiệu lực của identity-based policy. Không ảnh hưởng đến resource-based policy. |
| Session Policy | Được truyền khi tạo phiên tạm thời (ví dụ: sts:AssumeRole). Giới hạn quyền của phiên tạm thời. | Truy cập đặc quyền tối thiểu cho người dùng liên kết, quyền theo ngữ cảnh cụ thể cho tác vụ. | Quyền hiệu lực là giao của identity-based policy của vai trò và session policy. Không thể cấp quyền vượt quá vai trò. |
| SCP (Service Control Policy) | Một phần của AWS Organizations. Đặt giới hạn quyền tối đa cho các tài khoản thành viên. Không tự cấp quyền. | Quản trị tập trung, thực thi tuân thủ, ngăn chặn các hành động rủi ro cao trên toàn tổ chức. | Ghi đè tất cả các chính sách khác trong tài khoản thành viên (ngoại trừ tài khoản quản lý). Deny trong SCP là tuyệt đối. |

    \*Lưu ý:\* Người học thường gặp khó khăn trong việc phân biệt khi nào nên sử dụng loại chính sách nào và cách chúng tương tác. Một cái nhìn tổng quan so sánh rõ ràng sẽ củng cố sự hiểu biết về vai trò riêng biệt và tác động đánh giá của chúng. Bảng này sẽ đóng vai trò là tài liệu tham khảo nhanh và là điểm tựa khái niệm, cho phép người học đưa ra quyết định sáng suốt hơn khi thiết kế các giải pháp IAM. Nó trực tiếp giải quyết nhu cầu làm rõ các cấu trúc IAM phức tạp.

* **Permissions Boundaries: Xác Định và Thực Thi Các Rào Chắn Đặc Quyền Tối Đa:**  
  * Mục đích: Đặt các quyền tối đa mà một identity-based policy có thể cấp cho một IAM user hoặc role. Chúng không tự cấp quyền mà hoạt động như một giới hạn trần. 20  
  * Các trường hợp sử dụng: Ủy quyền tạo vai trò cho các nhà phát triển trong khi vẫn duy trì quyền kiểm soát trung tâm đối với các quyền tối đa cho phép, ngăn chặn leo thang đặc quyền. 21  
  * Quan trọng: Permissions boundaries *không* giới hạn resource-based policies. 20  
    * Permissions boundaries là một công cụ quan trọng để cho phép sự linh hoạt của nhà phát triển trong khi vẫn duy trì các rào cản bảo mật. Chúng cho phép các nhà phát triển tự phục vụ các IAM roles cho ứng dụng của họ nhưng đảm bảo các vai trò này không thể vượt quá giới hạn được xác định trước bởi một nhóm bảo mật trung tâm. Các tổ chức muốn trao quyền cho các nhà phát triển nhưng lo ngại về việc vô tình hoặc cố ý tạo ra các vai trò quá rộng rãi. Một permissions boundary được đính kèm với IAM user của nhà phát triển sẽ giới hạn các quyền mà họ có thể gán cho bất kỳ vai trò nào họ tạo ra, giới hạn hiệu quả bán kính ảnh hưởng tiềm ẩn. Đây là một cơ chế quan trọng để ủy quyền IAM có thể mở rộng và an toàn.  
* **Session Policies: Kiểm Soát Chi Tiết Đối Với Thông Tin Xác Thực Tạm Thời:**  
  * Được truyền khi tạo một phiên tạm thời theo chương trình cho một vai trò (ví dụ: qua sts:AssumeRole). 22  
  * Giới hạn quyền của phiên tạm thời. Các quyền hiệu lực là giao điểm của identity-based policies của vai trò và session policies. 23  
  * Các trường hợp sử dụng: Hạn chế quyền truy cập cho các tác vụ cụ thể, đặc quyền tối thiểu cho người dùng liên kết, quyền theo ngữ cảnh cụ thể. 22  
    * Session policies cung cấp một cơ chế mạnh mẽ để thu hẹp phạm vi quyền một cách linh hoạt cho một phiên cụ thể, ngay cả khi IAM role cơ bản có các quyền rộng hơn. Điều này rất quan trọng đối với quyền truy cập "just-in-time". Một IAM role có thể có quyền đọc/ghi vào nhiều S3 bucket. Tuy nhiên, đối với một tác vụ tự động cụ thể chỉ cần ghi vào bucket-a/task-specific-path/, một session policy có thể được truyền trong quá trình AssumeRole để hạn chế thông tin xác thực tạm thời chỉ cho hành động và đường dẫn tài nguyên đó. Điều này giảm đáng kể rủi ro nếu những thông tin xác thực tạm thời đó bị xâm phạm.  
* **Service Control Policies (SCPs): Quản Trị Tập Trung Trên Toàn Tổ Chức Của Bạn:**  
  * Là một phần của AWS Organizations, SCPs cung cấp quyền kiểm soát trung tâm đối với các quyền tối đa có sẵn cho tất cả IAM users và roles trong các tài khoản thành viên. 24  
  * SCPs không cấp quyền; chúng hoạt động như các rào chắn, xác định những hành động nào có thể hoặc không thể được ủy quyền trong một tài khoản. 24  
  * SCPs ảnh hưởng đến tất cả các principal trong các tài khoản thành viên nhưng *không* ảnh hưởng đến tài khoản quản lý. 24  
  * Chiến lược: Danh sách từ chối (cho phép theo mặc định, từ chối các hành động cụ thể) so với Danh sách cho phép (từ chối theo mặc định, cho phép các hành động cụ thể). Danh sách từ chối thường được khuyến nghị để tránh gián đoạn dịch vụ không chủ ý. 24  
    * SCPs là công cụ bao quát nhất để thực thi tuân thủ và các bất biến bảo mật trên toàn bộ một AWS Organization. Chúng rất cần thiết để ngăn chặn một số hành động có rủi ro cao hoặc đảm bảo rằng tất cả các tài khoản tuân thủ các yêu cầu bảo mật cơ bản (ví dụ: vô hiệu hóa các khu vực không được phê duyệt, ngăn chặn việc xóa ghi nhật ký bảo mật). Một tổ chức có thể muốn ngăn chặn bất kỳ IAM user hoặc role nào trong bất kỳ tài khoản thành viên nào (ngoại trừ các vai trò quản trị viên được ủy quyền cụ thể) vô hiệu hóa CloudTrail hoặc sửa đổi các cấu hình nhóm bảo mật quan trọng. Một SCP với Deny rõ ràng cho các hành động này, được áp dụng ở cấp OU hoặc gốc, sẽ thực thi điều này một cách tập trung, bất kể các chính sách IAM cục bộ trong các tài khoản thành viên.  
* **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Xây Dựng và Khắc Phục Sự Cố Chính Sách Nâng Cao**  
  * **Lab 1.1: Xây Dựng Chính Sách Đặc Quyền Tối Thiểu với Các Điều Kiện Phức Tạp.**  
    * **Kịch bản:** Thiết kế một chính sách IAM cho một vai trò được đảm nhận bởi một EC2 instance, cho phép nó đọc từ một tiền tố S3 cụ thể, ghi nhật ký vào một CloudWatch Log Group cụ thể, nhưng chỉ khi EC2 instance có một thẻ cụ thể (Environment=Prod) và yêu cầu bắt nguồn từ một VPC cụ thể.  
    * **Tại sao quan trọng:** Bài thực hành này buộc người học phải kết hợp nhiều khóa điều kiện (s3:prefix, aws:SourceVpc, ec2:ResourceTag/Environment) và hiểu cách các toán tử StringEquals, StringLike, v.v. hoạt động, củng cố khái niệm về các quyền nhận biết ngữ cảnh.  
  * **Lab 1.2: Khắc Phục Sự Cố "Access Denied" với Các Chính Sách Xung Đột.**  
    * **Kịch bản:** Tạo một kịch bản với một IAM user, một resource-based policy trên một SQS queue, một permissions boundary trên người dùng và một SCP tương tác để gây ra lỗi "Access Denied" không mong muốn. Người học phải sử dụng IAM policy simulator và CloudTrail để chẩn đoán nguyên nhân gốc rễ. 15  
    * **Tại sao quan trọng:** Việc khắc phục sự cố IAM trong thực tế thường liên quan đến việc phân tích các tương tác chính sách phức tạp. Bài thực hành này xây dựng các kỹ năng chẩn đoán quan trọng.  
  * **Lab 1.3: Triển Khai và Kiểm Tra Permissions Boundaries cho Quản Trị Được Ủy Quyền.**  
    * **Kịch bản:** Một nhóm bảo mật trung tâm muốn cho phép một trưởng nhóm phát triển tạo IAM roles cho các Lambda functions của nhóm họ. Tuy nhiên, các Lambda roles này không được có quyền vượt quá việc truy cập các DynamoDB tables và SQS queues cụ thể. Người học sẽ tạo một permissions boundary và một chính sách IAM cho trưởng nhóm để thực thi điều này. 20  
    * **Tại sao quan trọng:** Bài thực hành này cung cấp kinh nghiệm thực tế trong việc ủy quyền quản lý IAM một cách an toàn, một yêu cầu phổ biến của doanh nghiệp.  
  * **Lab 1.4: Bảo Mật Việc Đảm Nhận Vai Trò với Session Policies.**  
    * **Kịch bản:** Một IAM role có các quyền rộng để quản lý EC2 instances. Một script cần đảm nhận vai trò này nhưng chỉ được phép khởi động/dừng các instance được gắn thẻ Project=Blue. Người học sẽ tạo một session policy để được truyền trong quá trình AssumeRole nhằm thực thi điều này. 22  
    * **Tại sao quan trọng:** Minh họa việc định phạm vi quyền động trong thời gian chạy, một kỹ thuật mạnh mẽ cho đặc quyền tối thiểu.  
  * **Lab 1.5: Thực Thi Các Rào Chắn Tổ Chức với SCPs.**  
    * **Kịch bản:** Trong một thiết lập đa tài khoản, ngăn chặn các tài khoản thành viên khởi chạy EC2 instances bên ngoài các AMI được phê duyệt và vô hiệu hóa CloudTrail. Người học sẽ thiết kế và kiểm tra SCPs để thực thi các hạn chế này. 24  
    * **Tại sao quan trọng:** Dạy về quản trị tập trung và cách ngăn chặn các cấu hình rủi ro trên toàn tổ chức.

**Module 2: Xác Thực Hiện Đại, Liên Kết Danh Tính và Làm Chủ STS**

* **Tìm Hiểu Sâu Sắc về Liên Kết Danh Tính: OIDC và SAML 2.0 trong Các Kịch Bản Doanh Nghiệp:**  
  * **Khái Niệm Liên Kết Danh Tính:** Hệ thống tin cậy giữa một Nhà Cung Cấp Danh Tính (IdP) và AWS (Nhà Cung Cấp Dịch Vụ \- SP) để xác thực người dùng và cấp quyền truy cập AWS tạm thời. 28  
  * **OpenID Connect (OIDC):**  
    * Cơ chế: Sử dụng JWTs (JSON Web Tokens) do một IdP tương thích OIDC cấp (ví dụ: GitHub Actions, GitLab, Auth0, Okta). AWS IAM có thể được cấu hình như một nhà cung cấp OIDC. 30  
    * Quy trình: Ứng dụng/dịch vụ nhận JWT từ IdP \-\> Gọi sts:AssumeRoleWithWebIdentity với JWT \-\> STS xác thực JWT với nhà cung cấp OIDC \-\> STS cấp thông tin xác thực AWS tạm thời.  
    * Các Tham Số Chính cho Nhà Cung Cấp IAM OIDC: URL Nhà Cung Cấp, Đối Tượng (Client ID), Dấu Vân Tay (để xác thực chứng chỉ máy chủ). 30  
  * **SAML 2.0:**  
    * Cơ chế: Tiêu chuẩn dựa trên XML để trao đổi dữ liệu xác thực và ủy quyền giữa một IdP (ví dụ: ADFS, Okta, Azure AD) và AWS. 28  
    * Quy trình: Người dùng xác thực với IdP của công ty \-\> IdP gửi xác nhận SAML đến AWS \-\> AWS STS xác thực xác nhận \-\> sts:AssumeRoleWithSAML được gọi \-\> STS cấp thông tin xác thực AWS tạm thời.  
    * Cấu hình: Bao gồm việc thiết lập SAML IdP trong IAM, cấu hình tin cậy bên phụ thuộc và ánh xạ yêu cầu. 33  
  * **Lựa Chọn Giữa OIDC và SAML:** OIDC thường được ưu tiên cho các ứng dụng web/di động hiện đại và xác thực dịch vụ-đến-dịch vụ (như các pipeline CI/CD), trong khi SAML phổ biến cho liên kết danh tính nhân viên doanh nghiệp. 29  
  * **Bảng: Các Giao Thức Liên Kết \- OIDC vs. SAML cho AWS**

| Tính Năng | OIDC | SAML |
| :---- | :---- | :---- |
| Loại Token | JWT (JSON Web Token) | Xác nhận SAML (XML) |
| IdP Điển Hình | GitHub Actions, GitLab, Auth0, Okta (cho ứng dụng) | ADFS, Okta, Azure AD (cho nhân viên) |
| Độ Phức Tạp Cấu Hình AWS | Thường đơn giản hơn cho các tích hợp theo chương trình | Có thể phức tạp hơn trong việc thiết lập IdP và ánh xạ thuộc tính |
| Lệnh Gọi API STS Tương Ứng | sts:AssumeRoleWithWebIdentity | sts:AssumeRoleWithSAML |
| Trường Hợp Sử Dụng Phổ Biến | CI/CD pipelines, ứng dụng di động/web hiện đại, xác thực dịch vụ-đến-dịch vụ | Đăng nhập một lần (SSO) cho nhân viên doanh nghiệp, truy cập bảng điều khiển AWS |

    \*Lưu ý:\* Người học cần hiểu không chỉ OIDC và SAML là gì, mà còn \*khi nào và tại sao\* nên chọn cái này thay vì cái kia. Một bảng so sánh làm nổi bật các yếu tố khác biệt chính và các kịch bản ứng dụng điển hình. Điều này hỗ trợ việc ra quyết định kiến trúc, đảm bảo người học chọn giao thức liên kết phù hợp cho các yêu cầu doanh nghiệp khác nhau, một kỹ năng quan trọng đối với một chuyên gia IAM.  
\*   Khả năng truyền các thuộc tính người dùng (như trung tâm chi phí, chức danh, phòng ban) từ IdP đến AWS thông qua SAML hoặc SCIM với IAM Identity Center, và sau đó sử dụng các thuộc tính này cho Kiểm Soát Truy Cập Dựa Trên Thuộc Tính (ABAC), là một mẫu mạnh mẽ cho các quyền chi tiết và có thể mở rộng. \[29\] Việc quản lý quyền cho hàng ngàn người dùng riêng lẻ là không thể mở rộng. Nếu các thuộc tính người dùng đã được duy trì trong một IdP trung tâm, việc liên kết các thuộc tính này vào AWS cho phép các chính sách IAM đưa ra quyết định dựa trên các thuộc tính này (ví dụ: \`aws:PrincipalTag/department\`). Điều này tách rời logic quyền khỏi danh tính người dùng riêng lẻ và làm cho việc giới thiệu/loại bỏ/thay đổi vai trò trở nên đơn giản hơn nhiều (quản lý trong IdP, quyền tự động cập nhật trong AWS).

* **AWS STS Được Hé Lộ: Làm Chủ AssumeRole, GetFederationToken, và Thông Tin Xác Thực Tạm Thời:**  
  * **AWS Security Token Service (STS):** Dịch vụ cốt lõi để yêu cầu thông tin xác thực tạm thời, có đặc quyền giới hạn. 34  
  * **sts:AssumeRole:**  
    * Cho phép một danh tính IAM (user hoặc role) hoặc dịch vụ AWS nhận thông tin xác thực tạm thời cho một IAM role cụ thể. 34  
    * Yêu cầu một trust policy trên vai trò mục tiêu cho phép principal gọi đảm nhận nó. 34  
    * Các Tham Số Chính: RoleArn, RoleSessionName, Policy (cho session policies), DurationSeconds, ExternalId, SerialNumber (cho MFA). 34  
    * ExternalId là một cơ chế bảo mật quan trọng để ngăn chặn vấn đề "confused deputy", đặc biệt trong các kịch bản liên tài khoản nơi một bên thứ ba có thể bị lừa để đảm nhận một vai trò. 34 Nếu Tài khoản A tin cậy Tài khoản B để đảm nhận một vai trò, và một kẻ tấn công trong Tài khoản B biết ARN của vai trò đó, họ có thể cố gắng đảm nhận nó. Nếu vai trò trong Tài khoản A cũng yêu cầu một ExternalId (một bí mật duy nhất được chia sẻ giữa A và B), kẻ tấn công không thể đảm nhận vai trò nếu không biết ExternalId này, ngay cả khi principal IAM của họ trong Tài khoản B có quyền sts:AssumeRole.  
  * **sts:AssumeRoleWithWebIdentity & sts:AssumeRoleWithSAML:** Được sử dụng tương ứng cho liên kết OIDC và SAML, như đã thảo luận ở trên. 29  
  * **sts:GetFederationToken:**  
    * Cung cấp thông tin xác thực tạm thời cho một *người dùng liên kết*. Thường được sử dụng cho các nhà môi giới danh tính tùy chỉnh hoặc các kịch bản mà bạn muốn cấp quyền truy cập tạm thời dựa trên các quyền của một IAM user hiện có nhưng thu hẹp phạm vi của chúng. 35  
    * Các quyền là giao điểm của các chính sách của IAM user và bất kỳ chính sách nào được truyền trong lệnh gọi.  
  * **sts:GetSessionToken:**  
    * Cung cấp thông tin xác thực tạm thời cho một IAM user hoặc root user, thường được sử dụng để cấp quyền truy cập bảng điều khiển tạm thời hoặc cho các hoạt động yêu cầu MFA. 35  
  * **Thành Phần Thông Tin Xác Thực Tạm Thời:** Access Key ID, Secret Access Key, Session Token. Cả ba phải được sử dụng cho các lệnh gọi API. Chúng hết hạn sau một khoảng thời gian xác định. 34  
* **Sức Mạnh của sts:SetSourceIdentity: Đảm Bảo Trách Nhiệm Giải Trình trong Chuỗi Vai Trò Phức Tạp:**  
  * **Mục đích:** Cho phép một IAM role trung gian (vai trò "setter") đặt một "danh tính nguồn" khi nó đảm nhận một vai trò khác (vai trò "target"). Danh tính nguồn này sau đó được ghi lại trong CloudTrail cho các hành động được thực hiện bởi vai trò target. \[38 (không thể truy cập), 166 (không thể truy cập), 167 (không thể truy cập)\]  
  * **Các trường hợp sử dụng:** Quan trọng đối với việc kiểm toán và quy kết hành động trở lại người dùng người ban đầu hoặc danh tính dịch vụ ban đầu khi nhiều vai trò được xâu chuỗi, đặc biệt trong các tài khoản dịch vụ dùng chung hoặc tự động hóa phức tạp.  
  * **Cách hoạt động:**  
    * Trust policy của vai trò setter phải cho phép sts:SetSourceIdentity. 38  
    * Vai trò setter gọi sts:AssumeRole trên vai trò target, cung cấp tham số SourceIdentity.  
    * Trust policy của vai trò target có thể (và nên) có các điều kiện dựa trên sts:SourceIdentity để hạn chế ai có thể đặt danh tính nguồn nào. 38  
    * Resource policies trên các dịch vụ được truy cập bởi vai trò target cũng có thể sử dụng khóa điều kiện sts:SourceIdentity để đưa ra quyết định truy cập. 39  
  * **Ghi Nhật Ký CloudTrail:** Giá trị sts:SourceIdentity xuất hiện trong trường userIdentity.sourceIdentity trong nhật ký CloudTrail cho các hành động được thực hiện bởi vai trò target. 41  
    * sts:SetSourceIdentity là một yếu tố thay đổi cuộc chơi cho khả năng truy vết trong các kịch bản phức tạp như một pipeline CI/CD trung tâm đảm nhận các vai trò trong nhiều tài khoản ứng dụng, hoặc một Lambda dịch vụ dùng chung đảm nhận các vai trò để thực hiện các tác vụ thay mặt cho các đối tượng thuê/người dùng khác nhau. Nếu không có nó, nhật ký CloudTrail trong tài khoản đích sẽ chỉ hiển thị ARN vai trò CI/CD hoặc Lambda, che khuất trình kích hoạt hoặc người dùng ban đầu. Hãy tưởng tượng một vai trò pipeline CI/CD (PipelineDeployerRole) trong tài khoản Công cụ đảm nhận AppSpecificDeployRole trong tài khoản Sản xuất. Nếu PipelineDeployerRole gọi sts:AssumeRole trên AppSpecificDeployRole và đặt SourceIdentity thành "GitHubWorkflow:my-app-deploy", thì các hành động được thực hiện bởi AppSpecificDeployRole trong Sản xuất (ví dụ: ec2:RunInstances) sẽ có "GitHubWorkflow:my-app-deploy" trong trường sourceIdentity của CloudTrail. Điều này ngay lập tức cho kiểm toán viên biết nguồn gốc của hành động, thay vì chỉ nhìn thấy AppSpecificDeployRole.  
  * **Tương Tác với Session Policies và Resource-Based Policies:** Session policies được truyền trong quá trình gọi AssumeRole cho vai trò target vẫn được áp dụng. Resource-based policies trên các dịch vụ target có thể sử dụng Condition: {"StringEquals": {"sts:SourceIdentity": "expected-value"}} để đảm bảo chỉ các hành động có danh tính nguồn cụ thể mới được phép. 39  
  * **Điều kiện tiên quyết:** Vai trò thực hiện sts:SetSourceIdentity cần quyền sts:SetSourceIdentity trong identity policy *của chính nó*, và trust policy của vai trò *đang được đảm nhận* phải cho phép hành động sts:SetSourceIdentity từ principal đảm nhận nó. Principal đảm nhận vai trò cũng cần quyền sts:AssumeRole. 38  
* **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Triển Khai và Bảo Mật Các Mẫu Truy Cập Liên Kết**  
  * **Lab 2.1: Cấu Hình Liên Kết OIDC với GitHub Actions để Triển Khai AWS An Toàn.**  
    * **Kịch bản:** Thiết lập một nhà cung cấp IAM OIDC cho GitHub Actions. Tạo một IAM role tin cậy nhà cung cấp này và các kho lưu trữ/nhánh GitHub cụ thể. Cấp cho vai trò này các quyền đặc quyền tối thiểu để triển khai một Lambda function và cập nhật một S3 bucket. Cấu hình một quy trình làm việc GitHub Actions để đảm nhận vai trò này bằng cách sử dụng sts:AssumeRoleWithWebIdentity và thực hiện việc triển khai. 31  
    * **Tại sao quan trọng:** Đây là một mẫu rất phổ biến và an toàn cho CI/CD, loại bỏ nhu cầu về thông tin xác thực AWS tồn tại lâu dài trong GitHub.  
  * **Lab 2.2: Triển Khai Liên Kết SAML 2.0 với một IdP Giả Lập cho SSO Doanh Nghiệp.**  
    * **Kịch bản:** Mô phỏng một IdP của công ty (sử dụng một công cụ như SimpleSAMLphp hoặc một trình tạo phản hồi SAML giả lập). Cấu hình nhà cung cấp IAM SAML và các vai trò. Ánh xạ các thuộc tính SAML (ví dụ: phòng ban, vai trò) vào các thẻ phiên AWS. Tạo các chính sách IAM cấp quyền truy cập dựa trên các thẻ phiên này (ABAC). Kiểm tra đăng nhập SSO và truy cập dựa trên thuộc tính. 28  
    * **Tại sao quan trọng:** Cung cấp kinh nghiệm thực tế với liên kết danh tính nhân viên doanh nghiệp và ABAC, rất quan trọng đối với các tổ chức lớn.  
  * **Lab 2.3: Xâu Chuỗi Vai Trò Nâng Cao với sts:SetSourceIdentity để Kiểm Toán.**  
    * **Kịch bản:** Người dùng A đảm nhận Vai trò B (ví dụ: "JenkinsOrchestratorRole"). Vai trò B sau đó cần đảm nhận Vai trò C (ví dụ: "ProdEC2ManagementRole") trong một tài khoản khác để thực hiện một hành động. Triển khai sts:SetSourceIdentity để các hành động được thực hiện bởi Vai trò C được quy cho Người dùng A trong nhật ký CloudTrail. Cấu hình trust policies trên Vai trò C để chỉ cho phép Vai trò B đảm nhận nó nếu một mẫu SourceIdentity cụ thể được cung cấp. 36  
    * **Tại sao quan trọng:** Bài thực hành này giải quyết một thách thức kiểm toán và trách nhiệm giải trình phức tạp nhưng quan trọng trong các kịch bản đảm nhận vai trò nhiều bước. Nó minh họa ứng dụng thực tế của sts:SetSourceIdentity và các khóa điều kiện liên quan.  
  * **Lab 2.4: Bảo Mật sts:AssumeRole với Điều Kiện ExternalId và MFA.**  
    * **Kịch bản:** Cấu hình một vai trò liên tài khoản yêu cầu cả ExternalId và người dùng đảm nhận phải xác thực bằng MFA. Kiểm tra việc đảm nhận có và không có các điều kiện này được đáp ứng. 34  
    * **Tại sao quan trọng:** Củng cố các thực hành bảo mật tốt nhất cho việc đảm nhận vai trò, đặc biệt là vấn đề confused deputy và thực thi MFA.

**Module 3: IAM cho Các Workload Doanh Nghiệp Phức Tạp**

* **: Xây Dựng Pipeline CI/CD Đa Tài Khoản An Toàn (IAM Roles, OIDC, Temporary Security Credentials, Phân Tách Nhiệm Vụ, Khả Năng Kiểm Toán Toàn Diện)**  
  * **Mục tiêu:** Thiết kế và triển khai một pipeline CI/CD trải dài trên nhiều tài khoản AWS (ví dụ: Tài khoản Công cụ/CI-CD, Tài khoản Phát triển, Tài khoản Sản xuất) sử dụng AWS CodePipeline và AWS CodeBuild, với GitHub làm kho lưu trữ nguồn. Tập trung mạnh vào các thực hành tốt nhất của IAM:  
    * **Tài khoản Công cụ (ví dụ: Tài khoản A):**  
      * **CodePipeline Service Role:** Được cấp đặc quyền tối thiểu để điều phối pipeline, đọc từ nguồn (GitHub qua OIDC), lưu trữ các tạo phẩm trong một S3 bucket trung tâm (trong tài khoản Công cụ), và đảm nhận các vai trò trong tài khoản Dev/Prod cho các hành động triển khai. 51  
      * **IAM OIDC Provider cho GitHub:** Được cấu hình để thiết lập tin cậy với GitHub Actions. 43  
      * **GitHub Actions OIDC Role:** Được đảm nhận bởi các quy trình làm việc GitHub Actions (ví dụ: để kiểm tra mã, kiểm tra trước khi commit, hoặc kích hoạt CodePipeline). Vai trò này sẽ có các quyền tối thiểu, có thể chỉ để khởi động CodePipeline hoặc ghi vào một S3 bucket cho các tạo phẩm sơ bộ. Trust policy được điều kiện hóa theo tổ chức/kho lưu trữ/nhánh GitHub cụ thể. 43  
    * **Tài khoản Phát triển (ví dụ: Tài khoản B) & Tài khoản Sản xuất (ví dụ: Tài khoản C):**  
      * **Cross-Account Deployment Roles (ví dụ: DevDeployRole, ProdDeployRole):** Các vai trò này được đảm nhận bởi CodePipeline service role (từ tài khoản Công cụ) hoặc bởi các CodeBuild roles (chạy trong tài khoản Công cụ nhưng đảm nhận các vai trò này).  
        * Trust Policy: Tin cậy ARN của CodePipeline service role hoặc CodeBuild role từ tài khoản Công cụ. 56  
        * Permissions Policy: Cấp các quyền đặc quyền tối thiểu cần thiết cho CodeBuild để xây dựng ứng dụng (ví dụ: truy cập ECR, biên dịch các phụ thuộc) và cho CodeDeploy/CloudFormation/Lambda để triển khai ứng dụng (ví dụ: cloudformation:CreateStack, lambda:UpdateFunctionCode, s3:PutObject vào các bucket ứng dụng). Các quyền nên được định phạm vi chặt chẽ cho các tài nguyên cụ thể. 43  
      * **CodeBuild Service Role (nếu CodeBuild chạy trong Dev/Prod, ít phổ biến hơn cho mẫu này nhưng có thể):** Nếu các dự án CodeBuild được xác định trực tiếp trong tài khoản Dev/Prod (thay vì đảm nhận các vai trò từ một CodeBuild tài khoản Công cụ trung tâm), các vai trò này sẽ cần các quyền để truy cập các nguồn xây dựng, ghi các tạo phẩm, và tương tác với các dịch vụ như ECR, S3, v.v., trong các tài khoản tương ứng của chúng. 53  
    * **Artifact S3 Bucket (trong tài khoản Công cụ):**  
      * Bucket Policy: Cho phép PutObject từ CodePipeline service role và CodeBuild roles, và GetObject cho các deployment roles trong tài khoản Dev/Prod. Được mã hóa bằng KMS.  
    * **Phân Tách Nhiệm Vụ (Segregation of Duties):**  
      * Các nhà phát triển có thể commit mã vào GitHub nhưng không thể trực tiếp triển khai lên sản xuất hoặc sửa đổi các IAM roles sản xuất.  
      * Pipeline tự động hóa việc triển khai, với các vai trò riêng biệt cho mỗi giai đoạn/tài khoản.  
      * Xem xét các giai đoạn phê duyệt thủ công trong CodePipeline cho các lần triển khai sản xuất, yêu cầu một principal IAM riêng biệt có quyền codepipeline:PutApprovalResult. 54  
    * **Thông Tin Xác Thực Tạm Thời:** Tất cả các hành động trong pipeline (CodeBuild, triển khai) phải sử dụng thông tin xác thực tạm thời có được bằng cách đảm nhận các vai trò thích hợp. Không có khóa truy cập tồn tại lâu dài. 43  
    * **Khả Năng Kiểm Toán Toàn Diện:**  
      * Đảm bảo CloudTrail được bật trong tất cả các tài khoản, ghi nhật ký vào một S3 bucket trung tâm (tốt nhất là trong một tài khoản Lưu trữ Nhật ký chuyên dụng). 60  
      * Sử dụng sts:SetSourceIdentity nếu xảy ra xâu chuỗi vai trò (ví dụ: nếu CodePipeline đảm nhận một vai trò trung gian sau đó đảm nhận vai trò triển khai) để truy vết các hành động trở lại quá trình thực thi pipeline hoặc người dùng/trình kích hoạt ban đầu.  
      * CloudWatch Logs cho CodeBuild và CodePipeline. 62  
      * Triển khai các truy vấn CloudTrail/CloudWatch Logs Insights cụ thể để giám sát việc đảm nhận vai trò, thay đổi chính sách và các hành động triển khai quan trọng.  
  * **Tại sao quan trọng:** Bài thực hành này kết hợp OIDC, vai trò liên tài khoản, vai trò dịch vụ, đặc quyền tối thiểu và kiểm toán, đại diện cho một thách thức bảo mật CI/CD doanh nghiệp rất thực tế và phức tạp. Nó trực tiếp giải quyết yêu cầu của người dùng về phân tách nhiệm vụ và khả năng kiểm toán.  
  * **Bảng: Ma Trận Vai Trò IAM CI/CD Đa Tài Khoản**

| Tác Nhân/Dịch Vụ Chính | Tài Khoản | Trách Nhiệm Chính | Quyền Hạn Quan Trọng Cần Thiết (Ví dụ) | Mối Quan Hệ Tin Cậy (Ai có thể đảm nhận / Tin cậy điều gì) |
| :---- | :---- | :---- | :---- | :---- |
| CodePipeline Service Role | Công cụ | Điều phối pipeline, đọc nguồn, lưu trữ tạo phẩm, đảm nhận vai trò triển khai | codecommit:GetBranch, s3:GetObject, s3:PutObject (vào artifact bucket), sts:AssumeRole (đến các vai trò triển khai ở Dev/Prod) | Tin cậy codepipeline.amazonaws.com |
| GitHub OIDC Role | Công cụ | Được đảm nhận bởi GitHub Actions để kích hoạt pipeline hoặc các tác vụ tiền CI | codepipeline:StartPipelineExecution (nếu kích hoạt pipeline), s3:PutObject (nếu có tạo phẩm ban đầu) | Tin cậy nhà cung cấp OIDC của GitHub (token.actions.githubusercontent.com) với các điều kiện về repo/branch |
| CodeBuild Role (trong tài khoản Công cụ, đảm nhận vai trò ở Dev/Prod) | Công cụ | Thực hiện các bước xây dựng, kiểm thử | codebuild:StartBuild, logs:CreateLogGroup, logs:CreateLogStream, logs:PutLogEvents, s3:GetObject (tạo phẩm nguồn), sts:AssumeRole (để đảm nhận các vai trò ở Dev/Prod cho các hành động cụ thể) | Tin cậy codebuild.amazonaws.com |
| DevDeployRole | Phát triển | Được đảm nhận bởi CodePipeline/CodeBuild để triển khai vào môi trường Phát triển | cloudformation:CreateStack, lambda:UpdateFunctionCode, ec2:RunInstances (được định phạm vi cho tài nguyên Phát triển), s3:GetObject (từ artifact bucket) | Tin cậy ARN của CodePipeline Service Role hoặc CodeBuild Role từ tài khoản Công cụ |
| ProdDeployRole | Sản xuất | Được đảm nhận bởi CodePipeline/CodeBuild để triển khai vào môi trường Sản xuất | Tương tự DevDeployRole nhưng cho tài nguyên Sản xuất, có thể có các phê duyệt thủ công nghiêm ngặt hơn | Tin cậy ARN của CodePipeline Service Role hoặc CodeBuild Role từ tài khoản Công cụ |

    \*Lưu ý:\* Một cạm bẫy phổ biến trong CI/CD đa tài khoản là các trust policies quá rộng rãi trên các vai trò triển khai hoặc các quyền quá rộng trong các vai trò dịch vụ CodePipeline/CodeBuild. Bài thực hành này sẽ buộc phải xem xét cẩn thận những điều này. Một khía cạnh quan trọng khác là đảm bảo chính sách bucket tạo phẩm được cấu hình chính xác để cho phép ghi từ quy trình xây dựng và đọc từ các quy trình triển khai trong các tài khoản đích, đồng thời ngăn chặn truy cập trái phép. Nếu \`ProdDeployRole\` tin cậy \`arn:aws:iam::ToolsAccount:root\` thay vì ARN vai trò CodePipeline/CodeBuild cụ thể, bất kỳ principal nào trong tài khoản Công cụ có \`sts:AssumeRole\` đều có thể đảm nhận nó. Tương tự, nếu CodePipeline service role có \`iam:\*\` vì "dễ hơn", nó tạo ra một rủi ro bảo mật lớn. Bài thực hành này nhấn mạnh việc định phạm vi những điều này.

* **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Thiết Kế IAM Đặc Quyền Tối Thiểu cho Kiến Trúc Microservice Serverless Phức Tạp (API Gateway, Lambda, DynamoDB, SQS)**  
  * **Kịch bản:** Thiết kế IAM cho một ứng dụng microservices.  
    * Dịch vụ A: Điểm cuối API Gateway \-\> Lambda A (Python). Lambda A cần ghi vào DynamoDB Table A và gửi tin nhắn đến SQS Queue B.  
    * Dịch vụ B: Lambda B (Node.js) được kích hoạt bởi SQS Queue B. Lambda B cần đọc từ DynamoDB Table A và ghi vào DynamoDB Table C.  
    * Tất cả các tài nguyên (API Gateway, Lambdas, DynamoDB tables, SQS queue) phải được bảo mật bằng các IAM roles và resource policies đặc quyền tối thiểu nếu có thể. Triển khai các nguyên tắc Zero Trust.  
  * **Các Thành Phần IAM Cần Thiết Kế:**  
    * **Lambda Execution Roles:** Một cho Lambda A, một cho Lambda B. Mỗi vai trò chỉ nên có các quyền cho các hành động cụ thể trên các tài nguyên cụ thể mà chúng tương tác (ví dụ: lambda\_A\_role nhận dynamodb:PutItem trên ARN Bảng A, sqs:SendMessage trên ARN Hàng đợi B). 65  
    * **API Gateway Resource Policy (Tùy chọn nhưng được khuyến nghị cho Zero Trust):** Nếu có, hạn chế ai có thể gọi điểm cuối API Gateway (ví dụ: VPC cụ thể, IP nguồn, hoặc yêu cầu xác thực IAM). 68  
    * **SQS Queue Resource Policy:** Cho phép sqs:SendMessage từ vai trò thực thi của Lambda A. Cho phép sqs:ReceiveMessage, sqs:DeleteMessage, sqs:GetQueueAttributes cho vai trò thực thi của Lambda B. 70  
    * **DynamoDB Table IAM Policies (Ngầm định qua Execution Roles):** Quyền truy cập vào DynamoDB thường được kiểm soát thông qua identity-based policies của các Lambda execution roles.  
  * **Cân Nhắc Zero Trust:**  
    * Mỗi Lambda function có vai trò riêng, được định phạm vi hẹp.  
    * Resource policies trên SQS xác định rõ ràng nhà sản xuất và người tiêu dùng.  
    * Nếu API Gateway sử dụng ủy quyền IAM, đảm bảo người gọi có các quyền tối thiểu để gọi.  
    * Xem xét sử dụng các điều kiện như aws:SourceArn hoặc aws:SourceAccount trong resource policies nếu các dịch vụ ở các tài khoản khác nhau hoặc để ngăn chặn các vấn đề confused deputy.  
  * **Tại sao quan trọng:** Kiến trúc serverless rất phổ biến, và việc bảo mật giao tiếp giữa các dịch vụ thông qua IAM đặc quyền tối thiểu là rất quan trọng. Bài thực hành này minh họa việc cấp quyền chi tiết cho các tương tác microservice cụ thể. 3  
  * Một lỗi phổ biến là tạo một "Lambda-Execution-Role" duy nhất với các quyền rộng được nhiều hàm sử dụng. Bài thực hành này nhấn mạnh việc tạo một vai trò riêng biệt, được điều chỉnh cho *mỗi* Lambda function. Nếu Lambda A và Lambda B chia sẻ một vai trò, và Lambda A bị xâm phạm, kẻ tấn công cũng giành được các quyền của Lambda B. Bằng cách cô lập các quyền vào các vai trò riêng biệt, bán kính ảnh hưởng của một vụ xâm phạm hàm đơn lẻ được hạn chế. Đây là một ứng dụng trực tiếp của đặc quyền tối thiểu và phòng thủ theo chiều sâu.  
* **: Triển Khai Kiểm Soát Truy Cập Chi Tiết cho Hồ Dữ Liệu Quy Mô Lớn (ABAC với Lake Formation, S3 Bucket Policies, Resource Policies, AWS KMS, Quy Trình ETL Tự Động Hóa)**  
  * **Mục tiêu:** Bảo mật một hồ dữ liệu được xây dựng trên Amazon S3, sử dụng AWS Lake Formation để kiểm soát truy cập chi tiết, AWS Glue cho ETL, và AWS KMS để mã hóa. Triển khai Kiểm Soát Truy Cập Dựa Trên Thuộc Tính (ABAC) cho các nhà phân tích dữ liệu và nhà khoa học dữ liệu.  
  * **Các Thành Phần Kịch Bản:**  
    * **S3 Data Lake Buckets:** Các vùng Raw, Staged, Curated. Dữ liệu được mã hóa bằng Khóa KMS do Khách hàng Quản lý (CMKs). 75  
    * **AWS Glue Data Catalog:** Các cơ sở dữ liệu và bảng được xác định cho dữ liệu trong S3.  
    * **AWS Lake Formation:**  
      * Đăng ký các vị trí S3. 77  
      * Xác định Quản trị viên Hồ dữ liệu. 80  
      * Cấp quyền trên cơ sở dữ liệu, bảng và cột bằng cách sử dụng các quyền của Lake Formation (LF-Tags cho ABAC). 80  
      * Thu hồi quyền của IAMAllowedPrincipals để thực thi các quyền của Lake Formation. 81  
    * **AWS Glue ETL Jobs:**  
      * IAM Role cho Glue ETL Jobs: Cần các quyền để đọc từ các vị trí S3 nguồn (thông qua Lake Formation), ghi vào các vị trí S3 đích (thông qua Lake Formation), truy cập Glue Data Catalog, và sử dụng các khóa KMS liên quan (kms:Decrypt cho nguồn, kms:Encrypt, kms:GenerateDataKey cho đích). 80  
      * Vai trò công việc Glue sẽ cần các quyền của Lake Formation được cấp cho nó đối với các tài nguyên Data Catalog cụ thể mà nó cần truy cập/sửa đổi. 80  
    * **IAM Roles cho Nhà Phân Tích Dữ Liệu/Nhà Khoa Học Dữ Liệu (ví dụ: AnalystRole, ScientistRole):**  
      * Các vai trò này sẽ được gắn thẻ với các thuộc tính (ví dụ: Department:Sales, Project:Alpha, Region:US).  
      * Các quyền của Lake Formation sẽ được cấp dựa trên các LF-Tags này (ABAC). Ví dụ, cấp SELECT trên Sales\_Table cho các principal có thẻ Department:Sales. Hoặc cấp quyền truy cập vào các cột cụ thể dựa trên thẻ Sensitivity:PII. 83  
    * **KMS Key Policies:**  
      * Đảm bảo các chính sách CMK cấp quyền sử dụng (kms:Decrypt, kms:Encrypt, kms:GenerateDataKey, kms:DescribeKey) cho các vai trò đăng ký Lake Formation, vai trò công việc Glue ETL, và các vai trò nhà phân tích/nhà khoa học dữ liệu cần truy cập dữ liệu được mã hóa. 77  
  * **Tại sao quan trọng:** Bảo mật hồ dữ liệu là tối quan trọng. Bài thực hành này kết hợp bảo mật S3, KMS, Glue, và các khả năng kiểm soát truy cập chi tiết nâng cao của Lake Formation, bao gồm ABAC, rất cần thiết để quản lý truy cập ở quy mô lớn trong các môi trường dữ liệu phức tạp.  
  * Sự tương tác giữa các quyền IAM, S3 bucket policies, Lake Formation permissions, và KMS key policies có thể phức tạp. Một điểm thất bại phổ biến là bỏ qua một trong các lớp này, dẫn đến lỗi từ chối truy cập. Ví dụ, một người dùng có thể có quyền SELECT của Lake Formation trên một bảng, nhưng nếu vai trò công việc Glue cơ bản hoặc vai trò đăng ký Lake Formation không có quyền giải mã KMS cho các đối tượng S3, các truy vấn sẽ thất bại. Truy cập dữ liệu trong một S3 bucket được quản lý bởi Lake Formation và được mã hóa bằng KMS bao gồm nhiều lần kiểm tra quyền:  
    1. IAM: Principal (người dùng/vai trò) có quyền IAM để truy vấn (ví dụ: thông qua Athena) không?  
    2. Lake Formation (Siêu dữ liệu): Principal có quyền LF trên cơ sở dữ liệu/bảng/cột Glue không?  
    3. Lake Formation (Truy cập dữ liệu): LF cung cấp thông tin xác thực tạm thời từ vai trò của vị trí S3 đã đăng ký. Vai trò này có quyền S3 GetObject không?  
    4. KMS: Vai trò cuối cùng đọc đối tượng S3 (vai trò đăng ký LF hoặc vai trò của công cụ truy vấn nếu LF chuyển nó qua) có quyền kms:Decrypt trên khóa KMS mã hóa đối tượng S3 không? Bài thực hành này buộc người học phải cấu hình và khắc phục sự cố tất cả các lớp này.  
* **: Kiến Trúc Quản Trị IAM Liên Khu Vực và Liên Phân Vùng (Session Policies Phức Tạp, sts:SetSourceIdentity cho Chủ Quyền Dữ Liệu và Kiểm Toán)**  
  * **Mục tiêu:** Thiết kế một chiến lược IAM cho một ứng dụng toàn cầu với các yêu cầu về nơi lưu trữ dữ liệu, đòi hỏi quyền truy cập vào các tài nguyên trên các Khu vực AWS khác nhau và có thể cả các Phân vùng AWS khác nhau (ví dụ: aws thương mại, aws-cn Trung Quốc, aws-us-gov GovCloud).  
  * **Các Thành Phần Kịch Bản:**  
    * Một tài khoản quản lý IAM trung tâm trong phân vùng aws.  
    * Các tài nguyên ứng dụng (ví dụ: S3 buckets, DynamoDB tables) trong us-east-1, eu-west-1, và một aws-cn mô phỏng (sử dụng một khu vực thương mại khác cho mục đích thực hành, nhưng kiến trúc như thể đó là aws-cn).  
    * Người dùng/vai trò trong tài khoản trung tâm cần truy cập các tài nguyên trong các khu vực/phân vùng phân tán này với các kiểm soát nghiêm ngặt.  
  * **Cân Nhắc Thiết Kế IAM:**  
    * **Truy Cập Liên Khu Vực:** Thường đơn giản bằng cách sử dụng các IAM roles tiêu chuẩn và ARN tài nguyên chỉ định khu vực.  
    * **Truy Cập Liên Phân Vùng:** Các danh tính IAM (users, roles, policies) là duy nhất cho một phân vùng. Không thể trực tiếp sử dụng thông tin xác thực IAM từ một phân vùng để truy cập tài nguyên trong một phân vùng khác. 95  
      * Chiến lược: Yêu cầu tạo các IAM roles riêng biệt trong phân vùng đích. Principal trong phân vùng nguồn sẽ đảm nhận một vai trò trong phân vùng của chính nó, sau đó có thể có logic (ví dụ: một Lambda function hoạt động như một proxy) để tương tác với một vai trò trong phân vùng đích, hoặc người dùng có thể cần các thiết lập thông tin xác thực/liên kết riêng biệt cho mỗi phân vùng. Bài thực hành này sẽ tập trung vào sự phức tạp của việc *quản lý* quyền truy cập như vậy.  
    * **Session Policies để Kiểm Soát Khu Vực Chi Tiết:** Khi đảm nhận các vai trò để truy cập liên khu vực, sử dụng session policies để hạn chế các hành động chỉ đối với các tài nguyên cần thiết trong khu vực đích. 22  
      * Ví dụ: Một vai trò quản trị viên toàn cầu có thể tồn tại, nhưng khi đảm nhận nó cho các tác vụ trong eu-west-1, một session policy có thể được truyền để giới hạn các hành động chỉ đối với các tài nguyên eu-west-1.  
    * **sts:SetSourceIdentity để Kiểm Toán Liên Phân Vùng:** Nếu một vai trò trong phân vùng aws (ví dụ: GlobalAuditRole) cần kích hoạt một hành động thông qua một trung gian sau đó tương tác với một vai trò trong aws-cn (ví dụ: ChinaDataReadRole), sts:SetSourceIdentity có thể được sử dụng bởi trung gian để truyền danh tính của GlobalAuditRole ban đầu (hoặc một đại diện của nó) cho ChinaDataReadRole. Điều này giúp duy trì một dấu vết kiểm toán qua các phân vùng. \[38 (không thể truy cập), 166 (không thể truy cập), 167 (không thể truy cập)\]  
      * Trust policy của ChinaDataReadRole sẽ cần cho phép sts:SetSourceIdentity và có thể có các điều kiện trên sts:SourceIdentity.  
      * Resource policies trong phân vùng aws-cn cũng có thể sử dụng sts:SourceIdentity để kiểm soát truy cập dựa trên danh tính ban đầu từ phân vùng aws.  
    * **Thực Thi Chủ Quyền Dữ Liệu:** Sử dụng SCPs và các chính sách IAM với các điều kiện (ví dụ: aws:RequestedRegion) để hạn chế nơi dữ liệu có thể được lưu trữ hoặc truy cập, phù hợp với các yêu cầu về nơi lưu trữ dữ liệu. 24  
  * **Tại sao quan trọng:** Các tổ chức toàn cầu đối mặt với những thách thức IAM phức tạp do chủ quyền dữ liệu, các môi trường quy định khác nhau giữa các phân vùng, và nhu cầu kiểm toán tập trung. Bài thực hành này khám phá những phức tạp nâng cao này. 17  
  * Thách thức chính với IAM liên phân vùng là IAM là một dịch vụ được khu vực hóa *trong* một phân vùng, nhưng bản thân các phân vùng lại là các ranh giới cứng đối với IAM. sts:SetSourceIdentity trở nên quan trọng hơn ở đây để duy trì một vẻ ngoài của việc truy vết danh tính từ đầu đến cuối khi các hành động vượt qua các ranh giới phân vùng này thông qua các dịch vụ hoặc vai trò trung gian. Một người dùng trong aws (phân vùng thương mại) không thể trực tiếp đảm nhận một vai trò trong aws-cn (phân vùng Trung Quốc). Một ứng dụng hoặc dịch vụ trong aws có thể thực hiện một lệnh gọi API đến một điểm cuối (ví dụ: API Gateway) trong aws-cn. Điểm cuối này, được hỗ trợ bởi một Lambda trong aws-cn, sẽ có IAM role riêng của nó trong phân vùng aws-cn. Nếu lệnh gọi ban đầu từ phân vùng aws bao gồm một số thông tin danh tính (ví dụ: trong một tiêu đề hoặc tải trọng tùy chỉnh), Lambda aws-cn có thể sử dụng sts:SetSourceIdentity khi đảm nhận *một vai trò khác* trong aws-cn để thực hiện các hành động, truyền danh tính đã nhận này. Điều này phức tạp và đòi hỏi thiết kế cẩn thận của trung gian.

**Module 4: Bảo Mật IAM Chủ Động: Kiểm Toán, Tự Động Hóa và Thực Hành Tốt Nhất**

* **Tận Dụng Các Công Cụ Đánh Giá Bảo Mật IAM Nâng Cao:**  
  * Giới thiệu các công cụ tự động để xác định các cấu hình sai IAM, các đường dẫn leo thang đặc quyền và việc tuân thủ đặc quyền tối thiểu.  
  * **Bảng: Tổng Quan về Công Cụ Đánh Giá Bảo Mật IAM**

| Công Cụ | Chức Năng Chính | Điểm Mạnh Chính | Đầu Ra Điển Hình | Khi Nào Nên Sử Dụng |
| :---- | :---- | :---- | :---- | :---- |
| Cloudsplaining | Xác định vi phạm đặc quyền tối thiểu, ưu tiên rủi ro. | Báo cáo HTML dễ hiểu, tập trung vào leo thang đặc quyền và lộ dữ liệu, có thể tùy chỉnh thông qua loại trừ. | Báo cáo HTML, JSON. | Đánh giá định kỳ toàn diện các chính sách IAM, xác định các rủi ro có thể hành động. 1 |
| PMapper (Principal Mapper) | Lập bản đồ và trực quan hóa các quyền IAM, xác định các đường dẫn leo thang. | Trực quan hóa đồ thị mạnh mẽ, mô phỏng logic ủy quyền của AWS cục bộ, truy vấn tùy chỉnh. | Dữ liệu đồ thị (cho Neo4j), hình ảnh SVG. | Phân tích sâu các mối quan hệ quyền phức tạp, khám phá các đường dẫn tấn công tiềm ẩn. 103 |
| ScoutSuite | Kiểm toán bảo mật đa đám mây, bao gồm các kiểm tra IAM chi tiết. | Phạm vi bao phủ rộng các dịch vụ AWS, báo cáo HTML toàn diện, dễ sử dụng. | Báo cáo HTML. | Kiểm toán bảo mật tổng thể môi trường AWS, bao gồm IAM như một phần của bức tranh lớn hơn. 2 |
| AWS IAM Access Analyzer (Policy Validation & Unused Access) | Xác thực chính sách theo các tiêu chuẩn, xác định quyền truy cập không sử dụng và truy cập từ bên ngoài, tạo chính sách. | Tích hợp gốc AWS, kiểm tra chủ động và phát hiện, tạo chính sách dựa trên hoạt động. | Kết quả phân tích trong bảng điều khiển, JSON, đề xuất chính sách. | Giám sát liên tục, tinh chỉnh quyền chủ động, tạo chính sách đặc quyền tối thiểu. 107 |

    \*Lưu ý:\* Yêu cầu của người dùng liệt kê cụ thể một số công cụ. Người học cần hiểu đề xuất giá trị độc đáo của từng công cụ. Một bảng so sánh giúp người học chọn đúng công cụ cho các tác vụ đánh giá cụ thể (ví dụ: PMapper để trực quan hóa, Cloudsplaining cho các báo cáo ưu tiên rủi ro). Điều này cho phép sử dụng hiệu quả và có mục tiêu các công cụ bảo mật, cải thiện hiệu quả của việc kiểm toán IAM.  
\*   \*\*++PHÒNG\_LAB\_THỰC\_HÀNH++: Đánh Giá Tình Trạng Bảo Mật IAM với Cloudsplaining\*\*  
    \*   \*\*Kịch bản:\*\* Cài đặt Cloudsplaining. Tải xuống chi tiết ủy quyền IAM từ một tài khoản AWS thử nghiệm (được cấu hình sẵn với một số chính sách quá rộng rãi). Chạy quét Cloudsplaining. \[\[101\] (không thể truy cập), \[102, 168, 169, 170, 171\]\]  
    \*   \*\*Nhiệm vụ:\*\* Phân tích báo cáo HTML được tạo ra, tập trung vào:  
        \*   Xác định các chính sách có rủi ro cao và các đường dẫn leo thang đặc quyền.  
        \*   Hiểu các rủi ro lộ dữ liệu (ví dụ: \`s3:GetObject\` trên \`\*\`).  
        \*   Giải thích các đánh giá rủi ro và lời khuyên khắc phục.  
        \*   Tạo một tệp loại trừ để lọc ra các cấu hình cố ý hoặc các kết quả dương tính giả.  
    \*   \*\*Tại sao quan trọng:\*\* Cloudsplaining là một công cụ mã nguồn mở mạnh mẽ để xác định các vi phạm đặc quyền tối thiểu. Bài thực hành này cung cấp kinh nghiệm thực tế trong việc sử dụng nó cho các cuộc kiểm toán IAM toàn diện. \[111, 112, 113\]  
\*   \*\*++PHÒNG\_LAB\_THỰC\_HÀNH++: Trực Quan Hóa Quyền IAM và Đường Dẫn Leo Thang với PMapper\*\*  
    \*   \*\*Kịch bản:\*\* Cài đặt PMapper và các phụ thuộc của nó (Graphviz). Tạo dữ liệu IAM cho một tài khoản thử nghiệm. Sử dụng PMapper để tạo một trực quan hóa đồ thị của các principal IAM và các quyền của chúng. \[103, 104, 105, 114, 115, 116\]  
    \*   \*\*Nhiệm vụ:\*\*  
        \*   Truy vấn đồ thị để xác định ai có thể thực hiện các hành động cụ thể (ví dụ: \`iam:CreateUser\`, \`sts:AssumeRole\` trên một vai trò quan trọng).  
        \*   Trực quan hóa các đường dẫn leo thang đặc quyền tiềm ẩn.  
        \*   Hiểu cách PMapper mô phỏng logic ủy quyền của AWS.  
    \*   \*\*Tại sao quan trọng:\*\* Việc trực quan hóa các mối quan hệ IAM phức tạp có thể tiết lộ các đường dẫn tấn công và sự gia tăng quyền không rõ ràng mà khó phát hiện chỉ bằng các chính sách JSON. \[117\]  
\*   \*\*++PHÒNG\_LAB\_THỰC\_HÀNH++: Kiểm Toán Môi Trường Toàn Diện với ScoutSuite\*\*  
    \*   \*\*Kịch bản:\*\* Cài đặt ScoutSuite. Cấu hình nó để quét một tài khoản AWS thử nghiệm.  
    \*   \*\*Nhiệm vụ:\*\*  
        \*   Xem xét báo cáo HTML của ScoutSuite, đặc biệt chú ý đến các phát hiện IAM (ví dụ: việc sử dụng tài khoản gốc, trạng thái MFA, chính sách mật khẩu, các vai trò quá rộng rãi). \[2, 106, 118, 119\]  
        \*   So sánh các phát hiện với các công cụ khác như Cloudsplaining.  
        \*   Hiểu cách ScoutSuite bao gồm một phạm vi rộng hơn các dịch vụ AWS ngoài IAM.  
    \*   \*\*Tại sao quan trọng:\*\* ScoutSuite cung cấp khả năng kiểm toán bảo mật đa đám mây. Bài thực hành này giới thiệu các tính năng đánh giá IAM của nó và cách chúng phù hợp với một cuộc kiểm toán bảo mật đám mây rộng hơn.

* **Phát Hiện và Khắc Phục Các Cấu Hình Sai IAM bằng Script Tùy Chỉnh và Dịch Vụ Gốc AWS:**  
  * **Script Tùy Chỉnh cho Kiểm Toán IAM:**  
    * Sử dụng Python (Boto3) hoặc AWS CLI với jq để viết script kiểm tra tùy chỉnh cho các cấu hình IAM cụ thể không được bao gồm bởi các công cụ tiêu chuẩn. 105  
    * Ví dụ: Script để tìm các IAM roles không có thẻ cụ thể, người dùng có khóa truy cập cũ, các vai trò có thể được đảm nhận bởi các principal không đáng tin cậy.  
  * **AWS IAM Access Analyzer:**  
    * **External Access Analyzers:** Xác định các tài nguyên được chia sẻ với các thực thể bên ngoài (liên tài khoản, công khai). 109  
    * **Unused Access Analyzers:** Phát hiện các vai trò, khóa truy cập và mật khẩu không sử dụng. 109  
    * **Policy Validation:** Kiểm tra các chính sách IAM theo các thực hành tốt nhất và các tiêu chuẩn bảo mật tùy chỉnh. 109  
    * **Policy Generation:** Tạo các chính sách đặc quyền tối thiểu dựa trên nhật ký truy cập CloudTrail. 107  
    * IAM Access Analyzer đang phát triển thành một bộ công cụ toàn diện cho các biện pháp kiểm soát IAM chủ động và phát hiện. Khả năng tạo chính sách của nó, mặc dù cần xem xét cẩn thận, có thể tăng tốc đáng kể con đường đạt được đặc quyền tối thiểu. Việc tạo thủ công các chính sách đặc quyền tối thiểu tốn thời gian và dễ xảy ra lỗi. Bằng cách phân tích các mẫu sử dụng thực tế từ CloudTrail, IAM Access Analyzer có thể đề xuất các chính sách gần hơn nhiều với các quyền tối thiểu thực sự cần thiết, giảm nỗ lực của nhà phát triển và cải thiện tình trạng bảo mật.  
  * **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Viết Script Kiểm Toán IAM Tùy Chỉnh**  
    * **Kịch bản:** Viết một script Python Boto3 để:  
      1. Tìm tất cả IAM users chưa xoay vòng khóa truy cập của họ trong hơn 90 ngày. 5  
      2. Xác định các IAM roles có thể được đảm nhận bởi bất kỳ principal nào ("AWS": "\*") hoặc từ một tập hợp các principal quá rộng.  
      3. Kiểm tra xem tất cả IAM users có quyền truy cập bảng điều khiển đã bật MFA chưa. 5  
    * **Tại sao quan trọng:** Dạy cách tự động hóa các kiểm tra tuân thủ tùy chỉnh phù hợp với các chính sách tổ chức cụ thể.  
  * **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Tận Dụng IAM Access Analyzer cho Đặc Quyền Tối Thiểu**  
    * **Kịch bản:**  
      1. Sử dụng IAM Access Analyzer để tạo một chính sách cho một IAM role ban đầu được cấp các quyền quá rộng nhưng đã được sử dụng cho các tác vụ cụ thể. Xem xét và tinh chỉnh chính sách được tạo ra. 107  
      2. Cấu hình một external access analyzer và xác định bất kỳ S3 buckets hoặc IAM roles nào vô tình bị lộ cho các tài khoản bên ngoài.  
      3. Sử dụng policy validation để kiểm tra một tập hợp các chính sách IAM được viết sẵn xem có các lỗ hổng bảo mật phổ biến không (ví dụ: các hành động ký tự đại diện, thiếu điều kiện). 123  
    * **Tại sao quan trọng:** Cung cấp kinh nghiệm thực tế với các công cụ gốc của AWS để đạt được và duy trì đặc quyền tối thiểu.  
* **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Áp Dụng Các Nguyên Tắc Zero Trust vào Chính Sách IAM cho Microservices**  
  * **Kịch bản:** Xem lại kiến trúc microservice serverless từ Module 3 (API Gateway, Lambda A, SQS, Lambda B, DynamoDB).  
  * **Nhiệm vụ:**  
    * Tinh chỉnh các IAM execution roles cho Lambda A và Lambda B để cực kỳ chi tiết, đảm bảo mỗi hàm *chỉ* có thể thực hiện tác vụ cụ thể của nó trên tài nguyên được chỉ định của nó (ví dụ: Lambda A chỉ có thể sqs:SendMessage đến QueueB\_ARN, không phải QueueC\_ARN). 3  
    * Triển khai resource policies trên SQS Queue B cho phép rõ ràng sqs:SendMessage *chỉ* từ ARN vai trò thực thi của Lambda A và sqs:ReceiveMessage *chỉ* từ ARN vai trò thực thi của Lambda B.  
    * Nếu API Gateway gọi Lambda A, đảm bảo API Gateway có một execution role (nếu sử dụng proxy dịch vụ AWS cho Lambda) hoặc resource policy của Lambda A (nếu API Gateway gọi trực tiếp) chỉ cho phép gọi từ ARN API Gateway cụ thể.  
    * Sử dụng các điều kiện như aws:SourceArn trong các chính sách để ngăn một dịch vụ bị gọi bởi một dịch vụ ngược dòng không mong muốn.  
  * **Tại sao quan trọng:** Bài thực hành này minh họa cách áp dụng Zero Trust ở cấp chính sách IAM cho các ứng dụng serverless, nơi mọi tương tác đều được ủy quyền rõ ràng và thu hẹp phạm vi.  
  * Zero Trust trong IAM có nghĩa là loại bỏ sự tin cậy rộng rãi dựa trên vị trí mạng hoặc danh tính dịch vụ ngầm định, hướng tới xác minh rõ ràng cho mọi hành động và principal. Các khóa điều kiện rất quan trọng cho việc này. Trong môi trường microservices, một dịch vụ bị xâm phạm không nên có khả năng xâm phạm các dịch vụ khác. Nếu vai trò của Lambda A có thể gửi tin nhắn đến *bất kỳ* hàng đợi SQS nào, một vụ xâm phạm có thể dẫn đến việc tin nhắn bị đưa vào các hàng đợi không mong muốn. Bằng cách hạn chế Lambda A *chỉ* vào SQS Queue B thông qua vai trò thực thi của nó và resource policy của Queue B, tác động sẽ được hạn chế.  
* **Tuân Thủ Liên Tục và Khắc Phục Tự Động Hóa:**  
  * **AWS Config cho Tuân Thủ IAM:**  
    * Sử dụng các quy tắc AWS Config được quản lý cho IAM (ví dụ: iam-user-no-policies-attached, mfa-enabled-for-iam-console-access, iam-role-no-inline-policy-check). 125  
    * Phát triển các quy tắc AWS Config tùy chỉnh bằng cách sử dụng Lambda functions hoặc AWS CloudFormation Guard để kiểm tra các yêu cầu tuân thủ IAM cụ thể của tổ chức (ví dụ: các vai trò phải có permissions boundaries, không có iam:PassRole đến \*). \[6 (không thể truy cập), 177 (không thể truy cập)\]  
  * **Khắc Phục Tự Động Hóa với Lambda:**  
    * Kích hoạt Lambda functions từ các phát hiện không tuân thủ của AWS Config hoặc CloudWatch Events để tự động khắc phục một số vấn đề IAM (ví dụ: tách các chính sách người dùng trực tiếp, vô hiệu hóa các khóa truy cập không sử dụng, thực thi MFA trên các vai trò). 63  
  * **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Tự Động Hóa Vệ Sinh IAM: Lambda cho Quản Lý Thông Tin Xác Thực Không Sử Dụng & Thực Thi MFA**  
    * **Kịch bản 1 (Khóa không sử dụng):** Phát triển một Lambda function Python Boto3:  
      1. Quét tất cả IAM users.  
      2. Kiểm tra GetAccessKeyLastUsed cho mỗi khóa truy cập đang hoạt động.  
      3. Nếu một khóa chưa được sử dụng trong X ngày (ví dụ: 90 ngày), hãy hủy kích hoạt nó (UpdateAccessKey thành Inactive). 129  
      4. (Tùy chọn) Gửi thông báo (ví dụ: đến SNS hoặc Slack).  
      * Cấp cho Lambda execution role các quyền cần thiết: iam:ListUsers, iam:ListAccessKeys, iam:GetAccessKeyLastUsed, iam:UpdateAccessKey.  
    * **Kịch bản 2 (Thực thi MFA trên Roles):** Phát triển một Lambda function Python Boto3 được kích hoạt bởi các sự kiện CloudTrail cho CreateRole hoặc UpdateAssumeRolePolicy.  
      1. Lambda kiểm tra trust policy của vai trò.  
      2. Nếu trust policy cho phép đảm nhận bởi IAM users nhưng *không* bao gồm điều kiện yêu cầu MFA ("Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}), Lambda sẽ:  
         * Báo cáo vai trò không tuân thủ.  
         * (Nâng cao) Cố gắng cập nhật trust policy để bao gồm điều kiện MFA (yêu cầu cấp quyền cẩn thận cho vai trò Lambda).  
      * Cấp cho Lambda execution role quyền iam:GetRole, iam:UpdateAssumeRolePolicy (nếu khắc phục).  
    * **Tại sao quan trọng:** Minh họa tự động hóa theo sự kiện để duy trì vệ sinh IAM và thực thi các chính sách bảo mật, giảm nỗ lực thủ công và cải thiện tình trạng bảo mật.  
  * **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Phát Triển Các Quy Tắc AWS Config Tùy Chỉnh cho Tuân Thủ IAM Chủ Động**  
    * **Kịch bản 1 (Không có hành động ký tự đại diện):** Tạo một quy tắc AWS Config tùy chỉnh (sử dụng Lambda hoặc Guard) kiểm tra các customer-managed policies của IAM. Quy tắc sẽ gắn cờ bất kỳ chính sách nào là KHÔNG TUÂN THỦ nếu nó chứa một câu lệnh với "Effect": "Allow", "Action": "\*", "Resource": "\*". 125  
    * **Kịch bản 2 (Kiểm tra Permission Boundary):** Tạo một quy tắc AWS Config tùy chỉnh kiểm tra tất cả các IAM roles mới được tạo hoặc cập nhật. Quy tắc sẽ gắn cờ một vai trò là KHÔNG TUÂN THỦ nếu nó *không* có một permissions boundary cụ thể, do tổ chức xác định được đính kèm. (Gợi ý: kiểm tra attachedManagedPolicies cho ARN boundary hoặc sử dụng GetRole để kiểm tra PermissionsBoundary). 138  
    * **Kịch bản 3 (Không có iam:PassRole đến \*):** Tạo một quy tắc AWS Config tùy chỉnh để phát hiện các chính sách IAM cấp iam:PassRole trên Resource: "\*". \[138 (không thể truy cập)\]  
    * **Kịch bản 4 (IAM Users không có MFA):** Sử dụng quy tắc Config được quản lý iam-user-mfa-enabled hoặc tạo một quy tắc tùy chỉnh dựa trên Lambda để kiểm tra xem IAM users có mật khẩu bảng điều khiển đã bật MFA chưa. 126  
    * **Triển khai:** Triển khai các quy tắc này bằng CloudFormation.  
    * **Tại sao quan trọng:** Bài thực hành này dạy cách mã hóa và tự động hóa các kiểm tra tuân thủ cho các chính sách IAM, cho phép giám sát liên tục và xác định chủ động các cấu hình sai.

**Module 5: Học Hỏi Từ Các Sự Cố IAM Thực Tế**

* **Phân Tích Sâu Các Nghiên Cứu Điển Hình: Phân Tích Các Thất Bại IAM trong Các Vụ Vi Phạm Lớn:**  
  * **Vụ Vi Phạm Capital One (2019):**  
    * Nguyên nhân gốc rễ: WAF được cấu hình sai (lỗ hổng SSRF), IAM role quá rộng rãi được gắn vào EC2 instance chạy WAF, cho phép nó liệt kê các S3 buckets và đọc các đối tượng. Kẻ tấn công đã khai thác SSRF để truy cập dịch vụ metadata của EC2, lấy thông tin xác thực tạm thời cho IAM role, và sau đó lấy cắp dữ liệu từ S3. 144  
    * Bài học IAM:  
      * **Vi phạm Nguyên tắc Đặc quyền Tối thiểu:** IAM role của WAF có các quyền S3 quá mức (s3:ListAllMyBuckets, s3:GetObject trên nhiều bucket). 146  
      * **Bảo mật Dịch vụ Metadata của EC2 Instance:** Cần IMDSv2 để bảo vệ chống lại SSRF.  
      * **Tầm quan trọng của Giám sát và Cảnh báo:** Các hành động của kẻ tấn công (số lượng lớn các hoạt động liệt kê/lấy S3) lẽ ra phải kích hoạt cảnh báo. 146  
  * **Các Sự Cố Khác (Đã Ẩn Danh):**  
    * Lộ Khóa Truy Cập trong Kho Lưu Trữ Mã Công Khai: Dẫn đến việc tạo/truy cập tài nguyên trái phép. \[144 (đề cập đến ransomware Codefinger sử dụng thông tin xác thực bị xâm phạm)\]  
    * Leo Thang Đặc Quyền qua iam:PassRole hoặc iam:CreatePolicyVersion: Người dùng có quyền hạn chế có thể leo thang lên quản trị viên.  
    * Trust Policies Được Cấu Hình Sai: Các vai trò có thể được đảm nhận bởi các principal không mong muốn.  
  * Các vụ vi phạm thực tế thường khai thác một chuỗi các lỗ hổng, trong đó các cấu hình sai IAM là một mắt xích quan trọng. Hiểu các đường dẫn tấn công này là điều cần thiết để thiết kế các biện pháp phòng thủ kiên cường. Vụ vi phạm Capital One không chỉ là một lỗ hổng SSRF; đó là sự kết hợp của SSRF *và* một IAM role quá rộng rãi đã dẫn đến việc lấy cắp dữ liệu quy mô lớn. Nếu IAM role đã tuân theo đặc quyền tối thiểu (ví dụ: không có quyền truy cập danh sách S3, hoặc quyền truy cập chỉ vào một bucket/tiền tố cụ thể, không nhạy cảm), tác động của SSRF sẽ bị hạn chế đáng kể. Điều này nhấn mạnh IAM là một mặt phẳng kiểm soát quan trọng như thế nào.  
* **\++PHÒNG\_LAB\_THỰC\_HÀNH++: Mô Phỏng và Phòng Chống Các Vector Tấn Công IAM Phổ Biến**  
  * **Lab 5.1: Mô Phỏng Việc Đánh Cắp Thông Tin Xác Thực Vai Trò EC2 Instance qua SSRF (Khái niệm).**  
    * **Kịch bản:** (Không thể sao chép hoàn toàn SSRF nếu không có ứng dụng dễ bị tấn công, nhưng có thể mô phỏng *hậu quả*). Tạo một EC2 instance với một IAM role có các quyền quá rộng (ví dụ: S3 toàn quyền truy cập). "Mô phỏng" việc đánh cắp thông tin xác thực bằng cách lấy thông tin xác thực tạm thời của vai trò từ EC2 instance (ví dụ: qua script dữ liệu người dùng hoặc SSH). Sử dụng các thông tin xác thực này từ máy cục bộ của người học (bên ngoài EC2) để truy cập các S3 bucket.  
    * **Phòng thủ:** Thảo luận và triển khai việc thực thi IMDSv2. Sửa đổi IAM role để áp dụng đặc quyền tối thiểu (ví dụ: quyền truy cập chỉ vào một bucket/tiền tố cụ thể).  
    * **Tại sao quan trọng:** Nêu bật rủi ro của thông tin xác thực hồ sơ instance và tầm quan trọng của IMDSv2 và đặc quyền tối thiểu cho các vai trò EC2.  
  * **Lab 5.2: Khai Thác iam:PassRole để Leo Thang Đặc Quyền.**  
    * **Kịch bản:** Tạo Người dùng A có quyền iam:PassRole trên Resource: \* và ec2:RunInstances. Tạo Vai trò X với AdministratorAccess. Người dùng A thường không thể thực hiện các hành động quản trị.  
    * **Nhiệm vụ:** Người dùng A khởi chạy một EC2 instance và truyền Vai trò X cho nó. Người dùng A sau đó truy cập EC2 instance và sử dụng thông tin xác thực của Vai trò X (thông qua dịch vụ metadata) để thực hiện các hành động quản trị.  
    * **Phòng thủ:** Hạn chế iam:PassRole cho các vai trò cụ thể, ít đặc quyền hơn. Sử dụng permissions boundaries trên Người dùng A.  
    * **Tại sao quan trọng:** Minh họa một kỹ thuật leo thang đặc quyền IAM cổ điển.  
  * **Lab 5.3: Khai Thác Trust Policy Được Cấu Hình Sai.**  
    * **Kịch bản:** Tạo Vai trò Y với các quyền nhạy cảm. Trust policy của nó vô tình cho phép AssumeRole từ AWS: \* hoặc một tài khoản bên ngoài quá rộng.  
    * **Nhiệm vụ:** Từ một ngữ cảnh không được ủy quyền (ví dụ: một người dùng/tài khoản khác), cố gắng đảm nhận Vai trò Y.  
    * **Phòng thủ:** Triển khai các trust policies nghiêm ngặt, chỉ định chính xác ARN principal hoặc sử dụng các điều kiện như aws:PrincipalOrgID.  
    * **Tại sao quan trọng:** Cho thấy một trust policy được cấu hình sai có thể dễ dàng dẫn đến việc đảm nhận vai trò trái phép như thế nào.

**Đồ Án Tốt Nghiệp: Kiến Trúc và Bảo Vệ Chiến Lược IAM Cấp Doanh Nghiệp**

* **Mục tiêu:** Tổng hợp tất cả các khái niệm và kỹ năng đã học bằng cách thiết kế một chiến lược IAM toàn diện, an toàn và kiên cường cho một tổ chức giả định phức tạp. Người học sẽ chọn **một** trong các kịch bản sau (hoặc một kịch bản phức tạp tương tự do họ đề xuất):  
  * **Kịch bản A: Nền Tảng Thương Mại Điện Tử Toàn Cầu với Nhu Cầu Tuân Thủ PCI-DSS:**  
    * AWS Organization đa tài khoản (Sản xuất, Phát triển, Thử nghiệm, Dịch vụ Chung, Lưu trữ Bảo mật/Nhật ký).  
    * Các ứng dụng web/di động hướng tới khách hàng, các microservices backend (EC2, ECS/EKS, Lambda).  
    * Cơ sở dữ liệu (RDS, DynamoDB), S3 cho tài sản tĩnh và nhật ký.  
    * Các thành phần xử lý thanh toán yêu cầu tuân thủ PCI-DSS.  
    * Các pipeline CI/CD để triển khai cơ sở hạ tầng và ứng dụng.  
    * Tích hợp của bên thứ ba (ví dụ: cổng thanh toán, dịch vụ phân tích).  
    * Truy cập của nhân viên thông qua liên kết SAML từ IdP của công ty.  
    * 147  
  * **Kịch bản B: Nền Tảng Chăm Sóc Sức Khỏe Tuân Thủ HIPAA:**  
    * AWS Organization đa tài khoản (Sản xuất-PHI, Phát triển-Không PHI, Phân tích, Bảo mật/Nhật ký).  
    * Cổng thông tin bệnh nhân (ứng dụng web), các pipeline nhập dữ liệu EHR, nền tảng phân tích cho dữ liệu đã được khử nhận dạng.  
    * Các dịch vụ xử lý Thông tin Sức khỏe Được Bảo vệ (PHI) phải đủ điều kiện HIPAA và được cấu hình an toàn.  
    * Kiểm soát truy cập nghiêm ngặt cho bác sĩ lâm sàng, nhà nghiên cứu, bệnh nhân và quản trị viên hệ thống.  
    * Mã hóa dữ liệu khi lưu trữ và truyền tải bằng KMS.  
    * Kiểm toán và ghi nhật ký mạnh mẽ để tuân thủ.  
    * Truy cập liên kết cho các chuyên gia chăm sóc sức khỏe.  
    * 147  
* **Sản Phẩm Giao Nộp:**  
  1. **Tài Liệu Kiến Trúc IAM:**  
     * Chiến lược IAM tổng thể, lý do cấu trúc tài khoản.  
     * Định nghĩa các vai trò IAM chính (người dùng người, vai trò dịch vụ, vai trò instance, vai trò CI/CD, vai trò liên tài khoản) với trách nhiệm và mối quan hệ tin cậy của chúng.  
     * Chiến lược liên kết (OIDC/SAML) cho nhân viên và ứng dụng.  
     * Chiến lược cho thông tin xác thực tạm thời và quản lý khóa truy cập.  
     * Chiến lược permissions boundary cho quản trị được ủy quyền.  
     * Thiết kế SCP cho các rào chắn tổ chức.  
  2. **Ví Dụ Chính Sách IAM:**  
     * Các chính sách IAM chính (identity-based, resource-based, session policies, SCPs) minh họa đặc quyền tối thiểu, sử dụng điều kiện và tuân thủ các yêu cầu tuân thủ (PCI hoặc HIPAA).  
     * Chính sách cho tự động hóa CI/CD.  
     * Chính sách cho truy cập dữ liệu (ví dụ: S3 bucket policies, KMS key policies, Lake Formation nếu có).  
  3. **Sổ Tay Vận Hành:**  
     * Quy trình quản lý vòng đời người dùng/vai trò IAM (giới thiệu, loại bỏ, xem xét quyền truy cập).  
     * Kế hoạch ứng phó sự cố cho các sự kiện bảo mật liên quan đến IAM (ví dụ: thông tin xác thực bị xâm phạm, cố gắng leo thang đặc quyền).  
     * Quy trình kiểm toán và giám sát (phân tích CloudTrail, kiểm tra quy tắc Config).  
  4. **Chiến Lược Phòng Thủ:**  
     * Cách thiết kế giảm thiểu các vector tấn công IAM phổ biến.  
     * Cách áp dụng các nguyên tắc Zero Trust.  
     * Cách duy trì tuân thủ liên tục.  
* **Tại sao quan trọng:** Đồ án tốt nghiệp này yêu cầu người học phải suy nghĩ như một kiến trúc sư IAM doanh nghiệp, đưa ra các quyết định thiết kế phức tạp, biện minh cho chúng và xem xét các tác động vận hành và bảo mật. Đó là bài kiểm tra cuối cùng về chuyên môn của họ.  
* Sự lựa chọn giữa một mô hình IAM tập trung cao độ so với một mô hình liên kết/ủy quyền nhiều hơn trong tổ chức thường phụ thuộc vào quy mô, văn hóa và yêu cầu linh hoạt của tổ chức. Đồ án tốt nghiệp nên khám phá những đánh đổi này. Một nền tảng thương mại điện tử lớn có thể có nhiều nhóm phát triển. Một phương pháp quản lý IAM hoàn toàn tập trung có thể trở thành một nút thắt cổ chai. Việc ủy quyền tạo vai trò bằng cách sử dụng permissions boundaries cho phép các nhóm di chuyển nhanh hơn. Tuy nhiên, việc ủy quyền này phải được cân bằng với các rào chắn trung tâm mạnh mẽ (SCPs) và kiểm toán mạnh mẽ để đảm bảo tuân thủ (như PCI-DSS) không bị xâm phạm. Đồ án tốt nghiệp yêu cầu trình bày rõ ràng sự cân bằng này.

**Kết Luận: Hành Trình Liên Tục Của Một Chuyên Gia IAM**

* **Luôn Đi Đầu trong Bối Cảnh IAM Không Ngừng Phát Triển:**  
  * Các dịch vụ AWS và khả năng IAM không ngừng phát triển. Nhấn mạnh sự cần thiết của việc học hỏi liên tục, cập nhật các tính năng mới, blog bảo mật và các thông báo tại re:Invent. 149  
  * Bối cảnh mối đe dọa cũng thay đổi, đòi hỏi sự thích ứng liên tục của các chiến lược IAM.  
* **Trách Nhiệm Lớn Lao:**  
  * Là một chuyên gia IAM, người học nắm giữ chìa khóa của vương quốc. Với quyền lực lớn lao đi kèm trách nhiệm lớn lao. Những cân nhắc về đạo đức và tư duy ưu tiên bảo mật là tối quan trọng.  
* **Lời Động Viên Cuối Cùng:**  
  * Hành trình này đầy thử thách nhưng vô cùng xứng đáng. Những kỹ năng mà người học đã rèn giũa sẽ khiến họ trở thành một tài sản vô giá trong việc bảo mật các môi trường đám mây. Hãy tiếp tục thực hành, đặt câu hỏi và đổi mới. Giờ đây, người học được trang bị không chỉ để quản lý IAM mà còn để dẫn dắt và định hình sự xuất sắc của IAM.

#### **Nguồn trích dẫn**

1. PlayCloud Hands-On Labs \- Tutorials Dojo Portal, truy cập vào tháng 5 14, 2025, [https://portal.tutorialsdojo.com/playcloud/](https://portal.tutorialsdojo.com/playcloud/)  
2. Hands-On AWS Penetration Testing with Kali Linux | Security | Paperback \- Packt, truy cập vào tháng 5 14, 2025, [https://www.packtpub.com/en-co/product/hands-on-aws-penetration-testing-with-kali-linux-9781789136722](https://www.packtpub.com/en-co/product/hands-on-aws-penetration-testing-with-kali-linux-9781789136722)  
3. Best practices for creating least-privilege AWS IAM policies \- Datadog, truy cập vào tháng 5 14, 2025, [https://www.datadoghq.com/blog/iam-least-privilege/](https://www.datadoghq.com/blog/iam-least-privilege/)  
4. Strategies for achieving least privilege at scale – Part 1 | AWS Security Blog, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/strategies-for-achieving-least-privilege-at-scale-part-1/](https://aws.amazon.com/blogs/security/strategies-for-achieving-least-privilege-at-scale-part-1/)  
5. 13 Essential AWS IAM Best Practices \- Wiz, truy cập vào tháng 5 14, 2025, [https://www.wiz.io/academy/aws-iam-best-practices](https://www.wiz.io/academy/aws-iam-best-practices)  
6. How to Configure IAM Roles and Policies on AWS for Maximum Security | SecOps® Solution, truy cập vào tháng 5 14, 2025, [https://www.secopsolution.com/blog/how-to-configure-iam-roles-and-policies-on-aws-for-maximum-security](https://www.secopsolution.com/blog/how-to-configure-iam-roles-and-policies-on-aws-for-maximum-security)  
7. Zero Trust Architecture in AWS: A Practical Implementation Guide \- NovelVista, truy cập vào tháng 5 14, 2025, [https://www.novelvista.com/blogs/cloud-and-aws/zero-trust-architecture-in-aws-a-practical-implementation-guide](https://www.novelvista.com/blogs/cloud-and-aws/zero-trust-architecture-in-aws-a-practical-implementation-guide)  
8. Implementing Zero Trust Security On AWS: What CTOs Must Know?, truy cập vào tháng 5 14, 2025, [https://marutitech.com/zero-trust-security-aws-guide/](https://marutitech.com/zero-trust-security-aws-guide/)  
9. Mapping AWS to the 7 Key Principles of Zero Trust \- Tim Layton, truy cập vào tháng 5 14, 2025, [https://timlaytonllc.com/2025/04/03/mapping-aws-to-the-7-key-principles-of-zero-trust/](https://timlaytonllc.com/2025/04/03/mapping-aws-to-the-7-key-principles-of-zero-trust/)  
10. Implement Zero Trust on AWS \- SUDO Consultants, truy cập vào tháng 5 14, 2025, [https://sudoconsultants.com/implementing-a-zero-trust-security-model-on-aws/](https://sudoconsultants.com/implementing-a-zero-trust-security-model-on-aws/)  
11. Implementing Zero Trust Security in Cloud-Native Applications (AWS & Kubernetes), truy cập vào tháng 5 14, 2025, [https://dev.to/devvemeka/implementing-zero-trust-security-in-cloud-native-applications-aws-kubernetes-51b6](https://dev.to/devvemeka/implementing-zero-trust-security-in-cloud-native-applications-aws-kubernetes-51b6)  
12. Inside AWS IAM: A Deep Dive into Identity and Access Management \- Cloudchipr, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-iam](https://cloudchipr.com/blog/aws-iam)  
13. An AWS IAM Roles Deep Dive: Terms, Concepts, and Examples, truy cập vào tháng 5 14, 2025, [https://awsfundamentals.com/blog/aws-iam-roles-terms-concepts-and-examples](https://awsfundamentals.com/blog/aws-iam-roles-terms-concepts-and-examples)  
14. AWS IAM Roles vs Policies: What's the Difference? | StrongDM, truy cập vào tháng 5 14, 2025, [https://www.strongdm.com/blog/aws-iam-roles-vs-policies](https://www.strongdm.com/blog/aws-iam-roles-vs-policies)  
15. Comprehensive Guide of AWS IAM Policy evaluation logic \- Community.aws, truy cập vào tháng 5 14, 2025, [https://community.aws/content/2d1bIioM3UgQZqyaYquu3kTaWAg/comprehensive-guide-of-aws-iam-policy-evaluation-logic](https://community.aws/content/2d1bIioM3UgQZqyaYquu3kTaWAg/comprehensive-guide-of-aws-iam-policy-evaluation-logic)  
16. AWS IAM Policies: A Practical Approach, truy cập vào tháng 5 14, 2025, [https://awsfundamentals.com/blog/aws-iam-policies-a-practical-approach](https://awsfundamentals.com/blog/aws-iam-policies-a-practical-approach)  
17. IAM and AWS STS condition context keys \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_iam-condition-keys.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_iam-condition-keys.html)  
18. AWS global condition context keys \- AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_condition-keys.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)  
19. AWS Identity-Based Policy Vs. Resource-Based Policy \- Sonrai Security, truy cập vào tháng 5 14, 2025, [https://sonraisecurity.com/blog/aws-identity-based-policy-vs-resource-based-policy/](https://sonraisecurity.com/blog/aws-identity-based-policy-vs-resource-based-policy/)  
20. Advancing your Security with IAM Permission Boundaries \- AWS Fundamentals, truy cập vào tháng 5 14, 2025, [https://awsfundamentals.com/blog/advancing-your-security-with-permission-boundaries-c71d21450016](https://awsfundamentals.com/blog/advancing-your-security-with-permission-boundaries-c71d21450016)  
21. When and where to use IAM permissions boundaries | AWS Security Blog, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/](https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/)  
22. Creating a session policy for an Amazon S3 bucket \- AWS Transfer Family, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/transfer/latest/userguide/users-policies-session.html](https://docs.aws.amazon.com/transfer/latest/userguide/users-policies-session.html)  
23. AWS Session Policy \- Cloud Security Masterclass, truy cập vào tháng 5 14, 2025, [https://cloudsecuritymasterclass.com/aws-session-policy/](https://cloudsecuritymasterclass.com/aws-session-policy/)  
24. Protecting AWS Organization with SCPs \- Community.aws, truy cập vào tháng 5 14, 2025, [https://community.aws/content/2bdbE4ATk9cZ6sju5Yy81QrqS4C/protecting-your-aws-organization-with-service-control-policies-scps-best-practices-and-strategies](https://community.aws/content/2bdbE4ATk9cZ6sju5Yy81QrqS4C/protecting-your-aws-organization-with-service-control-policies-scps-best-practices-and-strategies)  
25. AWS SCP: Tutorials & Best Practices for MSPs \- CloudBolt, truy cập vào tháng 5 14, 2025, [https://www.cloudbolt.io/msp-best-practices/aws-scp/](https://www.cloudbolt.io/msp-best-practices/aws-scp/)  
26. Troubleshoot IAM policies \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot\_policies.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_policies.html)  
27. Troubleshoot IAM policy access denied or unauthorized operation errors \- AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/troubleshoot-iam-policy-issues](https://repost.aws/knowledge-center/troubleshoot-iam-policy-issues)  
28. Use the IAM Identity Center with IAM identities for federation \- AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/iam-identity-center-federation](https://repost.aws/knowledge-center/iam-identity-center-federation)  
29. Identity federation in AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/identity/federation/](https://aws.amazon.com/identity/federation/)  
30. AWS::IAM::OIDCProvider \- AWS CloudFormation \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-oidcprovider.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-oidcprovider.html)  
31. Configure OpenID Connect in AWS to retrieve temporary credentials \- GitLab Docs, truy cập vào tháng 5 14, 2025, [https://docs.gitlab.com/ci/cloud\_services/aws/](https://docs.gitlab.com/ci/cloud_services/aws/)  
32. Create an OpenID Connect (OIDC) identity provider in IAM \- AWS ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_providers\_create\_oidc.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)  
33. Cross account resource access in IAM \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies-cross-account-resource-access.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies-cross-account-resource-access.html)  
34. AssumeRole \- AWS Security Token Service \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)  
35. AWS STS: A Complete Guide to AWS Security Token Service \- Cloudchipr, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-sts](https://cloudchipr.com/blog/aws-sts)  
36. AWS IAM Roles \- Everything You Need to Know & Examples \- Spacelift, truy cập vào tháng 5 14, 2025, [https://spacelift.io/blog/aws-iam-roles](https://spacelift.io/blog/aws-iam-roles)  
37. AWS STS GetFederationToken \+ resource based policy, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUnNLZtmx-R72HvLZf16k2YA/aws-sts-getfederationtoken-resource-based-policy](https://repost.aws/questions/QUnNLZtmx-R72HvLZf16k2YA/aws-sts-getfederationtoken-resource-based-policy)  
38. The IAM Roles Anywhere trust model \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/rolesanywhere/latest/userguide/trust-model.html](https://docs.aws.amazon.com/rolesanywhere/latest/userguide/trust-model.html)  
39. AWS STS condition context keys for IAM Identity Center, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/condition-context-keys-sts-idc.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/condition-context-keys-sts-idc.html)  
40. Secure access keys \- AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/securing\_access-keys.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/securing_access-keys.html)  
41. Logging IAM and AWS STS API calls with AWS CloudTrail \- AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html)  
42. Assumed Role Actions (AWS): Improve CloudTrail Logging | Netskope, truy cập vào tháng 5 14, 2025, [https://www.netskope.com/blog/aws-improve-cloudtrail-logging-for-assumedrole-actions](https://www.netskope.com/blog/aws-improve-cloudtrail-logging-for-assumedrole-actions)  
43. Bootstrap AWS OIDC with GitHub Actions: Secure CI/CD Integration \- Paul M. Duvall, truy cập vào tháng 5 14, 2025, [https://www.paulmduvall.com/securing-ci-cd-pipelines-with-github-oidc-and-aws-iam/](https://www.paulmduvall.com/securing-ci-cd-pipelines-with-github-oidc-and-aws-iam/)  
44. How enable access to AWS STS AssumeRole \- Stack Overflow, truy cập vào tháng 5 14, 2025, [https://stackoverflow.com/questions/41337079/how-enable-access-to-aws-sts-assumerole](https://stackoverflow.com/questions/41337079/how-enable-access-to-aws-sts-assumerole)  
45. assumable role iam policy with conditions for aws:RequestTag and sts:SourceIdentity, truy cập vào tháng 5 14, 2025, [https://stackoverflow.com/questions/78045123/assumable-role-iam-policy-with-conditions-for-awsrequesttag-and-stssourceident](https://stackoverflow.com/questions/78045123/assumable-role-iam-policy-with-conditions-for-awsrequesttag-and-stssourceident)  
46. IAM AssumeRole Basics \- 1-minute IAM \- Amazon Web Services \- YouTube, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=UOWx-dy9Q9c](https://www.youtube.com/watch?v=UOWx-dy9Q9c)  
47. AssumeRole with External-Id \- 1-Minute AWS IAM Lesson \- YouTube, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=DLVlW3dOJww](https://www.youtube.com/watch?v=DLVlW3dOJww)  
48. How to Securely Access AWS APIs with IAM Roles Anywhere \- StratusGrid, truy cập vào tháng 5 14, 2025, [https://stratusgrid.com/blog/how-to-securely-access-aws-apis-with-iam-roles-anywhere](https://stratusgrid.com/blog/how-to-securely-access-aws-apis-with-iam-roles-anywhere)  
49. Understanding the AssumeRolePolicyDocument in AWS IAM | Baeldung on Ops, truy cập vào tháng 5 14, 2025, [https://www.baeldung.com/ops/aws-iam-assumerolepolicydocument](https://www.baeldung.com/ops/aws-iam-assumerolepolicydocument)  
50. AWS Identity And Access Management (IAM) | AWS Tutorial | Simplilearn \- YouTube, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=o0p04B7-NFY](https://www.youtube.com/watch?v=o0p04B7-NFY)  
51. How to Build a CI/CD Pipeline with AWS? \- GeeksforGeeks, truy cập vào tháng 5 14, 2025, [https://www.geeksforgeeks.org/how-to-build-a-ci-cd-pipeline-with-aws/](https://www.geeksforgeeks.org/how-to-build-a-ci-cd-pipeline-with-aws/)  
52. AWS CodePipeline, CodeBuild, and CodeDeploy: CI/CD on AWS \- DEV Community, truy cập vào tháng 5 14, 2025, [https://dev.to/imsushant12/aws-codepipeline-codebuild-and-codedeploy-cicd-on-aws-4fc6](https://dev.to/imsushant12/aws-codepipeline-codebuild-and-codedeploy-cicd-on-aws-4fc6)  
53. Allow CodeBuild to interact with other AWS services, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/codebuild/latest/userguide/setting-up-service-role.html](https://docs.aws.amazon.com/codebuild/latest/userguide/setting-up-service-role.html)  
54. How AWS CodePipeline works with IAM, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/codepipeline/latest/userguide/security\_iam\_service-with-iam.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/security_iam_service-with-iam.html)  
55. Policy best practices \- AWS CodePipeline, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/codepipeline/latest/userguide/security\_iam\_service-with-iam-policy-best-practices.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/security_iam_service-with-iam-policy-best-practices.html)  
56. Create a pipeline in CodePipeline that uses resources from another ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create-cross-account.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-create-cross-account.html)  
57. IAM tutorial: Delegate access across AWS accounts using IAM roles \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial\_cross-account-with-roles.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)  
58. Provision least-privilege IAM roles by deploying a role vending machine solution \- AWS Prescriptive Guidance \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/provision-least-privilege-iam-roles-by-deploying-a-role-vending-machine-solution.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/provision-least-privilege-iam-roles-by-deploying-a-role-vending-machine-solution.html)  
59. Implementing OIDC Authentication in GitLab CI/CD for AWS Deployments | Firefly, truy cập vào tháng 5 14, 2025, [https://www.firefly.ai/academy/implementing-oidc-authentication-in-gitlab-ci-cd-for-aws-deployments](https://www.firefly.ai/academy/implementing-oidc-authentication-in-gitlab-ci-cd-for-aws-deployments)  
60. How to Leverage AWS CloudTrail to Stay Compliant and Secure in the Cloud?, truy cập vào tháng 5 14, 2025, [https://www.cloudoptimo.com/blog/how-to-leverage-aws-cloudtrail-to-stay-compliant-and-secure-in-the-cloud/](https://www.cloudoptimo.com/blog/how-to-leverage-aws-cloudtrail-to-stay-compliant-and-secure-in-the-cloud/)  
61. AWS CloudTrail 101: How to Keep Tabs on Every Action in Your AWS Environment, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-cloudtrail](https://cloudchipr.com/blog/aws-cloudtrail)  
62. AWS CodePipeline "not authorized to assume role" due to missing CloudWatch Logs permissions, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUJ4w3CjUSTzyV1kQvFOyT\_A/aws-codepipeline-not-authorized-to-assume-role-due-to-missing-cloudwatch-logs-permissions](https://repost.aws/questions/QUJ4w3CjUSTzyV1kQvFOyT_A/aws-codepipeline-not-authorized-to-assume-role-due-to-missing-cloudwatch-logs-permissions)  
63. MITRE ATT\&CK for AWS: Understanding Tactics, Detection, and Mitigation \- Stream Security, truy cập vào tháng 5 14, 2025, [https://www.stream.security/post/mitre-attck-for-aws-understanding-tactics-detection-and-mitigation](https://www.stream.security/post/mitre-attck-for-aws-understanding-tactics-detection-and-mitigation)  
64. Logging CodePipeline API calls with AWS CloudTrail, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/codepipeline/latest/userguide/monitoring-cloudtrail-logs.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/monitoring-cloudtrail-logs.html)  
65. Defining Lambda function permissions with an execution role \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)  
66. Identity-based IAM policies for Lambda \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html](https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html)  
67. Managing permissions in AWS Lambda, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html)  
68. API Gateway resource policy examples \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-resource-policies-examples.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-resource-policies-examples.html)  
69. Best Practices for API Gateway \- what am I missing?, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUG7Nt\_CKwSVmSnCZnyP8MSQ/best-practices-for-api-gateway-what-am-i-missing](https://repost.aws/questions/QUG7Nt_CKwSVmSnCZnyP8MSQ/best-practices-for-api-gateway-what-am-i-missing)  
70. How to set up least privilege access to your encrypted Amazon SQS queue \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/how-to-set-up-least-privilege-access-to-your-encrypted-amazon-sqs-queue/](https://aws.amazon.com/blogs/security/how-to-set-up-least-privilege-access-to-your-encrypted-amazon-sqs-queue/)  
71. Update an SQS access policy for least privilege | AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/sqs-access-policy-least-privilege](https://repost.aws/knowledge-center/sqs-access-policy-least-privilege)  
72. AWS Policies \- AWS In Practice \- IT Assist, truy cập vào tháng 5 14, 2025, [https://awsinpractice.itassist.com/study-group/aws-certified-solutions-architect-associate/domain-1-design-secure-architectures/task-statement-1.1-design-secure-access-to-aws-resources/aws-policies](https://awsinpractice.itassist.com/study-group/aws-certified-solutions-architect-associate/domain-1-design-secure-architectures/task-statement-1.1-design-secure-access-to-aws-resources/aws-policies)  
73. Applying the principles of least privilege in AWS lambda: Specialized Lambda functions compared with all-purpose functions | Orchestra, truy cập vào tháng 5 14, 2025, [https://www.getorchestra.io/guides/applying-the-principles-of-least-privilege-in-aws-lambda-specialized-lambda-functions-compared-with-all-purpose-functions](https://www.getorchestra.io/guides/applying-the-principles-of-least-privilege-in-aws-lambda-specialized-lambda-functions-compared-with-all-purpose-functions)  
74. Security best practices in IAM \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)  
75. Accessing Amazon S3 tables using the AWS Glue Iceberg REST endpoint, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables-integrating-glue-endpoint.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables-integrating-glue-endpoint.html)  
76. Permission requirements for S3 Tables SSE-KMS encryption \- Amazon Simple Storage Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables-kms-permissions.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables-kms-permissions.html)  
77. Registering an encrypted Amazon S3 location \- AWS Lake Formation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/register-encrypted.html](https://docs.aws.amazon.com/lake-formation/latest/dg/register-encrypted.html)  
78. Registering an encrypted Amazon S3 location across AWS accounts \- AWS Lake Formation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/register-cross-encrypted.html](https://docs.aws.amazon.com/lake-formation/latest/dg/register-cross-encrypted.html)  
79. Running ETL jobs on Amazon S3 tables with AWS Glue \- Amazon Simple Storage Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables-integrating-glue.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables-integrating-glue.html)  
80. AWS Lake Formation and Glue Integration: A Step-by-Step Guide | DataCamp, truy cập vào tháng 5 14, 2025, [https://www.datacamp.com/tutorial/aws-lake-formation-and-glue-integration](https://www.datacamp.com/tutorial/aws-lake-formation-and-glue-integration)  
81. Lake Formation permissions reference \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/lf-permissions-reference.html](https://docs.aws.amazon.com/lake-formation/latest/dg/lf-permissions-reference.html)  
82. Setting up AWS Lake Formation with IAM Identity Center, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/tip-tutorial-lf.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/tip-tutorial-lf.html)  
83. Amazon SageMaker Lakehouse now supports attribute-based access control \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/big-data/amazon-sagemaker-lakehouse-now-supports-attribute-based-access-control/](https://aws.amazon.com/blogs/big-data/amazon-sagemaker-lakehouse-now-supports-attribute-based-access-control/)  
84. Attribute-based access control \- AWS Lake Formation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/attribute-based-access-control.html](https://docs.aws.amazon.com/lake-formation/latest/dg/attribute-based-access-control.html)  
85. AWS Lake Formation: What is it and what is it used for? \- Paradigma Digital, truy cập vào tháng 5 14, 2025, [https://en.paradigmadigital.com/dev/aws-lake-formation-what-is-it-what-is-it-used-for/](https://en.paradigmadigital.com/dev/aws-lake-formation-what-is-it-what-is-it-used-for/)  
86. AWS service integrations with Lake Formation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/service-integrations.html](https://docs.aws.amazon.com/lake-formation/latest/dg/service-integrations.html)  
87. Troubleshoot AWS Glue Crawler or ETL job failures with the error "Insufficient Lake Formation permission(s)", truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/glue-insufficient-lakeformation-permissions](https://repost.aws/knowledge-center/glue-insufficient-lakeformation-permissions)  
88. AWS Glue \- Decrypt class, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-pyspark-transforms-Decrypt.html](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-pyspark-transforms-Decrypt.html)  
89. I can't connect to an external s3 bucket as a data source for a glue crawler | AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUYVB28aKOTmmoFjd92NNceA/i-can-t-connect-to-an-external-s3-bucket-as-a-data-source-for-a-glue-crawler](https://repost.aws/questions/QUYVB28aKOTmmoFjd92NNceA/i-can-t-connect-to-an-external-s3-bucket-as-a-data-source-for-a-glue-crawler)  
90. Create an AWS Glue table \- Splunk Documentation, truy cập vào tháng 5 14, 2025, [https://docs.splunk.com/Documentation/SplunkCloud/9.3.2408/FederatedSearch/fss3CreateTable](https://docs.splunk.com/Documentation/SplunkCloud/9.3.2408/FederatedSearch/fss3CreateTable)  
91. Lake Formation personas and IAM permissions reference \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/permissions-reference.html](https://docs.aws.amazon.com/lake-formation/latest/dg/permissions-reference.html)  
92. Requirements for roles used to register locations \- AWS Lake Formation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lake-formation/latest/dg/registration-role.html](https://docs.aws.amazon.com/lake-formation/latest/dg/registration-role.html)  
93. Step 3: Configure security settings \- AWS Glue, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/glue/latest/dg/define-crawler-configure-security-settings.html](https://docs.aws.amazon.com/glue/latest/dg/define-crawler-configure-security-settings.html)  
94. Encrypting data written by AWS Glue \- AWS Glue, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/glue/latest/dg/encryption-security-configuration.html](https://docs.aws.amazon.com/glue/latest/dg/encryption-security-configuration.html)  
95. Partitions \- AWS Fault Isolation Boundaries, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/partitions.html](https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/partitions.html)  
96. Considerations for choosing an AWS Region \- AWS IAM Identity Center, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/identity-center-region-considerations.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/identity-center-region-considerations.html)  
97. Manage AWS STS in an AWS Region \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_credentials\_temp\_enable-regions.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html)  
98. IAM best practices – the complete guide to AWS infrastructure security | RST Software, truy cập vào tháng 5 14, 2025, [https://www.rst.software/blog/iam-best-practices---the-complete-guide-to-aws-infrastructure-security](https://www.rst.software/blog/iam-best-practices---the-complete-guide-to-aws-infrastructure-security)  
99. AWS Identity and Access Management (IAM) Best Practices \- Amazon Web Services, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/resources/best-practices/](https://aws.amazon.com/iam/resources/best-practices/)  
100. Security-Reference-Guide/cloud.md at main \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/s0cm0nkey/Security-Reference-Guide/blob/main/cloud.md](https://github.com/s0cm0nkey/Security-Reference-Guide/blob/main/cloud.md)  
101. cloudsplaining/README.md at master · salesforce/cloudsplaining ..., truy cập vào tháng 5 14, 2025, [https://github.com/salesforce/cloudsplaining/blob/master/README.md](https://github.com/salesforce/cloudsplaining/blob/master/README.md)  
102. Cloudsplaining, truy cập vào tháng 5 14, 2025, [https://cloudsplaining.readthedocs.io/en/latest/](https://cloudsplaining.readthedocs.io/en/latest/)  
103. AWS IAM Hands-on Lab Tutorial | Learn Identity and Access Management in 15 minutes, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=qY-G06E232Q](https://www.youtube.com/watch?v=qY-G06E232Q)  
104. IAM tutorials \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorials.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorials.html)  
105. FalconForceTeam/dAWShund: Putting a leash on naughty ... \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/FalconForceTeam/dAWShund](https://github.com/FalconForceTeam/dAWShund)  
106. How to use ScoutSuite? \- YouTube, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=S55MB8fkIV4](https://www.youtube.com/watch?v=S55MB8fkIV4)  
107. IAM Access Analyzer Policy Generation Tutorial \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/awstv/watch/856b1a0fe71/](https://aws.amazon.com/awstv/watch/856b1a0fe71/)  
108. IAM Access Analyzer policy generation services \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-generation-action-last-accessed-support.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-generation-action-last-accessed-support.html)  
109. AWS IAM Access Analyzer, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/access-analyzer/](https://aws.amazon.com/iam/access-analyzer/)  
110. IAM least privilege \- how restricted? : r/aws \- Reddit, truy cập vào tháng 5 14, 2025, [https://www.reddit.com/r/aws/comments/1adhuvg/iam\_least\_privilege\_how\_restricted/](https://www.reddit.com/r/aws/comments/1adhuvg/iam_least_privilege_how_restricted/)  
111. AWS Security: A Comprehensive Guide to Protecting Your Cloud Infrastructure, truy cập vào tháng 5 14, 2025, [https://securedebug.com/aws-security-protecting-cloud-infrastructure/](https://securedebug.com/aws-security-protecting-cloud-infrastructure/)  
112. cloudsplaining/CODE\_OF\_CONDUCT.md at master \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/salesforce/cloudsplaining/blob/master/CODE\_OF\_CONDUCT.md](https://github.com/salesforce/cloudsplaining/blob/master/CODE_OF_CONDUCT.md)  
113. cloudsplaining.md \- 3ls3if/Cybersecurity-Notes \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/3ls3if/Cybersecurity-Notes/blob/main/readme/cloud-pentesting/aws-pentesting/tools/cloudsplaining.md](https://github.com/3ls3if/Cybersecurity-Notes/blob/main/readme/cloud-pentesting/aws-pentesting/tools/cloudsplaining.md)  
114. AWS IAM Security Assessment: Auditing and Hardening Your AWS Identity and Access Management | Cloud Native Security \- Armur AI, truy cập vào tháng 5 14, 2025, [https://armur.ai/cloud-native-security/aws/aws/aws-iam-security-assessment/](https://armur.ai/cloud-native-security/aws/aws/aws-iam-security-assessment/)  
115. AWS IAM Privilege Escalation Labs \- Cybr, truy cập vào tháng 5 14, 2025, [https://cybr.com/courses/iam-privilege-escalation-labs/](https://cybr.com/courses/iam-privilege-escalation-labs/)  
116. nccgroup/PMapper: A tool for quickly evaluating IAM permissions in AWS. \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/nccgroup/PMapper](https://github.com/nccgroup/PMapper)  
117. How to visualize IAM Access Analyzer policy validation findings with QuickSight \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/how-to-visualize-iam-access-analyzer-policy-validation-findings-with-quicksight/](https://aws.amazon.com/blogs/security/how-to-visualize-iam-access-analyzer-policy-validation-findings-with-quicksight/)  
118. Auditing Cloud Infrastructure using Scout Suite \- YouTube, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=d8O9Z-KBTcg](https://www.youtube.com/watch?v=d8O9Z-KBTcg)  
119. Scanning Your AWS Environment for Vulnerabilities With ScoutSuite \- Simple Thread, truy cập vào tháng 5 14, 2025, [https://www.simplethread.com/scanning-your-aws-environment-for-vulnerabilities-with-scoutsuite/](https://www.simplethread.com/scanning-your-aws-environment-for-vulnerabilities-with-scoutsuite/)  
120. utwente-scs/iam-collector: Tool that collects and monitors ... \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/utwente-scs/iam-collector](https://github.com/utwente-scs/iam-collector)  
121. vladionescu/aws-audit-scripts: Collection of AWS helper ... \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/vladionescu/aws-audit-scripts](https://github.com/vladionescu/aws-audit-scripts)  
122. Run Python scripts from GitHub \- AWS Systems Manager, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-github-python.html](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-github-python.html)  
123. How to Automate IAM Best Practices in CI/CD with IAM Access Analyzer \- DEV Community, truy cập vào tháng 5 14, 2025, [https://dev.to/aws-builders/how-to-automate-iam-best-practices-in-cicd-with-iam-access-analyzer-1keo](https://dev.to/aws-builders/how-to-automate-iam-best-practices-in-cicd-with-iam-access-analyzer-1keo)  
124. IAM Least Privilege | AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUDAV1Zdv2Snm20Tjkz7SGKQ/iam-least-privilege](https://repost.aws/questions/QUDAV1Zdv2Snm20Tjkz7SGKQ/iam-least-privilege)  
125. Security Hub controls for IAM \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/securityhub/latest/userguide/iam-controls.html](https://docs.aws.amazon.com/securityhub/latest/userguide/iam-controls.html)  
126. iam-user-mfa-enabled \- AWS Config, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/config/latest/developerguide/iam-user-mfa-enabled.html](https://docs.aws.amazon.com/config/latest/developerguide/iam-user-mfa-enabled.html)  
127. Automating Cloud Compliance Monitoring with AWS Config and Lambda \- NimbusStack, truy cập vào tháng 5 14, 2025, [https://nimbusstack.com/automating-cloud-compliance-monitoring-with-aws-config-and-lambda/](https://nimbusstack.com/automating-cloud-compliance-monitoring-with-aws-config-and-lambda/)  
128. What is Identity Threat Detection and Response (ITDR)? \- Wiz, truy cập vào tháng 5 14, 2025, [https://www.wiz.io/academy/identity-threat-detection-and-response-itdr](https://www.wiz.io/academy/identity-threat-detection-and-response-itdr)  
129. Secure Credentials: Rule for Disabling Unused Accounts \- CloudDefense.AI, truy cập vào tháng 5 14, 2025, [https://www.clouddefense.ai/compliance-rules/cis-v130/iam/cis-v130-1-12](https://www.clouddefense.ai/compliance-rules/cis-v130/iam/cis-v130-1-12)  
130. Automate Deactivation of Dormant AWS IAM Users \- hashnode.dev, truy cập vào tháng 5 14, 2025, [https://hrushikeshk.hashnode.dev/aws-deactivate-iam](https://hrushikeshk.hashnode.dev/aws-deactivate-iam)  
131. Securing AWS Lambda Functions With IAM Roles And Policies | GeeksforGeeks, truy cập vào tháng 5 14, 2025, [https://www.geeksforgeeks.org/securing-aws-lambda-functions-with-iam-roles-and-policies/](https://www.geeksforgeeks.org/securing-aws-lambda-functions-with-iam-roles-and-policies/)  
132. ANZ Summit 2022 Dev Lab: Automate Setting AWS Account Password Policy for IAM Users, truy cập vào tháng 5 14, 2025, [https://github.com/aws-samples/devlab-iam-password-policy](https://github.com/aws-samples/devlab-iam-password-policy)  
133. Tutorial: Using an Amazon S3 trigger to invoke a Lambda function \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html](https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html)  
134. Actions in AWS lambda: RemovePermission \- Orchestra, truy cập vào tháng 5 14, 2025, [https://www.getorchestra.io/guides/actions-in-aws-lambda-removepermission](https://www.getorchestra.io/guides/actions-in-aws-lambda-removepermission)  
135. delete\_user\_policy \- Boto3 1.37.32 documentation \- AWS, truy cập vào tháng 5 14, 2025, [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iam/client/delete\_user\_policy.html](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iam/client/delete_user_policy.html)  
136. Working with IAM policies \- Boto3 1.38.12 documentation, truy cập vào tháng 5 14, 2025, [https://boto3.amazonaws.com/v1/documentation/api/latest/guide/iam-example-policies.html](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/iam-example-policies.html)  
137. Create AWS Config custom rules by using AWS CloudFormation Guard policies \- AWS Prescriptive Guidance \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/create-aws-config-custom-rules-by-using-aws-cloudformation-guard-policies.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/create-aws-config-custom-rules-by-using-aws-cloudformation-guard-policies.html)  
138. awslabs/aws-iam-access-analyzer-policy-validation-config-rule \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/awslabs/aws-iam-access-analyzer-policy-validation-config-rule](https://github.com/awslabs/aws-iam-access-analyzer-policy-validation-config-rule)  
139. Least Privileges for IAM user to enable AWS Config, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUufTsdPtETMir6m4TUSJbag/least-privileges-for-iam-user-to-enable-aws-config](https://repost.aws/questions/QUufTsdPtETMir6m4TUSJbag/least-privileges-for-iam-user-to-enable-aws-config)  
140. iam-user-no-policies-check \- AWS Config \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/config/latest/developerguide/iam-user-no-policies-check.html](https://docs.aws.amazon.com/config/latest/developerguide/iam-user-no-policies-check.html)  
141. How can I resolve access denied issues caused by permissions boundaries? \- AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/iam-access-denied-permissions-boundary](https://repost.aws/knowledge-center/iam-access-denied-permissions-boundary)  
142. Check MFA status \- AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_credentials\_mfa\_checking-status.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_checking-status.html)  
143. Creating AWS Config Custom Policy Rules, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config\_develop-rules\_cfn-guard.html](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_cfn-guard.html)  
144. AWS Data Breach: Lesson From 4 High Profile Breaches | BlackFog, truy cập vào tháng 5 14, 2025, [https://www.blackfog.com/aws-data-breach/](https://www.blackfog.com/aws-data-breach/)  
145. Cloud Security's Perfect Storm: Dissecting the Capital One Breach \- Destination Certification, truy cập vào tháng 5 14, 2025, [https://destcert.com/resources/capital-one-breach/](https://destcert.com/resources/capital-one-breach/)  
146. Lessons learned: The Capital One breach \- Infosec, truy cập vào tháng 5 14, 2025, [https://www.infosecinstitute.com/resources/news/lessons-learned-the-capital-one-breach/](https://www.infosecinstitute.com/resources/news/lessons-learned-the-capital-one-breach/)  
147. Top 15 AWS Project Ideas in 2025 \[Beginner to Advanced\] \- KnowledgeHut, truy cập vào tháng 5 14, 2025, [https://www.knowledgehut.com/blog/cloud-computing/aws-projects](https://www.knowledgehut.com/blog/cloud-computing/aws-projects)  
148. 30+ AWS Projects Ideas for Beginners to Practice in 2025 \- ProjectPro, truy cập vào tháng 5 14, 2025, [https://www.projectpro.io/article/aws-projects-ideas-for-beginners/453](https://www.projectpro.io/article/aws-projects-ideas-for-beginners/453)  
149. Top Cloud Architect Skills for Success and Growth 2025 \- upGrad, truy cập vào tháng 5 14, 2025, [https://www.upgrad.com/blog/cloud-architect-skills/](https://www.upgrad.com/blog/cloud-architect-skills/)  
150. Designing Scalable IAM Architectures for Multi-Cloud Environments \- Adnovum, truy cập vào tháng 5 14, 2025, [https://www.adnovum.com/blog/designing-scalable-iam-architectures-for-multi-cloud-environments](https://www.adnovum.com/blog/designing-scalable-iam-architectures-for-multi-cloud-environments)  
151. Identity management in a multi-account environment | AWS Marketplace, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/marketplace/solutions/control-tower/identity-management](https://aws.amazon.com/marketplace/solutions/control-tower/identity-management)  
152. Building a Stronger Security Posture with AWS Security Hub \- DEV Community, truy cập vào tháng 5 14, 2025, [https://dev.to/aws-builders/building-a-stronger-security-posture-with-aws-security-hub-2foi](https://dev.to/aws-builders/building-a-stronger-security-posture-with-aws-security-hub-2foi)  
153. Compliance | AWS In Practice \- IT Assist, truy cập vào tháng 5 14, 2025, [https://awsinpractice.itassist.com/study-group/aws-certified-solutions-architect-associate/domain-1-design-secure-architectures/task-statement-1.3-determine-appropriate-data-security-controls/compliance](https://awsinpractice.itassist.com/study-group/aws-certified-solutions-architect-associate/domain-1-design-secure-architectures/task-statement-1.3-determine-appropriate-data-security-controls/compliance)  
154. Design patterns for multi-tenant access control on Amazon S3 | AWS Storage Blog, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/storage/design-patterns-for-multi-tenant-access-control-on-amazon-s3/](https://aws.amazon.com/blogs/storage/design-patterns-for-multi-tenant-access-control-on-amazon-s3/)  
155. Designing Multi-tenant SaaS Architecture on AWS: Complete Guide \- Clickittech, truy cập vào tháng 5 14, 2025, [https://www.clickittech.com/software-development/multi-tenant-architecture/](https://www.clickittech.com/software-development/multi-tenant-architecture/)  
156. Verify operational best practices for PCI DSS 4.0 by using AWS Config \- AWS Prescriptive Guidance \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/verify-ops-best-practices-pci-dss-4.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/verify-ops-best-practices-pci-dss-4.html)  
157. Transforming transactions: Streamlining PCI compliance using AWS serverless architecture, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/transforming-transactions-streamlining-pci-compliance-using-aws-serverless-architecture/](https://aws.amazon.com/blogs/security/transforming-transactions-streamlining-pci-compliance-using-aws-serverless-architecture/)  
158. HIPAA Compliance \- Amazon Web Services (AWS), truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/compliance/hipaa-compliance/](https://aws.amazon.com/compliance/hipaa-compliance/)  
159. AWS HIPAA compliance checklist for modern healthtechs \- Just After Midnight, truy cập vào tháng 5 14, 2025, [https://www.justaftermidnight247.com/insights/aws-hipaa-compliance-checklist-healthtechs/](https://www.justaftermidnight247.com/insights/aws-hipaa-compliance-checklist-healthtechs/)  
160. HIPAA on AWS: Requirements and Best Practices \- Exabeam, truy cập vào tháng 5 14, 2025, [https://www.exabeam.com/explainers/hipaa-compliance/hipaa-on-aws-requirements-and-best-practices/](https://www.exabeam.com/explainers/hipaa-compliance/hipaa-on-aws-requirements-and-best-practices/)  
161. 7 IAM Security Best Practices for HIPAA Compliance, truy cập vào tháng 5 14, 2025, [https://www.logicworks.com/blog/2015/05/aws-iam-security-cloud-hipaa-compliance/](https://www.logicworks.com/blog/2015/05/aws-iam-security-cloud-hipaa-compliance/)  
162. SOC 2, GDPR, HIPAA Compliance on AWS: A Complete Guide \- NovelVista, truy cập vào tháng 5 14, 2025, [https://www.novelvista.com/blogs/cloud-and-aws/soc-2-gdpr-hipaa-compliance-on-aws-a-complete-guide](https://www.novelvista.com/blogs/cloud-and-aws/soc-2-gdpr-hipaa-compliance-on-aws-a-complete-guide)  
163. Understanding HIPAA-Compliance AWS Architecture \- TechVariable, truy cập vào tháng 5 14, 2025, [https://techvariable.com/blogs/understanding-hipaa-compliance-aws-architecture](https://techvariable.com/blogs/understanding-hipaa-compliance-aws-architecture)  
164. Unleashing the multicloud advantage: Identity and Access Management (IAM), truy cập vào tháng 5 14, 2025, [https://techcommunity.microsoft.com/blog/marketplace-blog/unleashing-the-multicloud-advantage-identity-and-access-management-iam/4401091](https://techcommunity.microsoft.com/blog/marketplace-blog/unleashing-the-multicloud-advantage-identity-and-access-management-iam/4401091)  
165. truy cập vào tháng 1 1, 1970, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_credentials\_temp\_control-access\_source-identity.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_source-identity.html)  
166. truy cập vào tháng 1 1, 1970, [https://aws.amazon.com/blogs/security/how-to-use-iam-roles-to-set-source-identity-for-monitoring-in-aws-cloudtrail/](https://aws.amazon.com/blogs/security/how-to-use-iam-roles-to-set-source-identity-for-monitoring-in-aws-cloudtrail/)  
167. truy cập vào tháng 1 1, 1970, [https://aws.amazon.com/blogs/security/how-to-attribute-actions-to-the-original-user-after-a-series-of-role-assumptions/](https://aws.amazon.com/blogs/security/how-to-attribute-actions-to-the-original-user-after-a-series-of-role-assumptions/)  
168. AWS Cloud Foundation | Module 4- LAB 1 Introduction to AWS IAM | Step-by-Step Guide, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=bwaJwkPr6yA](https://www.youtube.com/watch?v=bwaJwkPr6yA)  
169. link-hub/README.md at main \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/Tanq16/link-hub/blob/main/README.md](https://github.com/Tanq16/link-hub/blob/main/README.md)  
170. IAM Hands on Walkthrough \- YouTube, truy cập vào tháng 5 14, 2025, [https://www.youtube.com/watch?v=4jLwjylseXA](https://www.youtube.com/watch?v=4jLwjylseXA)  
171. Hands-on tutorials \- Cloudscape Design System, truy cập vào tháng 5 14, 2025, [https://cloudscape.design/patterns/general/onboarding/hands-on-tutorials/](https://cloudscape.design/patterns/general/onboarding/hands-on-tutorials/)  
172. Automating Cost Optimization Governance with AWS Config | AWS Cloud Operations Blog, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/mt/automating-cost-optimization-governance-with-aws-config/](https://aws.amazon.com/blogs/mt/automating-cost-optimization-governance-with-aws-config/)  
173. Create a role to give permissions to an IAM user \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create\_for-user.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)  
174. Control CloudFormation access with AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/control-access-with-iam.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/control-access-with-iam.html)  
175. How to use the PassRole permission with IAM roles | AWS Security Blog, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/how-to-use-the-passrole-permission-with-iam-roles/](https://aws.amazon.com/blogs/security/how-to-use-the-passrole-permission-with-iam-roles/)  
176. aws config permission error \- AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUGgsN4GsRRkWBQGVf4Q5cdw/aws-config-permission-error](https://repost.aws/questions/QUGgsN4GsRRkWBQGVf4Q5cdw/aws-config-permission-error)  
177. truy cập vào tháng 1 1, 1970, [https://aws.amazon.com/blogs/mt/implement-preventive-and-detective-iam-controls-using-aws-cloudformation-guard-and-aws-config/](https://aws.amazon.com/blogs/mt/implement-preventive-and-detective-iam-controls-using-aws-cloudformation-guard-and-aws-config/)