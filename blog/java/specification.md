# Spring Data JPA Specifications


Spring Data JPA Specifications là một công cụ khác mà chúng tôi sử dụng để thực hiện các truy vấn cơ sở dữ liệu với Spring hoặc Spring Boot.
Specifications được xây dựng dựa trên Criteria API.
Khi tạo một truy vấn Criteria chúng ta bắt buộc phải tụ xây dựng và quản lý các đối tượng Root, CriteraQuery, và CriteriaBuilder:

``` java
    EntityManager entityManagr = getEntityManager();

    CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery<Product> productQuery = builder.createQuery(Product.class);

    Root<Person> personRoot = productQuery.from(Product.class);
``` 

    
Vấn đề với các query methods là chúng ta chỉ có thể chỉ định một số tiêu chí cố định. Ngoài ra, số lượng các query methods tăng lên nhanh chóng khi các trường hợp sử dụng tăng lên.

Tại một số thời điểm, có nhiều tiêu chí chồng chéo trên các query methods và nếu có sự thay đổi trong bất kỳ tiêu chí nào trong số đó, chúng ta sẽ phải thực hiện các thay đổi trong nhiều query methods.

Ngoài ra, độ dài của query methods có thể tăng đáng kể khi chúng ta có tên trường dài và nhiều tiêu chí trong truy vấn của mình. Ngoài ra, có thể mất một lúc để ai đó hiểu được một truy vấn dài dòng như vậy và mục đích của nó:
``` java
    List<Product> findAllByNameLikeAndCategoryIn(
                  String name,
                  List<Category> categories
                  );
    List<Product> findAllByNameLikeAndCategoryInAndPriceBetween(
                    String name,
                    List<Category> categories,
                    Double bottom,
                    Double top
                    );
    }
```

Với Specifications, chúng ta có thể giải quyết những vấn đề này bằng cách tạo atomic predicates. Và bằng cách đặt cho predicates đó một cái tên có ý nghĩa, chúng ta có thể xác định rõ mục đích của chúng. Chúng ta sẽ có thể chuyển đổi những điều trên thành một truy vấn có ý nghĩa hơn nhiều trong phần viết các truy vấn với Specifications.

Specifications cho phép chúng ta viết các truy vấn linh hoạt theo yêu cầu. Do đó, chúng ta có thể xây dựng các truy vấn động dựa trên đầu vào của người dùng. Chúng ta sẽ xem điều này chi tiết hơn trong phần truy vấn động với Specifications.

## Cài đặt
chúng ta cần cài đạt jpa model gen để class tĩnh
``` java

    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-jpamodelgen</artifactId>
        <version>6.0.0.Alpha7</version>
        <type>pom</type>
        <scope>provided</scope>
    </dependency>
    
```

Trong repository cần extends JpaSpecificationExecutor
``` java
    interface ProductRepository extends JpaRepository<Product, String>,
                  JpaSpecificationExecutor<Product> {
    }



    private Specification<Product> nameLike(String name){
    return (root, query, criteriaBuilder)
        -> criteriaBuilder.like(root.get(Product_.NAME), "%"+name+"%");
    }

    public List<Product> getPremiumProducts(String name,
                    List<Category> categories) {
    return productRepository.findAll(
        where(belongsToCategory(categories))
            .and(nameLike(name))
            .and(isPremium()));
    }

    private Specification<Product> belongsToCategory(List<Category> categories){
    return (root, query, criteriaBuilder)->
        criteriaBuilder.in(root.get(Product_.CATEGORY)).value(categories);
    }

    private Specification<Product> isPremium() {
    return (root, query, criteriaBuilder) ->
        criteriaBuilder.and(
            criteriaBuilder.equal(
                root.get(Product_.MANUFACTURING_PLACE)
                            .get(Address_.STATE),
                STATE.CALIFORNIA),
            criteriaBuilder.greaterThanOrEqualTo(
                root.get(Product_.PRICE), PREMIUM_PRICE));
    }


    // có thể join các bảng khác
    Join<Copier, MasterPortfolio> portfolioJoin = root.join(CopierQuery_.MASTER_PORTFOLIO);
    Join<Copier, Customer> customerJoin = root.join(CopierQuery_.CUSTOMER);
    root.fetch(CopierItemQuery_.RESTRUCTURE_LOG_QUERY_LIST, JoinType.LEFT)
        .fetch(CopierRestructureItemLogQuery_.ORDER_QUERY_LIST, JoinType.LEFT)
        .fetch(CopierOrderQuery_.ORDER_MATCH_QUERY_LIST, JoinType.LEFT);

    // tạo list các predicate để add 1 lượt
    Path<LocalDateTime> approveDatePath = portfolioJoin.get(MasterPortfolio_.APPROVE_DATE);

    Predicate statusPredicate = cb.equal(portfolioJoin.get(MasterPortfolio_.STATUS), MPStatusCode.ACTIVE.getCode());
    Predicate startDatePredicate = cb.greaterThanOrEqualTo(approveDatePath, startDate);
    Predicate endDatePredicate = cb.lessThanOrEqualTo(approveDatePath, endDate);
    Predicate emptyEntryPricePredicate = cb.isNull(root.get(MasterPortfolioItemQuery_.ENTRY_PRICE));

```

To call
``` java
    Sort.Order order = Sort.Order.desc(AuditingEntity_.UPDATED_DATE);

    Pageable pageable = PageRequest.of(requestDTO.getPage(), requestDTO.getSize(), Sort.by(order));

    Page<Product> return = ProductRepository.findALL(getPremiumProducts, pageable);

```


















