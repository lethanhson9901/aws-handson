# **Làm chủ Quản lý Truy cập và Định danh AWS: Hướng dẫn Toàn diện**

## **1\. Giới thiệu: Vai trò Không thể Thiếu của IAM trong Bảo mật Đám mây**

Trong kiến trúc đám mây hiện đại, Quản lý Truy cập và Định danh (IAM) không chỉ là một tính năng mà là nền tảng bảo mật cơ bản của bất kỳ môi trường Amazon Web Services (AWS) nào. IAM hoạt động như một công cụ kiểm soát truy cập cho AWS, cho phép xác thực (xác định ai đang đăng nhập) và ủy quyền (cấp quyền) cho người dùng và dịch vụ.1 Mục đích cốt lõi của IAM là triển khai nguyên tắc đặc quyền tối thiểu, đảm bảo mỗi người dùng hoặc dịch vụ chỉ có những quyền cần thiết để thực hiện nhiệm vụ của họ, không hơn không kém.1 Nó đóng vai trò là người gác cổng trung tâm, nơi các tổ chức xác định ai (định danh) có thể làm gì (hành động) trên tài nguyên nào, và trong những điều kiện nào.2 Việc cấu hình sai trong IAM có thể dẫn đến những hậu quả bảo mật trực tiếp và nghiêm trọng.

Việc triển khai IAM hiệu quả không chỉ củng cố tình hình bảo mật mà còn trực tiếp thúc đẩy sự linh hoạt trong kinh doanh. Khi quyền truy cập được kiểm soát một cách an toàn và rõ ràng, các nhóm phát triển có thể đổi mới nhanh hơn, triển khai tài nguyên một cách tự tin và đáp ứng các yêu cầu tuân thủ mà không tạo ra rủi ro không cần thiết. Điều này thay đổi nhận thức về IAM từ một cơ chế thuần túy hạn chế thành một yếu tố nền tảng cho các hoạt động đám mây có khả năng mở rộng và an toàn. Ví dụ, bằng cách ủy quyền một cách an toàn thông qua các tính năng như giới hạn quyền (permissions boundaries), bộ phận CNTT trung tâm có thể trao quyền cho các nhóm nhà phát triển mà không mất kiểm soát, từ đó tăng tốc chu kỳ phát triển.3

Tuy nhiên, tính chi tiết và sức mạnh của IAM, cho phép kiểm soát chính xác, cũng chính là nguồn gốc của sự phức tạp của nó.2 Nếu không có sự hiểu biết sâu sắc và quản lý cẩn thận, sự phức tạp này có thể dẫn đến cấu hình sai, cấp quyền quá mức và các lỗ hổng bảo mật. Bản thân sự phát triển của IAM, hướng tới các công cụ như IAM Identity Center và các tính năng như Kiểm soát Truy cập Dựa trên Thuộc tính (ABAC), là một phản ứng trực tiếp để quản lý sự phức tạp này ở quy mô lớn.5

### **Điều hướng Sự tiến hóa của AWS IAM: Từ Người dùng Tĩnh đến Kiểm soát Động, Lấy Định danh làm Trung tâm**

Hiểu được quá trình phát triển của IAM là rất quan trọng vì nó làm nổi bật các vấn đề mà mỗi tính năng hoặc phương pháp mới nhằm giải quyết.5 Bối cảnh lịch sử này giải thích *tại sao* một số phương pháp thực hành tốt nhất nhất định (như ưu tiên vai trò hơn người dùng) tồn tại và cách AWS đã cải tiến lặp đi lặp lại việc quản lý danh tính để đáp ứng quy mô và nhu cầu bảo mật ngày càng tăng.

* **Giai đoạn một: Người dùng IAM và Truy cập tĩnh:** Trong giai đoạn đầu, AWS IAM xoay quanh **người dùng IAM tĩnh** với thông tin xác thực dài hạn, được cấu hình và phân phối thủ công. Mô hình này, mặc dù phù hợp với các nhóm nhỏ hoặc thiết lập một tài khoản, nhanh chóng trở nên có vấn đề khi các tổ chức mở rộng quy mô. Điều này dẫn đến các tài khoản được cấp quá nhiều quyền, vi phạm nguyên tắc đặc quyền tối thiểu, và tạo ra **quyền truy cập thường trực**, làm tăng rủi ro bảo mật. Việc **lan tràn thông tin xác thực** và khả năng kiểm toán kém khiến việc theo dõi và quản lý khóa truy cập trở nên khó khăn. Hơn nữa, **chi phí vận hành cao** cho các tác vụ như xoay vòng khóa, chấm dứt quyền truy cập và ứng phó sự cố đã cản trở sự linh hoạt và khả năng hiển thị, đặc biệt là trong môi trường nhiều tài khoản.5  
* **Giai đoạn hai: Vai trò, STS và Truy cập Liên kết:** Để khắc phục những hạn chế của người dùng IAM tĩnh, AWS đã giới thiệu **Vai trò IAM** và **Dịch vụ Mã thông báo Bảo mật (STS)**. Vai trò IAM cho phép giả định thông tin xác thực tạm thời dựa trên các chính sách tin cậy, trong khi STS cho phép truy cập dựa trên phiên với giới hạn thời gian xác định. Giai đoạn này cũng chứng kiến sự ra đời của **truy cập liên kết**, cho phép các tổ chức kết nối các nhà cung cấp danh tính (IdP) hiện có của họ với AWS. Điều này đã cải thiện đáng kể **quản lý vòng đời truy cập**, đặc biệt là đối với việc quản lý các thay đổi quyền truy cập của người dùng. **Tình hình bảo mật** được tăng cường bằng cách giảm sự phụ thuộc vào thông tin xác thực dài hạn và quyền truy cập thường trực. **Khả năng kiểm toán** được cải thiện với nhật ký phiên và khả năng liên kết danh tính. Tuy nhiên, việc quản lý vai trò và mối quanahệ tin cậy trên nhiều tài khoản AWS vẫn đòi hỏi sự phối hợp phức tạp và thủ công.5  
* **Giai đoạn ba: AWS Organizations và Mô hình Đa tài khoản:** Khi việc sử dụng AWS ngày càng tăng, các tổ chức bắt đầu áp dụng **chiến lược đa tài khoản** bằng cách sử dụng **AWS Organizations** để tách biệt khối lượng công việc, môi trường và các đơn vị kinh doanh. Mô hình này đã cải thiện **khả năng cô lập và khả năng chịu lỗi**, tăng cường **ranh giới tuân thủ**, đồng thời cung cấp khả năng **phân bổ chi phí và quản trị tốt hơn**. Tuy nhiên, việc quản lý nhất quán các quyền trên nhiều tài khoản AWS đã trở thành một thách thức mới. Các vai trò IAM phải được sao chép và ánh xạ thủ công trên các tài khoản, và việc quản trị thường dựa vào các phương pháp thủ công như bảng tính. Sự phức tạp này đã làm nổi bật sự cần thiết của một giải pháp tập trung hơn.5  
* **Giai đoạn bốn: AWS IAM Identity Center (Trước đây là AWS SSO):** Để giải quyết các thách thức phối hợp trong môi trường đa tài khoản, AWS đã giới thiệu **IAM Identity Center**. Dịch vụ này cung cấp khả năng điều phối danh tính và truy cập tập trung trên AWS Organizations. Các đổi mới chính bao gồm **Bộ quyền (Permission Sets)**, là các mẫu xác định chính sách truy cập có thể được ánh xạ tới các vai trò trong nhiều tài khoản. **Việc gán dựa trên nhóm** cho phép cấp quyền truy cập ở cấp độ nhóm thông qua các IdP bên ngoài. **Khả năng hiển thị trên nhiều tài khoản** cho phép quản trị viên quản lý quyền truy cập một cách tập trung, trong khi người dùng có thể đăng nhập thông qua một bảng điều khiển hợp nhất. Việc sử dụng **thông tin xác thực ngắn hạn** tiếp tục giảm thiểu tác động tiềm ẩn của việc xâm phạm thông tin xác thực và cải thiện khảibility kiểm toán. Giai đoạn này đánh dấu một bước tiến quan trọng hướng tới **coi danh tính là mặt phẳng kiểm soát**, phù hợp với các nguyên tắc Zero Trust.5  
* **Ngày giới thiệu các tính năng chính:** Giới hạn quyền (Permissions Boundaries) được giới thiệu vào tháng 7 năm 2018, vai trò liên kết dịch vụ (Service-linked roles) vào tháng 4 năm 2017, thông tin về ABAC được bổ sung vào tháng 10 năm 2019 và Thẻ phiên (Session Tags) vào tháng 11 năm 2019\.6

Sự phát triển của IAM phản ánh một sự chuyển dịch chiến lược từ bảo mật dựa trên vành đai tài nguyên sang bảo mật dựa trên vành đai định danh. IAM ban đầu tập trung vào việc bảo vệ các tài nguyên riêng lẻ bằng thông tin xác thực người dùng tĩnh. Sự ra đời của vai trò, liên kết danh tính và đặc biệt là IAM Identity Center cho thấy "định danh" (dù là con người hay khối lượng công việc) ngày càng trở thành mặt phẳng kiểm soát chính.5 Các quyết định truy cập ngày càng dựa trên các thuộc tính và ngữ cảnh của định danh, thay vì chỉ dựa trên việc ai sở hữu tài nguyên. Điều này phù hợp với các nguyên tắc Zero Trust hiện đại.5

Hơn nữa, những thách thức về khả năng mở rộng đã thúc đẩy sự đổi mới của IAM. Mỗi giai đoạn phát triển của IAM đều trực tiếp giải quyết các vấn đề về khả năng mở rộng. Người dùng tĩnh không thể mở rộng quy mô.5 Việc quản lý vai trò trên nhiều tài khoản trở nên phức tạp.5 IAM Identity Center và các tính năng như ABAC được thiết kế để quản lý quyền truy cập hiệu quả trong các tổ chức lớn, đa tài khoản. Các vấn đề về "lan tràn thông tin xác thực" 5 và "bùng nổ vai trò" 7 là hậu quả trực tiếp của việc các giải pháp không theo kịp sự tăng trưởng của tổ chức, thúc đẩy các phương pháp tiếp cận mới hơn, có khả năng mở rộng tốt hơn.

## **2\. Các Thành phần Cơ bản của AWS IAM: Định danh, Thông tin Xác thực và Quyền**

Để quản lý quyền truy cập một cách hiệu quả trong AWS, điều cần thiết là phải hiểu rõ các thành phần cốt lõi của IAM: định danh (identities), thông tin xác thực (credentials) và quyền (permissions). Những yếu tố này phối hợp với nhau để xác định ai có thể làm gì và trên những tài nguyên nào.

### **Người dùng IAM: Mục đích, Thông tin Xác thực và Các Phương pháp Tốt nhất cho Việc Sử dụng Hạn chế**

Người dùng IAM đại diện cho các cá nhân hoặc ứng dụng cần truy cập vào tài khoản AWS của bạn.1 Họ là các tài khoản riêng lẻ với các quyền cụ thể.

Một đặc điểm quan trọng của người dùng IAM là họ sở hữu **thông tin xác thực dài hạn**. Các thông tin này bao gồm:

* **Mật khẩu (Passwords):** Được sử dụng để người dùng đăng nhập vào Bảng điều khiển Quản lý AWS (AWS Management Console).8  
* **Khóa truy cập (Access Keys):** Bao gồm một ID Khóa truy cập (Access Key ID) và một Khóa truy cập bí mật (Secret Access Key). Chúng được sử dụng để truy cập theo chương trình vào các dịch vụ AWS thông qua Giao diện Dòng lệnh AWS (AWS CLI), Bộ công cụ Phát triển Phần mềm (SDK) và các API khác.8  
* **Chứng chỉ ký X.509 (X.509 Signing Certificates):** Được sử dụng cho các tích hợp dịch vụ AWS cụ thể yêu cầu xác thực các yêu cầu đã ký bằng khóa riêng tương ứng. Chúng không dùng cho mục đích xác thực người dùng chung chung như mật khẩu hay khóa truy cập.9 Điều quan trọng là phải phân biệt chúng với các loại chứng chỉ khác, chẳng hạn như chứng chỉ SSL/TLS dùng cho HTTPS hoặc chứng chỉ máy khách dùng trong IoT.12

Mặc dù người dùng IAM là một thành phần cơ bản, việc sử dụng họ với thông tin xác thực dài hạn được khuyến nghị hạn chế trong các trường hợp cụ thể. Các trường hợp sử dụng được chấp nhận bao gồm:

* **Truy cập khẩn cấp (Emergency access):** Trong các tình huống "phá kính" khi các hệ thống quản lý danh tính khác có thể không khả dụng.9  
* **Các khối lượng công việc không thể sử dụng vai trò IAM:** Điều này bao gồm một số dịch vụ AWS cụ thể như AWS CodeCommit và Amazon Keyspaces (cho Apache Cassandra), hoặc một số máy khách AWS của bên thứ ba cũ hơn có thể dựa vào khóa truy cập của người dùng IAM.9  
* **Khi AWS IAM Identity Center không khả dụng:** Trong trường hợp không có sẵn các dịch vụ quản lý danh tính nâng cao hơn và không có nhà cung cấp danh tính nào khác, người dùng IAM có thể là cách duy nhất để cấp quyền truy cập.9

Tuy nhiên, người dùng IAM có những hạn chế đáng kể:

* **Khả năng mở rộng kém:** Việc quản lý quyền và bảo mật cho một số lượng lớn người dùng IAM trở nên khó khăn khi tổ chức phát triển.9  
* **Thiếu khả năng hiển thị và kiểm toán tập trung:** So với các giải pháp quản lý danh tính khác của AWS, người dùng IAM thiếu khả năng hiển thị và kiểm toán tập trung, gây khó khăn hơn trong việc duy trì bảo mật và tuân thủ quy định.9  
* **Triển khai các phương pháp bảo mật tốt nhất phức tạp hơn:** Việc triển khai các biện pháp bảo mật như Xác thực Đa yếu tố (MFA), chính sách mật khẩu và tách biệt vai trò trở nên phức tạp hơn với người dùng IAM.9

Do những hạn chế này, các phương pháp thực hành tốt nhất của AWS thường khuyến nghị *không* sử dụng người dùng IAM cho hầu hết các tình huống, thay vào đó ưu tiên sử dụng Vai trò IAM và thông tin xác thực tạm thời.1 Nếu bắt buộc phải sử dụng người dùng IAM, điều cực kỳ quan trọng là phải bảo mật họ bằng MFA 10, thường xuyên xoay vòng thông tin xác thực 10 và luôn tuân thủ nguyên tắc đặc quyền tối thiểu.

Mặc dù các phương pháp thực hành tốt nhất ủng hộ mạnh mẽ việc sử dụng thông tin xác thực tạm thời thông qua vai trò, sự tồn tại của các trường hợp sử dụng hợp pháp cho người dùng IAM 9 có nghĩa là chúng không thể bị loại bỏ hoàn toàn. Điều này tạo ra một sự căng thẳng: các tổ chức phải hỗ trợ các trường hợp sử dụng này trong khi quản lý nghiêm ngặt các rủi ro liên quan đến thông tin xác thực dài hạn. Thực tế này cho thấy rằng người dùng IAM, mặc dù ít lý tưởng hơn, đôi khi là không thể tránh khỏi, khiến việc quản trị chặt chẽ của họ càng trở nên quan trọng hơn.

**Bảng 2.1: Tổng quan về Thông tin Xác thực của Người dùng IAM**

| Loại Thông tin Xác thực | Mô tả | Trường hợp Sử dụng Chính | Cân nhắc Bảo mật | Khuyến nghị Quản lý |
| :---- | :---- | :---- | :---- | :---- |
| Mật khẩu (Passwords) | Chuỗi ký tự bí mật được sử dụng để đăng nhập vào Bảng điều khiển Quản lý AWS. | Truy cập Bảng điều khiển Quản lý AWS cho người dùng IAM. | Dễ bị tấn công brute-force, phishing. Cần chính sách mật khẩu mạnh. | Sử dụng mật khẩu mạnh, duy nhất. Bật MFA. Xoay vòng định kỳ. Hạn chế sử dụng người dùng IAM có mật khẩu, ưu tiên liên kết danh tính. |
| Khóa truy cập (Access Keys) | Bao gồm ID Khóa truy cập và Khóa truy cập bí mật. Được sử dụng cho truy cập theo chương trình. | Truy cập AWS CLI, SDK, API cho các ứng dụng hoặc tập lệnh. | Rủi ro cao nếu bị lộ vì chúng là thông tin xác thực dài hạn. Không nên nhúng vào mã nguồn. | Hạn chế tối đa việc tạo và sử dụng. Ưu tiên vai trò IAM cho thông tin xác thực tạm thời. Nếu bắt buộc, lưu trữ an toàn, xoay vòng thường xuyên (ví dụ: 90 ngày), và xóa khi không còn cần thiết. Bật MFA cho người dùng sở hữu khóa. Giám sát việc sử dụng lần cuối. 10 |
| Chứng chỉ ký X.509 | Chứng chỉ kỹ thuật số được sử dụng để xác thực các yêu cầu đã ký cho một số dịch vụ AWS cụ thể. 9 | Tích hợp với các dịch vụ AWS cụ thể yêu cầu xác thực dựa trên chứng chỉ (ví dụ: SOAP API cũ). | Ít phổ biến hơn, quản lý vòng đời chứng chỉ (phát hành, thu hồi, xoay vòng) là cần thiết. | Chỉ sử dụng khi dịch vụ AWS yêu cầu cụ thể. Quản lý vòng đời chứng chỉ một cách cẩn thận. Không phải là cơ chế xác thực người dùng chung. 11 |

Bảng này phân biệt rõ ràng các loại thông tin xác thực dài hạn liên quan đến người dùng IAM, ứng dụng cụ thể của chúng và các rủi ro bảo mật cố hữu, củng cố lý do để giảm thiểu việc sử dụng chúng và ưu tiên thông tin xác thực tạm thời.

### **Nhóm IAM: Hợp lý hóa Quản lý Quyền cho Người dùng**

Nhóm IAM là tập hợp những người dùng IAM có chung các quyền.1 Chúng đơn giản hóa việc quản lý quyền bằng cách cho phép gán chính sách cho nhiều người dùng cùng một lúc, thay vì phải gán riêng lẻ cho từng người.1 Người dùng trong một nhóm sẽ kế thừa các quyền của nhóm đó.19

Đây là một phương pháp thực hành tốt nhất: sử dụng nhóm để quản lý quyền và quản lý quyền ở cấp độ nhóm.1 Nhóm là một công cụ cơ bản để tổ chức người dùng và quản lý quyền của họ một cách hiệu quả, đặc biệt khi xử lý nhiều người dùng yêu cầu các cấp độ truy cập tương tự. Chúng là một thành phần quan trọng trong việc áp dụng nguyên tắc đặc quyền tối thiểu ở quy mô lớn cho người dùng IAM.

Cần lưu ý rằng Nhóm IAM chủ yếu dùng để quản lý người dùng IAM và không tương tác trực tiếp với Vai trò IAM để kế thừa quyền. Đoạn trích 19 nêu rõ: "Vai trò IAM dùng để quản lý thông tin xác thực tạm thời... trong khi Nhóm IAM tổ chức người dùng để quản lý quyền dễ dàng hơn." Mặc dù một người dùng trong một nhóm có thể được *phép* đảm nhận một vai trò (nếu chính sách của nhóm cấp quyền sts:AssumeRole), bản thân nhóm đó không đảm nhận vai trò, cũng như các vai trò không kế thừa quyền từ các nhóm mà chúng không thuộc về. Sự phân biệt này rất quan trọng để hiểu cách các quyền được tổng hợp và áp dụng trong các tình huống IAM khác nhau. Lợi ích chính của nhóm là đơn giản hóa việc đính kèm chính sách cho *người dùng*.

### **Chính sách IAM: Ngôn ngữ của Ủy quyền**

Chính sách IAM là các tài liệu JSON xác định rõ ràng các quyền được cấp cho người dùng, nhóm hoặc vai trò.2 Chúng quy định những hành động nào được phép hoặc bị từ chối trên những tài nguyên AWS nào.

#### **Cấu trúc Cốt lõi: Version, Statement (Sid, Effect, Principal, Action, Resource, Condition)**

Việc hiểu cấu trúc JSON là điều cần thiết để viết, diễn giải và khắc phục sự cố các chính sách IAM. Mỗi phần tử đóng một vai trò quan trọng trong việc xác định phạm vi và hiệu lực của các quyền.

* **Version**: Chỉ định phiên bản ngôn ngữ chính sách. Giá trị được hỗ trợ phổ biến nhất là "2012-10-17".20  
* **Statement**: Là khối xây dựng chính của chính sách JSON, chứa một mảng các câu lệnh riêng lẻ. Mỗi câu lệnh xác định một tập hợp các quyền.20  
* **Sid (Statement ID)**: Một yếu tố tùy chọn cung cấp một mã định danh cho câu lệnh riêng lẻ trong một chính sách. Nó có thể được sử dụng để tham chiếu đến một câu lệnh cụ thể sau này.20  
* **Effect**: Một yếu tố bắt buộc chỉ định liệu câu lệnh dẫn đến cho phép (Allow) hay từ chối rõ ràng (Deny).2  
* **Principal**: Chỉ định tài khoản, người dùng, nhóm hoặc dịch vụ được phép hoặc bị từ chối truy cập vào các tài nguyên được chỉ định trong câu lệnh. Yếu tố này thường được sử dụng trong các chính sách dựa trên tài nguyên để chỉ ra ai là người được cấp quyền. Đối với các chính sách IAM được đính kèm với người dùng, nhóm hoặc vai trò IAM, principal ngầm định là định danh mà chính sách được đính kèm, do đó không cần chỉ định yếu tố này.2  
* **Action**: Một yếu tố bắt buộc chỉ định (các) hành động mà principal được phép hoặc bị từ chối thực hiện trên (các) tài nguyên được chỉ định. Nó chứa một mảng các tên hành động, thường là dành riêng cho dịch vụ (ví dụ: "s3:GetObject", "ec2:RunInstances"). Có thể sử dụng ký tự đại diện (\*) để chỉ định nhiều hành động.2  
* **Resource**: Một yếu tố bắt buộc chỉ định (các) đối tượng mà câu lệnh bao gồm. Tài nguyên cũng dành riêng cho dịch vụ và thường được xác định bằng Tên Tài nguyên Amazon (ARN). Tương tự như Action, nó chứa một mảng và có thể sử dụng ký tự đại diện.2  
* **Condition**: Một yếu tố tùy chọn chỉ định một hoặc nhiều điều kiện phải được đáp ứng để câu lệnh được áp dụng. Các điều kiện được thể hiện dưới dạng các cặp khóa-giá trị, trong đó khóa là một khóa điều kiện AWS được xác định trước hoặc một khóa dành riêng cho dịch vụ, và giá trị là giá trị cần được so sánh. Điều kiện có thể được sử dụng để tinh chỉnh thêm các quyền dựa trên các yếu tố khác nhau như địa chỉ IP nguồn, ngày giờ hoặc sự hiện diện của các thẻ cụ thể.20

#### **Các loại Chính sách: Dựa trên Định danh và Dựa trên Tài nguyên**

Sự phân biệt và tương tác giữa các loại chính sách này là nền tảng để hiểu cách các quyền được đánh giá trong AWS.

* **Chính sách dựa trên định danh (Identity-based policies):** Được đính kèm với các định danh IAM (người dùng, nhóm, vai trò).2 Chúng xác định những hành động mà định danh đó có thể thực hiện.  
* **Chính sách dựa trên tài nguyên (Resource-based policies):** Được đính kèm trực tiếp vào tài nguyên (ví dụ: S3 bucket, hàng đợi SQS, hàm Lambda, khóa KMS).2 Chúng xác định ai có thể truy cập tài nguyên cụ thể đó. Chính sách dựa trên tài nguyên đặc biệt quan trọng đối với quyền truy cập giữa các tài khoản và cấp quyền cho các dịch vụ AWS.

Khi một thực thể IAM trong cùng một tài khoản truy cập một tài nguyên, các quyền là sự *kết hợp (union)* của các chính sách dựa trên định danh và dựa trên tài nguyên.26 Một sự từ chối rõ ràng (Deny) trong một trong hai loại chính sách sẽ ghi đè lên bất kỳ sự cho phép (Allow) nào.26 Đối với quyền truy cập S3 giữa các tài khoản, cả chính sách định danh của người dùng và chính sách tài nguyên của bucket đều phải cấp quyền truy cập.29 Một ngoại lệ quan trọng là chính sách khóa KMS; chúng là chính sách chính, và các chính sách IAM không thể cấp quyền truy cập trừ khi chính sách khóa cho phép.25

Chính sách dựa trên tài nguyên hoạt động như "cửa trước" để truy cập tài nguyên, đặc biệt là truy cập giữa các tài khoản. Trong khi các chính sách dựa trên định danh xác định những gì một định danh *có thể làm*, các chính sách dựa trên tài nguyên xác định ai *có thể truy cập chính tài nguyên đó*. Đối với các dịch vụ như S3, KMS, Lambda, v.v., chính sách dựa trên tài nguyên là một điểm kiểm soát quan trọng. Như đã đề cập, đối với quyền truy cập S3 giữa các tài khoản, *cả hai* chính sách đều phải cho phép.29 Tương tự, chính sách khóa KMS là *chính yếu*, và chỉ riêng các chính sách IAM không thể cấp quyền truy cập nếu chính sách khóa không cho phép.25 Điều này ngụ ý rằng các chính sách dựa trên tài nguyên thường có tiếng nói quyết định hơn đối với quyền truy cập vào tài nguyên mà chúng được đính kèm, đặc biệt là từ các principal hoặc dịch vụ bên ngoài.

#### **Chính sách Được quản lý: AWS Managed vs. Customer Managed**

Việc lựa chọn chiến lược quản lý chính sách phù hợp sẽ ảnh hưởng đến bảo mật, khả năng bảo trì và việc tuân thủ nguyên tắc đặc quyền tối thiểu.

* **Chính sách do AWS quản lý (AWS Managed Policies):** Được AWS xác định trước và duy trì. Chúng được AWS cập nhật khi có các dịch vụ và quyền mới được thêm vào. Chúng cung cấp một cách nhanh chóng để cấp quyền cho các trường hợp sử dụng phổ biến và các chức năng công việc. Tuy nhiên, chúng thường cấp nhiều quyền hơn mức cần thiết cho một tác vụ cụ thể, có khả năng gây ra rủi ro bảo mật.14  
* **Chính sách do khách hàng quản lý (Customer Managed Policies):** Là các chính sách mà bạn tạo và quản lý trong tài khoản AWS của mình. Một chính sách duy nhất có thể được đính kèm với nhiều thực thể chính (người dùng, nhóm và vai trò). Các thay đổi đối với một chính sách được áp dụng cho tất cả các thực thể được đính kèm. IAM lưu giữ tối đa năm phiên bản chính sách của bạn, cho phép bạn hoàn nguyên về các phiên bản trước đó. Bạn có toàn quyền kiểm soát để xác định các quyền chính xác cần thiết cho các tác vụ cụ thể, tuân theo nguyên tắc đặc quyền tối thiểu. Chúng cũng có giới hạn ký tự lớn hơn so với chính sách nội tuyến của nhóm.14  
* **Khuyến nghị:** Nên bắt đầu với Chính sách do AWS quản lý, sau đó chuyển sang Chính sách do khách hàng quản lý để đạt được đặc quyền tối thiểu.14 Ưu tiên các chính sách được quản lý hơn các chính sách nội tuyến.22

**Bảng 2.2: So sánh các Loại Quản lý Chính sách IAM**

| Loại Chính sách | Tạo & Quản lý | Khả năng Tái sử dụng | Phiên bản | Mức độ chi tiết (Đặc quyền Tối thiểu) | Trường hợp Sử dụng | Ưu điểm | Nhược điểm |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **AWS Managed Policies** | Do AWS tạo và duy trì.22 | Cao | Không có (AWS quản lý) | Thấp (thường rộng hơn cần thiết).14 | Bắt đầu nhanh, các chức năng công việc phổ biến, hiểu các quyền cần thiết ban đầu. | Dễ sử dụng, cập nhật tự động bởi AWS, không tốn công quản lý.22 | Không đảm bảo đặc quyền tối thiểu, có thể cấp quá nhiều quyền.14 |
| **Customer Managed Policies** | Do người dùng tạo và quản lý.22 | Cao | Có (tối đa 5 phiên bản).22 | Cao (người dùng toàn quyền kiểm soát).14 | Hầu hết các trường hợp, định nghĩa quyền tái sử dụng, triển khai đặc quyền tối thiểu, cần phiên bản hóa. | Tái sử dụng, quản lý thay đổi tập trung, phiên bản và khôi phục, ủy quyền quản lý, giới hạn ký tự lớn hơn, đạt được đặc quyền tối thiểu.22 | Đòi hỏi nỗ lực tạo và duy trì, cần chuyên môn để viết chính sách đặc quyền tối thiểu.22 |
| **Inline Policies** | Nhúng trực tiếp vào một định danh duy nhất.22 | Không | Không có.22 | Cao (dành riêng cho một định danh) | Quyền rất cụ thể cho một định danh duy nhất, không có ý định tái sử dụng.22 | Mối quan hệ 1-1 chặt chẽ, tự động xóa khi định danh bị xóa.22 | Không thể tái sử dụng, quản lý thay đổi phi tập trung, không có phiên bản, giới hạn ký tự nhỏ hơn (đối với nhóm).22 |

Bảng này cung cấp một so sánh rõ ràng, song song, cho phép người dùng đưa ra quyết định sáng suốt về loại chính sách nào sẽ sử dụng trong các tình huống khác nhau, trực tiếp giải quyết nhu cầu về thông tin chi tiết và dễ hiểu. Đây là một khái niệm cốt lõi được yêu cầu làm rõ một cách rõ ràng.22

Sự cân bằng giữa "tiện lợi và kiểm soát" là một chủ đề trung tâm trong các phương pháp thực hành tốt nhất của IAM. Chính sách do AWS quản lý mang lại sự tiện lợi và rất tốt để bắt đầu.14 Tuy nhiên, sự tiện lợi này đi kèm với cái giá là các quyền có khả năng được cấp quá mức.14 Chính sách do khách hàng quản lý cung cấp khả năng kiểm soát chi tiết và cho phép đạt được đặc quyền tối thiểu thực sự nhưng đòi hỏi nhiều nỗ lực hơn để tạo và duy trì.22 Chính sách nội tuyến cung cấp sự kết hợp chặt chẽ nhất nhưng lại ít khả năng tái sử dụng và quản lý nhất.22

#### **Chính sách Nội tuyến: Khi Tính Đặc thù là Quan trọng**

Chính sách nội tuyến (Inline Policies) được nhúng trực tiếp vào một người dùng, nhóm hoặc vai trò IAM duy nhất.22 Chúng thiết lập một mối quan hệ một-một nghiêm ngặt và sẽ tự động bị xóa khi định danh liên quan bị xóa.22 Chúng không thể tái sử dụng và không có phiên bản.22

Chính sách nội tuyến là dành cho các "trường hợp ngoại lệ", không phải là quy tắc chung. Các đặc điểm của chính sách nội tuyến (không thể tái sử dụng, không có phiên bản, gắn liền với một thực thể duy nhất) 22 khiến chúng chỉ phù hợp với các yêu cầu về quyền thực sự duy nhất và sẽ không được sao chép. Việc sử dụng chúng rộng rãi sẽ dẫn đến các quyền không thể quản lý và không nhất quán, trực tiếp mâu thuẫn với lợi ích của các chính sách được quản lý và nhóm IAM. Chúng đại diện cho một lựa chọn có chủ ý cho một tập hợp quyền rất cụ thể, biệt lập.

### **Xác thực Đa yếu tố (MFA): Tăng cường Bảo mật Định danh**

Xác thực Đa yếu tố (MFA) là một biện pháp bảo mật quan trọng, bổ sung một lớp xác thực thứ hai ngoài tên người dùng và mật khẩu, yêu cầu người dùng cung cấp hai hoặc nhiều yếu tố xác minh để truy cập.15 Điều này tăng cường đáng kể tính bảo mật cho tài khoản và tài nguyên AWS của bạn.

Các loại MFA được hỗ trợ bao gồm:

* **Khóa mật khẩu và Khóa bảo mật (Passkeys and Security Keys \- FIDO):** Dựa trên tiêu chuẩn FIDO (Fast Identity Online), cung cấp xác thực mạnh mẽ, chống lừa đảo bằng cách sử dụng mật mã khóa công khai.15  
* **Ứng dụng Xác thực Ảo (Virtual Authenticator Apps \- TOTP):** Các ứng dụng này triển khai thuật toán mật khẩu dùng một lần dựa trên thời gian (TOTP).15  
* **Mã thông báo TOTP Phần cứng (Hardware TOTP Tokens):** Thiết bị vật lý cũng sử dụng thuật toán TOTP.15

MFA có thể được kích hoạt cho cả người dùng gốc của tài khoản AWS và người dùng IAM cá nhân.15 Đây là một phương pháp bảo mật tốt nhất không thể bỏ qua, đặc biệt đối với người dùng gốc và quản trị viên.14 IAM Identity Center cũng hỗ trợ các khả năng MFA.14

MFA đang phát triển theo hướng các phương pháp chống lừa đảo (phishing) hiệu quả hơn. Việc nhấn mạnh vào khóa mật khẩu và khóa bảo mật dựa trên FIDO 15 cho thấy một xu hướng hướng tới xác thực mạnh mẽ hơn, có khả năng chống lại các cuộc tấn công lừa đảo. Mặc dù các ứng dụng TOTP phổ biến, FIDO cung cấp khả năng bảo vệ vượt trội trước các cuộc tấn công tinh vi. Việc AWS cung cấp khóa bảo mật MFA miễn phí 15 càng nhấn mạnh xu hướng này. Điều này cho thấy các tổ chức nên ưu tiên MFA dựa trên FIDO khi có thể.

## **3\. Vai trò IAM: Ủy quyền An toàn và Thông tin Xác thực Tạm thời**

Vai trò IAM là một khái niệm trung tâm trong kiến trúc bảo mật của AWS, cho phép ủy quyền truy cập một cách an toàn và linh hoạt thông qua việc sử dụng thông tin xác thực tạm thời.

### **Sức mạnh của Vai trò IAM: Mục đích, Lợi ích so với Thông tin Xác thực Dài hạn**

Vai trò IAM là một định danh IAM với các quyền cụ thể, có thể được đảm nhận bởi các thực thể đáng tin cậy như người dùng, ứng dụng hoặc dịch vụ AWS.2 Điểm khác biệt cốt lõi và lợi thế lớn nhất của vai trò so với người dùng IAM là chúng cung cấp **thông tin xác thực bảo mật tạm thời**, loại bỏ sự cần thiết phải quản lý thông tin xác thực dài hạn như mật khẩu hoặc khóa truy cập.2

Chính vì lý do này, vai trò IAM được ưu tiên hơn người dùng IAM trong hầu hết các tình huống. Việc sử dụng vai trò giúp tăng cường bảo mật bằng cách giảm thiểu rủi ro liên quan đến thông tin xác thực dài hạn bị xâm phạm, cho phép quản lý quyền tập trung hơn và tránh việc nhúng thông tin xác thực cứng vào mã ứng dụng.1

Vai trò IAM về cơ bản thay đổi mô hình bảo mật từ việc xác định "bạn là ai" (người dùng tĩnh với quyền cố định) sang "bạn cần làm gì" (vai trò tạm thời cho một tác vụ cụ thể). Trong khi Người dùng IAM đại diện cho một danh tính bền vững với các quyền thường trực, Vai trò IAM, bằng cách cung cấp thông tin xác thực tạm thời cho các tác vụ cụ thể 2, chuyển trọng tâm sang các quyền cần thiết cho một hoạt động hoặc khung thời gian cụ thể. Đây là một mô hình năng động và an toàn hơn vì quyền truy cập chỉ được cấp khi cần thiết và tự động hết hạn. Điều này phù hợp với sự phát triển của IAM hướng tới quyền truy cập tạm thời và nhận biết ngữ cảnh hơn.5

### **Cơ chế Đảm nhận Vai trò: Chính sách Tin cậy và Dịch vụ Mã thông báo Bảo mật AWS (STS)**

Quá trình một thực thể đảm nhận vai trò IAM được điều chỉnh bởi hai thành phần chính: chính sách tin cậy của vai trò và Dịch vụ Mã thông báo Bảo mật AWS (STS).

* **Chính sách Tin cậy (Trust Policy):** Đây là một tài liệu chính sách JSON được đính kèm với một vai trò, xác định rõ ràng những principal nào (người dùng, vai trò khác, tài khoản AWS hoặc dịch vụ AWS) được *tin cậy* để đảm nhận vai trò đó.13 Chính sách tin cậy hoạt động như một người gác cổng, kiểm soát ai có thể "mượn" danh tính và quyền hạn của vai trò.  
* **AWS Security Token Service (STS):** Đây là một dịch vụ web cho phép yêu cầu thông tin xác thực tạm thời, có đặc quyền giới hạn cho người dùng IAM hoặc cho người dùng mà bạn xác thực (người dùng liên kết).5 STS là công cụ tạo ra các thông tin xác thực tạm thời sau khi một thực thể đã được chính sách tin cậy cho phép đảm nhận vai trò.

Các lệnh gọi API STS chính liên quan đến việc đảm nhận vai trò và lấy thông tin xác thực tạm thời bao gồm:

* **AssumeRole**: Được sử dụng bởi người dùng IAM hoặc các vai trò khác để đảm nhận một vai trò. Lệnh gọi này yêu cầu ARN của vai trò cần đảm nhận và tên phiên.13  
* **AssumeRoleWithSAML**: Dành cho người dùng được liên kết thông qua SAML 2.0. Lệnh gọi này yêu cầu ARN của nhà cung cấp SAML, ARN của vai trò và khẳng định SAML từ IdP.35  
* **AssumeRoleWithWebIdentity**: Dành cho người dùng được liên kết thông qua OpenID Connect (OIDC), ví dụ như từ các nhà cung cấp danh tính web như Google hoặc Facebook. Lệnh gọi này yêu cầu mã thông báo OIDC và ARN của vai trò.35  
* **GetSessionToken**: Được sử dụng bởi người dùng IAM (đặc biệt là những người được bảo vệ bằng MFA) để nhận thông tin xác thực tạm thời dựa trên quyền của chính họ. Thông tin xác thực này có cùng quyền với người dùng, nhưng có một số hạn chế nhất định (ví dụ: không thể gọi các API IAM mà không có MFA, và hầu hết các API STS).41  
* **GetFederationToken**: Được sử dụng bởi các ứng dụng proxy để nhận thông tin xác thực tạm thời cho các ứng dụng phân tán, được gọi bằng thông tin xác thực người dùng IAM dài hạn. Thông tin xác thực này cũng có các khả năng hạn chế (không thể gọi IAM hoặc hầu hết các API STS).44

Chính sách tin cậy của một vai trò cũng quan trọng như chính sách quyền của nó. Chính sách quyền của vai trò xác định *những gì* vai trò có thể làm, nhưng chính sách tin cậy xác định *ai* có thể đảm nhận vai trò và do đó có được những quyền đó.13 Một chính sách tin cậy được cấu hình sai (ví dụ: principal quá rộng rãi) có thể nguy hiểm như một chính sách quyền quá rộng rãi, vì nó có thể cho phép các thực thể không mong muốn leo thang đặc quyền. Điều này nhấn mạnh bản chất kép của bảo mật vai trò.

Điều quan trọng cần lưu ý là GetSessionToken và GetFederationToken là các hoạt động STS chuyên biệt, khác biệt với việc đảm nhận vai trò. Trong khi các hoạt động AssumeRole\* liên quan đến việc một thực thể đảm nhận một tập hợp quyền *khác* được xác định bởi một vai trò, GetSessionToken cung cấp thông tin xác thực tạm thời với các quyền *giống như* người dùng IAM gọi lệnh, chủ yếu cho các lệnh gọi API được bảo vệ bằng MFA.41 GetFederationToken dành cho một mẫu ứng dụng proxy kế thừa cụ thể và cũng dựa trên quyền của người dùng IAM gọi lệnh.44 Sự phân biệt này rất quan trọng vì chúng giải quyết các vấn đề khác với việc ủy quyền dựa trên vai trò chung.

**Bảng 3.1: Các Hoạt động API STS để Lấy Thông tin Xác thực Tạm thời**

| Hoạt động API | Danh tính Người gọi | Mục đích/Trường hợp Sử dụng | Thông tin Xác thực Trả về | Hạn chế Chính | Hỗ trợ MFA | Hỗ trợ Chính sách Phiên |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| AssumeRole | Người dùng IAM, Vai trò IAM khác, Dịch vụ AWS | Ủy quyền truy cập trong tài khoản hoặc giữa các tài khoản cho người dùng hoặc dịch vụ. | Khóa truy cập tạm thời, Khóa bí mật, Mã thông báo phiên. | Quyền được xác định bởi vai trò được đảm nhận. | Có thể yêu cầu MFA thông qua điều kiện trong chính sách tin cậy của vai trò.36 | Có.36 |
| AssumeRoleWithSAML | Người dùng liên kết qua SAML 2.0 (ví dụ: từ Okta, Azure AD) | Cho phép người dùng doanh nghiệp truy cập AWS bằng thông tin xác thực công ty của họ. | Khóa truy cập tạm thời, Khóa bí mật, Mã thông báo phiên. | Quyền được xác định bởi vai trò được đảm nhận. Thời lượng phiên bị giới hạn bởi khẳng định SAML hoặc cài đặt vai trò.38 | MFA thường được xử lý bởi Nhà cung cấp Danh tính (IdP) SAML. | Có.38 |
| AssumeRoleWithWebIdentity | Người dùng liên kết qua OIDC (ví dụ: từ Google, GitHub Actions) | Cho phép ứng dụng di động/web hoặc quy trình CI/CD truy cập AWS thay mặt người dùng đã xác thực. | Khóa truy cập tạm thời, Khóa bí mật, Mã thông báo phiên. | Quyền được xác định bởi vai trò được đảm nhận. Không thể gọi GetFederationToken hoặc GetSessionToken.40 | MFA thường được xử lý bởi Nhà cung cấp Danh tính (IdP) OIDC. | Có.40 |
| GetSessionToken | Người dùng IAM (thường có MFA), Người dùng gốc (không khuyến nghị) | Lấy thông tin xác thực tạm thời cho người dùng IAM, đặc biệt cho các lệnh gọi API yêu cầu MFA. 41 | Khóa truy cập tạm thời, Khóa bí mật, Mã thông báo phiên. | Kế thừa quyền của người dùng IAM gọi lệnh. Không thể gọi API IAM trừ khi có thông tin MFA; không thể gọi hầu hết API STS.41 | Bắt buộc nếu chính sách của người dùng yêu cầu MFA; người dùng phải cung cấp mã MFA hợp lệ.41 | Không. |
| GetFederationToken | Người dùng IAM (thường cho ứng dụng proxy), Người dùng gốc (không khuyến nghị) | Cung cấp thông tin xác thực tạm thời cho các ứng dụng proxy thay mặt các ứng dụng phân tán. 44 | Khóa truy cập tạm thời, Khóa bí mật, Mã thông báo phiên, ARN người dùng liên kết. | Kế thừa quyền của người dùng IAM gọi lệnh. Không thể gọi API IAM; không thể gọi hầu hết API STS.44 | Không được xử lý trực tiếp bởi lệnh gọi; dựa trên thông tin xác thực dài hạn của người dùng IAM. | Có (dưới dạng chính sách được truyền vào).44 |

Bảng này hợp nhất thông tin quan trọng về các API STS khác nhau được sử dụng để lấy thông tin xác thực tạm thời. Nó làm rõ các trường hợp sử dụng riêng biệt của chúng, bản chất của thông tin xác thực mà chúng cung cấp và các chi tiết hoạt động quan trọng như hỗ trợ MFA và chính sách phiên, điều này rất quan trọng để hiểu chi tiết.13

### **Các Trường hợp Sử dụng Chính: Phiên bản EC2, Hàm Lambda, Truy cập Dịch vụ AWS, Truy cập giữa các Tài khoản**

Vai trò IAM thể hiện tính linh hoạt và sự cần thiết trong nhiều kiến trúc AWS khác nhau:

* **Phiên bản Amazon EC2 và Hàm AWS Lambda:** Vai trò có thể được đính kèm với các phiên bản EC2 hoặc hàm Lambda để cấp cho chúng quyền truy cập các dịch vụ AWS khác mà không cần nhúng thông tin xác thực trực tiếp vào mã ứng dụng.8 Ví dụ, một phiên bản EC2 chạy ứng dụng web có thể đảm nhận một vai trò cho phép nó đọc dữ liệu từ một bucket Amazon S3.  
* **Truy cập Dịch vụ AWS:** Một số dịch vụ AWS có thể đảm nhận các vai trò dịch vụ để thực hiện các hành động thay mặt bạn. Các vai trò này có các chính sách tin cậy cho phép dịch vụ AWS cụ thể đảm nhận chúng. Một ví dụ điển hình là Vai trò Liên kết Dịch vụ (Service-Linked Roles).13  
* **Truy cập giữa các Tài khoản (Cross-Account Access):** Vai trò là phương pháp chính để cấp quyền truy cập tài nguyên trong một tài khoản AWS cho một principal đáng tin cậy trong một tài khoản khác.8 Tài khoản tin cậy tạo một vai trò với chính sách tin cậy cho phép tài khoản được tin cậy đảm nhận nó, và một chính sách quyền xác định những hành động nào có thể được thực hiện trên tài nguyên của nó.

Việc đảm nhận vai trò giữa các tài khoản là một mẫu hình cơ bản cho các kiến trúc đa tài khoản. Khi các tổ chức áp dụng chiến lược đa tài khoản để cô lập và quản trị 5, vai trò giữa các tài khoản trở thành cơ chế tiêu chuẩn để cho phép tương tác có kiểm soát giữa các tài khoản này (ví dụ: để ghi nhật ký tập trung, dịch vụ chia sẻ hoặc các mô hình hub-and-spoke). Đoạn trích 34 cung cấp một hướng dẫn chi tiết, và 50/50 thảo luận về các mẫu kiến trúc như hub-and-spoke, nhấn mạnh tầm quan trọng của nó.

## **4\. Liên kết Định danh: Kết nối các Thư mục Bên ngoài với AWS**

Liên kết định danh trong AWS cho phép người dùng sử dụng thông tin xác thực hiện có của công ty (hoặc từ các nhà cung cấp danh tính web) để truy cập AWS mà không cần tạo người dùng IAM cho mọi người.13 Nó dựa trên các tiêu chuẩn như SAML 2.0 và OpenID Connect (OIDC) 37 và liên quan đến một Nhà cung cấp Danh tính (IdP) và AWS với tư cách là Nhà cung cấp Dịch vụ (SP).37

Liên kết giúp đơn giản hóa việc quản lý người dùng, tăng cường bảo mật bằng cách tận dụng các hệ thống danh tính và MFA hiện có, đồng thời cải thiện trải nghiệm người dùng với Đăng nhập Một lần (SSO). Một lợi ích quan trọng của việc liên kết là nó chuyển gánh nặng quản lý danh tính từ AWS sang các IdP bên ngoài. Bằng cách liên kết, các tổ chức tận dụng các hệ thống quản lý danh tính hiện có, thường đã trưởng thành của họ (như Active Directory, Okta, Azure AD).37 AWS IAM sau đó tập trung vào việc *ủy quyền* cho các danh tính được xác thực bên ngoài này thay vì quản lý vòng đời của chúng (tạo, mật khẩu, v.v.). Việc tách biệt các mối quan tâm này là một lợi ích chính, giúp giảm chi phí quản trị trong AWS.

### **Liên kết SAML 2.0**

SAML (Security Assertion Markup Language) 2.0 là một tiêu chuẩn mở được nhiều nhà cung cấp danh tính (IdP) sử dụng để cho phép đăng nhập một lần (SSO) liên kết. AWS hỗ trợ liên kết dựa trên SAML 2.0, cho phép người dùng từ thư mục công ty của bạn (ví dụ: Active Directory thông qua ADFS, Okta, Azure AD) đăng nhập vào Bảng điều khiển quản lý AWS hoặc gọi các hoạt động API AWS mà không cần tạo người dùng IAM cho mỗi người.37

**Luồng Xác thực và Thiết lập Tin cậy:**

1. **Người dùng Khởi tạo Xác thực:** Người dùng trong tổ chức của bạn cố gắng truy cập tài nguyên AWS hoặc Bảng điều khiển Quản lý AWS.  
2. **Chuyển hướng đến IdP:** Dịch vụ AWS chuyển hướng trình duyệt hoặc ứng dụng của người dùng đến IdP tương thích SAML 2.0 của tổ chức bạn để xác thực.  
3. **Xác thực IdP:** IdP xác thực người dùng dựa trên kho lưu trữ danh tính của tổ chức bạn (ví dụ: Active Directory).  
4. **Tạo Khẳng định SAML:** Sau khi xác thực thành công, IdP tạo một khẳng định SAML. Đây là một tài liệu XML chứa thông tin về người dùng, chẳng hạn như danh tính và các thuộc tính của họ (ví dụ: tư cách thành viên nhóm, vai trò). Nếu mã hóa SAML được bật, khẳng định này sẽ được IdP bên ngoài của bạn mã hóa.37  
5. **Truyền Khẳng định SAML:** IdP gửi khẳng định SAML trở lại trình duyệt hoặc ứng dụng của người dùng.  
6. **Gọi API AssumeRoleWithSAML của AWS STS:** Trình duyệt hoặc ứng dụng của người dùng sau đó gọi API AssumeRoleWithSAML của Dịch vụ Mã thông báo Bảo mật AWS (STS). Lệnh gọi này bao gồm ARN của nhà cung cấp SAML bạn đã cấu hình trong IAM, ARN của vai trò IAM mà người dùng đang yêu cầu đảm nhận và khẳng định SAML nhận được từ IdP. Nếu mã hóa được bật, khẳng định vẫn được mã hóa trong quá trình truyền.37  
7. **(Tùy chọn) Giải mã Khẳng định SAML:** AWS STS có thể tùy chọn sử dụng khóa riêng bạn đã tải lên từ IdP bên ngoài để giải mã khẳng định SAML được mã hóa.37  
8. **Cấp Thông tin Xác thực Bảo mật Tạm thời:** Nếu khẳng định SAML hợp lệ và người dùng được ủy quyền đảm nhận vai trò được chỉ định, AWS STS sẽ trả về một bộ thông tin xác thực bảo mật tạm thời (ID khóa truy cập, khóa truy cập bí mật và mã thông báo phiên) cho trình duyệt hoặc ứng dụng của người dùng.37  
9. **Truy cập Tài nguyên AWS:** Trình duyệt hoặc ứng dụng của người dùng sau đó có thể sử dụng các thông tin xác thực bảo mật tạm thời này để thực hiện các yêu cầu đã ký tới các dịch vụ AWS. Các thông tin xác thực tạm thời này có các quyền được xác định bởi vai trò IAM đã được đảm nhận.37

Thiết lập Quan hệ Tin cậy:  
Quan hệ tin cậy được thiết lập thông qua một quy trình cấu hình trong đó cả IdP và AWS đều được thông báo về nhau:

1. **Đăng ký AWS làm Nhà cung cấp Dịch vụ (SP) với IdP:** Bạn cung cấp cho IdP siêu dữ liệu về AWS với tư cách là nhà cung cấp dịch vụ, thường sử dụng tài liệu siêu dữ liệu SAML do AWS cung cấp.37  
2. **Đăng ký IdP làm Nhà cung cấp Danh tính SAML trong IAM:** Trong bảng điều khiển AWS IAM, bạn tạo một nhà cung cấp danh tính SAML và tải lên tệp XML siêu dữ liệu SAML do IdP của tổ chức bạn tạo. Siêu dữ liệu này mô tả IdP của bạn, bao gồm tên nhà phát hành, ngày hết hạn và các khóa công khai mà AWS có thể sử dụng để xác thực chữ ký của các khẳng định SAML đến từ IdP của bạn. Nếu sử dụng khẳng định SAML được mã hóa, bạn cũng tải khóa giải mã riêng do IdP của bạn tạo lên cấu hình IAM SAML của mình.37  
3. **Tạo Vai trò IAM với Chính sách Tin cậy:** Trong IAM, bạn tạo một hoặc nhiều vai trò IAM mà người dùng liên kết của bạn sẽ đảm nhận. Phần quan trọng là chính sách tin cậy của vai trò. Trong chính sách này, bạn chỉ định ARN của nhà cung cấp danh tính SAML bạn đã tạo ở bước trước làm Principal. Điều này thiết lập mối quan hệ tin cậy, cho phép người dùng được IdP của bạn xác thực đảm nhận vai trò này. Bạn cũng xác định các hành động (sts:AssumeRoleWithSAML) mà nhà cung cấp SAML được ủy quyền thực hiện trên vai trò này. Ngoài ra, bạn có thể sử dụng các yếu tố Condition trong chính sách tin cậy để hạn chế thêm những người có thể đảm nhận vai trò dựa trên các thuộc tính trong khẳng định SAML, chẳng hạn như saml:aud (khán giả) và saml:iss (nhà phát hành).37

Ánh xạ Thuộc tính SAML:  
Các thuộc tính (như tư cách thành viên nhóm, phòng ban) được truyền trong khẳng định SAML 37 không chỉ dùng để nhận dạng. Chúng được AWS chủ động sử dụng để ánh xạ người dùng tới các Vai trò IAM thích hợp và để cung cấp thông tin cho các chính sách ABAC.7 Điều này cho phép cấp quyền năng động và nhận biết ngữ cảnh hơn so với việc gán vai trò tĩnh. Ví dụ, thuộc tính phòng ban của người dùng từ IdP có thể xác định vai trò AWS mà họ đảm nhận và những tài nguyên nào họ có thể truy cập dựa trên các thẻ phù hợp.

### **Liên kết OpenID Connect (OIDC)**

OpenID Connect (OIDC) là một lớp nhận dạng đơn giản dựa trên giao thức OAuth 2.0. AWS IAM hỗ trợ liên kết OIDC, cho phép các ứng dụng (đặc biệt là ứng dụng di động và web) xác thực người dùng thông qua các nhà cung cấp danh tính tương thích OIDC (ví dụ: Google, Facebook, hoặc các nhà cung cấp OIDC tùy chỉnh) và sau đó trao đổi mã thông báo OIDC để lấy thông tin xác thực AWS tạm thời.13

**Luồng Xác thực và Trao đổi Mã thông báo OIDC:**

1. **Xác thực với Nhà cung cấp OIDC:** Ứng dụng của bạn (chạy bên ngoài AWS) khởi tạo quy trình xác thực với nhà cung cấp OIDC đã chọn. Người dùng xác thực với nhà cung cấp OIDC bằng thông tin xác thực hiện có của họ. Sau khi thành công, nhà cung cấp OIDC cấp một mã thông báo xác thực dưới dạng JSON Web Token (JWT). JWT này chứa các xác nhận quyền sở hữu (claims) về danh tính của người dùng.39  
2. **Lấy Mã thông báo OIDC:** Ứng dụng của bạn nhận JWT từ nhà cung cấp OIDC.  
3. **Thiết lập Tin cậy:** Để cho phép trao đổi này, bạn cần thiết lập mối quan hệ tin cậy giữa tài khoản AWS của mình và nhà cung cấp OIDC. Điều này được thực hiện bằng cách tạo một nhà cung cấp danh tính OpenID Connect (OIDC) trong AWS IAM. Khi tạo nhà cung cấp danh tính OIDC trong IAM, bạn cung cấp URL của nhà cung cấp OIDC, "khán giả" (audience) hoặc (các) ID khách hàng mà ứng dụng của bạn sẽ sử dụng, và (các) dấu vân tay (thumbprints) của (các) chứng chỉ máy chủ của IdP.39  
4. **Yêu cầu Thông tin Xác thực AWS Tạm thời:** Ứng dụng của bạn sau đó gửi JWT nhận được từ nhà cung cấp OIDC đến AWS, thường bằng cách gọi hoạt động API AssumeRoleWithWebIdentity. Trong lệnh gọi API này, ứng dụng của bạn chỉ định JWT, ARN của vai trò IAM mà bạn đã cấu hình để tin cậy nhà cung cấp danh tính OIDC, và tùy chọn là tên phiên.39  
5. **Xác thực Mã thông báo OIDC:** AWS nhận JWT và xác thực nó dựa trên nhà cung cấp danh tính OIDC đã cấu hình. Việc xác thực này bao gồm việc xác minh chữ ký của JWT, kiểm tra xác nhận quyền sở hữu issuer và audience, và kiểm tra xác nhận quyền sở hữu exp (thời gian hết hạn).39  
6. **Trao đổi lấy Thông tin Xác thực Tạm thời:** Nếu JWT được xác thực thành công, AWS sẽ cấp một bộ thông tin xác thực bảo mật tạm thời (ID khóa truy cập, khóa truy cập bí mật và mã thông báo phiên). Các thông tin xác thực tạm thời này có các quyền được xác định bởi vai trò IAM được chỉ định trong yêu cầu AssumeRoleWithWebIdentity.39  
7. **Truy cập Tài nguyên AWS:** Ứng dụng của bạn sau đó có thể sử dụng các thông tin xác thực AWS tạm thời này để thực hiện các yêu cầu đã ký tới các dịch vụ AWS. Các thông tin xác thực tạm thời này có thời hạn sử dụng ngắn, giúp tăng cường bảo mật.39

Liên kết OIDC rất quan trọng đối với "IAM Roles Anywhere" và quyền truy cập ứng dụng an toàn từ bên ngoài AWS. Luồng AssumeRoleWithWebIdentity 39 là nền tảng cho các ứng dụng chạy *bên ngoài* AWS 39 để lấy thông tin xác thực AWS tạm thời. Mẫu này cho phép các khối lượng công việc bên ngoài hệ sinh thái AWS tương tác an toàn với các dịch vụ AWS mà không cần nhúng thông tin xác thực dài hạn. Mã thông báo OIDC hoạt động như một bằng chứng xác thực bởi một bên thứ ba đáng tin cậy bên ngoài.

**Bảng 4.1: So sánh các Giao thức Liên kết: SAML 2.0 và OIDC**

| Tính năng | SAML 2.0 | OpenID Connect (OIDC) |
| :---- | :---- | :---- |
| **Trường hợp Sử dụng Chính** | Liên kết doanh nghiệp (Enterprise Federation), SSO cho người dùng công ty.37 | Liên kết danh tính web (Web Identity Federation), ứng dụng di động và web.39 |
| **Khẳng định/Mã thông báo Danh tính** | Khẳng định SAML (Tài liệu XML).37 | Mã thông báo OIDC (JWT \- JSON Web Token).39 |
| **Các IdP Chính** | Microsoft ADFS, Okta, Azure AD, Ping Identity.37 | Google, Facebook, Amazon Cognito, GitHub Actions (với tư cách là nhà cung cấp OIDC), các nhà cung cấp OIDC tùy chỉnh.39 |
| **Cơ chế Tin cậy trong IAM** | Tạo Nhà cung cấp Danh tính SAML trong IAM (tải lên siêu dữ liệu IdP).37 | Tạo Nhà cung cấp Danh tính OIDC trong IAM (cung cấp URL nhà cung cấp, khán giả, dấu vân tay).39 |
| **API STS Liên quan** | AssumeRoleWithSAML.35 | AssumeRoleWithWebIdentity.35 |

Bảng này phân định rõ ràng hai giao thức liên kết chính, giúp người dùng lựa chọn giao thức phù hợp dựa trên IdP và trường hợp sử dụng của họ. Điều này trực tiếp giải quyết nhu cầu về sự rõ ràng đối với các cơ chế liên kết phức tạp nhưng quan trọng này.13

## **5\. Các Chiến lược Kiểm soát Truy cập Nâng cao trong IAM**

Ngoài các thành phần cơ bản, AWS IAM cung cấp các cơ chế kiểm soát truy cập nâng cao để giải quyết các yêu cầu bảo mật và quản trị phức tạp hơn. Các chiến lược này cho phép các tổ chức triển khai các mô hình ủy quyền chi tiết, linh hoạt và có khả năng mở rộng.

### **Giới hạn Quyền (Permissions Boundaries): Ủy quyền Quản trị IAM An toàn và Ngăn chặn Leo thang Đặc quyền**

Giới hạn quyền là một tính năng IAM cho phép các nhóm IAM trung tâm trao quyền cho các nhà phát triển tạo vai trò và chính sách mới một cách an toàn trong AWS.3 Chúng hoạt động như một lan can bảo vệ, đặt ra các quyền tối đa mà một principal IAM (như người dùng hoặc vai trò) có thể có.3 Điều này đảm bảo rằng ngay cả khi một nhà phát triển tạo ra một chính sách rất rộng rãi, các quyền thực tế có hiệu lực sẽ không vượt quá giới hạn đã được xác định.

Các quyền hiệu quả của một principal được đính kèm giới hạn quyền là sự *giao nhau* giữa các chính sách dựa trên định danh của nó và giới hạn quyền.3 Điều này có nghĩa là một hành động chỉ được phép nếu nó được cho phép bởi *cả* chính sách dựa trên định danh *và* giới hạn quyền. Một từ chối rõ ràng trong một trong hai sẽ ghi đè lên bất kỳ sự cho phép nào.

Mục đích chính của giới hạn quyền là để ủy quyền khả năng tạo và quản lý vai trò IAM cho các nhà phát triển (được gọi là quản trị viên được ủy quyền).3 Các nhà phát triển thường cần tạo vai trò và chính sách IAM cho ứng dụng của họ. Giới hạn quyền giải quyết vấn đề này bằng cách cho phép các nhà phát triển tự tạo các vai trò cần thiết, trong khi vẫn duy trì sự giám sát bảo mật. Ví dụ, một quản trị viên trung tâm có thể đính kèm một điều kiện vào chính sách IAM của nhà phát triển, cho phép họ tạo vai trò chỉ khi một chính sách giới hạn quyền được đính kèm với nó. Điều này đảm bảo rằng bất kỳ vai trò nào mà nhà phát triển tạo ra sẽ có các quyền bị giới hạn bởi ranh giới đã xác định.3

Giới hạn quyền ngăn chặn hiệu quả việc leo thang đặc quyền. Ngay cả khi một nhà phát triển vô tình hoặc cố ý bao gồm các quyền quá rộng rãi trong chính sách IAM của vai trò, giới hạn quyền đính kèm sẽ ngăn chặn các quyền nâng cao đó có hiệu lực nếu chúng nằm ngoài phạm vi của giới hạn.3

Các phương pháp hay nhất khi sử dụng giới hạn quyền bao gồm việc áp dụng chúng cho các vai trò IAM thay vì trực tiếp cho các nhà phát triển (do giới hạn không gian chính sách đối với người dùng), chỉ sử dụng các câu lệnh Allow trong chính sách giới hạn quyền và tránh sử dụng các điều kiện phức tạp trong chính sách giới hạn quyền, thay vào đó nên đặt chúng trong các loại chính sách khác.3

Giới hạn quyền cho phép một mô hình "tin cậy nhưng xác minh" cho việc ủy quyền IAM. Các quản trị viên trung tâm *tin cậy* các nhà phát triển tạo vai trò và chính sách cho ứng dụng của họ.3 Tuy nhiên, giới hạn quyền hoạt động như một cơ chế *xác minh*, đảm bảo rằng quyền hạn được ủy quyền này không thể bị lạm dụng để tạo ra các vai trò quá rộng rãi có thể dẫn đến leo thang đặc quyền.3 Điều này cho phép quản lý IAM phân tán mà không làm mất đi sự kiểm soát trung tâm đối với các quyền tối đa được phép. Điều quan trọng là logic "giao nhau" (intersection) là chìa khóa để hiểu cách giới hạn quyền thực thi. Một giới hạn quyền *riêng lẻ không cấp bất kỳ quyền nào*.3 Nó chỉ xác định *tối đa* được phép. Một hành động chỉ được phép nếu nó được cho phép bởi *cả* chính sách dựa trên định danh VÀ giới hạn quyền.3 Logic giao nhau này là nền tảng cho cách giới hạn quyền ngăn chặn leo thang đặc quyền.

### **Kiểm soát Truy cập Dựa trên Thuộc tính (ABAC): Quyền Linh hoạt và Có Khả năng Mở rộng**

Kiểm soát Truy cập Dựa trên Thuộc tính (ABAC) là một chiến lược ủy quyền xác định các quyền dựa trên các thuộc tính. Trong AWS, các thuộc tính này thường được triển khai dưới dạng thẻ (tags).7 Các thuộc tính người dùng từ IdP (thông qua IAM Identity Center hoặc thẻ phiên SAML) hoặc thẻ principal IAM được sử dụng làm thẻ phiên.7 Các chính sách cho phép các hoạt động khi thẻ của principal khớp với thẻ tài nguyên.7

Các khóa điều kiện (Condition Keys) quan trọng cho ABAC bao gồm aws:ResourceTag/key-name, aws:PrincipalTag/key-name 69, aws:RequestTag/key-name, và aws:TagKeys.61 Đối với KMS, có các khóa cụ thể như kms:ResourceAliases và kms:RequestAlias.62

Những lợi ích của ABAC bao gồm việc yêu cầu ít bộ quyền/vai trò hơn, khả năng mở rộng theo sự phát triển, tận dụng các thuộc tính nhân viên hiện có và cải thiện khả năng theo dõi.7 ABAC lý tưởng cho các môi trường phát triển nhanh và các tình huống quản lý chính sách phức tạp.7

**Bảng 5.1: So sánh ABAC và RBAC trong AWS**

| Tính năng | Kiểm soát Truy cập Dựa trên Vai trò (RBAC) | Kiểm soát Truy cập Dựa trên Thuộc tính (ABAC) |
| :---- | :---- | :---- |
| **Mức độ chi tiết (Granularity)** | Quyền được gắn với vai trò được xác định trước.7 | Quyền chi tiết dựa trên nhiều thuộc tính (người dùng, tài nguyên, môi trường).7 |
| **Khả năng mở rộng (Scalability)** | Có thể dẫn đến "bùng nổ vai trò" khi số lượng quyền riêng biệt tăng lên.7 | Khả năng mở rộng tốt hơn; số lượng chính sách ít hơn khi các thuộc tính thay đổi.7 |
| **Quyền động (Dynamic Permissions)** | Tương đối tĩnh; thay đổi quyền thường yêu cầu thay đổi vai trò hoặc chính sách vai trò. | Rất linh hoạt; quyền tự động điều chỉnh dựa trên thay đổi thuộc tính của người dùng hoặc tài nguyên.60 |
| **Chi phí Quản lý (Vai trò/Chính sách)** | Quản lý nhiều vai trò khi yêu cầu chi tiết tăng lên.7 | Quản lý thuộc tính (thẻ) và một số lượng chính sách ABAC ít hơn nhưng có thể phức tạp hơn.7 |
| **Độ phức tạp (Complexity)** | Đơn giản hơn để hiểu và triển khai ban đầu.63 | Phức tạp hơn để thiết kế và quản lý chính sách ban đầu; đòi hỏi chiến lược gắn thẻ mạnh mẽ.7 |
| **Trường hợp Sử dụng Lý tưởng** | Môi trường có vai trò công việc được xác định rõ ràng, ít thay đổi; tổ chức nhỏ hơn.63 | Môi trường phát triển nhanh, yêu cầu quyền chi tiết, nhiều dự án/nhóm, tuân thủ quy định nghiêm ngặt.7 |
| **Cơ chế Triển khai AWS Chính** | Người dùng IAM, Nhóm IAM, Vai trò IAM với các chính sách dựa trên định danh được đính kèm trực tiếp. | Thẻ trên principal IAM và tài nguyên; điều kiện trong chính sách IAM sử dụng aws:PrincipalTag, aws:ResourceTag, aws:RequestTag; thuộc tính người dùng từ IAM Identity Center.60 |

Bảng này cung cấp một so sánh ngắn gọn về ABAC và RBAC trong bối cảnh AWS, làm nổi bật điểm mạnh, điểm yếu của chúng và cách chúng được triển khai bằng các tính năng IAM. Điều này giải quyết một điểm quan tâm chung và có khả năng gây nhầm lẫn cho người dùng khi thiết kế chiến lược kiểm soát truy cập.7

ABAC chuyển logic cấp quyền từ "Người dùng là ai?" (Vai trò) sang "Đặc điểm của người dùng, tài nguyên và môi trường là gì?" (Thuộc tính). RBAC chủ yếu gắn quyền với (các) vai trò được gán cho người dùng.7 ABAC, bằng cách sử dụng các thuộc tính (thẻ) của principal, tài nguyên và đôi khi là môi trường/yêu cầu 7, đưa ra quyết định truy cập dựa trên một tập hợp thông tin theo ngữ cảnh phong phú hơn. Điều này cho phép các chính sách chi tiết và năng động hơn nhiều, có thể thích ứng với những thay đổi trong các thuộc tính này mà không cần gán lại vai trò hoặc viết lại chính sách. Ví dụ, quyền truy cập vào một bucket S3 "ProjectX" có thể được cấp nếu thẻ Project của người dùng là "ProjectX" và thẻ Project của bucket S3 cũng là "ProjectX".7

Tuy nhiên, ABAC hiệu quả phụ thuộc rất nhiều vào chiến lược gắn thẻ nhất quán và quản lý thuộc tính mạnh mẽ. Vì các chính sách ABAC phụ thuộc vào các thẻ khớp nhau 7, một chiến lược gắn thẻ được xác định rõ ràng và được thực thi nhất quán cho cả principal và tài nguyên là điều tối quan trọng. Các thẻ không nhất quán hoặc bị thiếu sẽ phá vỡ logic ABAC. Tương tự, nếu các thuộc tính người dùng được lấy từ một IdP bên ngoài 37, tính chính xác và kịp thời của các thuộc tính này trong IdP là rất quan trọng.7 Điều này tạo ra sự phụ thuộc vào các quy trình bên ngoài cấu hình IAM trực tiếp.

### **Vai trò Liên kết Dịch vụ (SLRs): Quyền được Xác định Trước cho Dịch vụ AWS**

Vai trò Liên kết Dịch vụ (SLR) là một loại vai trò IAM duy nhất được liên kết trực tiếp với một dịch vụ AWS.49 Chúng được dịch vụ xác định trước và bao gồm tất cả các quyền mà dịch vụ yêu cầu để gọi các dịch vụ AWS khác thay mặt bạn.49 Điều này giúp đơn giản hóa việc thiết lập vì không cần thêm quyền thủ công.49

Dịch vụ kiểm soát các chính sách đính kèm và thời điểm vai trò có thể bị xóa.72 Chính sách quyền của SLR không thể được đính kèm với các thực thể IAM khác.71 SLR thường được tạo tự động bởi dịch vụ khi bạn thực hiện một hành động cụ thể trong dịch vụ đó.71 Quy ước đặt tên thường được xác định trước, ví dụ: AWSServiceRoleFor\<ServiceName\>.71 Các quyền của SLR không thể sửa đổi và việc xóa chúng cũng bị hạn chế (phải xóa các tài nguyên liên quan trước).49

SLR đại diện cho một hình thức "ủy quyền được quản lý" từ người dùng cho các dịch vụ AWS. Thay vì người dùng tự tạo vai trò và xây dựng chính sách cho các dịch vụ, các dịch vụ AWS tự xác định và quản lý các SLR này.49 Đây là sự ủy quyền quản lý quyền *cho dịch vụ*, đảm bảo dịch vụ có chính xác những gì nó cần và ngăn người dùng vô tình phá vỡ chức năng dịch vụ bằng cách sửa đổi hoặc xóa các quyền quan trọng. Các ràng buộc về sửa đổi và xóa 49 củng cố bản chất được quản lý này.

### **Chính sách Phiên (Session Policies): Kiểm soát Chi tiết cho các Phiên Vai trò Tạm thời**

Chính sách phiên là các chính sách nâng cao được sử dụng khi đảm nhận một vai trò (thông qua AssumeRole, AssumeRoleWithSAML, và AssumeRoleWithWebIdentity) hoặc cho người dùng liên kết.26 Chúng có thể được truyền dưới dạng một tài liệu chính sách JSON nội tuyến duy nhất hoặc dưới dạng ARN của tối đa 10 chính sách được quản lý.36

Các quyền của phiên kết quả là sự *giao nhau* giữa chính sách dựa trên định danh của vai trò và các chính sách phiên.26 Điều quan trọng là chính sách phiên không thể được sử dụng để cấp nhiều quyền hơn những gì được phép bởi chính sách dựa trên định danh của vai trò đang được đảm nhận.36 Có một giới hạn về kích thước văn bản thuần túy (ví dụ: 2048 ký tự) cho các chính sách này.36

Chính sách phiên cung cấp một cơ chế để giới hạn động các quyền cho một phiên tạm thời cụ thể, cho phép kiểm soát chi tiết hơn so với chỉ các quyền tĩnh của vai trò. Chúng hoạt động như một "giới hạn quyền động" cho một phiên duy nhất. Tương tự như cách giới hạn quyền đặt ra các quyền tối đa cho một vai trò trong suốt vòng đời của nó, chính sách phiên đặt ra các quyền tối đa cho một *phiên đảm nhận vai trò cụ thể*.26 Chúng được áp dụng tại thời điểm đảm nhận vai trò và hạn chế các quyền của vai trò được đảm nhận chỉ trong phiên đó. Điều này rất hữu ích cho các tình huống mà bạn muốn cấp cho một vai trò các quyền rộng rãi nói chung, nhưng hạn chế những gì một người dùng hoặc ứng dụng cụ thể có thể làm với vai trò đó trong một ngữ cảnh cụ thể hoặc trong một thời gian giới hạn, mà không cần tạo nhiều vai trò có phạm vi hẹp. Logic "giao nhau" 26 đảm bảo chúng chỉ thu hẹp, không bao giờ mở rộng, các quyền.

## **6\. Quản trị IAM trên Toàn bộ Tổ chức AWS của Bạn**

Khi các tổ chức mở rộng quy mô sử dụng AWS, việc quản lý danh tính và quyền truy cập trên nhiều tài khoản trở thành một thách thức quan trọng. AWS Organizations và các tính năng liên quan cung cấp các công cụ để quản trị IAM một cách tập trung và nhất quán.

### **AWS Organizations: Quản lý Tập trung cho Môi trường Đa tài khoản**

AWS Organizations giúp bạn quản lý và điều hành tập trung môi trường của mình khi bạn mở rộng quy mô tài nguyên AWS.77 Nó cho phép tạo tài khoản, nhóm các tài khoản thành các Đơn vị Tổ chức (OU), áp dụng các chính sách để quản trị và đơn giản hóa việc thanh toán.77 Đây là một phương pháp thực hành tốt nhất được khuyến nghị để mở rộng quy mô.77

AWS Organizations là yếu tố hỗ trợ cho việc quản trị IAM tập trung ở quy mô lớn. Nhiều tính năng quản trị IAM nâng cao, đặc biệt là Chính sách Kiểm soát Dịch vụ (SCP) 77, và việc triển khai hiệu quả IAM Identity Center 5, đều phụ thuộc vào cấu trúc AWS Organizations. Nó cung cấp khung phân cấp (gốc, OU, tài khoản) cần thiết để áp dụng các lan can bảo mật nhất quán và quản lý danh tính trên toàn doanh nghiệp. Nếu không có Organizations, việc quản lý IAM trong một thiết lập đa tài khoản sẽ trở nên phức tạp hơn đáng kể và dễ bị thiếu nhất quán.5

### **Chính sách Kiểm soát Dịch vụ (SCPs): Thực thi Lan can Quyền trên các Tài khoản**

Chính sách Kiểm soát Dịch vụ (SCPs) là một loại chính sách của tổ chức mà bạn có thể sử dụng để quản lý các quyền tối đa có sẵn cho người dùng và vai trò IAM trong các tài khoản thành viên của tổ chức bạn.77 Chúng hoạt động như những lan can quyền, đảm bảo rằng các tài khoản tuân thủ các nguyên tắc kiểm soát truy cập của tổ chức.77 SCPs không cấp quyền; thay vào đó, chúng xác định giới hạn trên về những hành động mà người dùng và vai trò IAM trong các tài khoản thành viên có thể thực hiện.

SCPs áp dụng cho mọi người dùng và vai trò trong các tài khoản thành viên, *bao gồm cả người dùng gốc* của tài khoản thành viên đó.78 Tuy nhiên, SCPs *không* ảnh hưởng đến người dùng hoặc vai trò trong **tài khoản quản lý** của AWS Organization.78 Các quyền hiệu quả là kết quả của sự giao nhau logic giữa những gì được phép bởi tất cả các SCP hiện hành và các chính sách dựa trên định danh và dựa trên tài nguyên.26 Một sự từ chối rõ ràng trong một SCP sẽ ghi đè lên bất kỳ sự cho phép nào.26 SCPs cũng có thể được sử dụng để hạn chế quyền truy cập vào các dịch vụ và Khu vực AWS.77

SCPs xác định "giới hạn bên ngoài" của những gì có thể thực hiện được trong các tài khoản thành viên. Chúng hoạt động như một bộ lọc *trước khi* các quyền IAM được đánh giá trong một tài khoản.26 Nếu một SCP từ chối một hành động (ví dụ: iam:CreateUser hoặc quyền truy cập vào một khu vực cụ thể), không có chính sách IAM nào trong tài khoản thành viên có thể ghi đè lên sự từ chối đó, ngay cả đối với người dùng gốc của tài khoản thành viên.78 Điều này làm cho SCPs trở thành cơ quan có thẩm quyền cuối cùng để đặt ra các hạn chế trên toàn tổ chức, đảm bảo tuân thủ và ngăn chặn các cấu hình rủi ro ở cấp độ tài khoản.

Sự miễn nhiễm của tài khoản quản lý khỏi SCPs là một lựa chọn thiết kế quan trọng cho việc kiểm soát tổ chức. Tài khoản quản lý phải duy trì đầy đủ khả năng để quản lý chính tổ chức, bao gồm cả SCPs. Nếu SCPs có thể hạn chế tài khoản quản lý, điều đó có thể dẫn đến các tình huống bị khóa hoặc không thể quản trị tổ chức một cách hiệu quả.78 Tuy nhiên, sự miễn nhiễm này cũng có nghĩa là bản thân tài khoản quản lý phải được bảo mật đặc biệt tốt.

### **Các Mẫu Kiến trúc cho Truy cập Đa tài khoản**

Việc lựa chọn mẫu truy cập đa tài khoản phù hợp là rất quan trọng để cân bằng giữa bảo mật, khả năng quản lý và hiệu quả hoạt động.

* Vai trò IAM giữa các Tài khoản: Mô hình Hub-and-Spoke và các Mô hình Khác  
  Vai trò IAM là phương pháp chính để ủy quyền truy cập giữa các tài khoản.13  
  * **Mô hình Hub-and-Spoke (Tài khoản Định danh Tập trung):** Trong mô hình này, một quan hệ tin cậy SAML được thiết lập với một tài khoản "định danh" duy nhất (hub). Người dùng từ tài khoản này sau đó đảm nhận các vai trò giữa các tài khoản để truy cập vào các tài khoản "spoke". Điều này đơn giản hóa việc quản lý liên kết nhưng đòi hỏi quản trị mạnh mẽ cho các tài khoản spoke.50 Các trường hợp sử dụng điển hình bao gồm các tài khoản sandbox/thử nghiệm tồn tại trong thời gian ngắn hoặc các tài khoản sinh viên dựa trên dự án.50  
  * **Liên kết IAM Đa tài khoản (Tin cậy SAML Phân tán):** Một quan hệ tin cậy SAML riêng biệt được thiết lập với mỗi tài khoản. Quyền được quản lý riêng cho từng tài khoản. Mô hình này linh hoạt hơn cho các quyền riêng biệt theo từng tài khoản, nhưng các nhà cung cấp SAML phải được quản lý trong mỗi tài khoản.50  
  * **Mô hình Push-based và Pull-based cho kho lưu trữ mô hình trong hub-and-spoke:** Đối với các trường hợp như kho lưu trữ mô hình SageMaker, có thể sử dụng hai cách tiếp cận. Push-based: tài khoản spoke ghi trực tiếp vào hub (đơn giản hơn, nhưng spoke cần quyền ghi). Pull-based: hub đọc từ spoke (spoke không có quyền truy cập vào hub, an toàn hơn nhưng phức tạp hơn).51

Việc lựa chọn mẫu liên kết đa tài khoản (tập trung so với phân tán) phụ thuộc vào mô hình tin cậy và khả năng chấp nhận chi phí quản trị của tổ chức. Một tài khoản định danh tập trung (hub-and-spoke cho liên kết) đơn giản hóa cấu hình IdP (một mối quan hệ tin cậy) nhưng tập trung hóa việc ánh xạ vai trò và đòi hỏi quản lý vai trò giữa các tài khoản cẩn thận.50 Các mối quan hệ tin cậy SAML phân tán mang lại nhiều quyền tự chủ hơn cho mỗi tài khoản trong việc xác định vai trò nhưng làm tăng chi phí quản lý nhiều cấu hình IdP.50 Mô hình "push vs. pull" cho SageMaker 51 tiếp tục minh họa điều này: push đơn giản hơn nhưng cấp nhiều tin cậy hơn cho các spoke, trong khi pull cô lập hơn nhưng phức tạp hơn. Quyết định phụ thuộc vào nơi tổ chức muốn đặt niềm tin và nỗ lực quản trị.

* Cân nhắc cho Tài khoản Ghi nhật ký và Công cụ Bảo mật Tập trung  
  Truy cập giữa các tài khoản rất hữu ích cho việc ghi nhật ký tập trung.52 Một Tài khoản Lưu trữ Nhật ký (Log Archive account) là một mẫu phổ biến 81, tương tự như một Tài khoản Công cụ Bảo mật (Security Tooling account).81 CloudTrail có thể được cấu hình để ghi nhật ký trên toàn tổ chức vào một bucket S3 trung tâm trong tài khoản quản lý hoặc một tài khoản lưu trữ nhật ký chuyên dụng.52 AWS Config cũng có thể được tập trung hóa.52 Vai trò IAM được sử dụng để cấp các quyền giữa các tài khoản cần thiết cho các dịch vụ này.52  
  Việc tập trung hóa dữ liệu và công cụ bảo mật là một phương pháp thực hành tốt nhất để tăng cường khả năng hiển thị và quản trị trong môi trường đa tài khoản. Các dịch vụ bảo mật tập trung tạo ra một mô hình "hub-and-spoke ngược" cho việc thu thập dữ liệu. Trong khi quyền truy cập của người dùng có thể tuân theo mô hình hub-and-spoke nơi người dùng trong một IdP trung tâm truy cập các tài khoản spoke, việc ghi nhật ký tập trung 52 và các công cụ bảo mật thường liên quan đến việc các tài khoản spoke *gửi dữ liệu đến* hoặc *được truy cập bởi* một tài khoản lưu trữ nhật ký/bảo mật trung tâm. Điều này đòi hỏi các vai trò giữa các tài khoản được cấu hình cẩn thận, cấp cho các dịch vụ trung tâm (ví dụ: CloudTrail trong tài khoản quản lý, các công cụ bảo mật trong tài khoản công cụ bảo mật) quyền đọc/ghi dữ liệu từ/đến các tài khoản spoke. Đây là một yếu tố hỗ trợ quan trọng cho khả năng hiển thị trên toàn tổ chức và ứng phó sự cố.

## **7\. AWS IAM Identity Center: Hiện đại hóa Quyền truy cập cho Lực lượng Lao động**

AWS IAM Identity Center (trước đây là AWS Single Sign-On) là giải pháp được AWS khuyến nghị để quản lý quyền truy cập của lực lượng lao động vào các tài khoản và ứng dụng AWS, giúp đơn giản hóa nhiều sự phức tạp của IAM truyền thống và liên kết trong các thiết lập đa tài khoản.

### **Sự phát triển từ AWS SSO: Lợi ích và Kiến trúc**

IAM Identity Center là sự kế thừa của AWS SSO.5 Nó cung cấp một nơi trung tâm để tạo hoặc kết nối danh tính của lực lượng lao động và quản lý tập trung quyền truy cập vào nhiều tài khoản và ứng dụng AWS.5

**Lợi ích chính bao gồm:**

* **Tích hợp với các ứng dụng do AWS quản lý:** Cung cấp một cái nhìn chung về người dùng và nhóm cho các ứng dụng như Amazon Q Developer và Amazon QuickSight.79  
* **Truyền bá danh tính đáng tin cậy:** Cho phép chia sẻ an toàn danh tính của người dùng trên các ứng dụng AWS được tích hợp, đơn giản hóa việc kiểm toán hoạt động của người dùng.79  
* **Quản lý quyền tập trung cho nhiều tài khoản AWS:** Cung cấp một nơi duy nhất để gán quyền cho các nhóm người dùng trên nhiều tài khoản AWS, dựa trên chức năng công việc hoặc nhu cầu bảo mật tùy chỉnh.79  
* **Liên kết đơn giản hóa:** Giảm nỗ lực quản trị trong việc quản lý quyền truy cập vào nhiều ứng dụng và tài khoản AWS bằng cách cung cấp một điểm liên kết duy nhất.79  
* **Cổng truy cập AWS thân thiện với người dùng:** Cung cấp một cổng web để người dùng truy cập liền mạch vào các ứng dụng và tài khoản AWS được chỉ định của họ.79

**Kiến trúc tổng thể:** IAM Identity Center hoạt động như một trung tâm để quản lý danh tính và quyền truy cập. Nó tích hợp với các nguồn danh tính khác nhau, bao gồm các nhà cung cấp danh tính hiện có (ví dụ: Okta, Azure AD) hoặc cho phép tạo và quản lý người dùng trực tiếp trong IAM Identity Center.5 Sau khi danh tính được quản lý, các **Bộ quyền (Permission Sets)** được sử dụng để kiểm soát những gì người dùng có thể làm trong các tài khoản hoặc ứng dụng AWS khác nhau. Dịch vụ này hỗ trợ các phiên bản tổ chức (khuyến nghị cho việc quản lý quyền truy cập vào tài khoản AWS và sử dụng trong sản xuất) và các phiên bản tài khoản (cho các triển khai biệt lập của một số ứng dụng AWS được chọn).79

IAM Identity Center đại diện cho đỉnh cao của sự phát triển của IAM hướng tới quyền truy cập lực lượng lao động tập trung, thân thiện với người dùng và có khả năng mở rộng. Nó trực tiếp giải quyết những điểm yếu của các giai đoạn IAM trước đó: quản lý người dùng IAM cá nhân trên các tài khoản 5, sự phức tạp của việc thiết lập liên kết cho mỗi tài khoản 5, và nhu cầu về trải nghiệm người dùng nhất quán. Bằng cách tích hợp với AWS Organizations và cung cấp các khái niệm như Bộ quyền 5, nó mang lại một giải pháp toàn diện và dễ quản lý hơn so với việc ghép nối các thành phần IAM riêng lẻ. Việc dịch vụ này miễn phí 85 cũng khuyến khích việc áp dụng.

### **Bộ quyền (Permission Sets): Quản lý Vai trò và Chính sách Tập trung**

Bộ quyền là một khái niệm cốt lõi trong IAM Identity Center để quản lý quyền truy cập vào các tài khoản AWS. Chúng về cơ bản là tập hợp các chính sách xác định mức độ truy cập được cấp cho người dùng hoặc nhóm trong một tài khoản AWS cụ thể.5 Chúng hoạt động như các mẫu vai trò, xác định các chính sách cần đưa vào vai trò mà Identity Center sẽ tạo trong mỗi tài khoản trong tổ chức AWS của bạn.88

Một bộ quyền chứa một hoặc nhiều chính sách IAM của AWS. Các chính sách này xác định các hành động cụ thể mà người dùng với bộ quyền này có thể thực hiện trên các tài nguyên AWS trong các tài khoản được chỉ định. Bạn có thể chọn từ các chính sách do AWS quản lý được xác định trước hoặc tạo các chính sách tùy chỉnh của riêng mình.79 Việc có thể sử dụng Chính sách do Khách hàng quản lý (CMP) cho phép điều chỉnh chính sách cho từng tài khoản để tham chiếu các tài nguyên cụ thể.85

Bộ quyền với Chính sách do Khách hàng quản lý (CMP) mang lại sự cân bằng tối ưu giữa định nghĩa tập trung và tùy chỉnh theo từng tài khoản. Ban đầu, các bộ quyền bị hạn chế hơn. Khả năng sử dụng CMP 85 là một cải tiến đáng kể. Nó cho phép định nghĩa tập trung một *loại vai trò* (ví dụ: "DeveloperAccess") thông qua một bộ quyền, trong khi các tài nguyên cụ thể mà vai trò đó có thể truy cập có thể được xác định trong một CMP được triển khai cho từng tài khoản đích. Điều này tránh được các quyền quá chung chung hoặc các chính sách nội tuyến đồ sộ trong chính bộ quyền, thúc đẩy đặc quyền tối thiểu trong khi vẫn duy trì quyền kiểm soát trung tâm đối với các *loại* quyền truy cập được cấp.

### **Tích hợp với Nguồn Danh tính và Triển khai ABAC với Thuộc tính Người dùng**

IAM Identity Center có thể kết nối với các nhà cung cấp danh tính (IdP) bên ngoài như Okta, Azure AD, hoặc bất kỳ IdP tương thích SAML 2.0 nào, hoặc sử dụng thư mục nội bộ của riêng nó để quản lý người dùng.50

Một khả năng mạnh mẽ là triển khai Kiểm soát Truy cập Dựa trên Thuộc tính (ABAC). Với ABAC, IAM Identity Center sử dụng các thuộc tính người dùng từ nguồn danh tính được kết nối và chuyển chúng dưới dạng thẻ (tags) trong AWS.60 Khi người dùng đăng nhập, IAM Identity Center gửi các thuộc tính này trong phiên AWS.60

**Ánh xạ Thuộc tính:** Quá trình này bao gồm việc ánh xạ các thuộc tính từ IdP (ví dụ: costCenter của Okta) sang các thẻ phiên (ví dụ: aws:PrincipalTag/CostCenter) trong cài đặt của IAM Identity Center.60 Các thẻ này sau đó được sử dụng trong các điều kiện của chính sách IAM để đưa ra quyết định truy cập.

Lợi ích của ABAC với IAM Identity Center bao gồm việc yêu cầu ít bộ quyền hơn, cho phép thay đổi nhóm linh hoạt, tận dụng các thuộc tính công ty hiện có và cải thiện khả năng theo dõi truy cập.60

ABAC trong IAM Identity Center tăng cường sức mạnh cho các Bộ quyền bằng cách làm cho chúng nhận biết được ngữ cảnh. Một bộ quyền duy nhất có thể cấp các quyền hiệu quả khác nhau cho những người dùng khác nhau dựa trên thuộc tính của họ.60 Ví dụ, Bob và Sally có thể có cùng một bộ quyền "ProjectContributor", nhưng nếu thuộc tính project\_id của Bob là "Alpha" và của Sally là "Beta", các chính sách ABAC (sử dụng aws:PrincipalTag/project\_id) trong bộ quyền đó (hoặc trên tài nguyên) sẽ đảm bảo họ chỉ truy cập các tài nguyên được gắn thẻ với ID dự án tương ứng của họ. Điều này làm giảm đáng kể số lượng bộ quyền cần thiết so với cách tiếp cận hoàn toàn dựa trên vai trò để đạt được mức độ chi tiết như vậy.

## **8\. Giải mã Logic Đánh giá Chính sách IAM**

Hiểu cách AWS IAM đánh giá các chính sách để đưa ra quyết định ủy quyền là điều cốt yếu để thiết kế các biện pháp kiểm soát truy cập hiệu quả và khắc phục sự cố về quyền.

### **Hành trình Ủy quyền: Cách AWS Xác định Quyền truy cập**

Khi một principal (người dùng, vai trò hoặc dịch vụ) cố gắng thực hiện một hành động trên một tài nguyên AWS, một quy trình gồm nhiều bước sẽ diễn ra. Quá trình này bao gồm xác thực (nếu cần), xử lý ngữ cảnh yêu cầu (thu thập thông tin về principal, hành động, tài nguyên và các điều kiện) và cuối cùng là đánh giá chính sách.26

### **Thứ tự Đánh giá: Từ chối Rõ ràng, Cho phép Rõ ràng, Từ chối Ngầm định theo Mặc định**

Logic đánh giá của IAM tuân theo một trật tự ưu tiên nghiêm ngặt:

1. **Từ chối Rõ ràng (Explicit Deny) Luôn Thắng:** Nếu bất kỳ chính sách hiện hành nào (chính sách dựa trên định danh, dựa trên tài nguyên, SCP, giới hạn quyền, chính sách phiên) chứa một câu lệnh Deny rõ ràng áp dụng cho yêu cầu, yêu cầu đó sẽ ngay lập tức bị từ chối. Điều này ghi đè lên bất kỳ câu lệnh Allow nào có thể tồn tại.26 Đây là yếu tố quyết định mạnh mẽ nhất.  
2. **Cho phép Rõ ràng (Explicit Allow):** Nếu không có từ chối rõ ràng nào, hệ thống sẽ tìm kiếm một câu lệnh Allow rõ ràng trong một chính sách liên quan (dựa trên định danh hoặc dựa trên tài nguyên, tùy thuộc vào ngữ cảnh) cấp quyền cho hành động đó.26  
3. **Từ chối Ngầm định theo Mặc định (Implicit Deny by Default):** Nếu không có từ chối rõ ràng và cũng không có cho phép rõ ràng nào được tìm thấy, yêu cầu sẽ bị từ chối ngầm định theo mặc định.26

Thứ tự cơ bản này là nền tảng của việc ra quyết định trong IAM. Việc hiểu rõ "từ chối theo mặc định" và "từ chối rõ ràng ghi đè tất cả" là rất quan trọng đối với bảo mật. Nguyên tắc "từ chối theo mặc định" là một nguyên lý cốt lõi của thiết kế hệ thống an toàn trong AWS IAM. Việc yêu cầu một Allow rõ ràng để cấp quyền truy cập 26 đảm bảo rằng các quyền không bao giờ được cấp một cách vô tình. Nếu một quyền không được nêu rõ ràng, nó sẽ bị từ chối. Điều này buộc các quản trị viên phải cân nhắc kỹ lưỡng khi cấp quyền truy cập, phù hợp với nguyên tắc đặc quyền tối thiểu. Sức mạnh của một Explicit Deny 26 hoạt động như một mạng lưới an toàn quan trọng, cho phép các quyền Allow rộng rãi được hạn chế một cách cẩn thận.

### **Tương tác của các Loại Chính sách: Dựa trên Định danh, Dựa trên Tài nguyên, SCPs, Giới hạn Quyền, Chính sách Phiên**

Sự tương tác phức tạp giữa các loại chính sách này xác định các quyền hiệu quả thực sự. Việc hiểu sai những tương tác này là một nguồn phổ biến gây ra các vấn đề về quyền.

* **Chính sách Kiểm soát Dịch vụ (SCPs) của AWS Organizations:** Được đánh giá đầu tiên. Một từ chối rõ ràng trong SCP sẽ ghi đè tất cả các quyền khác. Các quyền kết quả sau khi đánh giá SCP là sự *giao nhau* của chính sách người dùng, SCP (và RCPs \- Chính sách Kiểm soát Tài nguyên, một thuật ngữ cũ hơn cho một số loại kiểm soát ở cấp tổ chức).26 Điều này có nghĩa là một hành động phải được cho phép bởi cả ba để được tiến hành.  
* **Chính sách Dựa trên Tài nguyên (Resource-based Policies):** Được đánh giá tiếp theo. Nếu tài nguyên đang được truy cập có chính sách dựa trên tài nguyên đính kèm (ví dụ: chính sách bucket S3, chính sách khóa KMS), các chính sách này sẽ được đánh giá. Đối với quyền truy cập trong cùng một tài khoản, các quyền là sự *kết hợp (union)* của các quyền được cấp bởi cả chính sách dựa trên định danh và chính sách dựa trên tài nguyên.26 Nếu một hành động được cho phép bởi một trong hai hoặc cả hai, AWS sẽ cho phép hành động đó, trừ khi có một từ chối rõ ràng. (Lưu ý: Chính sách khóa KMS là chính yếu; các chính sách IAM đơn lẻ không thể cấp quyền truy cập nếu chính sách khóa không cho phép 25).  
* **Chính sách Dựa trên Định danh (Identity-based Policies):** Được đính kèm với người dùng, nhóm hoặc vai trò IAM. Chúng được đánh giá cùng với các chính sách dựa trên tài nguyên như đã mô tả ở trên.26  
* **Giới hạn Quyền (Permissions Boundaries):** Nếu một giới hạn quyền được đính kèm với người dùng hoặc vai trò IAM, nó sẽ được đánh giá cùng với các chính sách dựa trên định danh. Các quyền kết quả là sự *giao nhau* của các quyền được cấp bởi chính sách dựa trên định danh và giới hạn quyền.3 Một hành động chỉ được phép nếu nó được cho phép bởi *cả hai*.  
* **Chính sách Phiên (Session Policies):** Được sử dụng khi đảm nhận một vai trò hoặc khi người dùng liên kết truy cập AWS. Các quyền kết quả là sự *giao nhau* của chính sách dựa trên định danh của vai trò hoặc người dùng liên kết và chính sách phiên.26

**Bảng 8.1: Tóm tắt Logic Đánh giá Chính sách IAM**

| Giai đoạn/Loại Chính sách Đánh giá | Ảnh hưởng đến Quyền (Cấp/Hạn chế) | Tương tác với các Chính sách Khác | Đặc điểm Chính |
| :---- | :---- | :---- | :---- |
| **Từ chối Rõ ràng (Tất cả Chính sách)** | Hạn chế (Ghi đè tất cả) | Ghi đè mọi Allow trong bất kỳ chính sách nào.26 | Quyết định cuối cùng nếu có mặt. |
| **SCPs của Organizations** | Hạn chế (Đặt giới hạn tối đa) | Đánh giá trước tiên. Quyền hiệu quả là giao của chính sách người dùng và SCPs. Từ chối trong SCP ghi đè tất cả.26 | Lan can bảo vệ cấp tổ chức, ảnh hưởng đến cả người dùng gốc của tài khoản thành viên.78 Không cấp quyền. |
| **Chính sách Dựa trên Tài nguyên** | Cấp hoặc Hạn chế (tùy thuộc Effect) | Đối với truy cập cùng tài khoản, quyền là hợp của chính sách định danh và chính sách tài nguyên (ví dụ S3).26 Chính sách khóa KMS là chính yếu.25 | Gắn trực tiếp vào tài nguyên. Quan trọng cho truy cập giữa các tài khoản và cấp quyền cho dịch vụ AWS. |
| **Chính sách Dựa trên Định danh** | Cấp hoặc Hạn chế (tùy thuộc Effect) | Đánh giá cùng với chính sách tài nguyên. | Gắn với người dùng, nhóm hoặc vai trò IAM. Xác định những gì định danh có thể làm. |
| **Giới hạn Quyền** | Hạn chế (Đặt giới hạn tối đa cho định danh) | Quyền hiệu quả là giao của chính sách định danh và giới hạn quyền. Từ chối trong một trong hai sẽ ghi đè.3 | Ngăn chặn leo thang đặc quyền khi ủy quyền tạo vai trò/chính sách.3 Không tự cấp quyền. |
| **Chính sách Phiên** | Hạn chế (Thu hẹp quyền của vai trò được đảm nhận cho phiên cụ thể) | Quyền hiệu quả là giao của chính sách định danh của vai trò và chính sách phiên.26 | Được truyền khi đảm nhận vai trò. Không thể cấp nhiều quyền hơn vai trò gốc. |
| **Từ chối Ngầm định** | Hạn chế (Mặc định nếu không có Allow rõ ràng) | Áp dụng nếu không có Allow rõ ràng nào và không có Deny rõ ràng nào.26 | Nguyên tắc bảo mật cơ bản: quyền phải được cấp một cách rõ ràng. |

Bảng này cung cấp một cái nhìn tổng quan có cấu trúc về logic đánh giá phức tạp, làm rõ hệ thống phân cấp và sự tương tác của các loại chính sách khác nhau. Điều này rất cần thiết để chẩn đoán các vấn đề về quyền và thiết kế các biện pháp kiểm soát truy cập hiệu quả.25

Quyền "hạn chế nhất" sẽ thắng thế trong các phép giao, trong khi "bất kỳ sự cho phép nào" cũng đủ trong các phép hợp (khi không có sự từ chối rõ ràng). Khi các chính sách giao nhau (SCPs với chính sách IAM, Giới hạn Quyền với chính sách IAM, Chính sách Phiên với chính sách Vai trò), tập hợp quyền kết quả là những gì chung cho *cả hai*.26 Điều này có nghĩa là chính sách hạn chế hơn trong hai chính sách sẽ xác định một cách hiệu quả ranh giới. Ngược lại, đối với việc đánh giá chính sách dựa trên định danh và dựa trên tài nguyên trong cùng một tài khoản 26, một Allow trong *một trong hai* chính sách là đủ để cấp quyền truy cập, miễn là không có Deny rõ ràng nào trong bất kỳ chính sách hiện hành nào. Điều này làm nổi bật các phương thức tổng hợp quyền khác nhau tùy thuộc vào các loại chính sách liên quan. (Lưu ý: Chính sách khóa KMS là một ngoại lệ đối với phép hợp đơn giản, vì chúng là chính yếu 25).

### **Vai trò Quan trọng của Điều kiện**

Điều kiện là một yếu tố tùy chọn trong một câu lệnh chính sách.20 Chúng chỉ định khi nào một câu lệnh được áp dụng, bằng cách sử dụng các cặp khóa-giá trị.20 Điều kiện có thể có mặt trong tất cả các loại chính sách 26 và được sử dụng rộng rãi trong ABAC với các khóa như aws:ResourceTag, aws:PrincipalTag, aws:RequestTag, aws:TagKeys.61 Các biến chính sách (ví dụ: ${aws:username}, ${aws:PrincipalTag/team}) có thể được sử dụng trong các yếu tố Resource và Condition.20

Điều kiện cho phép kiểm soát truy cập chi tiết, nhận biết ngữ cảnh, vượt ra ngoài việc chỉ cho phép/từ chối đơn giản các hành động trên tài nguyên. Chúng là động cơ đằng sau ABAC và nhiều tình huống IAM nâng cao khác. Nếu không có điều kiện, các chính sách sẽ là các cấp phép hoặc từ chối tĩnh. Điều kiện 20 cho phép các chính sách phản ứng với ngữ cảnh của một yêu cầu (địa chỉ IP nguồn, thời gian trong ngày, trạng thái MFA, thẻ trên principal hoặc tài nguyên). Các biến chính sách 69 tiếp tục tăng cường điều này bằng cách cho phép viết các chính sách chung chung có thể thích ứng với người dùng hoặc tài nguyên cụ thể tại thời điểm đánh giá (ví dụ: cho phép truy cập vào s3:::mybucket/${aws:username}/\*). Sự kết hợp này là những gì cho phép các mẫu mạnh mẽ như ABAC 60 và giảm sự lan rộng của chính sách.

## **9\. Bảo mật IAM: Các Phương pháp Tốt nhất, Kiểm toán và Khắc phục Sự cố**

Việc duy trì một môi trường AWS an toàn phụ thuộc rất nhiều vào việc triển khai các phương pháp thực hành tốt nhất của IAM, kiểm toán thường xuyên các cấu hình và hoạt động, cũng như khả năng khắc phục sự cố quyền một cách hiệu quả.

### **Các Phương pháp Tốt nhất Nền tảng**

* Bảo vệ Người dùng Gốc: Định danh Quan trọng Nhất  
  Người dùng gốc của tài khoản AWS có toàn quyền truy cập vào tất cả các dịch vụ và tài nguyên AWS trong tài khoản đó, bao gồm cả thông tin thanh toán nhạy cảm.16 Do đó, việc sử dụng người dùng gốc cho các tác vụ hàng ngày là cực kỳ rủi ro và không được khuyến khích. Thay vào đó, hãy tạo người dùng quản trị hoặc sử dụng vai trò IAM cho các tác vụ cụ thể.  
  Các phương pháp tốt nhất để bảo vệ người dùng gốc bao gồm:  
  * Sử dụng mật khẩu mạnh và duy nhất.32  
  * Kích hoạt Xác thực Đa yếu tố (MFA), và xem xét việc sử dụng nhiều thiết bị MFA để tăng khả năng phục hồi.15  
  * Không tạo khóa truy cập cho người dùng gốc.18  
  * Sử dụng địa chỉ email nhóm do doanh nghiệp quản lý cho thông tin xác thực người dùng gốc.32  
  * Giám sát việc truy cập và sử dụng người dùng gốc bằng các dịch vụ AWS như CloudWatch, EventBridge và GuardDuty.32  
* Nguyên tắc Đặc quyền Tối thiểu: Một Yêu cầu Liên tục  
  Đây là nguyên tắc bảo mật quan trọng nhất trong IAM. Chỉ cấp những quyền cần thiết để thực hiện một tác vụ cụ thể.1  
  * Bắt đầu với các chính sách do AWS quản lý, sau đó chuyển sang các chính sách do khách hàng quản lý để đạt được đặc quyền tối thiểu.1  
  * Thường xuyên xem xét và loại bỏ các quyền không sử dụng.14

Việc đạt được đặc quyền tối thiểu là một quá trình lặp đi lặp lại, không phải là một thiết lập một lần. Các yêu cầu thay đổi, ứng dụng phát triển và người dùng thay đổi vai trò. Do đó, các thực hành như thường xuyên xem xét các quyền 14, sử dụng các công cụ như IAM Access Analyzer để xác định quyền truy cập không sử dụng 14, và tinh chỉnh các chính sách 14 là rất cần thiết. Đó là một hành trình liên tục.93

* Quản lý Thông tin Xác thực Hiệu quả: Xoay vòng, MFA và Tránh Khóa Dài hạn  
  Thông tin xác thực bị xâm phạm là một vectơ tấn công chính. Việc giảm sự phụ thuộc vào thông tin xác thực dài hạn và bảo vệ những thông tin xác thực hiện có là rất quan trọng.  
  * Ưu tiên thông tin xác thực tạm thời thông qua vai trò.1  
  * Nếu khóa dài hạn (khóa truy cập người dùng IAM) là cần thiết, hãy xoay vòng chúng thường xuyên (ví dụ: cứ sau 90 ngày hoặc ít hơn).10  
  * Kích hoạt MFA cho tất cả người dùng khi có thể.10  
* Sử dụng Chiến lược Nhóm IAM  
  Sử dụng nhóm để quản lý quyền cho người dùng IAM có nhu cầu truy cập tương tự.1 Đính kèm chính sách vào nhóm thay vì người dùng cá nhân.1 Điều này giúp đơn giản hóa việc quản trị và đảm bảo tính nhất quán cho các quyền của người dùng IAM.  
  Mặc dù IAM Identity Center là tương lai cho quyền truy cập của lực lượng lao động, Nhóm IAM vẫn có liên quan để quản lý quyền của người dùng IAM "gốc" khi chúng không thể tránh khỏi. AWS khuyến nghị mạnh mẽ IAM Identity Center cho người dùng là con người.9 Tuy nhiên, đối với các trường hợp sử dụng hợp pháp (mặc dù hạn chế) mà người dùng IAM trực tiếp vẫn được yêu cầu (ví dụ: truy cập khẩn cấp, các tài khoản dịch vụ cụ thể không thể sử dụng vai trò \- 9), Nhóm IAM vẫn là phương pháp thực hành tốt nhất để quản lý quyền của họ.1 Điều này làm nổi bật một khía cạnh thực tế của việc quản lý một bối cảnh danh tính hỗn hợp.

### **Kiểm toán và Giám sát IAM**

* Tận dụng AWS CloudTrail để Ghi nhật ký Hoạt động IAM  
  AWS CloudTrail ghi lại hoạt động của người dùng và các lệnh gọi API trong tài khoản AWS của bạn.33 Nó ghi lại các sự kiện quản lý (thay đổi IAM, đăng nhập bảng điều khiển) và tùy chọn là các sự kiện dữ liệu (hoạt động ở cấp đối tượng S3, thực thi Lambda).82 Mỗi bản ghi sự kiện bao gồm thông tin về ai đã thực hiện hành động, hành động gì đã được thực hiện, khi nào, ở đâu, các tài nguyên bị ảnh hưởng và kết quả của hành động.82 CloudTrail rất cần thiết để kiểm toán các thay đổi IAM, theo dõi quyền truy cập, điều tra các sự cố bảo mật và đảm bảo tuân thủ.33  
  Giá trị của CloudTrail đối với việc kiểm toán IAM vượt ra ngoài việc chỉ ghi lại các lệnh gọi API IAM. Mặc dù CloudTrail ghi lại các lệnh gọi API IAM trực tiếp (ví dụ: CreateUser, AttachRolePolicy), nó cũng ghi lại các hành động được thực hiện *bởi* các principal IAM đối với các dịch vụ khác (ví dụ: một vai trò phiên bản EC2 truy cập S3). Hoạt động API rộng hơn này, khi được tương quan với thông tin principal IAM trong sự kiện CloudTrail 95, cung cấp một bức tranh hoàn chỉnh về những gì các danh tính đang làm trên toàn bộ môi trường AWS, điều này rất quan trọng để kiểm toán toàn diện.  
* Tích hợp CloudTrail với CloudWatch Logs để Tăng cường Giám sát  
  CloudTrail có thể gửi nhật ký đến Amazon CloudWatch Logs để giám sát, cảnh báo và tạo bảng điều khiển theo thời gian thực.82 Bạn có thể thiết lập cảnh báo cho các sự kiện quan trọng, chẳng hạn như thay đổi chính sách IAM hoặc đăng nhập của người dùng gốc.84 Sự tích hợp này biến các bản ghi CloudTrail thụ động thành một hệ thống giám sát và cảnh báo chủ động.  
  Cảnh báo CloudWatch dựa trên các sự kiện CloudTrail cho IAM hoạt động như một hệ thống cảnh báo sớm cho các cấu hình sai bảo mật tiềm ẩn hoặc hoạt động độc hại. Bằng cách thiết lập cảnh báo CloudWatch cho các sự kiện CloudTrail liên quan đến IAM cụ thể (ví dụ: đăng nhập người dùng gốc, hủy kích hoạt MFA, thay đổi chính sách quan trọng, sửa đổi SCP) 84, các nhóm bảo mật có thể được thông báo gần như theo thời gian thực về các hoạt động có rủi ro cao. Điều này cho phép điều tra và ứng phó ngay lập tức, thay vì phát hiện các vấn đề một cách muộn màng trong quá trình kiểm toán. Việc cảnh báo chủ động này là một thành phần quan trọng của một tư thế bảo mật IAM mạnh mẽ.

### **Bảo mật và Xác thực Chủ động**

* AWS IAM Access Analyzer: Xác định Quyền truy cập Bên ngoài Không mong muốn và Quyền truy cập Không sử dụng  
  IAM Access Analyzer giúp xác định các tài nguyên được chia sẻ với các thực thể bên ngoài, chẳng hạn như bucket S3 và vai trò IAM.18 Nó phân tích các chính sách dựa trên tài nguyên bằng cách sử dụng suy luận dựa trên logic để xác định các tài nguyên được chia sẻ với các principal bên ngoài.93 Dịch vụ này tạo ra các phát hiện (findings) cho mỗi trường hợp tài nguyên được chia sẻ bên ngoài tài khoản của bạn, cung cấp thông tin chi tiết về quyền truy cập và principal bên ngoài đã cấp quyền đó.  
  IAM Access Analyzer cũng giúp đạt được đặc quyền tối thiểu bằng cách xác định các vai trò, khóa, mật khẩu và quyền không sử dụng, đồng thời đề xuất các chính sách được tinh chỉnh dựa trên hoạt động truy cập thực tế.14 Nó xác thực các chính sách dựa trên các phương pháp thực hành tốt nhất của IAM và các tiêu chuẩn bảo mật tùy chỉnh.14 Đây là một công cụ quan trọng để chủ động xác định và khắc phục các rủi ro bảo mật liên quan đến quyền.

#### **Nguồn trích dẫn**

1. Inside AWS IAM: A Deep Dive into Identity and Access Management \- Cloudchipr, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-iam](https://cloudchipr.com/blog/aws-iam)  
2. AWS IAM Roles and Policies: Understanding Cloud Security \- Delinea, truy cập vào tháng 5 14, 2025, [https://delinea.com/blog/aws-iam-roles-and-policies-explained](https://delinea.com/blog/aws-iam-roles-and-policies-explained)  
3. When and where to use IAM permissions boundaries | AWS Security ..., truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/](https://aws.amazon.com/blogs/security/when-and-where-to-use-iam-permissions-boundaries/)  
4. AWS Identity: Permission boundaries & delegation \- awsstatic.com, truy cập vào tháng 5 14, 2025, [https://d1.awsstatic.com/events/reinvent/2019/REPEAT\_1\_AWS\_identity\_Permission\_boundaries\_&\_delegation\_SEC402-R1.pdf](https://d1.awsstatic.com/events/reinvent/2019/REPEAT_1_AWS_identity_Permission_boundaries_&_delegation_SEC402-R1.pdf)  
5. The Evolution of AWS IAM: From IAM Users to Identity Center and Beyond \- Axiom Security, truy cập vào tháng 5 14, 2025, [https://axiom.security/the-evolution-of-aws-iam/](https://axiom.security/the-evolution-of-aws-iam/)  
6. Document history for IAM \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/document-history.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/document-history.html)  
7. ABAC (Attribute-Based Access Control): Guide and Examples \- Frontegg, truy cập vào tháng 5 14, 2025, [https://frontegg.com/guides/abac](https://frontegg.com/guides/abac)  
8. AWS Security | How to Create IAM User, Groups, Role, and Policy ..., truy cập vào tháng 5 14, 2025, [https://dev.to/s3cloudhub/aws-security-how-to-create-iam-user-groups-role-and-policy-3pak](https://dev.to/s3cloudhub/aws-security-how-to-create-iam-user-groups-role-and-policy-3pak)  
9. Use cases for IAM users \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/gs-identities-iam-users.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/gs-identities-iam-users.html)  
10. AWS security credentials \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html)  
11. UploadSigningCertificate \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/APIReference/API\_UploadSigningCertificate.html](https://docs.aws.amazon.com/IAM/latest/APIReference/API_UploadSigningCertificate.html)  
12. X.509 client certificates \- AWS IoT Core, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/iot/latest/developerguide/x509-client-certs.html](https://docs.aws.amazon.com/iot/latest/developerguide/x509-client-certs.html)  
13. IAM roles \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)  
14. Security best practices in IAM \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)  
15. IAM \- Multi-Factor Authentication \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/features/mfa/](https://aws.amazon.com/iam/features/mfa/)  
16. 13 Essential AWS IAM Best Practices \- Wiz, truy cập vào tháng 5 14, 2025, [https://www.wiz.io/academy/aws-iam-best-practices](https://www.wiz.io/academy/aws-iam-best-practices)  
17. AWS Identity and Access Management (IAM) Best Practices \- Amazon Web Services, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/resources/best-practices/](https://aws.amazon.com/iam/resources/best-practices/)  
18. Security control recommendations for managing identity and access \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/prescriptive-guidance/latest/security-controls-by-caf-capability/identity-and-access-controls.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-controls-by-caf-capability/identity-and-access-controls.html)  
19. IAM Roles and Groups. : r/aws \- Reddit, truy cập vào tháng 5 14, 2025, [https://www.reddit.com/r/aws/comments/1brgs6c/iam\_roles\_and\_groups/](https://www.reddit.com/r/aws/comments/1brgs6c/iam_roles_and_groups/)  
20. aws\_iam\_policy\_document | Data Sources | hashicorp/aws \- Terraform Registry, truy cập vào tháng 5 14, 2025, [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam\_policy\_document](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document)  
21. IAM JSON policy element reference \- AWS Identity and Access ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_elements.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)  
22. Choose between managed policies and inline policies \- AWS ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies-choosing-managed-or-inline.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies-choosing-managed-or-inline.html)  
23. Identity-based IAM policies for Lambda \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html](https://docs.aws.amazon.com/lambda/latest/dg/access-control-identity-based.html)  
24. Viewing resource-based IAM policies in Lambda \- AWS Lambda, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)  
25. Key policies in AWS KMS \- AWS Key Management Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)  
26. Policy evaluation logic \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_evaluation-logic.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)  
27. How Amazon S3 works with IAM \- Amazon Simple Storage Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/security\_iam\_service-with-iam.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security_iam_service-with-iam.html)  
28. Resource-based policy examples for AWS KMS \- AWS Database ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/dms/latest/userguide/security\_iam\_resource-based-policy-examples.html](https://docs.aws.amazon.com/dms/latest/userguide/security_iam_resource-based-policy-examples.html)  
29. Identity and Access Management for Amazon S3 \- Amazon Simple ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html)  
30. Customer managed policies \- AWS Private Certificate Authority, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/privateca/latest/userguide/auth-CustManagedPolicies.html](https://docs.aws.amazon.com/privateca/latest/userguide/auth-CustManagedPolicies.html)  
31. What is IAM? Guide to Identity and Access Management \- ManageEngine, truy cập vào tháng 5 14, 2025, [https://www.manageengine.com/active-directory-360/manage-and-protect-identities/iam-library/blogs/featured/learn-what-is-iam.html](https://www.manageengine.com/active-directory-360/manage-and-protect-identities/iam-library/blogs/featured/learn-what-is-iam.html)  
32. Root user best practices for your AWS account \- AWS Identity and ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)  
33. 10 AWS Identity and Access Management (IAM) Best Practices For Securing Cloud Permissions and Access Control\!. \- DEV Community, truy cập vào tháng 5 14, 2025, [https://dev.to/venkatramanan\_46/10-aws-identity-and-access-management-iam-best-practices-for-securing-cloud-permissions-and-2l4l](https://dev.to/venkatramanan_46/10-aws-identity-and-access-management-iam-best-practices-for-securing-cloud-permissions-and-2l4l)  
34. IAM tutorial: Delegate access across AWS accounts using IAM roles ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial\_cross-account-with-roles.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)  
35. AWS STS: A Complete Guide to AWS Security Token Service \- Cloudchipr, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-sts](https://cloudchipr.com/blog/aws-sts)  
36. AssumeRole \- AWS Security Token Service \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)  
37. SAML 2.0 federation \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_providers\_saml.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)  
38. AssumeRoleWithSAML \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRoleWithSAML.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html)  
39. OIDC federation \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_providers\_oidc.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)  
40. AssumeRoleWithWebIdentity \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_AssumeRoleWithWebIdentity.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html)  
41. GetSessionToken \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_GetSessionToken.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetSessionToken.html)  
42. get-session-token — AWS CLI 1.40.13 Command Reference, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html](https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html)  
43. get\_session\_token — Boto3 Docs 1.26.85 documentation \- AWS, truy cập vào tháng 5 14, 2025, [https://boto3.amazonaws.com/v1/documentation/api/1.26.85/reference/services/sts/client/get\_session\_token.html](https://boto3.amazonaws.com/v1/documentation/api/1.26.85/reference/services/sts/client/get_session_token.html)  
44. GetFederationToken \- AWS Security Token Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/STS/latest/APIReference/API\_GetFederationToken.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)  
45. AWS STS GetFederationToken \+ resource based policy, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUnNLZtmx-R72HvLZf16k2YA/aws-sts-getfederationtoken-resource-based-policy](https://repost.aws/questions/QUnNLZtmx-R72HvLZf16k2YA/aws-sts-getfederationtoken-resource-based-policy)  
46. OIDC federation \- AWS Identity and Access Management \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/en\_kr/IAM/latest/UserGuide/id\_roles\_providers\_oidc.html](https://docs.aws.amazon.com/en_kr/IAM/latest/UserGuide/id_roles_providers_oidc.html)  
47. Policies and permissions in AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)  
48. Defining Lambda function permissions with an execution role \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)  
49. Create a role to delegate permissions to an AWS service \- AWS ..., truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create\_for-service.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)  
50. IAM federation \- AWS Prescriptive Guidance, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/workplace-iam-federation.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/workplace-iam-federation.html)  
51. Patterns for multi-account, hub-and-spoke Amazon SageMaker model registry \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/machine-learning/patterns-for-multi-account-hub-and-spoke-amazon-sagemaker-model-registry/](https://aws.amazon.com/blogs/machine-learning/patterns-for-multi-account-hub-and-spoke-amazon-sagemaker-model-registry/)  
52. Securing Cross-Account Access in AWS Organizations: Monitoring and Governance \- CWL : Advanced Cyber Attack & Detection Learning Platform, truy cập vào tháng 5 14, 2025, [https://cyberwarfare.live/securing-cross-account-access-in-aws-organizations-monitoring-and-governance/](https://cyberwarfare.live/securing-cross-account-access-in-aws-organizations-monitoring-and-governance/)  
53. AWS Cross-Account Resource Access Patterns | Travis J. Gosselin, truy cập vào tháng 5 14, 2025, [https://travisgosselin.com/aws-cross-account-resource-access-patterns/](https://travisgosselin.com/aws-cross-account-resource-access-patterns/)  
54. Implementing a hub and spoke dashboard for multi-account data science projects \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/blogs/opensource/implementing-a-hub-and-spoke-dashboard-for-multi-account-data-science-projects/](https://aws.amazon.com/blogs/opensource/implementing-a-hub-and-spoke-dashboard-for-multi-account-data-science-projects/)  
55. Set up SAML 2.0 for WorkSpaces Personal \- AWS Documentation \- Amazon.com, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html](https://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html)  
56. truy cập vào tháng 1 1, 1970, [https.docs.aws.amazon.com/en\_kr/IAM/latest/UserGuide/id\_roles\_providers\_oidc.html](http://docs.google.com/https.docs.aws.amazon.com/en_kr/IAM/latest/UserGuide/id_roles_providers_oidc.html)  
57. Using SAML and token exchange to federate into AWS through the AWS Command Line Interface \- Ping Identity Docs, truy cập vào tháng 5 14, 2025, [https://docs.pingidentity.com/solution-guides/multi-factor\_authentication\_use\_cases/htg\_use\_saml\_and\_token\_exchange\_to\_federate\_into\_aws.html](https://docs.pingidentity.com/solution-guides/multi-factor_authentication_use_cases/htg_use_saml_and_token_exchange_to_federate_into_aws.html)  
58. AWS Permission Boundary: What Is It and How To Use It, truy cập vào tháng 5 14, 2025, [https://sonraisecurity.com/blog/aws-permission-boundary-what-is-it-and-how-to-use-it/](https://sonraisecurity.com/blog/aws-permission-boundary-what-is-it-and-how-to-use-it/)  
59. aws-samples/example-permissions-boundary: This repository contains a sample IAM permissions boundary as a starting point for creating your own permissions boundary to meet the security needs of your organization. The IAM permissions boundary sample, when attached to an IAM role, allow it to perform all expected workload tasks without being able \- GitHub, truy cập vào tháng 5 14, 2025, [https://github.com/aws-samples/example-permissions-boundary](https://github.com/aws-samples/example-permissions-boundary)  
60. Attribute-based access control \- AWS IAM Identity Center, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/abac.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/abac.html)  
61. How to create true attribute based access controls with IAM | AWS re ..., truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUJG6UhPsNTE21TqxFKnml5Q/how-to-create-true-attribute-based-access-controls-with-iam](https://repost.aws/questions/QUJG6UhPsNTE21TqxFKnml5Q/how-to-create-true-attribute-based-access-controls-with-iam)  
62. ABAC for AWS KMS \- AWS Key Management Service, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/kms/latest/developerguide/abac.html](https://docs.aws.amazon.com/kms/latest/developerguide/abac.html)  
63. RBAC vs. ABAC: Role-Based & Attribute-Based Access Control Compared | Splunk, truy cập vào tháng 5 14, 2025, [https://www.splunk.com/en\_us/blog/learn/rbac-vs-abac.html](https://www.splunk.com/en_us/blog/learn/rbac-vs-abac.html)  
64. RBAC vs. ABAC: What's the Difference? \- Descope, truy cập vào tháng 5 14, 2025, [https://www.descope.com/blog/post/rbac-vs-abac](https://www.descope.com/blog/post/rbac-vs-abac)  
65. For ABAC is there a standardised way to handle multiple tags for access, like I want to grant access to a resource based on a condition if a certain tag matches in a secure, readable, and organised way, what are your suggestions? : r/aws \- Reddit, truy cập vào tháng 5 14, 2025, [https://www.reddit.com/r/aws/comments/1jagmam/for\_abac\_is\_there\_a\_standardised\_way\_to\_handle/](https://www.reddit.com/r/aws/comments/1jagmam/for_abac_is_there_a_standardised_way_to_handle/)  
66. Attribute-Based Access Control (ABAC) for AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/identity/attribute-based-access-control/](https://aws.amazon.com/identity/attribute-based-access-control/)  
67. Select your attributes for access control \- AWS IAM Identity Center, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/configure-abac-attributes.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/configure-abac-attributes.html)  
68. ABAC with IAM Identity Center users (no external idP) \- AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/questions/QUEa6Qq52iTRqZicLDg9HMIg/abac-with-iam-identity-center-users-no-external-idp](https://repost.aws/questions/QUEa6Qq52iTRqZicLDg9HMIg/abac-with-iam-identity-center-users-no-external-idp)  
69. IAM policy elements: Variables and tags \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference\_policies\_variables.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)  
70. Identity and Access Management \- Amazon EKS, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/eks/latest/best-practices/identity-and-access-management.html](https://docs.aws.amazon.com/eks/latest/best-practices/identity-and-access-management.html)  
71. Understanding service-linked roles \- Amazon DocumentDB, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/documentdb/latest/developerguide/service-linked-roles.html](https://docs.aws.amazon.com/documentdb/latest/developerguide/service-linked-roles.html)  
72. CreateServiceLinkedRole \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/APIReference/API\_CreateServiceLinkedRole.html](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateServiceLinkedRole.html)  
73. Create a service-linked role \- AWS Identity and Access Management, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create-service-linked-role.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create-service-linked-role.html)  
74. Using service-linked roles for AWS Service Catalog \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/servicecatalog/latest/adminguide/using-service-linked-roles.html](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/using-service-linked-roles.html)  
75. Use a service-linked role (SLR) with ACM \- AWS Certificate Manager, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html](https://docs.aws.amazon.com/acm/latest/userguide/acm-slr.html)  
76. Resolve the AWS STS "PackedPolicyTooLarge" IAM assume role error | AWS re:Post, truy cập vào tháng 5 14, 2025, [https://repost.aws/knowledge-center/iam-role-aws-sts-error](https://repost.aws/knowledge-center/iam-role-aws-sts-error)  
77. What is AWS Organizations? \- AWS Organizations \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/organizations/latest/userguide/orgs\_introduction.html](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html)  
78. Service control policies (SCPs) \- AWS Organizations, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/organizations/latest/userguide/orgs\_manage\_policies\_scps.html](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)  
79. What is IAM Identity Center? \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)  
80. AWS Centralized Logging: A Complete Implementation Guide \- Last9, truy cập vào tháng 5 14, 2025, [https://last9.io/blog/aws-centralized-logging/](https://last9.io/blog/aws-centralized-logging/)  
81. AWS IAM Identity Center \- AWS Prescriptive Guidance \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/workplace-iam-identity-center.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/workplace-iam-identity-center.html)  
82. AWS CloudTrail 101: How to Keep Tabs on Every Action in Your AWS Environment, truy cập vào tháng 5 14, 2025, [https://cloudchipr.com/blog/aws-cloudtrail](https://cloudchipr.com/blog/aws-cloudtrail)  
83. Best Monitoring Practices for AWS CloudTrail Logs \- Middleware.io, truy cập vào tháng 5 14, 2025, [https://middleware.io/blog/aws-cloudtrail-logs/](https://middleware.io/blog/aws-cloudtrail-logs/)  
84. How to Leverage AWS CloudTrail to Stay Compliant and Secure in the Cloud?, truy cập vào tháng 5 14, 2025, [https://www.cloudoptimo.com/blog/how-to-leverage-aws-cloudtrail-to-stay-compliant-and-secure-in-the-cloud/](https://www.cloudoptimo.com/blog/how-to-leverage-aws-cloudtrail-to-stay-compliant-and-secure-in-the-cloud/)  
85. AWS IAM Identity Center Overview, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/cn/awstv/watch/e7f23b0e3b9/](https://aws.amazon.com/cn/awstv/watch/e7f23b0e3b9/)  
86. AWS IAM Identity Center Overview, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/it/awstv/watch/e7f23b0e3b9/](https://aws.amazon.com/it/awstv/watch/e7f23b0e3b9/)  
87. AWS IAM Identity Center Overview, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/awstv/watch/e7f23b0e3b9/](https://aws.amazon.com/awstv/watch/e7f23b0e3b9/)  
88. IAM Identity Center Permission Sets Tutorial \- AWS, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/awstv/watch/684b511ea12/](https://aws.amazon.com/awstv/watch/684b511ea12/)  
89. Untangle AWS IAM Policy Logic and Move Toward Least Privilege \- Sonrai Security, truy cập vào tháng 5 14, 2025, [https://sonraisecurity.com/blog/untangle-aws-iam-policy-logic/](https://sonraisecurity.com/blog/untangle-aws-iam-policy-logic/)  
90. Comprehensive Guide of AWS IAM Policy evaluation logic \- Community.aws, truy cập vào tháng 5 14, 2025, [https://community.aws/content/2d1bIioM3UgQZqyaYquu3kTaWAg/comprehensive-guide-of-aws-iam-policy-evaluation-logic](https://community.aws/content/2d1bIioM3UgQZqyaYquu3kTaWAg/comprehensive-guide-of-aws-iam-policy-evaluation-logic)  
91. AWS IAM Best Practices \- Coralogix, truy cập vào tháng 5 14, 2025, [https://coralogix.com/blog/aws-iam-best-practices/](https://coralogix.com/blog/aws-iam-best-practices/)  
92. Best practices for creating least-privilege AWS IAM policies \- Datadog, truy cập vào tháng 5 14, 2025, [https://www.datadoghq.com/blog/iam-least-privilege/](https://www.datadoghq.com/blog/iam-least-privilege/)  
93. AWS IAM Access Analyzer, truy cập vào tháng 5 14, 2025, [https://aws.amazon.com/iam/access-analyzer/](https://aws.amazon.com/iam/access-analyzer/)  
94. IAM Access Analyzer Findings \- Trend Micro, truy cập vào tháng 5 14, 2025, [https://www.trendmicro.com/cloudoneconformity/knowledge-base/aws/AccessAnalyzer/findings.html](https://www.trendmicro.com/cloudoneconformity/knowledge-base/aws/AccessAnalyzer/findings.html)  
95. CloudTrail concepts \- AWS CloudTrail \- AWS Documentation, truy cập vào tháng 5 14, 2025, [https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-concepts.html)  
96. A Deep Dive into IAM Access Analyzer: Enhancing AWS Security \- Gradient Cyber, truy cập vào tháng 5 14, 2025, [https://www.gradientcyber.com/resources/deep-dive-into-iam-access-analyzer](https://www.gradientcyber.com/resources/deep-dive-into-iam-access-analyzer)