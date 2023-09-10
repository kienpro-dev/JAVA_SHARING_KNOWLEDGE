# INDEX - Đánh chỉ mục trong Database

# Đánh chỉ mục là gì?
- Đánh chỉ mục giúp các cột được truy vấn nhanh hơn bằng cách tạo ra những chỉ dẫn tới nơi mà dữ liệu được lưu trữ trong CSDL.

- Khi bạn muốn tìm kiếm một bản ghi trong database dựa trên dữ liệu đầu vào, máy tính sẽ phải tìm theo từng bản ghi một cho đến khi tìm được bản ghi mong muốn. Nếu dữ liệu mà bạn đang tìm kiếm ở tận cuối cùng và lượng bản ghi lớn thì việc truy vấn sẽ tốn rất nhiều thời gian.

- Vì vậy việc đánh chỉ mục cho phép chúng ta tạo các danh sách được sắp xếp mà không cần phải tạo một bảng với dữ liệu được sắp xếp mới. Điều này giúp ta tiết kiệm rất nhiều bộ nhớ.

- Ví dụ khi tìm bản ghi với name="Zack" ở dưới, mất 8 lần để tìm
![alt text](https://media.techmaster.vn/api/static/bpc8thf0k7qhpi6j07lg/bvtgvt451cobimh85mbg)

- Sau khi đánh chỉ mục theo name tăng dần, việc tìm kiếm chỉ tốn 3 lần vì name đã được sắp xếp them Alphabet
![alt text](https://media.techmaster.vn/api/static/bpc8thf0k7qhpi6j07lg/bvtgvt451cobimh85mg0)

- Các chỉ mục cho phép chúng ta tạo các danh sách được sắp xếp mà không cần phải tạo một bảng với dữ liệu được sắp xếp mới. Điều này giúp ta tiết kiệm rất nhiều bộ nhớ.

# INDEX trong Database
- INDEX là một cấu trúc mà trong đó chứa đựng trường mà chỉ mục đang sắp xắp và liên kết từ nó tới dữ liệu tương ứng ở bảng dữ liệu gốc – nơi mà dữ liệu thực sự được lưu trữ. Có thể hiểu chỉ mục giống như một cuốn sách thần kỳ, nó lưu trữ thông tin theo thứ tự ta ghi vào và có hai chế độ đọc. Chế độ nguyên bản nơi dữ liệu như đúng chú ta ghi (bảng dữ liệu gốc) và chế độ sắp xếp giúp ta tìm kiếm thông tin nhanh hơn (cấu trúc chỉ mục).

- Hiểu đơn giản thì INDEX là một cách tối ưu hiệu suất truy vấn database bằng việc giảm lượng truy cập vào bộ nhớ khi thực hiện truy vấn

- Để dễ hình dung, chúng ta dùng ví dụ ở trên và xem cách mà chỉ mục kết nối tới bảng dữ liệu gốc ra sao:
![alt text](https://media.techmaster.vn/api/static/bpc8thf0k7qhpi6j07lg/bvtgvt451cobimh85mc0)

- Chúng ta thấy rằng, bảng Friends được sắp xếp theo id tăng dần mỗi khi ta thêm dữ liệu mới. Còn bảng Index được lưu trữ với việc sắp xếp tên theo anphabet.

# Cấu trúc của INDEX
- Index  gồm:
    - Cột Search Key: chứa bản sao các giá trị của cột được tạo Index
    - Cột Data Reference: chứa con trỏ trỏ đến địa chỉ của bản ghi có giá trị cột index tương ứng
![alt text](https://images.viblo.asia/c44b421d-0e4d-4bf9-966e-ddc03bc75993.png)

# Các kiểu INDEX
1. B-TREE INDEX: Thông thường khi nói đến INDEX mà không chỉ rõ loại INDEX thì default là sẽ sử dụng B-Tree INDEX.

- Cú pháp:
```sql
    -- Tạo INDEX sử dụng CREATE INDEX
    CREATE INDEX id_index ON table_name (column_name[, column_name…]) USING BTREE;

    -- Tạo INDEX sử dụng ALTER TABLE
    ALTER TABLE table_name ADD INDEX id_index (column_name[, column_name…]);

    -- Xóa INDEX khỏi TABLE 
    DROP INDEX index_name ON table_name;
```
- Đặc điểm của B-Tree INDEX:
    - Một cấu trúc dữ liệu tương tự với cây nhị phân (binary tree). 
    - Mỗi B-tree là “cấu trúc dữ liệu cây tự cân bằng nơi đảm bảo việc dữ trữ dữ liệu đã sắp xếp và cho phép tìm kiếm, truy cập tuần tự, thêm và xóa với với thời gian theo O(log(n)) thay vì O(n)”. 
    - Về cơ bản, nó tạo một cây – giống kiểu cấu trúc mà dữ liệu được sắp xếp để tìm kiếm nhanh.
    - B-Tree INDEX được sử dụng trong các biểu thức so sánh dạng: =, >, >=, <, <=, BETWEEN và LIKE. ⇒ Có thể tối ưu tốt cho câu lệnh ORDER BY
![alt text](https://media.techmaster.vn/api/static/bpc8thf0k7qhpi6j07lg/bvtgvt451cobimh85me0)

- Trên đây là một B-tree của chỉ mục. Đầu vào nhỏ nhất là ở điểm ngoài cùng bên trái, lớn nhất là ngoài cùng bên phải. Tất cả truy vấn có thể bắt đầu ở node trên cùng và đi dần xuống, nếu node mục tiêu nhỏ hơn node hiện tại thì sẽ di chuyển sang bên trái, nếu node mục tiêu lớn hơn thì di chuyển sang bên phải. Ở ví dụ trên nó sẽ di chuyển như sau: Matt => Todd => Zack.

- Để tăng tích hiệu quả, rất nhiều B-tree sẽ giới hạn số phần tử bạn nhập vào một mục. B-tree sẽ tự làm việc đó và không yêu cầu giới hạn dữ liệu cột. Ở ví dụ trên, B-tree giới hạn số mục ở 4.

- Các chỉ mục dùng B-TREE INDEX sử dụng cách tìm kiếm tối ưu là tìm kiếm nhị phân - binary search.

- So sánh việc sử dụng phương pháp này để truy vấn ở ví dụ đầu tiên, chúng ta đã giảm số lần tìm kiếm từ 8 xuống 3. Nếu sử dụng phương pháp này cho bảng có 1,000,000 chỉ mục thì nó giảm xuống chỉ còn 20 bước với binary search.
![alt text](https://media.techmaster.vn/api/static/bpc8thf0k7qhpi6j07lg/bvtgvt451cobimh85mfg)

2. HASH INDEX
- Hash index dựa trên giải thuật Hash Function (hàm băm). Tương ứng với mỗi khối dữ liệu (index) sẽ sinh ra một bucket key (giá trị băm) để phân biệt.

- Cú pháp:
```sql
    -- Tạo INDEX sử dụng CREATE INDEX
    CREATE INDEX id_index ON table_name (column_name[, column_name…]) USING HASH;

    -- Tạo INDEX sử dụng ALTER TABLE
    ALTER TABLE table_name ADD INDEX id_index (column_name[, column_name…]) USING HASH;
```
![alt text](https://i1.wp.com/tech.vtijapan.co.jp/wp-content/uploads/2019/09/hU4Tc.png?resize=728%2C502&ssl=1)

- Đặc điểm của HASH INDEX:
    - Dữ liệu index được tổ chức theo dạng Key - Value được liên kết với nhau.
    - Khác với B-Tree, thì Hash index chỉ nên sử dụng trong các biểu thức toán tử là = và <>. Không sử dụng cho toán từ tìm kiếm 1 khoảng giá trị như > hay < .
    - Không thể tối ưu hóa toán tử ORDER BY bằng việc sử dụng Hash index bởi vì nó không thể tìm kiếm được phần từ tiếp theo trong Order.
    - Toàn bộ nội dung của Key được sử dụng để tìm kiếm giá trị records, khác với B-Tree một phần của node cũng có thể được sử dụng để tìm kiếm.
    - Hash có tốc độ nhanh hơn kiểu B-Tree.

# Sử dụng INDEX sao cho hiệu quả
- Dù Index đóng vai trò quan trọng trong việc tối ưu truy vấn và tăng tốc độ tìm kiếm trong Database nhưng nhược điểm của nó là tốn thêm bộ nhớ để lưu trữ. Do vậy, việc Index cho các cột phải được tính toán, tránh lạm dụng.

- Dưới đây là một số Tips giúp bạn tạo Database index hiệu quả hơn:
    - Nên Index những cột được dùng trong WHERE, JOIN và ORDER BY
    - Dùng chức năng "index prefix" or "multi-columns index” của MySQL. Vd: Nếu bạn tạo Index(first_name, last_name) thì k cần tạo Index(first_name)
    - Dùng thuộc tính NOT NULL cho những cột được Index
    - Không dùng Index cho các bảng thường xuyên có UPDATE, INSERT
    - Không dùng Index cho các cột mà giá trị thường xuyên bị thay đổi
    - Dùng câu lệnh EXPLAIN giúp ta biết được MySQL sẽ chạy truy vấn ra sao. Nó thể hiện thứ tự join, các bảng được join như thế nào. Giúp việc xem xét để viết truy vấn tối ưu, chọn cột để Index dễ dàng hơn

# Kiểm tra hiệu suất bằng EXPLAIN
- Để kiểm tra xem đánh chỉ mục có giúp giảm thời gian hay không, bạn có thể  chạy tệp lệnh truy vấn, ghi thời gian nó cần để hoàn thành và sau đó tạo chỉ mục và chạy lại bài test.

- Nếu sử dụng PostgreSQL, sử dụng EXPLAIN ANALYZE:
```sql
    EXPLAIN ANALYZE SELECT * FROM friends WHERE name = 'Blake';
```

- Đối với Database nhỏ -> vừa, ta có kết quả sau:
![alt text](https://media.techmaster.vn/api/static/bpc8thf0k7qhpi6j07lg/bvtgvt451cobimh85mh0)

- Kết quả cho bạn biết phương pháp tìm kiếm nào từ kế hoạch truy vấn đã được chọn và việc lập kế hoạch và thực hiện truy vấn mất bao lâu.

- Chỉ tạo duy nhất một chỉ mục tại một thời điểm vì không phải tất cả các chỉ mục sẽ giảm bới thời gian truy vấn
    - Kế hoạch truy vấn của PostgreSQL khá hiệu quả do đó tạo chỉ mục mới có thể không ảnh hưởng hiệu suất tốc độ truy vấn.
    - Thêm chỉ mục sẽ đồng nghĩa với việc lưu trữ thêm dữ liệu
    - Thêm chỉ mục làm tăng thời gian update CSDL sau khi thêm dữ liệu.

# Tổng kết
- Lập chỉ mục có thể làm giảm đáng kể thời gian của các truy vấn
- Mỗi bảng có khóa chính đều có một chỉ mục nhóm
- Mỗi bảng có thể có nhiều chỉ mục không phân cụm để hỗ trợ việc truy vấn
- Các chỉ mục không phân cụm giữ các con trỏ quay lại bảng chính
- Không phải mọi cơ sở dữ liệu sẽ được hưởng lợi từ việc lập chỉ mục
- Không phải mọi chỉ mục sẽ tăng tốc độ truy vấn cho cơ sở dữ liệu