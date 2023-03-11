# hibernate Lifecycle

# 1. Giới thiệu Hibernate Lifecycle

## Có 4 states chính của 1 vòng đời hibernate

- Transient State
- Persistent State
- Detached State
- Removed State

![img.png](blog/java/img/hibernateLifecycle1.png)

## 1. Transient State (Tạm thời)

Đây là trạng thái đầu tiên của entity object. Khi ta tạo mới 1 object từ POJO class sử dụng toán tử `new` thì object sẽ ở trạng thái 'transient'. Object này đang không được kết nối với bất kỳ session nào đồng nghĩa nó không kết nói với bất kỳ 1 bảng nào trong database. Vì vậy, khi ta thay đổi bất kỳ 1 thông tin nào trong object này thì data trong database sẽ không thay đổi. `transient object tồn tại trong Heap memory`


![img.png](blog/java/img/hibernateLifecycle2.png)
![img.png](blog/java/img/hibernateLifecycle3.png)

Sẽ có 2 layout mà transient state thực hiện:
- Khi object được tạo ra bởi application nhưng ko kết nối vs bất kỳ session nào
- Những object được tạo ra bởi 1 closed session

```java
Employee em = new Employee();

em.setName("test");
em.setAge(5);
```

## 2. Persistent State (bền vững)

1 object được kết nối với hibernate session thì Object sẽ được chuyển sang trạng thái `Persistent`. Lúc này tất cả những thay đổi trong object sẽ được apply vào bảng trong database khi sử dụng các method lưu.

Có 2 cách để chuyển từ 'Transient State' sang 'Persistent State':

- sử dụng hibernate session để lưu entity object vào bảng trong database
- sử dụng hibernate session để load entity object từ bảng trong database

![img.png](blog/java/img/hibernateLifecycle4.png)

Các methods giúp chuyển qua trạng thái 'Persistent':

- session.persist(e);
- session.save(e);
- session.saveOrUpdate(e);
- session.update(e);
- session.merge(e);
- session.lock(e);

```java
// Transient State
Employee e = new Employee("Neha Shri Rudra", 21, 180103); 

//Persistent State
session.save(e);
```

## 3. Detached State (Đã bị tách riêng ra)

Đối tượng đã từng có trạng thái persistent nhưng hiện tại đã không còn giữ quan hệ với Session. Tức là khi chúng ta đóng session hoặc xóa cache thì những thay đổi tiếp theo đó sẽ ko được apply vào trong bảng trong database. Nhưng nếu muốn ta cũng có thể reconnect vào session để thực hiện thay đổi data với các methods sau:

- merge()
- update()
- load()
- refresh()
- save()
- update()

Các methods sử dụng để chuyển qua 'detached state':
- session.detach(e);
- session.evict(e);
- session.clear();
- session.close();

![img.png](blog/java/img/hibernateLifecycle5.png)

```java
// Transient State
Employee e = new Employee("Neha Shri Rudra", 21, 180103);

// Persistent State 
session.save(e); 

// Detached State
session.close(); 
```

## 4. Removed State (đã bị xóa)

đây là trạng thái cuối cùng. Trong trạng thái này, khi object bị xóa khởi database sau đó object sẽ được hiểu là ở 'removed state'. Nó được hoàn thành khi gọi toán tử `delete()`. Tất cả các sự thay đổi trong object sẽ ko được apply vào bảng trong database

![img.png](blog/java/img/hibernateLifecycle6.png)

```java
// Transient State
Employee e = new Employee();     
Session s = sessionfactory.openSession();
e.setId(01);

// Persistent State
session.save(e)  

// Detached
session.detach(e)

// return Persistent State
session.save(e)  

// Removed State                 
session.delete(e);   
```

# Summary

![img.png](blog/java/img/hibernateLifecycle7.png)

- (1) Transient: Trường hợp bạn tạo mới một đối tượng java từ một Entity, đối tượng đó có tình trạng là Transient. Hibernate không biết về sự tồn tại của nó. Nó nằm ngoài sự quản lý của Hibernate.
- (2) Persistent: Trường hợp bạn lấy ra đối tượng Entity bằng method get, load hoặc find, bạn có được một đối tượng nó tương ứng với 1 record dưới database. Đối tượng này có trạng thái Persistent. Nó được quản lý bởi Hibernate. Khi đối tượng ở trạng thái persistent, tất cả các thay đổi mà bạn thực hiện đối với đối tượng này sẽ được áp dụng cho các bản ghi và các trường cơ sở dữ liệu tương ứng khi flush session.
- (3) Transient -> Persistent: Session gọi một trong các method save, saveOrUpdate, persist, merge sẽ đẩy đối tượng Transient vào sự quản lý của Hibernate và đối tượng này chuyển sang trạng thái Persistent. Tùy tình huống nó sẽ insert hoặc update dữ liệu vào DB.
- (4) Persistent -> Detached: Session gọi evict(..) hoặc clear() để đuổi các đối tượng có trạng thái persistent (bền vững) ra khỏi sự quản lý của Hibernate, giờ các đối tượng này sẽ có trạng thái mới là Detached (Bị tách ra).  Nếu nó không được đính (Attached) trở lại, nó sẽ bị bộ gom rác của Java quét đi theo cơ chế thông thường.
- (5) Detached -> Persistent: Sử dụng update(..), saveOrUpdate(..), merge(..) sẽ đính trở lại các đối tượng Detached vào lại. Tùy tình huống nó sẽ tạo ra dưới DB câu lệnh update hoặc insert. Các đối tượng sẽ trở về trạng thái Persistent (bền vững).
- (6) Persistent -> Removed: Session gọi method remove(..), delete(..) để xóa một bản ghi, đối tượng persistent giờ chuyển sang trạng thái Removed (Đã bị xóa).

# 2. Thao tác Select, Insert, Update, Delete (CRUD) với Hibernate


## 2.1 Persistent
Trước khi đi vào chi tiết từng phương thức, các bạn nên nhớ rằng tất cả các phương thức như persist, save, update, merge, saveOrUpdate, remove, … không ngay lập tức thực thi các câu lệnh SQL UPDAT, INSERT hoặc DELETE tương ứng. Việc thực thi câu lệnh SQL thực tế vào cơ sở dữ liệu xảy ra khi commit transaction hoặc flush session.

Các phương thức được đề cập về cơ bản quản lý trạng thái của các thể hiện của thực thể bằng cách chuyển chúng giữa các trạng thái khác nhau theo vòng đời của đối tượng trong Hibernate.

![img.png](blog/java/img/hibernateLifecycle8.png)

### Phương thức load()

Dùng để load một đối tượng từ database lên, nó sẽ có trạng thái persistent, throw ObjectNotFoundException nếu id không tồn tại.

- Chỉ sử dụng method load() khi chắc chắn rằng đối tượng tồn tại trong database.
- Method load() sẽ ném ra 1 exception nếu đối tượng không tìm thấy trong database.
- Method load() chỉ trả về 1 đối tượng giả (proxy object) nó chỉ lấy dữ liệu từ database ra khi cần tới.

* Proxy Object là 1 đối tượng giả, nó chỉ có id, các thuộc tính khác không được khởi tạo, ví dụ khi bản để FETCH_TYPE = LAZY khi mapping thì nó cũng chỉ trả về 1 proxy object.

### Phương thức get()

Giống load(), tuy nhiên trả về null nếu không tồn tại.

- Nếu không chắc chắn rằng đối tượng có tồn tại trong database không thì hãy dùng get().
- Method get() sẽ trả về null nếu không tìm thấy đối tượng trong database.
- Method get() sẽ truy xuất vào database ngay lập tức để lấy đối tượng thực đang tồn tại.

### Phương thức find()

Cách thức hoạt động tương tự như get(). Sự khác biệt giữa find() và get() là:

- find() là một đặc tả của JPA, get() là đặc tả riêng của Hibernate.
- Một điểm khác biệt nữa về mặt ngữ nghĩa sử dụng: find() có thể có kết quả hoặc không, get() sẽ luôn trả về một vài thứ gì đó thậm chí là null.

```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    System.out.println("- Loading user 1");
    User user1 = session.load(User.class, 1L);
    System.out.println("- After called ");
    System.out.println("- Fullname of user 1 " + user1.getFullname());
 
    System.out.println("---");
 
    System.out.println("- Getting user 2");
    User user2 = session.get(User.class, 2L);
    System.out.println("- After called ");
    System.out.println("- Fullname of user 2 " + user2.getFullname());
 
    System.out.println("---");
 
    System.out.println("- Finding user 3");
    User user3 = session.find(User.class, 3L);
    System.out.println("- After called ");
    System.out.println("- Fullname of user 3 " + user3.getFullname());
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```
```console
- Loading user 1
- After called
Hibernate: select user0_.id as id1_3_0_, user0_.created_at as created_2_3_0_, user0_.fullname as fullname3_3_0_, user0_.modified_at as modified4_3_0_, user0_.password as password5_3_0_, user0_.username as username6_3_0_ from user user0_ where user0_.id=?
- Fullname of user 1 Changed 2nd
---
- Getting user 2
Hibernate: select user0_.id as id1_3_0_, user0_.created_at as created_2_3_0_, user0_.fullname as fullname3_3_0_, user0_.modified_at as modified4_3_0_, user0_.password as password5_3_0_, user0_.username as username6_3_0_ from user user0_ where user0_.id=?
- After called
- Fullname of user 2 Hibernate Example
---
- Finding user 3
Hibernate: select user0_.id as id1_3_0_, user0_.created_at as created_2_3_0_, user0_.fullname as fullname3_3_0_, user0_.modified_at as modified4_3_0_, user0_.password as password5_3_0_, user0_.username as username6_3_0_ from user user0_ where user0_.id=?
- After called
- Fullname of user 3 Hibernate Example
```

### Nên sử dụng phương thức nào?
Phương thức get()/ find() thực thi SQL để lấy data ngay khi được gọi, trong khi phương thức load() trả về một proxy object, câu lệnh SQL thực sự chỉ được thực thi cần thiết. Vì vậy load() tốt hơn về performance bởi vì nó hỗ trợ lazy loading.

Phương thức load() sẽ throw exception khi không tìm thấy dữ liệu, vì vậy chúng ta chỉ sử dụng khi đã biết dữ liệu chắc chắn tồn tại.

Phương thức get()/ find() được sử dụng khi ta muốn make sure dữ liệu tồn tại trong database.


## 2.2 Transient –> Persistent

![img.png](blog/java/img/hibernateLifecycle9.png)

### Phương thức persist()

Phương thức persist được thiết kế để thêm một thể hiện mới của thực thể vào persistent context, tức là chuyển một thể hiện từ trạng thái Transient sang trạng thái Persistent. Sau khi gọi persit(), đối tượng bây giờ trong persistence context, nhưng chưa được lưu vào cơ sở dữ liệu. Việc tạo ra các câu lệnh SQL INSERT sẽ chỉ xảy ra khi commit transaction, flush hoặc close session.

Ta thường sử dụng persit() khi ta muốn thêm một bản ghi vào cơ sở dữ liệu. Persit() hoạt động trên đối tượng được truyền “tại chỗ”, thay đổi trạng thái của chính nó và tham chiếu đến đối tượng tồn tại thực tế.

```java
Category cat = new Category();
cat.setName("Java");
session.persist(cat); // void
```

### Phương thức save()


Phương thức save() là original Hibernate API, nó không thuộc đặc tả JPA. Mục đích của nó về cơ bản giống như persist(), nhưng implementation details của nó thì khác. Phương thức này sẽ tạo ra một định danh, đảm bảo rằng nó sẽ return the Serializable của định danh này.

```java
Category cat = new Category();
cat.setName("Java");
 
Long id = (Long) session.save(cat);
System.out.println("Cat id = " + id); // 5
 
Long id2 = (Long) session.save(cat);
System.out.println("Cat id = " + id2); // 5
```
Hiệu quả của việc lưu một persisted instance là giống như với persist. Sự khác biệt xuất hiện khi ta cố gắng lưu một detached instance:

```java
Category cat = new Category();
cat.setName("Java");
 
Long id = (Long) session.save(cat);
System.out.println("Cat id = " + id); // 6
 
session.evict(cat);
 
Long id2 = (Long) session.save(cat);
System.out.println("Cat id = " + id2); // 7
```
Như bạn thấy, sau khi gọi evict():

- Id được save lần đầu khác với id được save lần sau.
- Khi save trên một detached instance sẽ tạo ra một persistent instance mới và gán nó cho một định danh mới. Kết quả là bị duplicate bản ghi trong database khi commit hoặc flush.

### Phương thức merge()

Mục đích chính của phương thức merge() là update một persistent entity instance với các giá trị mới từ một detached entity instance.

Phương thức này hoạt động như sau:

- Tìm một entity instance bằng id lấy từ đối tượng được truyền (hoặc một existing entity instance từ persistence context được lấy ra, hoặc một new instance được load ra từ database).
- Sao chép các trường từ đối tượng được truyền vào instance vừa tìm được.
- returns instance mới đã được update, đối tượng trả về có trạng thái persistence, còn đối tượng ban đầu vẫn sẽ không bị quản lý bởi session.
Trong ví dụ sau, ta evict (detach) thực thể đã lưu khỏi context, thay đổi name field, và sau đó merge với detached entity.


```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    Category cat = new Category();
    cat.setName("Java");
 
    Long id = (Long) session.save(cat);
    System.out.println("saved object is managed by hibernate session = " + session.contains(cat));
 
    session.evict(cat);
    System.out.println("evicted object is managed by hibernate session = " + session.contains(cat));
 
    cat.setName("Hibernate");
 
    Category cat2 = (Category) session.merge(cat);
 
    System.out.println("merged object is managed by hibernate session = " + session.contains(cat2));
    System.out.println("Saved object is equals with merged object = " + (cat == cat2));
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```

```console
Hibernate: insert into Category (name) values (?)
saved object is managed by hibernate session = true
Hibernate: select category0_.id as id1_0_0_, category0_.name as name2_0_0_ from Category category0_ where category0_.id=?
evicted object is managed by hibernate session = false
merged object is managed by hibernate session = true
Saved object is equals with merged object = false
Hibernate: update Category set name=? where id=?
```
Lưu ý rằng phương thức merge trả về một đối tượng – nó là đối tượng được merge, được load vào persistent context và được update, không phải đối tượng category mà ta đã truyền làm đối số. Đó là hai đối tượng khác nhau, và đối tượng category thường cần phải được loại bỏ sau đó.

Như với phương thức persist(), phương thức merge() được chỉ định bởi JSR-220 để có một số ngữ nghĩa nhất định mà bạn có thể dựa vào:

- Nếu thực thể là detached, nó được sao chép trên một existing persistent entity.
- Nếu thực thể là transient, nó được sao chép trên một newly created persistent entity.
- Hoạt động cascades cho tất cả các mối quan hệ : cascade = MERGE hoặc cascade = ALL.

### Phương thức update()

Tương tự như với persist() và save(), phương thức update() là một “original” Hibernate API method đã tồn tại trước khi phương thức merge được thêm vào. Ngữ nghĩa của nó khác nhau ở một số điểm chính:

- Nó hoạt động khi đối tượng được truyền là một persistence entity hoặc detached entity.
- Phương thức này chuyển đổi đối tượng được truyền từ trạng thái detached thành trạng thái persistent.
- Phương thức này ném một ngoại lệ nếu ta truyền vào nó một transient entity.

```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
    Category cat = new Category();
    cat.setName("Java");
 
    session.save(cat);
    System.out.println("saved object is managed by hibernate session = " + session.contains(cat));
 
    session.evict(cat);
    System.out.println("evicted object is managed by hibernate session = " + session.contains(cat));
 
    cat.setName("Hibernate");
 
    session.update(cat);
    System.out.println("updated object is managed by hibernate session = " + session.contains(cat));
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
```

```console
Hibernate: insert into Category (name) values (?)
saved object is managed by hibernate session = true
evicted object is managed by hibernate session = false
updated object is managed by hibernate session = true
Hibernate: update Category set name=? where id=?
```

### Phương thức SaveOrUpdate()

Phương thức này chỉ xuất hiện trong Hibernate API. Tương tự như update(), nó cũng có thể được sử dụng cho các trường hợp attached lại. Sự khác biệt chính của phương thức update() và saveOrUpdate() là nó không ném ngoại lệ khi được áp dụng cho một transient instance.

Phương thức này hoạt động như sau: nếu là đối tượng mới thì save xuống database, nếu không thì update xuống database. Nó sẽ thực hiện SELECT để kiểm tra trước khi save hoặc update.

- Nếu  đối tượng đã tồn tại trong session thì nó không làm gì cả.
- Nếu tồn tại 1 đối tượng khác trong cùng session mà có cùng id thì sẽ xảy ra exception.
- Nếu đối tượng tượng không có id (chưa tồn tại) thì sẽ thực hiện save().
- Nếu đối tượng có id nhưng id đó chưa có trong database thì thực hiện save().
- Các trường hợp còn lại thực hiện update().

```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    Category cat = new Category();
    cat.setName("Java");
 
    session.saveOrUpdate(cat);
    System.out.println("saved object is managed by hibernate session = " + session.contains(cat));
 
    session.evict(cat);
    System.out.println("evicted object is managed by hibernate session = " + session.contains(cat));
 
    cat.setName("Hibernate");
 
    session.saveOrUpdate(cat);
    System.out.println("updated object is managed by hibernate session = " + session.contains(cat));
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```
```console
Hibernate: insert into Category (name) values (?)
saved object is managed by hibernate session = true
evicted object is managed by hibernate session = false
updated object is managed by hibernate session = true
Hibernate: update Category set name=? where id=?
```


Như bạn thấy phương thức này là một công cụ phổ biến để làm cho một đối tượng persistent bất kể trạng thái của nó là transient hay detached.

Bây giờ hãy xem trường hợp một đối tượng bị giữ bởi 2 session, khi đó phương thức saveOrUpdate() sẽ throw ra một ngoại lệ.

```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    Category cat = new Category();
    cat.setId(20L);
    cat.setName("Java");
    session.saveOrUpdate(cat);
 
    Category cat2 = new Category();
    cat2.setId(20L);
    cat2.setName("Java");
    session.saveOrUpdate(cat2);
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```
```console
Exception in thread "main" org.hibernate.NonUniqueObjectException: A different object with the same identifier value was already associated with the session : [com.gpcoder.entities.Category#20]

```
### Nên sử dụng phương thức nào?

Nếu không có bất kỳ yêu cầu đặc biệt nào, theo quy tắc chung, ta nên tuân thủ các phương thức persist() và merge(), vì chúng được chuẩn hóa và được đảm bảo để tuân theo đặc tả JPA. Nó sẽ dễ dàng hơn trong trường hợp ta quyết định chuyển sang một persistence provider khác, chẳng hạn iBatis, Eclipse Link, OpenJPA, … Tuy nhiên, một số trường hợp đặc biệt các phương thức Hibernate “gốc” như save(), update() và saveOrUpdate() sẽ hữu dụng hơn nhiều.


## 2.3 Persistent –> Detached

![img.png](blog/java/img/hibernateLifecycle10.png)

### Phương thức evict()

Tách một đối tượng ra khỏi session, biến nó từ trạng thái persistent thành detached.

Các bạn có thể xem lại các ví dụ trên để thấy được cách sử dụng evict().

### Phương thức clear()

Tách tất cả đối tượng ra khỏi session, biến tất cả chúng từ trạng thái persistent thành detached.

```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    Category cat = new Category();
    cat.setName("Java");
    session.saveOrUpdate(cat);
 
    Category cat2 = new Category();
    cat2.setName("Hibernate");
    session.saveOrUpdate(cat2);
 
    System.out.println("cat1 is managed by hibernate session = " + session.contains(cat));
    System.out.println("cat2 is managed by hibernate session = " + session.contains(cat2));
 
    session.clear();
 
    System.out.println("After clear session");
 
    System.out.println("cat1 is managed by hibernate session = " + session.contains(cat));
    System.out.println("cat2 is managed by hibernate session = " + session.contains(cat2));
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```

```console
Hibernate: insert into Category (name) values (?)
Hibernate: insert into Category (name) values (?)
cat1 is managed by hibernate session = true
cat2 is managed by hibernate session = true
After clear session
cat1 is managed by hibernate session = false
cat2 is managed by hibernate session = false
```

## 2.4 Detached –> Persistent

![img.png](blog/java/img/hibernateLifecycle11.png)

### Phương thức refresh()

```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    Category cat = new Category();
    cat.setName("Java");
    session.saveOrUpdate(cat);
 
    System.out.println("saved object is managed by hibernate session = " + session.contains(cat));
 
    session.evict(cat);
 
    System.out.println("evicted object is managed by hibernate session = " + session.contains(cat));
 
    session.refresh(cat);
 
    System.out.println("refreshed object is managed by hibernate session = " + session.contains(cat));
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```
```console
Hibernate: insert into Category (name) values (?)
saved object is managed by hibernate session = true
evicted object is managed by hibernate session = false
Hibernate: select category0_.id as id1_0_0_, category0_.name as name2_0_0_ from Category category0_ where category0_.id=?
refreshed object is managed by hibernate session = true
```

## 2.5 Persistent –> Removed

### Phương thức delete()

Phương thức này không nằm trong đặc tả JPA, nó là một original hibernate.

Chúng ta cần load đối tượng lên và xoá nó đi thông qua phương thức delete().

Nếu không muốn load lên thì dùng session.createQuery(“DELETE FROM user WHERE …”).executeUpdate();


```java
try (Session session = HibernateUtils.getSessionFactory().openSession();) {
    // Begin a unit of work
    session.beginTransaction();
 
    Category cat = new Category();
    cat.setName("Java");
 
    session.save(cat);
    System.out.println("saved object is managed by hibernate session = " + session.contains(cat));
 
    session.delete(cat);
    System.out.println("deleted object is managed by hibernate session = " + session.contains(cat));
 
    // Commit the current resource transaction, writing any unflushed changes to the database.
    session.getTransaction().commit();
}
```
```console
Hibernate: insert into Category (name) values (?)
saved object is managed by hibernate session = true
deleted object is managed by hibernate session = false
Hibernate: delete from Category where id=?
```

### Phương thức remove()

Phương thức này nằm trong đặc tả JPA, và hoạt động tương tự như delete().
