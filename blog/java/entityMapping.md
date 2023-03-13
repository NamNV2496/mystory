# entityMapping

## Ta sẽ nghe về Oracle (SQL) và mongoDB (No-SQL) thế nó khác biệt gì?

![img.png](blog/java/img/entityMapping1.png)

![img.png](blog/java/img/entityMapping2.png)


## RDBMS là gì?

RDBMS là viết tắt của Relational Database Management System có nghĩa là hệ quản trị cơ sỡ dữ liệu quan hệ. RDBMS là cơ sở cho SQL, và cho tất cả các hệ thống cơ sở dữ liệu hiện đại như MS SQL Server, IBM DB2, Oracle, MySQL và Microsoft Access.

 * Chính vì thể hiện sự rằng buộc nên sẽ sinh ra các mối quan hệ 1-1, 1-n, n-1

### => bạn có chắc là bạn nắm rõ hết cách khai thác các mối quan hệ này ko?

![img.png](blog/java/img/entityMapping3.png)

**Ví dụ:** Xuất phát trong bảng `world` có tên 1 quốc gia `countries`. từ mã `countries.code` ta lấy đc nhiều `users` và nhiều `police`

ta sẽ phải viết:
- query theo tên quốc gia lấy từ `world.country_name` truy vấn cột `countries.name` trong bảng `countries`
- query theo `users` dựa trên thông tin từ cột `code` trong kết quả `List<countries>` vừa tìm đc ở bước 1
- query theo `police` dựa trên thông tin từ cột `code` trong kết quả `List<countries>` vừa tìm đc ở bước 1

### => ít nhất 3 lần truy vấn gây phiền phức

## Solution:
Đứng tại bảng `countries` truy vấn ra 2 bảng `users` và `police` theo quan hệ 1-n (@OneToMany)


```java
@Table(name = "countries")
public class Country {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;
    String continent_name;

    @OneToMany(mappedBy = "countryInUserTable")
    private List<User> users;

    @OneToMany(mappedBy = "countryInPoliceTable")
    private List<Police> polices;
}



@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String full_name

    @Column(name = "country_code")
    Country countryInUserTable;
}
@Table(name = "police")
public class Police {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String full_name;
    String area;

    @Column(name = "country_code")
    Country countryInPoliceTable;
}
```


# Cùng Tìm hiểu về các dạng relationship trong RDBMS nhé

Có 3 dạng:

- One to One (1-1)
- One to Many (1-n)
- Many to One (n-1)

# 1. @OneToOne

## Call OneToOne from people

```java

@Table(name = "people")
public class People {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id", referencedColumnName = "add_id")
//    @PrimaryKeyJoinColumn
    private Address address;
}


@Table(name = "address")
public class Address {

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  @Column(name = "add_id")
  Long id;
  String street;
  String born;
}
```

![img.png](blog/java/img/entityMapping4.png)


because in database we have address_id. so we can redirect to any address

Example: people 1 has address 1 or 2 or 3 or 4

    GET http://localhost:8080/oneToOne

Result:

```json
[
    {
        "id": 1,
        "name": "test1",
        "address": null
    },
    {
        "id": 2,
        "name": "test2",
        "address": {
            "id": 2,
            "street": "Cau Giay",
            "born": "HN"
        }
    }
]
```
## Call OneToOne from address
because address doesn't have people_id column. So it will map with address by add_id

It means address id 1 will map with people id 1.

It cannot redirect to another id. it means if people doesn't have id 1 so address id 1 will have data in people property is null

```java
@Table(name = "address")
public class AddressOneToOne {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "add_id")
    Long id;
    String street;
    String born;

    @OneToOne
    private People people;
}
@Table(name = "people")
public class People {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;
    @Column(name = "address_id")
    Long addresses;
}
```


    GET http://localhost:8080/oneToOne2

Result:

```json
[
  {
    "id": 2,
    "street": "Cau Giay",
    "born": "HN",
    "people": {
      "id": 2,
      "name": "test2",
      "addresses": 2
    }
  },
  {
    "id": 3,
    "street": "Lang",
    "born": "HN",
    "people": null
  }
]
```


# 2. @OneToMany


```sql
INSERT INTO db_example.people (id, name,address_id) VALUES
	 (1, 'test1',2),
	 (2, 'test2',2);
INSERT INTO db_example.address (add_id,born,street) VALUES
    (3,'HN','Lang'),
    (2,'HN','Cau Giay');


```
![img.png](blog/java/img/entityMapping5.png)


## => Vậy nên ta sẽ đứng từ phía `address` để gọi

![img.png](blog/java/img/entityMapping6.png)


only need define @OneToMany in 1 side and config mapped with reference_column in n side

    // mapping with column address_id in people table
    @OneToMany(mappedBy = "addresses")
    private List<PeopleOneToMany> people;

```java

@Table(name = "address")
public class AddressOneToMany {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "add_id")
    Long id;
    String street;
    String born;
    // mapping with column address_id in people table
    @OneToMany(mappedBy = "addresses")
    private List<PeopleOneToMany> people;
}
@Table(name = "people")
public class PeopleOneToMany {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;

    // this is column in people table
    @Column(name = "address_id")
    Long addresses;

}
```
## Request

    http://localhost:8080/oneToMany

```json
[
    {
        "id": 2,
        "street": "Cau Giay",
        "born": "HN",
        "people": [
            {
                "id": 1,
                "name": "test1",
                "addresses": 2
            },
            {
                "id": 2,
                "name": "test2",
                "addresses": 2
            }
        ]
    },
    {
        "id": 3,
        "street": "Lang",
        "born": "HN",
        "people": []
    }
]
```

# 3. @ManyToOne

```sql
INSERT INTO db_example.people (id, name,address_id) VALUES
	 (1, 'test1',2),
	 (2, 'test2',2);
INSERT INTO db_example.address (add_id,born,street) VALUES
    (3,'HN','Lang'),
    (2,'HN','Cau Giay');


```

![img.png](blog/java/img/entityMapping8.png)


## => Vậy nên ta sẽ đứng từ phía `people` để gọi


![img.png](blog/java/img/entityMapping7.png)

only need define in n side

    @ManyToOne
    @JoinColumn(name = "address_id", referencedColumnName = "add_id")
    Address addresses;

```java
@Table(name = "people")
public class PeopleManyToOne {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;

    @ManyToOne
    @JoinColumn(name = "address_id", referencedColumnName = "add_id")
    Address addresses;
}

@Table(name = "address")
public class Address {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "add_id")
    Long id;
    String street;
    String born;
}

```

## Request

    http://localhost:8080/manyToOne

```json
[
    {
        "id": 1,
        "name": "test1",
        "addresses": {
            "id": 2,
            "street": "Cau Giay",
            "born": "HN"
        }
    },
    {
        "id": 2,
        "name": "test2",
        "addresses": {
            "id": 2,
            "street": "Cau Giay",
            "born": "HN"
        }
    }
]
```



Ref: https://github.com/NamNV2496/EntityMapping
