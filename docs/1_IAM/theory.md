# Lớp học Chuyên sâu về AWS IAM: Từ Nền tảng đến Chuyên gia

Chào mừng bạn đến với lớp học chuyên sâu về AWS Identity and Access Management (IAM). Tôi là một Kiến trúc sư Giải pháp Cấp cao tại AWS, chuyên sâu về Bảo mật Đám mây và đặc biệt là IAM. Trong nhiều năm làm việc với các khách hàng lớn nhất và phức tạp nhất của AWS, cũng như tham gia vào việc thiết kế một số khía cạnh của chính IAM, tôi nhận thấy rằng việc nắm vững IAM không chỉ là một kỹ năng hữu ích – đó là nền tảng tuyệt đối cho mọi hoạt động an toàn, hiệu quả và có khả năng mở rộng trên AWS.

Lớp học này được thiết kế để đưa bạn từ những hiểu biết cơ bản nhất đến mức độ tinh thông mà chúng tôi mong đợi ở các chuyên gia nội bộ của mình. Chúng ta sẽ không chỉ xem xét *cái gì*, mà quan trọng hơn là *tại sao* và *như thế nào*. Hãy bắt đầu.

## 1. Giới thiệu: Nền tảng Bảo mật của AWS

### Tại sao IAM được cho là dịch vụ AWS quan trọng NHẤT?

Mọi tương tác với AWS, dù là thông qua Bảng điều khiển Quản lý (Management Console), Giao diện Dòng lệnh (CLI), Bộ phát triển Phần mềm (SDK) hay trực tiếp qua API, đều phải đi qua cửa kiểm soát của IAM. IAM xác định **ai** (hoặc **cái gì**) có thể làm **gì** đối với **tài nguyên nào** trong môi trường AWS của bạn.

Một cấu hình IAM yếu kém hoặc bị hiểu sai có thể dẫn đến:
*   Truy cập trái phép vào dữ liệu nhạy cảm.
*   Gián đoạn dịch vụ do thay đổi cấu hình không mong muốn.
*   Leo thang đặc quyền, cho phép kẻ tấn công giành quyền kiểm soát hoàn toàn môi trường của bạn.
*   Chi phí không kiểm soát do việc tạo tài nguyên không được phép.

Ngược lại, một chiến lược IAM vững chắc, được triển khai tốt là lớp phòng thủ đầu tiên và quan trọng nhất của bạn. Nó cho phép bạn đổi mới một cách an toàn, ủy quyền trách nhiệm một cách có kiểm soát và đáp ứng các yêu cầu tuân thủ nghiêm ngặt. Nếu bạn không làm đúng IAM, thì việc bạn làm đúng mọi thứ khác về bảo mật cũng trở nên vô nghĩa.

### Phạm vi kiểm soát của IAM (Xác thực vs. Ủy quyền)

IAM thực hiện hai chức năng cốt lõi:

1.  **Xác thực (Authentication):** Xác minh danh tính của thực thể (người dùng, ứng dụng, dịch vụ) đang cố gắng truy cập AWS. Ai là bạn? Làm thế nào bạn chứng minh điều đó? Điều này liên quan đến việc kiểm tra thông tin xác thực như mật khẩu, khóa truy cập, hoặc thông qua các cơ chế liên kết định danh.
2.  **Ủy quyền (Authorization):** Sau khi danh tính được xác thực, IAM xác định những hành động cụ thể nào mà danh tính đó được phép thực hiện trên các tài nguyên AWS cụ thể. Bạn được phép làm gì? Điều này được thực hiện thông qua việc đánh giá các **Chính sách (Policies)** được đính kèm.

Hiểu rõ sự phân biệt này là rất quan trọng. Bạn có thể được xác thực thành công nhưng vẫn bị từ chối thực hiện một hành động nếu bạn không được ủy quyền.

### Mục tiêu cuối cùng: Nguyên tắc Đặc quyền Tối thiểu (Least Privilege)

Đây là nguyên tắc vàng của IAM và bảo mật nói chung. Nguyên tắc Đặc quyền Tối thiểu quy định rằng một danh tính chỉ nên được cấp **chính xác những quyền hạn cần thiết** để thực hiện các nhiệm vụ được giao và **không hơn thế nữa**.

Việc áp dụng Đặc quyền Tối thiểu giảm thiểu bề mặt tấn công. Nếu một danh tính bị xâm phạm, thiệt hại tiềm ẩn sẽ bị giới hạn trong phạm vi quyền hạn hạn chế của nó. Việc đạt được và duy trì Đặc quyền Tối thiểu là một quá trình liên tục, đòi hỏi sự hiểu biết sâu sắc về các yêu cầu ứng dụng và công việc, cũng như các công cụ và kỹ thuật IAM.

### Tổng quan về cấu trúc lớp học chuyên sâu

Chúng ta sẽ đi qua các chủ đề sau:
*   **Nguyên tắc Cơ bản:** Các khối xây dựng của IAM - Principals, Credentials, Policies.
*   **Logic Đánh giá Chính sách:** Trái tim của IAM - cách AWS đưa ra quyết định Có/Không.
*   **Khái niệm Nâng cao:** Federation, ABAC, Truy cập Liên tài khoản, và nhiều hơn nữa.
*   **Thực tiễn Tốt nhất:** Các khuyến nghị đã được chứng minh theo Khuôn khổ Well-Architected.
*   **Vận hành IAM:** Kiểm toán, xử lý sự cố và góc nhìn về sự phát triển của dịch vụ.

Hãy bắt đầu với các thành phần nền tảng.

## 2. Nguyên tắc Cơ bản của IAM: Các Thành phần Nền tảng

Để hiểu IAM, trước tiên bạn cần nắm vững các thành phần cốt lõi của nó.

### Đối tượng định danh (Principals): Ai hoặc cái gì có thể thực hiện yêu cầu?

**Principal** là một thực thể trong AWS có thể thực hiện các hành động trên tài nguyên AWS. Có nhiều loại Principal khác nhau:

*   **Người dùng Gốc (Root User):**
    *   **Mục đích:** Đây là danh tính được tạo ra khi bạn lần đầu tiên tạo một tài khoản AWS. Nó có **quyền truy cập không hạn chế** vào tất cả các tài nguyên và dịch vụ trong tài khoản đó, bao gồm cả thông tin thanh toán và khả năng đóng tài khoản.
    *   **Lịch sử/Sự phát triển:** Ban đầu, Root user là cách duy nhất để tương tác với tài khoản. Tuy nhiên, việc sử dụng nó cho các hoạt động hàng ngày là cực kỳ rủi ro. Nếu thông tin xác thực Root bị xâm phạm, toàn bộ tài khoản của bạn sẽ gặp nguy hiểm.
    *   **Tại sao KHÔNG BAO GIỜ sử dụng hàng ngày:** Do quyền hạn tuyệt đối của nó, Root user chỉ nên được sử dụng cho một số ít nhiệm vụ quản lý tài khoản và dịch vụ cụ thể mà không thể thực hiện bởi các principal khác (ví dụ: thay đổi gói hỗ trợ, đóng tài khoản, cấu hình một số cài đặt bảo mật ban đầu).
    *   **Bảo mật:** *Luôn* bật Xác thực Đa yếu tố (MFA) cho Root user, sử dụng mật khẩu cực mạnh và *không bao giờ* tạo khóa truy cập (access keys) cho nó. Cất giữ thông tin xác thực Root một cách an toàn tuyệt đối.

*   **Người dùng IAM (IAM Users):**
    *   **Mục đích:** Đại diện cho một người hoặc một ứng dụng cần tương tác với AWS. Mỗi IAM User có một tên duy nhất trong tài khoản và một bộ thông tin xác thực dài hạn (mật khẩu cho truy cập Console, khóa truy cập cho truy cập chương trình).
    *   **Thông tin xác thực:** Mật khẩu, Khóa truy cập (Access Key ID & Secret Access Key).
    *   **Trường hợp sử dụng:** Theo truyền thống, được sử dụng cho quản trị viên, nhà phát triển, hoặc các tài khoản dịch vụ (service accounts) cho các ứng dụng chạy bên ngoài AWS. Tuy nhiên, việc sử dụng IAM Users cho con người đang ngày càng *không được khuyến khích* và nên được thay thế bằng liên kết định danh thông qua IAM Identity Center. Việc sử dụng chúng cho các ứng dụng cũng nên được xem xét cẩn thận so với việc sử dụng Roles.
    *   **Hạn chế:** Thông tin xác thực dài hạn là một rủi ro bảo mật nếu không được quản lý và xoay vòng đúng cách. Quản lý quyền hạn cho nhiều người dùng có thể trở nên phức tạp.

*   **Nhóm IAM (IAM Groups):**
    *   **Mục đích:** Một tập hợp các IAM Users. Bạn không thể đăng nhập với tư cách là một Group; thay vào đó, bạn gán các chính sách quyền hạn cho Group, và tất cả các Users trong Group đó sẽ kế thừa các quyền hạn đó.
    *   **Đơn giản hóa quản lý:** Groups là một cách tuyệt vời để quản lý quyền hạn cho nhiều Users có cùng vai trò công việc hoặc yêu cầu truy cập. Thay vì gán chính sách cho từng User riêng lẻ, bạn gán chúng cho Group. Thêm hoặc xóa User khỏi Group sẽ tự động cấp hoặc thu hồi quyền hạn tương ứng.
    *   **Lưu ý:** Một User có thể thuộc nhiều Groups. Groups không thể chứa các Groups khác. Roles không thể là thành viên của Groups.

*   **Vai trò IAM (IAM Roles):**
    *   **Mục đích:** Đây là loại principal **linh hoạt và được khuyến nghị nhất** trong nhiều trường hợp. Một Role là một danh tính IAM mà bạn có thể tạo trong tài khoản của mình có các quyền hạn cụ thể. Nó tương tự như một IAM User ở chỗ nó là một danh tính AWS với các chính sách quyền hạn xác định những gì danh tính có thể và không thể làm trong AWS. Tuy nhiên, thay vì được liên kết duy nhất với một người hoặc ứng dụng, Role được **đảm nhận (assumed)** bởi các thực thể đáng tin cậy.
    *   **Cơ chế Đảm nhận Vai trò (Assumption):** Khi một thực thể (IAM User, ứng dụng, dịch vụ AWS, người dùng được liên kết) đảm nhận một Role, AWS Security Token Service (STS) sẽ cấp cho thực thể đó các **thông tin xác thực bảo mật tạm thời** (temporary security credentials) có hiệu lực trong một khoảng thời gian giới hạn (ví dụ: 1 giờ). Thực thể đó sau đó sử dụng các thông tin xác thực tạm thời này để thực hiện các lệnh gọi API AWS.
    *   **Chính sách Tin cậy (Trust Policy):** Phần *quan trọng* định nghĩa một Role. Chính sách này chỉ định **ai (principal nào)** được phép đảm nhận Role đó. Principal đáng tin cậy có thể là:
        *   Một dịch vụ AWS (ví dụ: `ec2.amazonaws.com`, `lambda.amazonaws.com`)
        *   Một tài khoản AWS khác (cho phép truy cập liên tài khoản)
        *   Một nhà cung cấp danh tính Web (thông qua OIDC)
        *   Một nhà cung cấp danh tính SAML 2.0
        *   IAM Users hoặc Roles trong cùng tài khoản hoặc tài khoản khác.
    *   **Chính sách Quyền hạn (Permissions Policy):** Giống như Users và Groups, Roles sử dụng các chính sách quyền hạn (Identity-based Policies) để xác định những hành động và tài nguyên nào mà Role được phép truy cập *sau khi* nó được đảm nhận.
    *   **Trường hợp sử dụng:**
        *   **Dịch vụ AWS:** Cho phép các dịch vụ AWS (như EC2, Lambda, ECS) tương tác với các dịch vụ AWS khác thay mặt bạn (ví dụ: một phiên bản EC2 cần đọc/ghi vào S3). Đây là cách *an toàn nhất* và được khuyến nghị.
        *   **Truy cập Liên tài khoản:** Cho phép người dùng hoặc dịch vụ trong một tài khoản AWS truy cập tài nguyên trong một tài khoản khác.
        *   **Liên kết Định danh (Federation):** Cho phép người dùng từ thư mục công ty của bạn (ví dụ: Active Directory, Okta, Azure AD thông qua SAML) hoặc từ các nhà cung cấp danh tính web (Google, Facebook thông qua OIDC) truy cập tài khoản AWS của bạn mà không cần tạo IAM Users cho họ. Họ đảm nhận một Role sau khi được xác thực bởi nhà cung cấp danh tính của họ.
        *   **Ủy quyền Tạm thời:** Cấp quyền truy cập tạm thời cho người dùng hoặc ứng dụng cho các nhiệm vụ cụ thể.
    *   **Tại sao Roles được tạo ra và ưu tiên hơn Users:** Roles giải quyết vấn đề cơ bản của việc quản lý thông tin xác thực dài hạn một cách an toàn. Trước khi có Roles, cách phổ biến (và không an toàn) để cấp quyền cho một ứng dụng trên EC2 là nhúng khóa truy cập IAM User vào cấu hình hoặc mã nguồn của ứng dụng. Điều này tạo ra rủi ro bảo mật lớn. Roles sử dụng thông tin xác thực tạm thời, tự động xoay vòng, loại bỏ nhu cầu quản lý khóa truy cập dài hạn cho các ứng dụng và dịch vụ chạy trên AWS. Chúng cũng là nền tảng cho các mẫu hình truy cập liên tài khoản và liên kết định danh an toàn. **Nguyên tắc chung: Ưu tiên sử dụng Roles bất cứ khi nào có thể, đặc biệt là cho các ứng dụng, dịch vụ AWS và truy cập của con người thông qua liên kết định danh.**

### Thông tin xác thực (Credentials): Cách các đối tượng định danh chứng minh danh tính của họ

*   **Mật khẩu (Passwords):** Dành cho IAM Users khi truy cập Bảng điều khiển Quản lý AWS. Nên phức tạp và được bảo vệ bằng MFA.
*   **Khóa truy cập (Access Keys):** Bao gồm một Access Key ID và một Secret Access Key. Dành cho IAM Users hoặc Root User (không khuyến khích mạnh mẽ cho Root) để thực hiện các lệnh gọi API chương trình (CLI, SDK, công cụ). Đây là thông tin xác thực *dài hạn*, phải được bảo mật cẩn thận và *xoay vòng thường xuyên*.
*   **Chứng chỉ Ký (Signing Certificates):** Ít phổ biến hơn, được sử dụng cho một số loại yêu cầu SOAP cũ hoặc với các dịch vụ như CloudFront Key Pairs.
*   **Thông tin xác thực Tạm thời (Temporary Credentials):** Được cấp bởi AWS STS khi một Role được đảm nhận hoặc thông qua các lệnh gọi STS khác. Bao gồm Access Key ID, Secret Access Key và một **Session Token**. Chúng có hiệu lực trong thời gian ngắn và là phương pháp xác thực ưa thích cho các ứng dụng và kịch bản liên kết.
*   **Xác thực Đa yếu tố (Multi-Factor Authentication - MFA):** Một lớp bảo mật bổ sung quan trọng. Yêu cầu người dùng cung cấp một yếu tố xác thực thứ hai (ví dụ: mã từ thiết bị ảo hoặc vật lý) ngoài mật khẩu hoặc khóa truy cập. **Phải được bật cho Root User và tất cả IAM Users có quyền hạn đáng kể.**

### Quyền hạn & Chính sách (Permissions & Policies): Xác định *những gì* đối tượng định danh có thể làm

Chính sách là trái tim của việc ủy quyền trong IAM. Chúng là các tài liệu JSON định nghĩa các quyền hạn.

*   **Cấu trúc Chính sách JSON cốt lõi:**

    ```json
    {
      "Version": "2012-10-17", // Ngày phiên bản ngôn ngữ chính sách (thường là giá trị này)
      "Statement": [ // Một danh sách các câu lệnh riêng lẻ
        {
          "Sid": "AllowS3ListRead", // (Tùy chọn) Định danh câu lệnh, hữu ích cho việc đọc hiểu
          "Effect": "Allow", // Hoặc "Deny". Quyết định chính sách cho phép hay từ chối
          "Principal": { // *Chỉ* sử dụng trong Chính sách Dựa trên Tài nguyên hoặc Chính sách Tin cậy của Role
            "AWS": "arn:aws:iam::111122223333:root" // Ai được phép hoặc bị từ chối bởi câu lệnh này
          },
          "Action": [ // Hành động API nào được phép hoặc bị từ chối
            "s3:ListBucket",
            "s3:GetObject"
          ],
          "Resource": [ // Tài nguyên AWS nào mà hành động áp dụng tới
            "arn:aws:s3:::my-example-bucket",
            "arn:aws:s3:::my-example-bucket/*"
          ],
          "Condition": { // (Tùy chọn) Các điều kiện phải đúng để chính sách có hiệu lực
            "StringEquals": {
              "aws:SourceIp": "192.0.2.0/24"
            },
            "DateLessThan": {
              "aws:CurrentTime": "2024-12-31T23:59:59Z"
            }
          }
        }
        // Có thể có nhiều câu lệnh hơn trong danh sách
      ]
    }
    ```

    **Giải thích chi tiết từng yếu tố:**
    *   `Version`: Chỉ định phiên bản ngôn ngữ chính sách. Luôn sử dụng `"2012-10-17"` (phiên bản hiện tại và duy nhất được khuyến nghị).
    *   `Statement`: Một mảng chứa một hoặc nhiều câu lệnh chính sách riêng lẻ. Mỗi câu lệnh định nghĩa một tập hợp quyền hạn.
    *   `Sid` (Statement ID): Một định danh tùy chọn nhưng được khuyến nghị cho mỗi câu lệnh. Giúp bạn dễ dàng nhận biết mục đích của câu lệnh khi đọc hoặc gỡ lỗi chính sách. Không ảnh hưởng đến logic đánh giá.
    *   `Effect`: Giá trị cốt lõi, có thể là `"Allow"` hoặc `"Deny"`. Xác định kết quả của câu lệnh nếu nó khớp với yêu cầu.
    *   `Principal`: **Chỉ định tài khoản, người dùng, vai trò hoặc dịch vụ được phép hoặc bị từ chối truy cập.** Yếu tố này **chỉ** được sử dụng trong các chính sách mà bạn đính kèm vào tài nguyên (Resource-based Policies) hoặc trong Chính sách Tin cậy (Trust Policy) của một Role. **Nó KHÔNG được sử dụng trong các chính sách đính kèm cho Users, Groups, hoặc Roles (Identity-based Policies)** vì principal trong trường hợp đó chính là danh tính mà chính sách được đính kèm. Bạn có thể chỉ định tài khoản AWS (`"AWS": "arn:aws:iam::ACCOUNT-ID:root"`), IAM Users cụ thể (`"AWS": "arn:aws:iam::ACCOUNT-ID:user/UserName"`), IAM Roles (`"AWS": "arn:aws:iam::ACCOUNT-ID:role/RoleName"`), Dịch vụ AWS (`"Service": "ec2.amazonaws.com"`), hoặc các principal liên kết. Sử dụng `"*"` cho `Principal` trong chính sách tài nguyên có nghĩa là bất kỳ ai (kể cả ẩn danh nếu không có các biện pháp kiểm soát khác) cũng có thể truy cập, điều này thường rất nguy hiểm trừ khi có chủ đích (ví dụ: public S3 bucket).
    *   `Action`: Một hoặc nhiều hành động API AWS mà câu lệnh áp dụng (ví dụ: `"s3:GetObject"`, `"ec2:StartInstances"`). Bạn có thể sử dụng ký tự đại diện `*` (ví dụ: `"ec2:*"` để cho phép tất cả các hành động EC2, hoặc `"s3:Get*"` để cho phép tất cả các hành động S3 bắt đầu bằng "Get"). **Hãy cực kỳ cẩn thận với ký tự đại diện, vì chúng dễ cấp quyền quá mức.** Luôn cố gắng chỉ định các hành động cụ thể cần thiết.
    *   `Resource`: Một hoặc nhiều tài nguyên AWS mà hành động áp dụng tới, được xác định bằng Amazon Resource Name (ARN) của chúng (ví dụ: `"arn:aws:s3:::my-bucket/*"`, `"arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0"`). Một số hành động không hoạt động trên một tài nguyên cụ thể (ví dụ: `ec2:DescribeInstances` để liệt kê các phiên bản) và trong trường hợp đó, bạn sử dụng `"*"` cho `Resource`. Giống như `Action`, hãy cẩn thận với `*` trong `Resource`.
    *   `Condition`: (Tùy chọn nhưng rất mạnh mẽ) Chỉ định các điều kiện phải được đáp ứng để chính sách có hiệu lực. Điều này cho phép kiểm soát truy cập chi tiết. Có nhiều *khóa điều kiện* (condition keys) có sẵn, một số áp dụng chung (ví dụ: `aws:CurrentTime`, `aws:SourceIp`, `aws:PrincipalTag/TagName`, `aws:RequestedRegion`, `aws:MultiFactorAuthPresent`) và một số dành riêng cho dịch vụ (ví dụ: `s3:x-amz-acl`). Các toán tử điều kiện (ví dụ: `StringEquals`, `NumericLessThan`, `DateGreaterThan`, `Bool`, `IpAddress`) được sử dụng để so sánh giá trị trong bối cảnh yêu cầu với giá trị được chỉ định trong chính sách. Conditions là nền tảng của ABAC.

*   **Các loại Chính sách:** IAM sử dụng nhiều loại chính sách khác nhau, và hiểu rõ chúng là rất quan trọng vì chúng tương tác với nhau trong quá trình đánh giá ủy quyền.

    *   **Chính sách dựa trên danh tính (Identity-based Policies):** Đính kèm trực tiếp vào một **Principal** (User, Group, hoặc Role). Chúng xác định những gì *danh tính đó* có thể làm.
        *   **Được quản lý (Managed Policies):** Các chính sách độc lập mà bạn có thể đính kèm vào nhiều Users, Groups và Roles.
            *   **AWS Managed:** Được tạo và quản lý bởi AWS. Bao gồm các chính sách cho các vai trò công việc phổ biến (ví dụ: `AdministratorAccess`, `ReadOnlyAccess`) và các chính sách cụ thể cho dịch vụ.
                *   *Ưu điểm:* Thuận tiện, được AWS cập nhật khi có dịch vụ/tính năng mới.
                *   *Nhược điểm:* Thường quá rộng rãi (ví dụ: `AdministratorAccess` cấp *mọi* quyền), không tuân thủ Đặc quyền Tối thiểu. Nên sử dụng thận trọng, đặc biệt là các chính sách quá rộng. Tốt hơn cho việc bắt đầu hoặc các vai trò rất chung chung, nhưng nên thay thế bằng chính sách tùy chỉnh khi có thể.
            *   **Customer Managed:** Bạn tạo và quản lý trong tài khoản của mình.
                *   *Ưu điểm:* Cho phép bạn tạo các chính sách tái sử dụng, phù hợp chính xác với yêu cầu Đặc quyền Tối thiểu của bạn. Quản lý tập trung (thay đổi một lần, áp dụng cho tất cả các thực thể đính kèm).
                *   *Nhược điểm:* Cần nỗ lực để tạo và duy trì.
        *   **Nội tuyến (Inline Policies):** Được nhúng trực tiếp vào một User, Group hoặc Role duy nhất.
            *   *Ưu điểm:* Mối quan hệ 1:1 chặt chẽ giữa chính sách và danh tính.
            *   *Nhược điểm:* Không thể tái sử dụng. Nếu bạn xóa danh tính, chính sách cũng bị xóa. Khó quản lý hơn ở quy mô lớn vì các quyền hạn nằm rải rác thay vì tập trung. **Nói chung, ưu tiên Customer Managed Policies hơn Inline Policies để dễ quản lý và tái sử dụng.** Sử dụng Inline Policies chỉ khi bạn chắc chắn rằng một chính sách chỉ áp dụng cho một thực thể duy nhất và sẽ không bao giờ được sử dụng lại.

    *   **Chính sách dựa trên tài nguyên (Resource-based Policies):** Đính kèm trực tiếp vào một **Tài nguyên** (ví dụ: bucket S3, hàng đợi SQS, khóa KMS, chủ đề SNS, hàm Lambda, điểm cuối VPC). Chúng xác định **ai (principal nào)** có quyền truy cập vào *tài nguyên đó*.
        *   *Khi nào và tại sao:* Chúng rất hữu ích để cấp quyền truy cập liên tài khoản (ví dụ: cho phép một Role từ tài khoản B ghi vào bucket S3 trong tài khoản A) hoặc để cấp quyền truy cập công khai (cẩn thận!) hoặc để xác định quyền truy cập cho các dịch vụ AWS thay mặt bạn (ví dụ: cho phép SNS xuất bản vào hàng đợi SQS). Chúng cũng cho phép bạn tập trung logic kiểm soát truy cập vào chính tài nguyên.
        *   *Mối quan hệ với chính sách dựa trên danh tính:* Đối với các hành động *trong cùng một tài khoản*, một hành động chỉ được phép nếu nó được cho phép bởi *hoặc* chính sách dựa trên danh tính *hoặc* chính sách dựa trên tài nguyên (hoặc cả hai). Tuy nhiên, đối với truy cập *liên tài khoản*, cả chính sách dựa trên danh tính của principal trong tài khoản gọi *và* chính sách dựa trên tài nguyên trong tài khoản chứa tài nguyên **đều phải** cho phép hành động đó (trừ khi principal là Root user hoặc sử dụng Role liên tài khoản, nơi mà Chính sách Tin cậy của Role và chính sách quyền hạn của Role đóng vai trò quyết định). Logic đánh giá đầy đủ phức tạp hơn và sẽ được đề cập sau.
        *   *Ví dụ quan trọng:* Chính sách Tin cậy (Trust Policy) của một IAM Role về mặt kỹ thuật là một loại Chính sách Dựa trên Tài nguyên, vì Role được coi là một tài nguyên IAM và chính sách này xác định ai có thể "truy cập" (đảm nhận) Role đó.

    *   **Giới hạn Quyền hạn (Permissions Boundaries):**
        *   Một tính năng **nâng cao** được sử dụng để đặt **quyền hạn tối đa** mà một chính sách dựa trên danh tính có thể cấp cho một IAM Role hoặc User.
        *   *Trường hợp sử dụng:* Rất hữu ích khi bạn muốn **ủy quyền** việc tạo Users hoặc Roles cho người khác (ví dụ: quản trị viên nhóm dự án) nhưng muốn đảm bảo rằng họ không thể tạo ra các principal có nhiều quyền hơn những gì bạn cho phép (ngăn chặn leo thang đặc quyền). Ví dụ: bạn có thể cho phép một quản trị viên tạo Roles, nhưng đặt một Permissions Boundary yêu cầu rằng bất kỳ Role nào họ tạo không bao giờ có thể có quyền truy cập vào các dịch vụ thanh toán hoặc IAM cốt lõi.
        *   *Cách hoạt động:* Quyền hạn hiệu quả của một principal (User/Role) có Permissions Boundary là **giao điểm** (intersection) của các chính sách dựa trên danh tính của nó VÀ Permissions Boundary của nó. Một hành động chỉ được phép nếu nó được cho phép bởi *cả hai*. Permissions Boundary *không* cấp quyền; nó chỉ đặt ra giới hạn tối đa.

    *   **Chính sách Kiểm soát Dịch vụ của Organizations (Organizations SCPs):**
        *   Nếu bạn đang sử dụng AWS Organizations để quản lý nhiều tài khoản, SCPs hoạt động như những **rào cản bảo vệ (guardrails)** ở cấp độ Tổ chức (Organization), OU (Organizational Unit), hoặc Tài khoản thành viên.
        *   *Vai trò:* SCPs **giới hạn** quyền hạn có thể được cấp bởi các chính sách dựa trên danh tính hoặc dựa trên tài nguyên trong các tài khoản bị ảnh hưởng. Chúng *không* cấp quyền; chúng chỉ lọc hoặc hạn chế những gì có thể được phép. Ví dụ: bạn có thể sử dụng SCP để ngăn chặn hoàn toàn việc sử dụng một số dịch vụ AWS nhất định (ví dụ: các dịch vụ không được phê duyệt) trong các tài khoản phát triển, ngay cả khi quản trị viên tài khoản đó cố gắng cấp quyền thông qua chính sách IAM.
        *   *Tương tác:* SCPs ảnh hưởng đến tất cả các Users và Roles trong tài khoản bị ảnh hưởng, *bao gồm cả Root User*. Chúng là một phần quan trọng của chiến lược quản trị đa tài khoản.

    *   **Chính sách Phiên (Session Policies):**
        *   Khi bạn tạo thông tin xác thực tạm thời một cách chương trình bằng cách sử dụng lệnh gọi `AssumeRole`, `AssumeRoleWithSAML`, `AssumeRoleWithWebIdentity`, hoặc `GetFederationToken` của STS, bạn có thể tùy chọn chuyển một tài liệu chính sách nội tuyến gọi là **Session Policy**.
        *   *Mục đích:* Quyền hạn hiệu quả của phiên tạm thời là **giao điểm** của các chính sách dựa trên danh tính của Role (hoặc User) được đảm nhận VÀ Session Policy được truyền vào. Điều này cho phép bạn giới hạn thêm quyền hạn của một phiên tạm thời cho một mục đích rất cụ thể, thậm chí còn chặt chẽ hơn so với những gì Role gốc được phép. Rất hữu ích cho các kịch bản Đặc quyền Tối thiểu động.

*   **Biến Chính sách (Policy Variables):**
    *   IAM cho phép bạn sử dụng các biến được xác định trước trong các chính sách để tạo ra các quyền hạn chung chung hơn mà không cần liệt kê mọi người dùng hoặc tài nguyên.
    *   Các biến này được thay thế bằng các giá trị từ bối cảnh của yêu cầu thực tế khi chính sách được đánh giá.
    *   Ví dụ:
        *   `${aws:username}`: Tên của IAM User đang thực hiện yêu cầu. Hữu ích để tạo "thư mục nhà" trong S3, ví dụ: chỉ cho phép User `Bob` truy cập vào `s3://my-bucket/home/Bob/*`.
        *   `${aws:PrincipalTag/TagName}`: Giá trị của thẻ (tag) được đính kèm vào principal (User hoặc Role) đang thực hiện yêu cầu. Đây là chìa khóa cho ABAC.
        *   `${aws:SourceIp}`: Địa chỉ IP nguồn của yêu cầu.
        *   `${ec2:ResourceTag/TagName}`: Giá trị của thẻ được đính kèm vào tài nguyên EC2 đang được truy cập.
    *   Việc sử dụng biến làm cho chính sách linh hoạt và dễ quản lý hơn, đặc biệt trong các môi trường lớn.

## 3. Trái tim của IAM: Logic Đánh giá Chính sách

Đây là phần quan trọng nhất và đôi khi phức tạp nhất của IAM. Làm thế nào AWS quyết định liệu một yêu cầu cụ thể (ví dụ: User `Alice` cố gắng thực hiện hành động `s3:PutObject` trên tài nguyên `arn:aws:s3:::my-data-bucket/confidential.txt`) nên được `Allow` hay `Deny`?

### Bối cảnh Yêu cầu (Request Context)

Mỗi khi một principal thực hiện một yêu cầu tới AWS, AWS sẽ thu thập thông tin về yêu cầu đó vào một **bối cảnh yêu cầu**. Bối cảnh này bao gồm:
*   **Principal:** Ai đang thực hiện yêu cầu (User, Role, người dùng liên kết, dịch vụ AWS)?
*   **Action:** Hành động API nào đang được yêu cầu (ví dụ: `ec2:StartInstances`)?
*   **Resource:** Tài nguyên nào là mục tiêu của hành động (ví dụ: `arn:aws:ec2:us-east-1:123456789012:instance/i-0abcdef1234567890`)?
*   **Environment Data:** Thông tin về môi trường như địa chỉ IP nguồn (`aws:SourceIp`), thời gian (`aws:CurrentTime`), liệu MFA có được sử dụng hay không (`aws:MultiFactorAuthPresent`), User Agent, vùng AWS (`aws:RequestedRegion`), v.v.
*   **Resource Tags:** Các thẻ được đính kèm vào tài nguyên đang được truy cập.
*   **Principal Tags:** Các thẻ được đính kèm vào User hoặc Role đang thực hiện yêu cầu.

AWS sử dụng thông tin trong bối cảnh yêu cầu này để đánh giá tất cả các chính sách áp dụng.

### Luồng đánh giá: Từ chối tường minh (Explicit Deny) -> Cho phép tường minh (Explicit Allow) -> Từ chối ngầm định (Implicit Deny / Default Deny)

Logic đánh giá tuân theo các bước sau một cách nghiêm ngặt:

1.  **Bắt đầu với "Từ chối Ngầm định" (Implicit Deny):** Theo mặc định, mọi hành động đều bị từ chối. Quyền hạn phải được cấp một cách tường minh. Không có quyền nào được cấp chỉ vì nó không bị từ chối.
2.  **Đánh giá TẤT CẢ các chính sách áp dụng:** AWS thu thập *tất cả* các chính sách liên quan đến bối cảnh yêu cầu. Điều này bao gồm:
    *   Chính sách Kiểm soát Dịch vụ của Organizations (**SCPs**) áp dụng cho tài khoản.
    *   Chính sách Dựa trên Tài nguyên (**Resource-based Policies**) đính kèm vào tài nguyên được yêu cầu (nếu có).
    *   Giới hạn Quyền hạn (**Permissions Boundary**) đính kèm vào principal (User/Role) (nếu có).
    *   Chính sách Dựa trên Danh tính (**Identity-based Policies**) đính kèm vào principal (User, Group mà User thuộc về, hoặc Role).
    *   Chính sách Phiên (**Session Policies**) được truyền vào khi đảm nhận Role (nếu có).
3.  **Kiểm tra Từ chối Tường minh (Explicit Deny):** AWS kiểm tra xem có *bất kỳ* câu lệnh nào trong *bất kỳ* chính sách áp dụng nào có `Effect: Deny` và khớp với bối cảnh yêu cầu (Action, Resource, Conditions) hay không.
    *   **Nếu tìm thấy một `Deny` khớp:** Yêu cầu bị **TỪ CHỐI (DENIED)** ngay lập tức. Quá trình đánh giá dừng lại. Một `Deny` tường minh luôn ghi đè bất kỳ `Allow` nào.
4.  **Kiểm tra Cho phép Tường minh (Explicit Allow) (Chỉ khi không có Deny nào khớp):** Nếu không có `Deny` tường minh nào khớp, AWS sẽ kiểm tra xem có *ít nhất một* câu lệnh trong các chính sách **liên quan đến quyền hạn** (Identity-based, Resource-based, Session) có `Effect: Allow` và khớp với bối cảnh yêu cầu hay không.
    *   **Quan trọng:** SCPs và Permissions Boundaries *không thể* cấp quyền `Allow`. Chúng chỉ có thể hạn chế. Vì vậy, `Allow` phải đến từ Chính sách Dựa trên Danh tính, Chính sách Dựa trên Tài nguyên hoặc Chính sách Phiên.
    *   **Ngoài ra:**
        *   Nếu có **SCP** áp dụng, hành động cũng phải được SCP cho phép (hoặc không bị SCP từ chối). SCP hoạt động như một bộ lọc.
        *   Nếu có **Permissions Boundary** áp dụng, hành động cũng phải được Permissions Boundary cho phép. Boundary hoạt động như một bộ lọc khác.
        *   Nếu có **Session Policy** áp dụng, hành động cũng phải được Session Policy cho phép.
    *   **Nếu tìm thấy ít nhất một `Allow` khớp VÀ hành động đó cũng được cho phép bởi SCP (nếu có) VÀ Permissions Boundary (nếu có) VÀ Session Policy (nếu có):** Yêu cầu được **CHO PHÉP (ALLOWED)**.
5.  **Kết quả Cuối cùng (Nếu không có Deny và không có Allow nào khớp):** Nếu sau khi kiểm tra tất cả các chính sách, không tìm thấy `Deny` tường minh nào khớp và cũng không tìm thấy `Allow` tường minh nào khớp (hoặc `Allow` bị chặn bởi SCP/Boundary/Session Policy), thì quyết định cuối cùng là **TỪ CHỐI (DENIED)** do nguyên tắc "Từ chối Ngầm định" ban đầu.

**Phép loại suy:** Hãy tưởng tượng bạn đang cố gắng đi qua một loạt các cổng an ninh để vào một căn phòng bảo mật.
*   **Từ chối Ngầm định:** Cánh cửa cuối cùng vào phòng mặc định là khóa.
*   **SCPs:** Cổng an ninh đầu tiên ở lối vào tòa nhà. Nếu cổng này nói "Không ai từ bộ phận X được phép vào tòa nhà này", thì dù bạn có chìa khóa phòng, bạn cũng không thể vào.
*   **Permissions Boundary:** Cổng an ninh thứ hai ở tầng của bạn. Nó nói "Nhân viên ở tầng này chỉ được phép vào các phòng A, B, C". Ngay cả khi bạn có chìa khóa phòng D, cổng này sẽ chặn bạn.
*   **Explicit Deny (Identity/Resource/Session):** Một nhân viên bảo vệ cụ thể đứng trước cửa phòng của bạn với lệnh "Không cho người này vào phòng này". Lệnh này ghi đè mọi thứ khác.
*   **Explicit Allow (Identity/Resource/Session):** Chìa khóa (hoặc thẻ truy cập) của bạn cho căn phòng cụ thể đó.
*   **Quy trình:** Bạn phải đi qua cổng SCP và cổng Boundary (nếu có). Sau đó, bạn đến cửa phòng. Nếu có bảo vệ với lệnh "Deny", bạn không được vào. Nếu không có lệnh "Deny" và chìa khóa "Allow" của bạn hoạt động, bạn được vào. Nếu không có lệnh "Deny" và bạn không có chìa khóa "Allow", cửa vẫn khóa (Implicit Deny).

### Cách các loại chính sách khác nhau tương tác (Identity + Resource + SCP + Boundary + Session)

Đây là bản tóm tắt tương tác quan trọng:

*   **Deny luôn thắng:** Một `Deny` trong *bất kỳ* loại chính sách nào (SCP, Boundary, Identity, Resource, Session) sẽ ghi đè mọi `Allow`.
*   **Allow phải đến từ Identity hoặc Resource (hoặc Session):** Để một hành động được phép, phải có một `Allow` tường minh trong chính sách Identity-based *hoặc* Resource-based (hoặc cả hai đối với một số trường hợp liên tài khoản) *hoặc* Session Policy.
*   **SCPs làm bộ lọc:** SCP chỉ có thể hạn chế quyền. Một hành động phải được phép bởi SCP (nghĩa là không bị SCP `Deny`, và nếu SCP chỉ có `Allow`, hành động phải khớp với `Allow` đó) *ngoài việc* được phép bởi các chính sách khác.
*   **Boundaries làm bộ lọc:** Permissions Boundary chỉ có thể hạn chế quyền. Một hành động phải được phép bởi Boundary (khớp với `Allow` của Boundary) *ngoài việc* được phép bởi chính sách Identity-based.
*   **Session Policies làm bộ lọc:** Session Policy chỉ có thể hạn chế quyền. Một hành động phải được phép bởi Session Policy *ngoài việc* được phép bởi chính sách Identity-based của Role/User được đảm nhận.
*   **Quyền hạn hiệu quả cuối cùng:** Là giao điểm của tất cả các chính sách áp dụng, với `Deny` luôn được ưu tiên.

*Sơ đồ khái niệm:* Hãy tưởng tượng một lưu đồ quyết định:
1.  Bắt đầu yêu cầu.
2.  Kiểm tra SCP: Hành động có bị SCP `Deny` không? -> Nếu có, DENIED.
3.  Kiểm tra SCP: Hành động có được SCP `Allow` không (nếu SCP chỉ định `Allow`)? -> Nếu không, DENIED.
4.  Kiểm tra Resource Policy: Hành động có bị Resource Policy `Deny` không? -> Nếu có, DENIED.
5.  Kiểm tra Identity Policy: Hành động có bị Identity Policy `Deny` không? -> Nếu có, DENIED.
6.  Kiểm tra Boundary: Hành động có bị Boundary `Deny` không? -> Nếu có, DENIED.
7.  Kiểm tra Session Policy: Hành động có bị Session Policy `Deny` không? -> Nếu có, DENIED.
8.  (Nếu không có Deny nào ở trên) Kiểm tra Allow:
    *   Hành động có được Identity Policy `Allow` không?
    *   Hành động có được Resource Policy `Allow` không?
    *   Hành động có được Boundary `Allow` không (nếu Boundary tồn tại)?
    *   Hành động có được Session Policy `Allow` không (nếu Session Policy tồn tại)?
9.  Nếu (có Allow từ Identity HOẶC Resource) VÀ (được Allow bởi Boundary nếu tồn tại) VÀ (được Allow bởi Session Policy nếu tồn tại) -> ALLOWED.
10. Nếu không -> DENIED (Implicit Deny).
*(Lưu ý: Đây là một mô hình đơn giản hóa; logic thực tế cho truy cập liên tài khoản và các điều kiện phức tạp hơn.)*

### Tác động của các khóa `Condition`

Khóa `Condition` là một công cụ cực kỳ mạnh mẽ để tinh chỉnh quyền hạn. Chúng cho phép một câu lệnh chính sách chỉ có hiệu lực khi các điều kiện cụ thể trong bối cảnh yêu cầu là đúng.

*   **Nền tảng của ABAC:** Conditions là cốt lõi của Kiểm soát Truy cập Dựa trên Thuộc tính (Attribute-Based Access Control - ABAC). Bằng cách sử dụng các điều kiện dựa trên thẻ (tags) của principal (`aws:PrincipalTag/project`) hoặc thẻ của tài nguyên (`ec2:ResourceTag/cost-center`), bạn có thể tạo ra các chính sách linh hoạt, có khả năng mở rộng mà không cần xác định mọi principal hoặc tài nguyên một cách tường minh.
*   **Tăng cường bảo mật:** Bạn có thể sử dụng Conditions để thực thi các lớp bảo mật bổ sung:
    *   Giới hạn truy cập từ các dải IP cụ thể (`aws:SourceIp`).
    *   Yêu cầu MFA cho các hành động nhạy cảm (`aws:MultiFactorAuthPresent`).
    *   Chỉ cho phép hành động trong các vùng cụ thể (`aws:RequestedRegion`).
    *   Giới hạn thời gian truy cập (`aws:CurrentTime`).
    *   Chỉ cho phép các hành động nếu một thẻ cụ thể tồn tại hoặc có giá trị nhất định.

Khi một câu lệnh chính sách chứa khối `Condition`, tất cả các điều kiện trong khối đó phải được đánh giá là `true` để câu lệnh đó được coi là khớp với yêu cầu. Nếu bất kỳ điều kiện nào là `false`, câu lệnh đó sẽ không khớp.

## 4. Khái niệm & Mẫu hình IAM Nâng cao

Khi môi trường AWS của bạn phát triển về quy mô và độ phức tạp, bạn sẽ cần tận dụng các tính năng và mẫu hình IAM nâng cao hơn.

### Liên kết Định danh (Federation): Tích hợp các danh tính bên ngoài

Liên kết định danh cho phép người dùng từ các hệ thống quản lý danh tính bên ngoài (ví dụ: thư mục công ty, nhà cung cấp danh tính web) đăng nhập vào AWS mà không cần tạo IAM Users riêng lẻ cho họ. Thay vào đó, họ đảm nhận một IAM Role sau khi được xác thực bởi nhà cung cấp danh tính (Identity Provider - IdP) của họ. Đây là phương pháp **được khuyến nghị mạnh mẽ** để quản lý quyền truy cập của con người vào AWS.

*   **Liên kết SAML 2.0 (Security Assertion Markup Language):**
    *   **Mục đích:** Tích hợp với các IdP doanh nghiệp hỗ trợ SAML 2.0, như Microsoft Active Directory Federation Services (ADFS), Azure AD, Okta, Ping Identity, Shibboleth.
    *   **Luồng:**
        1.  Người dùng truy cập ứng dụng nội bộ (hoặc cổng thông tin của IdP) và cố gắng truy cập AWS.
        2.  Ứng dụng chuyển hướng người dùng đến IdP để xác thực.
        3.  Người dùng xác thực với IdP (ví dụ: bằng tên người dùng/mật khẩu công ty).
        4.  IdP tạo một **xác nhận SAML (SAML assertion)** - một tài liệu XML được ký số chứa thông tin về người dùng đã xác thực (ví dụ: tên người dùng, thành viên nhóm, thuộc tính khác) và xác nhận rằng người dùng đã được xác thực. Xác nhận này cũng chỉ định AWS là nhà cung cấp dịch vụ (Service Provider - SP).
        5.  IdP gửi xác nhận SAML này trở lại trình duyệt của người dùng.
        6.  Trình duyệt gửi xác nhận SAML đến điểm cuối Đăng nhập Liên kết AWS (AWS Federation Sign-in endpoint).
        7.  AWS xác minh chữ ký trên xác nhận SAML bằng cách sử dụng siêu dữ liệu của IdP mà bạn đã cấu hình trong IAM (thiết lập **Mối quan hệ Tin cậy - Trust Relationship**).
        8.  AWS gọi API `sts:AssumeRoleWithSAML`, chuyển xác nhận SAML và ARN của IAM Role mà người dùng sẽ đảm nhận. Chính sách tin cậy của Role này phải cho phép principal SAML (`arn:aws:iam::ACCOUNT-ID:saml-provider/PROVIDER-NAME`).
        9.  STS trả về thông tin xác thực tạm thời cho Role đó.
        10. AWS chuyển hướng người dùng đến Bảng điều khiển Quản lý AWS, đăng nhập với tư cách là Role đã đảm nhận.
    *   **Ánh xạ Xác nhận (Assertion Mapping):** Bạn cấu hình IdP để gửi các thuộc tính người dùng (ví dụ: nhóm Active Directory) trong xác nhận SAML. Trong AWS IAM, bạn có thể ánh xạ các thuộc tính này thành các Thẻ Principal (Principal Tags) trên phiên Role hoặc sử dụng chúng trong các điều kiện chính sách (`aws:PrincipalTag/GroupName`). Điều này cho phép ABAC hoặc cấp các Roles khác nhau dựa trên thuộc tính của người dùng.

*   **Liên kết OpenID Connect (OIDC):**
    *   **Mục đích:** Tích hợp với các nhà cung cấp danh tính hỗ trợ OIDC, thường là các nhà cung cấp danh tính web công cộng (ví dụ: Login with Amazon, Facebook, Google) hoặc các IdP doanh nghiệp hỗ trợ OIDC (ví dụ: Okta, Azure AD, Auth0). Cũng được sử dụng để cho phép các workload trong các môi trường như Kubernetes hoặc GitHub Actions đảm nhận IAM Roles một cách an toàn.
    *   **Luồng (Ví dụ: Đăng nhập Web):**
        1.  Người dùng trên ứng dụng của bạn nhấp vào "Đăng nhập bằng Google".
        2.  Ứng dụng chuyển hướng người dùng đến Google để xác thực và ủy quyền (cho phép ứng dụng truy cập thông tin hồ sơ cơ bản).
        3.  Người dùng xác thực với Google và đồng ý.
        4.  Google trả về một **Mã thông báo ID OIDC (OIDC ID Token)** - một JSON Web Token (JWT) được ký - cho ứng dụng của bạn thông qua trình duyệt.
        5.  Ứng dụng của bạn gọi API `sts:AssumeRoleWithWebIdentity` của AWS, chuyển Mã thông báo ID OIDC và ARN của Role mà người dùng sẽ đảm nhận.
        6.  AWS xác minh chữ ký của Mã thông báo ID bằng khóa công khai của nhà cung cấp OIDC (mà bạn đã cấu hình trong IAM), xác thực các tuyên bố (claims) trong mã thông báo (ví dụ: `iss` - nhà phát hành, `aud` - đối tượng dự định là ứng dụng của bạn). Chính sách tin cậy của Role phải cho phép principal OIDC (`arn:aws:iam::ACCOUNT-ID:oidc-provider/PROVIDER-URL`).
        7.  STS trả về thông tin xác thực tạm thời cho Role đó.
        8.  Ứng dụng của bạn bây giờ có thể sử dụng các thông tin xác thực tạm thời này để thực hiện các lệnh gọi API AWS thay mặt người dùng.

*   **AWS IAM Identity Center (Kế thừa AWS SSO):**
    *   **Cách được khuyến nghị:** Đây là dịch vụ được AWS **khuyến nghị mạnh mẽ** để quản lý tập trung quyền truy cập của lực lượng lao động (người dùng) và các ứng dụng vào nhiều tài khoản AWS và các ứng dụng đám mây (bao gồm cả các ứng dụng SAML của bên thứ ba).
    *   **Lợi ích:**
        *   **Quản lý Tập trung:** Cung cấp một nơi duy nhất để quản lý người dùng, nhóm và quyền truy cập của họ trên toàn bộ Tổ chức AWS của bạn.
        *   **Tích hợp IdP:** Tích hợp liền mạch với các IdP bên ngoài (Azure AD, Okta, v.v. qua SAML) hoặc có thể hoạt động như một thư mục danh tính độc lập.
        *   **Truy cập Đa tài khoản Dễ dàng:** Đơn giản hóa việc cấp quyền truy cập vào nhiều tài khoản AWS thông qua một cổng thông tin người dùng duy nhất.
        *   **Bộ quyền (Permission Sets):** Sử dụng khái niệm "Bộ quyền" thay vì quản lý trực tiếp các IAM Roles và chính sách trong từng tài khoản thành viên. Một Bộ quyền bao gồm một tập hợp các chính sách (AWS Managed hoặc Customer Managed) và IAM Identity Center tự động tạo và quản lý các IAM Roles cần thiết trong các tài khoản được chỉ định khi bạn gán Bộ quyền cho người dùng hoặc nhóm.
        *   **Tích hợp ABAC:** Hỗ trợ ánh xạ thuộc tính từ IdP của bạn (ví dụ: bộ phận, trung tâm chi phí) đến các thuộc tính trong IAM Identity Center, sau đó có thể được sử dụng làm Thẻ Principal trong các Bộ quyền để triển khai ABAC.
        *   **Truy cập CLI v2 Tạm thời:** Tích hợp với AWS CLI v2 để tự động tìm nạp thông tin xác thực tạm thời, loại bỏ nhu cầu quản lý khóa truy cập dài hạn cho người dùng CLI.
    *   **Kiến trúc:** Hoạt động ở cấp độ Tổ chức AWS. Khi bạn bật IAM Identity Center, nó sẽ thiết lập cơ sở hạ tầng cần thiết (ví dụ: các Roles trong tài khoản quản lý và thành viên). Bạn định cấu hình nguồn danh tính (IdP bên ngoài hoặc Thư mục Identity Center). Sau đó, bạn tạo các Bộ quyền và gán chúng cho người dùng/nhóm từ nguồn danh tính của bạn, chỉ định tài khoản AWS nào họ có thể truy cập với Bộ quyền đó. Người dùng đăng nhập qua cổng thông tin Identity Center (hoặc được chuyển hướng từ IdP của họ) và chọn tài khoản/Role (Bộ quyền) họ muốn truy cập.

### Truy cập Liên tài khoản (Cross-Account Access): Kiến trúc truy cập an toàn giữa các tài khoản AWS

Trong các môi trường AWS lớn, việc sử dụng nhiều tài khoản là một thực tiễn tốt nhất để phân tách tài nguyên, giới hạn phạm vi ảnh hưởng và quản lý thanh toán/bảo mật. IAM Roles là cơ chế chính để cho phép truy cập an toàn giữa các tài khoản này.

*   **Cơ chế:**
    1.  **Tài khoản A (Tài khoản Tin cậy/Chứa Tài nguyên):** Tạo một IAM Role (ví dụ: `CrossAccountAccessRole`).
        *   **Chính sách Quyền hạn (Permissions Policy):** Xác định những gì Role này có thể làm trong Tài khoản A (ví dụ: đọc từ một bucket S3 cụ thể).
        *   **Chính sách Tin cậy (Trust Policy):** Chỉ định rằng principal từ **Tài khoản B** (ví dụ: một IAM User hoặc Role cụ thể trong Tài khoản B, hoặc toàn bộ Tài khoản B - `arn:aws:iam::ACCOUNT-B-ID:root`) được phép gọi `sts:AssumeRole` cho `CrossAccountAccessRole` này.
    2.  **Tài khoản B (Tài khoản Đáng tin cậy/Gọi):** Một principal (ví dụ: User `Operator` hoặc một Role khác) trong Tài khoản B cần truy cập tài nguyên trong Tài khoản A.
        *   **Chính sách Quyền hạn (Permissions Policy):** Principal này trong Tài khoản B phải có quyền thực hiện hành động `sts:AssumeRole` đối với `arn:aws:iam::ACCOUNT-A-ID:role/CrossAccountAccessRole`.
    3.  **Luồng:** Principal trong Tài khoản B gọi `sts:AssumeRole` nhắm vào Role trong Tài khoản A. Nếu Chính sách Tin cậy trong Tài khoản A cho phép và Chính sách Quyền hạn trong Tài khoản B cho phép, STS sẽ trả về thông tin xác thực tạm thời cho `CrossAccountAccessRole`. Principal trong Tài khoản B sau đó sử dụng các thông tin xác thực tạm thời này để truy cập tài nguyên trong Tài khoản A theo các quyền được xác định trong Chính sách Quyền hạn của Role đó.

*   **Các Mẫu hình Phổ biến:**
    *   **Hub-and-Spoke (Trung tâm và Nan hoa):** Một tài khoản trung tâm (Hub) chứa các công cụ, người dùng hoặc vai trò quản lý/vận hành chung. Các tài khoản khác (Spokes) chứa các ứng dụng hoặc khối lượng công việc. Roles trong các tài khoản Spoke cấp quyền truy cập cho các principal trong tài khoản Hub để thực hiện các nhiệm vụ quản lý (ví dụ: vá lỗi, triển khai, giám sát).
    *   **Ghi log/Bảo mật Tập trung:** Một tài khoản chuyên dụng (ví dụ: Log Archive, Security Tooling) được thiết lập. Các tài khoản khác được cấu hình để gửi log (CloudTrail, VPC Flow Logs, v.v.) đến một bucket S3 trong tài khoản Log Archive hoặc cho phép các công cụ bảo mật trong tài khoản Security Tooling (ví dụ: GuardDuty, Security Hub) truy cập và phân tích dữ liệu từ các tài khoản thành viên. Điều này được thực hiện thông qua các Chính sách Dựa trên Tài nguyên (ví dụ: chính sách bucket S3 cho phép ghi từ các tài khoản khác) và/hoặc Roles liên tài khoản (ví dụ: cho phép Security Hub trong tài khoản quản trị đảm nhận một Role trong tài khoản thành viên để thu thập phát hiện).

*   **External ID:** Khi cấp quyền truy cập liên tài khoản cho một bên thứ ba (ví dụ: một nhà cung cấp SaaS cần truy cập vào tài khoản của bạn), hãy sử dụng tham số `ExternalId` trong Chính sách Tin cậy của Role và trong lệnh gọi `sts:AssumeRole`. Đây là một chuỗi duy nhất do bạn (chủ sở hữu tài khoản) cung cấp cho bên thứ ba và bên thứ ba phải bao gồm nó khi đảm nhận vai trò. Nó giúp giải quyết vấn đề "confused deputy" bằng cách đảm bảo rằng chỉ có khách hàng dự định (người biết External ID) mới có thể khiến bên thứ ba đảm nhận vai trò trong tài khoản của họ.

### Kiểm soát Truy cập Dựa trên Thuộc tính (Attribute-Based Access Control - ABAC): Quyền hạn động dựa trên thẻ (tags) và thuộc tính

ABAC là một chiến lược ủy quyền xác định quyền hạn dựa trên các **thuộc tính** (còn gọi là **thẻ - tags**). Trong AWS, bạn có thể đính kèm thẻ vào các tài nguyên IAM (Users, Roles) và vào nhiều tài nguyên AWS khác (EC2 instances, S3 buckets, v.v.). Sau đó, bạn có thể tạo các chính sách IAM sử dụng các thẻ này trong khối `Condition` để đưa ra quyết định truy cập động.

*   **Tại sao nó Mạnh mẽ và Có khả năng Mở rộng:**
    *   Trong mô hình Kiểm soát Truy cập Dựa trên Vai trò (Role-Based Access Control - RBAC) truyền thống, khi số lượng vai trò và tài nguyên tăng lên, việc quản lý các chính sách có thể trở nên rất phức tạp ("role explosion"). Bạn có thể cần tạo nhiều vai trò rất giống nhau chỉ khác nhau về tài nguyên chúng có thể truy cập.
    *   Với ABAC, bạn có thể tạo một số ít Roles hoặc chính sách chung chung hơn sử dụng các điều kiện dựa trên thẻ. Quyền hạn sẽ tự động điều chỉnh khi các thẻ trên người dùng hoặc tài nguyên thay đổi, mà không cần cập nhật chính sách. Điều này đặc biệt hữu ích trong các môi trường lớn, năng động.
*   **Cách nó Bổ sung/Tương phản với RBAC:** ABAC không nhất thiết thay thế RBAC; chúng thường được sử dụng cùng nhau. Bạn vẫn có thể có các Roles dựa trên chức năng công việc (RBAC), nhưng các chính sách được đính kèm vào các Roles đó sử dụng điều kiện dựa trên thuộc tính (ABAC) để tinh chỉnh quyền truy cập.
*   **Ví dụ Thực tế:**
    *   **Kịch bản:** Bạn có nhiều dự án (Alpha, Beta, Gamma) và nhiều nhà phát triển. Bạn muốn đảm bảo rằng nhà phát triển chỉ có thể quản lý (khởi động, dừng) các phiên bản EC2 thuộc về dự án của họ.
    *   **Triển khai ABAC:**
        1.  **Gắn thẻ Principal:** Gắn thẻ cho IAM Roles (hoặc Users) của nhà phát triển với thẻ `project` (ví dụ: Role `DevRole-Alice` có thẻ `project=Alpha`, Role `DevRole-Bob` có thẻ `project=Beta`).
        2.  **Gắn thẻ Tài nguyên:** Gắn thẻ cho các phiên bản EC2 của bạn với thẻ `project` (ví dụ: các phiên bản cho dự án Alpha có thẻ `project=Alpha`).
        3.  **Tạo Chính sách IAM:** Tạo một chính sách duy nhất và đính kèm nó vào tất cả các Roles nhà phát triển:

            ```json
            {
              "Version": "2012-10-17",
              "Statement": [
                {
                  "Sid": "AllowManageEC2InstancesInOwnProject",
                  "Effect": "Allow",
                  "Action": [
                    "ec2:StartInstances",
                    "ec2:StopInstances",
                    "ec2:RebootInstances",
                    "ec2:TerminateInstances" // Hãy cẩn thận với quyền này!
                  ],
                  "Resource": "arn:aws:ec2:*:*:instance/*", // Áp dụng cho tất cả các phiên bản
                  "Condition": {
                    "StringEquals": {
                      // Chỉ cho phép nếu thẻ 'project' của phiên bản khớp với thẻ 'project' của Role
                      "ec2:ResourceTag/project": "${aws:PrincipalTag/project}"
                    }
                  }
                },
                {
                  "Sid": "AllowDescribeInstancesToSeeAll", // Thường cần quyền xem để chọn phiên bản
                  "Effect": "Allow",
                  "Action": "ec2:DescribeInstances",
                  "Resource": "*" // DescribeInstances không hỗ trợ kiểm soát truy cập cấp tài nguyên chi tiết
                }
              ]
            }
            ```

    *   **Kết quả:** Với chính sách duy nhất này, Alice (đảm nhận Role có `project=Alpha`) chỉ có thể quản lý các phiên bản EC2 có thẻ `project=Alpha`. Bob (đảm nhận Role có `project=Beta`) chỉ có thể quản lý các phiên bản có thẻ `project=Beta`. Nếu một dự án mới (Delta) được thêm vào, bạn chỉ cần tạo Role/User với thẻ `project=Delta` và gắn thẻ các tài nguyên EC2 mới tương ứng; chính sách hiện có sẽ tự động hoạt động.

### Vai trò Liên kết Dịch vụ (Service-Linked Roles - SLRs): Các vai trò được tạo và quản lý bởi các dịch vụ AWS

*   **Mục đích:** Một loại IAM Role đặc biệt được liên kết trực tiếp với một dịch vụ AWS cụ thể. Dịch vụ đó đảm nhận Role này để thực hiện các hành động thay mặt bạn trong tài khoản của bạn, nhưng chỉ cho các tác vụ cần thiết cho chức năng của dịch vụ đó.
*   **Đặc điểm:**
    *   **Chính sách Tin cậy Được xác định trước:** Chính sách tin cậy của SLR được định cấu hình sẵn để chỉ cho phép dịch vụ liên kết đảm nhận nó (`"Principal": {"Service": "SERVICE-NAME.amazonaws.com"}`). Bạn không thể sửa đổi chính sách tin cậy này.
    *   **Chính sách Quyền hạn Được quản lý bởi Dịch vụ:** Chính sách quyền hạn của SLR được xác định và quản lý bởi dịch vụ liên kết. Nó chứa các quyền cần thiết để dịch vụ hoạt động đúng cách. Bạn thường không thể sửa đổi chính sách quyền hạn này trực tiếp (mặc dù bạn có thể xem nó).
    *   **Tạo và Xóa:** SLRs thường được tạo tự động khi bạn sử dụng một tính năng của dịch vụ AWS lần đầu tiên yêu cầu nó. Việc xóa SLR thường yêu cầu bạn trước tiên phải vô hiệu hóa hoặc dọn dẹp các tài nguyên trong dịch vụ đang sử dụng Role đó. Bạn không thể xóa một SLR nếu nó vẫn đang được sử dụng bởi dịch vụ.
*   **Ví dụ:** AWS Auto Scaling sử dụng SLR (`AWSServiceRoleForAutoScaling`) để khởi chạy và chấm dứt các phiên bản EC2. Elastic Load Balancing sử dụng SLR (`AWSServiceRoleForElasticLoadBalancing`) để đăng ký và hủy đăng ký các mục tiêu. AWS Config sử dụng SLR (`AWSServiceRoleForConfig`) để mô tả tài nguyên.
*   **Lợi ích:** Đơn giản hóa việc cấp quyền cho các dịch vụ AWS vì bạn không cần phải tự tạo và quản lý các Roles này. Đảm bảo rằng dịch vụ chỉ có các quyền cần thiết và không thể bị đảm nhận bởi các thực thể khác ngoài chính dịch vụ đó.

### Trình phân tích Truy cập IAM (IAM Access Analyzer): Chủ động xác định quyền truy cập không mong muốn

Đây là một công cụ bảo mật quan trọng giúp bạn xác định các tài nguyên trong tổ chức và tài khoản của mình, chẳng hạn như bucket S3 hoặc vai trò IAM, được chia sẻ với một thực thể bên ngoài.

*   **Cách hoạt động:** Access Analyzer liên tục phân tích các Chính sách Dựa trên Tài nguyên (S3 buckets, KMS keys, Lambda functions, SQS queues, IAM roles trust policies, Secrets Manager secrets) trong tài khoản (hoặc toàn bộ Tổ chức) của bạn. Nó sử dụng **lý luận tự động (automated reasoning)** và logic dựa trên toán học để phân tích *tất cả các đường dẫn truy cập có thể có* được cấp bởi các chính sách này.
*   **Phát hiện (Findings):** Nó tạo ra các "phát hiện" (findings) cho các tài nguyên cho phép truy cập từ bên ngoài khu vực tin cậy của bạn (tài khoản hoặc tổ chức của bạn). Ví dụ:
    *   Một bucket S3 cho phép truy cập công khai (Public Access).
    *   Một Role có Chính sách Tin cậy cho phép một tài khoản AWS không xác định hoặc bên thứ ba đảm nhận nó.
    *   Một khóa KMS có thể được sử dụng bởi một tài khoản bên ngoài.
*   **Tại sao quan trọng:** Giúp bạn chủ động xác định và khắc phục các cấu hình sai về quyền có thể dẫn đến rò rỉ dữ liệu hoặc truy cập trái phép, trước khi chúng bị lợi dụng. Nó giúp thực thi Đặc quyền Tối thiểu bằng cách làm nổi bật quyền truy cập rộng rãi hoặc ngoài ý muốn.
*   **Các tính năng khác:**
    *   **Phân tích chính sách:** Kiểm tra các chính sách IAM của bạn so với các thực tiễn tốt nhất và cung cấp các đề xuất có thể hành động.
    *   **Tạo chính sách:** Giúp tạo các chính sách Đặc quyền Tối thiểu dựa trên hoạt động truy cập được ghi lại trong CloudTrail.
    *   **Trình phân tích truy cập tùy chỉnh:** Cho phép bạn xác định các tiêu chuẩn bảo mật của riêng mình và phát hiện các truy cập không tuân thủ (ví dụ: phát hiện quyền truy cập ghi không được mã hóa).

### Thông tin xác thực Tạm thời & STS (Security Token Service): Động cơ đằng sau việc đảm nhận vai trò và liên kết định danh

AWS Security Token Service (STS) là một dịch vụ web toàn cầu cho phép bạn yêu cầu các thông tin xác thực tạm thời, có đặc quyền giới hạn cho người dùng IAM hoặc cho những người dùng mà bạn xác thực (người dùng liên kết).

*   **Mục đích:** Nền tảng cho việc đảm nhận Role và Liên kết Định danh. Thay vì sử dụng thông tin xác thực dài hạn, các thực thể yêu cầu thông tin xác thực tạm thời từ STS có hiệu lực trong một khoảng thời gian ngắn (từ vài phút đến vài giờ).
*   **Các Lệnh gọi API Chính:**
    *   `AssumeRole`: Được sử dụng bởi một principal IAM (User hoặc Role) để đảm nhận một Role khác trong cùng tài khoản hoặc tài khoản khác. Yêu cầu ARN của Role cần đảm nhận và một tên phiên (session name). Trả về thông tin xác thực tạm thời (Access Key ID, Secret Access Key, Session Token) và thời gian hết hạn. Principal gọi phải được liệt kê trong Chính sách Tin cậy của Role mục tiêu và phải có quyền `sts:AssumeRole` trong chính sách quyền hạn của chính nó.
    *   `AssumeRoleWithSAML`: Được sử dụng trong luồng Liên kết SAML 2.0. Yêu cầu ARN của Role, ARN của nhà cung cấp SAML và xác nhận SAML từ IdP. Trả về thông tin xác thực tạm thời.
    *   `AssumeRoleWithWebIdentity`: Được sử dụng trong luồng Liên kết OIDC. Yêu cầu ARN của Role, Mã thông báo ID OIDC từ nhà cung cấp danh tính web/OIDC, và một tên phiên. Trả về thông tin xác thực tạm thời.
    *   `GetFederationToken`: Ít phổ biến hơn. Được sử dụng để cấp thông tin xác thực tạm thời cho người dùng liên kết mà không cần tạo IAM Role. Bạn chuyển thông tin xác thực của một IAM User dài hạn và yêu cầu một mã thông báo tạm thời với các quyền hạn bị giới hạn bởi một chính sách được truyền vào. Thường được sử dụng trong các proxy liên kết tùy chỉnh cũ hơn; `AssumeRole` thường được ưu tiên.
    *   `GetSessionToken`: Được sử dụng bởi một IAM User để lấy thông tin xác thực tạm thời cho chính User đó. Hữu ích nếu bạn muốn thực thi MFA cho các hoạt động API. User gọi API bằng thông tin xác thực dài hạn của họ (và mã MFA nếu được yêu cầu), STS trả về thông tin xác thực tạm thời. Bất kỳ lệnh gọi API nào sau đó sử dụng thông tin xác thực tạm thời này sẽ có `aws:MultiFactorAuthPresent=true` trong bối cảnh yêu cầu, cho phép bạn yêu cầu MFA trong các điều kiện chính sách.
*   **Lợi ích của Thông tin xác thực Tạm thời:**
    *   **Giảm Rủi ro:** Không cần nhúng hoặc lưu trữ thông tin xác thực dài hạn. Nếu thông tin xác thực tạm thời bị lộ, chúng chỉ có hiệu lực trong một thời gian ngắn.
    *   **Đặc quyền Tối thiểu:** Quyền hạn của thông tin xác thực tạm thời được xác định bởi Role được đảm nhận (và có thể bị giới hạn thêm bởi Session Policy), cho phép bạn cấp quyền chi tiết cho các tác vụ cụ thể.
    *   **Kiểm toán:** Các hành động được thực hiện bằng thông tin xác thực tạm thời từ một Role được ghi lại trong CloudTrail, cho biết cả Role đã được đảm nhận và danh tính ban đầu đã đảm nhận Role đó (nếu có thể xác định).

## 5. Thực tiễn Tốt nhất về IAM: Theo Khuôn khổ Well-Architected

Trụ cột Bảo mật của Khuôn khổ Kiến trúc Tối ưu AWS (AWS Well-Architected Framework) nhấn mạnh tầm quan trọng của việc quản lý danh tính và quyền hạn. Dưới đây là các thực tiễn tốt nhất cốt lõi về IAM mà mọi kiến trúc sư và nhà vận hành AWS nên tuân thủ:

1.  **Thực thi Nguyên tắc Đặc quyền Tối thiểu một cách nghiêm ngặt:**
    *   **Tại sao:** Giảm thiểu bán kính vụ nổ nếu thông tin xác thực bị xâm phạm. Ngăn chặn các hành động vô tình hoặc cố ý gây hại.
    *   **Cách làm:** Bắt đầu với quyền hạn tối thiểu và chỉ cấp thêm khi thực sự cần thiết. Sử dụng các công cụ như IAM Access Analyzer để xác định các quyền không sử dụng và tạo chính sách dựa trên hoạt động CloudTrail. Thường xuyên xem xét và tinh chỉnh các chính sách. Sử dụng `Condition` để giới hạn quyền truy cập.

2.  **Sử dụng Roles cho các ứng dụng và dịch vụ AWS:**
    *   **Tại sao:** Loại bỏ nhu cầu quản lý thông tin xác thực IAM User dài hạn trong các ứng dụng hoặc phiên bản EC2. Roles cung cấp thông tin xác thực tạm thời, tự động xoay vòng.
    *   **Cách làm:** Sử dụng Hồ sơ Phiên bản EC2 (Instance Profiles), Vai trò Thực thi Lambda (Execution Roles), Vai trò Tác vụ ECS (ECS Task Roles), v.v. Không bao giờ nhúng khóa truy cập vào mã hoặc cấu hình.

3.  **Ưu tiên Liên kết Định danh và IAM Identity Center hơn IAM Users cho quyền truy cập của con người:**
    *   **Tại sao:** Cho phép người dùng sử dụng danh tính doanh nghiệp hiện có của họ. Đơn giản hóa việc quản lý người dùng (tạo/vô hiệu hóa ở một nơi). Cải thiện trải nghiệm người dùng (đăng nhập một lần). Giảm gánh nặng quản lý thông tin xác thực IAM User dài hạn (mật khẩu, khóa truy cập, MFA).
    *   **Cách làm:** Triển khai IAM Identity Center và tích hợp nó với IdP SAML hoặc OIDC của bạn (ví dụ: Azure AD, Okta). Nếu bạn không có IdP, hãy sử dụng thư mục tích hợp của Identity Center. Chỉ tạo IAM Users khi thực sự cần thiết (ví dụ: một số tài khoản dịch vụ cụ thể không thể sử dụng Roles, hoặc cho người dùng quản trị ban đầu trước khi thiết lập liên kết).

4.  **Bật và thực thi MFA ở mọi nơi có thể:**
    *   **Tại sao:** Cung cấp một lớp bảo mật bổ sung quan trọng chống lại việc thông tin xác thực bị xâm phạm.
    *   **Cách làm:** **Bắt buộc** MFA cho Root User. **Bắt buộc** MFA cho tất cả IAM Users, đặc biệt là những người có quyền hạn cao. Sử dụng các điều kiện chính sách (`aws:MultiFactorAuthPresent`) để yêu cầu MFA cho các hành động nhạy cảm, ngay cả đối với Roles.

5.  **Xoay vòng thông tin xác thực thường xuyên:**
    *   **Tại sao:** Giảm thiểu khoảng thời gian một khóa truy cập bị xâm phạm có thể được sử dụng.
    *   **Cách làm:** Đối với IAM Users (nếu bạn phải sử dụng chúng), thiết lập chính sách mật khẩu mạnh và thực thi việc thay đổi mật khẩu định kỳ. Quan trọng hơn, **xoay vòng khóa truy cập** một cách thường xuyên (ví dụ: 90 ngày hoặc ít hơn). Tự động hóa quy trình xoay vòng nếu có thể. Sử dụng các công cụ như IAM Credentials Report để theo dõi tuổi của thông tin xác thực. *Lưu ý: Vấn đề này phần lớn được giảm thiểu bằng cách sử dụng Roles và liên kết định danh, vốn sử dụng thông tin xác thực tạm thời.*

6.  **Sử dụng `Conditions` trong các chính sách để tăng cường các lớp bảo mật:**
    *   **Tại sao:** Cung cấp kiểm soát truy cập chi tiết và thực thi các chính sách bảo mật bổ sung.
    *   **Cách làm:** Sử dụng `aws:SourceIp` để giới hạn truy cập từ các mạng đáng tin cậy. Sử dụng `aws:RequestedRegion` để giới hạn hành động trong các vùng được phê duyệt. Sử dụng `aws:MultiFactorAuthPresent` để yêu cầu MFA. Sử dụng thẻ (tags) và các điều kiện liên quan đến thẻ (`aws:PrincipalTag`, `ec2:ResourceTag`, v.v.) để triển khai ABAC.

7.  **Triển khai Permissions Boundaries để ủy quyền an toàn:**
    *   **Tại sao:** Cho phép bạn ủy quyền việc tạo Role/User cho người khác mà không có nguy cơ họ tạo ra các principal có nhiều quyền hơn mức cho phép.
    *   **Cách làm:** Xác định các quyền hạn tối đa mà một nhóm quản trị viên có thể cấp. Tạo một Chính sách Quản lý Khách hàng (Customer Managed Policy) hoạt động như Permissions Boundary. Yêu cầu (thông qua chính sách hoặc SCP) rằng tất cả Roles/Users do nhóm đó tạo ra phải được đính kèm Boundary này.

8.  **Tận dụng IAM Groups để quản lý người dùng:**
    *   **Tại sao:** Đơn giản hóa việc quản lý quyền hạn cho nhiều IAM Users có cùng yêu cầu truy cập.
    *   **Cách làm:** Tạo Groups dựa trên vai trò công việc hoặc chức năng. Đính kèm chính sách IAM vào Groups thay vì Users riêng lẻ. Thêm/xóa Users khỏi Groups để quản lý quyền hạn của họ. *Lưu ý: Điều này chủ yếu áp dụng nếu bạn đang quản lý IAM Users trực tiếp, ít liên quan hơn khi sử dụng IAM Identity Center với các nhóm được đồng bộ hóa từ IdP.*

9.  **Sử dụng Chính sách do AWS quản lý (AWS Managed Policies) một cách thận trọng, ưu tiên Chính sách do Khách hàng quản lý (Customer Managed Policies) được điều chỉnh:**
    *   **Tại sao:** Các chính sách do AWS quản lý, đặc biệt là các chính sách cấp cao như `AdministratorAccess` hoặc `PowerUserAccess`, thường quá rộng và vi phạm Đặc quyền Tối thiểu. Chúng có thể thay đổi mà không cần thông báo trực tiếp, có khả năng cấp các quyền mới không mong muốn.
    *   **Cách làm:** Sử dụng các chính sách do AWS quản lý làm điểm khởi đầu hoặc cho các vai trò công việc rất chung chung nếu cần. Tuy nhiên, hãy dành thời gian để phân tích các yêu cầu và tạo các Chính sách do Khách hàng quản lý chỉ cấp các quyền cần thiết. Xem xét các chính sách do AWS quản lý cho các vai trò công việc cụ thể hơn (`AmazonRDSFullAccess`) nhưng vẫn kiểm tra nội dung của chúng.

10. **Thường xuyên kiểm toán quyền hạn bằng IAM Access Analyzer và CloudTrail:**
    *   **Tại sao:** Phát hiện các quyền truy cập không mong muốn, quyền không sử dụng và các cấu hình sai. Đảm bảo tuân thủ Đặc quyền Tối thiểu và các chính sách bảo mật.
    *   **Cách làm:** Bật IAM Access Analyzer cho tất cả các tài khoản (hoặc trên toàn Tổ chức). Thường xuyên xem xét các phát hiện của nó. Phân tích log CloudTrail để hiểu cách các quyền đang được sử dụng (hoặc không được sử dụng). Sử dụng thông tin này để tinh chỉnh các chính sách.

11. **Sử dụng SCPs trong AWS Organizations để đặt ra các rào cản bảo vệ rộng rãi:**
    *   **Tại sao:** Thực thi các yêu cầu bảo mật và tuân thủ trên toàn bộ tổ chức hoặc các đơn vị OU cụ thể. Ngăn chặn các hành động rủi ro ngay cả bởi quản trị viên tài khoản.
    *   **Cách làm:** Xác định các kiểm soát bảo mật cấp cao (ví dụ: cấm các vùng không được phê duyệt, yêu cầu gắn thẻ tài nguyên, ngăn chặn việc vô hiệu hóa các dịch vụ bảo mật như CloudTrail hoặc GuardDuty, giới hạn quyền IAM). Triển khai các kiểm soát này dưới dạng SCPs và áp dụng chúng vào gốc Tổ chức hoặc các OU thích hợp. Bắt đầu với danh sách từ chối (denylist) các hành động rủi ro cụ thể, cẩn thận không chặn các hành động cần thiết cho hoạt động bình thường.

12. **Gắn thẻ (Tag) tài nguyên IAM (Users, Roles) để quản lý tốt hơn và cho ABAC:**
    *   **Tại sao:** Cho phép quản lý chi phí, tự động hóa và triển khai Kiểm soát Truy cập Dựa trên Thuộc tính (ABAC).
    *   **Cách làm:** Xác định một chiến lược gắn thẻ nhất quán. Gắn thẻ cho Users và Roles với các thuộc tính có ý nghĩa như `cost-center`, `project`, `owner`, `environment`. Sử dụng các thẻ này trong điều kiện chính sách (`aws:PrincipalTag`) để cấp quyền động.

13. **Bảo mật Người dùng Gốc (Root User):**
    *   **Tại sao:** Root user có quyền truy cập không hạn chế. Việc xâm phạm nó là thảm họa.
    *   **Cách làm:** Không bao giờ sử dụng Root cho các hoạt động hàng ngày. Sử dụng một mật khẩu cực kỳ mạnh và duy nhất. **Bật MFA** cho Root. **Xóa tất cả các khóa truy cập** của Root. Lưu trữ thông tin xác thực Root một cách an toàn (ví dụ: trong két sắt vật lý). Thiết lập thông tin liên hệ tài khoản chính xác.

## 6. Vận hành IAM: Kiểm toán, Xử lý sự cố & Sự phát triển

Quản lý IAM không chỉ là thiết lập ban đầu; đó là một quá trình liên tục đòi hỏi sự giám sát, bảo trì và khắc phục sự cố.

### Kiểm toán: Theo dõi Ai đã làm Gì

*   **AWS CloudTrail:** Đây là dịch vụ nền tảng để kiểm toán trong AWS. CloudTrail ghi lại hầu hết các lệnh gọi API được thực hiện trong tài khoản của bạn, bao gồm cả các lệnh gọi liên quan đến IAM (tạo/xóa người dùng, vai trò, chính sách, thay đổi chính sách, đảm nhận vai trò) và các lệnh gọi được thực hiện bởi các principal IAM đó đến các dịch vụ khác.
*   **Thông tin Ghi lại:** Mỗi sự kiện CloudTrail chứa thông tin quan trọng:
    *   Danh tính người gọi (User ARN, Role ARN, thông tin phiên Role)
    *   Thời gian xảy ra sự kiện
    *   Địa chỉ IP nguồn
    *   Hành động API được gọi
    *   Tham số yêu cầu
    *   Tài nguyên bị ảnh hưởng
    *   Phản hồi (thành công/lỗi)
*   **Tầm quan trọng:** CloudTrail cung cấp nhật ký kiểm toán cần thiết cho việc điều tra bảo mật, khắc phục sự cố vận hành và tuân thủ quy định. **Luôn bật CloudTrail trong tất cả các tài khoản và vùng, và bảo vệ log của nó** (ví dụ: gửi đến một bucket S3 trong tài khoản lưu trữ log riêng biệt với quyền truy cập hạn chế và bật tính năng khóa đối tượng hoặc xác thực tệp log).
*   **Tích hợp:** Tích hợp CloudTrail với Amazon CloudWatch Logs để tìm kiếm và phân tích log dễ dàng hơn, và với CloudWatch Events (nay là EventBridge) để kích hoạt cảnh báo hoặc hành động tự động dựa trên các sự kiện IAM cụ thể (ví dụ: thay đổi chính sách quan trọng, lỗi đăng nhập Root). Sử dụng các dịch vụ như Amazon Athena để truy vấn log CloudTrail được lưu trữ trong S3 bằng SQL tiêu chuẩn.

### Xử lý sự cố: Các Vấn đề Phổ biến và Công cụ

Xử lý sự cố IAM có thể khó khăn do sự phức tạp của logic đánh giá chính sách. Dưới đây là một số vấn đề phổ biến và cách tiếp cận chúng:

*   **Nhầm lẫn Giữa Từ chối Ngầm định và Tường minh:** Người dùng bị từ chối truy cập mặc dù bạn nghĩ rằng họ có quyền.
    *   **Nguyên nhân có thể:** Không có câu lệnh `Allow` nào khớp với yêu cầu, hoặc một câu lệnh `Deny` tường minh ở đâu đó (trong chính sách Identity, Resource, SCP, Boundary, Session) đang ghi đè.
    *   **Cách khắc phục:** Sử dụng **Trình mô phỏng Chính sách IAM (IAM Policy Simulator)**. Công cụ này cho phép bạn chọn một User/Role/Group, chọn các hành động và tài nguyên, và mô phỏng yêu cầu để xem chính sách nào sẽ khớp và liệu kết quả cuối cùng là `Allow` hay `Deny`. Nó cũng hiển thị câu lệnh cụ thể gây ra `Deny` tường minh (nếu có).
*   **Lỗi Cú pháp Chính sách:** Chính sách không hoạt động như mong đợi hoặc không thể lưu.
    *   **Nguyên nhân có thể:** Lỗi JSON (dấu phẩy, dấu ngoặc), tên Action/Resource/Condition không hợp lệ, giá trị không đúng định dạng.
    *   **Cách khắc phục:** Sử dụng trình soạn thảo chính sách trực quan trong Bảng điều khiển Quản lý AWS, nó cung cấp kiểm tra cú pháp cơ bản. Đối với các chính sách phức tạp hơn, hãy xác thực JSON bằng các công cụ bên ngoài. Tham khảo tài liệu AWS để biết tên Action, Resource ARN và Condition Keys chính xác cho từng dịch vụ. IAM Access Analyzer cũng có thể giúp xác thực chính sách.
*   **Vấn đề Truy cập Liên tài khoản:** Một Role trong tài khoản A không thể được đảm nhận bởi principal trong tài khoản B, hoặc Role được đảm nhận không thể truy cập tài nguyên dự kiến.
    *   **Nguyên nhân có thể:**
        *   Chính sách Tin cậy của Role trong tài khoản A không chỉ định đúng principal từ tài khoản B (hoặc toàn bộ tài khoản B).
        *   Principal trong tài khoản B không có quyền `sts:AssumeRole` đối với Role trong tài khoản A.
        *   Chính sách Quyền hạn của Role trong tài khoản A không cấp quyền cần thiết cho tài nguyên trong tài khoản A.
        *   Nếu truy cập tài nguyên có Chính sách Dựa trên Tài nguyên (ví dụ: S3 bucket), chính sách đó cũng cần được kiểm tra.
        *   SCP trong tài khoản A hoặc B đang chặn hành động `sts:AssumeRole` hoặc hành động trên tài nguyên.
    *   **Cách khắc phục:** Kiểm tra kỹ Chính sách Tin cậy (trong A) và Chính sách Quyền hạn của principal gọi (trong B) và Chính sách Quyền hạn của Role được đảm nhận (trong A). Sử dụng Trình mô phỏng Chính sách trong cả hai tài khoản nếu cần. Kiểm tra log CloudTrail trong cả hai tài khoản để tìm lỗi `AccessDenied`.
*   **Bị chặn bởi SCP:** Người dùng (kể cả Root) không thể thực hiện hành động mặc dù chính sách IAM cho phép.
    *   **Nguyên nhân có thể:** Một SCP ở cấp độ OU hoặc Tổ chức đang áp dụng một `Deny` tường minh hoặc không có `Allow` nào trong SCP khớp với hành động (nếu SCP sử dụng chiến lược danh sách cho phép - allowlist).
    *   **Cách khắc phục:** Kiểm tra các SCP được áp dụng cho tài khoản từ bảng điều khiển AWS Organizations. Sử dụng Trình mô phỏng Chính sách, nó cũng tính đến SCPs khi đánh giá. Liên hệ với quản trị viên Tổ chức nếu cần điều chỉnh SCP.
*   **Vấn đề với Conditions:** Chính sách không hoạt động như mong đợi khi có khối `Condition`.
    *   **Nguyên nhân có thể:** Khóa điều kiện (condition key) không áp dụng cho hành động/tài nguyên đó, toán tử điều kiện sai, giá trị không khớp (phân biệt chữ hoa chữ thường đối với toán tử `StringEquals`), biến chính sách (`${aws:username}`) không được giải quyết như mong đợi.
    *   **Cách khắc phục:** Kiểm tra kỹ tài liệu về các khóa điều kiện có sẵn. Sử dụng Trình mô phỏng Chính sách, nó cho phép bạn cung cấp các giá trị bối cảnh (như IP nguồn, thẻ principal) để kiểm tra các điều kiện. Kiểm tra log CloudTrail, đôi khi thông báo lỗi có thể chứa thông tin về điều kiện không thành công.

**Công cụ Chính:**
*   **IAM Policy Simulator:** Công cụ tốt nhất để gỡ lỗi các quyết định ủy quyền cho một yêu cầu cụ thể.
*   **IAM Access Analyzer:** Xác định quyền truy cập bên ngoài, xác thực chính sách, tạo chính sách dựa trên hoạt động.
*   **AWS CloudTrail:** Nhật ký kiểm toán chi tiết về ai đã làm gì, khi nào, và kết quả.
*   **IAM Credentials Report:** Liệt kê tất cả người dùng IAM và trạng thái của thông tin xác thực của họ (mật khẩu, khóa truy cập, MFA).
*   **IAM Access Advisor:** Hiển thị các quyền dịch vụ được cấp cho một Role hoặc User và thời điểm các dịch vụ đó được truy cập lần cuối. Giúp xác định các quyền không sử dụng.

### Bối cảnh Lịch sử & Sự phát triển: Tại sao IAM lại như ngày nay?

Hiểu một chút về lịch sử giúp làm sáng tỏ lý do tại sao một số tính năng tồn tại:

*   **Giai đoạn đầu (Trước Roles):** Ban đầu, cách duy nhất để cấp quyền cho các ứng dụng chạy trên EC2 là tạo IAM Users và nhúng khóa truy cập dài hạn của họ vào ứng dụng hoặc cấu hình phiên bản. Đây là một cơn ác mộng về bảo mật và quản lý.
*   **Sự ra đời của Roles (Giải quyết vấn đề EC2):** IAM Roles được giới thiệu chủ yếu để giải quyết vấn đề này. Bằng cách cho phép các phiên bản EC2 đảm nhận một Role thông qua Instance Profile, AWS có thể tự động cung cấp thông tin xác thực tạm thời cho ứng dụng mà không cần khóa truy cập dài hạn. Đây là một bước tiến lớn về bảo mật. Sau đó, Roles được mở rộng để hỗ trợ truy cập liên tài khoản và liên kết định danh.
*   **Thách thức của Liên kết Thủ công:** Thiết lập liên kết SAML/OIDC ban đầu yêu cầu cấu hình thủ công đáng kể trong IAM (tạo nhà cung cấp danh tính, Roles, chính sách tin cậy).
*   **AWS Single Sign-On (SSO) -> IAM Identity Center:** AWS SSO được giới thiệu để đơn giản hóa đáng kể việc thiết lập và quản lý liên kết định danh và truy cập đa tài khoản, đặc biệt trong môi trường AWS Organizations. Nó tự động hóa việc tạo Roles và cung cấp một cổng thông tin người dùng tập trung. Việc đổi tên thành IAM Identity Center nhấn mạnh vai trò trung tâm của nó trong chiến lược quản lý danh tính của AWS.
*   **Từ RBAC đến ABAC:** Khi các môi trường trở nên lớn hơn, việc quản lý hàng trăm hoặc hàng nghìn Roles (RBAC) trở nên khó khăn. AWS đã đầu tư vào ABAC bằng cách mở rộng hỗ trợ gắn thẻ cho nhiều tài nguyên hơn và giới thiệu các khóa điều kiện dựa trên thẻ (`aws:PrincipalTag`, `ec2:ResourceTag`, v.v.), cho phép một cách tiếp cận linh hoạt và có khả năng mở rộng hơn để quản lý quyền hạn.
*   **Tập trung vào Phòng ngừa:** Các công cụ như IAM Access Analyzer và SCPs phản ánh sự chuyển dịch sang việc chủ động ngăn chặn và phát hiện các cấu hình sai về quyền, thay vì chỉ dựa vào kiểm toán sau sự kiện.

Sự phát triển của IAM phản ánh nhu cầu ngày càng tăng về bảo mật, khả năng mở rộng và quản trị trong đám mây.

## 7. Kết luận: Hành trình Không ngừng Nắm vững IAM

Chúng ta đã đi qua một hành trình dài, từ các khối xây dựng cơ bản của IAM đến các mẫu hình phức tạp và thực tiễn tốt nhất. Hy vọng rằng lớp học chuyên sâu này đã cung cấp cho bạn sự hiểu biết sâu sắc hơn về dịch vụ nền tảng này.

**Các Nguyên tắc Chính cần Ghi nhớ:**

*   **IAM là Nền tảng:** Bảo mật đám mây của bạn bắt đầu và kết thúc với IAM.
*   **Đặc quyền Tối thiểu là Tối quan trọng:** Luôn đặt mục tiêu cấp quyền hạn ít nhất cần thiết.
*   **Ưu tiên Roles và Liên kết Định danh:** Sử dụng Roles cho ứng dụng/dịch vụ và IAM Identity Center (hoặc liên kết) cho người dùng thay vì IAM Users với thông tin xác thực dài hạn.
*   **Hiểu Logic Đánh giá:** Nắm vững luồng Deny -> Allow -> Implicit Deny và cách các loại chính sách tương tác.
*   **Tận dụng các Công cụ:** Sử dụng Policy Simulator, Access Analyzer, CloudTrail và SCPs một cách hiệu quả.
*   **ABAC cho Khả năng Mở rộng:** Xem xét ABAC khi quản lý quyền hạn ở quy mô lớn.
*   **Bảo mật Root User:** Khóa chặt Root user và không bao giờ sử dụng nó cho các tác vụ hàng ngày.
*   **Kiểm toán và Tinh chỉnh Liên tục:** IAM không phải là "thiết lập và quên đi".

**Bản chất Liên tục của Việc Quản lý Bảo mật và IAM:**

Bối cảnh mối đe dọa, các dịch vụ AWS và các yêu cầu kinh doanh luôn thay đổi. Do đó, việc quản lý IAM là một quá trình liên tục. Bạn cần thường xuyên xem xét lại các chính sách, kiểm toán quyền truy cập, điều chỉnh theo các thực tiễn tốt nhất mới và cập nhật kiến thức của mình về các tính năng IAM mới. Việc thành thạo IAM không phải là đích đến cuối cùng, mà là một hành trình không ngừng nâng cao kỹ năng và cảnh giác.

**Gợi ý để Học thêm:**

*   **Thực hành Thực tế:** Kiến thức lý thuyết là cần thiết, nhưng không gì thay thế được kinh nghiệm thực tế. Hãy thử nghiệm trong tài khoản sandbox: tạo Users, Roles, Policies; cấu hình liên kết; thử nghiệm các điều kiện; sử dụng Policy Simulator và Access Analyzer.
*   **Tài liệu Chính thức của AWS:** Trang tài liệu IAM cực kỳ chi tiết và là nguồn thông tin đáng tin cậy nhất.
*   **AWS Well-Architected Framework - Trụ cột Bảo mật:** Đọc kỹ phần về Quản lý Danh tính và Truy cập.
*   **Whitepapers về Bảo mật AWS:** Tìm kiếm các whitepaper về IAM, Thực tiễn Tốt nhất về Bảo mật, Chiến lược Đa tài khoản.
*   **Các buổi nói chuyện tại AWS re:Invent:** Nhiều buổi nói chuyện chuyên sâu về IAM, ABAC, Liên kết Định danh và các chủ đề bảo mật khác được ghi lại và có sẵn trực tuyến. Tìm kiếm các buổi nói chuyện cấp 300-400.
*   **AWS Security Blog:** Theo dõi blog để cập nhật các tính năng mới và hướng dẫn bảo mật.

Việc nắm vững IAM là một khoản đầu tư đáng giá, mang lại lợi ích to lớn về bảo mật, hiệu quả và sự tự tin khi vận hành trên AWS. Chúc bạn thành công trên hành trình này!

---

*(Lưu ý: Mặc dù lớp học chuyên sâu này cố gắng toàn diện, việc thành thạo thực sự đòi hỏi phải áp dụng những khái niệm này vào thực tế, giải quyết các vấn đề trong thế giới thực và liên tục học hỏi. Hãy sử dụng kiến thức này làm nền tảng vững chắc cho hành trình của bạn.)*