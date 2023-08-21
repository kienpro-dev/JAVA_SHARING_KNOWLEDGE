# LẬP TRÌNH HƯỚNG KHÍA CẠNH - Aspect Oriented Programming (AOP)

## AOP là gì?

- AOP là một kỹ thuật lập trình cho phép phân tách chương trình thành cách module riêng rẽ, không phụ thuộc nhau. Khi hoạt động, chương trình sẽ kết hợp các module lại để thực hiện các chức năng nhưng khi sửa đổi chức năng thì chỉ cần sửa đổi trên một module cụ thể.
- AOP còn là một nguyên tắc thiết kế, giúp tách rời các yêu cầu hay các vấn đề được quan tâm (separation of concerns) trong chương trình thành các thành phần độc lập và từ đó tăng tính uyển chuyển cho chương trình

## Các thuật ngữ sử dụng trong AOP

![alt text](https://topdev.vn/blog/wp-content/uploads/2023/01/aspect-orientedprogramming-aop-in-java.png)

- Trong AOP, chương trình của chúng ta được chia thành 2 loại concern:

  - Core concern/ Primary concern: là requirement, logic xử lý chính của chương trình.
  - Cross-cutting concern: là những logic xử lý phụ cần được thực hiện của chương trình khi core concern được gọi như security, logging, tracing, monitoring, …

- Một số thuật ngữ khác trong AOP:

  - Joinpoint: là một điểm trong chương trình, là những nơi có thể được chèn những cross-cutting concern. Chẳng hạn chúng ta cần ghi log lại sau khi chạy method nào đó thì điểm ngay sau method đó được thực thi gọi là một Jointpoint. Một Jointpoint có thể là một phương thức được gọi, một ngoại lệ được throw ra, hay một field được thay đổi.
  - Pointcut: có nhiều cách để xác định Joinpoint, những cách như thế được gọi là Pointcut. Nó là các biểu thức được sử dụng để kiểm tra nó có khớp với các Jointpoint để xác định xem Advice có cần được thực hiện hay không.
  - Advice: những xử lý phụ (crosscutting concern) được thêm vào xử lý chính (core concern), code để thực hiện các xử lý đó được gọi Advice. Advice được chia thành các loại sau:
  - Before: được thực hiện trước join point.
  - After: được thực hiện sau join point.
  - Around: được thực hiện trước và sau join point.
  - After returning : được thực hiện sau join point hoàn thành một cách bình thường.
  - After throwing : được thực hiện sau khi join point được kết thúc bằng một Exception.
  - Aspect: tương tự như một Java class. Một Aspect đóng gói toàn bộ cross-cutting concern và có thể chứa các JointPoint, PointCut, Advice.
  - Target Object : là những đối tượng mà advice được áp dụng.

- Một vài cross-cutting concern thường thấy trong ứng dụng:

  - Logging
  - Monitor
  - Access control
  - Error handling
  - Transaction management
  - Session management
  - Input/output validation

## Weaving là gì?

![alt text](https://topdev.vn/blog/wp-content/uploads/2023/01/aspect-orientedprogramming-aop-weaving.png)

- Về cơ bản Weaving (đan/ dệt) là quá trình liên kết các thành phần aspect và non-aspect của một chương trình để tạo ra đầu ra mong muốn.

- Có một vài cách khác nhau giữa các hệ thống AOP về cách tạo ra Weaving. Có thể chia làm các loại Weaving: Compile-time weaving (static weaving), Load-Time Weaving và Run-time weaving (dynamic weaving).

- Compile-time weaving :
  - Pre-Compile Weaving : sử dụng bộ tiền xử lý (pre-processor) để combine code của aspect và code non-aspect lại với nhau trước khi code được biên dịch thành byte code Java (.class).
  - Post-Compile Weaving / Binary weaving : cách này dùng để inject code của aspect vào những tập tin .class của Java đã được compile.
  - Load-Time Weaving : cách này dùng để inject code của aspect khi class cần sử dụng aspect được load vào JVM, nghĩa là trong khi ứng dụng đang chạy.
  - Run-time weaving: thực hiện weaving và unweaving code của aspect và non-aspect tại run-time.
- Static weaving: là quá trình combine code của aspect và code non-aspect lại với nhau trước khi code được biên dịch thành Java byte code (.class) bằng cách sử dụng bộ tiền xử lý (pre-processor). Do đó, code gốc chỉ được thay đổi một lần tại thời gian biên dịch (compile). Hiệu suất của code được combine này tương đương với code được viết theo truyền thống.

- Hạn chế của phương pháp Static weaving là khó khăn trong việc xác định code của aspect sau này hoặc thực hiện thay đổi đối với code của aspect. Mỗi khi code của aspect bị thay đổi, tất cả tất cả code sử dụng aspect phải được biên dịch lại.

- Dynamic weaving: khắc phục một số hạn chế gặp phải khi weaving được thực hiện tại thời gian biên dịch (Compile-time weaving/ Static weaving). Có thể tránh được yêu cầu biên dịch lại (recompilation), triển khai lại (redeployment) và khởi động lại (restart) bằng cách thực hiện quy trình weaving trong thời gian chạy (run-time weaving).

- Có một chút khác biệt giữa load-time và run-time weaving.

  - Load-time weaving chỉ đơn giản là trì hoãn quá trình weaving cho đến khi các lớp được nạp bởi class loader. Cách tiếp cận này yêu cầu sử dụng một weaving class loader hoặc thay thế class loader bằng một loader khác. Hạn chế là load-time tăng lên và thiếu quyền truy cập vào các aspect trong khi chạy.
  - Run-time weaving là quá trình weaving và unweaving tại run-time. Cách tiếp cận này yêu cầu các cơ chế mới để can thiệp vào việc tính toán chạy.

  Các AOP Framework khác nhau có cách thực hiện dynamic weaving khác nhau. Trong khi AspectWerkz sử dụng sửa đổi byte code thông qua chức năng cấp JVM và kiến ​​trúc “hotswap” để thực hiện weaving các lớp tại run-time, thì Spring AOP Framework dựa trên các proxy thay vì các class loader hoặc dựa vào các đối số JVM.

- Dynamic weaving cho phép tăng tốc các giai đoạn thiết kế và kiểm thử trong phát triển phần mềm, vì các aspect mới có thể được thêm vào hoặc các aspect hiện tại có thể được thay đổi mà không cần biên dịch lại và triển khai lại các ứng dụng. Tuy nhiên, một nhược điểm lớn là hiệu suất giảm, vì weaving xảy ra tại run-time.
