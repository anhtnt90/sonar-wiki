# sonar-wiki
**Lỗi Sonar "Controller classes that use @SessionAttributes must call setComplete on their SessionStatus objects"** là một cảnh báo được sinh ra bởi SonarQube, một công cụ phân tích tĩnh mã nguồn tự động.

Trong Spring Framework, annotation "@SessionAttributes" được sử dụng để chỉ định rằng một controller class hoặc một phương thức trong controller class sử dụng session để lưu trữ thông tin giữa các yêu cầu. Khi sử dụng "@SessionAttributes", một đối tượng "SessionStatus" được tự động kích hoạt và được sử dụng để quản lý trạng thái của session.

Lỗi này xảy ra khi một controller class được đánh dấu bằng "@SessionAttributes" nhưng không gọi phương thức "setComplete()" trên đối tượng "SessionStatus" để xác định rằng session đã hoàn tất và cần được giải phóng.

Việc không gọi "setComplete()" có thể dẫn đến vấn đề về quản lý session và tiềm ẩn rủi ro bảo mật. Khi session không được giải phóng đúng cách, thông tin lưu trữ trong session có thể bị lộ ra ngoài hoặc gây ra sự cố về tài nguyên và hiệu suất.

Để khắc phục lỗi này, bạn cần đảm bảo rằng các controller class sử dụng "@SessionAttributes" gọi phương thức "setComplete()" trên đối tượng "SessionStatus" trong phương thức xử lý yêu cầu cuối cùng của session. Điều này đảm bảo rằng session được kết thúc đúng cách và tài nguyên được giải phóng. Ví dụ:
```
@Controller
@SessionAttributes("mySessionAttribute")
public class MyController {
    @RequestMapping("/process")
    public String processForm(@ModelAttribute("mySessionAttribute") MySessionAttribute attribute, SessionStatus sessionStatus) {
        // Xử lý logic
        
        // Khi đã hoàn thành xử lý, gọi setComplete() để đánh dấu session đã hoàn tất
        sessionStatus.setComplete();
        
        // Các xử lý khác
        return "result";
    }
}
```
Lời khuyên chung là luôn kiểm tra các annotation và đảm bảo rằng việc quản lý session được thực hiện đúng cách để tránh các lỗi và vấn đề bảo mật tiềm ẩn.

-----------

**Lỗi Sonar "RequestMapping methods should not be private" là một cảnh báo được sinh ra bởi SonarQube, một công cụ phân tích tĩnh mã nguồn tự động.**

Trong Spring Framework, annotation @RequestMapping được sử dụng để ánh xạ các phương thức xử lý yêu cầu HTTP vào các địa chỉ URL cụ thể. Tuy nhiên, một vấn đề tiềm ẩn là khi một phương thức được đánh dấu bằng @RequestMapping là private (riêng tư).

Lỗi này xảy ra vì các phương thức private không thể truy cập từ bên ngoài lớp hiện tại, và do đó, Spring sẽ không thể gọi và xử lý yêu cầu HTTP đến phương thức đó. Điều này làm cho việc sử dụng @RequestMapping với phương thức private trở nên vô ích và không phù hợp với mục đích của annotation này.

Để khắc phục lỗi này, bạn nên đảm bảo rằng các phương thức được đánh dấu bằng @RequestMapping có mức truy cập public hoặc mức truy cập mở rộng hơn như protected. Điều này cho phép Spring truy cập và gọi các phương thức này để xử lý yêu cầu HTTP từ các client.

Ví dụ:
```
@Controller
public class MyController {
    
    @RequestMapping("/my-url")
    public String handleRequest() {
        // Xử lý logic
        
        return "result";
    }
}
```
Trong ví dụ trên, phương thức handleRequest được đánh dấu bằng @RequestMapping và có mức truy cập là public, cho phép Spring truy cập và gọi phương thức này khi có yêu cầu tới URL "/my-url".

Lời khuyên chung là luôn đảm bảo rằng các phương thức sử dụng @RequestMapping có mức truy cập phù hợp để Spring có thể gọi và xử lý chúng một cách chính xác.

-----------
**Lỗi Sonar "SpringBootApplication and ComponentScan should not be used in the default package"** là một cảnh báo được sinh ra bởi SonarQube, một công cụ phân tích tĩnh mã nguồn tự động.

Trong Java, một package được gọi là "default package" khi không có tên package được định nghĩa hoặc khi mã nguồn được đặt trong thư mục gốc của dự án. Lỗi này xảy ra khi annotation "@SpringBootApplication" hoặc "@ComponentScan" được sử dụng trong default package.

Sử dụng default package không được khuyến nghị trong Java vì nó có thể gây ra những vấn đề về quản lý mã nguồn và tương tác với các công cụ và framework. Trong trường hợp của Spring Boot, việc đặt annotation "@SpringBootApplication" hoặc "@ComponentScan" trong default package không phù hợp với các quy ước và tiêu chuẩn của Spring Boot.

Để khắc phục lỗi này, bạn nên đặt mã nguồn của bạn trong một package được đặt tên thích hợp. Bạn có thể tạo một package mới và di chuyển tất cả các file mã nguồn của bạn vào package đó.

```
package com.example.myapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```
Trong ví dụ trên, package "com.example.myapp" được tạo và chứa các file mã nguồn của ứng dụng. Annotation "@SpringBootApplication" được đặt trực tiếp trên class "MyApplication" trong package này.

Lời khuyên chung là luôn đặt mã nguồn của bạn trong các package được đặt tên thích hợp, tránh sử dụng default package để đảm bảo sự rõ ràng và quản lý mã nguồn tốt hơn.
