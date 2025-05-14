# **Lớp học Chuyên sâu AWS IAM (AWS IAM Intensive Masterclass)**

## **Phần I: Giới thiệu Tổng quan về AWS IAM**

AWS Identity and Access Management (IAM) là một dịch vụ web cho phép kiểm soát truy cập an toàn vào các tài nguyên AWS. Nó là một thành phần nền tảng và thiết yếu trong kiến trúc bảo mật của bất kỳ môi trường AWS nào, đóng vai trò trung tâm trong việc xác định ai có thể làm gì đối với các tài nguyên nào.

### **A. Tầm quan trọng của IAM trong Kiến trúc Bảo mật AWS**

IAM là dịch vụ cốt lõi để quản lý danh tính và quyền truy cập trong Amazon Web Services. Nó cho phép các tổ chức xác thực (xác định ai đang đăng nhập) và ủy quyền (cấp quyền) cho người dùng và dịch vụ truy cập vào tài nguyên AWS một cách có kiểm soát.1 Về bản chất, IAM hoạt động như một người gác cổng trung tâm, nơi các quản trị viên định nghĩa rõ ràng "ai" (danh tính) có thể thực hiện "hành động gì" trên "tài nguyên nào", và trong những "điều kiện" nào.

Một trong những nguyên tắc bảo mật cơ bản mà IAM giúp thực thi là **nguyên tắc đặc quyền tối thiểu (Principle of Least Privilege \- PoLP)**. Nguyên tắc này quy định rằng mỗi danh tính – dù là người dùng, nhóm, vai trò hay ứng dụng – chỉ nên được cấp những quyền hạn tối thiểu cần thiết để thực hiện nhiệm vụ của mình, và không hơn thế.2 Việc tuân thủ chặt chẽ PoLP giúp giảm thiểu đáng kể bề mặt tấn công và hạn chế thiệt hại tiềm ẩn nếu một danh tính bị xâm phạm.

Việc thiết kế và triển khai một chiến lược IAM đúng đắn không chỉ đơn thuần là một yêu cầu kỹ thuật; nó mang những hàm ý chiến lược sâu sắc, ảnh hưởng trực tiếp đến nhiều khía cạnh của việc vận hành trên nền tảng AWS.  
Thứ nhất, về an ninh (security posture), một cấu hình IAM yếu kém, chẳng hạn như việc cấp quyền quá rộng rãi hoặc quản lý thông tin xác thực lỏng lẻo, có thể mở đường cho các hành vi truy cập trái phép, thay đổi cấu hình không mong muốn, hoặc thậm chí là rò rỉ dữ liệu nhạy cảm. Ngược lại, một hệ thống IAM được thiết kế tốt với các chính sách chi tiết và kiểm soát chặt chẽ sẽ là một lớp phòng thủ vững chắc.  
Thứ hai, về tuân thủ (compliance), nhiều quy định và tiêu chuẩn ngành (ví dụ: GDPR, HIPAA, PCI DSS) yêu cầu các tổ chức phải có cơ chế kiểm soát truy cập mạnh mẽ và khả năng kiểm toán chi tiết. IAM cung cấp các công cụ cần thiết để đáp ứng những yêu cầu này, nhưng việc cấu hình sai có thể dẫn đến vi phạm tuân thủ, kéo theo các hình phạt tài chính và tổn thất uy tín.  
Thứ ba, về chi phí (cost implication), quyền truy cập không được kiểm soát chặt chẽ có thể dẫn đến việc sử dụng tài nguyên không cần thiết, sai mục đích hoặc vượt quá ngân sách dự kiến. Ví dụ, một người dùng với quyền tạo tài nguyên không giới hạn có thể vô tình hoặc cố ý khởi tạo các dịch vụ tốn kém mà không có sự giám sát.  
Cuối cùng, về khả năng vận hành linh hoạt (operational agility), một hệ thống IAM quá phức tạp, khó quản lý hoặc các quy trình cấp phát quyền chậm chạp có thể cản trở tốc độ làm việc của các đội ngũ phát triển và vận hành. Mục tiêu là cân bằng giữa bảo mật và sự linh hoạt, đảm bảo rằng các quy trình IAM hỗ trợ thay vì cản trở hoạt động kinh doanh.  
Do đó, việc đầu tư thời gian và nguồn lực để xây dựng một nền tảng IAM vững chắc là một quyết định chiến lược, đảm bảo sự an toàn, tuân thủ, tối ưu chi phí và hiệu quả hoạt động cho toàn bộ hệ sinh thái AWS của tổ chức.

### **B. Các Khái niệm Cốt lõi**

Để hiểu sâu về IAM, cần nắm vững các khái niệm nền tảng sau:

* **Identities (Định danh):** Đây là các thực thể trong IAM mà bạn sẽ cấp quyền. Chúng bao gồm:  
  * **IAM Users (Người dùng IAM):** Đại diện cho một cá nhân hoặc một ứng dụng cần tương tác với AWS.1 Mỗi người dùng có một bộ thông tin xác thực dài hạn riêng (ví dụ: mật khẩu, khóa truy cập).  
  * **IAM Groups (Nhóm IAM):** Là một tập hợp các IAM Users. Thay vì gán quyền cho từng người dùng, bạn có thể gán quyền cho một nhóm, và tất cả người dùng trong nhóm đó sẽ thừa hưởng các quyền này.1  
  * **IAM Roles (Vai trò IAM):** Là một định danh IAM với các quyền cụ thể mà bất kỳ thực thể đáng tin cậy nào (người dùng, ứng dụng, hoặc dịch vụ AWS) cũng có thể đảm nhận tạm thời. Roles không có thông tin xác thực dài hạn cố định.1  
* **Authentication (Xác thực):** Là quá trình AWS sử dụng để xác minh danh tính của một *principal* (một người dùng, vai trò, hoặc ứng dụng) đang cố gắng đăng nhập hoặc thực hiện một yêu cầu đối với AWS.2 Quá trình này trả lời câu hỏi "Bạn là ai?". Các phương thức xác thực phổ biến bao gồm tên người dùng/mật khẩu, khóa truy cập, và Multi-Factor Authentication (MFA).  
* **Authorization (Ủy quyền):** Sau khi một principal đã được xác thực thành công, quá trình ủy quyền sẽ diễn ra để xác định những hành động (actions) nào mà principal đó được phép (hoặc bị từ chối) thực hiện trên các tài nguyên (resources) AWS cụ thể, và trong những điều kiện (conditions) nào.2 Quá trình này trả lời câu hỏi "Bạn được phép làm gì?". Ủy quyền trong IAM được thực hiện thông qua các *policies*.  
* **Resources (Tài nguyên):** Là các đối tượng tồn tại trong tài khoản AWS của bạn mà bạn muốn kiểm soát quyền truy cập, ví dụ như các máy chủ ảo Amazon EC2, các bucket lưu trữ Amazon S3, các bảng cơ sở dữ liệu Amazon DynamoDB, các hàm AWS Lambda, v.v.

Xác thực và ủy quyền là hai trụ cột không thể tách rời trong cơ chế hoạt động của IAM, tạo thành một vòng kiểm soát truy cập hoàn chỉnh. Không thể có một hệ thống ủy quyền hiệu quả nếu thiếu đi các cơ chế xác thực mạnh mẽ để đảm bảo rằng chỉ những danh tính hợp lệ mới có thể yêu cầu quyền truy cập. Ngược lại, ngay cả khi có một hệ thống xác thực cực kỳ nghiêm ngặt, nếu cơ chế ủy quyền không được thiết kế chi tiết và tuân thủ nguyên tắc đặc quyền tối thiểu, thì một khi danh tính hợp lệ bị xâm phạm (ví dụ, thông qua tấn công kỹ thuật xã hội hoặc phần mềm độc hại), kẻ tấn công vẫn có thể gây ra thiệt hại nghiêm trọng với những quyền hạn rộng lớn đã được cấp.  
Ví dụ, nếu một kẻ tấn công vượt qua được lớp xác thực (chẳng hạn như đánh cắp mật khẩu của một người dùng không sử dụng MFA), một hệ thống ủy quyền được thiết kế tốt theo nguyên tắc đặc quyền tối thiểu sẽ là lớp phòng thủ quan trọng tiếp theo, giới hạn phạm vi hành động của kẻ tấn công chỉ trong những quyền hạn thực sự cần thiết cho công việc của người dùng đó. Điều này giúp giảm thiểu thiệt hại tiềm ẩn.  
Ngược lại, nếu một tổ chức yêu cầu xác thực rất mạnh (ví dụ, MFA bắt buộc cho tất cả người dùng) nhưng sau đó lại cấp quyền quản trị viên (AdministratorAccess) cho hầu hết mọi người, thì khi một tài khoản người dùng hợp lệ bị xâm phạm, kẻ tấn công sẽ có toàn quyền kiểm soát môi trường AWS, bất chấp cơ chế xác thực ban đầu mạnh mẽ đến đâu.  
Do đó, một chiến lược IAM toàn diện phải đầu tư đồng đều vào việc củng cố cả hai khía cạnh: triển khai các cơ chế xác thực mạnh mẽ (như mật khẩu phức tạp, yêu cầu MFA) và xây dựng các chính sách ủy quyền chi tiết, tuân thủ nghiêm ngặt nguyên tắc đặc quyền tối thiểu.

### **C. Lịch sử Phát triển của AWS IAM và các Cột mốc Quan trọng**

AWS IAM không phải là một dịch vụ tĩnh mà đã trải qua nhiều giai đoạn phát triển, liên tục được cải tiến và bổ sung các tính năng mới để đáp ứng nhu cầu bảo mật ngày càng phức tạp của khách hàng và phù hợp với các xu hướng công nghệ.

* **Giai đoạn 1: IAM Users và Truy cập Tĩnh** 4  
  * Trong những ngày đầu, mô hình IAM chủ yếu xoay quanh các **IAM Users** tĩnh với **thông tin xác thực dài hạn** (long-lived credentials) như access keys được cấu hình và phân phối thủ công.  
  * Mô hình này tuy đơn giản cho các thiết lập nhỏ lẻ hoặc tài khoản đơn lẻ, nhưng nhanh chóng bộc lộ nhiều hạn chế khi quy mô tăng lên. Các vấn đề chính bao gồm: tài khoản thường được cấp quyền quá rộng rãi, vi phạm nguyên tắc đặc quyền tối thiểu; tình trạng "truy cập thường trực" (standing access) làm tăng rủi ro; thông tin xác thực (đặc biệt là access keys) có xu hướng bị "tràn lan" (credential sprawl), khó kiểm soát và kiểm toán; và chi phí vận hành cao cho việc xoay vòng, thu hồi thông tin xác thực và ứng phó sự cố.  
* **Giai đoạn 2: Roles, STS và Truy cập Liên kết** 4  
  * Để giải quyết những hạn chế của mô hình truy cập tĩnh, AWS đã giới thiệu **IAM Roles** và **Security Token Service (STS)**. IAM Roles cho phép các thực thể đáng tin cậy đảm nhận vai trò và nhận **thông tin xác thực tạm thời** (temporary credentials) thông qua các chính sách tin cậy (trust policies). STS là dịch vụ nền tảng cho phép yêu cầu và quản lý các session truy cập dựa trên thông tin xác thực tạm thời này, với thời gian hiệu lực được xác định trước.  
  * Sự ra đời của Roles và STS đã mở đường cho **truy cập liên kết (federated access)**, cho phép các công ty kết nối hệ thống quản lý danh tính hiện có của họ (Identity Providers \- IdPs) như Microsoft Active Directory, Okta, Entra ID (Azure AD), hoặc Google Workspace với AWS.  
  * Những cải tiến này đã giải quyết nhiều vấn đề: cải thiện đáng kể việc quản lý vòng đời truy cập (đặc biệt cho các quy trình thêm mới, thay đổi, và cho nhân viên nghỉ việc); tăng cường đáng kể tình hình bảo mật bằng cách giảm thiểu sự phụ thuộc vào truy cập tĩnh và thông tin xác thực dài hạn; và cải thiện khả năng kiểm toán thông qua việc ghi log session và liên kết danh tính.  
* **Giai đoạn 3: AWS Organizations và Mô hình Đa tài khoản** 4  
  * Khi việc sử dụng AWS ngày càng phổ biến và quy mô triển khai của các tổ chức ngày càng lớn, chiến lược **đa tài khoản (multi-account strategy)** sử dụng **AWS Organizations** đã trở thành một phương pháp tốt nhất. Mô hình này cho phép các tổ chức phân tách workloads, môi trường (ví dụ: dev, test, prod), các đơn vị kinh doanh, và các yêu cầu tuân thủ khác nhau vào các tài khoản AWS riêng biệt.  
  * Mô hình đa tài khoản mang lại nhiều lợi ích: cải thiện sự cô lập và khả năng chịu lỗi (một sự cố trong một tài khoản ít có khả năng ảnh hưởng đến các tài khoản khác); tăng cường ranh giới tuân thủ; và cho phép phân bổ chi phí cũng như quản trị tốt hơn.  
  * Tuy nhiên, nó cũng đặt ra một thách thức mới: làm thế nào để quản lý quyền truy cập một cách nhất quán và hiệu quả trên hàng chục, thậm chí hàng trăm tài khoản AWS? Việc phải sao chép và ánh xạ IAM Roles thủ công qua các tài khoản, cùng với việc quản trị thường dựa vào các phương pháp thủ công như bảng tính, đã tạo ra sự phức tạp và mở đường cho một giải pháp tập trung hơn.  
* **Giai đoạn 4: AWS IAM Identity Center (trước đây là AWS SSO)** 4  
  * Để giải quyết những thách thức về điều phối và quản lý truy cập trong môi trường đa tài khoản, AWS đã giới thiệu **AWS IAM Identity Center** (trước đây gọi là AWS Single Sign-On hay AWS SSO). Dịch vụ này cung cấp một trung tâm điều phối danh tính và truy cập tập trung, tích hợp chặt chẽ với AWS Organizations.  
  * Các cải tiến chính của IAM Identity Center bao gồm: **Permission Sets** (là các mẫu quyền được định nghĩa trước, về cơ bản là các IAM Roles được quản lý tập trung và triển khai vào các tài khoản thành viên); khả năng **gán quyền dựa trên nhóm** (group-based assignments) từ các IdP bên ngoài; **khả năng hiển thị liên tài khoản** (cross-account visibility) cho phép quản trị viên quản lý truy cập từ một nơi duy nhất; và việc sử dụng **thông tin xác thực ngắn hạn** cho tất cả các truy cập.  
* **Các cập nhật quan trọng khác trong lịch sử IAM** 5**:**  
  * **Session Tags** (tháng 11 năm 2019): Cho phép truyền các thuộc tính (tags) vào một session tạm thời khi một vai trò được đảm nhận, hỗ trợ cho Attribute-Based Access Control (ABAC).  
  * **Kiểm soát truy cập cho nhóm tài khoản trong AWS Organizations** (tháng 11 năm 2019): Cho phép tham chiếu đến các Organizational Units (OUs) trong các điều kiện của IAM policy (sử dụng condition key aws:PrincipalOrgPaths), tăng cường khả năng quản lý tập trung.  
  * **Permissions Boundaries** (tháng 7 năm 2018): Một tính năng cho phép ủy quyền việc tạo IAM roles và policies một cách an toàn bằng cách đặt ra giới hạn tối đa cho các quyền có thể được cấp.  
  * **Service-Linked Roles (SLRs)** (tháng 4 năm 2017): Một loại IAM Role đặc biệt được liên kết trực tiếp với một dịch vụ AWS, đơn giản hóa việc cấp quyền cho các dịch vụ AWS để hoạt động thay mặt người dùng.  
  * **Attribute-Based Access Control (ABAC)** (thông tin được bổ sung vào tháng 10 năm 2019): Hướng dẫn và hỗ trợ chính thức cho việc sử dụng tags để kiểm soát truy cập dựa trên thuộc tính.  
  * **IAM Access Analyzer** (liên tục được cập nhật): Bổ sung các tính năng như phân tích quyền truy cập không sử dụng (unused access analyzers) và kiểm tra chính sách tùy chỉnh (custom policy checks).  
  * **Hỗ trợ nhiều thiết bị MFA** (tháng 11 năm 2022): Cho phép người dùng root và IAM users đăng ký nhiều thiết bị MFA, tăng tính linh hoạt và khả năng phục hồi.

Quá trình phát triển của IAM không chỉ đơn thuần là việc bổ sung các tính năng mới. Nó phản ánh một sự thay đổi căn bản trong triết lý quản lý truy cập trên đám mây. Từ những ngày đầu với các access keys dài hạn, vốn tiềm ẩn nhiều rủi ro bảo mật và khó quản lý ở quy mô lớn 4, IAM đã chuyển dịch mạnh mẽ sang việc ưu tiên sử dụng thông tin xác thực tạm thời thông qua IAM Roles và STS.4 Sự ra đời của IAM Roles là một bước ngoặt, giải quyết vấn đề cố hữu của access keys dài hạn.  
Khi các tổ chức ngày càng áp dụng mô hình đa tài khoản để tăng cường bảo mật và quản trị, nhu cầu về một giải pháp quản lý truy cập tập trung trở nên cấp thiết. AWS IAM Identity Center ra đời chính là để đáp ứng nhu cầu này, cung cấp một giao diện thống nhất để quản lý quyền truy cập cho người dùng trên toàn bộ tổ chức AWS.4  
Song song đó, sự phát triển của các tính năng như Permissions Boundaries 7, Service Control Policies (SCPs) 8, và đặc biệt là Attribute-Based Access Control (ABAC) 9 cho thấy một xu hướng tinh vi hóa trong việc kiểm soát quyền. Các tính năng này cho phép các tổ chức ủy quyền quản lý một cách an toàn hơn, áp dụng các chính sách bảo mật vĩ mô không thể phá vỡ, và triển khai các mô hình kiểm soát truy cập động và linh hoạt hơn dựa trên thuộc tính.  
Việc AWS liên tục đầu tư và cải tiến các công cụ hỗ trợ như IAM Access Analyzer 5 và CloudTrail 10 cũng nhấn mạnh tầm quan trọng của việc giám sát, kiểm toán và chủ động cải thiện tình hình bảo mật trong một hệ thống IAM ngày càng năng động.  
Tất cả những thay đổi này không chỉ là sự thích ứng của AWS với nhu cầu ngày càng phức tạp của khách hàng mà còn phản ánh sự hội tụ hướng tới các nguyên tắc bảo mật hiện đại như Zero Trust – một mô hình yêu cầu "ít tin cậy hơn, xác minh nhiều hơn" và luôn áp dụng đặc quyền tối thiểu. Lịch sử phát triển của IAM là minh chứng cho cam kết không ngừng của AWS trong việc cung cấp một nền tảng quản lý danh tính và truy cập mạnh mẽ, an toàn và có khả năng mở rộng.

## **Phần II: Các Thành phần Chính của IAM (IAM Building Blocks)**

Để xây dựng một hệ thống quản lý truy cập hiệu quả và an toàn trên AWS, việc hiểu rõ các thành phần cấu thành nên IAM là điều kiện tiên quyết. Các thành phần này bao gồm IAM Users, IAM Groups, IAM Roles và IAM Policies, mỗi thành phần đóng một vai trò riêng biệt nhưng lại phối hợp chặt chẽ với nhau.

### **A. IAM Users**

IAM Users đại diện cho các cá nhân (con người) hoặc các ứng dụng (workloads) cần tương tác với các tài nguyên và dịch vụ trong tài khoản AWS của bạn.1 Mỗi IAM User là một thực thể riêng biệt với một bộ thông tin xác thực (credentials) và các quyền (permissions) được gán cụ thể.

**Thông tin xác thực (Credentials) của IAM User:**

IAM Users sử dụng nhiều loại thông tin xác thực khác nhau để chứng minh danh tính của mình khi truy cập AWS:

1. **Mật khẩu (Passwords):** Được sử dụng chủ yếu cho việc đăng nhập vào AWS Management Console, giao diện quản lý dựa trên web của AWS.11 Các tổ chức nên thiết lập chính sách mật khẩu mạnh và có quy trình định kỳ để người dùng thay đổi mật khẩu nhằm tăng cường bảo mật.12  
2. **Khóa truy cập (Access Keys):** Bao gồm một **Access Key ID** và một **Secret Access Key**. Đây là các thông tin xác thực dài hạn (long-term credentials) được sử dụng cho việc truy cập AWS theo chương trình (programmatic access) thông qua AWS Command Line Interface (CLI), AWS Software Development Kits (SDKs), hoặc các lệnh gọi API trực tiếp.1 Do tính chất dài hạn, access keys cần được bảo vệ cẩn thận, lưu trữ an toàn và xoay vòng (rotate) định kỳ theo các khuyến nghị bảo mật.2 AWS khuyến nghị mạnh mẽ việc hạn chế sử dụng access keys dài hạn và ưu tiên các thông tin xác thực tạm thời bất cứ khi nào có thể.12  
3. **Chứng chỉ ký (Signing Certificates \- X.509):** Đây là một loại thông tin xác thực ít phổ biến hơn, được sử dụng bởi một số dịch vụ AWS cụ thể yêu cầu xác thực dựa trên chứng chỉ X.509 cho các yêu cầu được ký bằng khóa riêng tư tương ứng.11 Ví dụ, chúng có thể được sử dụng để xác thực các yêu cầu SOAP (Simple Object Access Protocol) cũ hoặc cho một số API nhất định của các dịch vụ như Amazon EC2 (cho các AMI cũ) hoặc một số API của bên thứ ba. Chúng không phải là cơ chế xác thực người dùng chính cho hầu hết các tương tác với AWS hiện nay.  
4. **Multi-Factor Authentication (MFA \- Xác thực Đa yếu tố):** Mặc dù không phải là một loại credential theo nghĩa truyền thống, MFA là một lớp bảo mật bổ sung cực kỳ quan trọng. Khi được kích hoạt, người dùng ngoài việc cung cấp mật khẩu hoặc access key (yếu tố "something you know"), còn phải cung cấp một yếu tố xác thực thứ hai từ một thiết bị MFA (yếu tố "something you have" hoặc "something you are").11 Việc kích hoạt MFA được khuyến nghị cho tất cả IAM Users, đặc biệt là những người dùng có quyền hạn cao.

**Trường hợp sử dụng (Use Cases) của IAM User:**

Mặc dù AWS khuyến nghị sử dụng IAM Roles và các phương thức truy cập liên kết bất cứ khi nào có thể, vẫn có một số trường hợp cụ thể mà việc sử dụng IAM Users với thông tin xác thực dài hạn là cần thiết hoặc phù hợp hơn 2:

* **Truy cập khẩn cấp (Emergency Access):** Trong các tình huống "break-glass" khi các hệ thống quản lý danh tính trung tâm (như IdP hoặc IAM Identity Center) có thể không khả dụng, việc có một số ít IAM Users với quyền quản trị và MFA mạnh có thể cần thiết để truy cập khẩn cấp vào tài khoản AWS.11  
* **Các Workloads không thể sử dụng IAM Roles:** Một số dịch vụ AWS hoặc ứng dụng của bên thứ ba có thể chưa hỗ trợ đầy đủ việc sử dụng IAM Roles cho tất cả các tương tác. Ví dụ:  
  * Truy cập vào **AWS CodeCommit** repositories qua HTTPS (sử dụng Git credentials được tạo từ IAM user) hoặc SSH (sử dụng SSH keys được liên kết với IAM user).11  
  * Truy cập vào **Amazon Keyspaces (cho Apache Cassandra)** bằng một số công cụ client nhất định.11  
  * Một số **ứng dụng client của bên thứ ba cũ hơn** có thể được thiết kế để chỉ hoạt động với access keys dài hạn của IAM User.  
* **Khi IAM Identity Center không khả dụng và không có IdP nào khác:** Trong trường hợp một tổ chức chưa triển khai IAM Identity Center hoặc không có một Identity Provider (IdP) bên ngoài để thực hiện liên kết danh tính, IAM Users có thể là phương tiện ban đầu để cấp quyền truy cập cho quản trị viên hoặc người dùng.11

Tuy nhiên, điều quan trọng cần nhấn mạnh là việc sử dụng IAM Users với thông tin xác thực dài hạn nên được **hạn chế ở mức tối đa** và chỉ dành cho các trường hợp thực sự cần thiết.

**Giới hạn (Limitations) của IAM User:**

So với các giải pháp quản lý danh tính hiện đại hơn như IAM Roles và IAM Identity Center, IAM Users có một số hạn chế đáng kể 11:

* **Khó mở rộng (Scalability):** Việc quản lý một số lượng lớn IAM Users, mỗi người với bộ thông tin xác thực và quyền hạn riêng, trở nên rất phức tạp và tốn thời gian khi tổ chức phát triển.  
* **Thiếu khả năng hiển thị và kiểm toán tập trung (Centralized Visibility and Auditing):** Việc theo dõi và kiểm toán quyền truy cập của từng IAM User riêng lẻ khó khăn hơn so với việc quản lý thông qua các vai trò hoặc các giải pháp tập trung.  
* **Phức tạp hơn trong việc triển khai các phương pháp bảo mật tốt nhất:** Việc đảm bảo tất cả IAM Users tuân thủ các chính sách mật khẩu mạnh, kích hoạt MFA, và xoay vòng access keys định kỳ đòi hỏi nỗ lực quản lý lớn hơn.

**So sánh với IAM Roles:**

IAM Roles thường được ưu tiên hơn IAM Users trong hầu hết các kịch bản vì những lý do sau 3:

* **Bảo mật cao hơn:** Roles sử dụng thông tin xác thực tạm thời, có thời hạn sử dụng ngắn, giúp giảm thiểu rủi ro nếu thông tin xác thực bị lộ.  
* **Quản lý dễ dàng hơn:** Việc quản lý quyền thông qua Roles (đặc biệt khi kết hợp với Groups hoặc các giải pháp liên kết) thường đơn giản và nhất quán hơn.  
* **Linh hoạt hơn:** Roles có thể được đảm nhận bởi nhiều loại thực thể khác nhau (người dùng, ứng dụng, dịch vụ AWS), trong khi Users thường gắn với một cá nhân hoặc ứng dụng cụ thể.

Sự nhấn mạnh liên tục của AWS vào việc hạn chế sử dụng IAM Users và ưu tiên các giải pháp như IAM Roles, Identity Federation, và IAM Identity Center 2 cho thấy một sự thay đổi mô hình cơ bản trong cách tiếp cận quản lý danh tính trên nền tảng này. IAM Users, với thông tin xác thực dài hạn vốn có nhiều rủi ro nếu không được quản lý chặt chẽ 2, đang dần trở thành một giải pháp "di sản" (legacy) hoặc chỉ dành cho các "trường hợp đặc biệt" thay vì là phương pháp chính để cấp quyền truy cập cho con người hoặc ứng dụng. Sự ra đời và phát triển mạnh mẽ của IAM Roles, với cơ chế cung cấp thông tin xác thực tạm thời 3, cùng với các giải pháp liên kết danh tính và IAM Identity Center cũng dựa trên nền tảng này 4, đã cung cấp những lựa chọn thay thế an toàn hơn, linh hoạt hơn và dễ quản lý hơn ở quy mô lớn. Các khuyến nghị bảo mật tốt nhất từ AWS cũng liên tục hướng người dùng đến việc sử dụng thông tin xác thực tạm thời.12 Các trường hợp sử dụng được chấp nhận cho IAM Users ngày càng bị thu hẹp, chủ yếu giới hạn trong các kịch bản như truy cập khẩn cấp hoặc một số workloads rất cụ thể không có lựa chọn khác.11 Điều này phản ánh một nỗ lực không ngừng của AWS nhằm nâng cao tiêu chuẩn bảo mật và đơn giản hóa việc quản lý truy cập cho khách hàng.

### **B. IAM Groups**

IAM Groups là một cơ chế trong AWS IAM cho phép bạn tổ chức và quản lý quyền cho một tập hợp các IAM Users.1 Thay vì phải gán các chính sách quyền (IAM Policies) cho từng IAM User một cách riêng lẻ, bạn có thể tạo một Group, gán các chính sách cần thiết cho Group đó, và sau đó thêm các IAM Users vào Group. Tất cả các Users trong Group sẽ tự động thừa hưởng các quyền đã được định nghĩa cho Group.

**Mục đích và Quản lý quyền cho Users:**

Mục đích chính của IAM Groups là **đơn giản hóa việc quản lý quyền** cho nhiều người dùng có cùng nhu cầu truy cập hoặc cùng vai trò chức năng trong tổ chức.1 Ví dụ, bạn có thể tạo một Group tên là "Developers" và gán cho Group này các quyền cần thiết để phát triển và thử nghiệm ứng dụng, như quyền truy cập vào Amazon EC2, Amazon S3 (cho mã nguồn và tạo phẩm), và Amazon DynamoDB. Sau đó, khi có một nhà phát triển mới tham gia nhóm, bạn chỉ cần thêm IAM User của họ vào Group "Developers", và họ sẽ tự động có tất cả các quyền đã được cấu hình cho Group. Tương tự, nếu một nhà phát triển rời đi, bạn chỉ cần xóa User của họ khỏi Group (hoặc xóa User hoàn toàn).

**Các phương pháp tốt nhất (Best Practices) khi sử dụng IAM Groups:**

* **Sử dụng Groups để quản lý quyền cho IAM Users có cùng nhu cầu truy cập:** Đây là cách hiệu quả để áp dụng nguyên tắc đặc quyền tối thiểu ở cấp độ nhóm. Hãy tạo các Groups dựa trên các vai trò công việc hoặc các yêu cầu truy cập chức năng (ví dụ: "Administrators", "ReadOnlyUsers", "DatabaseOperators").1  
* **Groups không thể chứa Groups khác (No Group Nesting):** Một IAM Group không thể là thành viên của một IAM Group khác. Cấu trúc Group trong IAM là phẳng.  
* **Một User có thể thuộc nhiều Groups:** Một IAM User có thể là thành viên của nhiều Group khác nhau. Trong trường hợp này, User đó sẽ có tổng hợp các quyền từ tất cả các Group mà họ thuộc về (cộng với bất kỳ quyền nào được gán trực tiếp cho User đó thông qua inline policies).  
* **Groups không phải là một Identity thực sự:** IAM Groups không phải là một "principal" theo nghĩa là chúng không thể tự mình đăng nhập vào AWS hoặc đảm nhận một IAM Role. Chúng không có thông tin xác thực riêng. Mục đích duy nhất của Groups là một cơ chế để gắn các chính sách (policies) cho một tập hợp các IAM Users.

Mặc dù xu hướng chung trong quản lý danh tính AWS là giảm thiểu việc sử dụng IAM Users và ưu tiên các giải pháp như IAM Roles, Identity Federation, và IAM Identity Center, IAM Groups vẫn giữ một vai trò quan trọng và hữu ích. Trong các kịch bản mà IAM Users vẫn cần được sử dụng (ví dụ, cho các quản trị viên tài khoản, các người dùng dịch vụ cụ thể không thể sử dụng roles, hoặc trong các môi trường nhỏ hơn chưa áp dụng federation), Groups là công cụ không thể thiếu.  
Việc quản lý quyền cho từng IAM User riêng lẻ không chỉ tốn thời gian mà còn dễ dẫn đến lỗi cấu hình và sự thiếu nhất quán trong việc áp dụng các chính sách bảo mật.1 IAM Groups giải quyết vấn đề này bằng cách cho phép áp dụng các chính sách một cách đồng bộ cho một nhóm người dùng có cùng trách nhiệm hoặc nhu cầu truy cập.1 Ngay cả khi số lượng IAM Users trong một tổ chức giảm đi do sự phổ biến của Roles và Federation, những Users còn lại vẫn cần được quản lý quyền một cách hiệu quả và có tổ chức.  
Ví dụ, một nhóm "RootAccountAdmins" có thể được tạo ra để quản lý các IAM Users có quyền quản trị cao nhất (sau root user), đảm bảo rằng tất cả họ đều có cùng một bộ chính sách bảo mật nghiêm ngặt. Hoặc một nhóm "BillingUsers" có thể được cấp quyền chỉ để xem thông tin thanh toán. Do đó, IAM Groups vẫn là một công cụ có giá trị để tổ chức hóa việc áp dụng các chính sách, duy trì sự nhất quán, và thực thi nguyên tắc đặc quyền tối thiểu cho các IAM Users hiện có, giúp việc kiểm soát và kiểm toán trở nên dễ dàng hơn.

### **C. IAM Roles**

IAM Roles là một trong những khái niệm trung tâm và mạnh mẽ nhất trong AWS IAM. Chúng là các định danh IAM với các quyền cụ thể, nhưng không giống như IAM Users, Roles không gắn liền với một người dùng hay ứng dụng cụ thể. Thay vào đó, Roles được thiết kế để có thể được *đảm nhận* (assumed) bởi các thực thể đáng tin cậy (trusted entities) cần quyền truy cập tạm thời vào tài nguyên AWS.1 Một đặc điểm quan trọng của Roles là chúng **không có thông tin xác thực dài hạn** (như mật khẩu hay access keys cố định). Khi một thực thể đảm nhận một Role, AWS Security Token Service (STS) sẽ cấp cho thực thể đó một bộ **thông tin xác thực bảo mật tạm thời** (temporary security credentials) có thời hạn sử dụng ngắn.3

Việc ưu tiên sử dụng IAM Roles thay vì IAM Users với thông tin xác thực dài hạn là một khuyến nghị bảo mật hàng đầu của AWS. Lý do chính là vì thông tin xác thực tạm thời giúp giảm thiểu đáng kể rủi ro liên quan đến việc lộ thông tin xác thực. Nếu thông tin xác thực tạm thời bị xâm phạm, chúng chỉ có hiệu lực trong một khoảng thời gian ngắn, hạn chế thiệt hại tiềm ẩn. Ngoài ra, Roles còn mang lại khả năng quản lý quyền tập trung và tính linh hoạt cao hơn trong nhiều kịch bản truy cập khác nhau.3

**Cơ chế Đảm nhận Vai trò (Role Assumption Mechanism):**

Quá trình một thực thể đáng tin cậy nhận được quyền hạn của một IAM Role được gọi là "đảm nhận vai trò". Cơ chế này hoạt động như sau:

1. **Trust Policies (Chính sách Tin cậy):** Mỗi IAM Role phải có một **Trust Policy** đính kèm. Đây là một tài liệu JSON đặc biệt, định nghĩa rõ ràng những *principals* (ví dụ: tài khoản AWS khác, IAM Users, IAM Roles khác, hoặc các dịch vụ AWS) được *tin cậy* và được phép đảm nhận Role đó.3 Trust Policy về cơ bản là một loại resource-based policy dành riêng cho IAM Role, kiểm soát ai có thể "trở thành" Role đó.  
2. **AssumeRole API (và các biến thể):** Thực thể muốn đảm nhận Role sẽ thực hiện một lệnh gọi API đến AWS Security Token Service (STS). Lệnh gọi API phổ biến nhất cho mục đích này là AssumeRole. Ngoài ra, còn có các biến thể như AssumeRoleWithSAML (cho liên kết danh tính SAML 2.0) và AssumeRoleWithWebIdentity (cho liên kết danh tính OpenID Connect).16 Trong lệnh gọi này, thực thể cần cung cấp ARN (Amazon Resource Name) của Role mà họ muốn đảm nhận và một số thông tin khác tùy theo API (ví dụ: SAML assertion, OIDC token).  
3. **Cấp thông tin xác thực tạm thời:** Nếu Trust Policy của Role cho phép principal thực hiện yêu cầu, và principal đó cũng có quyền thực hiện hành động sts:AssumeRole (nếu cần thiết, ví dụ như một IAM User đảm nhận Role), STS sẽ xác thực yêu cầu và cấp một bộ thông tin xác thực tạm thời. Bộ thông tin này bao gồm một Access Key ID, một Secret Access Key, và một Session Token. Thực thể sau đó có thể sử dụng bộ thông tin xác thực tạm thời này để thực hiện các lệnh gọi API đến các dịch vụ AWS, với các quyền hạn được định nghĩa trong **Permissions Policies** của Role đó.

**Các Trường hợp sử dụng Phổ biến của IAM Roles:**

IAM Roles được sử dụng rộng rãi trong nhiều kịch bản khác nhau để quản lý truy cập một cách an toàn và linh hoạt:

* **Amazon EC2 Instances:** Thay vì lưu trữ access keys dài hạn trực tiếp trên một EC2 instance (một thực hành bảo mật kém), bạn có thể gắn một IAM Role cho instance đó. Các ứng dụng chạy trên instance này sau đó có thể tự động nhận được thông tin xác thực tạm thời từ metadata service của EC2 để truy cập các dịch vụ AWS khác (ví dụ: đọc/ghi dữ liệu vào S3, tương tác với DynamoDB) mà không cần quản lý access keys trong mã nguồn.1  
* **AWS Lambda Functions:** Tương tự như EC2 instances, mỗi hàm Lambda cũng được liên kết với một IAM Role (gọi là execution role). Khi hàm được thực thi, nó sẽ đảm nhận Role này để có được các quyền cần thiết để tương tác với các dịch vụ AWS khác, ví dụ như ghi log vào CloudWatch Logs, đọc dữ liệu từ S3, hoặc gửi tin nhắn đến SQS.1  
* **Truy cập Liên tài khoản (Cross-Account Access):** Đây là một trong những trường hợp sử dụng quan trọng nhất của IAM Roles. Chúng cho phép các principals (người dùng hoặc vai trò) trong một tài khoản AWS ("tài khoản gốc" \- originating account) đảm nhận một Role được tạo trong một tài khoản AWS khác ("tài khoản đích" \- destination account) để truy cập vào các tài nguyên trong tài khoản đích đó. Đây là phương pháp được AWS khuyến nghị để chia sẻ tài nguyên và ủy quyền truy cập giữa các tài khoản một cách an toàn.1  
* **Identity Federation (Liên kết Danh tính):** IAM Roles đóng vai trò trung tâm trong việc cho phép người dùng từ các hệ thống quản lý danh tính bên ngoài (ví dụ: Microsoft Active Directory thông qua SAML 2.0, hoặc các nhà cung cấp danh tính web như Google, Facebook thông qua OpenID Connect) truy cập vào tài nguyên AWS. Sau khi được xác thực bởi IdP bên ngoài, người dùng sẽ được ánh xạ để đảm nhận một IAM Role cụ thể trong AWS, Role này sẽ cấp cho họ các quyền truy cập tạm thời.3  
* **AWS Services:** Nhiều dịch vụ AWS cần quyền để thực hiện các hành động thay mặt bạn trên các tài nguyên của bạn. Ví dụ, AWS CloudFormation cần quyền để tạo các tài nguyên được định nghĩa trong template, hoặc AWS Auto Scaling cần quyền để khởi chạy hoặc chấm dứt EC2 instances. Các dịch vụ này thường sử dụng IAM Roles (Service Roles hoặc Service-Linked Roles) để có được các quyền cần thiết.1

**Service Roles và Service-Linked Roles (SLRs):**

Trong ngữ cảnh các dịch vụ AWS cần quyền để hoạt động, có hai loại Roles chính:

* **Service Roles:** Đây là các IAM Roles mà bạn tạo ra và cấu hình để một dịch vụ AWS cụ thể có thể đảm nhận và thực hiện các hành động thay mặt bạn.24 Bạn có toàn quyền kiểm soát việc định nghĩa Trust Policy (chỉ định dịch vụ nào được phép đảm nhận Role) và Permissions Policies (chỉ định những gì dịch vụ đó có thể làm). Ví dụ, bạn có thể tạo một Service Role cho Amazon EC2 để cho phép dịch vụ EC2 truy cập vào S3.  
* **Service-Linked Roles (SLRs):** Đây là một loại Service Role đặc biệt, được liên kết trực tiếp và duy nhất với một dịch vụ AWS cụ thể.25  
  * **Mục đích:** SLRs được thiết kế để đơn giản hóa việc cấp quyền cho các dịch vụ AWS bằng cách cung cấp một bộ quyền đã được định nghĩa trước, tối ưu cho hoạt động của dịch vụ đó. Điều này giúp loại bỏ nhu cầu người dùng phải tự mình nghiên cứu và cấu hình các quyền phức tạp.  
  * **Cách tạo:** SLRs thường được tạo tự động bởi dịch vụ AWS liên quan khi bạn lần đầu tiên sử dụng một tính năng yêu cầu vai trò đó. Trong một số trường hợp, bạn có thể cần tạo SLR thông qua IAM console, AWS CLI (sử dụng lệnh iam create-service-linked-role với tham số AWSServiceName 26), hoặc API.  
  * **Ràng buộc quản lý:** Một điểm khác biệt quan trọng của SLRs là dịch vụ AWS liên kết sẽ kiểm soát các chính sách (cả trust policy và permissions policy) được đính kèm với SLR và quyết định khi nào SLR có thể bị xóa.26 Bạn không thể tự ý thay đổi permissions policy của một SLR, và trust policy cũng được dịch vụ định nghĩa sẵn.25 Để xóa một SLR, bạn thường phải xóa tất cả các tài nguyên AWS liên quan mà SLR đó đang phục vụ trước.25  
  * **Naming convention:** SLRs thường có một quy ước đặt tên chuẩn, ví dụ: AWSServiceRoleFor\<ServiceName\> (như AWSServiceRoleForRDS được sử dụng bởi Amazon DocumentDB 25).

Sự đa dạng trong các trường hợp sử dụng của IAM Roles – từ việc cấp quyền cho các ứng dụng chạy trên EC2 và Lambda, cho phép truy cập liên tài khoản an toàn, làm nền tảng cho liên kết danh tính, đến việc các dịch vụ AWS sử dụng chúng để hoạt động – cho thấy rằng Roles không chỉ đơn thuần là một "tính năng" của IAM. Thay vào đó, chúng đã trở thành một khái niệm trung tâm và không thể thiếu trong kiến trúc IAM hiện đại. Chúng đóng vai trò như những "cầu nối" linh hoạt và an toàn, cho phép các loại thực thể khác nhau (con người, ứng dụng, dịch vụ AWS) tương tác với tài nguyên AWS một cách có kiểm soát, thông qua cơ chế thông tin xác thực tạm thời và các quyền hạn được định nghĩa chặt chẽ.  
Hầu hết mọi kịch bản truy cập nâng cao trong AWS, ngoại trừ việc sử dụng IAM User trực tiếp với thông tin xác thực dài hạn, đều xoay quanh việc sử dụng IAM Roles. Chúng là cơ chế cốt lõi để triển khai nguyên tắc đặc quyền tối thiểu một cách hiệu quả trong vô số ngữ cảnh khác nhau. Ngay cả sự phát triển của các dịch vụ quản lý truy cập tập trung như AWS IAM Identity Center, với khái niệm "Permission Sets", cũng dựa trên nền tảng của IAM Roles; về bản chất, mỗi Permission Set khi được áp dụng cho một tài khoản sẽ tạo ra hoặc sử dụng một IAM Role tương ứng trong tài khoản đó. Điều này càng củng cố vai trò trung tâm và tầm quan trọng của IAM Roles trong việc định hình cách chúng ta quản lý truy cập trên AWS ngày nay.

### **D. IAM Policies**

IAM Policies là các tài liệu định nghĩa các quyền (permissions) trong AWS. Chúng được viết dưới dạng JSON (JavaScript Object Notation) và là cơ chế cốt lõi để xác định những hành động (actions) nào được phép (Allow) hoặc bị từ chối rõ ràng (Deny) đối với các tài nguyên (resources) cụ thể, và trong những điều kiện (conditions) nào.1 Chính sách là nền tảng của việc ủy quyền trong IAM.

**Ngôn ngữ và Cấu trúc JSON của IAM Policies** 28**:**

Mỗi IAM Policy bao gồm một hoặc nhiều *statements*. Mỗi statement là một khối định nghĩa quyền, chứa các thành phần chính sau:

* **Version**: (Tùy chọn, nhưng khuyến nghị và bắt buộc nếu sử dụng biến policy) Chỉ định phiên bản ngôn ngữ của policy. Giá trị phổ biến và hiện tại là "2012-10-17".29  
* **Statement**: (Bắt buộc) Là thành phần chính của policy, chứa một mảng (array) gồm một hoặc nhiều câu lệnh (statement) riêng lẻ. Mỗi statement trong mảng này mô tả một tập hợp các quyền cụ thể.29  
* **Sid (Statement ID)**: (Tùy chọn) Là một mã định danh (chuỗi) mà bạn có thể gán cho mỗi statement. Sid rất hữu ích cho việc tổ chức, quản lý và gỡ lỗi các policy phức tạp, giúp bạn dễ dàng tham chiếu đến một statement cụ thể.29  
* **Effect**: (Bắt buộc trong mỗi statement) Xác định kết quả của statement khi nó được đánh giá. Giá trị hợp lệ là "Allow" (cho phép hành động) hoặc "Deny" (từ chối rõ ràng hành động).29 Một Deny rõ ràng luôn ghi đè một Allow.  
* **Principal**: (Bắt buộc trong resource-based policies như S3 bucket policies hoặc IAM role trust policies; không được sử dụng trong identity-based policies) Chỉ định tài khoản, người dùng, vai trò hoặc dịch vụ AWS (gọi chung là principal) mà statement này áp dụng cho (tức là ai được cấp hoặc bị từ chối quyền).3 Trong identity-based policies (gắn với user, group, role), principal được ngầm hiểu là chính identity đó.  
* **Action**: (Bắt buộc trong mỗi statement) Liệt kê một hoặc nhiều hành động của dịch vụ AWS mà statement này cho phép hoặc từ chối. Mỗi hành động được biểu diễn dưới dạng service-prefix:ActionName (ví dụ: "s3:GetObject", "ec2:RunInstances", "iam:CreateUser").28 Bạn có thể sử dụng ký tự đại diện (wildcard \*) để chỉ định nhiều hành động (ví dụ: "s3:\*" cho tất cả các hành động S3, hoặc "ec2:Describe\*" cho tất cả các hành động EC2 bắt đầu bằng "Describe").  
* **Resource**: (Bắt buộc trong mỗi statement, trừ một số trường hợp đặc biệt như một số action của IAM không áp dụng cho resource cụ thể) Chỉ định một hoặc nhiều tài nguyên AWS mà statement này áp dụng. Tài nguyên thường được xác định bằng Amazon Resource Name (ARN) của chúng (ví dụ: arn:aws:s3:::mybucket/\* cho tất cả các đối tượng trong bucket mybucket).28 Ký tự đại diện \* cũng có thể được sử dụng trong ARN.  
* **Condition**: (Tùy chọn) Cho phép bạn chỉ định các điều kiện phải được đáp ứng để statement có hiệu lực. Conditions được biểu diễn dưới dạng các cặp key-value, sử dụng các *condition operators* (ví dụ: StringEquals, IpAddress) để so sánh các *condition keys* (các biến chứa thông tin về request, principal, resource, hoặc môi trường) với các giá trị bạn cung cấp.28 Ví dụ, một condition có thể giới hạn quyền truy cập chỉ từ một dải địa chỉ IP cụ thể, hoặc chỉ trong một khoảng thời gian nhất định, hoặc chỉ khi một tag cụ thể tồn tại trên tài nguyên.

**Bảng 1: Phân tích chi tiết các thành phần của một IAM Policy Statement**

| Tên thành phần | Mô tả chi tiết | Bắt buộc/Tùy chọn | Ví dụ sử dụng |
| :---- | :---- | :---- | :---- |
| Version | Chỉ định phiên bản ngôn ngữ policy. Hiện tại, giá trị "2012-10-17" là phiên bản mới nhất và được khuyến nghị. Bắt buộc nếu sử dụng biến policy.29 | Tùy chọn | "Version": "2012-10-17" |
| Statement | Phần tử chính chứa một mảng các câu lệnh (statements) riêng lẻ. Mỗi statement định nghĩa một tập hợp quyền.29 | Bắt buộc | "Statement": \[{...}, {...}\] |
| Sid | (Statement ID) Mã định danh tùy chọn cho statement, hữu ích để phân biệt các statement trong một policy.29 | Tùy chọn | "Sid": "AllowS3ReadAccess" |
| Effect | Xác định statement cho phép ("Allow") hay từ chối rõ ràng ("Deny") quyền truy cập.29 | Bắt buộc | "Effect": "Allow" hoặc "Effect": "Deny" |
| Principal | (Chỉ trong resource-based policies hoặc trust policies) Chỉ định tài khoản, user, role, hoặc service được cấp/từ chối quyền.28 | Bắt buộc (trong RBPs) | "Principal": {"AWS": "arn:aws:iam::123456789012:root"} hoặc "Principal": {"Service": "ec2.amazonaws.com"} |
| Action | Danh sách các hành động của dịch vụ AWS được cho phép hoặc bị từ chối. Có thể dùng wildcard (\*).28 | Bắt buộc | "Action": "s3:GetObject" hoặc "Action": hoặc "Action": "iam:\*" |
| Resource | Danh sách các tài nguyên AWS mà statement áp dụng, thường là ARN. Có thể dùng wildcard (\*).28 | Bắt buộc | "Resource": "arn:aws:s3:::mybucket/\*" hoặc "Resource": "\*" |
| Condition | (Tùy chọn) Các điều kiện phải được đáp ứng để statement có hiệu lực, sử dụng condition keys và operators.28 | Tùy chọn | "Condition": {"StringEquals": {"aws:SourceIp": "192.0.2.0/24"}} hoặc "Condition": {"DateLessThan": {"aws:CurrentTime": "2024-12-31T23:59:59Z"}} |

**Các loại Policy:**

IAM cung cấp nhiều loại policy khác nhau, mỗi loại phục vụ một mục đích cụ thể:

* **Identity-based Policies (Chính sách dựa trên danh tính)** 3**:** Đây là các chính sách được bạn đính kèm (attach) trực tiếp vào một IAM identity (user, group, hoặc role). Chúng định nghĩa những gì identity đó có thể làm. Có ba loại identity-based policies:  
  * **AWS Managed Policies (Chính sách do AWS quản lý):** Là các chính sách độc lập được tạo và quản lý bởi AWS. Chúng được thiết kế để cung cấp quyền cho nhiều trường hợp sử dụng phổ biến (ví dụ: AdministratorAccess, AmazonS3ReadOnlyAccess).  
    * *Ưu điểm:* Dễ sử dụng, được AWS tự động cập nhật khi có dịch vụ hoặc tính năng mới.  
    * *Nhược điểm:* Thường cấp quyền rộng hơn mức cần thiết cho một tác vụ cụ thể, có thể không tuân thủ chặt chẽ nguyên tắc đặc quyền tối thiểu.31  
  * **Customer Managed Policies (Chính sách do khách hàng quản lý):** Là các chính sách độc lập mà bạn tạo và quản lý trong tài khoản AWS của mình. Bạn có toàn quyền kiểm soát nội dung của các chính sách này.  
    * *Ưu điểm:* Có thể tái sử dụng bằng cách đính kèm vào nhiều principals. Quản lý thay đổi tập trung (thay đổi policy một lần, áp dụng cho tất cả principals đính kèm). Hỗ trợ versioning, cho phép bạn theo dõi các thay đổi và rollback về phiên bản trước nếu cần. Cho phép bạn triển khai nguyên tắc đặc quyền tối thiểu một cách chính xác bằng cách chỉ cấp những quyền thực sự cần thiết. Có giới hạn ký tự lớn hơn so với inline policies cho group.31  
    * *Nhược điểm:* Đòi hỏi nỗ lực quản lý từ phía bạn (tạo, cập nhật, duy trì). Việc tạo ra các chính sách tuân thủ đặc quyền tối thiểu đòi hỏi kiến thức và thời gian.  
  * **Inline Policies (Chính sách nội tuyến):** Là các chính sách được nhúng trực tiếp vào một IAM user, group, hoặc role duy nhất. Chúng là một phần không thể tách rời của identity đó.  
    * *Ưu điểm:* Đảm bảo một mối quan hệ 1-1 chặt chẽ giữa policy và identity; policy chỉ áp dụng cho identity đó. Tự động bị xóa khi identity liên quan bị xóa.  
    * *Nhược điểm:* Không thể tái sử dụng cho các identities khác. Việc quản lý thay đổi trở nên phi tập trung (phải sửa policy trên từng identity). Không hỗ trợ versioning. Giới hạn ký tự cho inline policies của group nhỏ hơn so với managed policies.31

**Bảng 2: So sánh các loại Policy dựa trên Danh tính (AWS Managed, Customer Managed, Inline)**

| Tiêu chí | AWS Managed Policy | Customer Managed Policy | Inline Policy |
| :---- | :---- | :---- | :---- |
| **Người tạo & Quản lý** | AWS | Khách hàng (Bạn) | Khách hàng (Bạn), nhúng vào identity |
| **Khả năng tái sử dụng** | Cao (có thể gắn vào nhiều principals) | Cao (có thể gắn vào nhiều principals) | Không (chỉ cho một user, group, hoặc role duy nhất) |
| **Versioning** | AWS quản lý các phiên bản | Có (lưu trữ tối đa 5 phiên bản, cho phép rollback) 31 | Không |
| **Giới hạn ký tự** | Lớn | Lớn (ví dụ: 6,144 ký tự) 7 | Nhỏ hơn (ví dụ: 2,048 cho group, 5,120 cho user, 10,240 cho role) |
| **Ưu điểm** | Dễ sử dụng, tự động cập nhật, không cần quản lý 31 | Kiểm soát chi tiết, tái sử dụng, versioning, hỗ trợ đặc quyền tối thiểu tốt nhất 31 | Mối quan hệ 1-1 chặt chẽ, tự động xóa cùng identity 31 |
| **Nhược điểm** | Có thể quá rộng, ít kiểm soát chi tiết 31 | Cần nỗ lực quản lý, đòi hỏi kiến thức để tạo policy tốt 31 | Không tái sử dụng, khó quản lý thay đổi ở quy mô lớn, không versioning 31 |
| **Sử dụng tốt nhất** | Bắt đầu nhanh, các vai trò công việc chung 31 | Hầu hết các trường hợp, đặc biệt khi cần đặc quyền tối thiểu và tái sử dụng 31 | Khi quyền rất cụ thể cho một identity duy nhất và không cần tái sử dụng 31 |

* **Resource-based Policies (Chính sách dựa trên tài nguyên)** 3**:** Đây là các chính sách được đính kèm trực tiếp vào một tài nguyên AWS, thay vì một identity. Chúng xác định những principals nào (có thể là từ cùng tài khoản hoặc tài khoản khác) được phép hoặc bị từ chối thực hiện hành động trên tài nguyên đó. Một số ví dụ phổ biến:  
  * **S3 Bucket Policies:** Kiểm soát quyền truy cập vào các S3 buckets và các đối tượng (objects) bên trong chúng. Chúng tương tác với identity-based policies của người dùng/vai trò thực hiện yêu cầu. Quyền truy cập thường được cấp nếu một trong hai loại chính sách (bucket policy hoặc identity policy) cho phép hành động đó, và không có một Deny rõ ràng nào trong bất kỳ chính sách nào áp dụng. Một Deny rõ ràng sẽ luôn ghi đè một Allow.34  
  * **KMS Key Policies:** Là phương thức chính và bắt buộc để kiểm soát quyền truy cập vào các khóa mã hóa AWS Key Management Service (KMS). Mỗi KMS key phải có chính xác một key policy. IAM policies (identity-based) chỉ có thể cấp quyền truy cập vào một KMS key nếu key policy của khóa đó cho phép điều đó (thông qua việc cho phép tài khoản root hoặc các IAM principals cụ thể).35  
  * **Lambda Function Policies:** Cho phép bạn cấp quyền cho các tài khoản AWS khác, các dịch vụ AWS (như S3, API Gateway khi chúng cần gọi hàm Lambda), hoặc các AWS Organizations để gọi (invoke) hoặc quản lý các hàm Lambda của bạn.32  
  * **IAM Role Trust Policies:** Như đã đề cập, đây là một loại resource-based policy đặc biệt được gắn vào IAM Role, xác định những principals nào được tin cậy để đảm nhận Role đó.  
  * **Tương tác với Identity-based Policies:** Nói chung, khi một principal trong cùng tài khoản yêu cầu truy cập một tài nguyên, quyền truy cập được cấp nếu *hoặc* identity-based policy của principal đó *hoặc* resource-based policy của tài nguyên đó cho phép hành động, và không có một Deny rõ ràng nào từ bất kỳ chính sách nào.18 Đối với truy cập liên tài khoản, cả identity-based policy của principal ở tài khoản nguồn và resource-based policy của tài nguyên ở tài khoản đích đều phải cho phép. Tuy nhiên, có những trường hợp đặc biệt như với KMS, nơi key policy đóng vai trò trung tâm hơn trong việc *cho phép* các IAM policies khác có hiệu lực đối với key.35

**Logic Đánh giá Policy (Policy Evaluation Logic)** 18**:**

Khi một principal (user, role) thực hiện một yêu cầu đến AWS, hệ thống IAM sẽ đánh giá tất cả các chính sách có liên quan để quyết định liệu yêu cầu đó có được phép hay không. Logic đánh giá này khá phức tạp nhưng có thể tóm tắt như sau:

1. **Mặc định Từ chối (Default Deny):** Tất cả các yêu cầu đều bị từ chối theo mặc định. Để một yêu cầu được phép, phải có một statement Allow rõ ràng trong một chính sách áp dụng.  
2. **Ưu tiên của Explicit Deny:** Nếu có bất kỳ statement Deny rõ ràng nào trong bất kỳ chính sách nào áp dụng cho yêu cầu (identity-based, resource-based, permissions boundary, session policy, hoặc SCP), yêu cầu đó sẽ bị từ chối ngay lập tức, bất kể có statement Allow nào khác hay không. Deny luôn thắng Allow.  
3. **Đánh giá các loại Policy theo thứ tự:**  
   * **AWS Organizations SCPs (Service Control Policies):** Nếu tài khoản là thành viên của một AWS Organization, SCPs được đánh giá trước tiên. SCPs hoạt động như những "guardrails" ở cấp độ tổ chức hoặc OU, đặt ra giới hạn tối đa cho các quyền có thể được cấp trong các tài khoản thành viên. Chúng không tự cấp quyền mà chỉ có thể giới hạn những gì được phép bởi các chính sách khác.18  
   * **Resource-based Policies và Identity-based Policies (trong cùng tài khoản):** Khi một principal trong cùng tài khoản truy cập một tài nguyên, quyền hiệu quả là sự *kết hợp (union)* của những gì được cho phép bởi cả identity-based policy của principal và resource-based policy của tài nguyên (nếu có). Yêu cầu được phép nếu một trong hai (hoặc cả hai) cho phép, và không có explicit deny.  
   * **Permissions Boundaries:** Nếu một IAM user hoặc role có một permissions boundary đính kèm, quyền hiệu quả của user/role đó là sự *giao thoa (intersection)* giữa identity-based policies của nó và permissions boundary. Một hành động chỉ được phép nếu nó được cho phép bởi cả hai.  
   * **Session Policies:** Khi một role được đảm nhận và một session policy được truyền vào, quyền hiệu quả của session tạm thời đó là sự *giao thoa (intersection)* giữa identity-based policy của role và session policy đó.  
4. **Ảnh hưởng của Conditions:** Các Condition elements trong các statements của policy phải được thỏa mãn (đánh giá là true) dựa trên ngữ cảnh của request thì statement đó mới có hiệu lực.

Hệ thống chính sách của AWS IAM, với sự đa dạng về loại chính sách, cấu trúc JSON chi tiết và logic đánh giá nhiều lớp, vừa là một công cụ cực kỳ mạnh mẽ để kiểm soát truy cập, vừa là một thách thức không nhỏ đối với người quản trị. Sức mạnh của nó nằm ở khả năng cho phép các tổ chức định nghĩa quyền truy cập với mức độ chi tiết gần như vô hạn, từ đó thực thi nguyên tắc đặc quyền tối thiểu một cách hiệu quả. Tuy nhiên, sự phức tạp này cũng chính là thách thức. Việc hiểu đúng cách các loại chính sách tương tác với nhau, cách logic đánh giá được áp dụng, và cách cấu hình chính xác các thành phần như Principal, Action, Resource, Condition đòi hỏi kiến thức và kinh nghiệm.  
Khi có nhiều lớp chính sách cùng áp dụng cho một yêu cầu – ví dụ, một identity-based policy, một resource-based policy, một permissions boundary, và một SCP – việc xác định quyền hiệu quả cuối cùng có thể trở nên rất khó khăn nếu chỉ dựa vào việc đọc các tài liệu JSON. Một "Allow" trong identity-based policy có thể bị vô hiệu hóa bởi một "Deny" trong SCP, hoặc không có hiệu lực do bị giới hạn bởi một permissions boundary, hoặc đơn giản là không khớp với resource-based policy.  
Chính vì sự phức tạp này mà các công cụ hỗ trợ như IAM Policy Simulator trở nên vô cùng quan trọng.42 Chúng giúp người quản trị "thử nghiệm" các chính sách, mô phỏng các kịch bản truy cập khác nhau và hiểu rõ hơn về cách các chính sách sẽ được đánh giá trong thực tế, từ đó giảm thiểu lỗi cấu hình và đảm bảo an ninh cho môi trường AWS.

## **Phần III: Xác thực và Ủy quyền Nâng cao**

Ngoài các thành phần cơ bản, AWS IAM còn cung cấp nhiều tính năng và cơ chế nâng cao để tăng cường bảo mật và linh hoạt trong việc quản lý xác thực và ủy quyền. Các tính năng này bao gồm Multi-Factor Authentication (MFA), AWS Security Token Service (STS) cho thông tin xác thực tạm thời, Session Policies, Permissions Boundaries, và Service Control Policies (SCPs).

### **A. Multi-Factor Authentication (MFA)**

Multi-Factor Authentication (MFA) là một biện pháp bảo mật nền tảng và được AWS khuyến nghị mạnh mẽ. Nó bổ sung một lớp bảo vệ thứ hai cho quá trình đăng nhập, yêu cầu người dùng cung cấp không chỉ thông tin họ biết (như mật khẩu hoặc access key) mà còn cả thông tin từ một yếu tố xác thực thứ hai mà họ sở hữu (như một thiết bị vật lý hoặc ứng dụng tạo mã) hoặc một đặc điểm sinh trắc học của họ.45 Việc triển khai MFA giúp tăng cường đáng kể khả năng chống lại các cuộc tấn công chiếm đoạt tài khoản, ngay cả khi thông tin xác thực chính (mật khẩu/access key) bị lộ.

**Tầm quan trọng của MFA:**

MFA được coi là một trong những biện pháp bảo mật hiệu quả nhất để bảo vệ tài khoản AWS. Nó giải quyết điểm yếu cố hữu của việc chỉ dựa vào mật khẩu, vốn có thể bị đánh cắp, đoán mò, hoặc bẻ khóa. Bằng cách yêu cầu một yếu tố xác thực bổ sung, MFA làm cho việc truy cập trái phép trở nên khó khăn hơn rất nhiều.

**Các loại MFA được hỗ trợ** 45**:**

AWS IAM hỗ trợ nhiều loại thiết bị và phương thức MFA khác nhau, cho phép các tổ chức lựa chọn giải pháp phù hợp nhất với nhu cầu bảo mật và sự tiện lợi của người dùng:

1. **Passkeys và Security Keys (FIDO \- Fast Identity Online):**  
   * Đây là các phương thức dựa trên tiêu chuẩn FIDO, sử dụng mật mã khóa công khai để cung cấp khả năng xác thực mạnh mẽ, chống lừa đảo (phishing-resistant) và thường dễ sử dụng hơn.  
   * **Passkeys:** Là các khóa kỹ thuật số được tạo bởi các nhà cung cấp passkey (như iCloud Keychain, Google Password Manager, 1Password, Dashlane) sử dụng dấu vân tay, nhận dạng khuôn mặt, hoặc mã PIN của thiết bị. Passkeys có thể được đồng bộ hóa trên các thiết bị của người dùng.  
   * **Security Keys (còn gọi là device-bound passkeys):** Là các thiết bị phần cứng vật lý (thường là USB token) từ các nhà cung cấp bên thứ ba như Yubico. Một security key có thể được sử dụng để bảo vệ nhiều tài khoản root và IAM users.  
   * *Tính khả dụng:* Passkeys và security keys được hỗ trợ cho người dùng root và IAM users ở hầu hết các AWS Regions, ngoại trừ một số Region đặc biệt như Trung Quốc.  
2. **Virtual Authenticator Apps (Ứng dụng xác thực ảo):**  
   * Đây là các ứng dụng phần mềm (thường chạy trên điện thoại thông minh) triển khai thuật toán Time-based One-Time Password (TOTP). Chúng tạo ra các mã số gồm 6 chữ số, thay đổi sau mỗi khoảng thời gian ngắn (thường là 30 giây).  
   * *Ví dụ phổ biến:* Google Authenticator, Microsoft Authenticator, Twilio Authy Authenticator, Duo Mobile, Symantec VIP.  
   * Một ứng dụng xác thực ảo có thể hỗ trợ nhiều token (tức là bảo vệ nhiều tài khoản) trên cùng một thiết bị.  
   * *Tính khả dụng:* Được hỗ trợ cho IAM users ở tất cả các AWS Regions, bao gồm cả AWS GovCloud (US).  
3. **Hardware TOTP Tokens (Thiết bị TOTP phần cứng):**  
   * Là các thiết bị vật lý nhỏ gọn, chuyên dụng để tạo mã TOTP. Chúng cũng hiển thị mã số 6 chữ số thay đổi theo thời gian.  
   * Các nhà cung cấp được hỗ trợ bao gồm Thales (cho các tài khoản AWS tiêu chuẩn) và Hypersecu (cho các Region AWS GovCloud (US)).  
   * Để đảm bảo tính tương thích và bảo mật, các thiết bị TOTP phần cứng này phải được mua thông qua các liên kết được AWS cung cấp, vì AWS cần "token seeds" (mầm khóa) duy nhất để hoạt động.

**Bảng 3: Các tùy chọn MFA và tính năng**

| Loại MFA | Mô tả ngắn | Cơ chế hoạt động | Ưu điểm | Nhược điểm/Lưu ý | Tính khả dụng (ví dụ) |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Passkeys/Security Keys (FIDO)** | Khóa kỹ thuật số hoặc thiết bị vật lý dựa trên mật mã khóa công khai. | FIDO | Rất an toàn, chống lừa đảo hiệu quả, có thể không cần nhập mã, tiện lợi (passkeys). | Cần thiết bị/phần mềm hỗ trợ FIDO. Security key vật lý có thể bị mất/hỏng. | Hầu hết các AWS Regions (trừ Trung Quốc) cho root/IAM users.45 |
| **Virtual Authenticator App** | Ứng dụng phần mềm trên smartphone/desktop tạo mã TOTP. | TOTP | Miễn phí, dễ cài đặt, hỗ trợ nhiều tài khoản trên một thiết bị. | Cần smartphone/thiết bị cài app. Có thể bị ảnh hưởng nếu thiết bị bị xâm phạm. | Tất cả Regions cho IAM users, bao gồm GovCloud (US).45 |
| **Hardware TOTP Token** | Thiết bị phần cứng chuyên dụng tạo mã TOTP. | TOTP | An toàn hơn app ảo nếu thiết bị chuyên dụng. Không phụ thuộc vào smartphone. | Chi phí mua thiết bị. Cần mang theo thiết bị. Phải mua từ nguồn AWS chỉ định. | Tất cả Regions (Thales), GovCloud (US) (Hypersecu).45 |

**Cách kích hoạt và quản lý MFA:**

Việc kích hoạt và quản lý MFA có thể được thực hiện thông qua IAM console 45:

* **Đối với Root User:** Đây là ưu tiên hàng đầu sau khi tạo tài khoản AWS. Việc kích hoạt MFA cho root user là một trong những biện pháp bảo mật quan trọng nhất để bảo vệ toàn bộ tài khoản.45 AWS thậm chí còn khuyến nghị kích hoạt nhiều thiết bị MFA cho root user để tăng tính linh hoạt và khả năng phục hồi.  
* **Đối với IAM Users:** Nên yêu cầu MFA cho tất cả IAM users, đặc biệt là những người dùng có quyền quản trị hoặc quyền truy cập vào các tài nguyên nhạy cảm.2 Quản trị viên có thể cấu hình chính sách yêu cầu người dùng phải tự kích hoạt MFA.  
* **Quản lý thiết bị:** Người dùng thường có thể tự quản lý các thiết bị MFA của mình (ví dụ: thêm thiết bị mới, xóa thiết bị cũ) trong cài đặt bảo mật của họ. Quản trị viên cũng có thể quản lý thiết bị MFA cho người dùng trong IAM console.  
* **Khóa MFA miễn phí:** AWS cung cấp chương trình khóa MFA bảo mật miễn phí cho các chủ tài khoản AWS đủ điều kiện ở Hoa Kỳ, có thể đặt hàng thông qua Security Hub console.45

Việc AWS liên tục mở rộng hỗ trợ cho các tiêu chuẩn MFA hiện đại như FIDO 45 và không ngừng nhấn mạnh tầm quan trọng của việc sử dụng MFA cho cả người dùng root và IAM users 2 cho thấy một điều rõ ràng: MFA không còn là một tùy chọn "có thì tốt" (nice-to-have) mà đã trở thành một yêu cầu "bắt buộc phải có" (must-have) trong bất kỳ chiến lược bảo mật IAM nghiêm túc nào. Trong bối cảnh các mối đe dọa mạng ngày càng tinh vi, MFA đóng vai trò như một lớp phòng thủ thiết yếu, một trong những biện pháp hiệu quả nhất để chống lại các hình thức tấn công phổ biến như đánh cắp thông tin xác thực và chiếm đoạt tài khoản. Bỏ qua việc triển khai MFA đồng nghĩa với việc chấp nhận một rủi ro bảo mật đáng kể và không cần thiết.

### **B. AWS Security Token Service (STS)**

AWS Security Token Service (STS) là một dịch vụ web toàn cầu cho phép bạn yêu cầu các **thông tin xác thực tạm thời, có đặc quyền giới hạn** cho IAM users hoặc cho những người dùng mà bạn xác thực (ví dụ: người dùng liên kết \- federated users).48 Thay vì sử dụng thông tin xác thực dài hạn (như access keys của IAM user), việc sử dụng thông tin xác thực tạm thời từ STS là một phương pháp bảo mật tốt nhất vì chúng có thời hạn sử dụng ngắn, giúp giảm thiểu rủi ro nếu bị lộ.

**Mục đích của STS:**

Mục đích chính của STS là cung cấp một cơ chế an toàn và linh hoạt để cấp quyền truy cập tạm thời vào tài nguyên AWS. Điều này rất quan trọng trong nhiều kịch bản, bao gồm:

* Ủy quyền truy cập cho các ứng dụng chạy trên EC2 instances hoặc Lambda functions.  
* Cho phép truy cập liên tài khoản (cross-account access).  
* Tích hợp với các hệ thống quản lý danh tính bên ngoài thông qua liên kết danh tính (identity federation).  
* Thực thi các yêu cầu MFA cho các lệnh gọi API theo chương trình.

**Các API chính của STS và trường hợp sử dụng:**

STS cung cấp một số API quan trọng để tạo và quản lý thông tin xác thực tạm thời:

1. **AssumeRole** 17**:**  
   * **Mục đích:** Được sử dụng bởi một IAM user, một ứng dụng, hoặc một dịch vụ AWS để đảm nhận một IAM role đã được định nghĩa trước.  
   * **Đầu vào chính:** Amazon Resource Name (ARN) của IAM role cần đảm nhận, một tên session (session name) để định danh session.  
   * **Đầu ra:** Một bộ thông tin xác thực tạm thời (access key ID, secret access key, session token) cho phép thực thể gọi API hành động với các quyền của role đó.  
   * **Trường hợp sử dụng:** Rất quan trọng cho việc cấp quyền cho EC2 instances và Lambda functions, cho phép truy cập liên tài khoản, và là nền tảng cho nhiều dịch vụ AWS hoạt động thay mặt người dùng.  
2. **AssumeRoleWithSAML** 22**:**  
   * **Mục đích:** Được sử dụng trong các kịch bản liên kết danh tính dựa trên SAML 2.0.  
   * **Đầu vào chính:** SAML assertion (một tài liệu XML chứa thông tin xác thực và thuộc tính người dùng từ Identity Provider \- IdP), ARN của IAM role mà người dùng liên kết sẽ đảm nhận, ARN của SAML provider đã được cấu hình trong IAM.  
   * **Đầu ra:** Thông tin xác thực tạm thời của AWS.  
   * **Trường hợp sử dụng:** Cho phép người dùng từ các hệ thống doanh nghiệp (ví dụ: Microsoft Active Directory, Okta) đăng nhập vào AWS bằng thông tin xác thực hiện có của họ.  
3. **AssumeRoleWithWebIdentity** 23**:**  
   * **Mục đích:** Được sử dụng trong các kịch bản liên kết danh tính dựa trên OpenID Connect (OIDC), thường với các nhà cung cấp danh tính web công cộng.  
   * **Đầu vào chính:** OIDC token (thường là JSON Web Token \- JWT) từ một OIDC provider (ví dụ: Google, Facebook, Amazon Cognito), ARN của IAM role mà người dùng web/mobile sẽ đảm nhận.  
   * **Đầu ra:** Thông tin xác thực tạm thời của AWS.  
   * **Trường hợp sử dụng:** Cho phép các ứng dụng di động hoặc web xác thực người dùng thông qua các nhà cung cấp OIDC và sau đó cấp cho họ quyền truy cập tạm thời vào tài nguyên AWS.  
4. **GetFederationToken** 48**:**  
   * **Mục đích:** Trả về một bộ thông tin xác thực tạm thời cho một IAM user. API này phải được gọi bằng thông tin xác thực dài hạn của IAM user đó.  
   * **Đầu vào chính:** Tên của federated user (do bạn định nghĩa), thời hạn session, và tùy chọn là một policy để giới hạn thêm quyền.  
   * **Đầu ra:** Thông tin xác thực tạm thời.  
   * **Trường hợp sử dụng:** Thường được sử dụng trong các ứng dụng proxy hoặc các hệ thống quản lý danh tính tùy chỉnh (custom identity broker) để cấp quyền truy cập chi tiết hơn cho người dùng mà không cần tạo IAM user riêng cho mỗi người. Thông tin xác thực trả về có một số hạn chế: không thể gọi các API của IAM và chỉ có thể gọi API GetCallerIdentity của STS.  
5. **GetSessionToken** 49**:**  
   * **Mục đích:** Trả về một bộ thông tin xác thực tạm thời cho một IAM user hoặc người dùng root. API này cũng phải được gọi bằng thông tin xác thực dài hạn.  
   * **Đầu vào chính:** Thời hạn session, và quan trọng là có thể bao gồm thông tin MFA (số serial của thiết bị MFA và mã token).  
   * **Đầu ra:** Thông tin xác thực tạm thời.  
   * **Trường hợp sử dụng:** Mục đích chính là để xác thực người dùng bằng MFA cho các lệnh gọi API theo chương trình. Người dùng có MFA có thể gọi API này, cung cấp mã MFA, để nhận được một session token tạm thời mà sau đó có thể được sử dụng để gọi các API khác yêu cầu MFA. Thông tin xác thực này cũng có hạn chế: không thể gọi các API của IAM trừ khi có thông tin MFA, và không thể gọi các API của STS trừ AssumeRole hoặc GetCallerIdentity.

**Thông tin xác thực bảo mật tạm thời (Temporary Security Credentials):**

Như đã đề cập, kết quả của các lệnh gọi API STS thành công là một bộ thông tin xác thực tạm thời. Bộ này bao gồm ba thành phần 17:

1. **Access Key ID:** Tương tự như Access Key ID của thông tin xác thực dài hạn, nhưng là tạm thời.  
2. **Secret Access Key:** Tương tự như Secret Access Key của thông tin xác thực dài hạn, nhưng là tạm thời.  
3. **Session Token (hoặc Security Token):** Một giá trị bổ sung phải được bao gồm trong các lệnh gọi API AWS khi sử dụng thông tin xác thực tạm thời.

Những thông tin xác thực này có thời hạn sử dụng được xác định trước (ví dụ: từ 15 phút đến 12 giờ cho AssumeRole, hoặc lên đến 36 giờ cho GetFederationToken và GetSessionToken), sau đó chúng sẽ không còn hợp lệ.

**Bảng 4: Tổng quan các API của STS để lấy thông tin xác thực tạm thời**

| Tên API | Mục đích chính | Ai gọi API này? | Đầu vào chính | Loại credentials trả về | Trường hợp sử dụng điển hình |
| :---- | :---- | :---- | :---- | :---- | :---- |
| AssumeRole | Đảm nhận một IAM Role hiện có. | IAM User, Application, AWS Service | ARN của Role, Session Name | Tạm thời | Cấp quyền cho EC2/Lambda, cross-account access, service roles. 17 |
| AssumeRoleWithSAML | Trao đổi SAML assertion lấy credentials AWS. | Ứng dụng (thay mặt người dùng liên kết SAML) | SAML Assertion, ARN của Role, ARN của SAML Provider | Tạm thời | Liên kết danh tính doanh nghiệp (ví dụ: ADFS, Okta) với AWS. 50 |
| AssumeRoleWithWebIdentity | Trao đổi OIDC token lấy credentials AWS. | Ứng dụng (thay mặt người dùng liên kết OIDC) | OIDC Token, ARN của Role | Tạm thời | Đăng nhập vào ứng dụng web/mobile bằng Google, Facebook, Amazon Cognito, sau đó truy cập AWS. 52 |
| GetFederationToken | Cấp credentials tạm thời cho một "federated user" (được quản lý bởi người gọi). | IAM User (thường là một ứng dụng proxy/broker) | Tên Federated User, Duration, Policy (tùy chọn) | Tạm thời | Hệ thống quản lý danh tính tùy chỉnh, cấp quyền chi tiết mà không tạo IAM user. 48 |
| GetSessionToken | Cấp credentials tạm thời cho IAM User/Root, thường với mục đích MFA. | IAM User, Root User | Duration, Serial Number (MFA), Token Code (MFA) | Tạm thời | Cho phép IAM users có MFA thực hiện các lệnh gọi API được bảo vệ bởi MFA. 49 |

AWS Security Token Service (STS) đóng vai trò như một "động cơ" trung tâm, cung cấp năng lượng cho hầu hết các mô hình truy cập động và an toàn trong AWS. Nguyên tắc sử dụng thông tin xác thực tạm thời là một trong những khuyến nghị bảo mật quan trọng nhất của AWS 13, và STS chính là dịch vụ hiện thực hóa nguyên tắc này.  
Khi xem xét các kịch bản truy cập phổ biến:

* Các ứng dụng chạy trên EC2 hoặc Lambda cần quyền để tương tác với các dịch vụ khác? Chúng sử dụng IAM Roles, và việc đảm nhận vai trò đó (role assumption) được thực hiện ngầm thông qua cơ chế của STS, trả về thông tin xác thực tạm thời cho instance hoặc function.  
* Cần truy cập tài nguyên ở một tài khoản AWS khác? IAM Roles và lệnh gọi AssumeRole của STS là giải pháp tiêu chuẩn.  
* Muốn tích hợp với hệ thống danh tính doanh nghiệp (Active Directory, Okta) hoặc các nhà cung cấp danh tính web (Google, Facebook)? Liên kết danh tính (Identity Federation) dựa trên SAML hoặc OIDC sử dụng các API AssumeRoleWithSAML hoặc AssumeRoleWithWebIdentity của STS để trao đổi assertion/token lấy thông tin xác thực tạm thời của AWS.  
* Ngay cả khi một IAM user cần thực hiện các hành động yêu cầu MFA theo chương trình, họ cũng sử dụng GetSessionToken của STS để có được một session tạm thời đã được xác thực MFA. Như vậy, STS không chỉ là một dịch vụ phụ trợ mà là một thành phần không thể thiếu, làm nền tảng cho việc triển khai các kiến trúc IAM hiện đại, an toàn và linh hoạt. Việc hiểu rõ các API của STS và cách chúng tương tác với IAM Roles và các cơ chế liên kết là chìa khóa để thiết kế các giải pháp truy cập tuân thủ nguyên tắc đặc quyền tối thiểu và giảm thiểu rủi ro từ việc sử dụng thông tin xác thực dài hạn.

### **C. Session Policies**

Session Policies là một tính năng nâng cao trong AWS IAM cho phép bạn giới hạn thêm các quyền của một session thông tin xác thực tạm thời khi một IAM Role được đảm nhận. Chúng được truyền vào như một tham số trong các lệnh gọi API của STS như AssumeRole, AssumeRoleWithSAML, và AssumeRoleWithWebIdentity.17

**Cách sử dụng với các lệnh AssumeRole:**

Khi bạn thực hiện một lệnh gọi API để đảm nhận một vai trò (ví dụ: AssumeRole), bạn có thể tùy chọn truyền vào một hoặc nhiều session policies. Có hai cách để làm điều này:

1. **Inline Session Policy:** Bạn có thể cung cấp một tài liệu chính sách JSON duy nhất trực tiếp trong lệnh gọi API.  
2. **Managed Session Policies:** Bạn có thể chỉ định Amazon Resource Names (ARNs) của tối đa 10 managed policies (cả AWS managed policies và customer managed policies) đã tồn tại.

Tổng kích thước (dưới dạng plaintext) của tất cả các inline session policies và managed session policies được truyền vào không được vượt quá 2,048 ký tự.17 Tuy nhiên, AWS cũng thực hiện một quá trình chuyển đổi để nén các policy này (cùng với session tags nếu có) thành một định dạng nhị phân đóng gói (packed binary format) có giới hạn kích thước riêng. Một yêu cầu có thể thất bại do vượt quá giới hạn này ngay cả khi tổng kích thước plaintext đáp ứng yêu cầu 2,048 ký tự. Phản hồi từ API STS thường bao gồm một trường PackedPolicySize cho biết tỷ lệ phần trăm của giới hạn kích thước đóng gói đã được sử dụng.52

**Mục đích trong các kịch bản nâng cao:**

Mục đích chính của session policies là để **áp dụng nguyên tắc đặc quyền tối thiểu một cách linh hoạt và theo ngữ cảnh** cho các session tạm thời. Thay vì phải tạo ra nhiều IAM Roles với các biến thể quyền hạn rất nhỏ cho từng trường hợp sử dụng cụ thể (dẫn đến "role explosion"), bạn có thể sử dụng một Role có bộ quyền cơ bản rộng hơn một chút, và sau đó, khi Role đó được đảm nhận cho một tác vụ cụ thể, bạn truyền vào một session policy để thu hẹp các quyền đó xuống chỉ còn những gì thực sự cần thiết cho session đó.

Điều này đặc biệt hữu ích trong các kịch bản:

* Khi một ứng dụng hoặc người dùng đảm nhận một Role, nhưng chỉ cần thực hiện một tập hợp con các hành động mà Role đó cho phép.  
* Khi cần cấp quyền truy cập vào các tài nguyên cụ thể dựa trên thông tin chỉ có sẵn tại thời điểm request (ví dụ: cho phép người dùng chỉ truy cập vào "thư mục nhà" của họ trong một S3 bucket chung).  
* Khi muốn áp dụng các giới hạn tạm thời cho một session mà không cần phải thay đổi identity-based policy của Role chính.

**Tương tác với các policy khác:**

Điểm quan trọng nhất cần nhớ về session policies là cách chúng tương tác với identity-based policy (permissions policy) của IAM Role đang được đảm nhận 17:

* Quyền hạn của session tạm thời kết quả sẽ là **sự giao thoa (intersection)** của các quyền được cấp bởi identity-based policy của Role VÀ các quyền được cấp bởi tất cả các session policies được truyền vào.  
* Điều này có nghĩa là một hành động chỉ được phép trong session tạm thời nếu nó được cho phép bởi CẢ identity-based policy của Role VÀ (các) session policy.  
* Do đó, session policies **không thể được sử dụng để cấp thêm quyền** mà Role đó vốn không có. Chúng chỉ có thể **giới hạn hoặc thu hẹp** các quyền đã có của Role. Nếu một quyền không được cho phép trong identity-based policy của Role, thì việc cho phép nó trong session policy cũng sẽ không có tác dụng.

Session Policies cung cấp một cơ chế cực kỳ mạnh mẽ và linh hoạt để tinh chỉnh quyền truy cập ở mức độ session, cho phép các tổ chức triển khai một dạng "đặc quyền tối thiểu just-in-time" một cách hiệu quả. Hãy tưởng tượng một IAM Role có một tập hợp các quyền cố định được định nghĩa trong identity-based policy của nó. Tuy nhiên, trong nhiều trường hợp, một người dùng hoặc một ứng dụng khi đảm nhận Role đó có thể chỉ cần một phần rất nhỏ trong số các quyền đó để hoàn thành một tác vụ cụ thể. Việc tạo ra vô số IAM Roles chỉ để phục vụ các biến thể quyền nhỏ nhặt này sẽ dẫn đến tình trạng "role explosion", làm cho việc quản lý trở nên vô cùng phức tạp.  
Session Policies giải quyết vấn đề này bằng cách cho phép truyền một hoặc nhiều chính sách "động" vào thời điểm Role được đảm nhận.17 Những chính sách động này sau đó sẽ "giao" với chính sách cố định của Role, chỉ giữ lại những quyền được cả hai bên cho phép.17 Kết quả là, bạn có thể "thu hẹp" quyền hạn của một Role một cách linh hoạt cho từng session cụ thể, dựa trên nhu cầu thực tế của tác vụ đang được thực hiện, mà không cần phải thay đổi chính sách gốc của Role hay tạo thêm Role mới.  
Ví dụ, một Role có thể có quyền đọc và ghi đầy đủ đối với một S3 bucket. Khi một ứng dụng đảm nhận Role này để thực hiện một tác vụ chỉ cần đọc dữ liệu từ một prefix cụ thể trong bucket đó, ứng dụng có thể truyền vào một Session Policy chỉ cho phép hành động s3:GetObject đối với arn:aws:s3:::mybucket/specific-prefix/\*. Quyền hiệu quả của session đó sẽ chỉ là đọc từ prefix đó, mặc dù Role gốc có nhiều quyền hơn. Đây là một cách tiếp cận rất hiệu quả để thực thi nguyên tắc đặc quyền tối thiểu một cách chi tiết và theo ngữ cảnh, đồng thời giữ cho số lượng Roles cần quản lý ở mức hợp lý.

### **D. Permissions Boundaries**

Permissions Boundaries là một tính năng nâng cao của AWS IAM được thiết kế để giúp các nhóm quản trị IAM trung tâm có thể ủy quyền một cách an toàn việc tạo và quản lý IAM users và roles cho các nhà phát triển hoặc các quản trị viên khác trong tổ chức, mà vẫn đảm bảo các giới hạn về quyền hạn được tuân thủ.7 Về cơ bản, một permissions boundary là một managed policy (AWS managed hoặc customer managed) được sử dụng để đặt ra một "ranh giới" hoặc "giới hạn tối đa" cho các quyền mà một IAM identity (user hoặc role) có thể có.

**Mục đích của Permissions Boundaries:**

* **Ủy quyền an toàn (Safe Delegation):** Cho phép các quản trị viên trung tâm trao quyền cho các nhóm phát triển hoặc các đơn vị kinh doanh khác tự tạo và quản lý IAM roles và policies cần thiết cho ứng dụng của họ. Điều này giúp tăng tốc độ phát triển và giảm tải cho đội ngũ IAM trung tâm, mà không phải lo lắng rằng các nhóm này sẽ vô tình hoặc cố ý cấp cho mình hoặc các tài nguyên của họ những quyền hạn quá rộng, vượt ra ngoài phạm vi dự kiến.7  
* **Ngăn chặn leo thang đặc quyền (Preventing Privilege Escalation):** Ngay cả khi một nhà phát triển (đã được ủy quyền) tạo ra một IAM role với một identity-based policy rất rộng rãi (ví dụ, cho phép tất cả các hành động trên tất cả các tài nguyên \- \*:\*), nếu role đó có một permissions boundary đính kèm, thì các quyền hiệu quả của role đó sẽ không bao giờ vượt quá những gì được cho phép bởi permissions boundary.7  
* **Phân chia quyền quản lý trong cùng một tài khoản:** Cho phép các nhóm khác nhau trong cùng một tài khoản AWS quản lý các bộ quyền riêng biệt cho các ứng dụng hoặc dự án của họ, trong khi vẫn tuân thủ các giới hạn chung do quản trị viên cấp cao hơn đặt ra.55

**Tương tác với các policy khác:**

Điều cực kỳ quan trọng cần hiểu là một permissions boundary **không tự nó cấp bất kỳ quyền nào** cho một IAM identity. Nó chỉ hoạt động như một bộ lọc hoặc một giới hạn.7 Các quyền thực tế của một IAM user hoặc role được xác định bởi sự **giao thoa (intersection)** giữa:

1. **Identity-based policies** của user/role đó (bao gồm cả AWS managed policies, customer managed policies, và inline policies đính kèm).  
2. **Permissions boundary** được đính kèm với user/role đó.

Một hành động chỉ được phép nếu nó được cho phép bởi CẢ identity-based policies VÀ permissions boundary.18 Nếu một hành động được cho phép bởi identity-based policy nhưng bị từ chối (hoặc không được cho phép) bởi permissions boundary, thì hành động đó sẽ bị từ chối. Ngược lại, nếu một hành động được cho phép bởi permissions boundary nhưng không được cho phép bởi identity-based policy, hành động đó cũng sẽ bị từ chối (vì permissions boundary không cấp quyền).

Hơn nữa, nếu tài khoản là một phần của AWS Organization và có Service Control Policies (SCPs) áp dụng, thì quyền hiệu quả cuối cùng phải được cho phép bởi cả identity-based policy, permissions boundary, VÀ SCP.8

Để đảm bảo permissions boundary được áp dụng, quản trị viên trung tâm có thể sử dụng một policy đính kèm cho nhà phát triển (người được ủy quyền tạo role) yêu cầu rằng bất kỳ role nào họ tạo ra phải có một permissions boundary cụ thể được đính kèm (sử dụng condition key iam:PermissionsBoundary).7

Permissions Boundaries không phải là một cơ chế để cấp quyền trực tiếp cho người dùng hay vai trò. Thay vào đó, chúng là một công cụ quản trị tinh vi, được thiết kế để kiểm soát quá trình ủy quyền việc tạo ra các thực thể (users, roles) có quyền. Hãy tưởng tượng một kịch bản: quản trị viên IAM trung tâm muốn trao quyền cho các nhóm phát triển trong công ty tự tạo IAM roles cho các ứng dụng của họ để tăng tốc độ triển khai và giảm sự phụ thuộc vào đội ngũ trung tâm.7 Tuy nhiên, họ cũng lo ngại rằng các nhóm phát triển, do thiếu kinh nghiệm hoặc vô ý, có thể tạo ra các roles với những quyền hạn quá rộng, gây ra rủi ro bảo mật nghiêm trọng cho toàn bộ tài khoản.7  
Đây là lúc Permissions Boundary phát huy tác dụng. Quản trị viên trung tâm sẽ tạo ra một hoặc nhiều managed policies đóng vai trò là Permissions Boundaries. Những policy này định nghĩa "phạm vi quyền tối đa" mà bất kỳ role nào do nhóm phát triển tạo ra có thể có.7 Sau đó, khi ủy quyền cho nhóm phát triển (ví dụ, thông qua một IAM role mà họ đảm nhận), quản trị viên trung tâm sẽ yêu cầu (thông qua một condition trong policy của nhóm phát triển) rằng bất kỳ role mới nào được tạo ra phải được đính kèm với một trong những Permissions Boundaries này.7  
Khi nhóm phát triển tạo một role mới, họ vẫn định nghĩa các identity-based policies (permissions policies) cho role đó như bình thường, chỉ rõ những gì họ muốn role đó có thể làm. Tuy nhiên, các quyền thực tế mà role mới này có được sẽ là "phần chung" (intersection) giữa những gì được định nghĩa trong identity-based policy của nó và những gì được cho phép bởi Permissions Boundary đã đính kèm.7  
Như vậy, Permissions Boundary hoạt động như một "lưới an toàn" do quản trị viên trung tâm kiểm soát. Nó không cấp quyền, mà "giới hạn" những quyền mà người khác (trong trường hợp này là nhóm phát triển) có thể cấp cho các thực thể mà họ tạo ra. Đây thực chất là một công cụ để "quản lý những người quản lý quyền", cho phép một mô hình ủy quyền linh hoạt hơn trong khi vẫn duy trì các rào cản bảo mật cần thiết.

### **E. Service Control Policies (SCPs) trong AWS Organizations**

Service Control Policies (SCPs) là một tính năng mạnh mẽ của AWS Organizations, cho phép quản lý quyền ở cấp độ tổ chức một cách tập trung. Chúng đóng vai trò như những "guardrails" (hàng rào bảo vệ) để kiểm soát các quyền tối đa có sẵn cho tất cả IAM users và IAM roles (bao gồm cả root user) trong các tài khoản thành viên của một tổ chức hoặc một Organizational Unit (OU) cụ thể.8

**Vai trò trong quản trị tập trung:**

SCPs là công cụ then chốt để các quản trị viên trung tâm thực thi các chính sách tuân thủ và bảo mật trên toàn bộ tổ chức AWS của họ. Bằng cách đính kèm SCPs vào gốc (root) của tổ chức, các OUs, hoặc các tài khoản thành viên riêng lẻ, quản trị viên có thể đảm bảo rằng không có tài khoản nào có thể thực hiện các hành động vượt quá giới hạn đã được thiết lập, ngay cả khi quản trị viên của tài khoản thành viên đó cố gắng cấp các quyền rộng hơn thông qua IAM policies.

**Cách giới hạn quyền truy cập giữa các tài khoản:**

Một điều cực kỳ quan trọng cần hiểu về SCPs là chúng **không cấp bất kỳ quyền nào**. Thay vào đó, chúng chỉ định nghĩa các **quyền tối đa** được phép hoặc các hành động bị **từ chối rõ ràng**.8 Để một principal (user hoặc role) trong một tài khoản thành viên có thể thực hiện một hành động, hành động đó phải:

1. Được cho phép bởi các IAM policies (identity-based và/hoặc resource-based) áp dụng cho principal đó.  
2. Được cho phép (hoặc không bị từ chối) bởi tất cả các SCPs áp dụng cho tài khoản đó (từ gốc tổ chức, qua các OUs, đến chính tài khoản).

Nếu một quyền bị chặn bởi một SCP ở bất kỳ cấp nào trong hệ thống phân cấp của AWS Organizations (ví dụ, một SCP ở cấp OU từ chối quyền tạo một loại instance EC2 nhất định), thì không một user hay role nào trong các tài khoản thuộc OU đó có thể thực hiện hành động đó, ngay cả khi họ có policy AdministratorAccess trong IAM.8

**Ảnh hưởng đến Root User:**

Một trong những khía cạnh mạnh mẽ nhất của SCPs là chúng áp dụng cho **tất cả** IAM users và roles trong một tài khoản thành viên, **bao gồm cả root user của tài khoản thành viên đó**.8 Điều này có nghĩa là ngay cả thực thể có quyền cao nhất trong một tài khoản thành viên cũng không thể vượt qua các giới hạn do SCPs đặt ra. Tuy nhiên, SCPs **không** áp dụng cho **tài khoản quản lý (management account)** của AWS Organization. Tài khoản quản lý có toàn quyền đối với tổ chức và không bị giới hạn bởi SCPs mà nó tạo ra.

**Tương tác với các loại policy khác:**

Quyền hiệu quả của một principal trong một tài khoản thành viên được xác định bởi sự tương tác phức tạp của nhiều loại policy 8:

* **SCPs:** Đặt ra giới hạn tối đa.  
* **Identity-based Policies:** Cấp quyền cho user/role.  
* **Resource-based Policies:** Cấp quyền cho principal truy cập tài nguyên.  
* **Permissions Boundaries:** Giới hạn thêm quyền tối đa của user/role.  
* **Session Policies:** Giới hạn thêm quyền của một session tạm thời.

Một hành động chỉ được phép nếu nó được cho phép bởi tất cả các lớp kiểm soát này và không bị Deny rõ ràng ở bất kỳ đâu. SCPs không ảnh hưởng trực tiếp đến cách resource-based policies được viết hay đánh giá, nhưng chúng giới hạn những gì các principals *bên trong tổ chức* có thể làm, do đó gián tiếp ảnh hưởng đến việc ai có thể tận dụng các quyền được cấp bởi resource-based policies.

Service Control Policies (SCPs) có thể được ví như những "luật chơi" hoặc "hiến pháp" của một tổ chức AWS. Chúng thiết lập các quy tắc và giới hạn cơ bản, không thể phá vỡ ở cấp độ tổ chức hoặc các đơn vị tổ chức (OUs), đảm bảo rằng tất cả các tài khoản thành viên, ngay cả những tài khoản có quyền root, cũng không thể hoạt động vượt ra ngoài các khuôn khổ an ninh và tuân thủ đã được quản trị viên trung tâm thiết lập.8  
Trong một tổ chức lớn với nhiều tài khoản AWS, việc duy trì sự nhất quán về bảo mật và tuân thủ là một thách thức lớn.56 SCPs được thiết kế để giải quyết chính xác vấn đề này. Chúng hoạt động như những "hàng rào bảo vệ" (guardrails) cấp cao, định nghĩa những hành động hoặc dịch vụ nào "không được phép" sử dụng trong các tài khoản thành viên.8 Điểm mấu chốt là SCPs có hiệu lực đối với cả người dùng root của tài khoản thành viên.8 Điều này ngăn chặn tình huống một quản trị viên của tài khoản thành viên, dù có quyền cao nhất trong tài khoản của mình, cũng không thể "vượt rào" các chính sách chung của tổ chức.  
Ví dụ, một tổ chức có thể sử dụng SCP để:

* Cấm sử dụng các AWS Regions không được phê duyệt.  
* Ngăn chặn việc tắt các dịch vụ bảo mật thiết yếu như AWS CloudTrail hoặc Amazon GuardDuty.  
* Chỉ cho phép khởi chạy các loại Amazon EC2 instance đã được phê duyệt để kiểm soát chi phí và tuân thủ.  
* Từ chối quyền truy cập vào các dịch vụ AWS không cần thiết cho mục đích kinh doanh của một OU cụ thể. Điều quan trọng cần nhớ là SCPs không tự mình cấp quyền; chúng chỉ giới hạn những quyền có thể được cấp. Quyền truy cập thực tế vẫn phải được định nghĩa thông qua IAM policies (identity-based hoặc resource-based) bên trong mỗi tài khoản thành viên. Tuy nhiên, những quyền này không bao giờ có thể vượt quá những gì được cho phép bởi SCPs áp dụng. Do đó, SCPs là công cụ tối thượng để các quản trị viên trung tâm thực thi các chính sách bảo mật và quản trị vĩ mô trên toàn bộ cơ sở hạ tầng AWS của họ.

### **F. Policy Variables (Biến Policy)**

Policy Variables là một tính năng cực kỳ hữu ích trong AWS IAM, cho phép bạn tạo ra các chính sách (policies) linh hoạt và có khảibility mở rộng hơn bằng cách sử dụng các placeholder (ký tự giữ chỗ) thay vì các giá trị cố định. Khi một chính sách chứa biến được đánh giá trong quá trình xử lý một yêu cầu (request), các biến này sẽ được thay thế bằng các giá trị thực tế được lấy từ ngữ cảnh của yêu cầu đó.28

**Cách sử dụng Policy Variables:**

Các biến trong IAM policy được đánh dấu bằng tiền tố đô la $ theo sau là một cặp dấu ngoặc nhọn { } bao quanh tên của biến. Ví dụ: ${aws:username} là một biến đại diện cho tên của IAM user đang thực hiện yêu cầu.

Để sử dụng policy variables, có một yêu cầu quan trọng: phần tử Version trong policy statement phải được khai báo và đặt giá trị là "2012-10-17" hoặc một phiên bản mới hơn hỗ trợ biến. Nếu không, các chuỗi có dạng ${...} sẽ được coi là các chuỗi ký tự thông thường thay vì biến.30

**Các loại biến có sẵn** 30**:**

Giá trị của các policy variables được lấy từ các *condition context keys* có sẵn trong ngữ cảnh của yêu cầu. Có rất nhiều context keys, bao gồm các key toàn cục (global condition keys) và các key dành riêng cho từng dịch vụ (service-specific condition keys). Một số loại biến phổ biến bao gồm:

* **Thông tin về Principal (Người yêu cầu):**  
  * ${aws:username}: Tên của IAM user (ví dụ: Bob). Biến này không có giá trị nếu yêu cầu được thực hiện bởi root user, federated user, hoặc một assumed role.  
  * ${aws:userid}: Mã định danh duy nhất của principal (ví dụ: AROAEXAMPLEID:Bob cho một assumed role session, hoặc AIDAEXAMPLEID cho một IAM user).  
  * ${aws:PrincipalType}: Loại principal (ví dụ: User, Role, Account).  
  * ${aws:PrincipalArn}: ARN của principal.  
* **Thông tin về Tags:** Bạn có thể sử dụng các tag được gắn vào IAM principals (users hoặc roles) hoặc các tag được gắn vào tài nguyên AWS làm biến.  
  * ${aws:PrincipalTag/tag-key}: Lấy giá trị của tag có key là tag-key được gắn vào principal đang thực hiện yêu cầu. Ví dụ: ${aws:PrincipalTag/department} sẽ trả về giá trị của tag "department" của user/role đó.  
  * ${aws:ResourceTag/tag-key}: Lấy giá trị của tag có key là tag-key được gắn vào tài nguyên đang được truy cập.  
* **Thông tin về Request:**  
  * ${aws:CurrentTime}: Thời gian hiện tại (ngày và giờ) khi yêu cầu được nhận, ở định dạng ISO 8601\.  
  * ${aws:SourceIp}: Địa chỉ IP nguồn của người yêu cầu.  
  * ${aws:RequestedRegion}: AWS Region mà yêu cầu được gửi đến.  
* **Ký tự đặc biệt:** Đôi khi bạn cần sử dụng các ký tự như \*, ?, $ trong giá trị của resource ARN hoặc condition value mà không muốn chúng bị hiểu nhầm là wildcard hoặc biến. IAM cung cấp các biến định sẵn cho mục đích này:  
  * ${\*}\_TAG\_KEY\_TAG\_VALUE\_\*: Đại diện cho ký tự \*.  
  * ${?}: Đại diện cho ký tự ?.  
  * ${$}: Đại diện cho ký tự $.  
* **Giá trị mặc định (Default Values):** Bạn có thể chỉ định một giá trị mặc định cho một biến nếu context key tương ứng không tồn tại hoặc không có giá trị trong request. Cú pháp là ${variableName, 'defaultValue'}. Ví dụ: ${aws:PrincipalTag/team, 'company-wide'} sẽ sử dụng giá trị của tag "team" nếu có, nếu không sẽ sử dụng "company-wide".

**Vị trí sử dụng Policy Variables trong policy** 30**:**

Policy variables có thể được sử dụng ở hai vị trí chính trong một IAM policy statement:

1. **Phần tử Resource**: Bạn có thể sử dụng biến trong phần tài nguyên của một ARN, tức là phần chuỗi xuất hiện sau dấu hai chấm thứ năm (:). Bạn **không thể** sử dụng biến để thay thế các phần trước đó của ARN như service, region, hoặc account ID.  
   * Ví dụ: "Resource": "arn:aws:s3:::my-company-bucket/${aws:PrincipalTag/department}/${aws:username}/\*" Điều này cho phép user chỉ truy cập vào một đường dẫn cụ thể trong S3 bucket dựa trên tag "department" và username của họ.  
2. **Phần tử Condition (trong phần value của condition):** Bạn có thể sử dụng biến làm giá trị để so sánh trong các condition blocks, đặc biệt với các condition operators xử lý chuỗi (như StringEquals, StringLike) hoặc ARN (như ArnEquals, ArnLike).  
   * Ví dụ:  
     JSON  
     "Condition": {  
       "StringEquals": {  
         "s3:x-amz-metadata-owner": "${aws:username}"  
       }  
     }

Điều này yêu cầu metadata "owner" của đối tượng S3 phải khớp với username của người yêu cầu.

**Ví dụ về Policy Variables** 30**:**

* **Giới hạn quyền truy cập S3 bucket dựa trên user tag:**  
  JSON  
  {  
    "Version": "2012-10-17",  
    "Statement":,  
        "Resource": \["arn:aws:s3:::example-bucket"\],  
        "Condition": {"StringLike": {"s3:prefix":}}  
      },  
      {  
        "Effect": "Allow",  
        "Action": \["s3:GetObject", "s3:PutObject"\],  
        "Resource":  
      }  
    \]  
  }

  Chính sách này cho phép người dùng liệt kê các đối tượng trong example-bucket nếu prefix khớp với tag team của họ, và cho phép họ đọc/ghi các đối tượng trong "thư mục" của team họ.  
* **Yêu cầu một tag cụ thể khi tạo IAM user:**  
  JSON  
  {  
      "Version": "2012-10-17",  
      "Statement":  
                  }  
              }  
          }  
      \]  
  }

  Chính sách này chỉ cho phép tạo IAM user nếu request đó bao gồm tag department với giá trị là IT, và chỉ có tag department được phép trong request tạo user.

Policy Variables là một công cụ cực kỳ mạnh mẽ, mang lại khả năng tạo ra các chính sách IAM động, có thể tái sử dụng và dễ dàng mở rộng. Hãy xem xét một kịch bản phổ biến: cấp quyền cho mỗi người dùng truy cập vào "thư mục nhà" (home directory) của riêng họ trên một S3 bucket chung. Nếu không có policy variables, bạn sẽ phải tạo một IAM policy riêng cho từng người dùng, chỉ định rõ ràng đường dẫn S3 của họ. Điều này sẽ trở nên không thể quản lý nổi khi số lượng người dùng tăng lên.  
Với policy variables như ${aws:username} hoặc ${aws:PrincipalTag/home\_folder}, bạn có thể tạo ra một policy duy nhất.30 Policy này có thể được đính kèm vào một IAM Group. Khi một người dùng trong Group đó thực hiện một yêu cầu (ví dụ, cố gắng tải một tệp từ S3), biến ${aws:username} trong policy sẽ tự động được thay thế bằng tên người dùng thực tế của họ, hoặc ${aws:PrincipalTag/home\_folder} sẽ được thay thế bằng giá trị của tag home\_folder được gắn cho người dùng đó. Kết quả là, policy sẽ chỉ cho phép người dùng đó truy cập vào đúng thư mục của họ.  
Lợi ích ở đây là rất lớn:

1. **Giảm đáng kể số lượng policies cần quản lý:** Thay vì hàng trăm policy riêng lẻ, bạn chỉ cần một policy chung.  
2. **Đơn giản hóa việc tuân thủ nguyên tắc đặc quyền tối thiểu:** Mỗi người dùng vẫn chỉ có quyền truy cập vào những gì họ cần.  
3. **Làm cho hệ thống IAM linh hoạt hơn với những thay đổi:** Khi có một người dùng mới, bạn chỉ cần thêm họ vào Group và (nếu cần) gắn cho họ các tag phù hợp. Họ sẽ tự động nhận được các quyền truy cập chính xác mà không cần phải tạo hay sửa đổi policy. Đây chính là một bước tiến quan trọng hướng tới mô hình Attribute-Based Access Control (ABAC), nơi các thuộc tính (như tags của principal, context keys của request) đóng vai trò trung tâm trong việc xác định quyền truy cập, thay vì chỉ dựa vào các vai trò (roles) cố định. Policy Variables là một trong những công cụ chính để hiện thực hóa ABAC trong AWS IAM.

## **Phần IV: Quản lý Danh tính Hiện đại và Truy cập Liên tài khoản**

Trong các môi trường AWS hiện đại, đặc biệt là ở quy mô doanh nghiệp, việc quản lý danh tính và quyền truy cập không chỉ dừng lại ở việc tạo IAM users và groups. Các yêu cầu về tích hợp với hệ thống danh tính hiện có, quản lý truy cập tập trung cho nhiều tài khoản, và áp dụng các mô hình kiểm soát truy cập linh hoạt hơn đã thúc đẩy sự phát triển của các giải pháp như Identity Federation, AWS IAM Identity Center, Attribute-Based Access Control (ABAC), và các chiến lược truy cập liên tài khoản phức tạp.

### **A. Identity Federation (Liên kết Danh tính)**

Identity Federation là một cơ chế cho phép người dùng đã được xác thực trong một hệ thống quản lý danh tính bên ngoài (gọi là Identity Provider \- IdP) có thể truy cập vào các tài nguyên AWS mà không cần phải tạo và quản lý IAM users riêng biệt cho họ trong mỗi tài khoản AWS. Thay vào đó, sau khi được IdP xác thực, người dùng sẽ được phép *đảm nhận* một IAM Role trong AWS, Role này sẽ cấp cho họ các thông tin xác thực tạm thời và các quyền hạn cần thiết để tương tác với các dịch vụ AWS.3

Có hai giao thức liên kết danh tính chính được AWS hỗ trợ:

**1\. SAML 2.0 Federation (Security Assertion Markup Language 2.0)** 22**:**

SAML 2.0 là một tiêu chuẩn mở dựa trên XML, thường được sử dụng để cho phép single sign-on (SSO) giữa các hệ thống doanh nghiệp (ví dụ: Microsoft Active Directory Federation Services \- ADFS, Okta, Azure AD, Ping Identity) và các nhà cung cấp dịch vụ đám mây như AWS.

* **Luồng xác thực SAML 2.0 với AWS:**  
  1. **Khởi tạo:** Người dùng trong tổ chức cố gắng truy cập một ứng dụng hoặc tài nguyên AWS (ví dụ: AWS Management Console).  
  2. **Chuyển hướng đến IdP:** Ứng dụng hoặc AWS sẽ chuyển hướng trình duyệt của người dùng đến trang đăng nhập của IdP doanh nghiệp.  
  3. **Xác thực tại IdP:** Người dùng nhập thông tin xác thực của họ (ví dụ: tên người dùng và mật khẩu Active Directory). IdP xác thực người dùng dựa trên kho lưu trữ danh tính của tổ chức.  
  4. **Tạo SAML Assertion:** Nếu xác thực thành công, IdP sẽ tạo ra một **SAML assertion**. Đây là một tài liệu XML được ký điện tử, chứa thông tin về người dùng đã được xác thực (ví dụ: tên người dùng, địa chỉ email) và các thuộc tính khác (ví dụ: tư cách thành viên trong các nhóm, vai trò trong tổ chức). Nếu SAML encryption được kích hoạt, assertion này sẽ được IdP mã hóa.  
  5. **Gửi Assertion đến Trình duyệt:** IdP gửi SAML assertion (thường thông qua một HTTP POST request) trở lại trình duyệt của người dùng.  
  6. **Gọi API AssumeRoleWithSAML của AWS STS:** Trình duyệt của người dùng (hoặc một ứng dụng trung gian) sau đó gửi SAML assertion này đến AWS Security Token Service (STS) bằng cách gọi API AssumeRoleWithSAML. Yêu cầu này cũng bao gồm ARN của SAML provider đã được cấu hình trong IAM (đại diện cho IdP) và ARN của IAM Role mà người dùng muốn đảm nhận.  
  7. **(Tùy chọn) Giải mã Assertion:** Nếu assertion được mã hóa, AWS STS sẽ sử dụng khóa riêng tư (đã được tải lên từ IdP) để giải mã nó.  
  8. **Xác thực Assertion và Cấp Credentials Tạm thời:** STS xác thực chữ ký và các thông tin khác trong SAML assertion (ví dụ: issuer, audience, thời gian hiệu lực). Nếu assertion hợp lệ và người dùng được phép đảm nhận Role được yêu cầu (dựa trên trust policy của Role), STS sẽ trả về một bộ thông tin xác thực AWS tạm thời (access key ID, secret access key, và session token).  
  9. **Truy cập AWS:** Người dùng (hoặc ứng dụng) giờ đây có thể sử dụng các thông tin xác thực tạm thời này để truy cập vào các tài nguyên AWS được phép bởi IAM Role đã đảm nhận.  
* **Thiết lập Trust giữa IdP và AWS** 22**:**  
  * **Tại IdP:** Đăng ký AWS như một Service Provider (SP) hoặc Relying Party. Điều này thường liên quan đến việc nhập SAML metadata document của AWS (cung cấp bởi AWS, chứa thông tin về entity ID và các endpoint của AWS).  
  * **Tại AWS IAM:** Tạo một **SAML Identity Provider** trong IAM. Bạn sẽ tải lên SAML metadata document do IdP của bạn cung cấp. Tài liệu này mô tả IdP của bạn cho AWS, bao gồm issuer name, ngày hết hạn, và các khóa công khai mà AWS sẽ sử dụng để xác minh chữ ký của các SAML assertions đến từ IdP đó. Nếu sử dụng mã hóa assertion, bạn cũng cần tải lên khóa giải mã riêng tư.  
  * **Tạo IAM Roles với Trust Policies:** Tạo một hoặc nhiều IAM Roles mà người dùng liên kết sẽ đảm nhận. Điểm mấu chốt là **trust policy** của các Roles này. Trong trust policy, bạn phải chỉ định ARN của SAML Identity Provider đã tạo ở bước trên làm Principal, và cho phép hành động sts:AssumeRoleWithSAML. Bạn cũng có thể thêm các Condition elements để giới hạn thêm ai có thể đảm nhận Role dựa trên các thuộc tính trong SAML assertion (ví dụ: saml:aud \- audience, saml:iss \- issuer).  
* **Sử dụng SAML Assertions** 22**:** SAML assertions là trung tâm của quá trình liên kết. Chúng chứa các thông tin quan trọng như:  
  * Issuer: Xác định IdP đã phát hành assertion.  
  * Subject: Thông tin về người dùng được xác thực.  
  * Conditions: Thời gian hiệu lực của assertion, và AudienceRestriction (chỉ định rằng assertion dành cho AWS).  
  * AttributeStatement: Chứa các thuộc tính của người dùng (ví dụ: department, project, groups). Các thuộc tính này rất quan trọng cho việc ánh xạ vai trò và Attribute-Based Access Control (ABAC).  
  * Signature: Chữ ký điện tử của IdP.  
* **Ánh xạ thuộc tính cho Role Assumption và ABAC** 22**:** IdP được cấu hình để gửi các thuộc tính cụ thể trong SAML assertion. AWS có thể sử dụng các thuộc tính này để:  
  * **Quyết định Role nào người dùng có thể đảm nhận:** Ví dụ, nếu assertion chứa thuộc tính Groups với giá trị Developers, người dùng có thể được ánh xạ để đảm nhận DeveloperRole trong AWS.  
  * **Thực thi ABAC:** Các thuộc tính SAML (ví dụ: saml:department, saml:costCenter) có thể được sử dụng trong các Condition elements của IAM policies để cấp quyền truy cập chi tiết dựa trên thuộc tính của người dùng.

**2\. OpenID Connect (OIDC) Federation** 23**:**

OIDC là một lớp nhận dạng đơn giản được xây dựng trên nền tảng giao thức OAuth 2.0. Nó thường được sử dụng để cho phép các ứng dụng web và di động xác thực người dùng thông qua các nhà cung cấp danh tính web công cộng (như Google, Facebook, Amazon Cognito) hoặc các OIDC provider doanh nghiệp khác.

* **Luồng xác thực OIDC với AWS (thường cho Web Identity Providers):**  
  1. **Xác thực với OIDC Provider:** Ứng dụng của bạn (web hoặc mobile) chuyển hướng người dùng đến trang đăng nhập của OIDC provider (ví dụ: Google).  
  2. **Cấp OIDC Token (JWT):** Sau khi người dùng xác thực thành công, OIDC provider sẽ cấp cho ứng dụng một **OIDC token**, thường là một JSON Web Token (JWT). JWT này chứa các *claims* (thông tin) về người dùng và quá trình xác thực.  
  3. **Gọi API AssumeRoleWithWebIdentity của AWS STS:** Ứng dụng của bạn sau đó gọi API AssumeRoleWithWebIdentity của STS, truyền vào OIDC token (JWT) nhận được từ OIDC provider và ARN của IAM Role mà ứng dụng muốn người dùng đảm nhận.  
  4. **Xác thực Token và Cấp Credentials Tạm thời:** STS sẽ:  
     * Xác thực chữ ký của JWT bằng cách sử dụng các khóa công khai

#### **Nguồn trích dẫn**

1. AWS Security | How to Create IAM User, Groups, Role, and Policy ..., truy cập vào tháng 5 14, 2025, [https://dev.to/s3cloudhub/aws-security-how-to-create-iam-user-groups-role-and-policy-3pak](https://dev.to/s3cloudhub/aws-security-how-to-create-iam-user-groups-role-and-policy-3pak)  
2. Inside AWS IAM: A Deep Dive into Identity and Access Management \- Cloudchipr, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-iam](https://cloudchipr.com/blog/aws-iam)  
3. AWS IAM Roles and Policies: Understanding Cloud Security \- Delinea, truy cập vào tháng 5 14, 2025, [https://delinea.com/blog/aws-iam-roles-and-policies-explained](https://delinea.com/blog/aws-iam-roles-and-policies-explained)  
4. The Evolution of AWS IAM: From IAM Users to Identity Center, truy cập vào tháng 5 14, 2025, [https://axiom.security/the-evolution-of-aws-iam/](https://axiom.security/the-evolution-of-aws-iam/)  
5. Document history for IAM \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/document-history.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/document-history.html)  
6. What is IAM Identity Center? \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)  
7. When and where to use IAM permissions boundaries | AWS Security ..., truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/](https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/)  
8. Service control policies (SCPs) \- AWS Organizations, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/organizations/latest/userguide/orgs\_manage\_policies\_scps.html](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)  
9. How to create true attribute based access controls with IAM | AWS re ..., truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUJG6UhPsNTE21TqxFKnml5Q/how-to-create-true-attribute-based-access-controls-with-iam](https://repost.aws/questions/QUJG6UhPsNTE21TqxFKnml5Q/how-to-create-true-attribute-based-access-controls-with-iam)  
10. CloudTrail concepts \- AWS CloudTrail \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html)  
11. Use cases for IAM users \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/gs-identities-iam-users.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/gs-identities-iam-users.html)  
12. AWS security credentials \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html)  
13. AWS Identity and Access Management (IAM) Best Practices \- Amazon Web Services, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/resources/best-practices/](https://aws.amazon.com/iam/resources/best-practices/)  
14. UploadSigningCertificate \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/APIReference/API\_UploadSigningCertificate.html](https://docs.aws.amazon.com/IAM/latest/APIReference/API_UploadSigningCertificate.html)  
15. IAM Roles and Groups. : r/aws \- Reddit, truy cập vào tháng 5 14, 2025, [https://www.reddit.com/r/aws/comments/1brgs6c/iam\_roles\_and\_groups/](https://www.reddit.com/r/aws/comments/1brgs6c/iam_roles_and_groups/)  
16. IAM roles \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)  
17. AssumeRole \- AWS Security Token Service \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)  
18. Policy evaluation logic \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_evaluation-logic.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)  
19. Defining Lambda function permissions with an execution role \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)  
20. Create a role to give permissions to an IAM user \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create\_for-user.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)  
21. IAM tutorial: Delegate access across AWS accounts using IAM roles ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial\_cross-account-with-roles.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)  
22. SAML 2.0 federation \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_providers\_saml.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)  
23. OIDC federation \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_providers\_oidc.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)  
24. Create a role to delegate permissions to an AWS service \- AWS ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create\_for-service.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)  
25. Understanding service-linked roles \- Amazon DocumentDB, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/documentdb/latest/developerguide/service-linked-roles.html](https://docs.aws.amazon.com/documentdb/latest/developerguide/service-linked-roles.html)  
26. CreateServiceLinkedRole \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/APIReference/API\_CreateServiceLinkedRole.html](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateServiceLinkedRole.html)  
27. Create a service-linked role \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create-service-linked-role.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create-service-linked-role.html)  
28. aws\_iam\_policy\_document | Data Sources | hashicorp/aws \- Terraform Registry, truy cập vào tháng 5 14, 2025, [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam\_policy\_document](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document)  
29. IAM JSON policy element reference \- AWS Identity and Access ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_elements.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)  
30. IAM policy elements: Variables and tags \- AWS Identity and Access ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_variables.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)  
31. Choose between managed policies and inline policies \- AWS ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies-choosing-managed-or-inline.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies-choosing-managed-or-inline.html)  
32. Identity-based IAM policies for Lambda \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html](https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html)  
33. Resource-based policy examples for AWS KMS \- AWS Database ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/dms/latest/userguide/security\_iam\_resource-based-policy-examples.html](https://docs.aws.amazon.com/dms/latest/userguide/security_iam_resource-based-policy-examples.html)  
34. Identity-based policies for Amazon S3 \- Amazon Simple Storage Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/security\_iam\_id-based-policy-examples.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security_iam_id-based-policy-examples.html)  
35. Key policies in AWS KMS \- AWS Key Management Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)  
36. AWS Key Management Service \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/kms/latest/developerguide/overview.html](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)  
37. Viewing resource-based IAM policies in Lambda \- AWS Lambda, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)  
38. What is Amazon S3? \- Amazon Simple Storage Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)  
39. How Amazon S3 works with IAM \- Amazon Simple Storage Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/security\_iam\_service-with-iam.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security_iam_service-with-iam.html)  
40. Identity and Access Management for Amazon S3 \- Amazon Simple ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html)  
41. Policies and permissions in AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)  
42. Troubleshoot IAM policies \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot\_policies.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_policies.html)  
43. Test IAM policies and permissions with the IAM policy simulator ..., truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/iam-policy-simulator](https://repost.aws/knowledge-center/iam-policy-simulator)  
44. IAM policy testing with the IAM policy simulator \- AWS Identity and ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies\_testing-policies.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)  
45. IAM \- Multi-Factor Authentication \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/features/mfa/](https://aws.amazon.com/iam/features/mfa/)  
46. Root user best practices for your AWS account \- AWS Identity and ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)  
47. 13 Essential AWS IAM Best Practices \- Wiz, truy cập vào tháng 5 14, 2025, [https://www.wiz.io/academy/aws-iam-best-practices](https://www.wiz.io/academy/aws-iam-best-practices)  
48. GetFederationToken \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_GetFederationToken.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)  
49. GetSessionToken \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_GetSessionToken.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetSessionToken.html)  
50. AssumeRoleWithSAML \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRoleWithSAML.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html)  
51. OIDC federation \- AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/en\_kr/IAM/latest/UserGuide/id\_roles\_providers\_oidc.html](https://docs.aws.amazon.com/en_kr/IAM/latest/UserGuide/id_roles_providers_oidc.html)  
52. AssumeRoleWithWebIdentity \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRoleWithWebIdentity.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html)  
53. Resolve the AWS STS "PackedPolicyTooLarge" IAM assume role error | AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/iam-role-aws-sts-error](https://repost.aws/knowledge-center/iam-role-aws-sts-error)  
54. get-session-token — AWS CLI 1.40.13 Command Reference, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html](https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html)  
55. AWS Identity: Permission boundaries & delegation \- awsstatic.com, truy cập vào tháng 5 14, 2025, [https://d1.awsstatic.com/events/reinvent/2019/REPEAT\_1\_AWS\_identity\_Permission\_boundaries\_&\_delegation\_SEC402-R1.pdf](https://d1.awsstatic.com/events/reinvent/2019/REPEAT_1_AWS_identity_Permission_boundaries_&_delegation_SEC402-R1.pdf)  
56. What is AWS Organizations? \- AWS Organizations \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/organizations/latest/userguide/orgs\_introduction.html](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html)  
57. Set up SAML 2.0 for WorkSpaces Personal \- AWS Documentation \- Amazon.com, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html](https://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html)