Đã nhận và phân tích văn bản gốc, bao gồm kế hoạch chi tiết và nội dung đã biên soạn cho Lab 0, Lab 1 và Lab 2. Tôi đang hoạt động với tư cách là Bậc thầy Biên soạn Nội dung Khoa học & Thuyết phục.

Mục tiêu chính của chúng ta là tạo ra một chuỗi bài lab thực hành chất lượng cao, chi tiết, và có tính thuyết phục về AWS Billing & Cost Management sử dụng Terraform. Các bài lab này phải đảm bảo tính chính xác kỹ thuật, rõ ràng, dễ hiểu, và mang lại giá trị thực tiễn cao cho người học, đồng thời tuân thủ các phương pháp tốt nhất hướng tới năm 2025.

Nội dung bạn đã cung cấp cho **Giới thiệu tổng quan**, **Lab 0: Thiết lập Môi trường & Giới thiệu về Quản lý Chi phí AWS**, **Lab 1: Tạo AWS Budgets Cơ bản với Terraform**, và **Lab 2: Kích hoạt và Sử dụng Cost Allocation Tags với Terraform** đã rất xuất sắc, chi tiết, và tuân thủ chặt chẽ các yêu cầu đã đề ra. Chúng thể hiện sự hiểu biết sâu sắc về cả AWS và Terraform, cũng như cách trình bày nội dung kỹ thuật một cách hiệu quả.

*   **Tính khoa học và độ chính xác:** Các khái niệm, tài nguyên Terraform, và quy trình AWS được mô tả chính xác.
*   **Độ chi tiết và rõ ràng:** Hướng dẫn từng bước rất cụ thể, giải thích rõ ràng "tại sao" và "như thế nào".
*   **Tính hấp dẫn và lôi cuốn:** Việc lồng ghép các "Giải thích & Lý do", "Phương pháp Tốt nhất", và liên hệ với FinOps làm tăng giá trị và sự hấp dẫn của nội dung.
*   **Cấu trúc và định dạng:** Việc sử dụng Markdown, phân chia thành các mục rõ ràng, và sự nhất quán trong trình bày là rất tốt.

Tôi hoàn toàn đồng ý với chất lượng và cấu trúc của các phần bạn đã soạn thảo. Chúng là một nền tảng tuyệt vời.

Dưới đây là phiên bản được biên soạn lại và tiếp nối cho **Lab 3**, tập trung vào việc tối ưu hóa sự rõ ràng, chi tiết, tính khoa học, độ chính xác và sức hấp dẫn, dựa trên phác thảo của bạn:

---

## Lab 3: Tạo AWS Cost Categories với Terraform

### Mục tiêu
*   Hiểu rõ khái niệm và lợi ích của **AWS Cost Categories**.
*   Học cách định nghĩa và triển khai Cost Categories bằng Terraform để nhóm chi phí theo các quy tắc logic kinh doanh tùy chỉnh.
*   Phân biệt Cost Categories với Cost Allocation Tags và hiểu cách chúng bổ trợ lẫn nhau.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0, Lab 1 và Lab 2.
*   Quyền IAM để tạo và quản lý Cost Categories (`ce:CreateCostCategoryDefinition`, `ce:UpdateCostCategoryDefinition`, `ce:DeleteCostCategoryDefinition`, `ce:DescribeCostCategoryDefinition`, `ce:ListCostCategoryDefinitions`).
*   Ít nhất một số Cost Allocation Tags đã được kích hoạt (ví dụ: `Project`, `Environment` từ Lab 2) để sử dụng trong các quy tắc của Cost Category.

### Khái niệm được đề cập
*   **AWS Cost Categories:** Một tính năng của AWS Cost Management cho phép bạn nhóm chi phí và mức sử dụng thành các danh mục có ý nghĩa dựa trên các quy tắc bạn xác định. Các quy tắc này có thể dựa trên tài khoản, tag, dịch vụ, thậm chí là các Cost Categories khác.
*   **Cost Category Rules:** Các biểu thức logic xác định cách chi phí được ánh xạ vào một giá trị danh mục cụ thể. Mỗi quy tắc bao gồm:
    *   **Value:** Tên của danh mục chi phí mà quy tắc này sẽ gán cho các chi phí phù hợp (ví dụ: "Project Alpha Compute", "Shared Infrastructure").
    *   **Rule (Expression):** Điều kiện để chi phí được gán vào `Value` đó. Có thể là:
        *   `Dimensions`: Dựa trên các chiều như `LINKED_ACCOUNT`, `SERVICE`, `REGION`.
        *   `Tags`: Dựa trên `key` và `value` của Cost Allocation Tags.
        *   `CostCategories`: Dựa trên các Cost Categories khác (cho phép tạo cấu trúc phân cấp).
*   **Rule Version:** Hiện tại, chỉ có `CostCategoryExpression.v1` được hỗ trợ.
*   **Default Value:** Giá trị được gán cho các chi phí không khớp với bất kỳ quy tắc nào được định nghĩa.
*   **Split Charge Rules (Nâng cao):** Cho phép bạn phân chia chi phí của một nguồn cụ thể cho nhiều giá trị Cost Category dựa trên tỷ lệ phần trăm. (Chúng ta sẽ không đi sâu vào phần này trong lab cơ bản, nhưng cần biết về sự tồn tại của nó).
*   **Inherited Value & Effective On:** Cách các giá trị được kế thừa và thời điểm Cost Category có hiệu lực.

### Cấu hình Terraform

Tạo một thư mục mới cho lab này, ví dụ `lab3-cost-categories`. Bên trong thư mục đó, tạo các tệp sau:

1.  **`main.tf`**:
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
      region = var.aws_region # Cost Categories are global but often managed from a primary region
    }

    # Ensure the Cost Allocation Tags from Lab 2 are active if you plan to use them.
    # For this lab, we assume 'Project' and 'Environment' tags are active.
    # You would typically manage tag activation in a separate, foundational Terraform configuration.

    resource "aws_cost_category_definition" "project_based_grouping" {
      name          = "ProjectGrouping"
      rule_version  = "CostCategoryExpression.v1" # Currently the only supported version
      effective_start = strftime("%Y-%m-01T00:00:00Z", timestamp()) # Effective from the beginning of the current month

      rules {
        # Rule 1: Group costs for Project Phoenix
        value = "Project Phoenix Costs"
        rule {
          tags {
            key    = var.project_tag_key # Assumes "Project" tag from Lab 2
            values = ["Phoenix"]
            match_options = ["EQUALS"]
          }
        }
      }

      rules {
        # Rule 2: Group costs for Project Titan (Example of another project)
        value = "Project Titan Costs"
        rule {
          tags {
            key    = var.project_tag_key
            values = ["Titan"] # Assuming you might have resources tagged with Project:Titan
            match_options = ["EQUALS"]
          }
        }
      }

      rules {
        # Rule 3: Group Development environment costs across any project
        # This rule demonstrates using a different tag and will apply if not caught by Project-specific rules above
        # (Order matters if rules could overlap; AWS processes rules in order)
        value = "All Development Environment Costs"
        rule {
          tags {
            key    = var.environment_tag_key # Assumes "Environment" tag from Lab 2
            values = ["Development"]
            match_options = ["EQUALS"]
          }
        }
      }

      rules {
        # Rule 4: Group all S3 costs not otherwise categorized by project/environment rules above
        value = "Shared S3 Storage Costs"
        rule {
          dimension {
            key = "SERVICE"
            values = ["Amazon Simple Storage Service"]
            match_options = ["EQUALS"]
          }
        }
        # type = "REGULAR" # or "INHERITED_VALUE"
      }
      
      default_value = "Uncategorized Costs" # Costs not matching any rule

      tags = {
        ManagedBy = "Terraform"
        Purpose   = "Lab3 Cost Categories Example"
        Owner     = var.owner_tag
      }
    }
    ```

2.  **`variables.tf`**:
    ```terraform
    # variables.tf

    variable "aws_region" {
      description = "The AWS region (Cost Categories are global but provider needs a region)."
      type        = string
      default     = "us-east-1"
    }

    variable "project_tag_key" {
      description = "The key for the Project cost allocation tag (e.g., 'Project')."
      type        = string
      default     = "Project" # Should match the tag activated in Lab 2
    }

    variable "environment_tag_key" {
      description = "The key for the Environment cost allocation tag (e.g., 'Environment')."
      type        = string
      default     = "Environment" # Should match the tag activated in Lab 2
    }

    variable "owner_tag" {
      description = "Value for the Owner tag on the Cost Category definition itself."
      type        = string
      default     = "student@example.com"
    }
    ```

3.  **`outputs.tf`**:
    ```terraform
    # outputs.tf

    output "cost_category_arn" {
      description = "The ARN of the created AWS Cost Category definition."
      value       = aws_cost_category_definition.project_based_grouping.arn
    }

    output "cost_category_name" {
      description = "The name of the created AWS Cost Category definition."
      value       = aws_cost_category_definition.project_based_grouping.name
    }

    output "cost_category_effective_start" {
      description = "The effective start date of the Cost Category."
      value       = aws_cost_category_definition.project_based_grouping.effective_start
    }
    ```

### Hướng dẫn từng bước

1.  **Tạo thư mục dự án và các tệp `.tf`**:
    *   Tạo một thư mục có tên `lab3-cost-categories`.
    *   Bên trong thư mục này, tạo các tệp `main.tf`, `variables.tf`, và `outputs.tf` với nội dung như trên.
    *   **Quan trọng:** Đảm bảo rằng các giá trị `var.project_tag_key` và `var.environment_tag_key` khớp với các key tag bạn đã kích hoạt trong Lab 2 (hoặc các tag đang hoạt động khác mà bạn muốn sử dụng).

2.  **Chạy `terraform init`**:
    *   Mở terminal, điều hướng đến thư mục `lab3-cost-categories`.
    *   Chạy:
        ```bash
        terraform init
        ```

3.  **Chạy `terraform plan`**:
    *   Xem kế hoạch thực thi:
        ```bash
        terraform plan
        ```
    *   Terraform sẽ hiển thị rằng nó sẽ tạo một resource `aws_cost_category_definition` với các quy tắc bạn đã định nghĩa. Xem lại kỹ các quy tắc và giá trị mặc định.

4.  **Chạy `terraform apply`**:
    *   Áp dụng các thay đổi:
        ```bash
        terraform apply -auto-approve
        ```
    *   Terraform sẽ tạo định nghĩa Cost Category.

5.  **Xác minh trong AWS Management Console**:
    *   Đăng nhập vào AWS Management Console.
    *   Điều hướng đến dịch vụ **AWS Cost Management**.
    *   Nhấp vào **Cost Categories** trong menu điều hướng bên trái.
    *   Bạn sẽ thấy Cost Category `ProjectGrouping` (hoặc tên bạn đã đặt) được liệt kê.
    *   Nhấp vào tên Cost Category để xem chi tiết. Kiểm tra các quy tắc (`Rules`), giá trị mặc định (`Default value`), và ngày có hiệu lực (`Effective start date`).
    *   **Lưu ý:** Giống như Cost Allocation Tags, có thể mất đến 24 giờ để Cost Categories xử lý và ánh xạ chi phí của bạn, và sau đó hiển thị trong Cost Explorer.

6.  **(Sau 24 giờ) Kiểm tra trong Cost Explorer (Tùy chọn, do độ trễ)**:
    *   Sau khi Cost Category đã xử lý dữ liệu, bạn có thể sử dụng nó trong Cost Explorer.
    *   Điều hướng đến **AWS Cost Management > Cost Explorer**.
    *   Mở Cost Explorer và tạo một báo cáo mới hoặc chỉnh sửa báo cáo hiện có.
    *   Trong phần "Group by" ở phía bên phải, chọn "Cost Category" và sau đó chọn `ProjectGrouping` (hoặc tên Cost Category của bạn).
    *   Biểu đồ và bảng chi phí sẽ được nhóm theo các giá trị bạn đã định nghĩa trong Cost Category (ví dụ: "Project Phoenix Costs", "Shared S3 Storage Costs", "Uncategorized Costs").
    *   Bạn cũng có thể sử dụng Cost Category làm bộ lọc.

### Giải thích & Lý do

*   **Tại sao sử dụng Cost Categories?**
    *   **Logic kinh doanh phức tạp:** Khi việc gắn thẻ đơn thuần không đủ để phản ánh cách bạn muốn xem chi phí (ví dụ: nhóm nhiều tag lại với nhau, nhóm theo tài khoản cụ thể kết hợp với dịch vụ).
    *   **Đơn giản hóa báo cáo:** Tạo ra các "view" chi phí cấp cao, dễ hiểu cho các bên liên quan không rành về kỹ thuật.
    *   **Khắc phục thiếu sót trong gắn thẻ:** Nếu một số tài nguyên không được gắn thẻ đúng cách, Cost Categories có thể giúp "bắt" các chi phí đó dựa trên các tiêu chí khác (ví dụ: tài khoản, dịch vụ) và gán chúng vào một danh mục phù hợp.
    *   **Phân bổ chi phí dùng chung:** Dễ dàng hơn trong việc xác định và nhóm các chi phí dùng chung (ví dụ: chi phí mạng, chi phí công cụ bảo mật).

*   **Cost Categories vs. Cost Allocation Tags:**
    *   **Tags:** Là metadata được gắn trực tiếp vào tài nguyên AWS. Chúng là nền tảng.
    *   **Cost Categories:** Là các quy tắc logic được áp dụng *sau khi* chi phí được ghi nhận. Chúng thường sử dụng tags làm một trong các đầu vào để đưa ra quyết định phân loại.
    *   Chúng bổ trợ cho nhau: Một chiến lược gắn thẻ tốt làm cho Cost Categories trở nên mạnh mẽ và dễ định nghĩa hơn.

*   **`rule_version = "CostCategoryExpression.v1"`**:
    *   Đây là phiên bản cú pháp quy tắc hiện được AWS hỗ trợ. Terraform chỉ định rõ ràng điều này.

*   **`effective_start`**:
    *   Cost Categories không áp dụng cho chi phí lịch sử trước ngày này. Đặt nó thành đầu tháng hiện tại (`strftime("%Y-%m-01T00:00:00Z", timestamp())`) đảm bảo nó áp dụng cho chi phí của tháng hiện tại trở đi.

*   **Thứ tự của Rules:**
    *   AWS xử lý các quy tắc theo thứ tự bạn định nghĩa chúng. Quy tắc đầu tiên khớp với một mục chi phí sẽ được áp dụng. Điều này quan trọng nếu các quy tắc của bạn có thể chồng chéo. Trong ví dụ của chúng ta:
        1.  Chi phí có tag `Project: Phoenix` sẽ vào "Project Phoenix Costs".
        2.  Chi phí có tag `Project: Titan` (nếu có và không phải Phoenix) sẽ vào "Project Titan Costs".
        3.  Chi phí có tag `Environment: Development` (nếu không bị bắt bởi quy tắc Project ở trên) sẽ vào "All Development Environment Costs".
        4.  Chi phí dịch vụ S3 (nếu không bị bắt bởi các quy tắc tag ở trên) sẽ vào "Shared S3 Storage Costs".
        5.  Tất cả các chi phí khác sẽ vào "Uncategorized Costs".

*   **`default_value`**:
    *   Rất quan trọng để có một giá trị mặc định. Nó giúp bạn xác định các chi phí chưa được phân loại bởi bất kỳ quy tắc nào, cho thấy các lỗ hổng tiềm ẩn trong logic phân loại hoặc chiến lược gắn thẻ của bạn.

*   **Phương pháp Tốt nhất cho năm 2025:**
    *   **Bắt đầu đơn giản:** Đừng cố gắng tạo ra một cấu trúc Cost Category quá phức tạp ngay từ đầu. Bắt đầu với các nhóm chính và tinh chỉnh dần.
    *   **Sử dụng Tags làm nền tảng:** Đảm bảo chiến lược Cost Allocation Tags của bạn mạnh mẽ. Cost Categories hoạt động tốt nhất khi dựa trên dữ liệu tag nhất quán.
    *   **Thường xuyên xem xét và cập nhật:** Khi doanh nghiệp của bạn phát triển hoặc cấu trúc đám mây thay đổi, hãy xem xét lại và cập nhật các định nghĩa Cost Category của bạn.
    *   **Tài liệu hóa logic:** Ghi lại lý do đằng sau mỗi quy tắc Cost Category để người khác (và chính bạn trong tương lai) có thể hiểu được. Mã Terraform với các comment rõ ràng là một phần của tài liệu này.

### Xác minh
1.  Đăng nhập vào **AWS Management Console**.
2.  Điều hướng đến **AWS Cost Management > Cost Categories**.
3.  Xác nhận rằng Cost Category `ProjectGrouping` (hoặc tên bạn đã đặt) tồn tại.
4.  Nhấp vào tên Cost Category và kiểm tra các chi tiết:
    *   **Rules:** Đảm bảo các quy tắc khớp với những gì bạn đã định nghĩa trong Terraform.
    *   **Default value:** Xác minh giá trị mặc định.
    *   **Effective start date:** Kiểm tra ngày có hiệu lực.
5.  **(Sau 24-48 giờ)** Điều hướng đến **AWS Cost Management > Cost Explorer**.
    *   Thử nhóm ("Group by") theo Cost Category `ProjectGrouping`.
    *   Quan sát xem chi phí có được phân bổ vào các giá trị danh mục như "Project Phoenix Costs", "Shared S3 Storage Costs", "Uncategorized Costs" không. (Điều này phụ thuộc vào chi phí thực tế phát sinh trong tài khoản của bạn và các tag được áp dụng cho tài nguyên).

### Dọn dẹp
1.  **Chạy `terraform destroy`**:
    *   Trong terminal, tại thư mục `lab3-cost-categories`, chạy lệnh sau:
        ```bash
        terraform destroy -auto-approve
        ```
    *   Terraform sẽ xóa định nghĩa Cost Category.

2.  **Xác minh việc xóa trong AWS Management Console**:
    *   Quay lại trang **AWS Cost Management > Cost Categories**.
    *   Xác nhận rằng Cost Category bạn đã tạo không còn được liệt kê.

---

## Lab 3.2: Tạo AWS Cost Categories với Terraform

### Mục tiêu
*   Hiểu cách **AWS Cost Categories** có thể giúp bạn nhóm chi phí theo các logic nghiệp vụ tùy chỉnh, vượt ra ngoài khả năng của chỉ riêng tags hoặc tài khoản.
*   Học cách định nghĩa và triển khai một **AWS Cost Category** bằng Terraform, sử dụng các quy tắc dựa trên tài khoản, tag, và dịch vụ.
*   Khám phá cách sử dụng **Split Charge Rules** để phân bổ chi phí dùng chung.
*   Xem cách Cost Categories xuất hiện và có thể được sử dụng trong AWS Cost Explorer.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường.
*   Hoàn thành Lab 2: Kích hoạt và Sử dụng Cost Allocation Tags với Terraform (để có sẵn tag `Project` đã được kích hoạt, hoặc bạn có thể điều chỉnh ví dụ để sử dụng tag khác đã kích hoạt và cập nhật biến `project_tag_key`).
*   Quyền IAM để tạo và quản lý Cost Categories (`ce:CreateCostCategoryDefinition`, `ce:UpdateCostCategoryDefinition`, `ce:DeleteCostCategoryDefinition`, `ce:DescribeCostCategoryDefinition`, `ce:ListCostCategoryDefinitions`) và kích hoạt Cost Allocation Tags (`ce:UpdateCostAllocationTagsStatus`).
*   (Tùy chọn, để kiểm chứng đầy đủ quy tắc `LINKED_ACCOUNT`) Có ít nhất hai tài khoản AWS trong một AWS Organization. Nếu không, bạn có thể bỏ qua hoặc điều chỉnh phần quy tắc đó.

### Khái niệm được đề cập
*   **AWS Cost Categories:** Một tính năng của AWS Cost Management cho phép bạn tạo các nhóm logic (categories) và các giá trị (values) cho các nhóm đó. Sau đó, bạn ánh xạ chi phí của mình vào các giá trị này dựa trên các quy tắc bạn xác định. Các quy tắc này có thể dựa trên nhiều chiều khác nhau như tài khoản (`LINKED_ACCOUNT`), tag (`TAG`), dịch vụ (`SERVICE`), loại phí, hoặc thậm chí các Cost Categories khác (cho phép tạo cấu trúc phân cấp).
*   **Rule Version:** Phiên bản của schema quy tắc (hiện tại là `CostCategoryExpression.v1`).
*   **Rules:** Các điều kiện logic xác định cách chi phí được gán vào một Cost Category Value. Mỗi quy tắc bao gồm một `value` (tên của danh mục) và một `rule` (biểu thức điều kiện).
    *   **Dimension:** Thuộc tính được sử dụng trong quy tắc (ví dụ: `LINKED_ACCOUNT`, `TAG`, `SERVICE`).
    *   **Match Options:** Cách so sánh giá trị (ví dụ: `EQUALS`, `CONTAINS`, `STARTS_WITH`, `ENDS_WITH`, `ABSENT`). Mặc định là `EQUALS`.
*   **Default Value:** Giá trị được gán cho chi phí không khớp với bất kỳ quy tắc nào.
*   **Effective Start Date:** Ngày mà định nghĩa Cost Category bắt đầu có hiệu lực (thường là đầu tháng hiện tại nếu không chỉ định).
*   **Split Charge Rules:** Cho phép bạn phân chia chi phí của một `source` Cost Category Value cụ thể cho nhiều `target` Cost Category Values dựa trên một `method` (ví dụ: `PROPORTIONAL`, `FIXED`, `EVEN`).
*   **Rule Precedence:** Các quy tắc được đánh giá theo thứ tự chúng được liệt kê trong cấu hình. Quy tắc đầu tiên khớp sẽ được áp dụng.

### Cấu hình Terraform

Tạo một thư mục mới cho lab này, ví dụ `lab3-cost-categories`. Bên trong thư mục đó, tạo các tệp sau:

1.  **`main.tf`**:
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
      region = var.aws_region
      # Cost Categories are global, but the provider requires a region.
      # API calls for Cost Categories are typically made to us-east-1.
    }

    # Ensure the Project tag is active for cost allocation (referenced from Lab 2 logic)
    # This resource will only be created if var.activate_project_tag is true.
    resource "aws_cost_allocation_tag" "project_tag_activation" {
      count = var.activate_project_tag ? 1 : 0

      tag_key = var.project_tag_key
      status  = "Active"
    }

    # Define an AWS Cost Category
    resource "aws_cost_category_definition" "business_units_cc" {
      name          = var.cost_category_name
      rule_version  = "CostCategoryExpression.v1"
      # effective_start defaults to the first day of the current month if not specified.
      # For explicit control, you can set it:
      # effective_start = "2024-01-01T00:00:00Z" # Example: specific start date
      # Using strftime for dynamic start of current month:
      effective_start = strftime("%Y-%m-01T00:00:00Z", timestamp())


      # Rule 1: Categorize costs from specific AWS accounts as "SharedInfrastructure"
      # This rule is most effective in an AWS Organization with multiple accounts.
      # If using a single account, this rule might not categorize much, or you can adapt/remove it.
      rule {
        value = "SharedInfrastructure"
        rule {
          dimension {
            key    = "LINKED_ACCOUNT"
            values = var.shared_infrastructure_account_ids # List of AWS Account IDs
            # match_options = ["EQUALS"] # Default is EQUALS
          }
        }
        # type  = "REGULAR" # Default type is REGULAR
      }

      # Rule 2: Categorize costs tagged with Project=Phoenix or Project=BlueHarvest as "StrategicInitiatives"
      # This rule depends on the 'Project' tag being an active cost allocation tag.
      # Rules are evaluated in the order they are listed.
      rule {
        value = "StrategicInitiatives"
        rule {
          dimension {
            key     = "TAG"
            tag_key = var.project_tag_key # e.g., "Project"
            values  = ["Phoenix", "BlueHarvest"]
            # match_options = ["EQUALS"]
          }
        }
      }

      # Rule 3: Categorize costs from Amazon S3 and Amazon EC2 services as "CoreServices"
      # if not already caught by the above rules.
      rule {
        value = "CoreServices"
        rule {
          dimension {
            key    = "SERVICE"
            values = ["Amazon Simple Storage Service", "Amazon Elastic Compute Cloud - Compute"] # Use exact service names from Cost Explorer
          }
        }
      }

      default_value = "OtherBusinessOperations" # Costs not matching any rule go here

      # Split Charge Rule: Distribute costs from "SharedInfrastructure" proportionally
      # to "StrategicInitiatives" and "CoreServices".
      split_charge_rule {
        source  = "SharedInfrastructure" # The Cost Category Value whose costs will be split
        method  = "PROPORTIONAL"         # EVEN, FIXED, PROPORTIONAL
        targets = [                      # Cost Category Values to split costs among
          "StrategicInitiatives",
          "CoreServices",
          # "OtherBusinessOperations" # Cannot be the same as source, and default_value is also a valid target
        ]
        # For FIXED or PROPORTIONAL_PERCENTAGE (if supported by API version for method):
        # parameter {
        #   type  = "ALLOCATION_PERCENTAGES" # Check AWS docs for exact parameter type for PROPORTIONAL
        #   values = ["60", "40"] # Percentages for each target if using FIXED
        # }
      }

      tags = {
        ManagedBy = "Terraform"
        Purpose   = "Lab3 Business Unit Cost Categorization"
        Owner     = var.owner_tag
        Environment = "Global"
      }

      # Explicit dependency to ensure tag activation happens first if managed here.
      depends_on = [
        aws_cost_allocation_tag.project_tag_activation,
      ]
    }
    ```

2.  **`variables.tf`**:
    ```terraform
    # variables.tf

    variable "aws_region" {
      description = "The AWS region for provider configuration (Cost Categories are global)."
      type        = string
      default     = "us-east-1"
    }

    variable "cost_category_name" {
      description = "The name for the Cost Category definition."
      type        = string
      default     = "OrganizationalUnits"
    }

    variable "shared_infrastructure_account_ids" {
      description = "A list of AWS Account IDs to be categorized as SharedInfrastructure. Relevant for AWS Organizations."
      type        = list(string)
      default     = ["123456789012", "098765432109"] # Replace with actual account IDs or use an empty list [] if not applicable
      # Example: default = []
    }

    variable "project_tag_key" {
      description = "The tag key used for project-based cost categorization (must be an active cost allocation tag)."
      type        = string
      default     = "Project" # Ensure this matches the tag key from Lab 2 or your active tags
    }

    variable "activate_project_tag" {
      description = "Set to true to attempt to activate the project_tag_key via Terraform. Set to false if already active or managed elsewhere."
      type        = bool
      default     = true # Set to false if 'Project' tag is definitively active and managed outside this lab's scope.
    }

    variable "owner_tag" {
      description = "Value for the Owner tag applied to the Cost Category definition."
      type        = string
      default     = "FinOpsTeam@example.com"
    }
    ```

3.  **`outputs.tf`**:
    ```terraform
    # outputs.tf

    output "cost_category_arn" {
      description = "The ARN of the created AWS Cost Category definition."
      value       = aws_cost_category_definition.business_units_cc.arn
    }

    output "cost_category_name_output" {
      description = "The name of the created AWS Cost Category definition."
      value       = aws_cost_category_definition.business_units_cc.name
    }

    output "cost_category_effective_start" {
      description = "The effective start date of the Cost Category definition."
      value       = aws_cost_category_definition.business_units_cc.effective_start
    }

    output "cost_category_rules_summary" {
      description = "A summary of the rule values and default value for the Cost Category."
      value = {
        rules = [for r in aws_cost_category_definition.business_units_cc.rule : r.value]
        default_value = aws_cost_category_definition.business_units_cc.default_value
        split_charge_source = aws_cost_category_definition.business_units_cc.split_charge_rule[0].source # Accessing the first (and only in this case) split charge rule
        split_charge_targets = aws_cost_category_definition.business_units_cc.split_charge_rule[0].targets
      }
      # Note: Full rule block can be sensitive if it contains account IDs.
      # For full details, refer to the state file or use 'terraform show'.
    }
    ```

### Hướng dẫn từng bước

1.  **Tạo thư mục dự án và các tệp `.tf`**:
    *   Tạo một thư mục có tên `lab3-cost-categories`.
    *   Bên trong thư mục này, tạo các tệp `main.tf`, `variables.tf`, và `outputs.tf` với nội dung như trên.
    *   **Quan trọng:**
        *   Trong `variables.tf`, cập nhật `shared_infrastructure_account_ids` với các ID tài khoản AWS thực tế nếu bạn muốn kiểm thử quy tắc dựa trên tài khoản. Nếu không, hãy để là danh sách rỗng (`[]`).
        *   Đảm bảo `project_tag_key` (mặc định là "Project") khớp với tag key bạn đã kích hoạt trong Lab 2 hoặc một tag key đang hoạt động khác. Biến `activate_project_tag` (mặc định là `true`) sẽ cố gắng kích hoạt tag này; đặt thành `false` nếu bạn chắc chắn nó đã được kích hoạt và quản lý ở nơi khác.

2.  **Chạy `terraform init`**:
    *   Mở terminal, điều hướng đến thư mục `lab3-cost-categories`.
    *   Chạy:
        ```bash
        terraform init
        ```

3.  **Chạy `terraform plan`**:
    *   Xem kế hoạch thực thi:
        ```bash
        terraform plan
        ```
    *   Terraform sẽ hiển thị rằng nó sẽ tạo một resource `aws_cost_category_definition` (và có thể là một `aws_cost_allocation_tag` nếu `activate_project_tag` là `true` và tag chưa được kích hoạt). Xem lại các quy tắc, giá trị mặc định, và split charge rule.

4.  **Chạy `terraform apply`**:
    *   Áp dụng các thay đổi:
        ```bash
        terraform apply -auto-approve
        ```
    *   Terraform sẽ tạo định nghĩa Cost Category.

5.  **Xác minh trong AWS Management Console**:
    *   Đăng nhập vào AWS Management Console.
    *   Điều hướng đến dịch vụ **AWS Cost Management**.
    *   Nhấp vào **Cost Categories** trong menu điều hướng bên trái.
    *   Bạn sẽ thấy Cost Category mới của mình (ví dụ: `OrganizationalUnits`) được liệt kê.
    *   Nhấp vào tên Cost Category để xem chi tiết. Kiểm tra các quy tắc (ví dụ: cho `SharedInfrastructure`, `StrategicInitiatives`, `CoreServices`), giá trị mặc định (`OtherBusinessOperations`), và quy tắc chia sẻ chi phí (`split_charge_rule`).
    *   **Lưu ý:** Cost Categories có hiệu lực từ `effective_start` (đầu tháng hiện tại theo cấu hình). Có thể mất một khoảng thời gian (vài giờ đến 24 giờ) để chi phí được xử lý và phân loại theo định nghĩa Cost Category mới và xuất hiện trong Cost Explorer.

6.  **(Sau khi xử lý) Kiểm tra trong Cost Explorer**:
    *   Sau khi AWS đã xử lý dữ liệu chi phí với Cost Category mới (thường trong vòng 24 giờ), hãy điều hướng đến **AWS Cost Management > Cost Explorer**.
    *   Ở phía bên phải, trong phần "Filters", bạn sẽ thấy một bộ lọc mới có tên là Cost Category của bạn (ví dụ: `OrganizationalUnits`).
    *   Nhấp vào tên Cost Category này. Bạn sẽ có thể chọn các giá trị đã định nghĩa (ví dụ: `SharedInfrastructure`, `StrategicInitiatives`, `CoreServices`, `OtherBusinessOperations`) để lọc chi phí.
    *   Bạn cũng có thể sử dụng Cost Category này trong mục "Group by" để xem chi phí được phân bổ cho từng giá trị.
    *   Nếu bạn có chi phí trong các tài khoản được chỉ định cho `SharedInfrastructure`, hãy quan sát xem chi phí đó có được phân bổ theo quy tắc split charge không. Điều này có thể cần dữ liệu chi phí đáng kể và thời gian xử lý để thấy rõ.

### Giải thích & Lý do

*   **Tại sao sử dụng Cost Categories?**
    *   **Logic nghiệp vụ tùy chỉnh:** Cost Categories cho phép bạn nhóm chi phí theo những cách phù hợp với logic kinh doanh hoặc cấu trúc tổ chức của bạn, điều mà chỉ riêng tags hoặc tài khoản có thể không thực hiện được một cách linh hoạt.
    *   **Phân cấp và Tổ chức Cải thiện:** Chúng giúp ánh xạ chi phí AWS của bạn gần hơn với cấu trúc phòng ban, dự án, hoặc sản phẩm của công ty.
    *   **Đơn giản hóa Báo cáo và Phân tích:** Thay vì lọc theo nhiều tags hoặc tài khoản phức tạp trong Cost Explorer, bạn có thể lọc hoặc nhóm theo một Cost Category Value duy nhất, dễ hiểu hơn.
    *   **Phân bổ Chi phí Dùng chung (Split Charge Rules):** Đây là một tính năng mạnh mẽ cho phép phân bổ chi phí của các tài nguyên hoặc dịch vụ dùng chung (ví dụ: chi phí hỗ trợ, chi phí nền tảng chung, chi phí bảo mật) một cách công bằng và minh bạch cho các đơn vị kinh doanh hoặc dự án tiêu thụ. Phương pháp `PROPORTIONAL` phân bổ chi phí dựa trên tỷ lệ chi phí hiện có của các `targets`.

*   **Resource `aws_cost_category_definition`**:
    *   `name`: Tên duy nhất cho Cost Category của bạn. Đây sẽ là "key" khi bạn thấy nó trong Cost Explorer.
    *   `rule_version`: Hiện tại là `CostCategoryExpression.v1`.
    *   `rule {}`: Mỗi khối `rule` định nghĩa một điều kiện và một `value` (ví dụ: "SharedInfrastructure"). Các quy tắc được đánh giá theo thứ tự.
        *   `dimension {}`: Xác định chiều dữ liệu để khớp (ví dụ: `LINKED_ACCOUNT`, `TAG`, `SERVICE`). Đối với `TAG`, bạn cũng cần chỉ định `tag_key`.
    *   `default_value`: Nếu một mục chi phí không khớp với bất kỳ quy tắc nào, nó sẽ được gán giá trị này. Việc có một `default_value` rõ ràng giúp xác định các chi phí chưa được phân loại.
    *   `split_charge_rule {}`: Định nghĩa cách phân bổ chi phí từ một `source` Cost Category Value cho nhiều `targets` Cost Category Values.
        *   `method`: `PROPORTIONAL` (phân bổ dựa trên chi phí hiện có của các target), `EVEN` (chia đều), `FIXED` (dựa trên tỷ lệ phần trăm được chỉ định trong `parameter`).
    *   `effective_start`: Thời điểm Cost Category bắt đầu áp dụng. Việc đặt là đầu tháng hiện tại đảm bảo tính kịp thời.

*   **Thứ tự Quy tắc (Rule Precedence) và Tính cụ thể:**
    *   Các quy tắc được đánh giá từ trên xuống dưới. Quy tắc đầu tiên khớp sẽ được áp dụng. Do đó, hãy đặt các quy tắc cụ thể hơn (ví dụ: dựa trên tag dự án cụ thể) lên trước các quy tắc chung chung hơn (ví dụ: dựa trên dịch vụ).

*   **Sử dụng `depends_on`**:
    *   `depends_on = [aws_cost_allocation_tag.project_tag_activation]` đảm bảo rằng nếu Terraform đang kích hoạt tag `Project` trong cùng một lần chạy `apply`, nó sẽ hoàn thành việc đó *trước khi* cố gắng tạo Cost Category sử dụng tag đó. Điều này giúp tránh lỗi do tag chưa hoạt động.

*   **Phương pháp Tốt nhất cho năm 2025:**
    *   **Ánh xạ chi phí với logic kinh doanh:** Sử dụng Cost Categories để tạo ra một cái nhìn về chi phí AWS phản ánh cách doanh nghiệp của bạn thực sự hoạt động và phân bổ trách nhiệm.
    *   **Minh bạch chi phí dùng chung:** Tận dụng split charge rules để phân bổ chi phí của các dịch vụ dùng chung một cách công bằng và minh bạch, thúc đẩy việc sử dụng hiệu quả các tài nguyên này và tăng cường trách nhiệm giải trình.
    *   **Tự động hóa và Quản lý dưới dạng Mã (IaC):** Quản lý Cost Categories bằng Terraform đảm bảo tính nhất quán, khả năng lặp lại, kiểm soát phiên bản và tài liệu hóa cho các định nghĩa phân loại chi phí quan trọng này.
    *   **Xem xét và Tinh chỉnh Định kỳ:** Khi cấu trúc kinh doanh hoặc việc sử dụng AWS của bạn thay đổi, hãy xem xét và cập nhật các định nghĩa Cost Category để đảm bảo chúng vẫn phù hợp và chính xác.

### Xác minh
1.  Đăng nhập vào **AWS Management Console**.
2.  Điều hướng đến **AWS Cost Management > Cost Categories**.
3.  Xác nhận rằng Cost Category `OrganizationalUnits` (hoặc tên bạn đã đặt) tồn tại.
4.  Nhấp vào tên Cost Category và kiểm tra:
    *   **Rules:** Đảm bảo các quy tắc cho `SharedInfrastructure`, `StrategicInitiatives`, và `CoreServices` được định nghĩa chính xác.
    *   **Default value:** Phải là `OtherBusinessOperations`.
    *   **Split charge rules:** Xác minh quy tắc chia sẻ chi phí cho `SharedInfrastructure` được cấu hình với các target và phương thức đúng.
5.  **(Sau khi xử lý, có thể mất đến 24 giờ)** Điều hướng đến **AWS Cost Management > Cost Explorer**.
    *   Trong bộ lọc ở bên phải, tìm Cost Category `OrganizationalUnits`.
    *   Thử lọc hoặc nhóm theo Cost Category này. Bạn sẽ thấy các giá trị `SharedInfrastructure`, `StrategicInitiatives`, `CoreServices`, và `OtherBusinessOperations`.
    *   Nếu bạn có chi phí trong các tài khoản được chỉ định cho `SharedInfrastructure` và các tài nguyên được gắn thẻ hoặc sử dụng dịch vụ phù hợp với các quy tắc khác, hãy kiểm tra xem chi phí có được phân bổ theo quy tắc split charge không.

### Dọn dẹp
1.  **Chạy `terraform destroy`**:
    *   Trong terminal, tại thư mục `lab3-cost-categories`, chạy lệnh sau:
        ```bash
        terraform destroy -auto-approve
        ```
    *   Terraform sẽ xóa định nghĩa Cost Category. Nếu bạn cũng đã kích hoạt tag `Project` thông qua biến `activate_project_tag = true`, nó cũng sẽ cố gắng hủy kích hoạt tag đó.

2.  **Xác minh việc xóa trong AWS Management Console**:
    *   Quay lại trang **AWS Cost Management > Cost Categories**.
    *   Xác nhận rằng Cost Category bạn đã tạo không còn được liệt kê.
    *   Nếu tag `Project` được kích hoạt bởi lab này, trạng thái của nó trong **Billing > Cost allocation tags** có thể trở lại `Inactive` sau một thời gian (nếu không có gì khác giữ nó `Active`).

---
Lưu ý:
*   Tên dịch vụ trong quy tắc Cost Category (ví dụ: `"Amazon Elastic Compute Cloud - Compute"`) phải khớp chính xác với tên hiển thị trong Cost Explorer. Bạn có thể cần điều chỉnh chúng dựa trên dữ liệu thực tế trong tài khoản của mình.
*   Việc sử dụng `strftime("%Y-%m-01T00:00:00Z", timestamp())` cho `effective_start` là một cách tốt để đảm bảo nó luôn là đầu tháng hiện tại khi áp dụng.
*   Output `cost_category_rules_summary` được điều chỉnh để cung cấp một cái nhìn tổng quan hơn mà không làm lộ toàn bộ chi tiết quy tắc có thể nhạy cảm.

---

## Lab 4: Thiết lập AWS Cost and Usage Reports (CUR) với Terraform

### Mục tiêu
*   Hiểu tầm quan trọng của **AWS Cost and Usage Reports (CUR)** là nguồn dữ liệu chi tiết và toàn diện nhất về chi phí cũng như mức sử dụng tài nguyên AWS.
*   Học cách định nghĩa và triển khai một **CUR** bằng Terraform, bao gồm cấu hình định dạng tệp, tần suất cập nhật, lựa chọn cột dữ liệu, và đích lưu trữ an toàn trên Amazon S3.
*   Nắm vững cách cấu hình chính sách S3 bucket để cho phép dịch vụ CUR gửi báo cáo.
*   Hiểu sơ bộ về cấu trúc của một CUR và các trường dữ liệu quan trọng, chuẩn bị cho các phân tích sâu hơn.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường.
*   Quyền IAM để tạo và quản lý định nghĩa CUR (`cur:PutReportDefinition`, `cur:DeleteReportDefinition`, `cur:DescribeReportDefinitions`, `cur:ModifyReportDefinition`).
*   Quyền IAM để tạo S3 bucket (`s3:CreateBucket`), đặt chính sách bucket (`s3:PutBucketPolicy`), và quản lý phiên bản bucket (`s3:PutBucketVersioning`, `s3:GetBucketVersioning`).
*   (Tùy chọn nhưng khuyến nghị) Một công cụ để xem và phân tích các tệp Parquet hoặc CSV (ví dụ: trình xem CSV, Python với thư viện Pandas, hoặc các công cụ truy vấn như Amazon Athena).

### Khái niệm được đề cập
*   **AWS Cost and Usage Reports (CUR):** Dịch vụ cung cấp dữ liệu chi tiết nhất về chi phí và mức sử dụng AWS. Báo cáo này bao gồm thông tin về từng mục chi phí, bao gồm các tag tài nguyên, loại giá, chi tiết về Savings Plans và Reserved Instances, v.v., làm nền tảng cho các hoạt động FinOps.
*   **Report Content - `additional_schema_elements`:**
    *   `RESOURCES`: Bao gồm các cột bổ sung chứa Amazon Resource Names (ARNs) của từng tài nguyên, cho phép theo dõi chi phí ở mức độ tài nguyên. **Rất khuyến khích.**
    *   `SPLIT_COST_ALLOCATION_DATA`: Nếu sử dụng Cost Categories với split charge rules, tùy chọn này sẽ thêm các cột hiển thị chi tiết cách chi phí được phân bổ.
*   **Time Granularity - `time_unit`:**
    *   `HOURLY`: Dữ liệu được tổng hợp và báo cáo hàng giờ. Cung cấp độ chi tiết cao nhất.
    *   `DAILY`: Dữ liệu được tổng hợp hàng ngày.
    *   `MONTHLY`: Dữ liệu được tổng hợp hàng tháng (ít chi tiết hơn, thường không đủ cho phân tích sâu).
*   **Format & Compression - `format`, `compression`:**
    *   `textORcsv` (CSV): Định dạng tệp văn bản thuần túy, dễ đọc nhưng kém hiệu quả cho dữ liệu lớn.
    *   `Parquet`: Định dạng lưu trữ dạng cột, tối ưu cho phân tích dữ liệu lớn, tiết kiệm không gian và tăng tốc độ truy vấn. **Đây là lựa chọn hàng đầu.**
    *   Nén: `GZIP`, `ZIP` (cho CSV), `Parquet` (Parquet tự nó là một định dạng nén hiệu quả).
*   **S3 Bucket Delivery:** CUR được gửi đến một S3 bucket do bạn chỉ định. Chính sách bucket phải được cấu hình đúng.
*   **Report Path Prefix - `s3_prefix`:** Một tiền tố trong S3 bucket để tổ chức các báo cáo CUR, đặc biệt hữu ích nếu bucket được sử dụng cho nhiều mục đích.
*   **Report Versioning - `report_versioning`:** Cách CUR xử lý các phiên bản báo cáo khi dữ liệu được cập nhật (ví dụ: do hoàn tiền, tín dụng, hoặc dữ liệu đến muộn). `CREATE_NEW_REPORT` tạo một phiên bản báo cáo mới, trong khi `OVERWRITE_REPORT` ghi đè lên báo cáo hiện có.
*   **Refresh Closed Reports - `refresh_closed_reports`:** Cho phép AWS cập nhật các báo cáo của các tháng đã đóng nếu có dữ liệu mới hoặc điều chỉnh.
*   **Additional Artifacts - `additional_artifacts`:** Tạo các tệp manifest bổ sung để dễ dàng tích hợp với các dịch vụ khác như `ATHENA`, `QUICKSIGHT`, `REDSHIFT`.

### Cấu hình Terraform

Tạo một thư mục mới cho lab này, ví dụ `lab4-aws-cur`. Bên trong thư mục đó, tạo các tệp sau:

1.  **`main.tf`**:
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
      region = var.aws_region # CUR definitions are regional. The S3 bucket can be in any region,
                              # but for simplicity, we use the same region here.
    }

    # Get current AWS account ID and region for policy conditions
    data "aws_caller_identity" "current" {}
    data "aws_region" "current_region" {} # Ensures we use the provider's configured region

    # Create an S3 bucket to store the Cost and Usage Reports
    resource "aws_s3_bucket" "cur_storage_bucket" {
      bucket = var.s3_bucket_name
      # ACLs are disabled by default for new buckets.
      # Consider server-side encryption (SSE-S3 or SSE-KMS) for enhanced security.
    }

    # Enable versioning on the S3 bucket (highly recommended for CUR)
    resource "aws_s3_bucket_versioning" "cur_storage_bucket_versioning" {
      bucket = aws_s3_bucket.cur_storage_bucket.id
      versioning_configuration {
        status = "Enabled"
      }
    }

    # Policy to allow CUR service to write to the S3 bucket
    # This policy is based on AWS documentation for CUR delivery.
    data "aws_iam_policy_document" "cur_bucket_policy_doc" {
      statement {
        sid    = "CURPutObjectAccess"
        effect = "Allow"
        principals {
          type        = "Service"
          identifiers = ["billingreports.amazonaws.com"] # The CUR service principal
        }
        actions   = ["s3:PutObject"]
        resources = ["${aws_s3_bucket.cur_storage_bucket.arn}/${var.s3_report_prefix}/*"] # Grants write access to the specified prefix

        # Condition to ensure the CUR service is writing on behalf of your account
        # and for reports defined in your account and region.
        condition {
          test     = "StringEquals"
          variable = "aws:SourceAccount"
          values   = [data.aws_caller_identity.current.account_id]
        }
        condition {
          test     = "ArnLike"
          variable = "aws:SourceArn"
          # Using data.aws_region.current_region.name to ensure the region matches the provider's configured region
          values   = ["arn:aws:cur:${data.aws_region.current_region.name}:${data.aws_caller_identity.current.account_id}:definition/*"]
        }
      }

      statement {
        sid    = "CURBucketOwnershipCheck"
        effect = "Allow"
        principals {
          type        = "Service"
          identifiers = ["billingreports.amazonaws.com"]
        }
        actions   = ["s3:GetBucketAcl", "s3:GetBucketPolicy"] # Permissions CUR needs to validate bucket ownership and policy
        resources = [aws_s3_bucket.cur_storage_bucket.arn]

        condition {
          test     = "StringEquals"
          variable = "aws:SourceAccount"
          values   = [data.aws_caller_identity.current.account_id]
        }
        condition {
          test     = "ArnLike"
          variable = "aws:SourceArn"
          values   = ["arn:aws:cur:${data.aws_region.current_region.name}:${data.aws_caller_identity.current.account_id}:definition/*"]
        }
      }
    }

    resource "aws_s3_bucket_policy" "cur_storage_bucket_policy" {
      bucket = aws_s3_bucket.cur_storage_bucket.id
      policy = data.aws_iam_policy_document.cur_bucket_policy_doc.json
    }

    # Define the Cost and Usage Report
    resource "aws_cur_report_definition" "primary_cur_definition" {
      report_name                = var.cur_report_name
      time_unit                  = var.cur_time_unit         # HOURLY, DAILY, or MONTHLY
      format                     = var.cur_format            # textORcsv or Parquet
      compression                = var.cur_compression       # GZIP, ZIP, or Parquet (if format is Parquet)
      additional_schema_elements = var.cur_schema_elements # e.g., ["RESOURCES", "SPLIT_COST_ALLOCATION_DATA"]
      
      s3_bucket                  = aws_s3_bucket.cur_storage_bucket.bucket # Name of the S3 bucket
      s3_prefix                  = var.s3_report_prefix                    # Path prefix within the bucket
      s3_region                  = data.aws_region.current_region.name     # Region where the S3 bucket is located

      additional_artifacts       = var.cur_additional_artifacts # e.g., ["REDSHIFT", "QUICKSIGHT", "ATHENA"]
      refresh_closed_reports     = true  # Recommended: Enable to refresh reports with late-arriving data (e.g. refunds)
      report_versioning          = "CREATE_NEW_REPORT" # Recommended: OVERWRITE_REPORT or CREATE_NEW_REPORT

      tags = {
        ManagedBy   = "Terraform"
        Purpose     = "Lab4 Detailed Cost and Usage Report"
        Owner       = var.owner_tag
        Sensitivity = "High" # CUR data is sensitive
      }

      # Ensure bucket policy is in place before CUR definition tries to validate/use the bucket
      depends_on = [aws_s3_bucket_policy.cur_storage_bucket_policy]
    }
    ```

2.  **`variables.tf`**:
    ```terraform
    # variables.tf

    variable "aws_region" {
      description = "The AWS region for CUR definition and S3 bucket. CUR definitions are regional."
      type        = string
      default     = "us-east-1" # Choose a region where you want to define CUR and create the S3 bucket
    }

    variable "s3_bucket_name" {
      description = "A globally unique name for the S3 bucket to store CUR reports. Will have a random suffix if not unique."
      type        = string
      # It's critical that this name is globally unique.
      # Example: default = "my-org-cur-reports-1a2b3c"
      # You MUST provide a unique name or ensure your naming convention generates one.
    }

    variable "s3_report_prefix" {
      description = "The prefix for the report path within the S3 bucket (e.g., 'billing/cur-data'). No leading/trailing slashes."
      type        = string
      default     = "cost-usage-reports"
      validation {
        condition     = !can(regex("^/|/$", var.s3_report_prefix)) && var.s3_report_prefix != ""
        error_message = "s3_report_prefix must not be empty and must not have leading or trailing slashes."
      }
    }

    variable "cur_report_name" {
      description = "The name for the Cost and Usage Report definition."
      type        = string
      default     = "PrimaryOrganizationCUR"
    }

    variable "cur_time_unit" {
      description = "The granularity of time for the report (HOURLY, DAILY, MONTHLY)."
      type        = string
      default     = "HOURLY" # HOURLY provides the most detail
      validation {
        condition     = contains(["HOURLY", "DAILY", "MONTHLY"], var.cur_time_unit)
        error_message = "Allowed values for cur_time_unit are HOURLY, DAILY, or MONTHLY."
      }
    }

    variable "cur_format" {
      description = "The format of the report files (textORcsv or Parquet)."
      type        = string
      default     = "Parquet" # Parquet is highly recommended for performance and cost
      validation {
        condition     = contains(["textORcsv", "Parquet"], var.cur_format)
        error_message = "Allowed values for cur_format are textORcsv or Parquet."
      }
    }

    variable "cur_compression" {
      description = "The compression type for the report (GZIP, ZIP for textORcsv; Parquet for Parquet format)."
      type        = string
      default     = "Parquet" # If format is Parquet, compression must be Parquet.
      # More robust validation could be added here to link cur_format and cur_compression.
      # For example, if cur_format is "Parquet", cur_compression must be "Parquet".
      # If cur_format is "textORcsv", cur_compression can be "GZIP" or "ZIP".
      validation {
        condition = (var.cur_format == "Parquet" && var.cur_compression == "Parquet") ||
                      (var.cur_format == "textORcsv" && contains(["GZIP", "ZIP"], var.cur_compression))
        error_message = "Invalid compression type for the selected format. For Parquet format, use Parquet compression. For textORcsv, use GZIP or ZIP."
      }
    }

    variable "cur_schema_elements" {
      description = "A list of additional schema elements to include (e.g., RESOURCES, SPLIT_COST_ALLOCATION_DATA)."
      type        = list(string)
      default     = ["RESOURCES", "SPLIT_COST_ALLOCATION_DATA"] # RESOURCES is highly recommended
    }

    variable "cur_additional_artifacts" {
      description = "A list of additional artifacts to generate (e.g., REDSHIFT, QUICKSIGHT, ATHENA)."
      type        = list(string)
      default     = ["ATHENA", "QUICKSIGHT"] # Generates manifest files for easy integration
    }

    variable "owner_tag" {
      description = "Value for the Owner tag applied to the CUR definition."
      type        = string
      default     = "FinOpsLead@example.com"
    }
    ```
    *   **Quan trọng:** Bạn **phải** cung cấp một giá trị duy nhất toàn cầu cho `s3_bucket_name` khi chạy `terraform apply` (ví dụ, qua một tệp `.tfvars` hoặc `-var` flag) vì tên S3 bucket phải là duy nhất toàn cầu. Ví dụ: `terraform apply -var="s3_bucket_name=my-org-cur-reports-unique123"`.

3.  **`outputs.tf`**:
    ```terraform
    # outputs.tf

    output "cur_report_definition_name" {
      description = "The name of the created Cost and Usage Report definition."
      value       = aws_cur_report_definition.primary_cur_definition.report_name
    }

    output "cur_s3_delivery_bucket" {
      description = "The S3 bucket where the CUR reports will be delivered."
      value       = aws_cur_report_definition.primary_cur_definition.s3_bucket
    }

    output "cur_s3_delivery_prefix" {
      description = "The S3 prefix for the CUR reports within the bucket."
      value       = aws_cur_report_definition.primary_cur_definition.s3_prefix
    }

    output "cur_s3_full_path_example" {
      description = "Example S3 path structure for the CUR reports and manifest file."
      # Example path: s3://your-bucket-name/your-prefix/YourReportName/YYYYMMDD-YYYYMMDD/YourReportName-manifest.json
      value       = "s3://${aws_cur_report_definition.primary_cur_definition.s3_bucket}/${aws_cur_report_definition.primary_cur_definition.s3_prefix}/${aws_cur_report_definition.primary_cur_definition.report_name}/<DateRange>/${aws_cur_report_definition.primary_cur_definition.report_name}-manifest.json"
    }

    output "cur_bucket_policy_json" {
      description = "The JSON policy applied to the S3 bucket for CUR delivery."
      value       = data.aws_iam_policy_document.cur_bucket_policy_doc.json
      sensitive   = true # Policy might contain account ID
    }
    ```

### Hướng dẫn từng bước

1.  **Tạo thư mục dự án và các tệp `.tf`**:
    *   Tạo một thư mục có tên `lab4-aws-cur`.
    *   Bên trong thư mục này, tạo các tệp `main.tf`, `variables.tf`, và `outputs.tf` với nội dung như trên.
    *   **Quan trọng:** Chuẩn bị một tên S3 bucket duy nhất toàn cầu. Bạn sẽ cần cung cấp nó khi chạy `apply`.

2.  **Chạy `terraform init`**:
    *   Mở terminal, điều hướng đến thư mục `lab4-aws-cur`.
    *   Chạy:
        ```bash
        terraform init
        ```

3.  **Chạy `terraform plan`**:
    *   Xem kế hoạch thực thi. Bạn phải cung cấp `s3_bucket_name`:
        ```bash
        terraform plan -var="s3_bucket_name=your-globally-unique-bucket-name"
        ```
        (Thay `your-globally-unique-bucket-name` bằng tên bucket bạn đã chọn).
    *   Terraform sẽ hiển thị rằng nó sẽ tạo một S3 bucket (với policy và versioning) và một resource `aws_cur_report_definition`.

4.  **Chạy `terraform apply`**:
    *   Áp dụng các thay đổi. Nhớ cung cấp `s3_bucket_name`:
        ```bash
        terraform apply -var="s3_bucket_name=your-globally-unique-bucket-name" -auto-approve
        ```
    *   Terraform sẽ tạo S3 bucket và định nghĩa CUR.

5.  **Xác minh trong AWS Management Console**:
    *   Đăng nhập vào AWS Management Console.
    *   Điều hướng đến dịch vụ **Billing and Cost Management**.
    *   Nhấp vào **Cost & Usage Reports** trong menu điều hướng bên trái.
    *   Bạn sẽ thấy định nghĩa báo cáo mới của mình (ví dụ: `PrimaryOrganizationCUR`) được liệt kê.
    *   Nhấp vào tên báo cáo để xem chi tiết. Xác minh rằng đích S3, tiền tố (`s3_prefix`), định dạng, đơn vị thời gian, và các phần tử schema bổ sung (`additional_schema_elements`) khớp với cấu hình Terraform của bạn.
    *   Điều hướng đến dịch vụ **S3**.
    *   Tìm bucket S3 mà bạn đã tạo.
    *   Kiểm tra tab **Permissions > Bucket policy**. Bạn sẽ thấy chính sách cho phép dịch vụ CUR (`billingreports.amazonaws.com`) ghi vào bucket.
    *   **Lưu ý:** AWS sẽ bắt đầu gửi các tệp CUR đến S3 bucket này. Quá trình gửi báo cáo đầu tiên có thể mất đến 24 giờ. Các báo cáo tiếp theo sẽ được gửi định kỳ (thường một hoặc nhiều lần mỗi ngày, tùy thuộc vào tần suất dữ liệu được cập nhật).

6.  **(Sau khi báo cáo được gửi) Kiểm tra S3 Bucket và Nội dung CUR (Tùy chọn)**:
    *   Sau khoảng 24 giờ, kiểm tra S3 bucket của bạn trong đường dẫn `s3://<your-bucket-name>/<your-s3-report-prefix>/<your-cur-report-name>/`.
    *   Bạn sẽ thấy một cấu trúc thư mục được tạo bởi CUR, thường là:
        `<cur-report-name>/<YYYYMMDD-YYYYMMDD>/<assemblyId>/<cur-report-name>-<file_number>.parquet` (nếu dùng Parquet) hoặc `.csv.gz` (nếu dùng CSV).
        Cũng sẽ có một tệp `<cur-report-name>-manifest.json` trong thư mục `<YYYYMMDD-YYYYMMDD>`.
    *   Nếu bạn đã chọn `ATHENA` và/hoặc `QUICKSIGHT` trong `additional_artifacts`, bạn sẽ thấy các tệp manifest tương ứng (ví dụ: `<cur-report-name>-PrestoAthena-manifest.json`) hữu ích cho việc thiết lập tích hợp.
    *   Bạn có thể tải xuống một tệp báo cáo (ví dụ: tệp `.parquet`) và thử mở nó bằng một công cụ tương thích để xem lướt qua các cột.

### Giải thích & Lý do

*   **Tại sao CUR là tối quan trọng?**
    *   **Nguồn Dữ liệu Gốc Toàn Diện Nhất:** CUR cung cấp mức độ chi tiết cao nhất về chi phí và mức sử dụng AWS, bao gồm thông tin không có sẵn trong Cost Explorer hoặc các báo cáo tóm tắt khác. Đây là "single source of truth" cho dữ liệu chi phí.
    *   **Nền tảng cho Phân tích FinOps Nâng cao:** Nó là nguồn dữ liệu chính để xây dựng các dashboard tùy chỉnh (ví dụ: với QuickSight), thực hiện các truy vấn phức tạp (ví dụ: với Athena), và tích hợp với các công cụ FinOps của bên thứ ba.
    *   **Phân bổ Chi phí Chính xác:** Với cột `lineItem/ResourceId` (khi `RESOURCES` được bao gồm trong `additional_schema_elements`) và dữ liệu tag chi tiết, bạn có thể phân bổ chi phí đến từng tài nguyên cụ thể.
    *   **Hiểu các Mô hình Giá Phức tạp:** CUR giúp làm sáng tỏ chi phí liên quan đến Savings Plans, Reserved Instances, Spot Instances, chi phí truyền dữ liệu, v.v.

*   **Resource `aws_cur_report_definition`**:
    *   `time_unit = "HOURLY"`: Cung cấp chi tiết nhất, rất hữu ích cho việc phân tích các thay đổi chi phí đột ngột hoặc hiểu mô hình sử dụng theo giờ.
    *   `format = "Parquet"`: Định dạng cột, hiệu quả hơn nhiều cho việc lưu trữ và truy vấn so với `textORcsv`, đặc biệt với khối lượng dữ liệu lớn. **Parquet là phương pháp tốt nhất cho năm 2025.**
    *   `compression = "Parquet"`: Khi `format` là `Parquet`, `compression` cũng phải là `Parquet`.
    *   `additional_schema_elements = ["RESOURCES", "SPLIT_COST_ALLOCATION_DATA"]`:
        *   `RESOURCES`: **Cực kỳ quan trọng!** Bao gồm cột `lineItem/ResourceId` chứa ARN của từng tài nguyên, cho phép bạn theo dõi chi phí đến từng tài nguyên.
        *   `SPLIT_COST_ALLOCATION_DATA`: Nếu bạn sử dụng Cost Categories với các quy tắc chia sẻ chi phí, tùy chọn này sẽ bao gồm các cột bổ sung hiển thị cách chi phí được phân bổ.
    *   `s3_bucket`, `s3_prefix`, `s3_region`: Xác định nơi CUR sẽ được gửi. `s3_prefix` giúp tổ chức các báo cáo trong bucket.
    *   `additional_artifacts = ["ATHENA", "QUICKSIGHT"]`: Tạo các tệp manifest giúp dễ dàng tạo bảng Athena để truy vấn CUR hoặc nhập dữ liệu vào QuickSight.
    *   `refresh_closed_reports = true`: Cho phép AWS cập nhật các báo cáo đã đóng của các tháng trước nếu có dữ liệu đến muộn. **Khuyến nghị đặt thành `true` để đảm bảo tính chính xác.**
    *   `report_versioning = "CREATE_NEW_REPORT"`: Mỗi khi AWS cập nhật báo cáo, nó sẽ tạo một tập hợp tệp báo cáo mới. `OVERWRITE_REPORT` sẽ ghi đè lên các tệp hiện có. **`CREATE_NEW_REPORT` an toàn hơn và được khuyến nghị.**

*   **Chính sách S3 Bucket (Bucket Policy):**
    *   Chính sách được cung cấp tuân theo các khuyến nghị của AWS, cho phép dịch vụ CUR (`billingreports.amazonaws.com`) có quyền `s3:PutObject` (để ghi báo cáo) và `s3:GetBucketAcl`, `s3:GetBucketPolicy` (để CUR xác minh quyền sở hữu bucket và chính sách trước khi gửi dữ liệu).
    *   Các điều kiện `aws:SourceAccount` và `aws:SourceArn` trong policy là các biện pháp bảo mật quan trọng để đảm bảo chỉ dịch vụ CUR từ tài khoản của bạn, và cho các định nghĩa báo cáo trong tài khoản và khu vực đó, mới có thể tương tác với bucket.

*   **Phương pháp Tốt nhất cho năm 2025:**
    *   **Luôn kích hoạt CUR:** Đây là nền tảng không thể thiếu cho mọi hoạt động FinOps và quản lý chi phí đám mây hiệu quả.
    *   **Sử dụng `HOURLY` và `Parquet`:** Để có độ chi tiết tối đa và hiệu quả truy vấn, giúp phân tích nhanh chóng và tiết kiệm chi phí lưu trữ/truy vấn.
    *   **Bao gồm `RESOURCES` và `SPLIT_COST_ALLOCATION_DATA` (nếu có Cost Categories):** Để có khả năng hiển thị và phân bổ chi phí tốt nhất.
    *   **Kích hoạt `refresh_closed_reports = true` và `report_versioning = "CREATE_NEW_REPORT"`:** Để đảm bảo dữ liệu chính xác và an toàn.
    *   **Tự động hóa việc thiết lập và quản lý CUR bằng IaC (Terraform):** Đảm bảo tính nhất quán, kiểm soát phiên bản và dễ dàng tái tạo.
    *   **Tích hợp với các công cụ phân tích (ví dụ: Athena, QuickSight):** Tận dụng các tệp manifest được tạo để nhanh chóng bắt đầu khai thác giá trị từ dữ liệu CUR.

### Xác minh
1.  Đăng nhập vào **AWS Management Console**.
2.  Điều hướng đến **Billing and Cost Management > Cost & Usage Reports**.
3.  Xác nhận rằng định nghĩa báo cáo `PrimaryOrganizationCUR` (hoặc tên bạn đã đặt) tồn tại và các cấu hình của nó (đích S3, định dạng, đơn vị thời gian) là chính xác.
4.  Điều hướng đến **S3**.
5.  Tìm bucket S3 đã tạo. Kiểm tra **Permissions > Bucket policy**.
6.  **(Sau 24 giờ)** Kiểm tra S3 bucket trong đường dẫn `s3://<your-bucket-name>/<your-s3-report-prefix>/<your-cur-report-name>/` để xem các tệp CUR và manifest đã được gửi chưa.
7.  Nếu bạn đã chọn `ATHENA` trong `additional_artifacts`, hãy tìm tệp manifest tương ứng (ví dụ: `<cur-report-name>-PrestoAthena-manifest.json`).

### Dọn dẹp
1.  **Chạy `terraform destroy`**:
    *   Trong terminal, tại thư mục `lab4-aws-cur`, chạy lệnh sau, nhớ cung cấp `s3_bucket_name`:
        ```bash
        terraform destroy -var="s3_bucket_name=your-globally-unique-bucket-name" -auto-approve
        ```
    *   Terraform sẽ cố gắng xóa định nghĩa CUR và S3 bucket.
    *   **Lưu ý quan trọng về việc xóa S3 Bucket:**
        *   Theo mặc định, Terraform không thể xóa một S3 bucket nếu nó không trống. Các tệp CUR sẽ được gửi vào bucket này.
        *   **Đối với mục đích lab**, để đảm bảo `terraform destroy` thành công, bạn có thể thêm `force_destroy = true` vào resource `aws_s3_bucket.cur_storage_bucket`. **CẢNH BÁO: `force_destroy = true` sẽ xóa tất cả các đối tượng trong bucket khi bucket bị xóa. KHÔNG sử dụng tùy chọn này một cách bất cẩn trong môi trường sản xuất.**

        ```terraform
        # TRONG main.tf, CHỈ DÀNH CHO LAB:
        resource "aws_s3_bucket" "cur_storage_bucket" {
          bucket = var.s3_bucket_name
          force_destroy = true # !!! LAB ONLY - DELETES ALL OBJECTS IN BUCKET ON DESTROY !!!
        }
        ```
        *   Nếu không sử dụng `force_destroy`, bạn có thể cần xóa thủ công các đối tượng trong S3 bucket trước khi `terraform destroy` có thể xóa bucket.

2.  **Xác minh việc xóa trong AWS Management Console**:
    *   Quay lại trang **Billing and Cost Management > Cost & Usage Reports**. Xác nhận định nghĩa báo cáo không còn.
    *   Quay lại trang **S3**. Xác nhận S3 bucket không còn tồn tại.

---
Lưu ý:
*   Bucket policy đã được cập nhật để bao gồm các quyền `s3:GetBucketAcl` và `s3:GetBucketPolicy` mà dịch vụ CUR cần để xác minh quyền sở hữu bucket, theo tài liệu mới nhất của AWS.
*   Điều kiện `aws:SourceArn` trong bucket policy đã được làm rõ hơn để sử dụng `data.aws_region.current_region.name` nhằm đảm bảo tính chính xác của ARN theo vùng cấu hình của provider.
*   Validation cho `s3_report_prefix` và `cur_compression` đã được thêm/cải thiện.

*(Chúng ta sẽ tiếp tục với Lab 5, tập trung vào các tính năng nâng cao của AWS Budgets, bao gồm Budget Actions.)*

---

## Lab 5: AWS Budgets Nâng cao: Budget Actions và Thông báo Chi tiết với Terraform

### Mục tiêu
*   Hiểu và triển khai **AWS Budget Actions** để tự động hóa các phản ứng khi budget đạt ngưỡng nhất định, ví dụ như áp dụng một IAM policy để hạn chế quyền.
*   Học cách cấu hình thông báo budget chi tiết hơn bằng cách sử dụng **Amazon SNS (Simple Notification Service)**.
*   Sử dụng Terraform để quản lý các cấu hình nâng cao này cho AWS Budgets, bao gồm cả việc tạo các IAM roles và policies cần thiết.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường và Lab 1: Tạo AWS Budgets Cơ bản với Terraform.
*   Quyền IAM để tạo và quản lý Budgets và Budget Actions (`budgets:*`).
*   Quyền IAM để tạo và quản lý SNS topics và subscriptions (`sns:*`).
*   Quyền IAM để tạo và quản lý IAM policies và roles (`iam:CreatePolicy`, `iam:DeletePolicy`, `iam:GetPolicy`, `iam:CreateRole`, `iam:DeleteRole`, `iam:AttachRolePolicy`, `iam:DetachRolePolicy`, `iam:ListRoles`).
*   Một IAM Role đã tồn tại trong tài khoản của bạn để làm mục tiêu cho Budget Action (hoặc bạn sẽ tạo một role ví dụ như một phần của lab này).
*   Địa chỉ email hợp lệ để nhận thông báo SNS và xác nhận subscription.

### Khái niệm được đề cập
*   **AWS Budget Actions:** Cho phép bạn cấu hình các hành động tự động được thực hiện khi một budget đạt đến một ngưỡng chi phí cụ thể (thực tế hoặc dự kiến).
    *   **Action Types:**
        *   `APPLY_IAM_POLICY`: Áp dụng một IAM policy (do khách hàng quản lý hoặc do AWS quản lý) cho một user, group, hoặc role. Đây là một cách hiệu quả để hạn chế quyền khi chi phí vượt ngưỡng.
        *   `APPLY_SCP_POLICY`: (Trong AWS Organizations) Áp dụng một Service Control Policy (SCP) cho một tài khoản hoặc Organizational Unit (OU).
        *   `RUN_SSM_DOCUMENTS` (Systems Manager): Chạy một hoặc nhiều tài liệu Systems Manager (SSM Documents) trên các EC2 instances được chỉ định.
    *   **Approval Model:** Hành động có thể yêu cầu phê duyệt thủ công (`MANUAL`) hoặc tự động thực thi (`AUTOMATIC`).
    *   **Execution Role:** Một IAM role mà dịch vụ AWS Budgets sẽ đảm nhận (assume) để thực hiện hành động. Role này cần có các quyền cần thiết cho hành động đó (ví dụ: quyền đính kèm/gỡ bỏ IAM policy).
*   **Thông báo Budget qua Amazon SNS:** Thay vì chỉ gửi email trực tiếp, việc sử dụng SNS topic cho thông báo budget mang lại sự linh hoạt cao hơn, cho phép gửi đến nhiều điểm cuối (email, SMS, Lambda, SQS) và dễ dàng tích hợp với các quy trình tự động khác.
*   **IAM Policy:** Tài liệu JSON định nghĩa các quyền. Trong lab này, chúng ta sẽ tạo một policy hạn chế (ví dụ: deny tạo EC2 instances) để Budget Action áp dụng.

### Cấu hình Terraform

Tạo một thư mục mới cho lab này, ví dụ `lab5-advanced-budgets`. Bên trong thư mục đó, tạo các tệp sau:

1.  **`main.tf`**:
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
      region = var.aws_region
    }

    data "aws_caller_identity" "current" {}
    data "aws_partition" "current" {} # To construct ARNs correctly across partitions (aws, aws-cn, etc.)

    # --- IAM Resources for Budget Action ---

    # 1. The restrictive IAM policy to be APPLIED by the budget action.
    resource "aws_iam_policy" "restrictive_policy_for_budget_action" {
      name        = "BudgetAction_DenyEC2Creation-${var.environment}"
      description = "Denies ec2:RunInstances. Applied by AWS Budget action."
      policy = jsonencode({
        Version = "2012-10-17",
        Statement = [
          {
            Effect   = "Deny",
            Action   = "ec2:RunInstances",
            Resource = "*" # Denies for all resources; can be more specific
          }
        ]
      })
      tags = {
        ManagedBy = "Terraform-Lab5"
      }
    }

    # 2. The IAM Role that AWS Budgets service will assume to EXECUTE the action.
    resource "aws_iam_role" "budget_action_execution_role" {
      name = "AWSBudgetsActionExecutionRole-${var.aws_region}-${var.environment}"
      # Trust policy allowing budgets.amazonaws.com to assume this role.
      assume_role_policy = jsonencode({
        Version = "2012-10-17",
        Statement = [
          {
            Effect = "Allow",
            Principal = {
              Service = "budgets.amazonaws.com"
            },
            Action = "sts:AssumeRole",
            # Optional: Condition to restrict which budget can assume this role.
            # This adds an extra layer of security.
            Condition = {
              StringEquals = {
                "aws:SourceAccount" = data.aws_caller_identity.current.account_id
              },
              ArnLike = {
                # This ARN should ideally point to the specific budget ARN after it's created.
                # However, due to circular dependency, we use a wildcard here for simplicity in the lab.
                # In production, you might create the role first, then the budget, then update the role's trust policy.
                "aws:SourceArn" = "arn:${data.aws_partition.current.partition}:budgets::${data.aws_caller_identity.current.account_id}:budget/*"
              }
            }
          }
        ]
      })
      tags = {
        ManagedBy = "Terraform-Lab5"
      }
    }

    # 3. The IAM Policy granting the Execution Role permissions to perform the defined action.
    #    In this case, to attach/detach the specific restrictive_policy_for_budget_action to other roles.
    resource "aws_iam_policy" "budget_execution_role_permissions_policy" {
      name        = "BudgetActionExecutionPermissions-${var.environment}"
      description = "Allows budget action role to manage a specific IAM policy on target roles."
      policy = jsonencode({
        Version = "2012-10-17",
        Statement = [
          {
            Effect   = "Allow",
            Action = [
              "iam:AttachRolePolicy", # Permission to attach the policy
              "iam:DetachRolePolicy"  # Permission to detach the policy (when action is reversed or budget deleted)
            ],
            # IMPORTANT: This resource should be the ARN of the policy that will be APPLIED (restrictive_policy_for_budget_action).
            Resource = aws_iam_policy.restrictive_policy_for_budget_action.arn
            # Optional: Condition to restrict which roles this execution role can modify.
            # Condition = {
            #   ArnEquals = {
            #     "iam:PolicyARN" = aws_iam_policy.restrictive_policy_for_budget_action.arn
            #   }
            # }
          },
          # The following permission is often needed if your target definition uses role names
          # and Budgets service needs to resolve the name to an ARN, or to list roles.
          # However, for applying a known policy ARN to a known target role ARN, it might not be strictly necessary.
          # For simplicity and broader applicability, it's often included.
          # {
          #   Effect = "Allow",
          #   Action = "iam:ListRoles",
          #   Resource = "*"
          # }
          # If targeting users: "iam:AttachUserPolicy", "iam:DetachUserPolicy", "iam:ListUsers"
          # If targeting groups: "iam:AttachGroupPolicy", "iam:DetachGroupPolicy", "iam:ListGroups"
        ]
      })
      tags = {
        ManagedBy = "Terraform-Lab5"
      }
    }

    # 4. Attach the permissions policy to the execution role.
    resource "aws_iam_role_policy_attachment" "budget_execution_role_policy_attachment" {
      role       = aws_iam_role.budget_action_execution_role.name
      policy_arn = aws_iam_policy.budget_execution_role_permissions_policy.arn
    }

    # --- (Optional) Target IAM Role for the Budget Action ---
    # This role is the one that will have the restrictive_policy_for_budget_action applied to it.
    # For the lab, we create a simple target role. In a real scenario, this would be an existing role.
    resource "aws_iam_role" "example_target_role_for_action" {
      count = var.create_example_target_role ? 1 : 0 # Only create if variable is true

      name = var.target_iam_role_name_for_action # e.g., "DeveloperTeamRole"
      assume_role_policy = jsonencode({
        Version = "2012-10-17",
        Statement = [{
          Action    = "sts:AssumeRole",
          Effect    = "Allow",
          Principal = { Service = "ec2.amazonaws.com" } # Example: role for EC2 instances
        }]
      })
      tags = {
        ManagedBy = "Terraform-Lab5-TargetRole"
        Purpose   = "Target for Budget Action Lab"
      }
    }

    # --- SNS Topic for Budget Notifications ---
    resource "aws_sns_topic" "budget_alerts_sns_topic" {
      name = "budget-alerts-sns-topic-${var.environment}"
      tags = {
        ManagedBy = "Terraform-Lab5"
      }
    }

    # Subscribe an email endpoint to the SNS topic.
    resource "aws_sns_topic_subscription" "budget_email_sns_subscription" {
      topic_arn = aws_sns_topic.budget_alerts_sns_topic.arn
      protocol  = "email"
      endpoint  = var.notification_email # User must confirm this subscription via email.
    }

    # --- AWS Budget with Action and Enhanced Notification ---
    resource "aws_budgets_budget" "monthly_cost_budget_advanced" {
      name              = "MonthlyAdvBudget-${var.environment}"
      budget_type       = "COST"
      limit_amount      = var.budget_limit_amount
      limit_unit        = "USD"
      time_unit         = "MONTHLY"
      account_id        = data.aws_caller_identity.current.account_id # Explicitly setting account ID

      # Notification via SNS when actual spend reaches 85% of the budgeted amount.
      notification {
        comparison_operator       = "GREATER_THAN"
        threshold                 = 85
        threshold_type            = "PERCENTAGE"
        notification_type         = "ACTUAL" # Can also be FORECASTED
        subscriber_sns_topic_arns = [aws_sns_topic.budget_alerts_sns_topic.arn]
        # subscriber_email_addresses = [var.notification_email] # Can also use direct email for simpler notifications
      }

      # Budget Action: Apply the restrictive IAM policy when actual spend reaches 95%.
      action {
        action_type        = "APPLY_IAM_POLICY"
        approval_model     = "AUTOMATIC" # or "MANUAL"
        execution_role_arn = aws_iam_role.budget_action_execution_role.arn
        notification_type  = "ACTUAL"

        action_threshold {
          action_threshold_type  = "PERCENTAGE" # or "ABSOLUTE_VALUE"
          action_threshold_value = 95
        }

        definition {
          iam_action_definition {
            policy_arn = aws_iam_policy.restrictive_policy_for_budget_action.arn
            # Apply to the target IAM role.
            # If var.create_example_target_role is true, it targets the role created in this lab.
            # Otherwise, it targets the role name provided in var.target_iam_role_name_for_action, which must exist.
            roles = [var.target_iam_role_name_for_action]
            # users  = ["arn:aws:iam::ACCOUNT_ID:user/some-user"] # Can also target users or groups by ARN or name
            # groups = ["FinanceTeamGroup"]
          }
          # Example for SCP Action (conceptual, requires AWS Organizations setup):
          # scp_action_definition {
          #   policy_id = "p-xxxxxxxx" # ARN or ID of the SCP
          #   target_ids = ["ou-xxxxxxxx-yyyyyyyy", data.aws_caller_identity.current.account_id] # OU IDs or Account IDs
          # }
          # Example for SSM Action (conceptual):
          # ssm_action_definition {
          #   action_sub_type = "STOP_EC2_INSTANCES" # or "STOP_RDS_INSTANCES"
          #   instance_ids    = ["i-xxxxxxxxxxxxxxxxx"]
          #   region          = var.aws_region
          # }
        }
        
        # Notify via SNS when the action is triggered.
        subscriber_sns_topic_arns = [aws_sns_topic.budget_alerts_sns_topic.arn]
      }

      tags = {
        ManagedBy   = "Terraform-Lab5"
        Purpose     = "Advanced Budget with Action and SNS"
        Environment = var.environment
        Owner       = var.owner_tag
      }

      depends_on = [
        aws_iam_role_policy_attachment.budget_execution_role_policy_attachment,
        aws_sns_topic_subscription.budget_email_sns_subscription,
        aws_iam_role.example_target_role_for_action, # Ensure target role exists if created here
      ]
    }
    ```

2.  **`variables.tf`**:
    ```terraform
    # variables.tf

    variable "aws_region" {
      description = "The AWS region for deploying resources."
      type        = string
      default     = "us-east-1"
    }

    variable "environment" {
      description = "An environment identifier (e.g., dev, test, prod) for resource naming."
      type        = string
      default     = "lab-dev"
    }

    variable "budget_limit_amount" {
      description = "The monetary amount for the budget limit (as a string)."
      type        = string
      default     = "50.00" # Example: $50.00
    }

    variable "notification_email" {
      description = "The email address to send budget notifications and SNS subscription confirmation."
      type        = string
      # User must provide this value.
      # Example: default = "your-test-email@example.com"
    }

    variable "target_iam_role_name_for_action" {
      description = "The name of an IAM Role to which the restrictive policy will be applied by the budget action."
      type        = string
      default     = "LabBudgetActionTargetRole" # This role will be created if create_example_target_role is true.
    }

    variable "create_example_target_role" {
      description = "Set to true to create an example IAM role as the target for the budget action. If false, 'target_iam_role_name_for_action' must be an existing role."
      type        = bool
      default     = true
    }

    variable "owner_tag" {
      description = "Value for the Owner tag applied to resources."
      type        = string
      default     = "FinOpsAdmin@example.com"
    }
    ```

3.  **`outputs.tf`**:
    ```terraform
    # outputs.tf

    output "advanced_budget_id" {
      description = "The ID of the created AWS Budget with an action."
      value       = aws_budgets_budget.monthly_cost_budget_advanced.id
    }

    output "budget_action_execution_role_arn" {
      description = "ARN of the IAM Role that AWS Budgets will assume to execute actions."
      value       = aws_iam_role.budget_action_execution_role.arn
    }

    output "restrictive_iam_policy_arn_for_action" {
      description = "ARN of the restrictive IAM policy created to be applied by the budget action."
      value       = aws_iam_policy.restrictive_policy_for_budget_action.arn
    }

    output "sns_topic_arn_for_budget_alerts" {
      description = "ARN of the SNS topic used for budget notifications and action alerts."
      value       = aws_sns_topic.budget_alerts_sns_topic.arn
    }

    output "target_iam_role_name" {
      description = "The name of the IAM role targeted by the budget action."
      value       = var.target_iam_role_name_for_action
    }

    output "target_iam_role_arn_if_created" {
      description = "ARN of the example target IAM role if it was created by this lab."
      value       = var.create_example_target_role ? aws_iam_role.example_target_role_for_action[0].arn : "Not created by this lab configuration."
    }
    ```

### Hướng dẫn từng bước

1.  **Chuẩn bị IAM Role Mục tiêu (Nếu `create_example_target_role = false`)**:
    *   Nếu bạn đặt `var.create_example_target_role` thành `false` trong `variables.tf` (hoặc tệp `.tfvars`), bạn **phải** đảm bảo rằng IAM role được chỉ định bởi `var.target_iam_role_name_for_action` đã tồn tại trong tài khoản AWS của bạn. Budget Action cần một IAM principal (user, group, hoặc role) hiện có để áp dụng policy.

2.  **Tạo thư mục dự án và các tệp `.tf`**:
    *   Tạo một thư mục có tên `lab5-advanced-budgets`.
    *   Bên trong thư mục này, tạo các tệp `main.tf`, `variables.tf`, và `outputs.tf` với nội dung như trên.
    *   **Quan trọng:** Mở tệp `variables.tf` (hoặc tạo một tệp `terraform.tfvars`) và cung cấp một giá trị hợp lệ cho biến `notification_email`.

3.  **Chạy `terraform init`**:
    *   Mở terminal, điều hướng đến thư mục `lab5-advanced-budgets`.
    *   Chạy:
        ```bash
        terraform init
        ```

4.  **Chạy `terraform plan`**:
    *   Xem kế hoạch thực thi. Cung cấp email nếu chưa đặt default:
        ```bash
        terraform plan -var="notification_email=your-actual-email@example.com"
        ```
    *   Terraform sẽ hiển thị các tài nguyên sẽ được tạo: `aws_budgets_budget` với khối `action` và `notification`, các `aws_iam_policy`, `aws_iam_role` (bao gồm cả role mục tiêu nếu `create_example_target_role` là `true`), và `aws_sns_topic` với subscription.

5.  **Chạy `terraform apply`**:
    *   Áp dụng các thay đổi:
        ```bash
        terraform apply -var="notification_email=your-actual-email@example.com" -auto-approve
        ```
    *   Terraform sẽ tạo tất cả các tài nguyên.

6.  **Xác minh trong AWS Management Console**:
    *   **Xác nhận SNS Subscription:** Kiểm tra hộp thư email của bạn (địa chỉ `notification_email`) để tìm thư xác nhận từ AWS SNS. Nhấp vào liên kết để xác nhận subscription cho SNS topic. Nếu không có, kiểm tra thư mục spam/junk.
    *   **Budget:**
        *   Điều hướng đến **AWS Cost Management > Budgets**.
        *   Tìm budget (ví dụ: `MonthlyAdvBudget-lab-dev`).
        *   Nhấp vào budget. Kiểm tra tab **Details**.
        *   Chuyển sang tab **Alerts** (hoặc Notifications). Bạn sẽ thấy thông báo được cấu hình để gửi đến SNS topic khi đạt 85% chi phí thực tế.
        *   Chuyển sang tab **Actions**. Bạn sẽ thấy một action được cấu hình:
            *   **Action type:** `Apply IAM policy`.
            *   **Trigger:** 95% of actual budgeted amount.
            *   **IAM policy to apply:** ARN của `BudgetAction_DenyEC2Creation-${var.environment}`.
            *   **Target:** IAM role bạn đã chỉ định (ví dụ: `LabBudgetActionTargetRole`).
            *   **Execution role:** ARN của `AWSBudgetsActionExecutionRole-${var.aws_region}-${var.environment}`.
            *   **Approval model:** `AUTOMATIC`.
    *   **IAM:**
        *   Điều hướng đến **IAM > Policies**. Tìm policy `BudgetAction_DenyEC2Creation-${var.environment}`. Xem nội dung JSON của nó. Tìm policy `BudgetActionExecutionPermissions-${var.environment}`.
        *   Điều hướng đến **IAM > Roles**. Tìm role `AWSBudgetsActionExecutionRole-${var.aws_region}-${var.environment}`. Kiểm tra tab **Permissions** và **Trust relationships**.
        *   Nếu `create_example_target_role = true`, tìm role `LabBudgetActionTargetRole` (hoặc tên bạn đặt).
    *   **SNS:**
        *   Điều hướng đến **Simple Notification Service (SNS) > Topics**. Tìm topic (ví dụ: `budget-alerts-sns-topic-lab-dev`).
        *   Kiểm tra tab **Subscriptions**. Bạn sẽ thấy email của mình được liệt kê với trạng thái "Confirmed" (sau khi bạn nhấp vào liên kết xác nhận).

7.  **(Tùy chọn) Kiểm thử Budget Action (Khó trong môi trường lab ngắn hạn với chi phí thực):**
    *   Để action thực sự được kích hoạt, chi phí trong tài khoản AWS của bạn cần vượt qua ngưỡng 95% của budget. Điều này có thể không khả thi hoặc không mong muốn trong một lab ngắn.
    *   Nếu action được kích hoạt, IAM policy `BudgetAction_DenyEC2Creation-${var.environment}` sẽ được đính kèm vào role mục tiêu. Bất kỳ thực thể nào đảm nhận role đó sau đó sẽ không thể thực hiện `ec2:RunInstances`.
    *   Bạn có thể xem lịch sử thực thi action trong tab "Actions" của budget trong console. AWS cũng sẽ gửi thông báo qua SNS khi action được thực hiện.

### Giải thích & Lý do

*   **Tại sao sử dụng Budget Actions?**
    *   **Kiểm soát Chi phí Chủ động và Tự động:** Budget Actions cho phép bạn tự động thực hiện các biện pháp cụ thể để hạn chế chi tiêu khi budget sắp hoặc đã bị vượt, thay vì chỉ dựa vào cảnh báo và can thiệp thủ công.
    *   **Giảm thiểu Rủi ro Chi phí Leo thang:** Bằng cách tự động áp dụng các policy hạn chế (ví dụ: deny tạo tài nguyên mới, chuyển sang quyền read-only cho một số dịch vụ), bạn có thể ngăn chặn hoặc làm chậm lại việc chi phí tăng đột biến không kiểm soát.
    *   **Thực thi Chính sách FinOps:** Tự động hóa việc tuân thủ các giới hạn chi tiêu đã được phê duyệt và các quy tắc quản trị chi phí của tổ chức.

*   **Cấu hình `action` trong `aws_budgets_budget`**:
    *   `action_type = "APPLY_IAM_POLICY"`: Trong lab này, chúng ta tập trung vào việc áp dụng IAM policy.
    *   `approval_model = "AUTOMATIC"`: Hành động được thực hiện ngay khi ngưỡng đạt. `MANUAL` yêu cầu phê duyệt trong console. `AUTOMATIC` phù hợp cho các biện pháp kiểm soát tức thời.
    *   `execution_role_arn`: ARN của IAM role mà Budgets sẽ đảm nhận. Role này **phải** có quyền để thực hiện hành động được chỉ định.
    *   `definition { iam_action_definition { ... } }`: Chứa cấu hình cụ thể cho việc áp dụng IAM policy, bao gồm `policy_arn` của policy sẽ được áp dụng và `roles`, `users`, hoặc `groups` là mục tiêu.

*   **IAM Role cho Budget Action (`budget_action_execution_role`):**
    *   **Trust Policy:** Phải tin cậy principal dịch vụ `budgets.amazonaws.com` để cho phép dịch vụ Budgets đảm nhận role này. Việc thêm điều kiện `aws:SourceAccount` và `aws:SourceArn` tăng cường bảo mật.
    *   **Permissions Policy (`budget_execution_role_permissions_policy`):** Phải cấp các quyền cần thiết để thực hiện hành động. Đối với `APPLY_IAM_POLICY`, role này cần quyền `iam:AttachRolePolicy` và `iam:DetachRolePolicy` đối với IAM policy cụ thể (`restrictive_policy_for_budget_action`) mà nó sẽ áp dụng. **Tuân thủ Nguyên tắc Đặc quyền Tối thiểu (Least Privilege)** là rất quan trọng: chỉ cấp các quyền thực sự cần thiết cho action đó.

*   **Thông báo Budget qua SNS:**
    *   Sử dụng SNS topic cho `notification` (cảnh báo budget) và `action.subscriber_sns_topic_arns` (thông báo về action) mang lại sự linh hoạt:
        *   Gửi thông báo đến nhiều loại điểm cuối (email, Lambda, SQS, HTTP/S).
        *   Cho phép tùy chỉnh xử lý thông báo (ví dụ, kích hoạt một Lambda function để thực hiện các hành động phức tạp hơn).
        *   Tạo một kênh thông tin tập trung và nhất quán về trạng thái budget và các hành động được thực hiện.

*   **Phương pháp Tốt nhất cho năm 2025:**
    *   **Tự động hóa Phản ứng với Ngưỡng Chi phí:** Sử dụng Budget Actions để tự động hóa các biện pháp kiểm soát chi phí, giảm sự phụ thuộc vào can thiệp thủ công sau khi nhận cảnh báo.
    *   **Chính sách Hạn chế Theo Ngữ cảnh và Mục tiêu Rõ ràng:** Thiết kế các IAM policies hoặc SCPs được áp dụng bởi Budget Actions một cách cẩn thận. Chúng nên hạn chế chi tiêu không cần thiết hoặc rủi ro mà không làm gián đoạn các hoạt động kinh doanh quan trọng. Xác định rõ ràng các roles/users/groups mục tiêu.
    *   **Kiểm tra và Xác thực Actions (Testability):** Nếu có thể, hãy kiểm thử các budget actions trong môi trường non-production để đảm bảo chúng hoạt động như mong đợi và không gây ra tác dụng phụ không mong muốn.
    *   **Thông báo Minh bạch và Kịp thời:** Đảm bảo rằng các bên liên quan (ví dụ: chủ sở hữu tài nguyên, nhóm FinOps) được thông báo khi một budget action được kích hoạt hoặc cần phê duyệt (nếu dùng `MANUAL`).

### Xác minh
1.  Kiểm tra email của bạn và **xác nhận SNS subscription**.
2.  Trong **AWS Cost Management > Budgets**:
    *   Xác minh budget (ví dụ: `MonthlyAdvBudget-lab-dev`) tồn tại.
    *   Kiểm tra tab **Alerts** cho thông báo SNS ở 85%.
    *   Kiểm tra tab **Actions** cho hành động `APPLY_IAM_POLICY` ở 95%, nhắm vào role và policy chính xác, với execution role và approval model đúng.
3.  Trong **IAM > Policies**:
    *   Xác minh policy `BudgetAction_DenyEC2Creation-${var.environment}` tồn tại và có nội dung deny `ec2:RunInstances`.
    *   Xác minh policy `BudgetActionExecutionPermissions-${var.environment}` tồn tại và cấp quyền cần thiết cho execution role.
4.  Trong **IAM > Roles**:
    *   Xác minh role `AWSBudgetsActionExecutionRole-${var.aws_region}-${var.environment}` tồn tại với trust policy và permissions policy chính xác.
    *   Nếu `var.create_example_target_role` là `true`, xác minh role `var.target_iam_role_name_for_action` (ví dụ: `LabBudgetActionTargetRole`) tồn tại.
5.  Trong **SNS > Topics**:
    *   Xác minh topic (ví dụ: `budget-alerts-sns-topic-lab-dev`) tồn tại.
    *   Kiểm tra subscription đã được xác nhận.

### Dọn dẹp
1.  **Chạy `terraform destroy`**:
    *   Trong terminal, tại thư mục `lab5-advanced-budgets`, chạy lệnh sau (cung cấp email nếu cần):
        ```bash
        terraform destroy -var="notification_email=your-actual-email@example.com" -auto-approve
        ```
    *   Terraform sẽ xóa budget, IAM policies, IAM roles (bao gồm cả role mục tiêu nếu được tạo bởi lab này), SNS topic và subscription.

2.  **Xác minh việc xóa trong AWS Management Console**:
    *   Kiểm tra các dịch vụ Budgets, IAM (Policies, Roles), và SNS để xác nhận các tài nguyên đã được xóa.

---
Lưu ý:
*   Đã làm rõ hơn vai trò của từng IAM policy và execution role.
*   Thêm tùy chọn `create_example_target_role` để lab linh hoạt hơn trong việc quản lý role mục tiêu.
*   Nhấn mạnh việc xác nhận SNS subscription.
*   Cập nhật tên tài nguyên để bao gồm `var.environment` nhằm tăng tính duy nhất và dễ quản lý.

*(Chúng ta sẽ tiếp tục với Lab 6, tập trung vào AWS Cost Anomaly Detection.)*

---

## Lab 6: Thiết lập AWS Cost Anomaly Detection với Terraform

### Mục tiêu
*   Hiểu cách **AWS Cost Anomaly Detection** sử dụng machine learning để tự động xác định các kiểu chi tiêu bất thường và giảm thiểu các bất ngờ về chi phí.
*   Học cách tạo **Anomaly Monitors** để theo dõi chi phí AWS (toàn bộ tài khoản, theo dịch vụ cụ thể, cost category, hoặc tag).
*   Học cách tạo **Anomaly Subscriptions** để nhận thông báo (ví dụ: qua email hoặc SNS) khi phát hiện bất thường.
*   Sử dụng Terraform để quản lý các cấu hình này.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường.
*   Quyền IAM để tạo và quản lý Cost Anomaly Detection monitors và subscriptions (`ce:CreateAnomalyMonitor`, `ce:DeleteAnomalyMonitor`, `ce:GetAnomalyMonitors`, `ce:CreateAnomalySubscription`, `ce:DeleteAnomalySubscription`, `ce:GetAnomalySubscriptions`, `ce:UpdateAnomalySubscription`).
*   Quyền IAM để tạo và quản lý SNS topics (`sns:*`) nếu sử dụng SNS cho thông báo.
*   Địa chỉ email hợp lệ để nhận thông báo.
*   (Tùy chọn) Đã có Cost Categories (từ Lab 3) hoặc Cost Allocation Tags (từ Lab 2) được kích hoạt nếu bạn muốn tạo monitor dựa trên chúng.

### Khái niệm được đề cập
*   **AWS Cost Anomaly Detection:** Một dịch vụ sử dụng machine learning để liên tục theo dõi chi phí và mức sử dụng của bạn, xác định các mẫu chi tiêu bất thường và cung cấp cảnh báo.
*   **Anomaly Monitor:** Cấu hình xác định phạm vi dữ liệu chi phí mà Cost Anomaly Detection sẽ theo dõi.
    *   **Monitor Types:**
        *   `AWS_SERVICES`: Theo dõi chi phí cho một hoặc nhiều dịch vụ AWS cụ thể.
        *   `LINKED_ACCOUNT`: Theo dõi chi phí cho toàn bộ tài khoản hoặc các tài khoản được liên kết cụ thể.
        *   `COST_CATEGORY`: Theo dõi chi phí cho một Cost Category cụ thể.
        *   `TAGS`: Theo dõi chi phí cho các tài nguyên có một key tag cụ thể (giá trị tag không được chỉ định ở cấp độ monitor, nhưng có thể được lọc trong phân tích).
    *   **Monitor Dimension Values:** Các giá trị cụ thể cho loại monitor (ví dụ: tên dịch vụ, ID tài khoản, tên Cost Category, key của tag).
*   **Anomaly Subscription:** Cấu hình xác định cách bạn nhận thông báo khi một monitor phát hiện ra một bất thường.
    *   **Frequency:** Tần suất nhận thông báo (ví dụ: `DAILY`, `WEEKLY`, `IMMEDIATE`).
    *   **Threshold:** Ngưỡng giá trị tuyệt đối (ví dụ: $100) hoặc tỷ lệ phần trăm (ví dụ: 50% so với chi tiêu dự kiến) mà một bất thường phải vượt qua để kích hoạt thông báo.
    *   **Subscribers:** Điểm cuối nhận thông báo (ví dụ: địa chỉ email, ARN của SNS topic).
*   **Machine Learning Model:** Cost Anomaly Detection xây dựng một mô hình dựa trên dữ liệu lịch sử chi tiêu của bạn (thường cần ít nhất 10 ngày dữ liệu để mô hình ban đầu được huấn luyện hiệu quả).

### Cấu hình Terraform

Tạo một thư mục mới cho lab này, ví dụ `lab6-cost-anomaly-detection`. Bên trong thư mục đó, tạo các tệp sau:

1.  **`main.tf`**:
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
      region = var.aws_region # Cost Anomaly Detection is a regional service in terms of API endpoints
    }

    # --- SNS Topic for Anomaly Notifications (Recommended) ---
    resource "aws_sns_topic" "anomaly_notifications_topic" {
      name = "cost-anomaly-alerts-${var.environment}"
      tags = {
        ManagedBy = "Terraform"
      }
    }

    resource "aws_sns_topic_subscription" "anomaly_email_subscription" {
      count     = var.notification_email != "" ? 1 : 0 # Only create if email is provided
      topic_arn = aws_sns_topic.anomaly_notifications_topic.arn
      protocol  = "email"
      endpoint  = var.notification_email
    }

    # --- Anomaly Monitor for All AWS Services (Account Wide) ---
    resource "aws_cost_anomaly_detection_monitor" "all_services_monitor" {
      name                = "AllServicesMonitor-${var.environment}"
      monitor_type        = "LINKED_ACCOUNT" # Monitors all costs for the account
      # monitor_dimension = "SERVICE" # This would be used if monitor_type was AWS_SERVICES
      # For LINKED_ACCOUNT, no dimension key/value is typically needed unless specifying specific linked accounts.
      # If you have an AWS Organization and want to monitor specific member accounts:
      # monitor_dimension_values = ["111122223333", "444455556666"]

      tags = {
        ManagedBy   = "Terraform"
        Environment = var.environment
        Owner       = var.owner_tag
      }
    }

    # --- Anomaly Monitor for a Specific Service (e.g., Amazon EC2) ---
    resource "aws_cost_anomaly_detection_monitor" "ec2_service_monitor" {
      name                = "EC2ServiceMonitor-${var.environment}"
      monitor_type        = "AWS_SERVICES"
      monitor_dimension_values = ["Amazon Elastic Compute Cloud - Compute"] # Use the exact service name as in Cost Explorer

      tags = {
        ManagedBy   = "Terraform"
        Environment = var.environment
        Owner       = var.owner_tag
      }
    }

    # --- Anomaly Monitor for a Specific Cost Category (if you have one from Lab 3) ---
    # Ensure the cost category 'BusinessUnits' (or your chosen name) exists.
    # This is conditional based on whether you want to create it.
    resource "aws_cost_anomaly_detection_monitor" "cost_category_monitor" {
      count = var.enable_cost_category_monitor ? 1 : 0

      name                = "CostCategoryMonitor-${var.cost_category_to_monitor}-${var.environment}"
      monitor_type        = "COST_CATEGORY"
      monitor_dimension_values = [var.cost_category_to_monitor] # e.g., "BusinessUnits"

      tags = {
        ManagedBy   = "Terraform"
        Environment = var.environment
        Owner       = var.owner_tag
      }
    }


    # --- Anomaly Subscription for the "All Services Monitor" ---
    resource "aws_cost_anomaly_detection_subscription" "all_services_subscription" {
      name                    = "AllServicesAnomalySubscription-${var.environment}"
      monitor_arn_list        = [aws_cost_anomaly_detection_monitor.all_services_monitor.arn]
      frequency               = var.subscription_frequency # DAILY, WEEKLY, IMMEDIATE
      threshold_expression {
        # Send notification if anomaly total impact is > $50
        absolute_value = 50
      }
      # Or, use percentage-based threshold (cannot use both absolute and percentage in the same block)
      # threshold_expression {
      #   dimension {
      #     key = "TOTAL_IMPACT_PERCENTAGE"
      #     match_options = ["GREATER_THAN_OR_EQUAL_TO"]
      #     values = ["50"] # 50%
      #   }
      # }


      subscriber {
        type    = "SNS"
        address = aws_sns_topic.anomaly_notifications_topic.arn
      }
      # Can also add direct email subscribers:
      # subscriber {
      #   type    = "EMAIL"
      #   address = var.notification_email
      # }

      tags = {
        ManagedBy   = "Terraform"
        Environment = var.environment
        Owner       = var.owner_tag
      }
      depends_on = [aws_sns_topic_subscription.anomaly_email_subscription]
    }

    # --- Anomaly Subscription for the "EC2 Service Monitor" ---
    resource "aws_cost_anomaly_detection_subscription" "ec2_service_subscription" {
      name                 = "EC2ServiceAnomalySubscription-${var.environment}"
      monitor_arn_list     = [aws_cost_anomaly_detection_monitor.ec2_service_monitor.arn]
      frequency            = "DAILY"
      threshold_expression {
        absolute_value = 20 # Lower threshold for a specific service
      }

      subscriber {
        type    = "SNS"
        address = aws_sns_topic.anomaly_notifications_topic.arn
      }
      tags = {
        ManagedBy   = "Terraform"
        Environment = var.environment
        Owner       = var.owner_tag
      }
      depends_on = [aws_sns_topic_subscription.anomaly_email_subscription]
    }
    ```

2.  **`variables.tf`**:
    ```terraform
    # variables.tf

    variable "aws_region" {
      description = "The AWS region."
      type        = string
      default     = "us-east-1"
    }

    variable "environment" {
      description = "An environment tag, e.g., dev, test, prod."
      type        = string
      default     = "dev"
    }

    variable "notification_email" {
      description = "The email address to send anomaly notifications and SNS subscription confirmation. Leave empty to skip email subscription."
      type        = string
      default     = "" # User should provide this if they want email notifications
    }

    variable "enable_cost_category_monitor" {
      description = "Set to true to create an anomaly monitor for a specific cost category."
      type        = bool
      default     = false # Set to true if you have a cost category from Lab 3 you want to monitor
    }

    variable "cost_category_to_monitor" {
      description = "The name of the Cost Category to monitor (e.g., 'BusinessUnits' from Lab 3). Only used if enable_cost_category_monitor is true."
      type        = string
      default     = "BusinessUnits" # Ensure this cost category exists if enabled
    }

    variable "subscription_frequency" {
      description = "Frequency for anomaly notifications (DAILY, WEEKLY, IMMEDIATE)."
      type        = string
      default     = "DAILY"
      validation {
        condition     = contains(["DAILY", "WEEKLY", "IMMEDIATE"], var.subscription_frequency)
        error_message = "Allowed values for subscription_frequency are DAILY, WEEKLY, or IMMEDIATE."
      }
    }

    variable "owner_tag" {
      description = "Value for the Owner tag."
      type        = string
      default     = "student@example.com"
    }
    ```

3.  **`outputs.tf`**:
    ```terraform
    # outputs.tf

    output "all_services_monitor_arn" {
      description = "ARN of the Anomaly Monitor for all services."
      value       = aws_cost_anomaly_detection_monitor.all_services_monitor.arn
    }

    output "ec2_service_monitor_arn" {
      description = "ARN of the Anomaly Monitor for EC2 service."
      value       = aws_cost_anomaly_detection_monitor.ec2_service_monitor.arn
    }

    output "cost_category_monitor_arn" {
      description = "ARN of the Anomaly Monitor for the specified Cost Category (if created)."
      value       = var.enable_cost_category_monitor ? aws_cost_anomaly_detection_monitor.cost_category_monitor[0].arn : "Not Created"
    }

    output "all_services_subscription_arn" {
      description = "ARN of the Anomaly Subscription for all services monitor."
      value       = aws_cost_anomaly_detection_subscription.all_services_subscription.arn
    }

    output "sns_topic_arn_for_anomaly_notifications" {
      description = "ARN of the SNS topic for anomaly notifications."
      value       = aws_sns_topic.anomaly_notifications_topic.arn
    }
    ```

### Hướng dẫn từng bước

1.  **Tạo thư mục dự án và các tệp `.tf`**:
    *   Tạo một thư mục có tên `lab6-cost-anomaly-detection`.
    *   Bên trong thư mục này, tạo các tệp `main.tf`, `variables.tf`, và `outputs.tf` với nội dung như trên.
    *   **Quan trọng:**
        *   Mở tệp `variables.tf`. Nếu bạn muốn nhận thông báo qua email, hãy cung cấp một giá trị hợp lệ cho `notification_email`.
        *   Nếu bạn muốn tạo monitor cho một Cost Category (ví dụ: từ Lab 3), hãy đặt `enable_cost_category_monitor = true` và đảm bảo `cost_category_to_monitor` khớp với tên Cost Category hiện có của bạn.

2.  **Chạy `terraform init`**:
    *   Mở terminal, điều hướng đến thư mục `lab6-cost-anomaly-detection`.
    *   Chạy:
        ```bash
        terraform init
        ```

3.  **Chạy `terraform plan`**:
    *   Xem kế hoạch thực thi:
        ```bash
        terraform plan -var="notification_email=your-email@example.com"
        ```
        (Thay thế bằng email của bạn nếu bạn muốn nhận thông báo).
    *   Terraform sẽ hiển thị rằng nó sẽ tạo các resource `aws_cost_anomaly_detection_monitor` và `aws_cost_anomaly_detection_subscription`, cùng với SNS topic (nếu email được cung cấp).

4.  **Chạy `terraform apply`**:
    *   Áp dụng các thay đổi:
        ```bash
        terraform apply -var="notification_email=your-email@example.com" -auto-approve
        ```
    *   Terraform sẽ tạo tất cả các tài nguyên.

5.  **Xác minh trong AWS Management Console**:
    *   **Xác nhận SNS Subscription (nếu có):** Nếu bạn cung cấp email, hãy kiểm tra hộp thư của bạn để tìm thư xác nhận từ AWS SNS. Nhấp vào liên kết để xác nhận subscription.
    *   **Cost Anomaly Detection:**
        *   Điều hướng đến **AWS Cost Management > Anomaly Detection**.
        *   **Monitors Tab:** Bạn sẽ thấy các monitors đã tạo (`AllServicesMonitor-dev`, `EC2ServiceMonitor-dev`, và `CostCategoryMonitor-...-dev` nếu được kích hoạt). Nhấp vào một monitor để xem chi tiết cấu hình của nó.
            *   Lưu ý rằng ban đầu, monitor có thể ở trạng thái "Pending" hoặc "Evaluating" khi nó thu thập dữ liệu và huấn luyện mô hình. Có thể mất vài giờ hoặc đến một ngày để có đủ dữ liệu cho việc phát hiện bất thường ban đầu.
        *   **Alert subscriptions Tab:** Bạn sẽ thấy các subscriptions đã tạo (`AllServicesAnomalySubscription-dev`, `EC2ServiceAnomalySubscription-dev`). Nhấp vào một subscription để xem chi tiết, bao gồm các monitors nó được liên kết, tần suất, ngưỡng, và người đăng ký (SNS topic).
    *   **SNS (nếu có):**
        *   Điều hướng đến **Simple Notification Service (SNS) > Topics**. Tìm topic `cost-anomaly-alerts-dev`.
        *   Kiểm tra tab **Subscriptions**. Bạn sẽ thấy email của mình được liệt kê với trạng thái "Confirmed" (sau khi xác nhận).

6.  **(Chờ đợi phát hiện bất thường - Phụ thuộc vào dữ liệu chi phí của bạn):**
    *   Cost Anomaly Detection cần thời gian để tìm hiểu các mẫu chi tiêu bình thường của bạn. Việc phát hiện bất thường thực sự phụ thuộc vào việc có sự thay đổi đáng kể và bất ngờ trong chi phí của bạn hay không.
    *   Nếu một bất thường được phát hiện (và nó vượt qua ngưỡng subscription), bạn sẽ nhận được thông báo qua SNS (và email nếu đã đăng ký).
    *   Bạn cũng có thể xem các bất thường đã phát hiện trong tab **Anomalies** của trang Cost Anomaly Detection trong console.

### Giải thích & Lý do

*   **Tại sao sử dụng Cost Anomaly Detection?**
    *   **Phát hiện sớm các chi phí không mong muốn:** Giúp bạn nhanh chóng xác định và giải quyết các vấn đề như tài nguyên bị rò rỉ, lỗi cấu hình gây tăng chi phí, hoặc hoạt động gian lận tiềm ẩn.
    *   **Giảm thiểu "bill shock":** Bằng cách nhận cảnh báo về các xu hướng chi tiêu bất thường, bạn có thể hành động trước khi nhận được hóa đơn cuối tháng quá cao.
    *   **Bổ sung cho Budgets:** Trong khi Budgets theo dõi chi phí so với một ngưỡng cố định, Anomaly Detection tìm kiếm các thay đổi *so với hành vi bình thường được dự kiến*, ngay cả khi chi phí tổng thể vẫn nằm trong budget.
    *   **Sử dụng Machine Learning:** Tự động điều chỉnh theo các mẫu chi tiêu thay đổi theo thời gian, giảm nhu cầu phải liên tục cập nhật các ngưỡng cảnh báo thủ công.

*   **Resource `aws_cost_anomaly_detection_monitor`**:
    *   `name`: Tên bạn đặt cho monitor.
    *   `monitor_type`: Xác định phạm vi theo dõi.
        *   `LINKED_ACCOUNT`: Theo dõi toàn bộ tài khoản (hoặc các tài khoản con cụ thể trong một Org). Đây là điểm khởi đầu tốt.
        *   `AWS_SERVICES`: Hữu ích để theo dõi các dịch vụ cụ thể có xu hướng biến động hoặc có chi phí cao.
        *   `COST_CATEGORY`: Cho phép bạn theo dõi các nhóm chi phí logic mà bạn đã định nghĩa.
        *   `TAGS`: Theo dõi chi phí cho các tài nguyên được gắn thẻ với một key cụ thể.
    *   `monitor_dimension_values`: Các giá trị cho `monitor_type` (ví dụ: tên dịch vụ, tên cost category).

*   **Resource `aws_cost_anomaly_detection_subscription`**:
    *   `name`: Tên bạn đặt cho subscription.
    *   `monitor_arn_list`: Danh sách ARN của một hoặc nhiều monitors mà subscription này áp dụng.
    *   `frequency`:
        *   `IMMEDIATE`: Gửi thông báo ngay khi một bất thường được xác nhận (có thể gây nhiều thông báo).
        *   `DAILY`: Gửi một bản tóm tắt hàng ngày về các bất thường. **Thường là lựa chọn tốt nhất.**
        *   `WEEKLY`: Gửi một bản tóm tắt hàng tuần.
    *   `threshold_expression`: Xác định mức độ nghiêm trọng của một bất thường để kích hoạt thông báo. Bạn có thể sử dụng:
        *   `absolute_value`: Một giá trị tiền tệ cố định (ví dụ: $50).
        *   `dimension` với `key = "TOTAL_IMPACT_PERCENTAGE"`: Một tỷ lệ phần trăm của tổng chi phí dự kiến (ví dụ: 50%).
        Chỉ có thể sử dụng một trong hai loại ngưỡng này trong một khối `threshold_expression`.
    *   `subscriber {}`: Xác định người nhận.
        *   `type = "SNS"`: Gửi đến một SNS topic (khuyến nghị để có sự linh hoạt).
        *   `type = "EMAIL"`: Gửi trực tiếp đến một địa chỉ email.

*   **Thời gian huấn luyện mô hình:**
    *   Cost Anomaly Detection cần dữ liệu lịch sử để xây dựng mô hình cơ sở về chi tiêu bình thường. AWS khuyến nghị ít nhất 10 ngày dữ liệu, nhưng càng nhiều càng tốt (lên đến vài tháng) để mô hình chính xác hơn. Các bất thường có thể không được phát hiện ngay lập tức sau khi tạo monitor.

*   **Phương pháp Tốt nhất cho năm 2025:**
    *   **Kích hoạt Anomaly Detection cho toàn bộ tài khoản:** Bắt đầu với một monitor `LINKED_ACCOUNT` và một subscription `DAILY` làm cơ sở.
    *   **Tạo monitors cụ thể cho các dịch vụ hoặc ứng dụng quan trọng:** Đối với các dịch vụ có chi phí cao hoặc dễ biến động, hãy tạo các monitors riêng với ngưỡng phù hợp.
    *   **Sử dụng SNS cho thông báo:** Cho phép tích hợp với các hệ thống cảnh báo khác, gửi đến nhiều người nhận, hoặc kích hoạt các hành động tự động (ví dụ: Lambda).
    *   **Định kỳ xem xét các bất thường:** Ngay cả khi không có thông báo, hãy kiểm tra bảng điều khiển Anomaly Detection để hiểu các mẫu chi tiêu và điều chỉnh các monitor/subscription nếu cần.

### Xác minh
1.  Nếu đã cung cấp email, hãy kiểm tra và **xác nhận SNS subscription**.
2.  Trong **AWS Cost Management > Anomaly Detection**:
    *   **Monitors Tab:** Xác minh các monitors (`AllServicesMonitor-dev`, `EC2ServiceMonitor-dev`, etc.) tồn tại.
    *   **Alert subscriptions Tab:** Xác minh các subscriptions tồn tại, được liên kết với đúng monitors, và có cấu hình tần suất, ngưỡng, người đăng ký chính xác.
3.  Trong **SNS > Topics** (nếu có):
    *   Xác minh topic `cost-anomaly-alerts-dev` tồn tại và subscription đã được xác nhận.
4.  **(Chờ đợi)** Theo dõi tab **Anomalies** trong những ngày tới để xem có bất thường nào được phát hiện không (phụ thuộc vào dữ liệu chi phí của bạn).

### Dọn dẹp
1.  **Chạy `terraform destroy`**:
    *   Trong terminal, tại thư mục `lab6-cost-anomaly-detection`, chạy lệnh sau:
        ```bash
        terraform destroy -var="notification_email=your-email@example.com" -auto-approve
        ```
    *   Terraform sẽ xóa các anomaly monitors, anomaly subscriptions, và SNS topic (cùng với subscription của nó).

2.  **Xác minh việc xóa trong AWS Management Console**:
    *   Kiểm tra các trang Anomaly Detection (Monitors, Alert subscriptions) và SNS để xác nhận các tài nguyên đã được xóa.

---
*(Chúng ta sẽ tiếp tục với Lab 7, thảo luận về chiến lược quản lý chi phí đa tài khoản.)*

## Lab 7: Tổng quan về Chiến lược Quản lý Chi phí Đa Tài khoản (Multi-Account) với AWS Organizations

### Mục tiêu
*   Hiểu rõ các lợi ích và thách thức cốt lõi của việc quản lý chi phí trong môi trường **AWS Organizations** đa tài khoản.
*   Nắm vững khái niệm **Consolidated Billing** (Thanh toán Hợp nhất) và cách nó ảnh hưởng đến việc theo dõi, phân bổ chi phí, cũng như việc tận dụng các ưu đãi về giá.
*   Thảo luận về cách triển khai và sử dụng hiệu quả **AWS Budgets** và **Cost Categories** trong bối cảnh đa tài khoản và các Organizational Units (OUs).
*   Giới thiệu vai trò then chốt của **Service Control Policies (SCPs)** trong việc thiết lập các biện pháp kiểm soát chi phí phòng ngừa (preventive cost controls) ở cấp độ tổ chức.
*   Phân tích cách Terraform có thể hỗ trợ tự động hóa và quản lý các cấu hình quản lý chi phí này ở quy mô lớn, đảm bảo tính nhất quán và tuân thủ.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường.
*   Hiểu biết cơ bản về các khái niệm của AWS Organizations: tài khoản quản lý (management account), tài khoản thành viên (member accounts), và Organizational Units (OUs).
*   (Tùy chọn) Có quyền truy cập (tối thiểu là read-only) vào một môi trường AWS Organizations để tham khảo và liên hệ thực tế.

### Khái niệm được đề cập
*   **AWS Organizations:** Dịch vụ cho phép bạn quản lý tập trung và điều chỉnh nhiều tài khoản AWS thuộc sở hữu của bạn. Nó là nền tảng cho việc quản trị đa tài khoản hiệu quả.
    *   **Management Account (Tài khoản Quản lý):** Tài khoản gốc tạo ra tổ chức. Đây là tài khoản duy nhất có thể thanh toán cho tất cả các tài khoản thành viên và là nơi quản lý các chính sách cấp tổ chức như SCPs. **Không nên chạy khối lượng công việc sản xuất trong tài khoản quản lý.**
    *   **Member Accounts (Tài khoản Thành viên):** Các tài khoản AWS tiêu chuẩn được mời và trở thành một phần của tổ chức. Chúng được sử dụng để cô lập khối lượng công việc, môi trường, hoặc đơn vị kinh doanh.
    *   **Organizational Units (OUs):** Các đơn vị tổ chức logic, cho phép bạn nhóm các tài khoản lại với nhau (ví dụ: theo phòng ban, môi trường dev/test/prod) và áp dụng các chính sách (như SCPs) hoặc Budgets một cách nhất quán cho cả nhóm.
*   **Consolidated Billing (Thanh toán Hợp nhất):** Một tính năng mặc định và cốt lõi của AWS Organizations. Tất cả chi phí từ các tài khoản thành viên được tổng hợp và thanh toán thông qua hóa đơn duy nhất của tài khoản quản lý.
    *   **Lợi ích:** Một hóa đơn tổng hợp, khả năng chia sẻ giảm giá theo khối lượng sử dụng (volume discounts) trên toàn tổ chức (ví dụ: cho S3, Data Transfer Out), và tiềm năng chia sẻ hiệu quả các cam kết chiết khấu như Reserved Instances (RIs) và Savings Plans.
    *   **Thách thức:** Cần có cơ chế và công cụ để phân bổ chi phí trở lại cho các tài khoản thành viên hoặc đơn vị kinh doanh một cách chính xác và công bằng (quy trình showback/chargeback).
*   **Phân bổ và Hiển thị Chi phí trong Organizations:**
    *   **Cost Explorer trong Tài khoản Quản lý:** Cung cấp cái nhìn tổng thể về chi phí của toàn bộ tổ chức. Có thể lọc và nhóm chi phí theo tài khoản thành viên, OU, dịch vụ, tag, v.v.
    *   **Cost Allocation Tags và Cost Categories:** Vẫn là công cụ cực kỳ quan trọng. Tags cần được áp dụng nhất quán trong các tài khoản thành viên và sau đó được kích hoạt trong tài khoản quản lý để phân tích. Cost Categories có thể được định nghĩa trong tài khoản quản lý để nhóm chi phí từ nhiều tài khoản theo logic kinh doanh phức tạp.
*   **AWS Budgets trong Môi trường Organizations:**
    *   Budgets có thể được tạo trong tài khoản quản lý để theo dõi chi phí của toàn bộ tổ chức, các OU cụ thể, hoặc các tài khoản thành viên cụ thể.
    *   Budgets cũng có thể được tạo trực tiếp trong tài khoản thành viên để các nhóm tự quản lý chi phí của mình (với quyền IAM phù hợp).
*   **Service Control Policies (SCPs):**
    *   Một loại chính sách của AWS Organizations dùng để quản lý các quyền tối đa có sẵn cho các IAM principals (users và roles) trong các tài khoản thành viên của một tổ chức hoặc OU.
    *   **Kiểm soát Chi phí Phòng ngừa (Preventive Cost Controls):** SCPs có thể được sử dụng để hạn chế việc tạo ra các loại tài nguyên đắt tiền, hạn chế việc sử dụng các region không được phê duyệt, hoặc yêu cầu tuân thủ các tiêu chuẩn quản trị (ví dụ: không cho phép tắt CloudTrail).
    *   SCPs **không cấp quyền**; chúng hoạt động như một "bộ lọc" hoặc "lan can bảo vệ", chỉ định các hành động API tối đa được phép. Các quyền thực tế vẫn phải được cấp thông qua IAM policies trong tài khoản thành viên, và các quyền đó không thể vượt quá những gì SCP cho phép.
    *   Ví dụ SCP: Ngăn chặn việc khởi chạy các instance type EC2 rất lớn, cấm sử dụng các dịch vụ không cần thiết hoặc chưa được phê duyệt.
*   **Chia sẻ Chi phí (Cost Sharing) và Phân bổ Chi phí Dùng chung:** Các cơ chế và chiến lược để phân bổ chi phí của các tài nguyên dùng chung (ví dụ: tài khoản network tập trung, tài khoản công cụ bảo mật, chi phí hỗ trợ doanh nghiệp, chi phí CUR/Athena) cho các đơn vị kinh doanh hoặc tài khoản tiêu thụ chúng. Cost Categories với split charge rules (Lab 3) là một công cụ hữu ích cho việc này.

### Thảo luận và Cấu hình Khái niệm (Tập trung vào vai trò của Terraform)

Việc thiết lập và quản lý toàn bộ một AWS Organization bằng Terraform là một chủ đề nâng cao, thường bao gồm các giải pháp như AWS Control Tower hoặc các framework tùy chỉnh. Tuy nhiên, Terraform đóng vai trò quan trọng trong việc quản lý các khía cạnh liên quan đến chi phí *bên trong* một tổ chức hiện có, đặc biệt là khi được thực thi từ tài khoản quản lý.

#### 1. Consolidated Billing và Tăng cường Hiển thị Chi phí với Terraform

*   **Consolidated Billing:** Là một tính năng tự động của Organizations, không được Terraform trực tiếp quản lý.
*   **Vai trò của Terraform trong việc tăng cường hiển thị:**
    *   **AWS Cost and Usage Reports (CUR) trong Tài khoản Quản lý:** Như đã thực hành trong Lab 4, việc thiết lập CUR trong tài khoản quản lý là bước đầu tiên và quan trọng nhất. CUR này sẽ chứa dữ liệu chi phí chi tiết từ *tất cả* các tài khoản thành viên, với các cột như `lineItem/UsageAccountId` và `lineItem/AvailabilityZone` giúp xác định nguồn gốc chi phí. Terraform đảm bảo CUR được cấu hình đúng và nhất quán.
    *   **Kích hoạt Cost Allocation Tags Tập trung:** Terraform có thể quản lý việc kích hoạt các user-defined tags trong tài khoản quản lý (sử dụng `aws_cost_allocation_tag`). Điều này cho phép các tag được áp dụng trong tài khoản thành viên (có thể được thực thi bằng SCPs hoặc AWS Config) trở nên hữu ích cho việc lọc và nhóm chi phí trên toàn tổ chức trong Cost Explorer và CUR.
    *   **Định nghĩa Cost Categories Tập trung:** Như trong Lab 3, Cost Categories có thể được định nghĩa trong tài khoản quản lý bằng Terraform (`aws_cost_category_definition`) để nhóm chi phí từ nhiều tài khoản hoặc OUs theo các quy tắc logic kinh doanh, cung cấp một cái nhìn chi phí phù hợp hơn cho các bên liên quan.

#### 2. AWS Budgets trong Môi trường Đa Tài khoản với Terraform

*   **Tạo Budgets Tập trung từ Tài khoản Quản lý:**
    *   Sử dụng resource `aws_budgets_budget` của Terraform, khi thực thi trong tài khoản quản lý, bạn có thể tạo budget cho:
        *   **Toàn bộ Tổ chức:** Bằng cách không chỉ định bộ lọc tài khoản hoặc OU trong `cost_filters`.
        *   **Organizational Units (OUs) Cụ thể:** Sử dụng `cost_filters` với dimension `LINKED_OU` và cung cấp ARN hoặc ID của OU.
            ```terraform
            # Terraform trong tài khoản quản lý: Budget cho một OU cụ thể
            data "aws_caller_identity" "current" {} # Để lấy account_id của tài khoản quản lý

            resource "aws_budgets_budget" "ou_development_budget" {
              name         = "DevelopmentOU-MonthlyBudget"
              budget_type  = "COST"
              limit_amount = "5000.00"
              limit_unit   = "USD"
              time_unit    = "MONTHLY"
              account_id   = data.aws_caller_identity.current.account_id # Chỉ định tài khoản quản lý

              cost_filters = {
                # Thay thế bằng ARN hoặc ID của OU Development của bạn
                "LINKED_OU" = ["ou-orgid-devouid"] 
              }
              # ... notifications, actions ...
            }
            ```
        *   **Tài khoản Thành viên Cụ thể:** Sử dụng `cost_filters` với dimension `LINKED_ACCOUNT` và cung cấp danh sách ID tài khoản thành viên.
*   **Budget Actions Cấp Tổ chức:**
    *   Khi một budget được tạo trong tài khoản quản lý và nhắm vào một OU hoặc tài khoản thành viên, các Budget Actions có thể được cấu hình. Đặc biệt, action `APPLY_SCP_POLICY` trở nên rất mạnh mẽ.
        ```terraform
        # Terraform trong tài khoản quản lý: Budget Action để áp dụng SCP
        # (Giả sử budget 'ou_development_budget' và SCP 'scp_restrict_expensive_services' đã được định nghĩa)
        # action {
        #   action_type        = "APPLY_SCP_POLICY"
        #   approval_model     = "AUTOMATIC"
        #   execution_role_arn = aws_iam_role.org_budget_action_execution_role.arn # Role trong tài khoản quản lý
        #   notification_type  = "ACTUAL"
        #   action_threshold { action_threshold_type = "PERCENTAGE"; action_threshold_value = 90; }
        #   definition {
        #     scp_action_definition {
        #       policy_id = aws_organizations_policy.scp_restrict_expensive_services.id
        #       target_ids = ["ou-orgid-devouid"] # OU mục tiêu
        #     }
        #   }
        #   # ... subscribers ...
        # }
        ```
    *   IAM role thực thi (`execution_role_arn`) cho các action cấp tổ chức này phải tồn tại trong tài khoản quản lý và có các quyền cần thiết để quản lý SCPs và các tài nguyên liên quan (ví dụ: `organizations:AttachPolicy`, `organizations:DetachPolicy`, `organizations:DescribePolicy`).

#### 3. Service Control Policies (SCPs) để Kiểm soát Chi phí Phòng ngừa với Terraform

*   **Nguyên tắc Hoạt động của SCPs:** SCPs là các "lan can bảo vệ" (guardrails) chứ không phải là cơ chế cấp quyền. Chúng xác định các hành động API tối đa mà một IAM principal (user hoặc role) trong tài khoản thành viên có thể thực hiện. Nếu một hành động bị SCP từ chối, thì ngay cả khi IAM policy trong tài khoản thành viên cho phép hành động đó, nó vẫn sẽ bị chặn.
*   **Ví dụ về SCPs Hữu ích cho Kiểm soát Chi phí:**
    *   **Hạn chế Sử dụng Regions:** Chỉ cho phép các hoạt động trong các AWS regions đã được phê duyệt để tránh chi phí phát sinh ở các region không mong muốn.
    *   **Hạn chế Loại Instance EC2 Đắt Tiền:** Ngăn chặn việc khởi chạy các instance type rất lớn, chuyên dụng (GPU, ML), hoặc có chi phí cao nếu chúng không phù hợp với nhu cầu của một OU/tài khoản cụ thể.
    *   **Cấm Sử dụng các Dịch vụ Không Cần thiết hoặc Rủi ro Cao:** Chặn quyền truy cập vào các dịch vụ không nằm trong danh mục được phê duyệt của tổ chức.
    *   **Yêu cầu Gắn thẻ (Enforce Tagging - Mức độ hạn chế):** SCPs có thể được sử dụng để yêu cầu một số tag nhất định khi tạo tài nguyên, mặc dù việc này có thể phức tạp và thường được bổ sung bằng AWS Config Rules.
*   **Quản lý SCPs bằng Terraform:**
    *   Sử dụng các resource như `aws_organizations_policy` để định nghĩa nội dung của SCP (thường từ một tệp JSON) và `aws_organizations_policy_attachment` để đính kèm SCP đó vào root của tổ chức, một OU, hoặc một tài khoản cụ thể.
        ```terraform
        # Terraform trong tài khoản quản lý: Định nghĩa và đính kèm SCP
        resource "aws_organizations_policy" "restrict_regions_scp" {
          name        = "RestrictToApprovedRegions"
          description = "Allows operations only in specified AWS regions."
          content     = templatefile("${path.module}/scp_templates/restrict_regions.json.tpl", {
            approved_regions = ["us-east-1", "eu-west-2"]
          })
          type        = "SERVICE_CONTROL_POLICY"
        }

        resource "aws_organizations_policy_attachment" "attach_restrict_regions_to_sandbox_ou" {
          policy_id = aws_organizations_policy.restrict_regions_scp.id
          target_id = "ou-orgid-sandboxouid" # ID của OU Sandbox
        }
        ```
    *   Việc quản lý SCPs bằng Terraform cho phép kiểm soát phiên bản, đánh giá (code review), và triển khai các chính sách kiểm soát chi phí một cách nhất quán và tự động trên toàn bộ tổ chức.

#### 4. Quản lý Chi phí Tập trung và Vai trò của Terraform

*   **Cấu hình Provider Terraform:** Khi làm việc với các tài nguyên cấp tổ chức, Terraform provider cần được cấu hình để hoạt động trong ngữ cảnh của tài khoản quản lý, thường bằng cách đảm nhận một IAM role trong tài khoản đó có đủ quyền (ví dụ: `organizations:*`, `budgets:*`, `ce:*`, `s3:*` cho CUR, `iam:*` cho roles của budget action).
*   **Kho Lưu trữ Mã Tập trung (Centralized Repository):** Các cấu hình Terraform cho CUR, Cost Categories, Budgets cấp tổ chức/OU, và SCPs nên được quản lý từ một hoặc một vài kho lưu trữ mã nguồn (ví dụ: Git repository) tập trung, phản ánh chiến lược quản trị đám mây của tổ chức.
*   **Lợi ích của Terraform:** Đảm bảo tính nhất quán, khả năng lặp lại, kiểm toán được, và giảm thiểu lỗi thủ công khi triển khai các cấu hình quản lý chi phí phức tạp này trên quy mô lớn.

### Giải thích & Lý do

*   **Tại sao cần chiến lược đa tài khoản cho quản lý chi phí hiệu quả?**
    *   **Cô lập Rủi ro và Thanh toán:** Phân tách khối lượng công việc, môi trường (dev/test/prod), hoặc các đơn vị kinh doanh thành các tài khoản riêng biệt giúp cải thiện bảo mật, giới hạn phạm vi ảnh hưởng của sự cố, và đơn giản hóa việc theo dõi và phân bổ chi phí.
    *   **Linh hoạt và Khả năng Mở rộng:** Dễ dàng thêm hoặc loại bỏ tài khoản khi tổ chức phát triển hoặc tái cấu trúc.
    *   **Tuân thủ và Quản trị:** Áp dụng các chính sách quản trị và tuân thủ khác nhau cho các nhóm tài khoản khác nhau.
*   **Thách thức của quản lý chi phí đa tài khoản cần giải quyết:**
    *   **Thiếu Hiển thị Tổng thể:** Cần có một cái nhìn hợp nhất về chi phí trên tất cả các tài khoản.
    *   **Khó khăn trong Phân bổ Chính xác:** Phân bổ chi phí dùng chung và chi phí của từng tài khoản cho các chủ sở hữu hoặc trung tâm chi phí tương ứng.
    *   **Thiếu Kiểm soát Nhất quán:** Đảm bảo các biện pháp kiểm soát chi phí được áp dụng đồng đều và hiệu quả trên nhiều tài khoản.
*   **Tầm quan trọng của Tài khoản Quản lý (Management Account):**
    *   Là trung tâm đầu não cho quản trị tổ chức, thanh toán, và bảo mật. **Tuyệt đối không nên chạy khối lượng công việc ứng dụng trong tài khoản này.**
    *   Là nơi lý tưởng để thiết lập CUR toàn tổ chức, định nghĩa Cost Categories, tạo Budgets cấp tổ chức/OU, và quản lý SCPs.
*   **SCPs như một Công cụ Phòng ngừa Chi phí Chiến lược:**
    *   SCPs cho phép bạn thực hiện "shift left" trong việc kiểm soát chi phí, tức là ngăn chặn các hành động có thể dẫn đến chi phí không mong muốn ngay từ khi chúng được thử thực hiện, thay vì chỉ phát hiện và phản ứng sau khi chi phí đã phát sinh.
    *   Chúng là một thành phần không thể thiếu của một chiến lược quản trị đám mây (cloud governance) và FinOps trưởng thành.

### Phương pháp Tốt nhất cho năm 2025 trong Môi trường Đa Tài khoản
*   **Thiết kế Cấu trúc OU Thông minh:** Cấu trúc OUs của bạn theo logic kinh doanh, vòng đời ứng dụng, yêu cầu bảo mật, hoặc các yêu cầu tuân thủ để dễ dàng áp dụng các chính sách (SCPs, Tag Policies) và Budgets một cách có mục tiêu.
*   **CUR Tập trung và Duy nhất:** Luôn có một CUR toàn diện, chi tiết (hourly, Parquet, bao gồm resource IDs) được định nghĩa trong tài khoản quản lý và là nguồn dữ liệu chi phí chính thức.
*   **Chiến lược Gắn thẻ (Tagging) Nhất quán và Được Thực thi Toàn Tổ chức:** Sử dụng Tag Policies của AWS Organizations để thực thi các tiêu chuẩn gắn thẻ và kích hoạt các tag quan trọng trong tài khoản quản lý.
*   **Sử dụng SCPs một cách Cẩn trọng và Có Chủ đích:** Bắt đầu với các SCPs rộng rãi nhưng ít gây ảnh hưởng (ví dụ: hạn chế region) và sau đó tinh chỉnh dần với các SCPs cụ thể hơn. Luôn kiểm thử SCPs kỹ lưỡng trong một OU thử nghiệm để tránh vô tình chặn các hoạt động kinh doanh hợp lệ.
*   **Budgets ở Nhiều Cấp độ và Phạm vi:** Thiết lập budgets ở cấp độ tổ chức, OU, và khuyến khích/cho phép tài khoản thành viên tự tạo budget cho phạm vi của họ để có nhiều lớp giám sát và tăng cường trách nhiệm.
*   **Tự động hóa Tối đa với Terraform (IaC):** Quản lý tất cả các cấu hình này (CUR, Cost Categories, Budgets, SCPs, Tag Policies) bằng Terraform từ tài khoản quản lý để đảm bảo tính nhất quán, kiểm soát phiên bản, khả năng kiểm toán, và giảm thiểu rủi ro do cấu hình thủ công.
*   **Thiết lập Quy trình Đánh giá và Tối ưu hóa Định kỳ:** Chi phí và cấu trúc tổ chức luôn thay đổi. Hãy có quy trình để định kỳ xem xét hiệu quả của các biện pháp kiểm soát chi phí và tối ưu hóa chúng.

### Xác minh
*   Lab này chủ yếu mang tính lý thuyết và thảo luận. Việc xác minh sẽ liên quan đến việc bạn hiểu rõ các khái niệm, cách chúng tương tác với nhau, và vai trò của Terraform.
*   Nếu bạn có quyền truy cập vào một môi trường AWS Organizations:
    *   Khám phá Cost Explorer trong tài khoản quản lý. Thử lọc theo tài khoản thành viên hoặc OU.
    *   Xem xét các SCPs hiện có (nếu có) trong phần AWS Organizations > Policies và cách chúng được áp dụng cho các OUs/tài khoản.
    *   Kiểm tra các budget được định nghĩa trong tài khoản quản lý và phạm vi của chúng.

### Dọn dẹp
*   Không có tài nguyên AWS nào được tạo trực tiếp bằng Terraform trong phần chính của lab này (ngoại trừ các đoạn mã Terraform ví dụ mang tính khái niệm). Do đó, không có bước dọn dẹp Terraform cụ thể.
*   Nếu bạn đã thử nghiệm tạo SCPs hoặc các cấu hình khác thủ công trong môi trường của mình dựa trên các ví dụ, hãy xem xét việc dọn dẹp chúng nếu cần thiết để tránh ảnh hưởng không mong muốn.

---
Lưu ý:
*   Đã nhấn mạnh hơn vai trò của tài khoản quản lý và việc không chạy workload sản xuất trên đó.
*   Làm rõ hơn về cách SCPs hoạt động (bộ lọc quyền, không cấp quyền).
*   Bổ sung thêm ví dụ về SCPs và cách Terraform có thể quản lý chúng với `templatefile`.
*   Tăng cường các "Phương pháp Tốt nhất" với các điểm cụ thể hơn về Tag Policies và quy trình đánh giá.

*(Chúng ta sẽ tiếp tục với Lab 8, giới thiệu về các công cụ tối ưu hóa chi phí AWS và vai trò của Terraform.)*
## Lab 8: Giới thiệu về Công cụ Tối ưu hóa Chi phí AWS và Vai trò của Terraform

### Mục tiêu
*   Giới thiệu các công cụ và dịch vụ chính của AWS giúp xác định các cơ hội tối ưu hóa chi phí, bao gồm **AWS Cost Explorer (RI & Savings Plans recommendations)**, **AWS Compute Optimizer**, và **AWS Trusted Advisor**.
*   Thảo luận về các loại đề xuất tối ưu hóa chi phí phổ biến (ví dụ: rightsizing, loại bỏ tài nguyên không sử dụng, mua Reserved Instances/Savings Plans).
*   Khám phá cách Terraform có thể được sử dụng để tự động hóa việc triển khai một số thay đổi dựa trên các đề xuất này (mang tính khái niệm và ví dụ đơn giản).
*   Nhấn mạnh tầm quan trọng của việc phân tích cẩn thận trước khi tự động hóa các thay đổi tối ưu hóa.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường.
*   Có một số tài nguyên đang chạy trong tài khoản AWS của bạn (ví dụ: EC2 instances, EBS volumes) để các công cụ tối ưu hóa có thể có dữ liệu để phân tích. Dữ liệu lịch sử càng nhiều, đề xuất càng chính xác.
*   Quyền IAM để truy cập AWS Cost Explorer, AWS Compute Optimizer, và AWS Trusted Advisor (thường là một phần của các quyền rộng hơn hoặc quyền read-only cho các dịch vụ này).

### Khái niệm được đề cập
*   **Tối ưu hóa Chi phí (Cost Optimization):** Một trong năm trụ cột của AWS Well-Architected Framework. Bao gồm quá trình liên tục tinh chỉnh và cải thiện việc sử dụng tài nguyên để đạt được kết quả kinh doanh tốt nhất với chi phí thấp nhất có thể.
*   **AWS Cost Explorer - Recommendations:**
    *   **Reserved Instance (RI) Recommendations:** Đề xuất mua RIs cho các dịch vụ như EC2, RDS, Redshift, ElastiCache, OpenSearch dựa trên lịch sử sử dụng ổn định của bạn.
    *   **Savings Plans Recommendations:** Đề xuất mua Savings Plans (Compute Savings Plans, EC2 Instance Savings Plans, SageMaker Savings Plans) dựa trên tổng chi tiêu điện toán của bạn.
*   **AWS Compute Optimizer:**
    *   Sử dụng machine learning để phân tích dữ liệu cấu hình và sử dụng của các tài nguyên tính toán (EC2 instances, Auto Scaling groups, EBS volumes, Lambda functions, ECS services trên Fargate) và đưa ra đề xuất rightsizing.
    *   **Finding Types:** Over-provisioned (quá lớn), Under-provisioned (quá nhỏ), Optimized.
    *   Cung cấp đến 3 tùy chọn rightsizing cho mỗi tài nguyên, cùng với ước tính rủi ro hiệu suất và tiết kiệm chi phí.
*   **AWS Trusted Advisor:**
    *   Cung cấp các đề xuất theo thời gian thực để giúp bạn tuân theo các phương pháp tốt nhất của AWS.
    *   **Cost Optimization Checks:** Bao gồm các kiểm tra như "Idle Load Balancers," "Underutilized Amazon EC2 Instances," "Amazon RDS Idle DB Instances," "Unassociated Elastic IP Addresses."
    *   Một số kiểm tra chỉ có sẵn với Business hoặc Enterprise Support plan.
*   **Rightsizing:** Điều chỉnh kích thước của tài nguyên (ví dụ: instance type của EC2) để phù hợp hơn với nhu cầu thực tế về hiệu suất và dung lượng, tránh lãng phí do cung cấp quá mức (over-provisioning).
*   **Idle Resources:** Các tài nguyên đã được cung cấp nhưng không còn được sử dụng (ví dụ: EBS volumes không được đính kèm, Elastic IPs không được liên kết).
*   **Automation of Optimization:** Sử dụng các công cụ như Terraform để tự động áp dụng các thay đổi được đề xuất. Điều này cần được thực hiện một cách cẩn thận.

### Khám phá các Công cụ Tối ưu hóa (Không có Mã Terraform Trực tiếp cho Việc Đọc Đề xuất)

Phần này tập trung vào việc khám phá các công cụ này trong AWS Management Console. Terraform không trực tiếp "đọc" các đề xuất này trong phiên bản hiện tại của AWS provider. Thay vào đó, Terraform có thể được sử dụng để *hành động* dựa trên các đề xuất mà bạn đã xem xét và quyết định áp dụng.

1.  **AWS Cost Explorer - RI & Savings Plans Recommendations:**
    *   Điều hướng đến **AWS Cost Management > Cost Explorer**.
    *   Trong menu bên trái, tìm **Recommendations** (hoặc **Savings Plans > Recommendations** và **Reservations > Recommendations**).
    *   **Savings Plans Recommendations:**
        *   Xem xét các thông số: Recommendation period (1-year, 3-year), Payment option (All Upfront, Partial Upfront, No Upfront).
        *   Cost Explorer sẽ hiển thị mức cam kết hàng giờ được đề xuất và ước tính tiết kiệm hàng tháng.
    *   **RI Recommendations:**
        *   Chọn dịch vụ (ví dụ: EC2).
        *   Xem xét các thông số tương tự.
    *   **Lý do:** Các đề xuất này dựa trên việc bạn có mức sử dụng ổn định mà có thể được cam kết để đổi lấy giá thấp hơn.

2.  **AWS Compute Optimizer:**
    *   Điều hướng đến **AWS Compute Optimizer**.
    *   Nếu đây là lần đầu tiên bạn sử dụng, bạn có thể cần phải **Opt in**. Quá trình phân tích ban đầu có thể mất đến 12-24 giờ (đôi khi lâu hơn tùy thuộc vào số lượng tài nguyên).
    *   Sau khi phân tích hoàn tất, bạn sẽ thấy một **Dashboard** tóm tắt các cơ hội tối ưu hóa.
    *   Khám phá các đề xuất cho:
        *   **EC2 instances:** Xem các instance được đánh dấu là "Over-provisioned." Nhấp vào một instance để xem các tùy chọn rightsizing được đề xuất, bao gồm instance type mới, chi phí ước tính, và mức độ rủi ro về hiệu suất.
        *   **Auto Scaling groups:** Đề xuất thay đổi instance type trong launch template/configuration.
        *   **EBS volumes:** Đề xuất thay đổi loại volume (ví dụ: từ gp2 sang gp3) hoặc kích thước.
        *   **Lambda functions:** Đề xuất điều chỉnh cấu hình bộ nhớ.
    *   **Lý do:** Compute Optimizer giúp bạn tìm ra các tài nguyên không được sử dụng hiệu quả, nơi bạn có thể giảm chi phí mà không ảnh hưởng (hoặc thậm chí cải thiện) hiệu suất.

3.  **AWS Trusted Advisor:**
    *   Điều hướng đến **AWS Trusted Advisor**.
    *   Trong menu bên trái, nhấp vào **Cost Optimization**.
    *   Xem lại các kiểm tra được liệt kê. Một số kiểm tra phổ biến bao gồm:
        *   **Idle Load Balancers:** Các ELB không có instance backend nào được đăng ký.
        *   **Underutilized Amazon EC2 Instances:** Các EC2 instance có mức sử dụng CPU thấp và lưu lượng mạng thấp.
        *   **Amazon RDS Idle DB Instances:** Các instance RDS không có kết nối trong một khoảng thời gian.
        *   **Unassociated Elastic IP Addresses:** Các địa chỉ EIP không được liên kết với bất kỳ tài nguyên nào (vẫn bị tính phí).
        *   **Amazon EBS Idle Volumes:** Các volume EBS ở trạng thái "available" (không được đính kèm).
    *   Nhấp vào một kiểm tra để xem danh sách các tài nguyên bị ảnh hưởng.
    *   **Lý do:** Trusted Advisor giúp bạn tìm thấy các "quả táo chín muộn" (low-hanging fruit) – các tài nguyên lãng phí rõ ràng có thể được loại bỏ hoặc điều chỉnh.

### Vai trò của Terraform trong việc Tự động hóa Tối ưu hóa

Mặc dù Terraform không tự động đọc đề xuất từ các công cụ trên, nó có thể được sử dụng để **triển khai các thay đổi** sau khi bạn đã phân tích và phê duyệt các đề xuất đó.

**Quan trọng: Tự động hóa việc tối ưu hóa chi phí cần được thực hiện một cách thận trọng. Luôn kiểm thử các thay đổi trong môi trường non-production trước và hiểu rõ tác động tiềm ẩn về hiệu suất và tính sẵn sàng.**

#### 1. Rightsizing EC2 Instances với Terraform (Dựa trên Đề xuất của Compute Optimizer)

Giả sử Compute Optimizer đề xuất thay đổi một EC2 instance từ `t3.large` sang `m5.large`.

*   **Quy trình thủ công:**
    1.  Xem đề xuất trong Compute Optimizer.
    2.  Phân tích rủi ro hiệu suất.
    3.  Quyết định áp dụng.
    4.  Cập nhật `instance_type` trong tệp `.tf` của bạn cho resource `aws_instance` tương ứng.
    5.  Chạy `terraform plan` và `terraform apply`.

*   **Mã Terraform (Ví dụ thay đổi `instance_type`):**

    ```terraform
    # main.tf

    resource "aws_instance" "my_app_server" {
      ami           = "ami-xxxxxxxxxxxxxxxxx" # Replace with your AMI
      # Old instance type:
      # instance_type = "t3.large"

      # New instance type based on Compute Optimizer recommendation (after manual review)
      instance_type = "m5.large" # Example: Changed after reviewing recommendation

      # ... other configurations (subnet, security groups, tags, etc.) ...

      tags = {
        Name      = "MyAppServer"
        ManagedBy = "Terraform"
      }
    }
    ```

*   **Tự động hóa có điều kiện (Nâng cao và Cần Cẩn Trọng):**
    *   Về mặt lý thuyết, bạn có thể xuất dữ liệu từ Compute Optimizer (ví dụ: qua API hoặc CSV export), xử lý nó bằng một script, và script đó có thể tự động cập nhật các tệp `.tfvars` hoặc tạo các thay đổi cấu hình Terraform.
    *   **Đây là một quy trình phức tạp và rủi ro nếu không được thiết kế và kiểm thử kỹ lưỡng.** Bạn cần cơ chế an toàn, quy trình phê duyệt, và khả năng rollback.
    *   Ví dụ, một script Python có thể đọc CSV export từ Compute Optimizer, tìm các instance được quản lý bởi Terraform (dựa trên tag), và nếu đề xuất đáp ứng các tiêu chí an toàn (ví dụ: rủi ro hiệu suất thấp, tiết kiệm đáng kể), nó có thể đề xuất thay đổi `instance_type` trong một tệp `terraform.tfvars`.

#### 2. Loại bỏ Tài nguyên Không sử dụng với Terraform (Dựa trên Đề xuất của Trusted Advisor)

Giả sử Trusted Advisor xác định một EBS volume không được đính kèm mà bạn đã xác nhận là không cần thiết nữa, và volume đó được quản lý bởi Terraform.

*   **Quy trình thủ công:**
    1.  Xem đề xuất trong Trusted Advisor.
    2.  Xác nhận volume không còn cần thiết.
    3.  Xóa hoặc comment out resource `aws_ebs_volume` trong tệp `.tf` của bạn.
    4.  Chạy `terraform plan` và `terraform apply`. Terraform sẽ thấy resource không còn trong cấu hình và sẽ xóa nó.

*   **Mã Terraform (Ví dụ xóa resource):**

    ```terraform
    # main.tf

    # resource "aws_ebs_volume" "unused_data_volume" {
    #   availability_zone = "us-east-1a"
    #   size              = 20
    #   tags = {
    #     Name = "OldDataVolume-ToBeDeleted"
    #     ManagedBy = "Terraform"
    #   }
    # }
    # After confirming it's unused and can be deleted, comment out or remove the block.
    # `terraform apply` will then destroy the resource.
    ```

#### 3. Mua Reserved Instances hoặc Savings Plans

*   Hiện tại, việc mua RIs và Savings Plans thường được thực hiện thông qua AWS Management Console hoặc AWS CLI/SDK.
*   Terraform provider của AWS có các resource để quản lý một số khía cạnh của RIs (ví dụ: `aws_ec2_capacity_reservation`) nhưng không phải là quy trình mua RI điển hình cho mục đích giảm giá.
*   Đối với Savings Plans, việc mua cũng chủ yếu qua Console/CLI.
*   **Vai trò của Terraform ở đây là gián tiếp:** Bằng cách quản lý cơ sở hạ tầng của bạn (ví dụ: EC2 instances) một cách nhất quán, Terraform cung cấp dữ liệu sử dụng ổn định mà Cost Explorer có thể sử dụng để đưa ra các đề xuất RI/SP chính xác.

### Giải thích & Lý do

*   **Tối ưu hóa là một quá trình liên tục:** Nhu cầu ứng dụng thay đổi, AWS giới thiệu các instance type mới, và các mẫu sử dụng phát triển. Bạn cần định kỳ xem xét các cơ hội tối ưu hóa.
*   **Phân tích trước khi hành động:**
    *   **Không phải tất cả các đề xuất đều phù hợp.** Một đề xuất rightsizing có thể có rủi ro hiệu suất không chấp nhận được đối với một ứng dụng quan trọng.
    *   **Hiểu rõ ứng dụng của bạn:** Biết các yêu cầu về CPU, memory, network, I/O của ứng dụng trước khi thay đổi instance type.
    *   **Kiểm thử:** Luôn kiểm thử các thay đổi rightsizing trong môi trường non-production.
*   **Lợi ích của việc sử dụng Terraform để áp dụng thay đổi:**
    *   **Khả năng lặp lại và Nhất quán:** Đảm bảo thay đổi được áp dụng đúng cách.
    *   **Kiểm soát Phiên bản:** Các thay đổi cấu hình (ví dụ: `instance_type`) được lưu trữ trong Git, cung cấp lịch sử và khả năng rollback.
    *   **Cơ sở hạ tầng dưới dạng Mã:** Ngay cả các thay đổi tối ưu hóa cũng là một phần của định nghĩa cơ sở hạ tầng của bạn.
*   **Hạn chế của Tự động hóa Hoàn toàn:**
    *   Việc diễn giải ngữ cảnh của một đề xuất (ví dụ: "underutilized" có thực sự có nghĩa là lãng phí không, hay nó đang chờ một công việc theo batch?) đòi hỏi sự thông minh của con người.
    *   Rủi ro của việc tự động áp dụng một thay đổi không chính xác có thể lớn (ví dụ: làm giảm hiệu suất của một ứng dụng sản xuất).

### Phương pháp Tốt nhất cho năm 2025
*   **Thiết lập quy trình xem xét tối ưu hóa định kỳ:** Dành thời gian hàng tuần hoặc hàng tháng để xem xét các đề xuất từ Cost Explorer, Compute Optimizer, và Trusted Advisor.
*   **Ưu tiên các "quả táo chín muộn":** Bắt đầu với việc loại bỏ các tài nguyên không sử dụng rõ ràng.
*   **Tiếp cận rightsizing một cách có hệ thống:**
    *   Xác định các ứng dụng ứng cử viên.
    *   Phân tích đề xuất của Compute Optimizer.
    *   Thảo luận với chủ sở hữu ứng dụng.
    *   Kiểm thử trong non-prod.
    *   Triển khai thay đổi bằng Terraform.
    *   Theo dõi hiệu suất sau khi thay đổi.
*   **Tận dụng Savings Plans và RIs:** Đối với khối lượng công việc ổn định, đây là một trong những cách dễ nhất để tiết kiệm chi phí đáng kể.
*   **Xây dựng văn hóa tối ưu hóa chi phí (FinOps):** Khuyến khích các nhóm kỹ thuật suy nghĩ về chi phí khi họ thiết kế và triển khai các dịch vụ.
*   **Xem xét "Tự động hóa có giám sát" (Human-in-the-loop automation):** Thay vì tự động hóa hoàn toàn, hãy sử dụng các script để phân tích đề xuất và *tạo ra* các thay đổi cấu hình Terraform (ví dụ: pull requests) cần được con người xem xét và phê duyệt trước khi áp dụng.

### Xác minh
*   Đăng nhập vào AWS Console và điều hướng đến:
    *   **Cost Explorer > Recommendations:** Xem có đề xuất nào cho RIs hoặc Savings Plans không.
    *   **Compute Optimizer:** Xem dashboard và các đề xuất (nếu có).
    *   **Trusted Advisor > Cost Optimization:** Xem lại các kiểm tra.
*   Nếu bạn đã thực hiện thay đổi `instance_type` trong một tệp Terraform ví dụ và áp dụng nó, hãy xác minh trong EC2 console rằng instance type đã được cập nhật.

### Dọn dẹp
*   Nếu bạn đã sửa đổi các tệp Terraform ví dụ để thay đổi `instance_type` hoặc xóa tài nguyên, bạn có thể muốn hoàn nguyên các thay đổi đó và chạy lại `terraform apply` để đưa tài nguyên trở lại trạng thái ban đầu (nếu đó là một phần của một lab khác hoặc một thiết lập hiện có).
*   Việc "opt out" khỏi Compute Optimizer có thể được thực hiện trong console của dịch vụ đó nếu bạn muốn dừng việc phân tích.
*   Không có tài nguyên nào được tạo mới bởi lab này mà cần `terraform destroy` trực tiếp, vì nó tập trung vào việc khám phá các công cụ và khái niệm.

---
*(Chúng ta sẽ tiếp tục với Lab 9, một lab "God Level" tập trung vào việc xây dựng nền tảng cho dashboard chi phí tùy chỉnh.)*

## Lab 9 (God Level): Xây dựng Nền tảng cho Dashboard Chi phí Tùy chỉnh với CUR, Athena và QuickSight

### Mục tiêu
*   Hiểu rõ tại sao **dashboard chi phí tùy chỉnh** lại quan trọng để có được những hiểu biết sâu sắc, phù hợp với nhu cầu kinh doanh cụ thể, và vượt ra ngoài khả năng của các công cụ báo cáo chi phí tiêu chuẩn như AWS Cost Explorer.
*   Đảm bảo **AWS Cost and Usage Report (CUR)** được cấu hình tối ưu (kế thừa từ Lab 4) để làm nguồn dữ liệu gốc, chi tiết và đáng tin cậy cho dashboard.
*   Thiết lập **Amazon Athena** để cho phép truy vấn dữ liệu CUR trực tiếp từ Amazon S3 bằng cách sử dụng SQL tiêu chuẩn, mở ra khả năng phân tích linh hoạt.
*   Giới thiệu quy trình kết nối **Amazon QuickSight** với Athena để trực quan hóa dữ liệu CUR và xây dựng các dashboard tương tác, dễ hiểu.
*   Sử dụng Terraform để tự động hóa việc thiết lập S3 bucket cho CUR, định nghĩa CUR, và cấu hình các thành phần của AWS Glue Data Catalog (database, crawler) và Amazon Athena (workgroup) cần thiết cho quy trình này.

### Điều kiện tiên quyết
*   Hoàn thành Lab 0: Thiết lập Môi trường và Lab 4: Thiết lập AWS Cost and Usage Reports (CUR) với Terraform.
*   **Quan trọng:** Bạn phải có một CUR đang hoạt động và đã gửi dữ liệu đến S3 bucket. Thông thường, cần đợi ít nhất 24-48 giờ sau khi tạo CUR để có dữ liệu đầu tiên. Lab này sẽ không hoạt động hiệu quả nếu chưa có dữ liệu CUR.
*   Quyền IAM để:
    *   Quản lý CUR (`cur:*`).
    *   Quản lý S3 buckets (`s3:*`).
    *   Quản lý AWS Glue Data Catalog (database, tables, crawlers: `glue:*`).
    *   Quản lý Amazon Athena (workgroups, queries: `athena:*`).
    *   (Tùy chọn, cho phần QuickSight) Quyền truy cập và sử dụng Amazon QuickSight. Điều này có thể yêu cầu đăng ký QuickSight và chọn phiên bản phù hợp (Standard hoặc Enterprise). QuickSight cũng cần quyền truy cập Athena và S3.
*   Kiến thức SQL cơ bản sẽ rất hữu ích cho phần làm việc với Athena.

### Khái niệm được đề cập
*   **Dashboard Chi phí Tùy chỉnh:** Các báo cáo trực quan được thiết kế riêng để đáp ứng nhu cầu thông tin cụ thể của tổ chức. Chúng hiển thị các chỉ số hiệu suất chính (KPIs) về chi phí, xu hướng chi tiêu theo các chiều tùy chỉnh (ví dụ: theo dòng sản phẩm, theo đơn vị kinh doanh được ánh xạ từ tags/cost categories), và các phân tích sâu khác mà Cost Explorer có thể không cung cấp trực tiếp hoặc không đủ linh hoạt.
*   **Amazon Athena:** Một dịch vụ truy vấn tương tác, serverless, giúp dễ dàng phân tích dữ liệu lớn được lưu trữ trong Amazon S3 bằng cách sử dụng SQL tiêu chuẩn. Bạn không cần quản lý cơ sở hạ tầng và chỉ trả tiền cho các truy vấn bạn chạy.
*   **AWS Glue Data Catalog:** Một kho metadata được quản lý đầy đủ, hoạt động như một sổ đăng ký trung tâm cho tất cả các tài sản dữ liệu của bạn. Athena sử dụng Data Catalog để lưu trữ và truy xuất thông tin về các database và table (bao gồm schema, vị trí dữ liệu trong S3, và các thuộc tính khác) mà nó truy vấn.
*   **AWS Glue Crawler:** Một chương trình tự động kết nối với kho dữ liệu (ví dụ: S3 bucket chứa CUR), quét qua dữ liệu để suy luận schema, và sau đó tạo hoặc cập nhật các định nghĩa table metadata tương ứng trong AWS Glue Data Catalog.
*   **Amazon QuickSight:** Một dịch vụ Business Intelligence (BI) dựa trên đám mây, có khả năng mở rộng, cho phép người dùng ở mọi cấp độ kỹ năng dễ dàng tạo và xuất bản các dashboard tương tác, giàu thông tin. QuickSight có thể kết nối với nhiều nguồn dữ liệu, bao gồm cả Amazon Athena.
*   **SPICE (Super-fast, Parallel, In-memory Calculation Engine):** Công cụ tính toán trong bộ nhớ (in-memory) mạnh mẽ của QuickSight, được thiết kế để tăng tốc độ thực hiện các truy vấn và trực quan hóa dữ liệu, mang lại trải nghiệm tương tác nhanh chóng cho người dùng cuối.

### Cấu hình Terraform

Tạo một thư mục mới cho lab này, ví dụ `lab9-custom-dashboards`. Bên trong thư mục đó, tạo các tệp sau:

1.  **`main.tf`**:
    ```terraform
    # main.tf

    terraform {
      required_providers {
        aws = {
          source  = "hashicorp/aws"
          version = "~> 5.0" # Ensure compatibility with latest features
        }
      }
    }

    provider "aws" {
      region = var.aws_region
    }

    data "aws_caller_identity" "current" {}
    data "aws_partition" "current" {}
    data "aws_region" "current_region" {}

    # --- S3 Bucket and CUR Definition (Building upon Lab 4 concepts) ---
    # Ensure you provide a globally unique bucket name for CUR storage.
    resource "aws_s3_bucket" "cur_data_storage_bucket" {
      bucket = var.cur_s3_bucket_name
      # Consider adding lifecycle policies for CUR data (e.g., transition to Glacier for older reports)
      # and server-side encryption for enhanced security.
    }

    resource "aws_s3_bucket_versioning" "cur_data_storage_bucket_versioning" {
      bucket = aws_s3_bucket.cur_data_storage_bucket.id
      versioning_configuration {
        status = "Enabled" # Recommended for CUR data
      }
    }

    # Bucket policy allowing CUR service to deliver reports.
    data "aws_iam_policy_document" "cur_bucket_delivery_policy_doc" {
      statement {
        sid    = "AllowCURPutObject"
        effect = "Allow"
        principals {
          type        = "Service"
          identifiers = ["billingreports.amazonaws.com"]
        }
        actions   = ["s3:PutObject"]
        resources = ["${aws_s3_bucket.cur_data_storage_bucket.arn}/${var.cur_s3_report_prefix}/*"]
        condition {
          test     = "StringEquals"
          variable = "aws:SourceAccount"
          values   = [data.aws_caller_identity.current.account_id]
        }
        condition {
          test     = "ArnLike"
          variable = "aws:SourceArn"
          values   = ["arn:${data.aws_partition.current.partition}:cur:${data.aws_region.current_region.name}:${data.aws_caller_identity.current.account_id}:definition/*"]
        }
      }
      statement {
        sid    = "AllowCURBucketValidation"
        effect = "Allow"
        principals {
          type        = "Service"
          identifiers = ["billingreports.amazonaws.com"]
        }
        actions   = ["s3:GetBucketAcl", "s3:GetBucketPolicy"] # Permissions CUR needs for validation
        resources = [aws_s3_bucket.cur_data_storage_bucket.arn]
        condition {
          test     = "StringEquals"
          variable = "aws:SourceAccount"
          values   = [data.aws_caller_identity.current.account_id]
        }
        condition {
          test     = "ArnLike"
          variable = "aws:SourceArn"
          values   = ["arn:${data.aws_partition.current.partition}:cur:${data.aws_region.current_region.name}:${data.aws_caller_identity.current.account_id}:definition/*"]
        }
      }
    }

    resource "aws_s3_bucket_policy" "cur_data_storage_bucket_policy" {
      bucket = aws_s3_bucket.cur_data_storage_bucket.id
      policy = data.aws_iam_policy_document.cur_bucket_delivery_policy_doc.json
    }

    # CUR Definition optimized for dashboarding.
    resource "aws_cur_report_definition" "dashboard_optimized_cur" {
      report_name                = var.cur_report_name
      time_unit                  = "HOURLY"    # Most granular data
      format                     = "Parquet"   # Essential for Athena performance and cost-effectiveness
      compression                = "Parquet"   # Matches format
      additional_schema_elements = ["RESOURCES", "SPLIT_COST_ALLOCATION_DATA"] # Crucial for detailed resource-level and allocated cost analysis
      s3_bucket                  = aws_s3_bucket.cur_data_storage_bucket.bucket
      s3_prefix                  = var.cur_s3_report_prefix
      s3_region                  = data.aws_region.current_region.name # Region of the S3 bucket
      additional_artifacts       = ["ATHENA", "QUICKSIGHT"] # Generates Athena manifest and QuickSight integration files
      refresh_closed_reports     = true        # Ensures data accuracy with late-arriving data
      report_versioning          = "CREATE_NEW_REPORT" # Safer option to avoid data loss

      tags = {
        ManagedBy   = "Terraform-Lab9"
        Purpose     = "CUR for Custom Dashboarding Pipeline"
        DataQuality = "High-Detail"
        Owner       = var.owner_tag
      }
      depends_on = [aws_s3_bucket_policy.cur_data_storage_bucket_policy]
    }

    # --- AWS Glue Data Catalog Database for CUR ---
    resource "aws_glue_catalog_database" "cur_data_analytics_database" {
      name        = var.glue_database_name
      description = "AWS Glue Data Catalog database for AWS Cost and Usage Reports (CUR) analytics."
      tags = {
        ManagedBy = "Terraform-Lab9"
        Purpose   = "CUR-Analytics"
      }
    }

    # --- IAM Role for AWS Glue Crawler ---
    resource "aws_iam_role" "cur_glue_crawler_execution_role" {
      name = "AWSGlueServiceRole-CURDataCrawler-${var.environment}"
      assume_role_policy = jsonencode({
        Version = "2012-10-17",
        Statement = [{
          Action    = "sts:AssumeRole",
          Effect    = "Allow",
          Principal = { Service = "glue.amazonaws.com" }
        }]
      })
      # AWS Managed policy providing baseline permissions for Glue service.
      managed_policy_arns = ["arn:${data.aws_partition.current.partition}:iam::aws:policy/service-role/AWSGlueServiceRole"]
      tags = {
        ManagedBy = "Terraform-Lab9"
      }
    }

    # Custom IAM policy granting the Glue Crawler read access to the CUR S3 bucket.
    data "aws_iam_policy_document" "glue_crawler_s3_read_policy_doc" {
      statement {
        effect = "Allow"
        actions = [
          "s3:GetObject", # To read CUR files
          "s3:ListBucket" # To list objects in the CUR bucket/prefix
        ]
        resources = [
          aws_s3_bucket.cur_data_storage_bucket.arn,                            # Bucket-level permission (for ListBucket)
          "${aws_s3_bucket.cur_data_storage_bucket.arn}/${var.cur_s3_report_prefix}/*", # Object-level permission for CUR data
          "${aws_s3_bucket.cur_data_storage_bucket.arn}/${var.cur_s3_report_prefix}/${var.cur_report_name}/*" # More specific path
        ]
      }
    }

    resource "aws_iam_policy" "glue_crawler_s3_read_policy" {
      name   = "GlueCrawlerCURS3ReadAccessPolicy-${var.environment}"
      policy = data.aws_iam_policy_document.glue_crawler_s3_read_policy_doc.json
      tags = {
        ManagedBy = "Terraform-Lab9"
      }
    }

    resource "aws_iam_role_policy_attachment" "glue_crawler_s3_read_policy_attachment" {
      role       = aws_iam_role.cur_glue_crawler_execution_role.name
      policy_arn = aws_iam_policy.glue_crawler_s3_read_policy.arn
    }

    # --- AWS Glue Crawler for CUR Data ---
    # This crawler will scan the CUR data in S3 and populate/update the Glue Data Catalog table.
    resource "aws_glue_crawler" "cur_data_discovery_crawler" {
      name          = "CUR-Data-Discovery-Crawler-${var.environment}"
      role          = aws_iam_role.cur_glue_crawler_execution_role.arn
      database_name = aws_glue_catalog_database.cur_data_analytics_database.name
      description   = "AWS Glue Crawler for discovering schema and partitions of CUR data in S3."

      s3_target {
        # The path should point to the root directory of your CUR report files for a specific report name.
        # CUR structure: s3_prefix/report_name/YYYYMMDD-YYYYMMDD/assemblyId/files.parquet
        # Crawler path: s3://<bucket-name>/<s3-prefix>/<report-name>/
        # The crawler will use the manifest files if present, or crawl the Parquet files directly.
        path    = "s3://${aws_s3_bucket.cur_data_storage_bucket.bucket}/${var.cur_s3_report_prefix}/${var.cur_report_name}/"
        # exclusions = ["**/temp/**", "**/*.tmp"] # Optional: Exclude temporary files or folders
      }

      # Configuration to handle schema evolution in CUR data.
      configuration = jsonencode({
        Version = 1.0,
        CrawlerOutput = {
          Partitions = { AddOrUpdateBehavior = "InheritFromTable" } # How to handle new partitions
        },
        Grouping = {
          TableGroupingPolicy = "CombineCompatibleSchemas" # Crucial for CUR as schema can evolve.
                                                          # This attempts to merge compatible schemas into a single table.
        }
      })

      # Optional: Define a schedule for the crawler (e.g., run daily after CUR delivery).
      # schedule = "cron(0 6 * * ? *)" # Example: Run daily at 06:00 UTC

      # Prefix for tables created by this crawler in the Glue Data Catalog.
      table_prefix = "cur_" # e.g., table created might be "cur_primarycurfordashboarding"

      tags = {
        ManagedBy   = "Terraform-Lab9"
        DataSource  = "CUR"
        Environment = var.environment
        Owner       = var.owner_tag
      }

      depends_on = [
        aws_cur_report_definition.dashboard_optimized_cur, # Ensure CUR is defined
        aws_iam_role_policy_attachment.glue_crawler_s3_read_policy_attachment # Ensure role has S3 access
      ]
    }

    # --- Amazon Athena Workgroup (Optional, but good practice) ---
    # Defines settings for query execution, like output location and encryption.
    resource "aws_athena_workgroup" "cur_data_analysis_workgroup" {
      name        = "CURDataAnalysisWorkgroup-${var.environment}"
      description = "Dedicated Amazon Athena workgroup for CUR analysis queries."

      configuration {
        result_configuration {
          # Store Athena query results in a sub-prefix of the CUR bucket for organization.
          output_location = "s3://${aws_s3_bucket.cur_data_storage_bucket.bucket}/athena-query-results/${var.environment}/"
          # encryption_configuration {
          #   encryption_option = "SSE_S3" # Or SSE_KMS, CSE_KMS for stronger encryption
          # }
        }
        enforce_workgroup_configuration    = true # Ensures users of this workgroup use these settings.
        publish_cloudwatch_metrics_enabled = true # For monitoring Athena query performance and costs.
      }
      tags = {
        ManagedBy   = "Terraform-Lab9"
        Purpose     = "Athena-CUR-Queries"
        Environment = var.environment
      }
      # Ensure S3 bucket for results exists.
      depends_on = [aws_s3_bucket.cur_data_storage_bucket]
    }
    ```

2.  **`variables.tf`**:
    ```terraform
    # variables.tf

    variable "aws_region" {
      description = "The AWS region for deploying all resources."
      type        = string
      default     = "us-east-1" # Ensure this region supports all services used (CUR, Glue, Athena, QuickSight).
    }

    variable "environment" {
      description = "An environment identifier (e.g., dev, test, prod) for resource naming and tagging."
      type        = string
      default     = "lab-dev"
    }

    variable "cur_s3_bucket_name" {
      description = "A globally unique name for the S3 bucket to store CUR reports and Athena query results. This MUST be provided and be unique."
      type        = string
      # Example: default = "my-org-cur-analytics-bucket-unique123"
      # No default provided to force user input for uniqueness.
    }

    variable "cur_s3_report_prefix" {
      description = "The prefix for the CUR report path within the S3 bucket (e.g., 'data/billing/cur'). No leading/trailing slashes."
      type        = string
      default     = "cur-reports-for-dashboard"
      validation {
        condition     = !can(regex("^/|/$", var.cur_s3_report_prefix)) && var.cur_s3_report_prefix != ""
        error_message = "cur_s3_report_prefix must not be empty and must not have leading or trailing slashes."
      }
    }

    variable "cur_report_name" {
      description = "The name for the Cost and Usage Report definition."
      type        = string
      default     = "DashboardSourceCUR"
    }

    variable "glue_database_name" {
      description = "Name for the AWS Glue Data Catalog database that will store CUR table metadata."
      type        = string
      default     = "aws_cur_analytics_db"
    }

    variable "owner_tag" {
      description = "Value for the Owner tag applied to resources."
      type        = string
      default     = "FinOpsDashboardTeam@example.com"
    }
    ```
    *   **Quan trọng:** Bạn **phải** cung cấp một giá trị duy nhất toàn cầu cho `cur_s3_bucket_name` khi chạy `terraform apply` (ví dụ, qua một tệp `.tfvars` hoặc `-var` flag).

3.  **`outputs.tf`**:
    ```terraform
    # outputs.tf

    output "cur_s3_bucket_name_for_dashboarding" {
      description = "The name of the S3 bucket storing CUR data and Athena query results."
      value       = aws_s3_bucket.cur_data_storage_bucket.bucket
    }

    output "cur_report_definition_name_for_dashboarding" {
      description = "The name of the Cost and Usage Report definition configured for dashboarding."
      value       = aws_cur_report_definition.dashboard_optimized_cur.report_name
    }

    output "glue_database_name_for_cur_analytics" {
      description = "The name of the AWS Glue Data Catalog database created for CUR analytics."
      value       = aws_glue_catalog_database.cur_data_analytics_database.name
    }

    output "glue_crawler_name_for_cur_data" {
      description = "The name of the AWS Glue Crawler configured to process CUR data."
      value       = aws_glue_crawler.cur_data_discovery_crawler.name
    }

    output "athena_workgroup_name_for_cur_analysis" {
      description = "The name of the Amazon Athena workgroup configured for CUR analysis."
      value       = aws_athena_workgroup.cur_data_analysis_workgroup.name
    }

    output "athena_query_results_s3_output_location" {
      description = "The S3 path where Athena query results for this workgroup will be stored."
      value       = aws_athena_workgroup.cur_data_analysis_workgroup.configuration[0].result_configuration[0].output_location
    }

    output "glue_crawler_iam_role_arn" {
      description = "ARN of the IAM Role used by the Glue Crawler."
      value       = aws_iam_role.cur_glue_crawler_execution_role.arn
    }
    ```

### Hướng dẫn từng bước

1.  **Tạo thư mục dự án và các tệp `.tf`**:
    *   Tạo một thư mục có tên `lab9-custom-dashboards`.
    *   Bên trong thư mục này, tạo các tệp `main.tf`, `variables.tf`, và `outputs.tf` với nội dung như trên.
    *   **Quan trọng:** Chuẩn bị một tên S3 bucket duy nhất toàn cầu. Bạn sẽ cần cung cấp nó khi chạy `apply` thông qua biến `cur_s3_bucket_name`.

2.  **Chạy `terraform init`**:
    *   Mở terminal, điều hướng đến thư mục `lab9-custom-dashboards`.
    *   Chạy: `terraform init`

3.  **Chạy `terraform plan`**:
    *   Xem kế hoạch thực thi. Bạn phải cung cấp `cur_s3_bucket_name`:
        ```bash
        terraform plan -var="cur_s3_bucket_name=your-globally-unique-cur-bucket-name"
        ```
    *   Terraform sẽ hiển thị các tài nguyên sẽ được tạo: S3 bucket, CUR definition, Glue database, IAM role cho crawler, Glue crawler, và Athena workgroup.

4.  **Chạy `terraform apply`**:
    *   Áp dụng các thay đổi. Nhớ cung cấp `cur_s3_bucket_name`:
        ```bash
        terraform apply -var="cur_s3_bucket_name=your-globally-unique-cur-bucket-name" -auto-approve
        ```
    *   Terraform sẽ tạo tất cả các tài nguyên.

5.  **Xác minh và Chạy Glue Crawler**:
    *   **CUR Delivery:** Đảm bảo CUR của bạn (được tạo bởi lab này hoặc một CUR hiện có được cấu hình tương tự) đã bắt đầu gửi dữ liệu đến S3 bucket. **Điều này là bắt buộc và có thể mất đến 24 giờ cho lần gửi đầu tiên.** Nếu crawler chạy mà không có dữ liệu CUR, nó sẽ không tạo được table.
    *   **AWS Glue Console:**
        *   Điều hướng đến **AWS Glue**.
        *   Trong menu bên trái, nhấp vào **Databases**. Bạn sẽ thấy database (ví dụ: `aws_cur_analytics_db`).
        *   Nhấp vào **Crawlers**. Bạn sẽ thấy crawler (ví dụ: `CUR-Data-Discovery-Crawler-lab-dev`).
        *   Chọn crawler và nhấp vào **Run crawler**. Quá trình này có thể mất vài phút (hoặc lâu hơn tùy thuộc vào lượng dữ liệu CUR và cấu hình).
        *   Theo dõi trạng thái của crawler. Sau khi crawler chạy thành công ("Succeeded"), quay lại **Databases**, nhấp vào database của bạn, và sau đó nhấp vào **Tables in database**. Bạn sẽ thấy một table được tạo bởi crawler (ví dụ: `cur_dashboardsourcecur`). Nhấp vào table để xem schema của nó. Schema này sẽ rất lớn, phản ánh tất cả các cột từ CUR.

6.  **Truy vấn Dữ liệu CUR bằng Amazon Athena**:
    *   Điều hướng đến **Amazon Athena**.
    *   **Workgroup:** Ở phía trên bên phải (hoặc trong tab "Workgroup:"), chọn workgroup bạn đã tạo (ví dụ: `CURDataAnalysisWorkgroup-lab-dev`). Điều này đảm bảo kết quả truy vấn được lưu vào đúng vị trí S3 đã cấu hình.
    *   **Data source và Database:** Trong ô "Data source" ở bên trái, chọn `AwsDataCatalog`. Trong ô "Database", chọn database của bạn (ví dụ: `aws_cur_analytics_db`).
    *   **Tables:** Bạn sẽ thấy table (ví dụ: `cur_dashboardsourcecur`) được liệt kê.
    *   **New query:** Mở một tab truy vấn mới và thử một truy vấn SQL đơn giản để kiểm tra:
        ```sql
        SELECT *
        FROM "cur_dashboardsourcecur" -- Sử dụng tên table chính xác, có thể cần dấu ngoặc kép nếu tên chứa ký tự đặc biệt hoặc là từ khóa.
        LIMIT 10;
        ```
        Hoặc một truy vấn hữu ích hơn để xem tổng chi phí theo dịch vụ cho một tháng cụ thể:
        ```sql
        SELECT
            line_item_product_code,
            line_item_usage_account_id,
            SUM(line_item_unblended_cost) AS total_unblended_cost
        FROM
            "cur_dashboardsourcecur" -- Thay thế bằng tên table thực tế của bạn
        WHERE
            year = '2024' AND month = '05' -- Điều chỉnh năm và tháng theo dữ liệu CUR của bạn
        GROUP BY
            line_item_product_code,
            line_item_usage_account_id
        ORDER BY
            total_unblended_cost DESC
        LIMIT 20;
        ```
        (Lưu ý: Tên cột trong CUR được tạo bởi Parquet thường là chữ thường và có dấu gạch dưới, ví dụ: `line_item_product_code`, `line_item_unblended_cost`. Kiểm tra schema trong Glue hoặc kết quả của `SELECT * LIMIT 1` để biết tên cột chính xác trong table của bạn.)
    *   Nhấp **Run**. Bạn sẽ thấy kết quả sau một lát.

7.  **Kết nối Amazon QuickSight với Athena và Xây dựng Dashboard (Hướng dẫn Khái niệm - Thực hành ngoài Terraform)**:
    *   **Đăng ký và Cấu hình Amazon QuickSight:**
        1.  Nếu bạn chưa sử dụng QuickSight, hãy đăng ký. Chọn phiên bản (Standard hoặc Enterprise).
        2.  Đảm bảo QuickSight có quyền truy cập vào Athena và S3 bucket chứa CUR. Điều này thường được cấu hình trong QuickSight: **Manage QuickSight > Security & permissions > Add or remove**. Cho phép QuickSight truy cập Athena và S3 bucket CUR của bạn.
    *   **Tạo Dataset Mới trong QuickSight từ Athena:**
        1.  Trong QuickSight, đi đến **Datasets** > **New dataset**.
        2.  Chọn **Athena** làm nguồn dữ liệu.
        3.  Đặt tên cho nguồn dữ liệu (ví dụ: `CUR_Athena_DataSource_Lab9`).
        4.  Chọn **Athena workgroup** mà bạn đã tạo (ví dụ: `CURDataAnalysisWorkgroup-lab-dev`).
        5.  Nhấp **Validate connection**.
        6.  Nhấp **Create data source**.
        7.  Trong màn hình tiếp theo, chọn **Catalog** (`AwsDataCatalog`), **Database** (ví dụ: `aws_cur_analytics_db`), và sau đó chọn **Table** (ví dụ: `cur_dashboardsourcecur`).
        8.  Nhấp **Select**.
        9.  Bạn có thể chọn **Import to SPICE for quicker analytics** (khuyến nghị cho hiệu suất dashboard tốt hơn, đặc biệt với CUR) hoặc **Directly query your data**.
        10. Nhấp **Visualize** (hoặc Edit/Preview data trước nếu muốn tùy chỉnh các kiểu dữ liệu hoặc thêm calculated fields).
    *   **Xây dựng Visualizations trong một Analysis của QuickSight:**
        1.  Bạn sẽ được chuyển đến một "Analysis" mới.
        2.  Từ danh sách "Fields list" ở bên trái (đây là các cột từ CUR table của bạn), kéo và thả các trường vào các "Field wells" (ví dụ: X axis, Y axis, Group/Color, Value) để tạo các biểu đồ khác nhau (ví dụ: biểu đồ thanh, biểu đồ đường, bảng tổng hợp - pivot table, biểu đồ tròn).
        3.  **Ví dụ về các Visuals có thể tạo:**
            *   **Biểu đồ thanh:** Tổng chi phí (`line_item_unblended_cost`) theo dịch vụ (`line_item_product_code` hoặc `product_product_name`).
            *   **Biểu đồ đường:** Xu hướng chi phí hàng ngày hoặc hàng tháng (`line_item_usage_start_date` và `line_item_unblended_cost`).
            *   **Bảng:** Chi tiết chi phí theo tag dự án (nếu bạn có các cột `resource_tags_user_project` hoặc tương tự) hoặc theo Cost Category.
            *   **Biểu đồ tròn:** Tỷ lệ chi phí theo tài khoản (`line_item_usage_account_id`).
        4.  Sử dụng filters, calculated fields, và các tham số (parameters) để làm cho dashboard của bạn trở nên tương tác và cung cấp nhiều thông tin hơn.
    *   **Xuất bản Dashboard:**
        1.  Khi bạn hài lòng với analysis của mình, bạn có thể **Share > Publish dashboard** để tạo một dashboard cố định có thể chia sẻ với người dùng khác trong tổ chức của bạn (tùy thuộc vào quyền của họ).

### Giải thích & Lý do

*   **Tại sao cần một Pipeline CUR + Athena + QuickSight?**
    *   **CUR là Nguồn Dữ liệu Vàng:** Cung cấp dữ liệu chi tiết nhất, là "single source of truth" cho tất cả chi phí và mức sử dụng AWS.
    *   **Athena cho Phân tích Linh hoạt:** Cho phép bạn sử dụng SQL tiêu chuẩn để truy vấn dữ liệu CUR thô, thực hiện các phép nối, tổng hợp, và lọc phức tạp mà không cần tải dữ liệu vào một data warehouse truyền thống. Tính serverless và chi phí theo truy vấn giúp tiết kiệm.
    *   **AWS Glue Data Catalog & Crawler:** Tự động hóa việc khám phá schema và quản lý metadata cho dữ liệu CUR, giúp Athena dễ dàng hiểu và truy vấn cấu trúc dữ liệu, ngay cả khi schema của CUR thay đổi.
    *   **QuickSight cho Trực quan hóa Trực quan:** Cung cấp một giao diện người dùng thân thiện để tạo các trực quan hóa và dashboard tương tác, giúp người dùng ở nhiều cấp độ kỹ năng (từ kỹ thuật đến kinh doanh) có thể khám phá và hiểu dữ liệu chi phí. Khả năng SPICE giúp tăng tốc độ phản hồi của dashboard.
*   **Tầm quan trọng của CUR được Cấu hình Tốt cho Dashboarding:**
    *   **`Parquet` format:** Hiệu quả hơn nhiều cho Athena về mặt hiệu suất và chi phí truy vấn so với CSV.
    *   **`HOURLY` granularity:** Cung cấp độ chi tiết cao nhất, cho phép phân tích xu hướng theo giờ, ngày, tháng.
    *   **`RESOURCES` trong `additional_schema_elements`:** Cực kỳ quan trọng để có thể phân tích chi phí xuống đến từng ARN tài nguyên cụ thể.
    *   **`SPLIT_COST_ALLOCATION_DATA`:** Hữu ích nếu bạn sử dụng Cost Categories với split charge rules và muốn trực quan hóa dữ liệu đã được phân bổ.
    *   **`ATHENA` và `QUICKSIGHT` trong `additional_artifacts`:** Giúp tự động tạo các tệp manifest mà Glue Crawler và QuickSight có thể sử dụng, đơn giản hóa đáng kể việc thiết lập ban đầu.
*   **AWS Glue Crawler và `TableGroupingPolicy = "CombineCompatibleSchemas"`:**
    *   Schema của CUR có thể thay đổi theo thời gian khi AWS thêm các cột mới hoặc dịch vụ mới. `CombineCompatibleSchemas` là một cài đặt quan trọng giúp crawler cố gắng hợp nhất các schema tương thích từ các tệp CUR khác nhau (ví dụ, từ các tháng khác nhau) vào một định nghĩa table duy nhất trong Glue Data Catalog, thay vì tạo ra nhiều table khác nhau, làm phức tạp việc truy vấn.
*   **Amazon Athena Workgroups:**
    *   Cho phép bạn tách biệt các khối lượng công việc truy vấn, quản lý vị trí lưu trữ kết quả truy vấn cho từng nhóm người dùng hoặc mục đích, và đặt các giới hạn kiểm soát chi phí cho việc sử dụng Athena.
*   **SPICE trong Amazon QuickSight:**
    *   Nhập dữ liệu vào SPICE (bộ nhớ đệm trong bộ nhớ của QuickSight) có thể cải thiện đáng kể hiệu suất và tốc độ phản hồi của dashboard, đặc biệt với các dataset lớn và phức tạp như CUR. Nó cũng giảm số lượng truy vấn trực tiếp đến Athena, có thể giúp kiểm soát chi phí Athena nếu dashboard được truy cập thường xuyên.

### Phương pháp Tốt nhất cho năm 2025
*   **Nền tảng Dữ liệu Vững chắc là Chìa khóa:** Một CUR được cấu hình tốt, chi tiết, và đáng tin cậy là nền tảng không thể thiếu cho mọi phân tích chi phí sâu sắc.
*   **Tự động hóa Thiết lập Hạ tầng Dữ liệu với Terraform:** Như đã thực hành trong lab này, tự động hóa việc tạo S3 bucket, định nghĩa CUR, Glue Database, Crawler, và Athena Workgroup để đảm bảo tính nhất quán, khả năng lặp lại, và quản lý dưới dạng mã.
*   **Thiết kế Dashboard Theo Persona và Mục tiêu Kinh doanh:** Tạo các dashboard khác nhau phục vụ các đối tượng người dùng khác nhau (ví dụ: kỹ sư hệ thống, quản lý tài chính, lãnh đạo cấp cao) với các KPIs, mức độ chi tiết, và góc nhìn phù hợp với vai trò và trách nhiệm của họ.
*   **Tập trung vào Việc Tạo ra "Insights" (Hiểu biết Sâu sắc), không chỉ Hiển thị "Data" (Dữ liệu):** Dashboard nên giúp người dùng trả lời các câu hỏi kinh doanh quan trọng, xác định các cơ hội tối ưu hóa, và thúc đẩy các hành động cụ thể, chứ không chỉ đơn thuần là hiển thị các con số và biểu đồ.
*   **Quản trị và Bảo mật Dữ liệu Chi phí:**
    *   Sử dụng quyền IAM chi tiết (principle of least privilege) cho Glue, Athena, và QuickSight.
    *   Xem xét việc mã hóa dữ liệu CUR trong S3 và kết quả truy vấn Athena khi lưu trữ.
    *   Quản lý quyền truy cập vào các dataset và dashboard trong QuickSight một cách cẩn thận.
*   **Tối ưu hóa Chi phí của Chính Giải pháp Dashboarding:**
    *   Theo dõi chi phí truy vấn Athena. Tối ưu hóa các truy vấn SQL (ví dụ: chỉ `SELECT` các cột cần thiết, sử dụng `WHERE` clause để lọc dữ liệu càng sớm càng tốt, tận dụng partitioning của CUR nếu có).
    *   Sử dụng SPICE trong QuickSight một cách hợp lý, cân nhắc giữa hiệu suất và chi phí của SPICE.
    *   Lên lịch làm mới dữ liệu cho các dataset trong QuickSight một cách thông minh, phù hợp với tần suất cập nhật của CUR và nhu cầu của người dùng.

### Xác minh
1.  S3 bucket và CUR definition đã được tạo thành công bằng Terraform.
2.  Glue database và Glue crawler đã được tạo thành công.
3.  Chạy Glue crawler thành công và một table (ví dụ: `cur_dashboardsourcecur`) đã được tạo trong Glue Data Catalog với schema phù hợp.
4.  Có thể truy vấn table này thành công từ Amazon Athena bằng cách sử dụng workgroup đã tạo, và nhận được kết quả (giả sử CUR đã có dữ liệu).
5.  **(Phần QuickSight - thực hành thủ công)** Có thể tạo một dataset mới trong QuickSight kết nối với Athena table đã tạo và xây dựng được ít nhất một biểu đồ đơn giản từ dữ liệu CUR.

### Dọn dẹp
1.  **Chạy `terraform destroy`**:
    *   Trong terminal, tại thư mục `lab9-custom-dashboards`, chạy lệnh sau, nhớ cung cấp `cur_s3_bucket_name`:
        ```bash
        terraform destroy -var="cur_s3_bucket_name=your-globally-unique-cur-bucket-name" -auto-approve
        ```
    *   Terraform sẽ cố gắng xóa Athena workgroup, Glue crawler, Glue database, IAM role & policy, CUR definition, và S3 bucket.
    *   **Lưu ý quan trọng về việc xóa S3 Bucket:** Nếu CUR đã gửi một lượng lớn dữ liệu vào S3 bucket, `terraform destroy` có thể thất bại nếu bucket không trống. Để đảm bảo `destroy` thành công cho mục đích lab, bạn có thể cần thêm `force_destroy = true` vào định nghĩa resource `aws_s3_bucket.cur_data_storage_bucket` (như đã thảo luận trong Lab 4). **CẢNH BÁO: `force_destroy = true` sẽ xóa tất cả các đối tượng trong bucket khi bucket bị xóa. KHÔNG sử dụng tùy chọn này một cách bất cẩn trong môi trường sản xuất.**

2.  **Dọn dẹp Tài sản Amazon QuickSight (Thủ công):**
    *   Terraform không quản lý các tài sản được tạo trong Amazon QuickSight (như datasets, analyses, dashboards).
    *   Nếu bạn đã tạo các tài sản này trong QuickSight, bạn cần vào QuickSight console và xóa thủ công các dataset, analyses, và dashboards liên quan đến lab này nếu bạn không muốn giữ chúng.
    *   Nếu bạn đã đăng ký QuickSight chỉ cho mục đích của lab này, hãy xem xét việc hủy đăng ký QuickSight (unsubscribing) nếu bạn không có nhu cầu sử dụng tiếp.

3.  **Xác minh việc xóa trong AWS Management Console**:
    *   Kiểm tra các dịch vụ Amazon Athena (Workgroups), AWS Glue (Databases, Crawlers), Amazon S3 (Buckets), và AWS Cost & Usage Reports (Report definitions) để xác nhận các tài nguyên đã được xóa bởi Terraform.

---

## 4. Lưu ý Kết luận Chuỗi Lab

Chúc mừng bạn đã hoàn thành chuỗi lab thực hành chuyên sâu về AWS Billing & Cost Management với Terraform! Qua 9 bài lab này, bạn đã được trang bị một bộ kiến thức và kỹ năng thực tế, bao gồm:

*   Thiết lập và quản lý **AWS Budgets** để theo dõi, cảnh báo và tự động hóa các hành động kiểm soát chi phí với **Budget Actions**.
*   Kích hoạt và sử dụng hiệu quả **Cost Allocation Tags** và **Cost Categories** để phân bổ và nhóm chi phí theo logic kinh doanh, tăng cường sự minh bạch.
*   Cấu hình và quản lý **AWS Cost and Usage Reports (CUR)** – nguồn dữ liệu chi tiết và toàn diện nhất – làm nền tảng cho mọi phân tích chi phí sâu sắc.
*   Triển khai **AWS Cost Anomaly Detection** để chủ động xác định và nhận cảnh báo về các xu hướng chi tiêu bất thường, giảm thiểu rủi ro "sốc hóa đơn".
*   Hiểu các khái niệm và chiến lược quản lý chi phí trong môi trường **đa tài khoản AWS Organizations**, bao gồm vai trò quan trọng của Service Control Policies (SCPs) trong việc thiết lập các "lan can bảo vệ" chi phí.
*   Tìm hiểu về các công cụ tối ưu hóa chi phí của AWS như **AWS Compute Optimizer** và **AWS Trusted Advisor**, cũng như cách tiếp cận việc hiện thực hóa các đề xuất của chúng.
*   Xây dựng nền tảng hạ tầng dữ liệu (CUR, S3, Glue, Athena) cần thiết cho việc tạo **dashboard chi phí tùy chỉnh** với các công cụ BI mạnh mẽ như Amazon QuickSight.
*   Và quan trọng nhất, bạn đã học cách **tự động hóa việc thiết lập và quản lý tất cả các cấu hình này bằng Terraform**, mang lại tính nhất quán, khả năng lặp lại, kiểm soát phiên bản, và hiệu quả cho các hoạt động FinOps của mình.

**Hạn chế và Lưu ý:**

*   Chuỗi lab này không thể bao gồm mọi khía cạnh của một lĩnh vực rộng lớn như tối ưu hóa chi phí AWS (ví dụ: chiến lược mua Reserved Instances/Savings Plans chi tiết, tối ưu hóa kiến trúc ứng dụng cụ thể cho từng dịch vụ, quản lý giấy phép phần mềm trên AWS, các công cụ của bên thứ ba).
*   Việc tự động hóa hoàn toàn các đề xuất tối ưu hóa (ví dụ: tự động rightsizing EC2 instances dựa trên output của Compute Optimizer mà không có sự can thiệp của con người) là một chủ đề nâng cao, đòi hỏi sự cẩn trọng tối đa, các cơ chế an toàn phức tạp, và quy trình kiểm thử nghiêm ngặt hơn những gì được trình bày.
*   Các ví dụ về SCPs và các cấu hình phức tạp khác mang tính minh họa và cần được điều chỉnh, kiểm thử kỹ lưỡng cho môi trường thực tế của bạn.
*   Chi phí phát sinh: Mặc dù các lab được thiết kế để sử dụng các dịch vụ có bậc miễn phí hoặc chi phí thấp, việc chạy tài nguyên trong một thời gian dài hoặc với dữ liệu lớn có thể phát sinh chi phí. Luôn dọn dẹp tài nguyên sau khi hoàn thành lab.

**Các Bước Tiếp theo Tiềm năng cho Hành trình Học tập của Bạn:**

*   **Đào sâu vào Truy vấn Dữ liệu CUR với Athena và SQL:** Học cách viết các truy vấn SQL phức tạp hơn để trích xuất những hiểu biết sâu sắc từ dữ liệu CUR, ví dụ như tính toán tỷ lệ sử dụng RI/SP, phân tích chi phí theo giờ cho các tác vụ batch, hoặc theo dõi chi phí truyền dữ liệu chi tiết.
*   **Trở thành Chuyên gia Amazon QuickSight:** Khám phá các tính năng nâng cao của QuickSight như calculated fields, parameters, controls, custom narratives (insight tự động), và các loại biểu đồ phức tạp để xây dựng các dashboard thực sự ấn tượng và hữu ích.
*   **Tích hợp Dữ liệu CUR với các Công cụ BI Khác:** Nếu tổ chức của bạn sử dụng các công cụ BI khác (ví dụ: Tableau, Microsoft Power BI, Looker), hãy tìm hiểu cách kết nối và phân tích dữ liệu CUR từ S3/Athena.
*   **Tự động hóa Báo cáo và Cảnh báo Chi phí Nâng cao:** Sử dụng AWS Lambda để xử lý dữ liệu CUR hoặc các thông báo SNS từ Budgets/Anomaly Detection nhằm tạo ra các báo cáo tùy chỉnh, gửi cảnh báo đến các kênh giao tiếp như Slack, hoặc thậm chí kích hoạt các quy trình khắc phục tự động đơn giản.
*   **Nghiên cứu Sâu về AWS Well-Architected Framework - Trụ cột Tối ưu hóa Chi phí:** Đọc tài liệu, whitepapers, và các blog của AWS để hiểu sâu hơn về các phương pháp tốt nhất và các mẫu thiết kế tối ưu chi phí.
*   **Làm chủ Chiến lược Mua Reserved Instances và Savings Plans:** Nghiên cứu cách phân tích khối lượng công việc, dự báo nhu cầu, và lựa chọn các gói RI/SP phù hợp nhất để tối đa hóa tiết kiệm cho các tài nguyên có mức sử dụng ổn định.
*   **Thực hành và Áp dụng các Nguyên tắc FinOps:** Chủ động áp dụng các nguyên tắc và quy trình FinOps trong tổ chức của bạn, thúc đẩy sự hợp tác chặt chẽ giữa các nhóm kỹ thuật, tài chính, và kinh doanh để quản lý chi phí đám mây một cách hiệu quả và có trách nhiệm.
*   **Khám phá các Công cụ Quản lý Chi phí của Bên Thứ Ba:** Tìm hiểu về các giải pháp thương mại có thể cung cấp thêm các tính năng và góc nhìn về quản lý chi phí đám mây.

Hy vọng rằng chuỗi lab này đã cung cấp cho bạn một nền tảng vững chắc và nguồn cảm hứng để tiếp tục khám phá và trở thành một chuyên gia trong việc quản lý và tối ưu hóa chi phí trên AWS, đặc biệt với sự hỗ trợ của Infrastructure as Code bằng Terraform. Chúc bạn thành công trên hành trình chinh phục đám mây của mình!
