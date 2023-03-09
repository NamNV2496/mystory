# Java 8

## 1. Giới thiệu Java 8

Oracle đã phát hành một phiên bản Java 8 vào ngày 18/03/2014. Đây là một phiên bản mang tính cách mạng của Java cho nền tảng phát triển phần mềm. Nó bao gồm các nâng cấp khác nhau cho lập trình Java, JVM, Tools và các thư viện.


Một số tính năng mới chủ yếu của Java 8 bao gồm:

- Default method : Cung cấp phương thức mặc định cho Interface.
- Lambda expression : Thêm khả năng xử lý function cho Java.
- Method references : Các hàm tham chiếu theo tên của phương thức thay vì gọi trực tiếp. Sử dụng các function làm tham số.
- Stream API : bao gồm các class, interface và enum để cho phép các hoạt động kiểu function trên các element (phần tử) của một Collection, Array. Nó thực hiện chỉ khi nó yêu cầu (lazy).
- Collection API Enhencements : một số cãi tiến về Collections như stream, parallel stream, map, compute, …
- Annotations : Repeating annotation – cho phép các annotation giống nhau có thể được khai báo nhiều lần cùng một vị trí. Type Annotation – có thể được áp dụng cho bất kỳ kiểu dữ liệu nào(Type), bao gồm: new operator, - type casts, implements clauses và throws clauses.
- Date Time API : cung cấp một số lớp mới trong gói java.time cùng với định dạng thời gian Joda.
- Optional : là một lớp được sử dụng để hạn chế với lỗi NullPointerException trong ứng dụng Java.
- New tools : Các công cụ và tiện ích mới cho trình biên dịch được thêm vào như jdeps.
- Nashorn, JavaScript Engine : là javascript Engine cho phép chạy javascript trên JVM. Nó tương tự như engine V8 cung cấp bởi chrome dùng để chạy Node.js. Nó tương thích với các ứng dụng Node.js trong khi đồng thời hỗ trợ các thư viện Java được gọi thông qua mã javascript chạy trên máy chủ.


![img.png](blog/java/img/java8_1.png)

## 2. Stream API trong Java 8

1 trong những thứ quan trọng nhất của java8 chính là stream data. Stream giúp cho việc thao tác trên collection và array trở nên dễ dàng và tối ưu hơn.


### Một số phương thức của Stream
Trong Java 8, Collection interface được hỗ trợ 2 phương thức để tạo ra Stream bao gồm:

- stream() : trả về một stream sẽ được xử lý theo tuần tự.
- parallelStream() : trả về một Stream song song, các xử lý sau đó sẽ thực hiện song song.

### Các đặc điểm của Java Stream
- Stream không lưu trữ các phần tử của collection hay array. Nó chỉ thực hiện các phép toán tổng hợp (chẳng hạn như filter() và count() mà chúng ta đã thấy trong ví dụ trên để có được stream dữ liệu mong muốn.
- Stream không phải là một cấu trúc dữ liệu (data structure).
- Stream là immutable object. Các hoạt động tổng hợp mà chúng ta thực hiện trên Collection, Array hoặc bất kỳ nguồn dữ liệu nào khác không làm thay đổi dữ liệu của nguồn, chúng chỉ trả lại stream mới. Chúng ta đã thấy ở ví dụ trên là thực hiện filter() các các số chẵn bằng cách sử dụng các hoạt động của stream nhưng nó không thay đổi các phần tử của List numbers.
- Tất cả các hoạt động stream là lazy (lười biếng), có nghĩa là chúng không được thực hiện cho đến khi cần thiết. Để làm được điều này, hầu hết các thao tác với Stream đều return lại một Stream mới, giúp tạo một mắc xích bao gồm một loạt các thao tác nhằm thực thi các thao tác đó một cách tối ưu nhất. Mắc xích này còn được gọi là pipeline.
- Các phần tử của luồng chỉ được truy cập một lần trong suốt vòng đời của Stream. Giống như một Iterator, một Stream mới phải được tạo ra để duyệt lại các phần tử của dữ liệu nguồn.
- Stream không dùng lại được, nghĩa là một khi đã sử dụng nó xong, chúng ta không thể gọi nó lại để sử dụng lần nữa.
- Chúng ta không thể dùng index để truy xuất các phần tử trong Stream.
- Stream hỗ trợ thao tác song song các phần tử trong Collection hay Array.


### So sánh Streams với Collections
Chúng ta sử dụng Stream hoặc Collection khi chúng ta phải làm việc theo danh sách các phần tử.
Collection là cấu trúc dữ liệu chứa các phần tử trong bộ nhớ. Những phần tử này sẽ được tính toán trước khi chúng thực sự được thêm vào Collection.
Ngược lại, Stream không phải là một cấu trúc dữ liệu. Stream là một luồng thực hiện tính toán các phần tử theo yêu cầu. Vì vậy, nó có thể được xem rằng các Collection có các yếu tố tính tức thời (eager), trong khi các Stream có yếu tố tính lười biếng (lazy). `Vì vậy với 1 lượng lớn data thì Collection sẽ tốn nhiều thời gian để load hết data lên rồi mới thực hiện, còn Stream sẽ load liên tục từng phần tử cho đến khi hết.`
Mặc dù chúng ta có thể tạo Stream từ Collection và sử dụng một số phương thức trên Collection. Tuy nhiên, Collection gốc vẫn không thay đổi. Do đó, Stream không thể thay đổi dữ liệu.
Và một đặc điểm quan trọng của Stream là chúng có thể chuyển đổi dữ liệu, vì các hoạt động trên Stream có thể tạo ra một cấu trúc dữ liệu khác, như map() và collect() như trong các ví dụ trên.

### Cách làm việc với Stream trong Java
Như chúng ta đã thấy trong ví dụ trên, hoạt động của luồng có thể được giải thích theo ba giai đoạn:

- Tạo Stream (stream source).
- Thực hiện các thao tác trung gian (intermediate operations) trên stream ban đầu để chuyển đổi nó thành một stream khác và tiếp tục thực hiện các hoạt động trung gian khác. Trong ví dụ trên, hoạt động filter() là hoạt động trung gian. Có thể có nhiều hoạt động trung gian.
- Thực hiện thao tác đầu cuối (terminal operation) trên stream cuối cùng để nhận kết quả và sau đó bạn không thể sử dụng lại chúng. Trong ví dụ trên, phép tính count() là hoạt động đầu cuối.


![img.png](blog/java/img/java8_2.png)

Một Stream pipeline bao gồm: 1 stream source, 0 hoặc nhiều intermediate operation, và 1 terminal operation.


### **Intermediate operations** 

#### filter()

Stream filter() giúp loại bỏ các phần tử dựa trên các tiêu chí nhất định.
```java
    Stream.iterate(1, count -> count + 1)
                .filter(number -> number % 3 == 0)
                .limit(6)
                .forEach(System.out::println);
```
#### skip(), limit()

Ý nghĩa của Stream skip(), limit() hoàn toàn tương tự với OFSET và LIMIT trong SQL.
```java
    List<String> data = Arrays.asList("Java", "C#", "C++", "PHP", "Javascript");
    data.stream() //
            .skip(1) //
            .limit(3) //
            .forEach(System.out::print); // C#C++PHP
```
#### map()
```java
    data.stream()
        .map(String::toUpperCase) // convert each element to upper case
        .forEach(System.out::println);
```
#### sorted()

Stream sorted() giúp sắp xếp các phần tử theo một thứ tự xác định

```java
    data.stream() //
        .sorted((s1, s2) -> s1.length() - s2.length()) //
        .forEach(System.out::println);

```
### **Terminal Operations**


#### forEach()

Phương thức forEach() giúp duyệt qua các phần tử của Stream.

    Stream.iterate(1, count -> count + 1) //
            .filter(number -> number % 3 == 0) //
            .limit(6) //
            .forEach(System.out::println);

#### collect()
Phương thức collect() giúp thu thập kết quả Stream sang một Collection. Đây là 1 phương thức quan trọng nhất
```java
    Stream<String> stream = Stream.of("Java", "C#", "C++", "PHP", "Javascript", "C++");
        List<String> languages = stream.collect(Collectors.toList()); // "Java", "C#", "C++", "PHP", "Javascript", "C++"
        List<String> languages2 = stream.collect(Collectors.toSet()); // "Java", "C#", "C++", "PHP", "Javascript"

    class User {
        String name;
        String role;
    }
    List<User> users = List.of(User.builder().name("name1").role("admin").build(), User.builder().name("name2").role("member").build());
        Map<String, String> mapNameRole = users.stream.collect(Collectors.toMap(User::getName, User::getRole)); // map name1/admin, name2/member
        Map<String, User> mapNameRole = users.stream.collect(Collectors.toMap(User::getName, Function.indentity())); // map name1/{object user1}, name2/{object user2}
```
####  anyMatch(), allMatch(), noneMatch()

Phương thức anyMatch() trả về một boolean tùy thuộc vào điều kiện được áp dụng trên Stream dữ liệu. Phương thức này trả về true ngay khi phần tử đầu tiên thõa mãn điều kiện, những phần tử còn lại sẽ không được kiểm tra.
```java
    List<String> data = Arrays.asList("Java", "C#", "C++", "PHP", "Javascript");
        boolean result = data.stream().anyMatch((s) -> s.equalsIgnoreCase("Java"));
        System.out.println(result); // true
```
#### count()

Phương thức count() trả về tổng số phần tử cho dữ liệu luồng.
```java
    List<Integer> data = Arrays.asList(2, 3, 5, 4, 6);
 
    long count = data.stream().filter(num -> num % 3 == 0).count();
    System.out.println("Count = " + count);
```
#### min(), max()
Stream.max(), Stream.max() chấp nhận đối số là một Comparator sao cho các item trong stream có thể được so sánh với nhau để tìm tối thiểu (min) hoặc tối đa (max).
```java
    Integer []numbers = {1, 8, 3, 4, 5, 7, 9, 6};
         
        // Find max, min with Array ====================
         
        // Max = 9
        Integer maxNumber = Stream.of(numbers)
                .max(Comparator.comparing(Integer::valueOf))
                .get();
 
        // Min = 1
        Integer minNumber = Stream.of(numbers)
                .min(Comparator.comparing(Integer::valueOf))
                .get();
         
        // Find max, min with Collection ====================
        List<Integer> listOfIntegers = Arrays.asList(numbers);
 
        // Max = 9
        Integer max = listOfIntegers.stream()
                .mapToInt(v -> v)
                .max()
                .getAsInt(); 
         
        // Min = 1
        Integer min = listOfIntegers.stream()
                .mapToInt(v -> v)
                .min()
                .getAsInt(); 

    // Ex2
    List<Programing> students = Arrays.asList( //
                new Programing("Java", 5), //
                new Programing("PHP", 2), //
                new Programing("C#", 1) //
        );
 
        // Max = 5
        Programing maxByExp = students.stream()
                .max(Comparator.comparing(Programing::getExp))
                .get(); 
         
        // Min = 1
        Programing minByExp = students.stream()
                .min(Comparator.comparing(Programing::getExp))
                .get(); 
```
#### summaryStatistics()

Phương thức summaryStatistics() được sử dụng để lấy giá trị count, min, max, sum và average với tập dữ liệu số.
```java
    List<Integer> primes = Arrays.asList(2, 3, 5, 7, 10);
 
    IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
    System.out.println("Count: " + stats.getCount());
    System.out.println("Max: " + stats.getMax());
    System.out.println("Min: " + stats.getMin());
    System.out.println("Sum: " + stats.getSum());
    System.out.println("Average: " + stats.getAverage());
```
#### reduce()
Phương thức reduce() kết hợp các phần tử luồng thành một bằng cách sử dụng một BinaryOperator.
```java
    int result = IntStream.of(1, 2, 3, 4).reduce(0, (a, b) -> a + b);
    System.out.println(result); // 10
```
#### flatmap()
```java
    List<String> students1 = new ArrayList<>();
    students1.add("Khanh");
    
    List<String> students2 = new ArrayList<>();
    students2.add("Thanh");
    students2.add("Dung");
    
    List<List<String>> students = Arrays.asList(students1, students2);
    
    Stream<List<String>> stream = students.stream();
    Stream<String> flatMap = stream.flatMap(l -> l.stream()); // [students1[Khanh]], [students2[Thanh, Dung]]

    flatMap.stream().filter(s -> "khanh".equals(s.toString())).forEach(o -> System.out.println(o.toString()));
```
## 3. Luồng song song – Parallel Streams
Như đã đề cập ở trên, các stream có thể là tuần tự (sequential) hoặc song song (parallel). Các thao tác trên các stream tuần tự được thực hiện trên một luồng đơn (single thread) trong khi các phép toán trên các stream song song được thực hiện đồng thời trên nhiều luồng (multi-thread).

![img.png](blog/java/img/java8_3.png)
![img.png](blog/java/img/java8_4.png)
```java
    public static void main(String[] args) {
        List<String> values = createDummyData();
 
        long startTime = System.nanoTime();
 
        long count = values.stream().sorted().count();
        System.out.println(count);
 
        long endTime = System.nanoTime();
 
        long millis = TimeUnit.NANOSECONDS.toMillis(endTime - startTime);
 
        System.out.println(String.format("sequential sort took: %d ms", millis));
        // sequential sort took: 804 ms
 
    }
 
    public static List<String> createDummyData() {
        int max = 1000000;
        List<String> values = new ArrayList<>(max);
        for (int i = 0; i < max; i++) {
            UUID uuid = UUID.randomUUID();
            values.add(uuid.toString());
        }
        return values;
    }


    // parallel

    public static void main(String[] args) {
        List<String> values = createDummyData();
 
        long startTime = System.nanoTime();
 
        long count = values.parallelStream().sorted().count();
        System.out.println(count);
 
        long endTime = System.nanoTime();
 
        long millis = TimeUnit.NANOSECONDS.toMillis(endTime - startTime);
 
        System.out.println(String.format("parallel sort took: %d ms", millis));
        // parallel sort took: 489 ms
 
    }
 
    public static List<String> createDummyData() {
        int max = 1000000;
        List<String> values = new ArrayList<>(max);
        for (int i = 0; i < max; i++) {
            UUID uuid = UUID.randomUUID();
            values.add(uuid.toString());
        }
        return values;
    }
```
## 4. Optional trong Java 8

Trong Java 8, chúng ta có một lớp Optional<T> mới được giới thiệu trong gói java.util. Nó được sử dụng để kiểm tra xem một biến có giá trị tồn tại giá trị hay không. Ưu điểm chính của cấu trúc mới này là không có quá nhiều kiểm tra null và tránh lỗi NullPointerException (NPE) lúc runtime. Giống như Collection và Array, Optional cũng là một vùng chứa chứa nhiều nhất một giá trị. Vùng chứa này có thể chứa bất kỳ đối tượng T nào.

### Một số phương thức của Optional<T>

- static <T> Optional<T> empty() : Trả về một Optional instance rỗng.
- static <T> Optional<T> of(T value) : Trả về đối tượng Optional kiểu T chứa giá trị của value.
- static <T> Optional<T> ofNullable(T value) : Trả về một Optional chứa giá trị được truyền vào nếu khác null, ngược lại sẽ trả về một Optional rỗng.
- T get() : Nếu như có giá trị trong Optional này, nó sẽ trả về giá trị đó, ngược lại sẽ ném ra NoSuchElementException nếu như đối tượng rỗng.
- void ifPresent(Consumer<? super T> consumer) : Nếu như đối tượng Optional đang chứa giá trị, nó sẽ áp dụng consumer được truyền vào cho giá trị của nó. Ngược lại thì không làm gì cả.
boolean isPresent() : Phương thức này được sử dụng để kiểm tra xem trong đối tượng Optional có đang chứa giá trị hay không. Giá trị trả về là True nếu có giá trị và ngược lại trả về false.
- orElse(T other) : Trả về giá trị của đối tượng Optional nếu có, ngược lại nó sẽ trả về đối tượng other mà bạn đã truyền vào phương thức này.
- T orElseGet(Supplier<? extends T> other) : Trả về giá trị nếu tồn tại, ngược lại nó sẽ gọi other mà bạn đã truyền vào sau đó trả về kết quả của Supplier.
- <U>Optional<U> map(Function<? super T,? extends U> mapper) : Nếu như có giá trị nó sẽ áp dụng Function lên giá trị đó và nếu kết quả sau khi áp dụng Function khác null thì nó sẽ trả về một Optional chứa giá trị của kết quả sau khi áp dụng Function.
- <U> Optional<U> flatMap(Function<? super T,Optional<U>> mapper) : Nếu như có giá trị hiện diện trong Optional, nó sẽ áp dụng Function được truyền vào cho nó và trả về một Optional với kiểu U, ngược lại thì sẽ return về một empty Optional.
- Optional<T> filter(Predicate<? super <T> predicate) : Nếu như có giá trị hiện diện trong đối tượng Optional này và giá trị của nó khớp với Predicate truyền vào, nó sẽ trả về một Optional chứa giá trị đó, mặc khác sẽ trả về một Optional rỗng.
- <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) : Nếu như đối tượng Optional có giá trị tồn tại thì nó sẽ trả về giá trị đó, ngược lại sẽ ném ra một Exception do chúng ta định nghĩa bởi Supplier đã truyền vào.


## 5. Collectors trong Java 8

### Collectors.groupingBy()
Collectors.groupingBy() : được sử dụng để nhóm các đối tượng theo một số thuộc tính và lưu trữ các kết quả trong một Map.
```java
    public static void main(String[] args) {
        List<Book> books = Arrays.asList( //
                new Book(1, "A", 1), //
                new Book(2, "B", 1), //
                new Book(3, "C", 2), //
                new Book(4, "D", 3), //
                new Book(5, "E", 1) //
        );
 
        Map<Integer, Set<Book>> result = books.stream()
                .collect(Collectors.groupingBy(Book::getCagegoryId, Collectors.toSet()));
        result.forEach((catId, booksInCat) -> System.out.println("Category " + catId + " : " + booksInCat.size()));
    }

    // output
    Category 1 : 3
    Category 2 : 1
    Category 3 : 1
```
### Collectors.partitioningBy()

Collectors.partitioningBy() : là một trường hợp đặc biệt của Collectors.groupingBy() chấp nhận một Predicate và thu thập các phần tử của Stream vào một Map với các giá trị Boolean như khóa và Collection như giá trị.

- Key = true, là một tập hợp các phần tử phù hợp với Predicate đã cho
- Key = false, là một tập hợp các phần tử không khớp với Predicate đã cho.
```java
    Map<Boolean, Set<Book>> partitioningBy = books.stream()
    .collect(Collectors.partitioningBy(b -> b.getCagegoryId() > 2, Collectors.toSet()));
    System.out.println(partitioningBy);
    
    //output

    {
    false=[Book [id=3, title=C, cagegoryId=2], 
        Book [id=2, title=B, cagegoryId=1], 
        Book [id=1, title=A, cagegoryId=1], 
        Book [id=5, title=E, cagegoryId=1]], 
    true=[Book [id=4, title=D, cagegoryId=3]]
    }
```
### Collectors.reducing()
Collectors.reducing() : thực hiện giảm các phần tử đầu vào của nó trong một BinaryOperator được chỉ định.

- identity : giá trị khởi tạo để thực hiện reduction (cũng là giá trị được trả về khi không có phần tử đầu vào).
- op : một BinaryOperator<T> được sử dụng để giảm các phần tử đầu vào.

```java
    // Find employees with the maximum age of each company
    Comparator<Employee> ageComparator = Comparator.comparing(Employee::getAge);

    Map<String, Optional<Employee>> map = list.stream().collect(
            Collectors.groupingBy(Employee::getCompanyName,
                    Collectors.reducing(BinaryOperator.maxBy(ageComparator))));

    map.forEach((k, v) -> System.out.println(
            "Company: " + k + 
            ", Age: " + ((Optional<Employee>) v).get().getAge() + 
            ", Name: " + ((Optional<Employee>) v).get().getName()));
        
    // Summary salary
    Integer bonus = 30;
    Integer totalSalaryExpense = list.stream()
            .map(emp -> emp.getSalary())
            .reduce(bonus, (a, b) -> a + b);
    System.out.println("Total salary expense: " + totalSalaryExpense);

    // output

    Company: A, Age: 23, Name: Emp2
    Company: B, Age: 22, Name: Emp3
    Total salary expense: 250
```
Ref: https://gpcoder.com/3973-lop-collectors-trong-java-8/#CollectorstoMap



