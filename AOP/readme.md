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
  - Post-Compile Weaving / Binary weaving : cách này dùng để inject code của aspect vào những tập tin.class của Java đã được compile.
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

## Lợi ích của AOP là gì?

- Tăng hiệu quả của Object-orented programming (OOP).
- AOP không phải dùng để thay thế OOP mà để bổ sung cho OOP, nơi mà OOP còn thiếu sót trong việc tạo những ứng dụng thuộc loại phức tạp.
- Tăng cường tối đa khả năng tái sử dụng của mã nguồn.
- Đảm bảo Single responsibility principle: mỗi một module chỉ làm cái mà nó cần phải làm.
- Tuân thủ nguyên tắc “You aren’t gonna need it – YAGNI” – chúng ta chỉ cài đặt những thứ chúng ta thực sự cần, không bao giờ làm trước.
- Module hóa ở mức tiến trình/ chức năng.
- Code gọn gàng hơn do tách biệt phần xử lý chính và phần xử lý liên quan.
- Chức năng chính của chương trình không cần biết đến các chức năng phụ khác.
- Các chức năng phụ có thể được thêm thắt, bật tắt tại thời điểm run-time tùy theo yêu cầu.
- Các thay đổi nếu có đối với các chức năng phụ sẽ không ảnh hưởng đến chương trình chính.
- Hệ thống sẽ uyển chuyển và giảm thiểu tính phụ thuộc lẫn nhau của các module.

## Khó khăn khi áp dụng AOP

- Thực tế các file cấu hình cho spring hay các AOP framework khác như AspectJ không đơn giản chút nào. Gọi trực tiếp method chỉ mất 1 dòng và cũng dễ theo dỏi logic của ứng dụng.
- Thay đổi file cấu hình liệu có an toàn hơn thay đổi mã nguồn hay không ? Những nguời ủng hộ AOP thì cho rằng nó rất nên làm. Những nguời còn hoài nghi thì luỡng lự.
- Các AOP framework khác nhau đều có cách tạo file cấu hình khác nhau. Không giống như EJB có thể chạy trong mọi EJB container, ứng dụng viết cho AOP framework này không chạy đuợc trong AOP framework khác. Những cố gắng để thống nhứt các AOP framework cho đến bây giờ vẫn chưa kết thúc.
- Việc container đón đầu các cú gọi có thể ảnh huởng đến tốc độ chạy của ứng dụng. Thêm nữa, không phải mọi class trong ứng dụng đều cần đuợc quản lí bởi AOP framework cũng như không phải mọi class đều trở thành EJB. Nếu ứng dụng thiết kế kém cỏi có thể dẫn đến số luợng class mà AOP container phải quản lí cùng với các file cấu hình tăng lên nhanh chóng.
- Có lẽ cũng chính vì những lí do này mà AOP vẫn chưa cất cánh nhanh chóng như OOP. Điểm khó khăn cuối cùng nữa của AOP là lí thuyết hoá nó để truyền đạt.
- AOP đưa ra những khái niệm lạ hoắc khó nắm bắt, dễ làm nản lòng những nguời mới bắt đầu. Phần kế tiếp nguời viết sẽ trình bày những khái niệm cơ bản của AOP, và nguyên tắc Inversion of Control hay còn gọi là Dependency Injection của springframework trong việc triển khai AOP.

## AOP và OOP

- Một điểm quan trọng nữa cần đuợc nêu ra, AOP đuợc xem là cái bổ sung cho OOP, chỗ mà OOP còn thiếu sót trong việc tạo những ứng dụng thuộc loại phức tạp. AOP khônng phải là cái thay thế OOP.
- Hạn chế đối với OOP:

  - Lấy ví dụ về transaction trong việc truy cập database. Trong Java, truy cập database đòi hỏi nhiều buớc: tạo ra Connection, bắt đầu transation, commit, clean up connection… Để đơn giản hoá, ta hãy tạo ra 1 class hay black box chuyên làm việc này.

  ```java
      public class DBTransaction {
          public DBTransaction () {
          //mã nguồn tạo ra Connection và bắt đầu 1 transaction
          }

          public Connection getConnection () {
          //mã nguồn return Connection đã tạo ra
          }

          public void commit () {
          //mã nguồn commit transaction và clean up resources
          }
      }
  ```

  - Giả sử trong 1 ứng dụng, có 1 class (hay blackbox) XuatNhapHang làm chức năng xuất nhập hàng và nó chứa 1 method làm chức năng nhập hang.

  ```java
      public void nhapHang ( int monHang, int soLuong ) {
          //sử dụng blackbox để bắt đầu 1 transaction
          DBTransation tx = new DBTransactiơn ();

          // ...mã nguồn truy cập database để nhập hàng...

          tx.commit ();

      }
  ```

  - Bạn dễ dàng hình dung đuợc mục đích sử dụng của blackbox này.
  - Mỗi khi cần truy cập database, nguời sử dụng chỉ việc gọi các public methods của nó mà không cần biết tới bên trong nó hoạt động ra sao.
  - Việc thay đổi bên trong của blackbox cũng không làm ảnh huởng tới nguời sử dụng nó. Đó là cái đẹp của việc tạo ra blackbox.
  - Mọi nguời hình như đều vui vẻ và độc lập với công việc của mình. Thực tế không hoàn toàn như vậy. Giả sử ứng dụng sau này đòi hỏi phải theo dõi tất cả mọi hoạt động liên quan đến việc truy cập database. Mỗi khi database đuợc truy cập, tên nguời sử dụng và thời gian sẽ đuợc lưu trữ lại. Ta có thể tạo ra 1 blackbox đơn giản chuyên làm việc này.

  ```java
      public class DBTruyCapTheoDoi {
        public DBTruyCapTheoDoi (Connection con, String tenNguoi) {
        //mã nguồn lưu lại trong database nguời sử dụng và thời gian
        .....
        }
    }
  ```

  - Method nhapHang hay những nơi trong ứng dụng đã truy cập database đều phải sửa đổi như sau

    ```java
        public void nhapHang ( int monHang, int soLuong ) {
            //sử dụng blackbox để bắt đầu 1 transaction
            DBTransation tx = new DBTransactiơn ();
            //theo doi truy cap
            DBTruyCapTheoDoi (tx.getConnection (), tenNguoi);

                // ...mã nguồn truy cập database để nhập hàng...

                tx.commit ();

            }
    ```

  - Sửa đổi không nhiều nhưng nó rộng khắp trong toàn ứng dụng. Đây chính là điểm làm cho nhiều nguời không hài lòng
  - Việc gọi trực tiếp những public method của 1 blackbox khi phải sử dụnng nó trong mã nguồn, tạm gọi là nối cứng (hard wired) blackbox vào mã nguồn.
  - Cần có 1 cách nào đó để gọi gián tiếp những public method, hay tạm gọi là nối mềm (soft wired) khi sử dụng những blackbox. Đây là chỗ OOP bỏ sót, và AOP ra đời để đáp ứng nhu cầu này .

## Cài đặt AOP trong java như thế nào

- Một vài ý tưởng để implement AOP trong chương trình của chúng ta:

  - Class-weaving : như đã đề cập ở phần trên.
  - Proxy-based : bạn có thể tưởng tượng nó như là một ví dụ sử dụng >Decorator Pattern. Sử dụng công cụ mã hóa byte code của một số thư viện như JDK proxy, CGLib proxy, chúng ta có thể chặn các lệnh gọi hàm và thêm code của riêng mình để được thực thi trước.
    - JDK proxy : đây là cách đơn giản nhất, nhưng chỉ có thể xử lý với các phương thức public được gọi, không thể xử lý các cuộc gọi nội bộ (các cuộc gọi bắt nguồn từ chính lớp đó).
    - CGLib proxy : yêu cầu chỉnh sửa byte code bị giới hạn và có thể xử lý các lời gọi phương thức private, nhưng vẫn không thể xử lý truy cập thuộc tính trực tiếp.
  - Sử dụng các thư viện sau: Google Guice, AspectJ, Spring AOP.

  ![alt text](https://topdev.vn/blog/wp-content/uploads/2023/01/springaop-process.jpg)

