# SQLSequence


SQL chắc hẳn đã quá quen thuộc với rất nhiều bạn. Khi làm việc với SQL, chắc chắn rằng bạn sẽ phải làm việc với những lệnh sau: INSERT, SELECT, UPDATE, DELETE,... . Đặc biệt với SELECT là một câu truy vấn khá phức tạp, SELECT có thể được áp dụng lồng trong UPDATE, DELETE để lọc ra đúng dữ liệu cần thực hiện truy vấn.

![img.png](blog/java/img/sqlsequence1.png)

## Trình tự xử lý của câu lệnh SELECT

```sql
[ WITH { [ XMLNAMESPACES ,] [ <common_table_expression> ] } ]

SELECT select_list [ INTO new_table ]

[ FROM table_source ] [ WHERE search_condition ]

[ GROUP BY group_by_expression ]

[ HAVING search_condition ]

[ ORDER BY order_expression [ ASC | DESC ] ]
```

Các bước sau đây cho thấy thứ tự xử lý logic hoặc thứ tự ràng buộc cho câu lệnh SELECT. Thứ tự này xác định trong bước một được cung cấp cho các mệnh đề trong các bước tiếp theo. Việc thực thi câu lệnh sẽ theo trình tự dưới đây:

- FROM
- ON
- JOIN
- WHERE
- GROUP BY
- WITH CUBE or WITH ROLLUP
- HAVING
- SELECT
- DISTINCT
- ORDER BY
- TOP

## 1. FROM

Mệnh đề FROM trong SQL được dùng để liệt kê các bảng cần thiết sử dụng trong truy vấn. Chính vì vậy khi thực hiện SELECT thì nó phải biết SELECT từ đâu, nên đương nhiên FROM nó sẽ phải chạy trước để xác định được những bảng cần dùng đến.

**Cú pháp**:

```sql
FROM table1
[ { INNER JOIN
| LEFT OUTER JOIN
| RIGHT OUTER JOIN
| FULL OUTER JOIN } table2
ON table1.column1 = table2.column1 ]
```
## 2. ON
Như ở trên ta thấy ON đi kèm với FROM, mục đích là để xác định điều kiện ràng buộc giữa 2 bảng với nhau, nó sẽ match dữ liệu ở 2 bảng theo điều kiện này, thường thực tế nó sẽ match các Id là chủ yếu.

## 3. JOIN

Mệnh đề JOIN được dùng để kết hợp các bản ghi từ hai hay nhiều bảng trong một Database. Các loại JOIN gồm có: INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN, SELF JOIN, CARTESIAN. Các bạn có thể search google về các loại này để tìm hiểu kỹ thêm nhé.

## 4. WHERE

Đây là mệnh đề được dùng để lọc kết quả bởi các điều kiện, kết quả trả về phải đáp ứng các điều kiện trong mệnh đề này. Khi đã có các table đích để lấy dữ liệu, thì chắc hẳn mới đến mệnh đề WHERE để lọc lại dữ liệu đó.

**Cú pháp**:

```sql 
SELECT column1, column2, columnN 
FROM table_name
WHERE [conditions]
```

## 5. GROUP BY

Mệnh đề GROUP BY được sử dụng để kết hợp với câu lệnh SELECT để sắp xếp dữ liệu giống nhau thành các nhóm, nó tuân theo mệnh đề WHERE và đứng trước ORDER BY.

Cú pháp:

SELECT column1, column2
FROM table_name
WHERE [ conditions ]
GROUP BY column1, column2
ORDER BY column1, column2
## 6. WITH CUBE or WITH ROLLUP

Đây là mệnh đề mở rộng của GROUP BY, sử dụng để phát sinh các tổng trung gian từ các cột trong GROUP BY. Ở đây bạn có thể tìm hiểu thêm hàm GROUPING khi sử dụng CUBE và ROLLUP. Vì thế hiển nhiên nó sẽ chạy sau mệnh đề GROUP BY.

Ví dụ:
```sql
SELECT Id, ProductName, AVG(Price) as [AveragePrice] 
FROM Product 
GROUP BY Id, ProductName WITH ROLLUP;
```
## 7. HAVING

Mệnh đề này cho phép bạn khả năng để xác định các điều kiện để lọc nhóm kết quả nào sẽ xuất hiện trong kết quả cuối cùng.

**Cú pháp**:

```sql
SELECT column1, column2
FROM table1, table2
WHERE [ conditions ]
GROUP BY column1, column2
HAVING [ conditions ]
ORDER BY column1, column2
```
## 8. SELECT

Dùng để lấy kết quả từ một hay nhiều bảng. Sau khi các truy vấn thực hiện đến các bảng cần sử dụng, các điều kiện lấy giữ liệu, nhóm các dữ liệu, thì SELECT sẽ quyết định lấy về những thông tin nào (các cột) trong những bảng nào.

Ví dụ:
```sql
SELECT b.Id, b.Name, t.TypeName

FROM Books b INNER JOIN Type t ON b.TypeId = t.Id
Ví trụ trên SELECT sẽ lấy ra các thông tin từ bảng Books là: Id, Name và thông tin từ bảng Type là: TypeName

SELECT sẽ kết hợp với các mệnh đề khác để trở thành một câu truy vấn hoàn chỉnh, tuỳ thuộc vào yêu cầu lấy dữ liệu như thế nào.
```
## 9. DISTINCT

Mệnh đề Distinct được dùng kết hợp với SELECT để loại bỏ các bản ghi trùng lặp, chỉ lấy một bản ghi duy nhất trong kết quả trả về.

Ví dụ:
```sql
SELECT DISTINCT b.Id, b.Name, b.Price, t.TypeName

FROM Books b INNER JOIN Type t ON b.TypeId = t.Id
```
Mục đích sử dụng của nó như trên, nên hiển nhiên nó phải chạy sau SELECT rồi, phải SELECT ra được data thì mới có data để mà xét trùng đúng không?

## 10. ORDER BY

Mệnh đề này dùng để sắp xếp dữ liệu theo thứ tự tăng dần hoặc giảm dần dựa trên một hoặc nhiều cột. Lệnh ASC được sử dụng để sắp xếp tăng giần, còn lệnh DESC được sử dụng để sắp xếp theo giảm dần. Mặc định ASC nếu không chỉ rõ.

**Cú pháp**:

```sql
SELECT column-list

FROM table_name

[WHERE condition]

[ORDER BY column1, column2, .. columnN] [ASC | DESC];
```
Khi có đủ dữ liệu trả về thì đúng theo như yêu cầu đặt ra, khi đó chúng ta mới sắp xếp lại trật tự của chúng.

## 11. TOP

Mệnh đề dùng để lấy ra N bản ghi hoặc X phần trăm bản ghi từ kết quả trả về.

Cú pháp:
```sql
SELECT TOP number|percent column_name(s)

FROM table_name

WHERE [condition]
```




