# Liquibase

Liquibase support việc quản lý Database tạo mới, thêm cột, chỉnh sửa dạng dữ liệu của từng cột, dummy data và chạy các câu SQL khi ứng dụng start up

## Cơ chế quản lý database thế nào?

- Liquibase sử dụng các **changeSet** được mã hóa và lưu lại trong **databasechangelog**. Với mỗi changeSet đã được thực hiện thay đổi lên Database thì chúng ta sẽ không thể thay đổi changeSet đó 1 lần nữa (Vì nếu thay đổi thì sẽ mã hóa changeSet này sẽ bị thay đổi và bị databasechangelog quản lý để refuse request thay đổi)

## Vậy thì làm sao để thay đổi Database khi bị quản lý?

- Ta sẽ phải tạo ra 1 changeSet khác can thiệp vào table đó trong database.

```text

    <changeSet author="admin" id="role"> <!-- this is author information-->
        <createTable tableName="ROLE"> <!-- name of table-->
            <column name="id" type="BIGINT" autoIncrement="true"> <!--int 4 bytes -->
                <constraints nullable="false" primaryKey="true" primaryKeyName="pk_user_id" />
            </column>
            <column name="name" type="VARCHAR(255)">
                <constraints nullable="false"/>
            </column>

        </createTable>
        <createSequence cacheSize="0"  minValue="0" sequenceName="User_SEQ" startValue="1"/>
    </changeSet>
    <changeSet author="admin" id="role">
        <dropColumn tableName="ROLE">
            <column name="name" type="VARCHAR(50)">
            </column>
        </dropColumn>
    </changeSet>
```

Hoặc run 1 câu SQL

```text
    <changeSet id="insertData" author="admin">
        <sqlFile path="db/changelog/insert-data.sql"/>
    </changeSet>
```
### Note: Chỉ tạo Sequence 1 lần

## Cài đặt
```text
application.yml

spring:
    liquibase:
        change-log: classpath:db/changelog/changelog-master.xml
        enabled: true
 
Và cần tắt entity auto generate table tự động của hibernate
jpa:
    hibernate:
        ddl-auto: none
```
change-log file trỏ đến file **changelog-master** là file tổng hợp các changeSet để chạy.


Ref: https://github.com/NamNV2496/liquibase

