Chắc chắn rồi! Chúng ta sẽ xây dựng một chuỗi bài lab thực hành AWS IAM với Terraform toàn diện, đưa bạn từ những khái niệm cơ bản nhất đến các kỹ thuật nâng cao, giúp bạn tự tin quản lý bảo mật AWS như một chuyên gia hàng đầu.

**Xác nhận Nhiệm vụ:** Tôi đã hiểu yêu cầu. Chúng ta sẽ tạo một chuỗi hướng dẫn lab thực hành chi tiết bằng **Markdown**, sử dụng **Terraform** để quản lý tài nguyên **AWS IAM**. Nội dung hướng dẫn và giải thích sẽ bằng **tiếng Việt**, trong khi mã nguồn Terraform, comment trong code và các thuật ngữ kỹ thuật cốt lõi (IAM, Role, Policy, Terraform, etc.) sẽ giữ nguyên **tiếng Anh**. Trọng tâm sẽ là **sự rõ ràng, tính thực tiễn, các phương pháp bảo mật tốt nhất (hướng tới 2025), khả năng mở rộng**, và đặc biệt là phần **"Giải thích & Lý do"** để hiểu sâu bản chất vấn đề.

**Phác thảo Kế hoạch (Danh sách các Lab):**

1.  **Lab 1: Users, Groups & Managed Policies Cơ bản:** Nền tảng về quản lý danh tính và cấp quyền cơ bản.
2.  **Lab 2: Custom & Inline Policies:** Tạo và quản lý các chính sách tùy chỉnh.
3.  **Lab 3: IAM Roles cho Dịch vụ AWS (EC2):** Cho phép các dịch vụ AWS tương tác an toàn.
4.  **Lab 4: IAM Roles cho Users & Cross-Account Access:** Ủy quyền truy cập cho người dùng và giữa các tài khoản.
5.  **Lab 5: Variables, Conditions & Context Keys:** Xây dựng chính sách động và chi tiết.
6.  **Lab 6: Permissions Boundaries:** Giới hạn quyền tối đa cho IAM entities.
7.  **Lab 7: IAM Identity Providers & Federation Roles:** Tích hợp với nhà cung cấp danh tính bên ngoài (Khái niệm & Setup Role cơ bản).
8.  **Lab 8: Attribute-Based Access Control (ABAC) với Tags:** Kiểm soát truy cập dựa trên thuộc tính.
9.  **Lab 9: Sử dụng IAM Access Analyzer:** Phân tích và xác thực quyền truy cập.
10. **Lab 10: Account Password Policy & MFA Enforcement:** Tăng cường bảo mật đăng nhập.
11. **Lab 11: IAM Identity Center & SCPs Overview:** Hiểu bức tranh lớn về quản lý truy cập trong môi trường multi-account (Lý thuyết).

Chúng ta hãy bắt đầu!

---

# Chuỗi Lab Thực Hành AWS IAM với Terraform

## Giới thiệu tổng quan

Chào mừng bạn đến với chuỗi lab thực hành chuyên sâu về AWS Identity and Access Management (IAM) sử dụng Terraform! Mục tiêu của chuỗi bài này là trang bị cho bạn kiến thức và kỹ năng thực tế để thiết kế, triển khai và quản lý các cấu hình IAM an toàn, hiệu quả và có khả năng mở rộng trên AWS bằng Infrastructure as Code (IaC).

Chúng ta sẽ bắt đầu từ những khái niệm cơ bản nhất như user và group, sau đó đi sâu vào các chủ đề phức tạp hơn như roles, policies nâng cao, federation, kiểm soát truy cập dựa trên thuộc tính (ABAC), và các phương pháp bảo mật tốt nhất. Mỗi bài lab được thiết kế để xây dựng dựa trên bài trước, giúp bạn dần dần làm chủ IAM thông qua thực hành với Terraform.

**Mục tiêu chính:**

*   Hiểu rõ các thành phần cốt lõi của AWS IAM (Users, Groups, Roles, Policies).
*   Sử dụng thành thạo Terraform để định nghĩa và quản lý tài nguyên IAM.
*   Áp dụng nguyên tắc đặc quyền tối thiểu (Principle of Least Privilege) trong mọi cấu hình.
*   Triển khai các mẫu IAM nâng cao như Conditions, Permissions Boundaries, và ABAC.
*   Hiểu các khái niệm về Federation và vai trò của IAM Identity Center.
*   Nắm vững các phương pháp bảo mật tốt nhất cho IAM (MFA, Password Policy, Access Analyzer).
*   Tự tin tự động hóa việc quản lý IAM trong các môi trường AWS thực tế.

**Điều kiện tiên quyết chung:**

*   **Tài khoản AWS:** Bạn cần có quyền truy cập vào một tài khoản AWS với quyền tạo tài nguyên IAM (tốt nhất là tài khoản cá nhân hoặc sandbox).
*   **Terraform đã cài đặt:** Đảm bảo bạn đã cài đặt Terraform CLI trên máy của mình. (Xem [hướng dẫn cài đặt Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)).
*   **AWS CLI đã cấu hình:** AWS CLI cần được cài đặt và cấu hình với thông tin xác thực (access key ID, secret access key, và default region) để Terraform có thể tương tác với tài khoản AWS của bạn. (Xem [hướng dẫn cấu hình AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)). Sử dụng thông tin xác thực của một IAM user có đủ quyền (ví dụ: `AdministratorAccess` *chỉ trong môi trường lab/sandbox*). **Tuyệt đối không sử dụng root user credentials.**
*   **Kiến thức cơ bản về AWS và Terraform:** Hiểu biết cơ bản về các khái niệm AWS (Region, EC2, S3 là một lợi thế) và cú pháp Terraform cơ bản (resources, variables, providers) sẽ giúp bạn dễ dàng theo dõi hơn.

**Lưu ý quan trọng về bảo mật và chi phí:**

*   Luôn tuân thủ **nguyên tắc đặc quyền tối thiểu**. Chỉ cấp các quyền thực sự cần thiết.
*   **Dọn dẹp tài nguyên** sau mỗi lab bằng lệnh `terraform destroy` để tránh phát sinh chi phí không mong muốn và giữ môi trường sạch sẽ.
*   **Bảo vệ thông tin xác thực AWS của bạn.** Không bao giờ đưa access keys vào mã nguồn hoặc chia sẻ công khai. Sử dụng các phương pháp quản lý credentials an toàn.

Hãy bắt đầu hành trình làm chủ AWS IAM với Terraform!

---

## Lab 1: Users, Groups & Managed Policies Cơ bản

### Mục tiêu

*   Tạo một IAM User mới bằng Terraform.
*   Tạo một IAM Group mới bằng Terraform.
*   Thêm User vào Group bằng Terraform.
*   Gán một AWS Managed Policy (ví dụ: `AmazonS3ReadOnlyAccess`) cho Group bằng Terraform.
*   Hiểu khái niệm cơ bản về User, Group và Managed Policy.

### Điều kiện tiên quyết

*   Hoàn thành các điều kiện tiên quyết chung đã nêu ở phần Giới thiệu.

### Khái niệm được đề cập

*   **IAM User:** Một thực thể (entity) bạn tạo trong AWS để đại diện cho người hoặc ứng dụng cần tương tác với các dịch vụ AWS. Users có thông tin xác thực dài hạn (long-term credentials) như mật khẩu (cho truy cập Console) hoặc access keys (cho truy cập programmatically/CLI). **Lưu ý:** Theo các phương pháp tốt nhất hiện đại (2025+), nên ưu tiên sử dụng IAM Roles và IAM Identity Center thay vì IAM Users với long-term credentials khi có thể. Tuy nhiên, hiểu về Users vẫn là nền tảng quan trọng.
*   **IAM Group:** Một tập hợp các IAM Users. Groups cho phép bạn chỉ định quyền cho nhiều user cùng một lúc. Khi bạn thay đổi quyền của group, thay đổi đó sẽ áp dụng cho tất cả user trong group. Đây là cách hiệu quả để quản lý quyền thay vì gán trực tiếp cho từng user.
*   **IAM Policy:** Một tài liệu (thường là JSON) định nghĩa các quyền (permissions). Có hai loại chính:
    *   **Identity-based policies:** Gắn trực tiếp vào một identity (User, Group, hoặc Role). Chúng xác định những hành động identity đó có thể thực hiện trên tài nguyên nào.
    *   **Resource-based policies:** Gắn vào một tài nguyên (ví dụ: S3 bucket policy, SQS queue policy). Chúng xác định principal nào (user, account, service, etc.) được phép truy cập tài nguyên đó.
*   **AWS Managed Policy:** Các policy được tạo và quản lý bởi AWS. Chúng được thiết kế để bao gồm các trường hợp sử dụng phổ biến (ví dụ: quyền chỉ đọc S3, quyền quản trị EC2). Sử dụng Managed Policies giúp đơn giản hóa việc quản lý quyền ban đầu.
*   **Terraform Resources:**
    *   `aws_iam_user`: Tạo và quản lý một IAM user.
    *   `aws_iam_group`: Tạo và quản lý một IAM group.
    *   `aws_iam_group_membership`: Quản lý việc thêm users vào một group.
    *   `aws_iam_policy_attachment`: Gắn một policy (managed hoặc customer-managed) vào một user, group, hoặc role.

### Cấu hình Terraform

Tạo một thư mục mới cho lab này (ví dụ: `lab1-iam-basics`). Bên trong thư mục đó, tạo tệp `main.tf`.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0" # Use a recent AWS provider version
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "us-east-1" # Choose your preferred region
  # Credentials are typically configured via environment variables,
  # shared credentials file (~/.aws/credentials), or EC2 instance profile.
  # Avoid hardcoding credentials in your Terraform code.
}

# Define an IAM User
resource "aws_iam_user" "developer" {
  name = "tf-lab1-developer-user"
  path = "/users/" # Optional: Organizational path for the user

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 1 User"
  }
}

# Define an IAM Group
resource "aws_iam_group" "developers" {
  name = "tf-lab1-developers-group"
  path = "/groups/" # Optional: Organizational path for the group
}

# Add the user to the group
resource "aws_iam_group_membership" "dev_team" {
  name = "tf-lab1-dev-team-membership"

  users = [
    aws_iam_user.developer.name # Reference the user created above
  ]

  group = aws_iam_group.developers.name # Reference the group created above

  # Terraform automatically handles dependencies.
  # It knows it needs to create the user and group before creating the membership.
}

# Attach an AWS Managed Policy to the Group
resource "aws_iam_policy_attachment" "s3_read_only_for_developers" {
  name = "tf-lab1-s3-read-attachment"

  groups = [
    aws_iam_group.developers.name # Attach to the developers group
  ]

  # Find the ARN of the AWS Managed Policy for S3 Read Only Access
  # Common ARNs:
  # - arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
  # - arn:aws:iam::aws:policy/AdministratorAccess (Use with extreme caution!)
  # - arn:aws:iam::aws:policy/PowerUserAccess (Use with caution!)
  policy_arn = "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
}

# Output the User ARN and Group ARN for verification
output "developer_user_arn" {
  value       = aws_iam_user.developer.arn
  description = "The ARN of the created IAM user."
}

output "developers_group_arn" {
  value       = aws_iam_group.developers.arn
  description = "The ARN of the created IAM group."
}

output "attached_policy_arn" {
  value = aws_iam_policy_attachment.s3_read_only_for_developers.policy_arn
  description = "The ARN of the policy attached to the group."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab1-iam-basics
    cd lab1-iam-basics
    touch main.tf
    # Copy the Terraform code above into main.tf
    ```
2.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
    Lệnh này sẽ tải xuống AWS provider plugin.
3.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Lệnh này sẽ hiển thị các tài nguyên mà Terraform sẽ tạo ra. Xem lại kế hoạch để đảm bảo nó khớp với mong đợi (1 user, 1 group, 1 group membership, 1 policy attachment).
4.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Terraform sẽ yêu cầu bạn xác nhận. Nhập `yes` để tiếp tục. Terraform sẽ bắt đầu tạo các tài nguyên IAM trên AWS. Sau khi hoàn tất, nó sẽ hiển thị các giá trị output (User ARN, Group ARN, Policy ARN).
5.  **Xác minh (Bước này thực hiện trong phần Xác minh dưới đây)**

### Giải thích & Lý do

*   **Tại sao dùng Group?** Quản lý quyền qua Group (`aws_iam_group`) thay vì gán trực tiếp cho User (`aws_iam_user`) là một **phương pháp hay nhất**. Khi có nhiều user với cùng bộ quyền, bạn chỉ cần quản lý quyền của Group. Thêm hoặc bớt user khỏi group sẽ tự động cập nhật quyền của họ. Điều này tuân thủ **nguyên tắc DRY (Don't Repeat Yourself)** và dễ dàng **mở rộng (scalability)** hơn.
*   **Tại sao dùng AWS Managed Policy?** Đối với các quyền phổ biến như truy cập chỉ đọc S3 (`AmazonS3ReadOnlyAccess`), sử dụng AWS Managed Policy là cách nhanh chóng và đáng tin cậy. AWS duy trì các policy này. Tuy nhiên, chúng có thể cấp nhiều quyền hơn mức cần thiết tối thiểu. Trong các lab sau, chúng ta sẽ học cách tạo policy tùy chỉnh (custom policies) để tuân thủ chặt chẽ hơn **nguyên tắc đặc quyền tối thiểu**.
*   **`path` là gì?** Thuộc tính `path` trong `aws_iam_user` và `aws_iam_group` cho phép bạn tạo một cấu trúc thư mục ảo để tổ chức các IAM entities. Điều này hữu ích trong các tài khoản lớn có nhiều users/groups/roles. Ví dụ: `/users/team-a/`, `/groups/project-x/`.
*   **`tags` để làm gì?** Gắn thẻ (tagging) tài nguyên (`aws_iam_user` có hỗ trợ tags, `aws_iam_group` thì không trực tiếp nhưng có thể được quản lý gián tiếp) là rất quan trọng cho việc quản lý chi phí, tự động hóa, và kiểm soát truy cập (chúng ta sẽ khám phá ABAC trong lab sau). Tag `ManagedBy = Terraform` giúp nhận biết các tài nguyên được quản lý bởi IaC.
*   **Terraform Dependencies:** Lưu ý cách `aws_iam_group_membership` tham chiếu đến `aws_iam_user.developer.name` và `aws_iam_group.developers.name`. Terraform tự động hiểu rằng user và group phải được tạo *trước* khi membership có thể được tạo. Tương tự, `aws_iam_policy_attachment` phụ thuộc vào group. Đây là một ưu điểm lớn của Terraform - nó quản lý thứ tự tạo tài nguyên.
*   **Bảo mật:** Mặc dù lab này tạo IAM user, hãy nhớ rằng **IAM Identity Center (trước đây là AWS SSO)** là phương pháp được AWS khuyến nghị cho việc quản lý truy cập của người dùng vào tài khoản AWS và ứng dụng, đặc biệt trong môi trường multi-account. Nó tích hợp với các nhà cung cấp danh tính hiện có (như Azure AD, Okta) hoặc có thể hoạt động như một thư mục độc lập, và sử dụng **temporary credentials** thay vì access keys dài hạn, giúp tăng cường bảo mật đáng kể. Chúng ta tạo user ở đây để hiểu các khối xây dựng cơ bản của IAM.

### Xác minh

Bạn có thể xác minh các tài nguyên đã được tạo bằng một trong các cách sau:

1.  **AWS Management Console:**
    *   Đăng nhập vào AWS Management Console.
    *   Điều hướng đến dịch vụ **IAM**.
    *   Trong bảng điều hướng bên trái, chọn **Users**. Bạn sẽ thấy user `tf-lab1-developer-user`. Click vào user đó, vào tab "Groups" để xác nhận user thuộc group `tf-lab1-developers-group`.
    *   Trong bảng điều hướng bên trái, chọn **Groups**. Bạn sẽ thấy group `tf-lab1-developers-group`. Click vào group đó, vào tab "Permissions" để xác nhận policy `AmazonS3ReadOnlyAccess` đã được đính kèm.
2.  **AWS CLI:** (Đảm bảo AWS CLI đã được cấu hình)
    ```bash
    # Liệt kê user và kiểm tra tên
    aws iam list-users --query 'Users[?UserName==`tf-lab1-developer-user`].UserName' --output text

    # Liệt kê group và kiểm tra tên
    aws iam list-groups --query 'Groups[?GroupName==`tf-lab1-developers-group`].GroupName' --output text

    # Liệt kê các group mà user thuộc về
    aws iam list-groups-for-user --user-name tf-lab1-developer-user --query 'Groups[?GroupName==`tf-lab1-developers-group`].GroupName' --output text

    # Liệt kê các policy đính kèm vào group
    aws iam list-attached-group-policies --group-name tf-lab1-developers-group --query 'AttachedPolicies[?PolicyName==`AmazonS3ReadOnlyAccess`].PolicyArn' --output text
    ```
    Các lệnh này sẽ trả về tên hoặc ARN nếu tài nguyên tồn tại và được cấu hình đúng.

### Dọn dẹp

Rất quan trọng là phải dọn dẹp các tài nguyên bạn đã tạo để tránh chi phí không cần thiết và giữ cho tài khoản AWS của bạn sạch sẽ.

1.  **Chạy lệnh destroy:**
    Trong thư mục `lab1-iam-basics`, chạy lệnh:
    ```bash
    terraform destroy
    ```
2.  **Xác nhận:** Terraform sẽ hiển thị các tài nguyên sẽ bị xóa. Xem lại danh sách và nhập `yes` để xác nhận.
3.  **Xác minh dọn dẹp (Tùy chọn):** Sử dụng AWS Management Console hoặc AWS CLI (như các lệnh ở phần Xác minh) để kiểm tra xem user, group và các liên kết đã bị xóa hay chưa. Các lệnh CLI bây giờ sẽ trả về kết quả rỗng hoặc lỗi "NotFound".

---

Chúc mừng! Bạn đã hoàn thành Lab 1, tạo thành công các thành phần IAM cơ bản bằng Terraform. Trong lab tiếp theo, chúng ta sẽ tìm hiểu cách tạo các chính sách tùy chỉnh để kiểm soát quyền truy cập chi tiết hơn.
## Lab 2: Custom & Inline Policies

### Mục tiêu

*   Tạo một IAM Policy tùy chỉnh (Customer Managed Policy) bằng Terraform để cấp quyền truy cập S3 cụ thể.
*   Gắn Policy tùy chỉnh này vào IAM Group đã tạo ở Lab 1 (hoặc tạo group mới).
*   Tạo một IAM User mới với một Inline Policy để cấp quyền EC2 cụ thể.
*   Hiểu sự khác biệt, ưu và nhược điểm của Managed Policies, Customer Managed Policies, và Inline Policies.

### Điều kiện tiên quyết

*   Hoàn thành Lab 1 hoặc có sẵn cấu hình Terraform cơ bản với provider AWS.
*   Hiểu cấu trúc cơ bản của một tài liệu JSON IAM Policy.

### Khái niệm được đề cập

*   **Customer Managed Policy:** IAM policy mà bạn tạo và quản lý trong tài khoản AWS của mình. Chúng cung cấp khả năng kiểm soát chi tiết hơn so với AWS Managed Policies. Bạn có thể tạo các policy này hoàn toàn theo **nguyên tắc đặc quyền tối thiểu**, chỉ cấp những quyền thực sự cần thiết. Chúng có thể được tái sử dụng và gắn vào nhiều principal (users, groups, roles).
*   **Inline Policy:** Một policy được nhúng trực tiếp vào một principal duy nhất (user, group, hoặc role). Có một mối quan hệ 1-1 chặt chẽ giữa principal và inline policy. Khi bạn xóa principal, inline policy cũng bị xóa theo. Inline policies hữu ích khi bạn muốn chắc chắn rằng các quyền không vô tình bị gắn vào một principal khác. Tuy nhiên, chúng khó quản lý hơn ở quy mô lớn vì không thể tái sử dụng và phải cập nhật riêng lẻ cho từng principal.
*   **IAM Policy JSON Structure:** Hiểu các thành phần chính của tài liệu policy JSON:
    *   `Version`: Phiên bản ngôn ngữ policy (thường là `2012-10-17`).
    *   `Statement`: Một danh sách chứa một hoặc nhiều câu lệnh (statement).
    *   `Sid` (Statement ID): (Tùy chọn) Mã định danh cho câu lệnh, hữu ích cho việc gỡ lỗi và quản lý.
    *   `Effect`: `Allow` hoặc `Deny`. Quyết định xem câu lệnh cho phép hay từ chối quyền.
    *   `Principal`: (Chỉ dùng trong resource-based policies hoặc assume role policies) Chỉ định tài khoản, user, role, hoặc dịch vụ được phép hoặc bị từ chối truy cập.
    *   `Action`: Danh sách các hành động API của dịch vụ mà policy cho phép hoặc từ chối (ví dụ: `s3:GetObject`, `ec2:DescribeInstances`). Có thể sử dụng ký tự đại diện (`*`).
    *   `Resource`: Danh sách các tài nguyên AWS mà hành động áp dụng tới, được xác định bằng ARN (Amazon Resource Name) hoặc ký tự đại diện (`*`).
    *   `Condition`: (Tùy chọn) Các điều kiện phải được đáp ứng để policy có hiệu lực (sẽ khám phá trong Lab 5).
*   **Terraform Resources:**
    *   `aws_iam_policy`: Tạo và quản lý một Customer Managed Policy.
    *   `aws_iam_policy_attachment`: (Đã sử dụng ở Lab 1) Gắn một policy (managed hoặc customer-managed) vào principal.
    *   `aws_iam_user_policy` / `aws_iam_group_policy` / `aws_iam_role_policy`: Tạo và quản lý một Inline Policy cho user/group/role tương ứng.
    *   `data "aws_iam_policy_document"`: Một cách tiện lợi để xây dựng tài liệu policy JSON trong Terraform HCL một cách có cấu trúc và dễ đọc hơn, thay vì viết chuỗi JSON trực tiếp.

### Cấu hình Terraform

Tạo thư mục `lab2-custom-policies`. Bạn có thể sao chép `main.tf` từ Lab 1 làm điểm khởi đầu và sửa đổi nó, hoặc tạo mới. Chúng ta sẽ tạo một bucket S3 giả định (chỉ tên thôi, không cần tạo bucket thật sự cho lab IAM này) để sử dụng trong policy.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured via environment variables or shared file
}

# --- Phần 1: Customer Managed Policy ---

# Define a variable for the bucket name (makes policy reusable)
variable "target_bucket_name" {
  description = "The name of the S3 bucket for specific access."
  type        = string
  default     = "my-lab2-unique-bucket-12345" # Replace with a unique name idea if needed
}

# Use a data source to construct the policy document (cleaner than raw JSON)
data "aws_iam_policy_document" "s3_list_and_read_specific_bucket" {
  statement {
    sid = "ListSpecificBucket"
    actions = [
      "s3:ListBucket",
    ]
    resources = [
      "arn:aws:s3:::${var.target_bucket_name}" # ARN for the bucket itself
    ]
    effect = "Allow"
  }

  statement {
    sid = "ReadObjectsInSpecificBucket"
    actions = [
      "s3:GetObject"
    ]
    resources = [
      "arn:aws:s3:::${var.target_bucket_name}/*" # ARN for objects within the bucket
    ]
    effect = "Allow"
  }
}

# Create the Customer Managed Policy
resource "aws_iam_policy" "s3_list_read_custom_policy" {
  name        = "tf-lab2-S3ListRead-${var.target_bucket_name}-Policy"
  path        = "/custom-policies/"
  description = "Allows listing and reading objects in a specific S3 bucket"

  # Use the JSON output from the data source
  policy = data.aws_iam_policy_document.s3_list_and_read_specific_bucket.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 2 Custom S3 Policy"
  }
}

# Create a group (or reuse one from Lab 1 if you adapt the code)
resource "aws_iam_group" "data_analysts" {
  name = "tf-lab2-data-analysts-group"
  path = "/groups/"
}

# Attach the Custom Managed Policy to the Group
resource "aws_iam_policy_attachment" "attach_custom_s3_to_analysts" {
  name = "tf-lab2-s3-custom-attach"

  groups = [
    aws_iam_group.data_analysts.name
  ]

  policy_arn = aws_iam_policy.s3_list_read_custom_policy.arn # Reference the custom policy ARN
}

# --- Phần 2: Inline Policy ---

# Create a new user specifically for EC2 tasks
resource "aws_iam_user" "ec2_operator" {
  name = "tf-lab2-ec2-operator-user"
  path = "/users/operators/"

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 2 User with Inline Policy"
  }
}

# Use a data source for the inline policy document
data "aws_iam_policy_document" "ec2_describe_only" {
  statement {
    sid    = "AllowDescribeEC2"
    actions = [
      "ec2:DescribeInstances",
      "ec2:DescribeVolumes",
      "ec2:DescribeNetworkInterfaces"
      # Add other Describe* actions as needed, following least privilege
    ]
    resources = ["*"] # Describe actions often require "*" resource, but can sometimes be restricted by region/account
    effect    = "Allow"
  }
}

# Create the Inline Policy and attach it directly to the user
resource "aws_iam_user_policy" "ec2_describe_inline" {
  name = "tf-lab2-ec2-describe-inline-policy"
  user = aws_iam_user.ec2_operator.name # Associate with the ec2_operator user

  # Use the JSON output from the data source
  policy = data.aws_iam_policy_document.ec2_describe_only.json
}

# --- Outputs ---
output "custom_policy_arn" {
  value       = aws_iam_policy.s3_list_read_custom_policy.arn
  description = "The ARN of the created Customer Managed Policy."
}

output "analyst_group_name" {
  value       = aws_iam_group.data_analysts.name
  description = "The name of the data analysts group."
}

output "ec2_operator_user_name" {
  value       = aws_iam_user.ec2_operator.name
  description = "The name of the EC2 operator user."
}

output "inline_policy_name_on_user" {
  value = aws_iam_user_policy.ec2_describe_inline.name
   description = "The name of the inline policy attached to the EC2 operator user."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab2-custom-policies
    cd lab2-custom-policies
    touch main.tf variables.tf # Optional: put variable definition in variables.tf
    # Copy the Terraform code into the appropriate files
    ```
2.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
3.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Xem lại các tài nguyên sẽ được tạo: 1 Customer Managed Policy, 1 Group, 1 Policy Attachment, 1 User, 1 User Inline Policy.
4.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes` để xác nhận. Terraform sẽ tạo các tài nguyên.
5.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Customer Managed Policy vs AWS Managed Policy:**
    *   **Ưu điểm Customer Managed:** Kiểm soát chi tiết (Least Privilege), dễ quản lý phiên bản (bạn kiểm soát cập nhật), có thể tái sử dụng trong tài khoản của bạn.
    *   **Nhược điểm Customer Managed:** Bạn phải tự định nghĩa và duy trì policy.
    *   **Khi nào dùng:** Khi bạn cần quyền hạn chế hơn những gì AWS Managed Policy cung cấp, hoặc khi bạn muốn quản lý chặt chẽ các phiên bản policy. **Đây là lựa chọn ưu tiên cho các quyền tùy chỉnh theo nguyên tắc đặc quyền tối thiểu.**
*   **Inline Policy vs Managed Policy (AWS hoặc Customer):**
    *   **Ưu điểm Inline:** Đảm bảo quyền chỉ áp dụng cho *một* principal cụ thể, không thể vô tình tái sử dụng ở nơi khác. Hữu ích cho các trường hợp đặc biệt, dùng một lần.
    *   **Nhược điểm Inline:** Khó quản lý ở quy mô lớn (không tái sử dụng, phải cập nhật từng nơi), giới hạn kích thước policy, không hỗ trợ phiên bản.
    *   **Khi nào dùng:** Sử dụng một cách **hạn chế** cho các bộ quyền rất cụ thể, không có khả năng tái sử dụng, và bạn muốn đảm bảo chúng gắn chặt với một principal duy nhất. **Nói chung, nên ưu tiên Customer Managed Policies hơn Inline Policies** vì khả năng quản lý và tái sử dụng tốt hơn.
*   **`data "aws_iam_policy_document"`:** Sử dụng data source này giúp mã Terraform dễ đọc và bảo trì hơn so với việc viết một chuỗi JSON dài trực tiếp trong thuộc tính `policy`. Nó cho phép bạn sử dụng các biến Terraform (`var.target_bucket_name`) và các hàm nội suy khác bên trong cấu trúc policy.
*   **Policy S3 chi tiết:** Lưu ý cách chúng ta cấp `s3:ListBucket` trên chính bucket ARN (`arn:aws:s3:::${var.target_bucket_name}`) và `s3:GetObject` trên các đối tượng *bên trong* bucket (`arn:aws:s3:::${var.target_bucket_name}/*`). Đây là cách tiếp cận **đặc quyền tối thiểu** tiêu chuẩn cho quyền đọc S3 bucket. Chúng ta không cấp `s3:*` hay quyền truy cập vào các bucket khác.
*   **Policy EC2 `Resource: "*"`:** Nhiều hành động `ec2:Describe*` yêu cầu `Resource: "*"` vì chúng trả về thông tin về các tài nguyên trên toàn bộ region hoặc tài khoản mà không thể lọc trước ở cấp độ API theo ARN cụ thể. Tuy nhiên, hãy luôn kiểm tra tài liệu AWS xem hành động có hỗ trợ giới hạn tài nguyên hoặc sử dụng `Condition` keys để hạn chế phạm vi hay không.
*   **Scalability:** Customer Managed Policies dễ mở rộng hơn nhiều so với Inline Policies vì chúng có thể được đính kèm vào nhiều group/role. Việc cập nhật policy chỉ cần thực hiện ở một nơi duy nhất.

### Xác minh

1.  **AWS Management Console:**
    *   Đi đến **IAM**.
    *   **Policies:** Tìm policy `tf-lab2-S3ListRead-...-Policy`. Click vào nó, xem tab "Permissions" để kiểm tra JSON policy. Xem tab "Entities attached" để thấy nó được gắn vào group `tf-lab2-data-analysts-group`.
    *   **Groups:** Tìm group `tf-lab2-data-analysts-group`. Click vào nó, xem tab "Permissions" để thấy custom policy đã được gắn.
    *   **Users:** Tìm user `tf-lab2-ec2-operator-user`. Click vào nó, xem tab "Permissions". Bạn sẽ thấy inline policy `tf-lab2-ec2-describe-inline-policy` được liệt kê trực tiếp ở đây (thường có thể mở rộng để xem JSON).
2.  **AWS CLI:**
    ```bash
    # Lấy ARN của custom policy (sử dụng output của terraform hoặc tìm theo tên)
    POLICY_ARN=$(terraform output -raw custom_policy_arn) # Hoặc dùng aws iam list-policies ...

    # Xem nội dung policy JSON
    aws iam get-policy --policy-arn $POLICY_ARN
    aws iam get-policy-version --policy-arn $POLICY_ARN --version-id v1 # Lấy phiên bản mặc định

    # Liệt kê các entity được gắn policy này
    aws iam list-entities-for-policy --policy-arn $POLICY_ARN

    # Liệt kê các policy gắn vào group
    aws iam list-attached-group-policies --group-name tf-lab2-data-analysts-group

    # Lấy inline policy của user
    aws iam get-user-policy --user-name tf-lab2-ec2-operator-user --policy-name tf-lab2-ec2-describe-inline-policy --query 'PolicyDocument' --output json | jq
    # (jq là công cụ tùy chọn để định dạng JSON)
    ```

### Dọn dẹp

1.  **Chạy lệnh destroy:**
    Trong thư mục `lab2-custom-policies`, chạy:
    ```bash
    terraform destroy
    ```
2.  **Xác nhận:** Nhập `yes`.
3.  **Xác minh dọn dẹp (Tùy chọn):** Sử dụng Console hoặc CLI để đảm bảo các policy, user, group đã bị xóa.

---

Tuyệt vời! Bạn đã học cách tạo cả Customer Managed Policies và Inline Policies bằng Terraform, hiểu được sự khác biệt và trường hợp sử dụng của chúng. Đây là một bước quan trọng để triển khai nguyên tắc đặc quyền tối thiểu. Lab tiếp theo sẽ giới thiệu về IAM Roles, một khái niệm cốt lõi để cấp quyền cho các dịch vụ AWS và người dùng một cách an toàn.
## Lab 3: IAM Roles cho Dịch vụ AWS (EC2)

### Mục tiêu

*   Tạo một IAM Role mà dịch vụ EC2 có thể đảm nhận (assume).
*   Định nghĩa Trust Policy (Chính sách Tin cậy) cho phép dịch vụ EC2 assume role này.
*   Tạo một Customer Managed Policy cấp quyền truy cập đọc vào một bucket S3 (tương tự Lab 2).
*   Gắn policy này vào IAM Role.
*   Tạo một EC2 Instance Profile liên kết với IAM Role.
*   Hiểu cách EC2 instances sử dụng Instance Profiles và IAM Roles để nhận thông tin xác thực tạm thời và tương tác với các dịch vụ AWS khác một cách an toàn, mà không cần lưu trữ access keys trên instance.

### Điều kiện tiên quyết

*   Hoàn thành Lab 1 và Lab 2 hoặc hiểu rõ về IAM Policies và Terraform cơ bản.
*   Kiến thức cơ bản về Amazon EC2 (không cần khởi chạy instance thực tế trong lab này, nhưng hiểu khái niệm Instance Profile là quan trọng).

### Khái niệm được đề cập

*   **IAM Role:** Tương tự như IAM User ở chỗ nó là một danh tính AWS với các chính sách quyền (permission policies) xác định những gì danh tính có thể và không thể làm trong AWS. Tuy nhiên, thay vì được liên kết duy nhất với một người hoặc ứng dụng, Role được **đảm nhận (assumed)** bởi bất kỳ ai cần nó (người dùng, ứng dụng, hoặc dịch vụ AWS như EC2). Roles không có thông tin xác thực dài hạn (mật khẩu hoặc access keys). Thay vào đó, khi một principal assume role, AWS Security Token Service (STS) cung cấp cho principal đó **thông tin xác thực bảo mật tạm thời (temporary security credentials)**.
*   **Trust Policy (Assume Role Policy):** Một phần *bắt buộc* của IAM Role. Đây là một tài liệu JSON đặc biệt xác định các **principal** (tài khoản, user, role, hoặc dịch vụ AWS) được **tin cậy** để đảm nhận (assume) role đó. Nó sử dụng action `sts:AssumeRole`.
*   **Permission Policy:** Các policy (AWS Managed hoặc Customer Managed) được gắn vào Role để xác định những hành động API AWS và tài nguyên mà principal có thể truy cập *sau khi* đã assume role thành công.
*   **AWS Service Principal:** Một định danh cho một dịch vụ AWS (ví dụ: `ec2.amazonaws.com`, `lambda.amazonaws.com`). Khi bạn chỉ định một Service Principal trong Trust Policy của một Role, bạn cho phép dịch vụ đó assume role này.
*   **EC2 Instance Profile:** Một bộ chứa (container) cho một IAM Role mà bạn có thể gắn vào một EC2 instance khi khởi chạy. Instance Profile chuyển tiếp thông tin Role đến EC2 instance, cho phép các ứng dụng chạy trên instance đó lấy thông tin xác thực tạm thời từ EC2 metadata service và thực hiện các lệnh gọi API AWS được cho phép bởi Role. **Đây là cách bảo mật và được khuyến nghị để cấp quyền cho ứng dụng chạy trên EC2.**
*   **Terraform Resources:**
    *   `aws_iam_role`: Tạo và quản lý một IAM Role, bao gồm cả Trust Policy (`assume_role_policy`).
    *   `aws_iam_policy`: (Đã sử dụng) Tạo Customer Managed Policy.
    *   `aws_iam_role_policy_attachment`: Gắn một policy (managed hoặc customer-managed) vào một Role.
    *   `aws_iam_instance_profile`: Tạo một Instance Profile và liên kết nó với một IAM Role.

### Cấu hình Terraform

Tạo thư mục `lab3-ec2-role`.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured
}

# Define a variable for the target bucket name
variable "target_bucket_name" {
  description = "The name of the S3 bucket EC2 needs read access to."
  type        = string
  default     = "my-lab3-unique-bucket-67890"
}

# --- Step 1: Define the Permissions Policy ---
# Policy defining what the role CAN DO after being assumed
data "aws_iam_policy_document" "s3_read_specific_bucket_policy_doc" {
  statement {
    sid = "AllowS3ReadSpecificBucket"
    actions = [
      "s3:GetObject",
      "s3:ListBucket"
    ]
    resources = [
      "arn:aws:s3:::${var.target_bucket_name}",
      "arn:aws:s3:::${var.target_bucket_name}/*",
    ]
    effect = "Allow"
  }
}

resource "aws_iam_policy" "s3_read_policy" {
  name        = "tf-lab3-S3Read-${var.target_bucket_name}-Policy"
  path        = "/custom-policies/"
  description = "Allows reading from a specific S3 bucket"
  policy      = data.aws_iam_policy_document.s3_read_specific_bucket_policy_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 3 EC2 Role Permissions"
  }
}

# --- Step 2: Define the IAM Role and its Trust Policy ---
# Trust Policy defines WHO CAN assume the role
data "aws_iam_policy_document" "ec2_assume_role_policy_doc" {
  statement {
    actions = ["sts:AssumeRole"]
    principals {
      type        = "Service"
      identifiers = ["ec2.amazonaws.com"] # Allows EC2 service to assume this role
    }
    effect = "Allow"
  }
}

resource "aws_iam_role" "ec2_s3_read_role" {
  name = "tf-lab3-ec2-s3-read-role"
  path = "/service-roles/"

  # Attach the Trust Policy
  assume_role_policy = data.aws_iam_policy_document.ec2_assume_role_policy_doc.json

  description = "Role assumed by EC2 instances to read from a specific S3 bucket"

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 3 EC2 Role"
  }
}

# --- Step 3: Attach the Permissions Policy to the Role ---
resource "aws_iam_role_policy_attachment" "attach_s3_read_to_ec2_role" {
  role       = aws_iam_role.ec2_s3_read_role.name
  policy_arn = aws_iam_policy.s3_read_policy.arn
}

# --- Step 4: Create the Instance Profile ---
# Instance Profile acts as a container for the Role to be attached to EC2 instances
resource "aws_iam_instance_profile" "ec2_s3_read_profile" {
  name = "tf-lab3-ec2-s3-read-profile"
  role = aws_iam_role.ec2_s3_read_role.name # Link the profile to the role

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 3 EC2 Instance Profile"
  }
}

# --- Outputs ---
output "ec2_role_arn" {
  value       = aws_iam_role.ec2_s3_read_role.arn
  description = "The ARN of the created EC2 IAM Role."
}

output "instance_profile_arn" {
  value       = aws_iam_instance_profile.ec2_s3_read_profile.arn
  description = "The ARN of the created EC2 Instance Profile."
}

output "instance_profile_name" {
  value       = aws_iam_instance_profile.ec2_s3_read_profile.name
  description = "The Name of the created EC2 Instance Profile (used when launching EC2)."
}

output "attached_policy_arn" {
  value = aws_iam_policy.s3_read_policy.arn
   description = "The ARN of the permissions policy attached to the role."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab3-ec2-role
    cd lab3-ec2-role
    touch main.tf variables.tf # Optional
    # Copy the Terraform code into the appropriate files
    ```
2.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
3.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Xem lại các tài nguyên: 1 IAM Policy, 1 IAM Role (với Trust Policy), 1 Role Policy Attachment, 1 Instance Profile.
4.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes` để xác nhận.
5.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Tại sao dùng IAM Role cho EC2?** Đây là **phương pháp bảo mật tốt nhất tuyệt đối** để cấp quyền cho các ứng dụng chạy trên EC2.
    *   **Tránh lưu trữ access keys:** Bạn không cần phải nhúng AWS access keys (là thông tin xác thực dài hạn và nhạy cảm) vào mã nguồn ứng dụng hoặc lưu trữ chúng trong các tệp cấu hình trên instance. Việc lộ lọt access keys là một rủi ro bảo mật nghiêm trọng.
    *   **Thông tin xác thực tạm thời:** Role cung cấp thông tin xác thực tạm thời (temporary credentials) thông qua dịch vụ metadata của EC2 (IMDS). Các thông tin này tự động hết hạn và được xoay vòng (rotated), giảm thiểu đáng kể rủi ro nếu chúng bị lộ.
    *   **Quản lý tập trung:** Bạn quản lý quyền của ứng dụng bằng cách cập nhật policy gắn vào Role, thay vì phải cập nhật cấu hình trên từng instance.
*   **Trust Policy là chìa khóa:** Phần `assume_role_policy` (Trust Policy) là cực kỳ quan trọng. Nó định nghĩa *ai* được phép yêu cầu thông tin xác thực tạm thời của Role này. Trong trường hợp này, `Service: ec2.amazonaws.com` chỉ cho phép chính dịch vụ EC2 (thông qua IMDS trên instance có gắn Instance Profile) thực hiện hành động `sts:AssumeRole` đối với Role này. Nếu Trust Policy sai, EC2 sẽ không thể assume role.
*   **Permission Policy:** Chính sách này (ví dụ: `s3_read_policy`) xác định những gì Role có thể làm *sau khi* được EC2 assume thành công. Nó tuân theo cùng cấu trúc và nguyên tắc (đặc quyền tối thiểu) như các policy chúng ta đã thấy ở Lab 2.
*   **Instance Profile:** Instance Profile là cầu nối giữa EC2 instance và IAM Role. Khi bạn khởi chạy một EC2 instance và chọn một Instance Profile, AWS sẽ đảm bảo rằng instance đó có thể truy cập metadata service để lấy thông tin xác thực tạm thời liên kết với Role trong Profile đó. **Tên của Instance Profile và tên của Role không nhất thiết phải giống nhau**, mặc dù Terraform resource `aws_iam_instance_profile` mặc định tạo ra profile cùng tên với role nếu bạn chỉ cung cấp `name` hoặc `name_prefix`. Trong ví dụ này, chúng ta đặt tên rõ ràng cho cả hai.
*   **Tách biệt mối quan tâm (Separation of Concerns):** Cấu trúc Role (Trust Policy + Permission Policies) thể hiện rõ sự tách biệt:
    *   Trust Policy: Ai có thể *trở thành* Role này?
    *   Permission Policies: Role này có thể *làm gì*?
*   **Scalability:** Sử dụng Roles và Instance Profiles cho phép bạn dễ dàng áp dụng cùng một bộ quyền cho hàng trăm hoặc hàng nghìn EC2 instances chỉ bằng cách gắn cùng một Instance Profile khi khởi chạy chúng.

### Xác minh

1.  **AWS Management Console:**
    *   Đi đến **IAM**.
    *   **Roles:** Tìm role `tf-lab3-ec2-s3-read-role`.
        *   Click vào role.
        *   Xem tab "Permissions": Xác nhận policy `tf-lab3-S3Read-...-Policy` được gắn.
        *   Xem tab "Trust relationships": Xác nhận Principal là `ec2.amazonaws.com`.
    *   Trong IAM, tìm mục **Instance Profiles** (có thể nằm dưới Roles hoặc là mục riêng tùy giao diện Console). Tìm profile `tf-lab3-ec2-s3-read-profile`. Xác nhận nó liên kết với role `tf-lab3-ec2-s3-read-role`.
2.  **AWS CLI:**
    ```bash
    # Lấy thông tin Role
    aws iam get-role --role-name tf-lab3-ec2-s3-read-role

    # Kiểm tra Trust Policy (Assume Role Policy Document)
    aws iam get-role --role-name tf-lab3-ec2-s3-read-role --query 'Role.AssumeRolePolicyDocument'

    # Liệt kê các policy gắn vào Role
    aws iam list-attached-role-policies --role-name tf-lab3-ec2-s3-read-role

    # Lấy thông tin Instance Profile
    aws iam get-instance-profile --instance-profile-name tf-lab3-ec2-s3-read-profile

    # Kiểm tra xem Role nào được liên kết với Instance Profile
    aws iam get-instance-profile --instance-profile-name tf-lab3-ec2-s3-read-profile --query 'InstanceProfile.Roles[0].RoleName' --output text
    ```

*(Tùy chọn nâng cao):* Nếu bạn muốn kiểm tra thực tế, bạn có thể:
1.  Khởi chạy một EC2 instance (ví dụ: Amazon Linux 2).
2.  Khi cấu hình instance, chọn Instance Profile `tf-lab3-ec2-s3-read-profile` trong phần IAM Role.
3.  SSH vào instance sau khi khởi chạy.
4.  Cài đặt AWS CLI trên instance (`sudo yum install aws-cli -y`).
5.  Thử chạy lệnh AWS CLI để tương tác với S3 (ví dụ: `aws s3 ls s3://<your-target-bucket-name>`). Nếu bucket tồn tại và Role được cấu hình đúng, lệnh sẽ thành công mà không cần bạn cấu hình credentials thủ công trên instance. Nếu bạn thử truy cập bucket khác hoặc thực hiện hành động khác (như `aws s3 ls`), nó sẽ bị từ chối theo policy. *Nhớ tạo bucket S3 với tên đã định nghĩa trong biến `target_bucket_name` để kiểm tra thực tế này.*

### Dọn dẹp

1.  *(Nếu bạn đã khởi chạy EC2 instance để kiểm tra, hãy chấm dứt nó trước)*
2.  **Chạy lệnh destroy:**
    Trong thư mục `lab3-ec2-role`, chạy:
    ```bash
    terraform destroy
    ```
3.  **Xác nhận:** Nhập `yes`.
4.  **Xác minh dọn dẹp (Tùy chọn):** Sử dụng Console hoặc CLI.

---

Xuất sắc! Bạn đã tạo thành công một IAM Role cho dịch vụ EC2, bao gồm Trust Policy, Permission Policy và Instance Profile, tất cả bằng Terraform. Bạn đã hiểu cách cung cấp quyền truy cập an toàn cho các dịch vụ AWS mà không cần hardcode credentials. Lab tiếp theo sẽ khám phá một loại Role quan trọng khác: Role dành cho người dùng (User Assumed Roles) và cách sử dụng chúng cho truy cập liên tài khoản (cross-account access).
## Lab 4: IAM Roles cho Users & Cross-Account Access

### Mục tiêu

*   Tạo một IAM Role mà một IAM User (trong cùng tài khoản hoặc tài khoản khác) có thể đảm nhận (assume).
*   Định nghĩa Trust Policy cho phép một IAM User cụ thể (hoặc tất cả user trong một tài khoản) assume role này.
*   Gắn một policy (ví dụ: quyền đọc EC2) vào Role để xác định quyền hạn sau khi assume.
*   Hiểu cách người dùng có thể chuyển đổi (switch) sang Role này bằng AWS Management Console hoặc AWS CLI.
*   Giới thiệu khái niệm cross-account access bằng cách cho phép một tài khoản AWS khác assume Role.

### Điều kiện tiên quyết

*   Hoàn thành các Lab trước, đặc biệt là Lab 3 về khái niệm Role và Trust Policy.
*   Có một IAM User (có thể là user bạn dùng để chạy Terraform hoặc một user khác) để thử nghiệm việc assume role.
*   (Tùy chọn, cho phần cross-account): Có quyền truy cập vào một tài khoản AWS thứ hai hoặc biết Account ID của tài khoản đó.

### Khái niệm được đề cập

*   **User Assumed Role:** Một IAM Role được thiết kế để người dùng (IAM Users hoặc người dùng liên kết - federated users) đảm nhận nhằm có được một bộ quyền khác với quyền mặc định của họ, thường là trong một khoảng thời gian tạm thời. Điều này rất hữu ích cho việc:
    *   **Nâng quyền tạm thời (Privilege Escalation):** Cho phép user thực hiện các tác vụ nhạy cảm mà không cần cấp quyền đó vĩnh viễn cho user chính của họ.
    *   **Phân chia nhiệm vụ (Task Segregation):** Tạo các roles với bộ quyền cụ thể cho các nhiệm vụ khác nhau (ví dụ: role quản trị CSDL, role quản trị mạng).
    *   **Truy cập liên tài khoản (Cross-Account Access):** Cho phép user ở Tài khoản A assume một Role ở Tài khoản B để truy cập tài nguyên trong Tài khoản B.
*   **`sts:AssumeRole` Action:** Hành động cốt lõi được sử dụng trong Trust Policy để cho phép một principal đảm nhận role.
*   **Principal trong Trust Policy:** Đối với user-assumed roles:
    *   **IAM User:** Chỉ định ARN của một IAM User cụ thể (`arn:aws:iam::<ACCOUNT_ID>:user/<USER_NAME>`).
    *   **AWS Account:** Chỉ định ARN của một tài khoản AWS (`arn:aws:iam::<ACCOUNT_ID>:root`) hoặc chỉ Account ID (`<ACCOUNT_ID>`). Điều này cho phép *bất kỳ* principal nào trong tài khoản được chỉ định (nếu họ có quyền `sts:AssumeRole` đối với Role này) được phép assume role. Đây là cách phổ biến nhất cho cross-account access.
    *   **IAM Role:** Một role có thể được phép assume một role khác (role chaining).
*   **Switching Roles:** Quá trình một user sử dụng thông tin xác thực của mình để yêu cầu thông tin xác thực tạm thời cho một Role khác thông qua `sts:AssumeRole`. Có thể thực hiện qua Console (menu "Switch Role") hoặc CLI (`aws sts assume-role` và sau đó cấu hình profile mới với credentials tạm thời).
*   **Cross-Account Access:** Kịch bản trong đó một principal (user/role) trong Tài khoản A được cấp quyền để assume một Role trong Tài khoản B. Role trong Tài khoản B phải có Trust Policy tin cậy Tài khoản A (hoặc principal cụ thể trong Tài khoản A). Principal trong Tài khoản A cũng cần có policy cho phép thực hiện `sts:AssumeRole` đối với Role trong Tài khoản B.
*   **Terraform Resources:**
    *   `aws_iam_role`: (Như Lab 3) Dùng để tạo Role.
    *   `aws_iam_policy`: (Như Lab 2) Dùng để tạo Permission Policy.
    *   `aws_iam_role_policy_attachment`: (Như Lab 3) Dùng để gắn Permission Policy vào Role.
    *   `data "aws_caller_identity"`: Một data source hữu ích để lấy thông tin về principal đang thực thi Terraform (Account ID, User ID, ARN).

### Cấu hình Terraform

Tạo thư mục `lab4-user-roles`.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured
}

# --- Configuration Variables ---
variable "target_account_id_for_cross_account" {
  description = "The AWS Account ID that should be trusted for cross-account access (Optional). If empty, only users in the current account can assume the role."
  type        = string
  default     = "" # Keep empty for same-account access initially
  # To enable cross-account, replace "" with the actual Account ID of the trusting account.
  # Example: default = "111122223333"
}

variable "specific_user_arn_to_trust" {
  description = "The ARN of a specific IAM User allowed to assume this role (Optional). If empty, any user in the trusted account(s) with sts:AssumeRole permission can assume."
  type        = string
  default     = "" # Example: default = "arn:aws:iam::123456789012:user/my-test-user"
}

# Get information about the caller (useful for constructing trust policy)
data "aws_caller_identity" "current" {}

# --- Step 1: Define the Permissions Policy for the Role ---
# What the role can do *after* being assumed
data "aws_iam_policy_document" "ec2_read_only_policy_doc" {
  statement {
    sid    = "AllowEC2ReadOnly"
    actions = [
      "ec2:DescribeInstances",
      "ec2:DescribeVolumes",
      "ec2:DescribeSecurityGroups",
      "ec2:DescribeImages",
      "ec2:DescribeSubnets",
      "ec2:DescribeVpcs"
      # Add other read-only EC2 actions as needed
    ]
    resources = ["*"] # Read-only actions often need '*'
    effect    = "Allow"
  }
}

resource "aws_iam_policy" "ec2_read_only_policy" {
  name        = "tf-lab4-EC2ReadOnlyPolicy"
  path        = "/custom-policies/"
  description = "Allows read-only access to EC2 resources"
  policy      = data.aws_iam_policy_document.ec2_read_only_policy_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 4 User Role Permissions"
  }
}

# --- Step 2: Define the IAM Role and its Trust Policy ---
# Trust Policy defines WHO CAN assume the role
data "aws_iam_policy_document" "user_assume_role_policy_doc" {
  statement {
    actions = ["sts:AssumeRole"]
    effect  = "Allow"

    # Dynamically build the principals list based on input variables
    dynamic "principals" {
      # If a specific user ARN is provided, trust only that user
      for_each = var.specific_user_arn_to_trust != "" ? [var.specific_user_arn_to_trust] : []
      content {
        type        = "AWS"
        identifiers = [principals.value] # The specific user ARN
      }
    }

    dynamic "principals" {
      # If no specific user is set AND no cross-account ID is set, trust the current account root
      for_each = var.specific_user_arn_to_trust == "" && var.target_account_id_for_cross_account == "" ? [data.aws_caller_identity.current.account_id] : []
      content {
        type        = "AWS"
        identifiers = ["arn:aws:iam::${principals.value}:root"] # Trust any principal in the current account
      }
    }

    dynamic "principals" {
       # If a cross-account ID is provided AND no specific user is set, trust the target account root
      for_each = var.specific_user_arn_to_trust == "" && var.target_account_id_for_cross_account != "" ? [var.target_account_id_for_cross_account] : []
      content {
        type        = "AWS"
        identifiers = ["arn:aws:iam::${principals.value}:root"] # Trust any principal in the target account
      }
    }
    # Note: This logic prioritizes specific_user_arn_to_trust if provided.
    # If specific_user_arn_to_trust is empty, it trusts either the current account or the target_account_id_for_cross_account.
    # You could adjust this logic for more complex scenarios (e.g., trusting both a specific user AND another account).
  }
}


resource "aws_iam_role" "ec2_read_only_assumable_role" {
  name = "tf-lab4-EC2ReadOnlyUserRole"
  path = "/user-roles/"

  # Attach the Trust Policy
  assume_role_policy = data.aws_iam_policy_document.user_assume_role_policy_doc.json

  description = "Role providing EC2 read-only access, assumable by specified users/accounts."

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 4 User Assumable Role"
  }
}

# --- Step 3: Attach the Permissions Policy to the Role ---
resource "aws_iam_role_policy_attachment" "attach_ec2_read_to_user_role" {
  role       = aws_iam_role.ec2_read_only_assumable_role.name
  policy_arn = aws_iam_policy.ec2_read_only_policy.arn
}

# --- Outputs ---
output "assumable_role_arn" {
  value       = aws_iam_role.ec2_read_only_assumable_role.arn
  description = "The ARN of the created user-assumable IAM Role. Users need this ARN to assume the role."
}

output "trust_policy_json" {
  value       = data.aws_iam_policy_document.user_assume_role_policy_doc.json
  description = "The generated Trust Policy JSON for inspection."
  sensitive   = true # Mark as sensitive if it might contain Account IDs you don't want in logs
}

output "permissions_policy_arn" {
  value = aws_iam_policy.ec2_read_only_policy.arn
   description = "The ARN of the permissions policy attached to the role."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab4-user-roles
    cd lab4-user-roles
    touch main.tf variables.tf # Recommended to put variables here
    # Copy the Terraform code into the appropriate files
    ```
2.  **Cấu hình biến (Quan trọng):** Mở tệp `variables.tf` (hoặc `main.tf` nếu bạn không tạo file riêng) và quyết định cấu hình Trust Policy:
    *   **Kịch bản 1: Cho phép User trong cùng tài khoản assume:** Giữ `target_account_id_for_cross_account = ""` và `specific_user_arn_to_trust = ""`. Trust Policy sẽ tự động cho phép root của tài khoản hiện tại (`data.aws_caller_identity.current.account_id`). *Bất kỳ* user nào trong tài khoản này có quyền `sts:AssumeRole` đối với Role ARN này sẽ có thể assume.
    *   **Kịch bản 2: Chỉ cho phép một User cụ thể assume:** Đặt `specific_user_arn_to_trust = "arn:aws:iam::<YOUR_ACCOUNT_ID>:user/<YOUR_TEST_USER_NAME>"`. Thay thế bằng ARN của user bạn muốn cho phép. Giữ `target_account_id_for_cross_account = ""`.
    *   **Kịch bản 3: Cho phép User từ tài khoản khác assume (Cross-Account):** Đặt `target_account_id_for_cross_account = "<OTHER_ACCOUNT_ID>"` (thay bằng Account ID của tài khoản *khác*). Giữ `specific_user_arn_to_trust = ""`. Trust Policy sẽ cho phép root của tài khoản đó. **Lưu ý:** User trong tài khoản đó cũng cần policy cho phép `sts:AssumeRole` vào Role ARN này.
3.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
4.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Kiểm tra kỹ Trust Policy sẽ được tạo ra trong phần `assume_role_policy` của `aws_iam_role`. Đảm bảo nó phản ánh đúng kịch bản bạn đã chọn.
5.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes`. Ghi lại giá trị `assumable_role_arn` từ output.
6.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Tại sao dùng User Assumed Roles?**
    *   **Bảo mật (Least Privilege):** Thay vì cấp quyền rộng (ví dụ: `ec2:*`) vĩnh viễn cho user, bạn cấp quyền tối thiểu cho user và tạo Role với các quyền nâng cao hơn. User chỉ "nhận" các quyền đó khi họ chủ động assume role cho một tác vụ cụ thể.
    *   **Kiểm toán (Auditing):** Khi user assume role, hành động `sts:AssumeRole` và các hành động tiếp theo được thực hiện *với vai trò đó* đều được ghi lại trong CloudTrail, giúp dễ dàng theo dõi ai đã làm gì và khi nào.
    *   **Cross-Account Access:** Đây là cơ chế **tiêu chuẩn và an toàn** để quản lý truy cập giữa các tài khoản AWS. Tài khoản chứa tài nguyên (Tài khoản B) tạo Role và định nghĩa ai từ Tài khoản A được tin cậy. Tài khoản A cấp quyền cho user/role của mình để assume Role đó trong Tài khoản B. Điều này tránh việc phải tạo IAM user trùng lặp hoặc chia sẻ access keys giữa các tài khoản.
*   **Cấu trúc Trust Policy động:** Việc sử dụng `dynamic "principals"` và `data "aws_caller_identity"` trong Terraform cho phép chúng ta tạo Trust Policy linh hoạt dựa trên các biến đầu vào, làm cho mã nguồn có thể tái sử dụng cho các kịch bản khác nhau (cùng tài khoản, user cụ thể, cross-account) mà không cần sửa đổi logic cốt lõi nhiều.
*   **Quyền cần thiết cho User:** Để một user (ví dụ: `my-test-user` trong Tài khoản A) có thể assume `tf-lab4-EC2ReadOnlyUserRole` (trong Tài khoản B):
    1.  **Trust Policy của Role (Tài khoản B):** Phải tin cậy principal của user (`arn:aws:iam::AAAAAAAAAAAA:user/my-test-user`) hoặc tài khoản của user (`arn:aws:iam::AAAAAAAAAAAA:root`). Mã Terraform của chúng ta đã xử lý việc này.
    2.  **Permission Policy của User (Tài khoản A):** User `my-test-user` phải có một policy (inline hoặc managed) cho phép hành động `sts:AssumeRole` đối với ARN của Role trong Tài khoản B (`arn:aws:iam::BBBBBBBBBBBB:role/tf-lab4-EC2ReadOnlyUserRole`). Ví dụ policy cho user:
        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Action": "sts:AssumeRole",
              "Resource": "arn:aws:iam::BBBBBBBBBBBB:role/tf-lab4-EC2ReadOnlyUserRole"
            }
          ]
        }
        ```
        *Việc tạo policy này cho user nằm ngoài phạm vi Terraform của lab này, nhưng rất quan trọng để hiểu luồng hoạt động hoàn chỉnh.*
*   **Lợi ích của IaC:** Việc định nghĩa các Role phức tạp và Trust Policies bằng Terraform đảm bảo tính nhất quán, khả năng lặp lại và kiểm soát phiên bản. Việc xem lại `terraform plan` trước khi áp dụng giúp phát hiện các lỗi cấu hình tiềm ẩn trong Trust Policy, vốn có thể khó gỡ lỗi nếu cấu hình thủ công.

### Xác minh

1.  **AWS Management Console (Trong tài khoản chứa Role):**
    *   Đi đến **IAM** -> **Roles**.
    *   Tìm role `tf-lab4-EC2ReadOnlyUserRole`.
    *   Kiểm tra tab "Permissions": Đảm bảo `tf-lab4-EC2ReadOnlyPolicy` được gắn.
    *   Kiểm tra tab "Trust relationships": **Quan trọng:** Xác minh rằng JSON của Trust Policy khớp với kịch bản bạn đã cấu hình (tin cậy user cụ thể, tài khoản hiện tại, hoặc tài khoản cross-account).
2.  **Thử Assume Role (Switch Role):**
    *   **Qua AWS Management Console:**
        *   Đăng nhập bằng IAM user được phép assume role (user bạn đã chỉ định trong `specific_user_arn_to_trust`, hoặc *bất kỳ* user nào trong tài khoản được tin cậy nếu bạn không chỉ định user cụ thể, *với điều kiện user đó có policy cho phép `sts:AssumeRole`*).
        *   Click vào tên người dùng của bạn ở góc trên bên phải -> **Switch Role**.
        *   Nếu switch role lần đầu: Click **Switch Role** lần nữa.
        *   Nhập **Account ID** của tài khoản chứa Role (nếu là cross-account) hoặc để trống/dùng ID hiện tại (nếu cùng tài khoản).
        *   Nhập **Role name**: `tf-lab4-EC2ReadOnlyUserRole`.
        *   Nhập **Display Name** (tùy chọn, ví dụ: `EC2-Reader`). Chọn màu.
        *   Click **Switch Role**.
        *   Nếu thành công, bạn sẽ thấy tên Role và Display Name ở góc trên bên phải. Bây giờ hãy thử điều hướng đến dịch vụ **EC2**. Bạn sẽ chỉ có thể xem (Describe) các tài nguyên, không thể tạo hoặc sửa đổi chúng.
        *   Để quay lại user ban đầu, click vào tên Role -> **Switch Back**.
    *   **Qua AWS CLI:**
        *   Đảm bảo AWS CLI được cấu hình với credentials của user được phép assume.
        *   Lấy ARN của role từ output Terraform: `ROLE_ARN=$(terraform output -raw assumable_role_arn)`
        *   Thực hiện lệnh `assume-role`:
            ```bash
            # Đặt tên session tùy ý
            SESSION_NAME="Lab4EC2ReadSession"

            # Chạy lệnh assume-role
            TEMP_CREDS=$(aws sts assume-role --role-arn "$ROLE_ARN" --role-session-name "$SESSION_NAME" --query 'Credentials' --output json)

            # Kiểm tra xem có credentials trả về không
            echo $TEMP_CREDS
            if [ -z "$TEMP_CREDS" ] || [ "$TEMP_CREDS" == "null" ]; then
              echo "Failed to assume role. Check Trust Policy and user's sts:AssumeRole permission."
              exit 1
            fi

            # (Tùy chọn nâng cao) Cấu hình một profile CLI mới với credentials tạm thời
            AWS_ACCESS_KEY_ID=$(echo $TEMP_CREDS | jq -r '.AccessKeyId')
            AWS_SECRET_ACCESS_KEY=$(echo $TEMP_CREDS | jq -r '.SecretAccessKey')
            AWS_SESSION_TOKEN=$(echo $TEMP_CREDS | jq -r '.SessionToken')

            aws configure set aws_access_key_id "$AWS_ACCESS_KEY_ID" --profile temp-role-profile
            aws configure set aws_secret_access_key "$AWS_SECRET_ACCESS_KEY" --profile temp-role-profile
            aws configure set aws_session_token "$AWS_SESSION_TOKEN" --profile temp-role-profile
            aws configure set region us-east-1 --profile temp-role-profile # Hoặc region của bạn

            echo "Credentials configured for profile 'temp-role-profile'. Use '--profile temp-role-profile' with AWS CLI commands."

            # Thử sử dụng profile mới để thực hiện hành động được Role cho phép
            aws ec2 describe-instances --profile temp-role-profile --query 'Reservations[*].Instances[*].[InstanceId, State.Name]' --output table

            # Thử thực hiện hành động không được phép (sẽ thất bại)
            # aws ec2 run-instances --image-id ami-xxxxxxxx --instance-type t2.micro --count 1 --profile temp-role-profile

            # Xóa profile tạm thời khi xong (hoặc credentials sẽ hết hạn)
            # aws configure --profile temp-role-profile # (Xóa thủ công trong ~/.aws/config và ~/.aws/credentials)
            ```

### Dọn dẹp

1.  **Chạy lệnh destroy:**
    Trong thư mục `lab4-user-roles`, chạy:
    ```bash
    terraform destroy
    ```
2.  **Xác nhận:** Nhập `yes`.
3.  **Xác minh dọn dẹp (Tùy chọn):** Sử dụng Console hoặc CLI. Đảm bảo Role và Policy đã bị xóa.

---

Hoàn thành Lab 4, bạn đã nắm vững cách tạo và sử dụng IAM Roles cho người dùng, bao gồm cả kịch bản truy cập liên tài khoản quan trọng. Bạn cũng đã thực hành việc chuyển đổi vai trò (switch role). Lab tiếp theo sẽ đi sâu vào việc làm cho các policy trở nên mạnh mẽ và linh hoạt hơn bằng cách sử dụng Variables, Condition Keys và Context Keys.
## Lab 5: Variables, Conditions & Context Keys

### Mục tiêu

*   Sử dụng Terraform variables để làm cho IAM policies trở nên linh hoạt và tái sử dụng được.
*   Viết IAM policy sử dụng `Condition` block để kiểm soát truy cập dựa trên các yếu tố ngữ cảnh (context).
*   Sử dụng các Condition Keys phổ biến như:
    *   `aws:SourceIp`: Giới hạn truy cập từ các địa chỉ IP cụ thể.
    *   `aws:CurrentTime`: Cho phép hoặc từ chối truy cập dựa trên thời gian hiện tại.
    *   `aws:RequestedRegion`: Chỉ cho phép hành động trong các Region cụ thể.
    *   `aws:PrincipalTag/<tag-key>`: Cho phép hoặc từ chối truy cập dựa trên tag được gắn vào principal (user/role) đang thực hiện yêu cầu (Giới thiệu về ABAC).
*   Hiểu cách các `Condition` block hoạt động và cách kết hợp nhiều điều kiện.

### Điều kiện tiên quyết

*   Hoàn thành các Lab trước, đặc biệt là Lab 2 (Custom Policies) và Lab 4 (Roles).
*   Hiểu cấu trúc JSON của IAM Policy và vị trí của `Condition` block.

### Khái niệm được đề cập

*   **Terraform Variables:** (Đã sử dụng trong các lab trước) Cho phép bạn tham số hóa cấu hình Terraform, giúp mã nguồn linh hoạt, dễ bảo trì và tái sử dụng hơn. Chúng ta sẽ dùng chúng để truyền các giá trị vào `Condition` block.
*   **IAM Policy `Condition` Block:** Một thành phần tùy chọn trong một IAM policy statement cho phép bạn chỉ định các điều kiện phải được đáp ứng để statement có hiệu lực (`Allow` hoặc `Deny`). Nếu các điều kiện không được đáp ứng, statement đó sẽ không được áp dụng.
*   **Condition Key:** Đại diện cho một thông tin ngữ cảnh của yêu cầu (request context) mà AWS sử dụng để đánh giá `Condition` block. Có các loại Condition Key:
    *   **Global Condition Keys:** Có tiền tố `aws:` và có thể được sử dụng trong hầu hết các policy (ví dụ: `aws:SourceIp`, `aws:CurrentTime`, `aws:PrincipalArn`, `aws:userid`, `aws:PrincipalTag/key`).
    *   **Service-specific Condition Keys:** Dành riêng cho một dịch vụ AWS cụ thể (ví dụ: `s3:x-amz-acl`, `ec2:InstanceType`).
*   **Condition Operator:** Loại so sánh được sử dụng để đối chiếu giá trị của Condition Key trong yêu cầu với giá trị bạn chỉ định trong policy. Ví dụ:
    *   **String operators:** `StringEquals`, `StringNotEquals`, `StringLike`, `StringNotLike`.
    *   **Numeric operators:** `NumericEquals`, `NumericNotEquals`, `NumericLessThan`, `NumericGreaterThan`.
    *   **Date operators:** `DateEquals`, `DateNotEquals`, `DateLessThan`, `DateGreaterThan`.
    *   **Boolean operators:** `Bool`.
    *   **IP address operators:** `IpAddress`, `NotIpAddress` (sử dụng định dạng CIDR).
    *   **ARN operators:** `ArnEquals`, `ArnLike`.
    *   **Set operators:** `ForAllValues:<operator>`, `ForAnyValue:<operator>` (dùng khi key có thể có nhiều giá trị).
    *   **Null check operators:** `Null` (kiểm tra xem key có tồn tại và có giá trị hay không).
*   **Context Keys:** Các giá trị thực tế trong một yêu cầu API mà các Condition Keys tham chiếu đến (ví dụ: địa chỉ IP nguồn của yêu cầu, thời gian yêu cầu được thực hiện, ARN của principal, các tag của principal).
*   **Attribute-Based Access Control (ABAC):** Một chiến lược ủy quyền định nghĩa quyền dựa trên các thuộc tính (attributes). Trong AWS, các tag là một cách phổ biến để triển khai ABAC. Condition keys như `aws:PrincipalTag/key` và `aws:ResourceTag/key` là nền tảng cho ABAC trong IAM. Chúng ta sẽ đi sâu hơn vào ABAC trong Lab 8.

### Cấu hình Terraform

Tạo thư mục `lab5-conditions`.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1" # Or your preferred region
  # Assume credentials are configured
}

# --- Input Variables ---
variable "allowed_ip_cidr" {
  description = "The CIDR block allowed to access specific resources (e.g., your public IP /32)."
  type        = string
  # IMPORTANT: Replace with your actual public IP address in CIDR notation.
  # You can find your public IP by searching "what is my ip" in Google.
  # Example: default = "203.0.113.10/32"
  default = "0.0.0.0/0" # WARNING: Default allows all IPs. CHANGE THIS!
  validation {
    condition     = can(cidrnetmask(var.allowed_ip_cidr))
    error_message = "The allowed_ip_cidr must be a valid CIDR block (e.g., 1.2.3.4/32)."
  }
}

variable "allowed_region" {
  description = "The specific AWS region where actions are permitted."
  type        = string
  default     = "us-east-1"
}

variable "required_project_tag_value" {
  description = "The required value for the 'Project' tag on the principal (user/role) to allow access."
  type        = string
  default     = "Blue" # Example project tag value
}

# Get caller identity to get the Account ID for policy construction if needed
data "aws_caller_identity" "current" {}

# --- Create a Test User (or Role) ---
# We'll apply the conditional policy to this user.
resource "aws_iam_user" "conditional_user" {
  name = "tf-lab5-conditional-access-user"
  path = "/users/conditional/"

  # IMPORTANT: Add the required tag for the condition aws:PrincipalTag to work!
  tags = {
    ManagedBy = "Terraform"
    Project   = var.required_project_tag_value # Match the variable value!
    Purpose   = "Lab 5 Conditional Access Test"
  }
}

# --- Define the Conditional Policy ---
data "aws_iam_policy_document" "conditional_s3_access_policy_doc" {
  # Policy allowing limited S3 actions under specific conditions
  statement {
    sid    = "AllowS3ListAllBucketsIfConditionsMet"
    effect = "Allow"
    actions = [
      "s3:ListAllMyBuckets", # Action usually allowed without resource constraint
      "s3:GetBucketLocation" # Often needed with ListAllMyBuckets
    ]
    resources = ["*"] # These actions apply globally

    # --- CONDITION BLOCK ---
    condition {
      # Condition 1: Request must come from the allowed IP address range
      test     = "IpAddress"
      variable = "aws:SourceIp" # Global condition key for source IP
      values   = [var.allowed_ip_cidr]
    }

    condition {
      # Condition 2: Request must be made within the allowed region
      test     = "StringEquals"
      variable = "aws:RequestedRegion" # Global condition key for target region
      values   = [var.allowed_region]
    }

    condition {
      # Condition 3: Principal (user/role) making the request MUST have the 'Project' tag with the specified value
      test     = "StringEquals"
      variable = "aws:PrincipalTag/Project" # Global key referencing the 'Project' tag on the user/role
      values   = [var.required_project_tag_value]
    }

    # Note: All conditions within a single statement block must be met (AND logic).
    # To achieve OR logic between conditions, use separate statements.
  }

  # Example of a time-based condition (Deny access outside business hours)
  # Add this as another statement if desired.
  /*
  statement {
    sid    = "DenyAccessOutsideBusinessHours"
    effect = "Deny"
    actions = [
      "s3:*" # Deny all S3 actions
    ]
    resources = ["*"]

    condition {
      # Condition: Deny if current time is before 9 AM UTC OR after 5 PM UTC
      # Using DateGreaterThan for "before 9 AM" and DateLessThan for "after 5 PM" relative to a fixed date component
      # Adjust time and timezone as needed. AWS uses UTC.
      test     = "DateGreaterThan"
      variable = "aws:CurrentTime"
      values   = ["${formatdate("YYYY-MM-DD", timestamp())}T17:00:00Z"] # After 5 PM UTC today
    }
    condition {
      test     = "DateLessThan"
      variable = "aws:CurrentTime"
      values   = ["${formatdate("YYYY-MM-DD", timestamp())}T09:00:00Z"] # Before 9 AM UTC today
    }
    # This DENY statement overrides any ALLOW statements for the same actions if conditions are met.
  }
  */
}

# Create the Customer Managed Policy
resource "aws_iam_policy" "conditional_s3_access_policy" {
  name        = "tf-lab5-ConditionalS3AccessPolicy"
  path        = "/custom-policies/conditional/"
  description = "Allows limited S3 access based on IP, Region, and Principal Tag"
  policy      = data.aws_iam_policy_document.conditional_s3_access_policy_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 5 Conditional Policy"
  }
}

# Attach the policy to the user
resource "aws_iam_user_policy_attachment" "attach_conditional_policy_to_user" {
  user       = aws_iam_user.conditional_user.name
  policy_arn = aws_iam_policy.conditional_s3_access_policy.arn
}

# --- Outputs ---
output "conditional_user_name" {
  value = aws_iam_user.conditional_user.name
}

output "conditional_user_arn" {
  value = aws_iam_user.conditional_user.arn
}

output "conditional_policy_arn" {
  value = aws_iam_policy.conditional_s3_access_policy.arn
}

output "conditional_policy_json" {
  value       = data.aws_iam_policy_document.conditional_s3_access_policy_doc.json
  description = "The generated conditional policy JSON."
  sensitive   = true
}

output "user_tags" {
  value = aws_iam_user.conditional_user.tags
  description = "Tags applied to the test user."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab5-conditions
    cd lab5-conditions
    touch main.tf variables.tf # Recommended
    # Copy the Terraform code into the appropriate files
    ```
2.  **Cấu hình biến quan trọng (`variables.tf` hoặc `main.tf`):**
    *   **`allowed_ip_cidr`:** **RẤT QUAN TRỌNG:** Thay đổi giá trị `default` thành địa chỉ IP công cộng *của bạn* ở định dạng CIDR (ví dụ: `YOUR_IP/32`). Nếu không, điều kiện IP sẽ không hoạt động đúng hoặc sẽ cho phép tất cả IP nếu để `0.0.0.0/0`.
    *   `allowed_region`: Đặt thành region bạn đang sử dụng hoặc muốn giới hạn.
    *   `required_project_tag_value`: Giữ nguyên `Blue` hoặc thay đổi nếu muốn, nhưng đảm bảo giá trị này **khớp** với giá trị tag `Project` được đặt trên `aws_iam_user.conditional_user`.
3.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
4.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan -var="allowed_ip_cidr=YOUR_IP/32" # Substitute YOUR_IP/32
    ```
    Kiểm tra kỹ `Condition` block trong policy sẽ được tạo. Xác nhận các giá trị từ biến đã được đưa vào đúng chỗ. Xem lại các tag trên user.
5.  **Áp dụng cấu hình:**
    ```bash
    terraform apply -var="allowed_ip_cidr=YOUR_IP/32" # Substitute YOUR_IP/32
    ```
    Nhập `yes`. Ghi lại tên user (`conditional_user_name`).
6.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Sức mạnh của Conditions:** `Condition` block là một công cụ cực kỳ mạnh mẽ để thực thi **nguyên tắc đặc quyền tối thiểu** và áp dụng các lớp bảo mật bổ sung. Thay vì chỉ kiểm soát *hành động* và *tài nguyên*, bạn còn kiểm soát được *ngữ cảnh* mà hành động đó được phép thực hiện.
*   **Các trường hợp sử dụng phổ biến:**
    *   **Giới hạn IP (Network Perimeter):** `aws:SourceIp` rất hữu ích để đảm bảo chỉ các yêu cầu từ mạng văn phòng hoặc VPN đã biết mới có thể truy cập các tài nguyên nhạy cảm.
    *   **Giới hạn Region:** `aws:RequestedRegion` giúp ngăn chặn việc vô tình tạo hoặc truy cập tài nguyên ở các region không mong muốn, hỗ trợ tuân thủ và kiểm soát chi phí.
    *   **Kiểm soát dựa trên thời gian:** `aws:CurrentTime` có thể dùng để giới hạn các hành động nhạy cảm chỉ trong giờ làm việc.
    *   **Yêu cầu MFA:** `aws:MultiFactorAuthPresent: true` (sẽ xem trong Lab 10) là một điều kiện cực kỳ quan trọng để yêu cầu xác thực đa yếu tố cho các hành động quan trọng.
    *   **ABAC với Tags:** `aws:PrincipalTag/key` (như trong lab này) và `aws:ResourceTag/key` (Lab 8) cho phép xây dựng các hệ thống quyền linh hoạt dựa trên siêu dữ liệu (tags) thay vì liệt kê cứng các user/resource ARN. Điều này **tăng khả năng mở rộng** đáng kể. Ví dụ: bạn có thể tạo policy nói rằng "User có tag `Project=Blue` chỉ có thể quản lý EC2 instances có tag `Project=Blue`".
*   **Logic AND/OR:**
    *   **AND:** Tất cả các `condition` block trong *cùng một statement* phải được đánh giá là `true` để statement đó có hiệu lực.
    *   **OR:** Để đạt được logic OR giữa các điều kiện khác nhau, bạn cần tạo các *statement riêng biệt*. Ví dụ: một statement cho phép nếu IP là A, một statement khác cho phép nếu tag là X.
*   **`Deny` luôn thắng:** Một `Deny` statement trong bất kỳ policy nào (identity-based, resource-based, SCP, permissions boundary) sẽ luôn ghi đè lên một `Allow` statement cho cùng một hành động/tài nguyên. Điều này làm cho `Deny` với `Condition` trở thành một công cụ mạnh mẽ để tạo ra các ngoại lệ hoặc giới hạn bảo vệ. (Ví dụ: Luôn `Deny` hành động xóa nếu không có MFA, bất kể các policy `Allow` khác).
*   **Terraform và Conditions:** Sử dụng `data "aws_iam_policy_document"` và các biến Terraform giúp việc xây dựng các policy phức tạp với `Condition` trở nên dễ quản lý hơn nhiều so với việc viết và duy trì các chuỗi JSON lớn, dễ lỗi.

### Xác minh

Việc xác minh policy có điều kiện phức tạp hơn một chút vì bạn cần đảm bảo yêu cầu của mình đáp ứng (hoặc không đáp ứng) các điều kiện.

1.  **Kiểm tra Cấu hình:**
    *   **AWS Console:** Đi đến **IAM** -> **Users** -> `tf-lab5-conditional-access-user`.
        *   Kiểm tra tab "Tags": Xác nhận tag `Project` có giá trị đúng (`Blue` hoặc giá trị bạn đặt).
        *   Kiểm tra tab "Permissions": Tìm policy `tf-lab5-ConditionalS3AccessPolicy`. Click vào nó, xem JSON và xác nhận `Condition` block chứa đúng IP CIDR, Region, và điều kiện tag.
    *   **AWS CLI:**
        ```bash
        USER_NAME=$(terraform output -raw conditional_user_name)
        POLICY_ARN=$(terraform output -raw conditional_policy_arn)

        # Kiểm tra tag của user
        aws iam list-user-tags --user-name $USER_NAME

        # Kiểm tra policy JSON
        aws iam get-policy-version --policy-arn $POLICY_ARN --version-id v1 --query 'PolicyVersion.Document' --output json | jq
        ```

2.  **Thử nghiệm Truy cập (Sử dụng AWS CLI):**
    *   **Quan trọng:** Bạn cần tạo access keys cho user `tf-lab5-conditional-access-user` **thông qua AWS Console** (IAM -> Users -> chọn user -> Security credentials -> Create access key). **Lưu lại Access Key ID và Secret Access Key một cách an toàn.**
    *   Cấu hình một profile AWS CLI mới cho user này:
        ```bash
        aws configure --profile lab5-user
        # Nhập Access Key ID, Secret Access Key bạn vừa tạo
        # Đặt Default region name là giá trị bạn đã đặt trong biến 'allowed_region' (ví dụ: us-east-1)
        # Đặt Default output format là 'json' (hoặc tùy chọn)
        ```
    *   **Chạy thử nghiệm:**
        *   **Trường hợp thành công (Tất cả điều kiện đúng):** Đảm bảo bạn đang chạy lệnh từ máy có địa chỉ IP nằm trong `allowed_ip_cidr` đã cấu hình.
            ```bash
            # Sử dụng profile của user và đúng region
            aws s3 ls --profile lab5-user --region us-east-1
            # Lệnh này nên thành công nếu tất cả điều kiện (IP, Region, Tag trên user) đều đúng.
            ```
        *   **Trường hợp thất bại (Sai IP):** Nếu bạn chạy từ IP khác, lệnh trên sẽ thất bại với lỗi "Access Denied".
        *   **Trường hợp thất bại (Sai Region):**
            ```bash
            # Thử chạy ở region khác (ví dụ: us-west-2)
            aws s3 ls --profile lab5-user --region us-west-2
            # Lệnh này nên thất bại với lỗi "Access Denied" do điều kiện aws:RequestedRegion.
            ```
        *   **Trường hợp thất bại (Sai Tag):** Nếu bạn tạm thời xóa tag `Project` khỏi user trong Console và chạy lại lệnh `aws s3 ls --profile lab5-user --region us-east-1`, nó cũng sẽ thất bại do điều kiện `aws:PrincipalTag/Project`. (Nhớ thêm lại tag sau khi thử nghiệm).

### Dọn dẹp

1.  **Xóa Access Keys:** Đi đến AWS Console -> IAM -> Users -> `tf-lab5-conditional-access-user` -> Security credentials. Xóa access key bạn đã tạo để thử nghiệm. Đây là bước bảo mật quan trọng.
2.  **Chạy lệnh destroy:**
    Trong thư mục `lab5-conditions`, chạy:
    ```bash
    # Không cần truyền biến ở đây vì destroy đọc từ state file
    terraform destroy
    ```
3.  **Xác nhận:** Nhập `yes`.
4.  **Xóa profile CLI (Tùy chọn):** Xóa profile `lab5-user` khỏi các tệp `~/.aws/credentials` và `~/.aws/config`.

---

Chúc mừng bạn đã hoàn thành Lab 5! Bạn đã học cách sử dụng `Condition` keys để tạo ra các IAM policies mạnh mẽ, linh hoạt và an toàn hơn, đồng thời làm quen với khái niệm ABAC thông qua `aws:PrincipalTag`. Lab tiếp theo sẽ giới thiệu về Permissions Boundaries, một cơ chế quan trọng khác để giới hạn quyền hiệu quả trong AWS.
## Lab 6: Permissions Boundaries

### Mục tiêu

*   Hiểu khái niệm và mục đích của IAM Permissions Boundaries.
*   Tạo một IAM User (hoặc Role).
*   Tạo một Customer Managed Policy đóng vai trò là Permissions Boundary (ví dụ: chỉ cho phép các hành động EC2 và S3 cụ thể).
*   Tạo một Customer Managed Policy khác (Identity-based Policy) cấp các quyền rộng hơn (ví dụ: `ec2:*` và `s3:*`).
*   Gắn cả Identity-based Policy và Permissions Boundary vào User/Role.
*   Xác minh rằng quyền *hiệu quả* (effective permissions) của User/Role là giao điểm (intersection) của cả hai policy.
*   Sử dụng Terraform để định nghĩa và gắn Permissions Boundary.

### Điều kiện tiên quyết

*   Hoàn thành các Lab trước, đặc biệt là Lab 2 (Custom Policies) và Lab 5 (Conditions).
*   Hiểu rõ về Identity-based Policies.

### Khái niệm được đề cập

*   **Permissions Boundary:** Một tính năng IAM nâng cao cho phép bạn sử dụng một **AWS Managed Policy** hoặc **Customer Managed Policy** để đặt **quyền tối đa (maximum permissions)** mà một identity-based policy có thể cấp cho một IAM entity (User hoặc Role). Permissions Boundary *không tự cấp quyền*; nó chỉ lọc hoặc giới hạn các quyền được cấp bởi identity-based policies.
*   **Quyền Hiệu Quả (Effective Permissions):** Quyền thực tế mà một User hoặc Role có được là **giao điểm (intersection)** của:
    1.  Tất cả các Identity-based Policies (AWS Managed, Customer Managed, Inline) được gắn vào User/Role hoặc các Group mà User thuộc về.
    2.  Permissions Boundary được gắn vào User/Role (nếu có).
    3.  (Trong ngữ cảnh AWS Organizations) Service Control Policies (SCPs) áp dụng cho tài khoản.
    *   **Nói cách khác:** Một hành động chỉ được phép nếu nó được `Allow` bởi cả Identity-based Policy VÀ Permissions Boundary (và không bị `Deny` bởi bất kỳ policy nào, bao gồm SCP).
*   **Mục đích sử dụng Permissions Boundaries:**
    *   **Ủy quyền an toàn (Safe Delegation):** Cho phép quản trị viên cấp cao (ví dụ: admin tài khoản) ủy quyền việc tạo user/role cho các quản trị viên cấp thấp hơn (ví dụ: team lead) mà không lo lắng họ sẽ tạo ra các principal với quyền vượt quá giới hạn cho phép. Team lead có thể gắn các identity-based policy khác nhau, nhưng quyền hiệu quả sẽ không bao giờ vượt qua boundary do admin cấp cao định nghĩa.
    *   **Thiết lập Guardrails:** Đảm bảo rằng các user hoặc role (đặc biệt là những role được tạo tự động hoặc bởi các developer ít kinh nghiệm về IAM) không bao giờ có được các quyền nhạy cảm (ví dụ: quyền thay đổi cấu hình IAM, xóa tài nguyên quan trọng), ngay cả khi identity-based policy vô tình cấp các quyền đó.
    *   **Tuân thủ:** Giúp thực thi các chính sách bảo mật và tuân thủ của tổ chức bằng cách giới hạn quyền ở mức độ rộng.
*   **Lưu ý quan trọng:**
    *   Permissions Boundary chỉ áp dụng cho User và Role, không áp dụng cho Group.
    *   Việc xóa Permissions Boundary khỏi một User/Role sẽ loại bỏ giới hạn đó, các quyền từ identity-based policies sẽ có hiệu lực đầy đủ (nếu không bị chặn bởi SCP).
    *   Một User/Role chỉ có thể có *một* Permissions Boundary được gắn vào.
*   **Terraform Resource Attribute:**
    *   Thuộc tính `permissions_boundary` có sẵn trong các resource `aws_iam_user` và `aws_iam_role`. Giá trị của nó là ARN của policy (Managed hoặc Customer Managed) được sử dụng làm boundary.

### Cấu hình Terraform

Tạo thư mục `lab6-boundaries`.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured
}

# --- Step 1: Define the Permissions Boundary Policy ---
# This policy defines the MAXIMUM allowed permissions.
data "aws_iam_policy_document" "boundary_policy_doc" {
  statement {
    sid    = "AllowOnlyLimitedEC2S3Actions"
    effect = "Allow"
    actions = [
      # Specific EC2 read actions allowed by boundary
      "ec2:DescribeInstances",
      "ec2:DescribeVolumes",
      # Specific S3 read actions allowed by boundary
      "s3:ListBucket",
      "s3:GetObject",
      # IMPORTANT: Allow user to manage their own credentials if needed
      "iam:GetUser",
      "iam:ListAccessKeys",
      "iam:CreateAccessKey",
      "iam:DeleteAccessKey",
      "iam:UpdateAccessKey"
      # DO NOT add broad permissions like "ec2:*" or "s3:*" here!
    ]
    resources = ["*"] # Keep broad for boundary, identity policy will restrict if needed
    # Note: Boundaries often use Resource="*", the identity policy should be more specific.
    # However, you can also restrict resources in the boundary.
  }
}

resource "aws_iam_policy" "limited_ec2_s3_boundary" {
  name        = "tf-lab6-LimitedEC2S3Boundary"
  path        = "/boundaries/"
  description = "Permissions Boundary allowing only specific EC2/S3 read actions and self-service IAM key management."
  policy      = data.aws_iam_policy_document.boundary_policy_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 6 Permissions Boundary"
    Type      = "Boundary"
  }
}

# --- Step 2: Define the Identity-based Policy ---
# This policy grants permissions, but they will be limited by the boundary.
data "aws_iam_policy_document" "identity_policy_doc" {
  statement {
    sid    = "GrantBroadEC2S3"
    effect = "Allow"
    actions = [
      "ec2:*", # Attempts to grant ALL EC2 actions
      "s3:*"  # Attempts to grant ALL S3 actions
    ]
    resources = ["*"]
  }
  # Add another statement allowing IAM self-service (must also be in boundary)
  statement {
     sid    = "AllowIAMSelfServiceForKeys"
     effect = "Allow"
     actions = [
        "iam:GetUser",
        "iam:ListAccessKeys",
        "iam:CreateAccessKey",
        "iam:DeleteAccessKey",
        "iam:UpdateAccessKey"
     ]
     # Restrict to the user themselves if possible, requires knowing the user ARN/name
     # Using a wildcard for simplicity here, but restricting is better practice.
     # resources = ["arn:aws:iam::${data.aws_caller_identity.current.account_id}:user/${aws_iam_user.developer_with_boundary.name}"]
     resources = ["*"] # Less secure, but simpler for lab example
  }
}

resource "aws_iam_policy" "broad_ec2_s3_identity_policy" {
  name        = "tf-lab6-BroadEC2S3IdentityPolicy"
  path        = "/custom-policies/"
  description = "Identity policy attempting to grant broad EC2/S3 access."
  policy      = data.aws_iam_policy_document.identity_policy_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 6 Identity Policy"
    Type      = "Identity"
  }
}

# --- Step 3: Create the User with the Boundary and Identity Policy ---
resource "aws_iam_user" "developer_with_boundary" {
  name = "tf-lab6-developer-with-boundary"
  path = "/users/bounded/"

  # Attach the Permissions Boundary
  permissions_boundary = aws_iam_policy.limited_ec2_s3_boundary.arn

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 6 User with Boundary"
  }
}

# Attach the Identity-based Policy to the User
resource "aws_iam_user_policy_attachment" "attach_identity_policy_to_bounded_user" {
  user       = aws_iam_user.developer_with_boundary.name
  policy_arn = aws_iam_policy.broad_ec2_s3_identity_policy.arn
}

# --- Outputs ---
output "bounded_user_name" {
  value = aws_iam_user.developer_with_boundary.name
}

output "permissions_boundary_arn" {
  value = aws_iam_user.developer_with_boundary.permissions_boundary
}

output "identity_policy_arn" {
  value = aws_iam_policy.broad_ec2_s3_identity_policy.arn
}

output "effective_permissions_explanation" {
  value = "The effective permissions for user '${aws_iam_user.developer_with_boundary.name}' are the INTERSECTION of the identity policy (${aws_iam_policy.broad_ec2_s3_identity_policy.name}) and the permissions boundary (${aws_iam_policy.limited_ec2_s3_boundary.name}). The user will ONLY be able to perform actions allowed by BOTH policies (e.g., ec2:DescribeInstances, s3:GetObject, iam:CreateAccessKey) but NOT actions only in the identity policy (e.g., ec2:RunInstances, s3:PutObject)."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab6-boundaries
    cd lab6-boundaries
    touch main.tf
    # Copy the Terraform code into main.tf
    ```
2.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
3.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Chú ý đến resource `aws_iam_user` và thuộc tính `permissions_boundary` đang được đặt thành ARN của policy boundary. Xem lại nội dung của cả hai policy (boundary và identity).
4.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes`. Ghi lại tên user (`bounded_user_name`).
5.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Giao điểm là chìa khóa:** Hãy hình dung Permissions Boundary như một cái khuôn hoặc bộ lọc. Identity-based policy cố gắng "rót" quyền vào khuôn đó. Chỉ những quyền nào lọt qua được các lỗ trên khuôn (tức là được `Allow` bởi cả hai) mới là quyền hiệu quả.
*   **Boundary không cấp quyền:** Điều quan trọng cần nhắc lại là Boundary tự nó không cấp bất kỳ quyền nào. Nếu một hành động chỉ được `Allow` trong Boundary mà không được `Allow` trong bất kỳ Identity-based policy nào, hành động đó vẫn bị từ chối.
*   **Tại sao Boundary lại `Allow` `iam:*AccessKey`?** Một trường hợp sử dụng phổ biến cho boundary là cho phép user tự quản lý access keys của chính họ (`iam:CreateAccessKey`, `iam:DeleteAccessKey`, etc.) nhưng chặn họ thực hiện các hành động IAM khác có thể thay đổi quyền (ví dụ: `iam:AttachUserPolicy`, `iam:CreateRole`). Boundary phải `Allow` các hành động này, và identity-based policy cũng phải `Allow` chúng thì user mới thực hiện được.
*   **Thiết kế Boundary Policy:** Boundary policy thường nên được thiết kế ở mức độ tương đối rộng (ví dụ: cho phép các hành động trong một dịch vụ như `ec2:*` hoặc các hành động đọc `*:Describe*`, `*:Get*`, `*:List*`) hoặc giới hạn ở các hành động an toàn đã được phê duyệt. Identity-based policies sau đó sẽ cung cấp các quyền chi tiết hơn *trong giới hạn* của boundary đó. Tuy nhiên, ví dụ của chúng ta làm ngược lại (boundary chi tiết, identity rộng) để minh họa rõ ràng hiệu ứng giới hạn.
*   **Sử dụng Terraform cho Boundaries:** Việc định nghĩa và gắn boundary bằng Terraform đảm bảo rằng giới hạn an toàn này luôn được áp dụng một cách nhất quán khi tạo user/role, giảm thiểu lỗi cấu hình thủ công. Thuộc tính `permissions_boundary` làm cho việc này trở nên đơn giản.
*   **Ủy quyền tạo Role với Boundary:** Một kịch bản nâng cao nhưng rất mạnh mẽ là tạo một Role (ví dụ: `TerraformExecutionRole`) với một Permissions Boundary giới hạn các hành động IAM mà Role đó có thể thực hiện. Sau đó, cấp cho các pipeline CI/CD hoặc các developer quyền `sts:AssumeRole` đối với Role này VÀ quyền `iam:PassRole` (chỉ cho những role *khác* mà họ được phép gắn vào tài nguyên). Điều này cho phép họ chạy Terraform để tạo tài nguyên, nhưng Terraform (chạy với `TerraformExecutionRole`) sẽ bị giới hạn bởi boundary, không thể tạo ra các role hoặc user với quyền vượt quá quy định.

### Xác minh

1.  **Kiểm tra Cấu hình:**
    *   **AWS Console:** Đi đến **IAM** -> **Users** -> `tf-lab6-developer-with-boundary`.
        *   Xem tab "Permissions". Bạn sẽ thấy cả hai policy: `tf-lab6-BroadEC2S3IdentityPolicy` (dưới dạng identity policy) và `tf-lab6-LimitedEC2S3Boundary` (được chỉ định rõ là Permissions boundary).
        *   Click vào từng policy để xem JSON và so sánh.
    *   **AWS CLI:**
        ```bash
        USER_NAME=$(terraform output -raw bounded_user_name)
        BOUNDARY_ARN=$(terraform output -raw permissions_boundary_arn)
        IDENTITY_POLICY_ARN=$(terraform output -raw identity_policy_arn)

        # Lấy thông tin user, bao gồm boundary được gắn
        aws iam get-user --user-name $USER_NAME

        # Xác nhận policy identity được gắn
        aws iam list-attached-user-policies --user-name $USER_NAME --query "AttachedPolicies[?PolicyArn=='$IDENTITY_POLICY_ARN'].PolicyArn" --output text

        # Kiểm tra nội dung boundary policy
        aws iam get-policy-version --policy-arn $BOUNDARY_ARN --version-id v1 --query 'PolicyVersion.Document'

        # Kiểm tra nội dung identity policy
        aws iam get-policy-version --policy-arn $IDENTITY_POLICY_ARN --version-id v1 --query 'PolicyVersion.Document'
        ```

2.  **Thử nghiệm Quyền Hiệu Quả (Sử dụng AWS Policy Simulator hoặc AWS CLI):**
    *   **AWS Policy Simulator (Khuyến nghị):**
        *   Đi đến **IAM** -> **Policy simulator** (trong menu bên trái).
        *   Chọn **User** và chọn `tf-lab6-developer-with-boundary`.
        *   Policy simulator sẽ tự động nhận diện cả identity policy và permissions boundary.
        *   Trong **Select service**, chọn `EC2`. Trong **Select actions**, chọn:
            *   `DescribeInstances` (Hành động này được Allow trong cả hai policy).
            *   `RunInstances` (Hành động này chỉ được Allow trong identity policy, bị chặn bởi boundary).
        *   Click **Run Simulation**.
        *   **Kết quả:** `DescribeInstances` sẽ hiển thị `allowed`. `RunInstances` sẽ hiển thị `denied` và lý do từ chối là do **permissions boundary**.
        *   Lặp lại với dịch vụ `S3`, thử `GetObject` (nên allowed) và `PutObject` (nên denied).
        *   Thử với dịch vụ `IAM`, thử `CreateAccessKey` (nên allowed) và `AttachUserPolicy` (nên denied).
    *   **AWS CLI (Yêu cầu tạo Access Key):**
        *   Tương tự Lab 5, tạo access key cho user `tf-lab6-developer-with-boundary` qua Console.
        *   Cấu hình profile CLI mới (`lab6-user`).
        *   Chạy các lệnh:
            ```bash
            # Hành động được phép bởi cả hai
            aws ec2 describe-instances --profile lab6-user --region us-east-1 --query 'Reservations[*].Instances[*].InstanceId' --output text
            # (Nên thành công, có thể trả về rỗng nếu không có instance)

            aws iam list-access-keys --profile lab6-user --user-name tf-lab6-developer-with-boundary
            # (Nên thành công)

            # Hành động chỉ được phép bởi identity policy, bị chặn bởi boundary
            aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --count 1 --dry-run --profile lab6-user --region us-east-1
            # (Nên thất bại với lỗi Access Denied, --dry-run để không tạo instance thật)

            aws s3api put-object --bucket <some-existing-bucket> --key test.txt --body test.txt --profile lab6-user --region us-east-1
            # (Nên thất bại với lỗi Access Denied. Cần có bucket để thử)

            aws iam attach-user-policy --user-name tf-lab6-developer-with-boundary --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --profile lab6-user
            # (Nên thất bại với lỗi Access Denied)
            ```
        *   **Quan trọng:** Nhớ xóa access key sau khi thử nghiệm.

### Dọn dẹp

1.  **Xóa Access Keys (Nếu đã tạo):** Xóa access key của `tf-lab6-developer-with-boundary` trong Console.
2.  **Chạy lệnh destroy:**
    Trong thư mục `lab6-boundaries`, chạy:
    ```bash
    terraform destroy
    ```
3.  **Xác nhận:** Nhập `yes`.
4.  **Xóa profile CLI (Tùy chọn):** Xóa profile `lab6-user`.

---

Hoàn thành Lab 6! Bạn đã triển khai thành công Permissions Boundaries bằng Terraform và hiểu cách chúng hoạt động cùng với identity-based policies để xác định quyền hiệu quả. Đây là một kỹ thuật quan trọng để ủy quyền và thiết lập guardrails trong môi trường AWS. Lab tiếp theo sẽ khám phá cách tích hợp AWS IAM với các nhà cung cấp danh tính bên ngoài thông qua IAM Identity Providers.
## Lab 7: IAM Identity Providers & Federation Roles (Khái niệm & Setup Role Cơ bản)

### Mục tiêu

*   Hiểu khái niệm về liên kết danh tính (Identity Federation) trong AWS IAM.
*   Phân biệt giữa SAML 2.0 và OpenID Connect (OIDC) là các giao thức federation phổ biến.
*   Hiểu vai trò của IAM Identity Provider (IdP) trong AWS.
*   Tạo một IAM Role với Trust Policy đặc biệt cho phép principal từ một IdP (SAML hoặc OIDC) đã được định cấu hình có thể assume role này.
*   Sử dụng Terraform để tạo resource `aws_iam_saml_provider` hoặc `aws_iam_openid_connect_provider` (chỉ tạo resource đại diện, không cần cấu hình IdP thực tế).
*   Tạo IAM Role liên kết với IdP đó.
*   Hiểu cách các thuộc tính (attributes/claims) từ IdP có thể được sử dụng trong `Condition` của policy role để kiểm soát truy cập chi tiết.

### Điều kiện tiên quyết

*   Hoàn thành các Lab trước, đặc biệt là Lab 4 (User Assumed Roles) và Lab 5 (Conditions).
*   Hiểu biết cơ bản về khái niệm Single Sign-On (SSO), SAML 2.0 Assertion, và OIDC ID Token là một lợi thế (nhưng sẽ được giải thích ở mức độ cơ bản).

### Khái niệm được đề cập

*   **Identity Federation (Liên kết danh tính):** Cho phép người dùng từ một hệ thống quản lý danh tính bên ngoài (ví dụ: Active Directory, Azure AD, Okta, Google Workspace, hoặc một ứng dụng tùy chỉnh) đăng nhập và truy cập tài nguyên AWS mà không cần tạo IAM User riêng biệt cho họ trong AWS. Thay vào đó, họ sử dụng danh tính hiện có của mình.
*   **Identity Provider (IdP):** Hệ thống quản lý danh tính bên ngoài chịu trách nhiệm xác thực người dùng và cung cấp thông tin về họ (ví dụ: tên người dùng, email, thuộc tính nhóm) cho AWS.
*   **Service Provider (SP):** Trong bối cảnh này, AWS là Service Provider. Nó tin tưởng IdP để xác thực người dùng và ủy quyền truy cập dựa trên thông tin do IdP cung cấp.
*   **SAML 2.0 (Security Assertion Markup Language):** Một tiêu chuẩn mở dựa trên XML để trao đổi dữ liệu xác thực và ủy quyền giữa IdP và SP. Khi người dùng đăng nhập qua IdP SAML, IdP sẽ gửi một **SAML Assertion** (một tài liệu XML được ký số) đến AWS STS. Assertion này chứa thông tin về người dùng đã được xác thực và các thuộc tính của họ.
*   **OpenID Connect (OIDC):** Một lớp định danh đơn giản được xây dựng trên nền tảng giao thức ủy quyền OAuth 2.0. Nó cho phép các client xác minh danh tính của người dùng cuối dựa trên sự xác thực được thực hiện bởi một Authorization Server, cũng như để có được thông tin hồ sơ cơ bản về người dùng cuối theo cách có thể tương tác và giống REST. OIDC sử dụng **JSON Web Tokens (JWTs)**, bao gồm **ID Token**, để truyền thông tin người dùng. Thường được sử dụng cho các ứng dụng web và di động, cũng như cho các workload như GitHub Actions truy cập AWS.
*   **IAM Identity Provider (trong AWS):** Một thực thể trong IAM đại diện cho một IdP bên ngoài (SAML hoặc OIDC) mà tài khoản AWS của bạn tin cậy. Bạn cần đăng ký IdP trong IAM bằng cách cung cấp siêu dữ liệu (metadata document cho SAML) hoặc thông tin về nhà cung cấp (URL nhà cung cấp, đối tượng, thumbprint cho OIDC).
*   **Role for Identity Federation:** Một IAM Role đặc biệt có Trust Policy cho phép principal được đại diện bởi một IdP (ví dụ: `Federated: arn:aws:iam::<ACCOUNT_ID>:saml-provider/<PROVIDER_NAME>`) thực hiện hành động `sts:AssumeRoleWithSAML` hoặc `sts:AssumeRoleWithWebIdentity` (cho OIDC).
*   **`sts:AssumeRoleWithSAML` / `sts:AssumeRoleWithWebIdentity`:** Các hành động API của AWS STS được sử dụng bởi các principal liên kết để yêu cầu thông tin xác thực tạm thời cho một Role, dựa trên SAML Assertion hoặc OIDC ID Token tương ứng.
*   **Attributes/Claims Mapping:** Thông tin người dùng từ SAML Assertion (attributes) hoặc OIDC ID Token (claims) có thể được sử dụng trong `Condition` block của IAM Role's policy hoặc Trust Policy để đưa ra quyết định truy cập chi tiết. Ví dụ: chỉ cho phép user thuộc nhóm "Admins" trong SAML Assertion được assume role, hoặc cấp quyền khác nhau dựa trên thuộc tính `department` trong OIDC token. Các condition key thường dùng là `saml:aud`, `saml:iss`, `saml:sub`, `oidc:aud`, `oidc:iss`, `oidc:sub`, và các key cho thuộc tính tùy chỉnh như `saml:AttributeName`.
*   **Terraform Resources:**
    *   `aws_iam_saml_provider`: Tạo và quản lý một SAML Identity Provider trong IAM. Yêu cầu nội dung XML của tài liệu siêu dữ liệu SAML từ IdP.
    *   `aws_iam_openid_connect_provider`: Tạo và quản lý một OIDC Identity Provider. Yêu cầu URL của OIDC provider, danh sách client ID (audiences), và danh sách thumbprint của CA gốc của server OIDC provider.
    *   `aws_iam_role`: (Như trước) Dùng để tạo Role, nhưng với Trust Policy tham chiếu đến ARN của IdP Provider.

### Cấu hình Terraform

Tạo thư mục `lab7-federation`. Chúng ta sẽ tạo cả hai loại IdP Provider và Role tương ứng để minh họa cú pháp, nhưng bạn chỉ cần một loại trong thực tế. **Lưu ý:** Chúng ta sẽ sử dụng dữ liệu giả định cho metadata/OIDC info vì chúng ta không cấu hình IdP thực tế.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured
}

# Get caller identity for Account ID
data "aws_caller_identity" "current" {}

# --- Phần 1: SAML 2.0 Federation ---

variable "saml_metadata_document_content" {
  description = "Placeholder for the SAML metadata XML content from the IdP."
  type        = string
  # In a real scenario, this would be the actual XML content or loaded from a file.
  # Using placeholder XML for demonstration purposes.
  default = <<EOT
<md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="http://example.com/saml/metadata/idp">
  <md:IDPSSODescriptor WantAuthnRequestsSigned="false" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    <md:KeyDescriptor use="signing">
      <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIDpDCCAoygAwIBAgIG...example...</ds:X509Certificate>
        </ds:X509Data>
      </ds:KeyInfo>
    </md:KeyDescriptor>
    <md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</md:NameIDFormat>
    <md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="http://example.com/saml/sso/post"/>
    <md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="http://example.com/saml/sso/redirect"/>
  </md:IDPSSODescriptor>
</md:EntityDescriptor>
EOT
  # Do not validate this default as it's just a placeholder
}

# Create the SAML Identity Provider in IAM
resource "aws_iam_saml_provider" "example_saml_idp" {
  name                   = "tf-lab7-ExampleSAMLIdP"
  saml_metadata_document = var.saml_metadata_document_content

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 7 SAML IdP"
  }
}

# Define the Trust Policy for the SAML Role
data "aws_iam_policy_document" "saml_assume_role_policy_doc" {
  statement {
    actions = ["sts:AssumeRoleWithSAML"]
    effect  = "Allow"

    principals {
      type = "Federated"
      # Reference the ARN of the SAML Provider created above
      identifiers = [aws_iam_saml_provider.example_saml_idp.arn]
    }

    # --- Example Condition: Restrict based on SAML Audience ---
    # 'Audience' in SAML typically refers to the Service Provider (AWS)
    # This ensures the assertion was intended for AWS.
    condition {
      test     = "StringEquals"
      variable = "saml:aud" # SAML condition key for Audience
      # The value should match the AWS endpoint (urn:amazon:webservices) or your specific relay state URL
      values   = ["https://signin.aws.amazon.com/saml"]
    }

    # --- Example Condition: Restrict based on a SAML attribute (e.g., 'Department') ---
    /*
    condition {
      test     = "StringEquals"
      # 'saml:AttributeName' references a specific attribute passed in the assertion.
      # The attribute name (e.g., https://example.com/SAML/Attributes/Department) depends on your IdP configuration.
      variable = "saml:https://example.com/SAML/Attributes/Department"
      values   = ["Engineering"] # Only allow users from the Engineering department
    }
    */
  }
}

# Create the IAM Role for SAML Federation
resource "aws_iam_role" "saml_federated_role" {
  name               = "tf-lab7-SAMLFederatedRole"
  path               = "/federation/"
  assume_role_policy = data.aws_iam_policy_document.saml_assume_role_policy_doc.json
  description        = "Role assumable by users federated via ExampleSAMLIdP"

  # Define permissions for the role (what users can do AFTER assuming)
  # Using a managed policy for simplicity here
  managed_policy_arns = ["arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess"]

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 7 SAML Role"
  }
}


# --- Phần 2: OpenID Connect (OIDC) Federation ---

variable "oidc_provider_url" {
  description = "URL of the OIDC identity provider (e.g., accounts.google.com, your Okta domain, GitHub Actions token URL)."
  type        = string
  default     = "https://token.actions.githubusercontent.com" # Example for GitHub Actions
  # For Google: default = "https://accounts.google.com"
  # For Okta: default = "https://your-domain.okta.com"
}

variable "oidc_client_id_list" {
  description = "List of client IDs (audiences) registered with the OIDC provider."
  type        = list(string)
  default     = ["sts.amazonaws.com"] # Typical audience for AWS access
  # For Google: ["your-google-client-id.apps.googleusercontent.com"]
  # For Okta: ["your-okta-client-id"]
}

variable "oidc_thumbprint_list" {
  description = "List of server certificate thumbprints for the OIDC provider. Obtain using openssl."
  type        = list(string)
  # IMPORTANT: Obtain the correct thumbprint for your IdP URL.
  # See AWS docs: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc_verify-thumbprint.html
  # Example thumbprint (likely invalid/outdated, DO NOT USE IN PRODUCTION):
  default = ["a031c46782e6e6c662c2c87c76da9aa62ccabd8e"] # Example thumbprint, replace with actual!
  # You might need multiple thumbprints during certificate rotation.
}

# Create the OIDC Identity Provider in IAM
resource "aws_iam_openid_connect_provider" "example_oidc_idp" {
  url = "https://${var.oidc_provider_url}" # Ensure 'https://' prefix
  client_id_list = var.oidc_client_id_list
  thumbprint_list = var.oidc_thumbprint_list

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 7 OIDC IdP"
  }
}

# Define the Trust Policy for the OIDC Role
data "aws_iam_policy_document" "oidc_assume_role_policy_doc" {
  statement {
    actions = ["sts:AssumeRoleWithWebIdentity"]
    effect  = "Allow"

    principals {
      type = "Federated"
      # Reference the ARN of the OIDC Provider created above
      identifiers = [aws_iam_openid_connect_provider.example_oidc_idp.arn]
    }

    # --- Condition: Restrict based on OIDC Audience (client ID) ---
    condition {
      test     = "StringEquals"
      # variable = "${var.oidc_provider_url}:aud" # Old format
      variable = "aws:TokenAudience" # Recommended Condition Key as of late 2023/2024
      values   = var.oidc_client_id_list
    }

    # --- Example Condition: Restrict based on OIDC Subject (e.g., GitHub Actions repo/branch) ---
    # This condition allows only specific GitHub repositories/branches to assume the role.
    /*
    condition {
      test     = "StringLike"
      # variable = "${var.oidc_provider_url}:sub" # Old format
      variable = "aws:TokenSubject" # Recommended Condition Key
      # Example: Only allow pushes to the main branch of a specific repo
      values   = ["repo:MyOrg/MyRepo:ref:refs/heads/main"]
    }
    */

     # --- Example Condition: Restrict based on a custom claim (e.g., 'groups') ---
     /*
    condition {
      test     = "StringEquals"
      # variable = "${var.oidc_provider_url}:groups" # Old format - refers to a claim named 'groups'
      variable = "aws:TokenClaim/groups" # Recommended Condition Key
      values   = ["Developers"] # Only allow if 'groups' claim contains 'Developers'
    }
    */
  }
}

# Create the IAM Role for OIDC Federation
resource "aws_iam_role" "oidc_federated_role" {
  name               = "tf-lab7-OिडCFederatedRole" # Typo intentional for illustration, fix if needed
  path               = "/federation/"
  assume_role_policy = data.aws_iam_policy_document.oidc_assume_role_policy_doc.json
  description        = "Role assumable by users/workloads federated via ExampleOIDCIdP"

  # Define permissions for the role
  managed_policy_arns = ["arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"]

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 7 OIDC Role"
  }
}

# --- Outputs ---
output "saml_provider_arn" {
  value = aws_iam_saml_provider.example_saml_idp.arn
}

output "saml_role_arn" {
  value = aws_iam_role.saml_federated_role.arn
}

output "oidc_provider_arn" {
  value = aws_iam_openid_connect_provider.example_oidc_idp.arn
}

output "oidc_role_arn" {
  value = aws_iam_role.oidc_federated_role.arn
}

output "oidc_trust_policy_json" {
  value = data.aws_iam_policy_document.oidc_assume_role_policy_doc.json
  sensitive = true
}

output "saml_trust_policy_json" {
  value = data.aws_iam_policy_document.saml_assume_role_policy_doc.json
  sensitive = true
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab7-federation
    cd lab7-federation
    touch main.tf variables.tf # Recommended
    # Copy the Terraform code into the appropriate files
    ```
2.  **Cấu hình biến (Quan trọng):**
    *   `saml_metadata_document_content`: Đối với lab này, bạn có thể giữ nguyên giá trị placeholder XML. Trong thực tế, bạn sẽ lấy tệp XML này từ IdP SAML của mình (Okta, Azure AD, ADFS, etc.).
    *   `oidc_provider_url`: Nếu muốn thử nghiệm với một IdP cụ thể (ví dụ: Google), hãy cập nhật URL. Giữ nguyên `token.actions.githubusercontent.com` là một ví dụ phổ biến cho CI/CD.
    *   `oidc_client_id_list`: Cập nhật nếu bạn đang nhắm mục tiêu một IdP cụ thể với client ID riêng. `sts.amazonaws.com` là phổ biến cho GitHub Actions.
    *   `oidc_thumbprint_list`: **RẤT QUAN TRỌNG TRONG THỰC TẾ.** Thumbprint phải khớp với chứng chỉ TLS của OIDC provider tại `oidc_provider_url`. Giá trị mặc định là ví dụ và có thể không hợp lệ. Để lấy thumbprint chính xác, hãy làm theo [hướng dẫn của AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc_verify-thumbprint.html). Đối với lab này, bạn có thể giữ nguyên giá trị mặc định để Terraform chạy thành công, nhưng IdP sẽ không hoạt động thực tế nếu thumbprint sai.
3.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
4.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Xem lại các resource `aws_iam_saml_provider`, `aws_iam_openid_connect_provider` và các `aws_iam_role` tương ứng. Kiểm tra kỹ `assume_role_policy` (Trust Policy) của các role, đặc biệt là phần `principals` tham chiếu đến ARN của provider và các `Condition` block.
5.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes`. Terraform sẽ tạo các IdP và Roles trong IAM.
6.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Tại sao dùng Federation?**
    *   **Trải nghiệm người dùng tốt hơn (SSO):** Người dùng chỉ cần đăng nhập một lần vào IdP của họ để truy cập nhiều ứng dụng, bao gồm cả AWS Console.
    *   **Quản lý danh tính tập trung:** Quản trị viên quản lý danh tính (tạo, vô hiệu hóa user, quản lý nhóm, đặt chính sách mật khẩu) tại một nơi duy nhất (IdP) thay vì phải quản lý cả trong IdP và trong AWS IAM Users.
    *   **Bảo mật nâng cao:** Không cần tạo và quản lý IAM Users với long-term credentials (access keys, passwords) trong AWS cho người dùng liên kết. Họ sử dụng thông tin xác thực tạm thời được cấp khi assume role.
    *   **Tích hợp Workload:** OIDC federation là cơ chế phổ biến để các hệ thống CI/CD (như GitHub Actions, GitLab CI), cụm Kubernetes (EKS), hoặc các ứng dụng khác có thể nhận quyền truy cập AWS tạm thời một cách an toàn mà không cần lưu trữ access keys.
*   **SAML vs OIDC:**
    *   **SAML:** Thường được sử dụng cho các kịch bản SSO từ doanh nghiệp vào web application (như AWS Console). Dựa trên XML.
    *   **OIDC:** Hiện đại hơn, dựa trên OAuth 2.0 và JSON/JWT. Phổ biến cho ứng dụng web, di động và các workload tự động (machine-to-machine).
    *   AWS hỗ trợ cả hai. Lựa chọn phụ thuộc vào IdP bạn đang sử dụng và trường hợp sử dụng cụ thể.
*   **Tầm quan trọng của Metadata/Thumbprint:** AWS cần thông tin này để xác minh tính xác thực của IdP và các assertion/token mà IdP gửi đến. Metadata SAML chứa khóa công khai của IdP để AWS có thể xác minh chữ ký số trên assertion. Thumbprint OIDC giúp AWS xác minh rằng nó đang kết nối đến đúng OIDC provider qua TLS. **Cấu hình sai các giá trị này sẽ khiến federation thất bại.**
*   **Trust Policy cho Federation:** Điểm khác biệt chính so với các role trước là `Principal` được đặt thành `Federated` và `identifiers` là ARN của `aws_iam_saml_provider` hoặc `aws_iam_openid_connect_provider`. Hành động cũng thay đổi thành `sts:AssumeRoleWithSAML` hoặc `sts:AssumeRoleWithWebIdentity`.
*   **Conditions với Attributes/Claims:** Đây là nơi sức mạnh thực sự của federation thể hiện. Bằng cách sử dụng các condition key như `saml:AttributeName` hoặc `oidc:ClaimName` (hoặc `aws:TokenClaim/ClaimName`), bạn có thể tạo các role với quyền truy cập khác nhau dựa trên thông tin mà IdP gửi về người dùng (ví dụ: vai trò, phòng ban, dự án). Điều này cho phép thực hiện **Fine-Grained Access Control** dựa trên danh tính được quản lý bên ngoài AWS.
*   **Terraform và IdP:** Terraform giúp tự động hóa việc đăng ký IdP và tạo các role liên kết, đảm bảo cấu hình nhất quán và dễ dàng cập nhật khi metadata IdP thay đổi.

### Xác minh

Vì chúng ta không cấu hình IdP thực tế, việc xác minh chủ yếu tập trung vào kiểm tra các tài nguyên đã được tạo đúng trong IAM.

1.  **AWS Management Console:**
    *   Đi đến **IAM**.
    *   Trong bảng điều hướng bên trái, chọn **Identity providers**. Bạn sẽ thấy `tf-lab7-ExampleSAMLIdP` và OIDC provider (tên sẽ là URL bạn cung cấp, ví dụ: `token.actions.githubusercontent.com`). Click vào từng provider để xem chi tiết (như Audience/Client ID cho OIDC).
    *   Đi đến **Roles**. Tìm các role `tf-lab7-SAMLFederatedRole` và `tf-lab7-OिडCFederatedRole`.
    *   Click vào từng role.
        *   Kiểm tra tab "Permissions": Đảm bảo policy quyền hạn (ví dụ: `AmazonEC2ReadOnlyAccess`) được gắn.
        *   Kiểm tra tab "Trust relationships": **Quan trọng:** Xác minh Trust Policy. Principal phải là `Federated` và tham chiếu đến ARN của IdP tương ứng. Kiểm tra các `Condition` block bạn đã định nghĩa.
2.  **AWS CLI:**
    ```bash
    # Liệt kê SAML providers
    aws iam list-saml-providers

    # Lấy thông tin SAML provider cụ thể (cần ARN)
    SAML_PROVIDER_ARN=$(terraform output -raw saml_provider_arn)
    aws iam get-saml-provider --saml-provider-arn $SAML_PROVIDER_ARN

    # Liệt kê OIDC providers
    aws iam list-open-id-connect-providers

    # Lấy thông tin OIDC provider cụ thể (cần ARN)
    OIDC_PROVIDER_ARN=$(terraform output -raw oidc_provider_arn)
    aws iam get-open-id-connect-provider --open-id-connect-provider-arn $OIDC_PROVIDER_ARN

    # Kiểm tra SAML role và trust policy
    SAML_ROLE_NAME=$(aws iam get-role --role-arn $(terraform output -raw saml_role_arn) --query 'Role.RoleName' --output text)
    aws iam get-role --role-name $SAML_ROLE_NAME --query 'Role.AssumeRolePolicyDocument'

    # Kiểm tra OIDC role và trust policy
    OIDC_ROLE_NAME=$(aws iam get-role --role-arn $(terraform output -raw oidc_role_arn) --query 'Role.RoleName' --output text)
    aws iam get-role --role-name $OIDC_ROLE_NAME --query 'Role.AssumeRolePolicyDocument'
    ```

*(Thử nghiệm thực tế):* Để thử nghiệm federation đầy đủ, bạn cần:
1.  Cấu hình một IdP thực tế (ví dụ: dùng tài khoản Google làm OIDC IdP, hoặc thiết lập một ứng dụng SAML trong Okta/Azure AD).
2.  Cập nhật Terraform code với metadata/thumbprint/client ID chính xác từ IdP đó.
3.  Thiết lập ứng dụng hoặc sử dụng công cụ/trang web hỗ trợ luồng đăng nhập SAML/OIDC để lấy assertion/token.
4.  Gọi API `sts:AssumeRoleWithSAML` hoặc `sts:AssumeRoleWithWebIdentity` với assertion/token và ARN của Role để nhận credentials tạm thời.
5.  Sử dụng credentials tạm thời để gọi API AWS.

### Dọn dẹp

1.  **Chạy lệnh destroy:**
    Trong thư mục `lab7-federation`, chạy:
    ```bash
    terraform destroy
    ```
2.  **Xác nhận:** Nhập `yes`. Terraform sẽ xóa Roles và Identity Providers.
3.  **Xác minh dọn dẹp (Tùy chọn):** Sử dụng Console hoặc CLI.

---

Lab 7 đã cung cấp cho bạn cái nhìn tổng quan về Identity Federation và cách thiết lập các thành phần cơ bản (IAM Identity Provider và Role) bằng Terraform. Bạn đã hiểu cách AWS tích hợp với các hệ thống danh tính bên ngoài và tiềm năng của việc sử dụng thuộc tính/claims để kiểm soát truy cập. Lab tiếp theo sẽ tập trung sâu hơn vào Attribute-Based Access Control (ABAC) sử dụng tags, một phương pháp mạnh mẽ để quản lý quyền ở quy mô lớn.
## Lab 8: Attribute-Based Access Control (ABAC) với Tags

### Mục tiêu

*   Hiểu rõ hơn về chiến lược Kiểm soát Truy cập Dựa trên Thuộc tính (Attribute-Based Access Control - ABAC).
*   Sử dụng tags trên cả principal (User/Role) và tài nguyên (ví dụ: EC2 instance) làm thuộc tính (attributes).
*   Viết IAM policy sử dụng các Condition key liên quan đến tag:
    *   `aws:PrincipalTag/<tag-key>`: Kiểm tra tag trên User/Role đang thực hiện yêu cầu.
    *   `aws:ResourceTag/<tag-key>`: Kiểm tra tag trên tài nguyên đang được truy cập.
    *   `aws:RequestTag/<tag-key>`: Kiểm tra tag được cung cấp *trong chính yêu cầu* API (ví dụ: khi tạo tài nguyên mới).
    *   `aws:TagKeys`: Kiểm tra xem các key tag cụ thể có được phép sử dụng trong yêu cầu hay không.
*   Triển khai kịch bản ABAC phổ biến: Cho phép user chỉ quản lý các tài nguyên có cùng tag "Project" với user đó.
*   Sử dụng Terraform để quản lý tags trên principals và định nghĩa các policy ABAC.

### Điều kiện tiên quyết

*   Hoàn thành Lab 5 (Conditions & Context Keys), nơi đã giới thiệu `aws:PrincipalTag`.
*   Hiểu về tagging trong AWS và Terraform.
*   Kiến thức cơ bản về EC2 (để hiểu tài nguyên đích).

### Khái niệm được đề cập

*   **Attribute-Based Access Control (ABAC):** Một mô hình ủy quyền trong đó quyền truy cập vào tài nguyên được xác định bằng cách đánh giá các *thuộc tính* (attributes) liên quan đến người dùng (principal), tài nguyên (resource), môi trường (environment), và bản thân hành động (action). Trong AWS IAM, **tags** là cách triển khai thuộc tính phổ biến nhất.
*   **Tags as Attributes:** Bạn có thể gắn các cặp key-value (tags) vào IAM principals (Users, Roles) và nhiều loại tài nguyên AWS (EC2, S3, RDS, etc.). Các tags này trở thành thuộc tính có thể được sử dụng trong các điều kiện của IAM policy.
*   **Scalability & Flexibility of ABAC:** So với mô hình Role-Based Access Control (RBAC) truyền thống (nơi bạn tạo nhiều role cho các bộ quyền khác nhau), ABAC có thể **linh hoạt và dễ mở rộng hơn** trong các môi trường lớn và thay đổi nhanh chóng. Thay vì tạo hàng trăm role, bạn có thể tạo một số ít policy ABAC chung và kiểm soát quyền truy cập bằng cách quản lý tags trên principals và resources. Khi một dự án mới được tạo hoặc nhân viên chuyển nhóm, bạn chỉ cần cập nhật tags thay vì phải tạo/gán lại roles/policies.
*   **ABAC Condition Keys:**
    *   `aws:PrincipalTag/key`: Giá trị của tag `key` trên User/Role thực hiện yêu cầu.
    *   `aws:ResourceTag/key`: Giá trị của tag `key` trên tài nguyên mà hành động đang nhắm tới. **Quan trọng:** Không phải tất cả các hành động AWS API đều hỗ trợ đánh giá `aws:ResourceTag`. Thường áp dụng cho các hành động thực hiện trên một tài nguyên *cụ thể* đã tồn tại và có tag.
    *   `aws:RequestTag/key`: Giá trị của tag `key` được truyền vào *trong chính yêu cầu API*. Hữu ích để kiểm soát việc gắn tag khi tạo tài nguyên mới (ví dụ: bắt buộc phải gắn tag `Project` khi tạo EC2 instance).
    *   `aws:TagKeys`: Danh sách các key của tag được truyền vào trong yêu cầu API. Hữu ích để kiểm soát *những key tag nào* được phép gắn hoặc sửa đổi.
*   **Common ABAC Use Case (Project-based access):** Cho phép user chỉ tương tác (ví dụ: start/stop/terminate) với các EC2 instances có cùng giá trị tag `Project` (hoặc `CostCenter`, `Environment`, etc.) với tag `Project` của chính user đó.
*   **Terraform for ABAC:** Terraform rất phù hợp để triển khai ABAC vì bạn có thể:
    *   Định nghĩa việc gắn tags cho cả principals (`aws_iam_user`, `aws_iam_role`) và resources (`aws_instance`, `aws_s3_bucket`, etc.) trong cùng một mã nguồn IaC.
    *   Sử dụng biến và vòng lặp để tạo các policy ABAC động và tái sử dụng được.

### Cấu hình Terraform

Tạo thư mục `lab8-abac`. Chúng ta sẽ tạo 2 user với các tag `Project` khác nhau và một policy ABAC chung áp dụng cho cả hai.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured
}

# --- Input Variables (for flexibility) ---
variable "project_tag_key" {
  description = "The tag key used for project-based access control."
  type        = string
  default     = "Project"
}

variable "cost_center_tag_key" {
  description = "The tag key used for cost center enforcement."
  type        = string
  default     = "CostCenter"
}

# --- Create Users with Different Project Tags ---
resource "aws_iam_user" "developer_blue" {
  name = "tf-lab8-dev-blue"
  path = "/users/abac/"

  tags = {
    ManagedBy                 = "Terraform"
    Purpose                   = "Lab 8 ABAC User Blue"
    (var.project_tag_key)     = "Blue" # User belongs to Project Blue
    (var.cost_center_tag_key) = "B123"
  }
}

resource "aws_iam_user" "developer_green" {
  name = "tf-lab8-dev-green"
  path = "/users/abac/"

  tags = {
    ManagedBy                 = "Terraform"
    Purpose                   = "Lab 8 ABAC User Green"
    (var.project_tag_key)     = "Green" # User belongs to Project Green
    (var.cost_center_tag_key) = "G456"
  }
}

# --- Define the ABAC Policy ---
# This single policy will be attached to both users (or a group containing them).
data "aws_iam_policy_document" "abac_ec2_project_access_policy_doc" {

  # Statement 1: Allow describing all instances (often needed for context)
  statement {
    sid    = "AllowEC2DescribeAll"
    effect = "Allow"
    actions = [
      "ec2:DescribeInstances",
      "ec2:DescribeVolumes",
      "ec2:DescribeTags" # Allows seeing tags, useful for context
    ]
    resources = ["*"] # Describe actions often require '*'
  }

  # Statement 2: Allow Start/Stop/Reboot actions ONLY on instances matching the user's Project tag
  statement {
    sid    = "AllowStartStopRebootIfProjectMatches"
    effect = "Allow"
    actions = [
      "ec2:StartInstances",
      "ec2:StopInstances",
      "ec2:RebootInstances"
    ]
    # Apply to all EC2 instances initially...
    resources = ["arn:aws:ec2:${provider.aws.region}:${data.aws_caller_identity.current.account_id}:instance/*"]
    # ...but add a CONDITION based on tags
    condition {
      test     = "StringEquals"
      # Check the instance's Project tag against the user's Project tag
      variable = "ec2:ResourceTag/${var.project_tag_key}" # Key for EC2 Resource Tag
      values   = ["${aws:PrincipalTag/${var.project_tag_key}}"] # Value references the Principal's tag value!
      # Note: Using ${aws:...} interpolation directly in the values list.
    }
  }

  # Statement 3: Allow creating new instances BUT enforce tagging
  statement {
    sid    = "AllowCreateEC2InstanceWithMandatoryTags"
    effect = "Allow"
    actions = [
      "ec2:RunInstances" # Allows launching new instances
    ]
    # Resource scope for RunInstances is complex (Network Interfaces, Volumes, etc.)
    # Using '*' here for simplicity, but restrict further in production (e.g., by VPC, subnet, AMI).
    resources = ["*"]

    # Condition 3a: Require the 'Project' tag in the request, matching the user's project
    condition {
      test     = "StringEquals"
      variable = "aws:RequestTag/${var.project_tag_key}" # Check tag provided in the API request
      values   = ["${aws:PrincipalTag/${var.project_tag_key}}"]
    }
    # Condition 3b: Require the 'CostCenter' tag in the request, matching the user's cost center
    condition {
        test     = "StringEquals"
        variable = "aws:RequestTag/${var.cost_center_tag_key}"
        values   = ["${aws:PrincipalTag/${var.cost_center_tag_key}}"]
    }
    # Condition 3c: Ensure that ONLY the allowed tag keys ('Project', 'CostCenter', maybe 'Name') are provided
    # This prevents users from adding arbitrary tags during creation.
    condition {
        test     = "ForAllValues:StringEquals" # Check ALL tag keys in the request
        variable = "aws:TagKeys" # Key representing all tag keys in the request
        values   = [var.project_tag_key, var.cost_center_tag_key, "Name"] # Allowed tag keys
    }
     # Condition 3d: Optional - prevent users from overriding the required tags via TagSpecifications
     condition {
        test = "Bool"
        variable = "aws:TagKeys"
        values = ["true"]
     }
     # Note: The combination of aws:RequestTag and aws:TagKeys is crucial for enforcing tagging on creation.
  }

  # Statement 4: Allow creating/deleting tags BUT only for the user's project and allowed keys
  statement {
    sid = "AllowTaggingResourcesForOwnProject"
    effect = "Allow"
    actions = [
      "ec2:CreateTags",
      "ec2:DeleteTags"
    ]
    resources = ["arn:aws:ec2:${provider.aws.region}:${data.aws_caller_identity.current.account_id}:instance/*"] # Or other taggable resources

    # Condition 4a: Only allow tagging resources that ALREADY have the user's Project tag
    # (Prevents users from tagging other projects' resources)
    condition {
       test = "StringEquals"
       variable = "ec2:ResourceTag/${var.project_tag_key}"
       values = ["${aws:PrincipalTag/${var.project_tag_key}}"]
    }
    # Condition 4b: Only allow creating/deleting the allowed tag keys
    # (Prevents users from modifying sensitive tags or adding arbitrary ones)
    condition {
       test = "ForAllValues:StringEquals"
       variable = "aws:TagKeys"
       values = [var.project_tag_key, var.cost_center_tag_key, "Name"] # Allowed tag keys
    }
     # Condition 4c: When creating tags, the RequestTag must also match the user's project
     # This adds another layer for CreateTags action specifically.
    condition {
       test = "StringEquals"
       variable = "aws:RequestTag/${var.project_tag_key}"
       values = ["${aws:PrincipalTag/${var.project_tag_key}}"]
    }
  }
}

# Create the Customer Managed ABAC Policy
resource "aws_iam_policy" "abac_ec2_policy" {
  name        = "tf-lab8-ABACEC2ProjectAccessPolicy"
  path        = "/abac/"
  description = "ABAC policy allowing EC2 actions based on Project tag"
  policy      = data.aws_iam_policy_document.abac_ec2_project_access_policy_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 8 ABAC Policy"
    Type      = "ABAC"
  }
}

# --- Attach policy to users (or ideally, a group) ---
# For simplicity, attaching directly to users here.
# In practice, create a group, add users, attach policy to the group.
resource "aws_iam_user_policy_attachment" "attach_abac_to_blue" {
  user       = aws_iam_user.developer_blue.name
  policy_arn = aws_iam_policy.abac_ec2_policy.arn
}

resource "aws_iam_user_policy_attachment" "attach_abac_to_green" {
  user       = aws_iam_user.developer_green.name
  policy_arn = aws_iam_policy.abac_ec2_policy.arn
}

# Get caller identity for Account ID (used in policy ARN)
data "aws_caller_identity" "current" {}


# --- Outputs ---
output "developer_blue_name" {
  value = aws_iam_user.developer_blue.name
}
output "developer_blue_tags" {
  value = aws_iam_user.developer_blue.tags
}
output "developer_green_name" {
  value = aws_iam_user.developer_green.name
}
output "developer_green_tags" {
  value = aws_iam_user.developer_green.tags
}
output "abac_policy_arn" {
  value = aws_iam_policy.abac_ec2_policy.arn
}
output "abac_policy_json" {
  value = data.aws_iam_policy_document.abac_ec2_project_access_policy_doc.json
  sensitive = true
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab8-abac
    cd lab8-abac
    touch main.tf variables.tf # Optional
    # Copy the Terraform code into the appropriate files
    ```
2.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
3.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Kiểm tra kỹ:
    *   Tags được gán cho `aws_iam_user.developer_blue` và `aws_iam_user.developer_green`.
    *   Nội dung của `aws_iam_policy.abac_ec2_policy`, đặc biệt là các `Condition` block sử dụng `aws:PrincipalTag`, `ec2:ResourceTag`, `aws:RequestTag`, và `aws:TagKeys`.
    *   Policy được gắn vào cả hai user.
4.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes`.
5.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Policy ABAC hoạt động như thế nào:**
    *   **Statement 1 (Describe):** Cho phép mọi người xem thông tin cơ bản để có ngữ cảnh.
    *   **Statement 2 (Start/Stop/Reboot):** Hành động chỉ được phép (`Allow`) NẾU (`Condition`) giá trị tag `Project` trên instance (`ec2:ResourceTag/Project`) bằng với giá trị tag `Project` trên user (`aws:PrincipalTag/Project`). User "Blue" chỉ có thể start/stop instance "Blue", user "Green" chỉ có thể start/stop instance "Green".
    *   **Statement 3 (RunInstances - Tạo mới):** Cho phép tạo instance, NHƯNG yêu cầu (`Condition`) người dùng phải cung cấp tag `Project` và `CostCenter` *trong yêu cầu tạo* (`aws:RequestTag/...`) và các giá trị này phải khớp với tag của chính user (`aws:PrincipalTag/...`). Đồng thời, `aws:TagKeys` đảm bảo họ chỉ sử dụng các tag keys được phép (`Project`, `CostCenter`, `Name`), ngăn chặn việc thêm tag tùy tiện khi tạo.
    *   **Statement 4 (Create/Delete Tags):** Cho phép quản lý tags, NHƯNG chỉ trên các instance đã thuộc dự án của họ (`ec2:ResourceTag/Project` == `aws:PrincipalTag/Project`) VÀ chỉ cho phép thao tác với các tag keys đã được phê duyệt (`aws:TagKeys`). Điều kiện `aws:RequestTag/Project` thêm một lớp kiểm tra cho hành động `CreateTags`.
*   **Ưu điểm của ABAC:**
    *   **Ít Policy hơn:** Bạn chỉ cần một policy ABAC chung thay vì nhiều policy RBAC cho từng dự án/nhóm.
    *   **Quản lý linh hoạt:** Khi thêm dự án mới hoặc user chuyển nhóm, chỉ cần cập nhật tags. Không cần sửa đổi IAM policies (trừ khi logic ABAC thay đổi).
    *   **Tự phục vụ (Self-service) an toàn:** Kết hợp với Permissions Boundaries (Lab 6), bạn có thể cho phép các nhóm tự quản lý tài nguyên trong phạm vi dự án của họ một cách an toàn.
*   **Thách thức của ABAC:**
    *   **Chiến lược Tagging:** Yêu cầu một chiến lược tagging nhất quán và được thực thi tốt trong toàn bộ tổ chức.
    *   **Gỡ lỗi Policy:** Việc hiểu và gỡ lỗi các policy ABAC phức tạp với nhiều điều kiện có thể khó khăn hơn RBAC ban đầu. Policy Simulator (đã dùng ở Lab 6) là công cụ hữu ích.
    *   **Hỗ trợ Resource Tag:** Không phải mọi hành động AWS đều hỗ trợ điều kiện `aws:ResourceTag`. Bạn cần kiểm tra tài liệu IAM cho từng dịch vụ/hành động. Các hành động tạo tài nguyên thường dựa vào `aws:RequestTag` và `aws:TagKeys`.
*   **`"${aws:PrincipalTag/Project}"`:** Đây là cú pháp đặc biệt trong IAM policy JSON để tham chiếu đến giá trị của một key khác trong ngữ cảnh yêu cầu (trong trường hợp này là tag của principal). Nó cho phép so sánh động giữa các thuộc tính.
*   **Terraform và Tagging:** Việc quản lý tags nhất quán qua Terraform là yếu tố then chốt để ABAC hoạt động hiệu quả. Bạn có thể sử dụng các biến, `locals`, hoặc module để chuẩn hóa việc áp dụng tags.

### Xác minh

Việc xác minh ABAC đòi hỏi phải có cả principals và resources với các tag phù hợp.

1.  **Kiểm tra Cấu hình IAM:**
    *   **AWS Console:**
        *   Kiểm tra tags trên user `tf-lab8-dev-blue` và `tf-lab8-dev-green`.
        *   Kiểm tra policy `tf-lab8-ABACEC2ProjectAccessPolicy` và các điều kiện của nó.
        *   Xác nhận policy được gắn vào cả hai user.
    *   **AWS CLI:**
        ```bash
        USER_BLUE=$(terraform output -raw developer_blue_name)
        USER_GREEN=$(terraform output -raw developer_green_name)
        POLICY_ARN=$(terraform output -raw abac_policy_arn)

        aws iam list-user-tags --user-name $USER_BLUE
        aws iam list-user-tags --user-name $USER_GREEN
        aws iam get-policy-version --policy-arn $POLICY_ARN --version-id v1 --query 'PolicyVersion.Document' | jq
        aws iam list-attached-user-policies --user-name $USER_BLUE
        aws iam list-attached-user-policies --user-name $USER_GREEN
        ```

2.  **Thử nghiệm Hành vi (Yêu cầu tạo EC2 instances với tags):**
    *   **Bước chuẩn bị:**
        *   Tạo access keys cho cả `developer_blue` và `developer_green` qua Console.
        *   Cấu hình 2 profile CLI riêng biệt: `lab8-blue` và `lab8-green`.
        *   **Quan trọng:** Sử dụng tài khoản AWS của bạn (với quyền admin hoặc đủ quyền EC2) để **tạo thủ công 2 EC2 instance** (ví dụ: t2.micro Amazon Linux 2):
            *   Instance 1: Gắn tag `Project = Blue` và `CostCenter = B123`. Ghi lại Instance ID (ví dụ: `i-blue123`).
            *   Instance 2: Gắn tag `Project = Green` và `CostCenter = G456`. Ghi lại Instance ID (ví dụ: `i-green456`).
    *   **Chạy thử nghiệm (Ví dụ với user Blue):**
        ```bash
        # User Blue cố gắng Stop instance Blue (Nên thành công)
        aws ec2 stop-instances --instance-ids i-blue123 --profile lab8-blue --region us-east-1

        # User Blue cố gắng Stop instance Green (Nên thất bại - Access Denied do ResourceTag)
        aws ec2 stop-instances --instance-ids i-green456 --profile lab8-blue --region us-east-1

        # User Blue cố gắng tạo instance mới với tag Project=Blue và CostCenter=B123 (Nên thành công)
        # Thay ami-id bằng một AMI hợp lệ trong region của bạn
        aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --count 1 \
          --tag-specifications 'ResourceType=instance,Tags=[{Key=Project,Value=Blue},{Key=CostCenter,Value=B123},{Key=Name,Value=BlueTest}]' \
          --profile lab8-blue --region us-east-1 --dry-run

        # User Blue cố gắng tạo instance mới với tag Project=Green (Nên thất bại - Access Denied do RequestTag)
        aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --count 1 \
          --tag-specifications 'ResourceType=instance,Tags=[{Key=Project,Value=Green},{Key=CostCenter,Value=B123},{Key=Name,Value=BlueTestWrongProj}]' \
          --profile lab8-blue --region us-east-1 --dry-run

        # User Blue cố gắng tạo instance mới thiếu tag CostCenter (Nên thất bại - Access Denied do RequestTag)
        aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --count 1 \
          --tag-specifications 'ResourceType=instance,Tags=[{Key=Project,Value=Blue},{Key=Name,Value=BlueTestNoCC}]' \
          --profile lab8-blue --region us-east-1 --dry-run

        # User Blue cố gắng tạo instance mới với tag tùy tiện (Nên thất bại - Access Denied do TagKeys)
        aws ec2 run-instances --image-id ami-0c55b159cbfafe1f0 --instance-type t2.micro --count 1 \
          --tag-specifications 'ResourceType=instance,Tags=[{Key=Project,Value=Blue},{Key=CostCenter,Value=B123},{Key=Arbitrary,Value=Yes}]' \
          --profile lab8-blue --region us-east-1 --dry-run

        # User Blue cố gắng thêm tag 'Name' vào instance Blue (Nên thành công)
        aws ec2 create-tags --resources i-blue123 --tags Key=Name,Value=BlueInstance --profile lab8-blue --region us-east-1

        # User Blue cố gắng thêm tag 'Arbitrary' vào instance Blue (Nên thất bại - Access Denied do TagKeys)
        aws ec2 create-tags --resources i-blue123 --tags Key=Arbitrary,Value=No --profile lab8-blue --region us-east-1

        # User Blue cố gắng thêm tag 'Name' vào instance Green (Nên thất bại - Access Denied do ResourceTag)
        aws ec2 create-tags --resources i-green456 --tags Key=Name,Value=TryingToTagGreen --profile lab8-blue --region us-east-1
        ```
    *   Lặp lại các thử nghiệm tương tự với profile `lab8-green`, đảo ngược vai trò của instance Blue và Green.
    *   **Quan trọng:** Nhớ xóa access keys và các instance EC2 đã tạo thủ công sau khi thử nghiệm.

### Dọn dẹp

1.  **Xóa Access Keys:** Xóa access keys của `developer_blue` và `developer_green` trong Console.
2.  **Chấm dứt EC2 Instances (Nếu đã tạo):** Chấm dứt các instance `i-blue123` và `i-green456` mà bạn đã tạo thủ công.
3.  **Chạy lệnh destroy:**
    Trong thư mục `lab8-abac`, chạy:
    ```bash
    terraform destroy
    ```
4.  **Xác nhận:** Nhập `yes`.
5.  **Xóa profile CLI (Tùy chọn):** Xóa profile `lab8-blue` và `lab8-green`.

---

Chúc mừng bạn đã hoàn thành Lab 8! Bạn đã triển khai một hệ thống ABAC cơ bản nhưng mạnh mẽ bằng Terraform, sử dụng tags để kiểm soát quyền truy cập EC2 dựa trên dự án. Bạn đã thấy cách ABAC có thể đơn giản hóa việc quản lý quyền trong môi trường phức tạp. Lab tiếp theo sẽ giới thiệu về IAM Access Analyzer, một công cụ giúp bạn xác định các quyền truy cập không mong muốn.
## Lab 9: Sử dụng IAM Access Analyzer

### Mục tiêu

*   Hiểu mục đích và lợi ích của AWS IAM Access Analyzer.
*   Sử dụng Terraform để tạo một IAM Access Analyzer cho tài khoản hoặc tổ chức (ở đây tập trung vào tài khoản).
*   Phân tích các loại phát hiện (finding types) chính của Access Analyzer, đặc biệt là quyền truy cập từ bên ngoài (External Access).
*   Tạo một tài nguyên có policy cho phép truy cập từ bên ngoài (ví dụ: S3 bucket policy cho phép truy cập public hoặc cross-account không mong muốn).
*   Quan sát cách Access Analyzer tạo ra một phát hiện (finding) cho tài nguyên đó.
*   Sử dụng Terraform data source `aws_accessanalyzer_analyzer` để tham chiếu analyzer hiện có (nếu cần).
*   (Tùy chọn) Tạo quy tắc lưu trữ (archive rule) bằng Terraform để tự động bỏ qua các phát hiện dự kiến.

### Điều kiện tiên quyết

*   Hoàn thành các Lab trước, đặc biệt là Lab 2 (Custom Policies) và Lab 4 (Cross-Account Roles).
*   Hiểu về Resource-based Policies (ví dụ: S3 Bucket Policies).

### Khái niệm được đề cập

*   **AWS IAM Access Analyzer:** Một dịch vụ AWS giúp bạn xác định các tài nguyên trong tài khoản của mình, như S3 buckets hoặc IAM roles, được chia sẻ với một thực thể bên ngoài (external entity). Thực thể bên ngoài có thể là một tài khoản AWS khác, một root user, một IAM user hoặc role, một dịch vụ AWS, hoặc người dùng internet ẩn danh. Access Analyzer giúp bạn xác định quyền truy cập ngoài ý muốn hoặc không cần thiết để bạn có thể thực hiện hành động khắc phục, tuân thủ **nguyên tắc đặc quyền tối thiểu**.
*   **Analyzer:** Một tài nguyên bạn tạo trong Access Analyzer để cho phép phân tích trong một vùng tin cậy (zone of trust) cụ thể. Vùng tin cậy thường là tài khoản AWS của bạn hoặc toàn bộ AWS Organization của bạn. Bạn cần tạo Analyzer cho mỗi Region mà bạn muốn phân tích tài nguyên.
*   **Phát hiện (Finding):** Kết quả do Access Analyzer tạo ra khi nó xác định được một policy (resource-based hoặc role trust policy) cấp quyền truy cập cho một principal bên ngoài vùng tin cậy của bạn. Mỗi finding cung cấp thông tin chi tiết về tài nguyên, principal bên ngoài, và các quyền được cấp.
*   **Các loại Finding:**
    *   **External Access:** Quyền truy cập được cấp cho principal *bên ngoài* vùng tin cậy của bạn (ví dụ: tài khoản AWS khác, public access). Đây là trọng tâm chính của Access Analyzer.
    *   **Unused Access (Truy cập không sử dụng):** (Tính năng nâng cao hơn, thường xem trong Console) Phân tích quyền IAM không sử dụng (cho user/role) dựa trên lịch sử CloudTrail, giúp loại bỏ quyền thừa.
*   **Trạng thái Finding:**
    *   `ACTIVE`: Phát hiện đang hoạt động, cho thấy quyền truy cập vẫn tồn tại.
    *   `ARCHIVED`: Phát hiện đã được bạn xem xét và đánh dấu là dự kiến hoặc chấp nhận được. Access Analyzer sẽ không báo cáo lại trừ khi có thay đổi.
    *   `RESOLVED`: Quyền truy cập gây ra phát hiện đã bị xóa hoặc thay đổi, làm cho nó không còn hợp lệ.
*   **Quy tắc Lưu trữ (Archive Rule):** Cho phép bạn tự động lưu trữ các phát hiện dựa trên các tiêu chí cụ thể (ví dụ: luôn lưu trữ phát hiện về quyền truy cập từ một tài khoản đối tác đáng tin cậy cụ thể).
*   **Phân tích dựa trên Logic Toán học (Mathematical Logic):** Access Analyzer không chỉ quét đơn giản. Nó sử dụng suy luận tự động và logic toán học để phân tích *tất cả* các đường dẫn truy cập có thể có được định nghĩa trong các policy, cung cấp một phân tích toàn diện hơn là chỉ kiểm tra các mẫu cụ thể.
*   **Terraform Resources & Data Sources:**
    *   `aws_accessanalyzer_analyzer`: Tạo và quản lý một Analyzer.
    *   `aws_accessanalyzer_archive_rule`: Tạo và quản lý một Archive Rule cho một Analyzer.
    *   `data "aws_accessanalyzer_analyzer"`: Lấy thông tin về một Analyzer hiện có (hữu ích nếu Analyzer được tạo bên ngoài Terraform hoặc trong một module khác).
    *   `aws_s3_bucket` và `aws_s3_bucket_policy`: (Ví dụ) Dùng để tạo tài nguyên có policy gây ra phát hiện.

### Cấu hình Terraform

Tạo thư mục `lab9-access-analyzer`. Chúng ta sẽ tạo Analyzer, một S3 bucket, và một bucket policy cố tình cho phép truy cập cross-account không an toàn để Access Analyzer có thể phát hiện.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    # Using random provider to generate unique bucket names
    random = {
      source  = "hashicorp/random"
      version = "~> 3.1"
    }
  }
}

provider "aws" {
  region = "us-east-1" # Access Analyzer is regional
  # Assume credentials are configured
}

provider "random" {
  # No configuration needed
}

# Get caller identity for Account ID
data "aws_caller_identity" "current" {}

# Generate a random suffix for the bucket name to ensure uniqueness
resource "random_string" "bucket_suffix" {
  length  = 8
  special = false
  upper   = false
}

# --- Step 1: Create the IAM Access Analyzer ---
# You typically only need one analyzer per account per region.
resource "aws_accessanalyzer_analyzer" "account_analyzer" {
  analyzer_name = "tf-lab9-AccountAnalyzer"
  type          = "ACCOUNT" # Analyze the current account. Use "ORGANIZATION" for org-wide analysis.

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 9 Access Analyzer"
  }
  # Note: Creating an analyzer doesn't incur direct costs, but enables findings.
}

# --- Step 2: Create a resource with a problematic policy ---
# Example: S3 bucket with a policy allowing access from another specific AWS account
#          (Imagine this other account ID is unintended or unknown)

variable "untrusted_account_id" {
  description = "A placeholder for an AWS Account ID that should NOT have access."
  type        = string
  default     = "111122223333" # Replace with a different dummy ID if needed
}

resource "aws_s3_bucket" "test_bucket" {
  # Bucket names must be globally unique
  bucket = "tf-lab9-accessanalyzer-test-${random_string.bucket_suffix.result}"

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 9 Test Bucket for Findings"
    Sensitive = "Yes" # Example tag indicating data sensitivity
  }
}

# Define the problematic S3 Bucket Policy
data "aws_iam_policy_document" "problematic_s3_policy_doc" {
  statement {
    sid    = "AllowUnintendedCrossAccountRead"
    effect = "Allow"
    principals {
      type = "AWS"
      # Granting access to the root of another AWS account
      identifiers = ["arn:aws:iam::${var.untrusted_account_id}:root"]
    }
    actions = [
      "s3:GetObject",
      "s3:ListBucket"
    ]
    resources = [
      aws_s3_bucket.test_bucket.arn,
      "${aws_s3_bucket.test_bucket.arn}/*",
    ]
  }
}

# Apply the policy to the bucket
resource "aws_s3_bucket_policy" "test_bucket_policy" {
  bucket = aws_s3_bucket.test_bucket.id
  policy = data.aws_iam_policy_document.problematic_s3_policy_doc.json

  depends_on = [aws_s3_bucket.test_bucket] # Ensure bucket exists first
}

# --- Step 3: (Optional) Create an Archive Rule ---
# Example: Automatically archive findings related to the 'untrusted' account
#          (Useful if, despite the name, this access IS actually intended and reviewed)
/*
resource "aws_accessanalyzer_archive_rule" "archive_untrusted_account_findings" {
  analyzer_name = aws_accessanalyzer_analyzer.account_analyzer.analyzer_name
  rule_name     = "ArchiveKnownPartnerAccess-${var.untrusted_account_id}"

  filter {
    # Criteria to match the finding: Principal is the untrusted account ID
    criteria = "principal.AWS"
    eq       = [var.untrusted_account_id] # Use 'contains' for broader matching if needed
  }

  filter {
     # Criteria to match the finding: Resource is owned by the current account
    criteria = "resourceOwnerAccount"
    eq = [data.aws_caller_identity.current.account_id]
   }

   # You can add more filters (e.g., based on resource type, condition keys)

   depends_on = [aws_accessanalyzer_analyzer.account_analyzer]
}
*/

# --- Outputs ---
output "analyzer_arn" {
  value = aws_accessanalyzer_analyzer.account_analyzer.arn
}

output "analyzer_name" {
  value = aws_accessanalyzer_analyzer.account_analyzer.analyzer_name
}

output "test_bucket_name" {
  value = aws_s3_bucket.test_bucket.bucket
}

output "test_bucket_policy_json" {
  value = data.aws_iam_policy_document.problematic_s3_policy_doc.json
  sensitive = true
}

output "access_analyzer_console_link" {
   value = "https://${provider.aws.region}.console.aws.amazon.com/access-analyzer/home?region=${provider.aws.region}#/findings?analyzerName=${aws_accessanalyzer_analyzer.account_analyzer.analyzer_name}"
   description = "Direct link to the Access Analyzer findings console for this analyzer."
}

/*
output "archive_rule_name" {
  value = aws_accessanalyzer_archive_rule.archive_untrusted_account_findings.rule_name
  description = "(If enabled) The name of the created archive rule."
}
*/
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab9-access-analyzer
    cd lab9-access-analyzer
    touch main.tf variables.tf # Optional
    # Copy the Terraform code into the appropriate files
    ```
2.  **Cấu hình biến (Nếu cần):**
    *   `untrusted_account_id`: Bạn có thể giữ nguyên giá trị mặc định `111122223333` vì đây chỉ là placeholder cho một tài khoản không xác định/không tin cậy.
3.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
4.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Xem lại các tài nguyên: `aws_accessanalyzer_analyzer`, `aws_s3_bucket`, `aws_s3_bucket_policy`. (Và `aws_accessanalyzer_archive_rule` nếu bạn bỏ comment).
5.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes`. Terraform sẽ tạo Analyzer và bucket với policy.
6.  **Chờ Access Analyzer quét:** Sau khi tài nguyên được tạo, Access Analyzer sẽ tự động bắt đầu quét các policy được hỗ trợ. Quá trình này có thể mất vài phút (đôi khi lên đến 30 phút, nhưng thường nhanh hơn cho các thay đổi nhỏ).
7.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Tại sao dùng Access Analyzer?** Nó tự động hóa việc kiểm tra bảo mật quan trọng là xác định quyền truy cập từ bên ngoài không mong muốn. Việc kiểm tra thủ công tất cả các resource-based policies và role trust policies trong một tài khoản là rất tốn thời gian và dễ xảy ra lỗi. Access Analyzer cung cấp một cách tiếp cận chủ động và liên tục để phát hiện các rủi ro này.
*   **Vùng Tin cậy (Zone of Trust):** Khi bạn tạo Analyzer loại `ACCOUNT`, vùng tin cậy là chính tài khoản đó. Bất kỳ quyền truy cập nào được cấp cho principal *bên ngoài* tài khoản này (tài khoản khác, public, etc.) sẽ bị gắn cờ là "External Access". Nếu bạn dùng loại `ORGANIZATION`, vùng tin cậy là tất cả các tài khoản trong Org đó, và chỉ quyền truy cập từ *bên ngoài Org* mới bị gắn cờ.
*   **Tập trung vào "External Access":** Mặc dù có các tính năng khác, giá trị cốt lõi ban đầu của Access Analyzer là phát hiện External Access. Đây là một trong những rủi ro bảo mật phổ biến nhất (ví dụ: S3 bucket vô tình bị public).
*   **Tích hợp với Security Hub:** Các phát hiện của Access Analyzer có thể được gửi đến AWS Security Hub, cung cấp một bảng điều khiển tập trung cho các cảnh báo bảo mật trên toàn bộ môi trường AWS của bạn.
*   **Không thay thế Kiểm tra Thủ công Hoàn toàn:** Access Analyzer là một công cụ mạnh mẽ, nhưng nó tập trung vào việc *ai* có thể truy cập từ bên ngoài. Nó không nhất thiết đánh giá *liệu các quyền được cấp có quá mức cần thiết hay không* (ví dụ: cấp `s3:*` thay vì chỉ `s3:GetObject`). Bạn vẫn cần xem xét các policy theo nguyên tắc đặc quyền tối thiểu.
*   **Archive Rules:** Hữu ích khi bạn có các chia sẻ tài nguyên dự kiến với các tài khoản đối tác hoặc tài khoản khác trong tổ chức (nếu dùng Analyzer loại ACCOUNT). Việc tự động lưu trữ các phát hiện đã biết giúp bạn tập trung vào các phát hiện mới và bất ngờ.
*   **Terraform và Access Analyzer:**
    *   Bạn có thể quản lý vòng đời của Analyzer và Archive Rules bằng Terraform.
    *   Quan trọng hơn, bạn có thể sử dụng Terraform để tạo các tài nguyên (như S3 bucket policy trong ví dụ) và sau đó kiểm tra xem Access Analyzer có phát hiện ra các vấn đề với cấu hình IaC của bạn hay không. Điều này giúp tích hợp phân tích bảo mật vào quy trình IaC.

### Xác minh

1.  **AWS Management Console (Cách dễ nhất để xem Findings):**
    *   Đợi vài phút sau khi `terraform apply` hoàn tất.
    *   Đi đến **IAM** -> **Access Analyzer** (trong menu bên trái). Hoặc sử dụng link trực tiếp từ output `access_analyzer_console_link`.
    *   Đảm bảo bạn đang ở đúng Region (`us-east-1` hoặc region bạn đã chọn).
    *   Trong danh sách **Analyzers**, bạn sẽ thấy `tf-lab9-AccountAnalyzer`.
    *   Xem cột **Active findings**. Sau một thời gian, bạn sẽ thấy ít nhất một phát hiện mới xuất hiện.
    *   Click vào số lượng phát hiện đang hoạt động (Active findings) để xem chi tiết.
    *   Tìm phát hiện liên quan đến S3 bucket `tf-lab9-accessanalyzer-test-...`.
    *   **Kiểm tra chi tiết Finding:**
        *   **Resource:** Tên S3 bucket.
        *   **Resource owner account:** Tài khoản của bạn.
        *   **External principal:** `AWS Account arn:aws:iam::111122223333:root` (hoặc ID bạn đặt).
        *   **Actions:** `s3:GetObject`, `s3:ListBucket`.
        *   **Finding type:** External access.
        *   **Status:** Active.
    *   (Nếu bạn đã bật Archive Rule) Bạn có thể cần xem tab "Archived findings" để tìm phát hiện nếu nó khớp với quy tắc.
2.  **AWS CLI (Kiểm tra sự tồn tại của Analyzer):**
    ```bash
    ANALYZER_NAME=$(terraform output -raw analyzer_name)
    ANALYZER_ARN=$(terraform output -raw analyzer_arn)

    # Liệt kê các analyzer trong region
    aws accessanalyzer list-analyzers --region us-east-1

    # Lấy thông tin analyzer cụ thể
    aws accessanalyzer get-analyzer --analyzer-name $ANALYZER_NAME --region us-east-1

    # Liệt kê các finding (có thể lọc theo analyzer ARN và status)
    aws accessanalyzer list-findings --analyzer-arn $ANALYZER_ARN --region us-east-1 --query "findings[?status=='ACTIVE']"
    # Chờ một lúc để finding xuất hiện ở đây nếu chưa có.
    ```
3.  **(Tùy chọn) Xác minh Archive Rule:**
    *   Nếu bạn đã tạo Archive Rule, hãy kiểm tra nó trong Console (Access Analyzer -> Archive rules) hoặc bằng CLI:
        ```bash
        aws accessanalyzer list-archive-rules --analyzer-name $ANALYZER_NAME --region us-east-1
        ```
    *   Quan sát xem finding liên quan đến tài khoản `111122223333` có bị chuyển sang trạng thái `ARCHIVED` hay không.

### Dọn dẹp

1.  **Chạy lệnh destroy:**
    Trong thư mục `lab9-access-analyzer`, chạy:
    ```bash
    terraform destroy
    ```
    *   **Lưu ý:** Việc xóa Analyzer sẽ xóa chính nó và các Archive Rules liên quan. Nó *không* xóa các tài nguyên (như S3 bucket) mà nó đã phân tích. Terraform sẽ xóa cả S3 bucket và policy trong trường hợp này.
2.  **Xác nhận:** Nhập `yes`.
3.  **Xác minh dọn dẹp (Tùy chọn):** Kiểm tra trong Console xem Analyzer, S3 bucket, và policy đã bị xóa hay chưa. Các phát hiện (findings) liên quan đến các tài nguyên đã xóa sẽ tự động chuyển sang `RESOLVED` sau một thời gian trong Access Analyzer (nếu bạn không xóa analyzer).

---

Lab 9 đã giới thiệu về IAM Access Analyzer và cách sử dụng nó để phát hiện các quyền truy cập từ bên ngoài không mong muốn, một phần quan trọng của việc duy trì bảo mật IAM. Bạn đã học cách tạo Analyzer và thấy cách nó hoạt động với một policy ví dụ. Lab cuối cùng trong chuỗi cơ bản này sẽ tập trung vào việc tăng cường bảo mật đăng nhập bằng cách cấu hình chính sách mật khẩu tài khoản và yêu cầu MFA.
## Lab 10: Account Password Policy & MFA Enforcement

### Mục tiêu

*   Hiểu tầm quan trọng của Chính sách Mật khẩu Tài khoản (Account Password Policy) mạnh.
*   Sử dụng Terraform để định cấu hình Account Password Policy cho tài khoản AWS, bao gồm các yêu cầu về độ phức tạp, độ dài tối thiểu, và hết hạn mật khẩu.
*   Hiểu cách yêu cầu Xác thực Đa Yếu tố (Multi-Factor Authentication - MFA) cho các hành động nhạy cảm hoặc cho tất cả các hành động của một user/role.
*   Viết IAM policy sử dụng Condition key `aws:MultiFactorAuthPresent` để yêu cầu MFA.
*   Áp dụng policy yêu cầu MFA cho một IAM User cụ thể.

### Điều kiện tiên quyết

*   Hoàn thành Lab 1 (IAM Users) và Lab 5 (Conditions).
*   Có quyền quản trị IAM để thay đổi Account Password Policy (`iam:UpdateAccountPasswordPolicy`) và tạo/gắn policy.
*   (Tùy chọn nhưng khuyến nghị cho thử nghiệm MFA) Có một thiết bị MFA ảo (virtual MFA device, ví dụ: Google Authenticator, Authy) hoặc vật lý (hardware MFA device) và một IAM User để bạn có thể kích hoạt MFA cho user đó thông qua Console (việc kích hoạt MFA device nằm ngoài Terraform).

### Khái niệm được đề cập

*   **Account Password Policy:** Một tập hợp các quy tắc áp dụng cho *tất cả* IAM Users trong tài khoản AWS của bạn khi họ đặt hoặc thay đổi mật khẩu đăng nhập Console. Nó giúp thực thi các tiêu chuẩn bảo mật mật khẩu cơ bản. Các cài đặt phổ biến bao gồm:
    *   Độ dài tối thiểu (Minimum password length).
    *   Yêu cầu ký tự đặc biệt (Require symbols).
    *   Yêu cầu số (Require numbers).
    *   Yêu cầu chữ hoa (Require uppercase letters).
    *   Yêu cầu chữ thường (Require lowercase letters).
    *   Cho phép user tự thay đổi mật khẩu (Allow users to change password).
    *   Ngăn chặn sử dụng lại mật khẩu (Password reuse prevention).
    *   Tuổi tối đa của mật khẩu (Password expiration period).
*   **Multi-Factor Authentication (MFA):** Một lớp bảo mật bổ sung quan trọng cho việc đăng nhập. Ngoài mật khẩu (thứ bạn biết), MFA yêu cầu một yếu tố xác thực thứ hai từ một thiết bị vật lý hoặc ảo mà bạn sở hữu (thứ bạn có), ví dụ: mã số dùng một lần (one-time password - OTP) từ ứng dụng authenticator hoặc khóa bảo mật U2F/FIDO. **AWS khuyến nghị mạnh mẽ việc kích hoạt MFA cho root user và tất cả IAM users, đặc biệt là những user có quyền hạn cao.**
*   **MFA Enforcement (Thực thi MFA):** Bạn có thể *yêu cầu* người dùng phải xác thực bằng MFA trước khi họ có thể thực hiện các hành động cụ thể (hoặc tất cả hành động). Điều này thường được thực hiện bằng cách sử dụng Condition key `aws:MultiFactorAuthPresent` trong IAM policy.
*   **`aws:MultiFactorAuthPresent` Condition Key:** Một Global Condition Key trả về giá trị `true` nếu phiên làm việc (session) của principal được thiết lập bằng MFA, và `false` nếu không. Bạn thường sử dụng nó trong `Condition` block của một statement `Allow` hoặc `Deny`.
    *   `"Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}`: Chỉ cho phép/áp dụng statement nếu MFA đã được sử dụng.
    *   `"Condition": {"BoolIfExists": {"aws:MultiFactorAuthPresent": "false"}}`: Áp dụng statement (thường là `Deny`) nếu MFA *không* được sử dụng. `BoolIfExists` được dùng vì key này có thể không tồn tại đối với các yêu cầu không liên quan đến user session (ví dụ: từ dịch vụ AWS).
*   **Terraform Resources:**
    *   `aws_iam_account_password_policy`: Quản lý Account Password Policy.
    *   `aws_iam_policy`: (Như trước) Dùng để tạo policy yêu cầu MFA.
    *   `aws_iam_user_policy_attachment`: (Như trước) Dùng để gắn policy MFA vào user.

### Cấu hình Terraform

Tạo thư mục `lab10-security-policies`.

```terraform
# main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
  # Assume credentials are configured with sufficient IAM permissions
}

# Get caller identity (optional, might be useful in policies)
data "aws_caller_identity" "current" {}

# --- Step 1: Configure Account Password Policy ---
resource "aws_iam_account_password_policy" "strict_password_policy" {
  minimum_password_length        = 14       # Require at least 14 characters
  require_lowercase_characters = true     # Must include lowercase letters
  require_uppercase_characters = true     # Must include uppercase letters
  require_numbers              = true     # Must include numbers
  require_symbols              = true     # Must include symbols like !@#$%^&*()_+-=[]{}|'
  allow_users_to_change_password = true     # Allow users to change their own password via Console
  password_reuse_prevention    = 24       # Remember last 24 passwords to prevent reuse
  max_password_age             = 90       # Require password change every 90 days
  # hard_expiry = false # Default: false. If true, prevents login after expiration instead of requiring change. Usually keep false.

  # Note: Applying this resource modifies the account-wide setting.
  # Ensure you have the necessary permissions (iam:UpdateAccountPasswordPolicy).
}

# --- Step 2: Create a Policy Requiring MFA for Specific Actions ---
# Example: Require MFA to stop or terminate EC2 instances

data "aws_iam_policy_document" "require_mfa_for_ec2_stop_terminate_doc" {
  statement {
    sid    = "AllowAllActionsIfMFA"
    effect = "Allow"
    actions = ["*"] # Allow all actions (will be attached to user with limited base perms)
    resources = ["*"]
    # Condition: Only allow if the session WAS established using MFA
    condition {
      test     = "Bool"
      variable = "aws:MultiFactorAuthPresent"
      values   = ["true"]
    }
  }

  statement {
     sid = "DenyStopTerminateEC2IfNotMFA"
     effect = "Deny" # Explicitly deny sensitive actions if MFA is NOT present
     actions = [
       "ec2:StopInstances",
       "ec2:TerminateInstances"
     ]
     resources = ["*"] # Deny applies broadly
     # Condition: Deny if MFA is NOT present (or key doesn't exist for non-user requests)
     condition {
       test = "BoolIfExists" # Use BoolIfExists for aws:MultiFactorAuthPresent when denying
       variable = "aws:MultiFactorAuthPresent"
       values = ["false"]
     }
   }

   # Optional: Allow specific non-sensitive actions without MFA (e.g., viewing billing, IAM self-service)
   statement {
     sid = "AllowIAMSelfServiceAndViewBillingWithoutMFA"
     effect = "Allow"
     actions = [
       "iam:GetUser",
       "iam:ListAccessKeys",
       "iam:GetAccountPasswordPolicy",
       # Actions related to managing MFA device itself SHOULD be allowed without MFA initially
       "iam:EnableMFADevice",
       "iam:ListMFADevices",
       "iam:ResyncMFADevice",
       "iam:DeactivateMFADevice", # Be cautious with allowing Deactivate without MFA
       "aws-portal:ViewBilling",
       "aws-portal:ViewUsage"
      ]
     resources = ["*"]
     # No MFA condition here - these are allowed regardless
   }
}

resource "aws_iam_policy" "require_mfa_for_sensitive_actions" {
  name        = "tf-lab10-RequireMFAForEC2StopTerminate"
  path        = "/security/"
  description = "Requires MFA for stopping/terminating EC2 instances, denies otherwise."
  policy      = data.aws_iam_policy_document.require_mfa_for_ec2_stop_terminate_doc.json

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 10 MFA Enforcement Policy"
  }
}

# --- Step 3: Create a Test User and Attach the MFA Policy ---
resource "aws_iam_user" "mfa_test_user" {
  name = "tf-lab10-mfa-required-user"
  path = "/users/mfa/"

  # Give the user some basic permissions (e.g., EC2 ReadOnly)
  # The MFA policy will then LAYER the deny/allow based on MFA status.
  # This user CANNOT stop/terminate EC2 without MFA due to the Deny statement.

  tags = {
    ManagedBy = "Terraform"
    Project   = "IAM-Labs"
    Purpose   = "Lab 10 MFA Test User"
  }
}

# Attach the MFA enforcement policy
resource "aws_iam_user_policy_attachment" "attach_mfa_policy" {
  user       = aws_iam_user.mfa_test_user.name
  policy_arn = aws_iam_policy.require_mfa_for_sensitive_actions.arn
}

# Attach basic EC2 read-only policy (example base permissions)
resource "aws_iam_user_policy_attachment" "attach_ec2_read_base" {
   user = aws_iam_user.mfa_test_user.name
   policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess"
}


# --- Outputs ---
output "account_password_policy_applied" {
  value = "Account password policy updated by Terraform. Verify settings in IAM Console."
}

output "mfa_policy_arn" {
  value = aws_iam_policy.require_mfa_for_sensitive_actions.arn
}

output "mfa_policy_json" {
  value = data.aws_iam_policy_document.require_mfa_for_sensitive_actions_doc.json
  sensitive = true
}

output "mfa_test_user_name" {
  value = aws_iam_user.mfa_test_user.name
}

output "mfa_setup_instructions" {
   value = "To test MFA enforcement: 1. Manually create Console password for user '${aws_iam_user.mfa_test_user.name}'. 2. Manually enable a virtual MFA device for this user via AWS Console (IAM -> Users -> ${aws_iam_user.mfa_test_user.name} -> Security credentials -> Assign MFA device). 3. Log in as this user WITHOUT MFA and try to stop/terminate an EC2 instance (should fail). 4. Log out. 5. Log in again WITH MFA and try to stop/terminate (should succeed, assuming base EC2 perms allow it - currently ReadOnly, so test might need adjustment or use Policy Simulator)."
}
```

### Hướng dẫn từng bước

1.  **Tạo thư mục và tệp:**
    ```bash
    mkdir lab10-security-policies
    cd lab10-security-policies
    touch main.tf
    # Copy the Terraform code into main.tf
    ```
2.  **Khởi tạo Terraform:**
    ```bash
    terraform init
    ```
3.  **Xem kế hoạch thực thi:**
    ```bash
    terraform plan
    ```
    Kiểm tra resource `aws_iam_account_password_policy` và các cài đặt của nó. Xem lại `aws_iam_policy` với các statement `Allow` và `Deny` dựa trên `aws:MultiFactorAuthPresent`.
4.  **Áp dụng cấu hình:**
    ```bash
    terraform apply
    ```
    Nhập `yes`. Terraform sẽ cập nhật chính sách mật khẩu tài khoản và tạo user/policy MFA.
5.  **Thiết lập MFA thủ công (Bắt buộc để thử nghiệm):**
    *   Đi đến AWS Console -> IAM -> Users.
    *   Tìm user `tf-lab10-mfa-required-user`.
    *   **Tạo mật khẩu Console:** Tab "Security credentials" -> "Console password" -> Đặt mật khẩu (tuân thủ policy bạn vừa tạo).
    *   **Gán thiết bị MFA:** Vẫn trong "Security credentials" -> "Assigned MFA device" -> Manage -> Chọn "Virtual MFA device". Làm theo hướng dẫn để quét mã QR bằng ứng dụng authenticator (Google Authenticator, Authy, etc.) và nhập hai mã liên tiếp.
6.  **Xác minh (Xem phần dưới).**

### Giải thích & Lý do

*   **Tại sao cần Password Policy mạnh?** Mật khẩu yếu là một trong những điểm tấn công phổ biến nhất. Việc thực thi độ phức tạp, độ dài và xoay vòng mật khẩu giúp giảm đáng kể nguy cơ tài khoản IAM user bị xâm phạm do đoán mật khẩu hoặc tấn công brute-force. `aws_iam_account_password_policy` là cách đơn giản để đặt tiêu chuẩn cơ bản cho toàn bộ tài khoản.
*   **Tại sao MFA là tối quan trọng?** Mật khẩu, ngay cả khi mạnh, vẫn có thể bị lộ (phishing, keylogger, dò mật khẩu từ các vụ rò rỉ khác). MFA cung cấp lớp bảo vệ thứ hai mạnh mẽ, yêu cầu kẻ tấn công phải có cả mật khẩu VÀ quyền truy cập vào thiết bị vật lý/ảo của người dùng. **Nên bật MFA cho tất cả user, đặc biệt là root và admin.**
*   **Cách thức hoạt động của `aws:MultiFactorAuthPresent`:** Khi bạn đăng nhập bằng MFA, AWS STS sẽ nhúng thông tin này vào ngữ cảnh của phiên làm việc tạm thời của bạn. Các IAM policy sau đó có thể kiểm tra sự tồn tại và giá trị (`true`/`false`) của key `aws:MultiFactorAuthPresent` trong ngữ cảnh đó để đưa ra quyết định `Allow` hoặc `Deny`.
*   **Sử dụng `Deny` với `BoolIfExists: {"aws:MultiFactorAuthPresent": "false"}`:** Đây là mẫu **rất hiệu quả và được khuyến nghị** để thực thi MFA cho các hành động nhạy cảm.
    *   `Deny` luôn ghi đè `Allow`.
    *   `BoolIfExists` xử lý các trường hợp key `aws:MultiFactorAuthPresent` không tồn tại (ví dụ: yêu cầu từ dịch vụ AWS thay mặt bạn) mà không vô tình từ chối chúng. Nó chỉ áp dụng `Deny` khi key tồn tại VÀ giá trị là `false` (nghĩa là user đã đăng nhập mà không có MFA).
*   **Cho phép quản lý MFA:** Policy cần phải `Allow` các hành động như `iam:EnableMFADevice`, `iam:ListMFADevices` mà không yêu cầu MFA, nếu không người dùng sẽ không thể tự thiết lập MFA lần đầu. Hãy cẩn thận với `iam:DeactivateMFADevice` - việc cho phép tắt MFA mà không cần MFA có thể tạo ra lỗ hổng.
*   **Terraform và Chính sách tài khoản:** Sử dụng Terraform để quản lý `aws_iam_account_password_policy` đảm bảo rằng cấu hình bảo mật cơ bản này được định nghĩa bằng code, dễ kiểm tra và nhất quán trên các môi trường.

### Xác minh

1.  **Kiểm tra Password Policy:**
    *   **AWS Console:** Đi đến **IAM** -> **Account settings** (trong menu bên trái). Xem phần "Password policy". Xác nhận các cài đặt (độ dài, ký tự yêu cầu, hết hạn, etc.) khớp với những gì bạn đã định cấu hình trong Terraform.
    *   **AWS CLI:**
        ```bash
        aws iam get-account-password-policy
        ```
2.  **Kiểm tra Policy MFA:**
    *   **AWS Console:** Đi đến **IAM** -> **Policies**. Tìm policy `tf-lab10-RequireMFAForEC2StopTerminate`. Xem JSON và các `Condition` block.
    *   Đi đến **IAM** -> **Users** -> `tf-lab10-mfa-required-user`. Kiểm tra tab "Permissions" để thấy policy này và policy `AmazonEC2ReadOnlyAccess` đã được gắn.
    *   **AWS CLI:**
        ```bash
        POLICY_ARN=$(terraform output -raw mfa_policy_arn)
        USER_NAME=$(terraform output -raw mfa_test_user_name)

        aws iam get-policy-version --policy-arn $POLICY_ARN --version-id v1 --query 'PolicyVersion.Document' | jq
        aws iam list-attached-user-policies --user-name $USER_NAME
        ```
3.  **Thử nghiệm Đăng nhập và Hành động (Quan trọng):**
    *   **Lần 1: Đăng nhập KHÔNG MFA:**
        *   Mở một cửa sổ trình duyệt ẩn danh (incognito/private).
        *   Truy cập AWS Management Console.
        *   Đăng nhập bằng tên user `tf-lab10-mfa-required-user` và mật khẩu bạn đã tạo. **KHÔNG** nhập mã MFA khi được hỏi (nếu Console hỏi).
        *   Đi đến dịch vụ **EC2**.
        *   **Thử xem Instances:** Bạn sẽ có thể xem danh sách instance (do policy `AmazonEC2ReadOnlyAccess`).
        *   **Thử Stop/Terminate Instance:** Chọn một instance (nếu có) và thử action "Stop instance" hoặc "Terminate instance". Hành động này **nên thất bại** với lỗi Access Denied, vì bạn chưa đăng nhập bằng MFA và statement `Deny` trong policy MFA đã chặn nó.
        *   **Thử xem Billing:** Đi đến Billing Dashboard (nếu tài khoản của bạn cho phép IAM user truy cập). Bạn **nên** xem được (do statement `AllowIAMSelfServiceAndViewBillingWithoutMFA`).
        *   Đăng xuất.
    *   **Lần 2: Đăng nhập CÓ MFA:**
        *   Đóng cửa sổ ẩn danh và mở lại.
        *   Đăng nhập lại bằng user `tf-lab10-mfa-required-user`. Lần này, **nhập mã MFA** từ ứng dụng authenticator của bạn khi được yêu cầu.
        *   Đi đến dịch vụ **EC2**.
        *   **Thử Stop/Terminate Instance:** Chọn instance và thử lại action "Stop instance" hoặc "Terminate instance". Lần này, hành động **nên thành công** (nếu user có quyền EC2 cơ bản để làm vậy - lưu ý: policy ví dụ chỉ cấp ReadOnly, bạn có thể cần dùng Policy Simulator hoặc điều chỉnh policy cơ bản để thấy hiệu ứng `Allow` hoạt động). Lý do thành công là vì `aws:MultiFactorAuthPresent` giờ là `true`, nên statement `Deny` không áp dụng, và statement `AllowAllActionsIfMFA` có hiệu lực.
        *   Đăng xuất.

### Dọn dẹp

1.  **Vô hiệu hóa/Xóa MFA Device (Tùy chọn):** Trong Console, đi đến user `tf-lab10-mfa-required-user` -> Security credentials -> Quản lý MFA device và xóa nó nếu bạn không cần nữa.
2.  **Chạy lệnh destroy:**
    Trong thư mục `lab10-security-policies`, chạy:
    ```bash
    terraform destroy
    ```
    *   **Lưu ý:** Terraform sẽ cố gắng **khôi phục chính sách mật khẩu về mặc định của AWS** khi xóa resource `aws_iam_account_password_policy`. Nếu bạn muốn giữ lại chính sách tùy chỉnh, bạn cần quản lý nó bên ngoài Terraform hoặc sử dụng `lifecycle { prevent_destroy = true }` (không khuyến khích cho lab).
3.  **Xác nhận:** Nhập `yes`.
4.  **Xác minh dọn dẹp (Tùy chọn):** Kiểm tra Account Settings trong IAM để xem Password Policy đã về mặc định chưa. Kiểm tra Users và Policies để đảm bảo chúng đã bị xóa.

---

Hoàn thành Lab 10! Bạn đã học cách cấu hình các biện pháp kiểm soát bảo mật đăng nhập cơ bản nhưng quan trọng: Account Password Policy và thực thi MFA bằng IAM policy thông qua Terraform. Đây là những bước thiết yếu để bảo vệ tài khoản AWS của bạn.

---

## Lab 11: IAM Identity Center & SCPs Overview (Lý thuyết)

### Mục tiêu

*   Hiểu vai trò và lợi ích của **AWS IAM Identity Center** (trước đây là AWS SSO) như một giải pháp quản lý truy cập tập trung, đặc biệt trong môi trường multi-account.
*   Hiểu khái niệm về **AWS Organizations** và **Service Control Policies (SCPs)** như những cơ chế kiểm soát cấp cao (guardrails).
*   Phân biệt sự khác nhau giữa IAM Policies, Permissions Boundaries, và SCPs.
*   Nhận biết khi nào nên sử dụng IAM Identity Center thay vì quản lý IAM Users truyền thống.
*   Hiểu vị trí của IAM Identity Center và SCPs trong bức tranh tổng thể về bảo mật và quản trị AWS hiện đại (hướng tới 2025).

### Điều kiện tiên quyết

*   Hoàn thành các Lab trước, đặc biệt là Lab 1 (Users), Lab 4 (Roles, Cross-Account), Lab 7 (Federation).
*   Hiểu khái niệm cơ bản về nhiều tài khoản AWS (multi-account environment).

### Khái niệm được đề cập

*   **AWS Organizations:** Dịch vụ cho phép bạn quản lý tập trung nhiều tài khoản AWS. Bạn có thể nhóm các tài khoản vào các **Organizational Units (OUs)**, áp dụng các chính sách quản trị, và đơn giản hóa việc thanh toán.
*   **Service Control Policies (SCPs):** Các chính sách dựa trên JSON, tương tự IAM policies, nhưng được áp dụng ở cấp độ **Organization Root, OU, hoặc Account** trong AWS Organizations. SCPs hoạt động như những **guardrails (lan can bảo vệ)**, định nghĩa **quyền tối đa** cho các principal (bao gồm cả root user) trong các tài khoản bị ảnh hưởng.
    *   **SCPs KHÔNG cấp quyền.** Chúng chỉ lọc hoặc giới hạn các quyền có thể được cấp bởi IAM policies (identity-based hoặc resource-based) hoặc Permissions Boundaries bên trong một tài khoản.
    *   Mặc định, policy SCP `FullAWSAccess` được gắn, cho phép tất cả các hành động. Bạn cần tạo và gắn các SCPs tùy chỉnh để giới hạn quyền.
    *   Quyền hiệu quả cuối cùng của một principal là giao điểm của tất cả các lớp kiểm soát: IAM Policies & Boundaries (bên trong tài khoản) VÀ SCPs (áp dụng từ Organization). Nếu một hành động bị `Deny` bởi SCP, nó sẽ bị từ chối ngay cả khi IAM policy cho phép.
    *   Ví dụ SCP: Ngăn chặn việc tắt CloudTrail, cấm sử dụng các Region không được phê duyệt, chặn các dịch vụ không sử dụng.
*   **AWS IAM Identity Center (successor to AWS SSO):** Dịch vụ quản lý truy cập **tập trung** giúp bạn tạo hoặc kết nối danh tính người dùng và quản lý quyền truy cập của họ vào **nhiều tài khoản AWS và ứng dụng kinh doanh** một cách nhất quán.
    *   **Nguồn danh tính (Identity Source):** Có thể là thư mục tích hợp sẵn của Identity Center, hoặc kết nối với IdP bên ngoài như Azure AD, Okta, Google Workspace (thông qua SAML 2.0).
    *   **Quản lý truy cập Multi-Account:** Cho phép bạn định nghĩa **Permission Sets** (tập hợp các quyền, tương tự như việc tạo role và gắn policy) và gán chúng cho Users hoặc Groups từ nguồn danh tính của bạn, cấp quyền truy cập vào các tài khoản AWS cụ thể trong Organization của bạn.
    *   **Portal đăng nhập tập trung:** Cung cấp một portal duy nhất nơi người dùng có thể đăng nhập bằng thông tin xác thực công ty (nếu dùng IdP ngoài) hoặc thông tin Identity Center, và sau đó truy cập vào các tài khoản AWS và ứng dụng đã được cấp quyền mà không cần đăng nhập lại (SSO). Khi truy cập tài khoản AWS, Identity Center tự động cung cấp **thông tin xác thực tạm thời** dựa trên Permission Set được gán.
    *   **Tại sao là tương lai (2025+)?** IAM Identity Center là giải pháp **được AWS khuyến nghị mạnh mẽ** cho việc quản lý truy cập của *con người* vào tài khoản AWS, đặc biệt trong môi trường multi-account. Nó giải quyết nhiều thách thức của việc quản lý IAM Users và cross-account roles thủ công: quản lý tập trung, SSO, sử dụng temporary credentials, tích hợp IdP dễ dàng. Nó đơn giản hóa đáng kể việc tuân thủ các phương pháp tốt nhất.
*   **Permission Sets:** Khái niệm trong IAM Identity Center. Một Permission Set bao gồm các policy (AWS Managed hoặc Customer Managed) định nghĩa một bộ quyền cụ thể. Khi bạn gán một User/Group vào một Permission Set cho một tài khoản AWS, Identity Center sẽ tự động tạo một IAM Role tương ứng trong tài khoản đích đó với các policy từ Permission Set và Trust Policy cho phép người dùng liên kết từ Identity Center assume role đó.

### So sánh các Cơ chế Kiểm soát

| Cơ chế                  | Phạm vi áp dụng                     | Mục đích chính                                    | Cấp quyền? | Ghi đè Deny? | Quản lý bởi Terraform?                                  |
| :---------------------- | :---------------------------------- | :------------------------------------------------ | :--------- | :----------- | :---------------------------------------------------- |
| **IAM Policy**          | User, Group, Role (Identity-based) | Cấp hoặc từ chối quyền cho principal cụ thể.       | **Có**     | Có           | `aws_iam_policy`, `aws_iam_user_policy`, etc.         |
|                         | Resource (Resource-based)           | Cấp hoặc từ chối quyền truy cập vào resource cụ thể. | **Có**     | Có           | `aws_s3_bucket_policy`, `aws_sqs_queue_policy`, etc. |
| **Permissions Boundary**| User, Role                          | Đặt **quyền tối đa** cho một User/Role.             | **Không**  | Có           | Thuộc tính `permissions_boundary` trên User/Role.      |
| **SCP**                 | Org Root, OU, Account             | Đặt **quyền tối đa** (guardrails) cho toàn bộ OU/Account. | **Không**  | **Có (Quan trọng)** | `aws_organizations_policy`, `aws_organizations_policy_attachment` |
| **IAM Identity Center** | Users/Groups (từ IdP/Identity Center) & Permission Sets | **Quản lý tập trung** việc gán quyền (Permission Sets) vào các tài khoản AWS cho Users/Groups. Sử dụng temporary credentials. | **Có** (thông qua Permission Sets tạo Roles) | Không trực tiếp (dựa vào IAM policy trong Permission Set) | Có (ví dụ: `aws_ssoadmin_permission_set`, `aws_ssoadmin_account_assignment`) - phức tạp hơn IAM cơ bản. |

**Luồng đánh giá quyền (Đơn giản hóa):**

1.  **Deny rõ ràng?** Có Deny trong SCP không? -> **DENIED**
2.  Có Deny trong Resource-based Policy không? -> **DENIED**
3.  Có Deny trong Identity-based Policy không? -> **DENIED**
4.  Có Deny trong Permissions Boundary không? -> **DENIED**
5.  **Allow rõ ràng?** Có Allow trong SCP không (hoặc SCP mặc định `FullAWSAccess`)?
6.  VÀ Có Allow trong Identity-based Policy không?
7.  VÀ (Nếu có Boundary) Có Allow trong Permissions Boundary không?
8.  VÀ (Nếu có Resource Policy) Có Allow trong Resource-based Policy không (hoặc không cần nếu Identity Policy đủ)?
9.  Nếu tất cả các bước Allow ở trên là YES và không có Deny nào -> **ALLOWED**
10. Nếu không có Allow nào khớp -> **DENIED (Mặc định)**

### Khi nào sử dụng IAM Identity Center?

*   Bạn có **nhiều tài khoản AWS** và muốn quản lý truy cập người dùng một cách tập trung.
*   Bạn muốn tích hợp với **IdP doanh nghiệp hiện có** (Azure AD, Okta, etc.) để cung cấp SSO vào AWS.
*   Bạn muốn **loại bỏ việc quản lý IAM Users và access keys/passwords dài hạn** cho người dùng truy cập Console/CLI.
*   Bạn muốn đơn giản hóa việc cấp quyền truy cập tạm thời, tuân thủ đặc quyền tối thiểu cho người dùng trên nhiều tài khoản.

**Khi nào KHÔNG (hoặc ít) sử dụng IAM Identity Center?**

*   **Truy cập Programmatic cho Workloads/Ứng dụng:** Đối với các ứng dụng, scripts, hoặc dịch vụ AWS cần quyền truy cập (ví dụ: EC2 instance, Lambda function), bạn vẫn sử dụng **IAM Roles for Service** (Lab 3) hoặc **IAM Roles for OIDC/SAML Federation** (Lab 7, ví dụ: cho GitHub Actions). Identity Center chủ yếu dành cho *người dùng*.
*   **Quản lý quyền cực kỳ chi tiết trong một tài khoản đơn lẻ (ít người dùng):** Nếu bạn chỉ có một tài khoản và vài người dùng, việc quản lý IAM Users/Roles/Policies truyền thống có thể đủ, mặc dù Identity Center vẫn mang lại lợi ích bảo mật từ temporary credentials.

### Terraform và Quản lý Nâng cao

*   Việc cấu hình AWS Organizations, SCPs, và IAM Identity Center (bao gồm Permission Sets và Account Assignments) **hoàn toàn có thể thực hiện được bằng Terraform**.
*   Tuy nhiên, các resource Terraform liên quan (ví dụ: `aws_organizations_*`, `aws_ssoadmin_*`) thường phức tạp hơn và đòi hỏi hiểu biết sâu về cấu trúc của các dịch vụ này. Chúng thường nằm ngoài phạm vi của một khóa học IAM cơ bản và thuộc về lĩnh vực quản trị AWS nâng cao hoặc quản lý Landing Zone.
*   Trong môi trường thực tế, việc thiết lập ban đầu cho Organizations và Identity Center có thể được thực hiện thủ công qua Console hoặc bằng các công cụ chuyên dụng như AWS Control Tower, sau đó Terraform có thể được sử dụng để quản lý các khía cạnh chi tiết hơn như tạo OU, gắn SCP, định nghĩa Permission Set tùy chỉnh, hoặc tự động hóa việc gán quyền truy cập tài khoản.

### Kết luận về Lab 11

Lab này không có phần thực hành Terraform vì sự phức tạp của việc thiết lập Organizations và Identity Center từ đầu. Mục đích chính là cung cấp cho bạn **bức tranh toàn cảnh** về các cơ chế quản lý truy cập và bảo mật hiện đại, tiên tiến nhất trong AWS:

*   **IAM Identity Center:** Giải pháp ưu tiên cho truy cập của người dùng vào multi-account AWS.
*   **SCPs:** Guardrails cấp cao nhất trong môi trường multi-account, thực thi các giới hạn không thể bị ghi đè bởi quản trị viên tài khoản.

Hiểu được vị trí và mục đích của các công cụ này, cùng với kiến thức về IAM Policies, Roles, và Boundaries bạn đã học trong các lab trước, sẽ giúp bạn thiết kế và triển khai các giải pháp IAM toàn diện, an toàn và có khả năng mở rộng, đáp ứng các tiêu chuẩn bảo mật hàng đầu.

---

## Phần kết luận Chuỗi Lab

Chúc mừng bạn đã hoàn thành chuỗi lab thực hành AWS IAM với Terraform!

Qua các bài lab này, bạn đã đi từ những khái niệm cơ bản nhất đến các kỹ thuật nâng cao, bao gồm:

*   Tạo và quản lý **Users, Groups, Roles, Policies (Managed, Custom, Inline)** bằng Terraform.
*   Triển khai các phương pháp bảo mật tốt nhất như sử dụng **Roles cho EC2 và Federation**, tránh hardcode credentials.
*   Xây dựng các policy linh hoạt và an toàn với **Conditions, Context Keys, và Variables**.
*   Áp dụng các cơ chế kiểm soát nâng cao như **Permissions Boundaries** và hiểu về **ABAC** sử dụng tags.
*   Sử dụng **IAM Access Analyzer** để chủ động phát hiện rủi ro truy cập.
*   Thực thi **Account Password Policy** và yêu cầu **MFA**.
*   Hiểu được bức tranh lớn hơn với **IAM Identity Center** và **SCPs** trong môi trường multi-account.

Bằng cách sử dụng Terraform trong suốt chuỗi lab, bạn đã thực hành việc quản lý cơ sở hạ tầng bảo mật dưới dạng mã (IaC), mang lại lợi ích về tính nhất quán, khả năng lặp lại, kiểm soát phiên bản và tự động hóa.

**Các bước tiếp theo và chủ đề nâng cao:**

*   **Thực hành nhiều hơn:** Thử nghiệm các Condition key khác, xây dựng các kịch bản ABAC phức tạp hơn, tạo các loại Role khác (ví dụ: cho Lambda, ECS).
*   **Terraform Modules for IAM:** Học cách đóng gói các cấu hình IAM phổ biến (ví dụ: tạo role chuẩn cho developer) thành các Terraform module có thể tái sử dụng.
*   **Quản lý State Terraform:** Tìm hiểu các phương pháp hay nhất để quản lý tệp trạng thái Terraform một cách an toàn và cộng tác (ví dụ: sử dụng S3 backend với DynamoDB locking).
*   **AWS Organizations & SCPs:** Nếu bạn làm việc trong môi trường multi-account, hãy tìm hiểu sâu hơn về cách cấu hình Organizations và viết SCPs hiệu quả bằng Terraform (`aws_organizations_*` resources).
*   **IAM Identity Center với Terraform:** Khám phá các resource `aws_ssoadmin_*` để tự động hóa việc tạo Permission Sets và Account Assignments.
*   **Tích hợp IAM với các dịch vụ khác:** Tìm hiểu cách IAM tương tác với các dịch vụ như AWS Key Management Service (KMS) cho chính sách khóa, AWS Secrets Manager cho resource policies, hoặc AWS Lake Formation cho kiểm soát truy cập dữ liệu.
*   **Công cụ Kiểm tra Bảo mật:** Sử dụng các công cụ như Prowler, ScoutSuite, hoặc các tính năng của AWS Security Hub để liên tục quét và đánh giá cấu hình IAM của bạn.

Kiến thức và kỹ năng bạn đã xây dựng trong chuỗi lab này là nền tảng vững chắc để bạn tự tin đối mặt với các thách thức về quản lý danh tính và truy cập trong AWS. Hãy tiếp tục học hỏi, thực hành và luôn đặt bảo mật lên hàng đầu!