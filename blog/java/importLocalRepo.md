# **How to import local jar to your project or jar file which build from another project?**

# Có 2 cách để import local jar vào trong project
- 1 là import trong tiếp trong local folder
- 2 là import thông qua server online


## Ưu nhược điểm của từng phương thức
### +) **Phương thức 1:**
- **Ưu điểm**: ta có thể import từ 1 file `jar` bất kỳ từ đâu đó. Có thể ở trên mạng, hoặc từ 1 bên cung cấp nào đó mà họ chỉ cho ta file jar chứ không cho ta source.
- **Nhược điểm**: Ta phải có 1 file `jar` dạng chuẩn (tức là file dùng để làm library để import vào service khác, chứ không phải thực thi bình thường ta hay build bằng deploy hoặc package)

### +) **Phương thức 2:**

- **Ưu điểm**: load động mà không cần copy file `jar` vào thư mục trong project. Có thể dễ dàng cập nhật version bất cứ lúc nào

- **Nhược điểm**: ta phải có source của project import và cần config server (ở đây dùng Nexus)

=> tùy vào từng nhu cầu để chúng ta tự lựa chọn cách thức phù hợp


# **import local repository**
Example in:
- importLocalRepo
- demo

## create standard java library


![img.png](blog/java/img/importLocalRepo1.png)

run package to create library

![img_1.png](blog/java/img/importLocalRepo2.png)

the output in target folder

![img_2.png](blog/java/img/importLocalRepo3.png)

Lưu ý là file cần dùng là file không có `-exec`. về bản chất thì đây chỉ là file build ra các file có đuôi `.class` và package dạng chuẩn để chúng ta có thể import


Khác biệt giữa 2 file thường và file chạy (đuôi -exec)

![img_2.png](blog/java/img/importLocalRepo22.png)

## import standard java library to target project

copy the lib jar file to /lib folder

![img_3.png](blog/java/img/importLocalRepo4.png)


config in pom to import

## **Note**: ở đây cần lưu ý bắt buộc phải có `<scope>system</scope>` để load file dạng tĩnh mà ko cần compile (mặc định nếu ko khai báo là complie)

![img_4.png](blog/java/img/importLocalRepo5.png)

get groupId, artifactId, version from pom of importLocalRepo

![img_6.png](blog/java/img/importLocalRepo7.png)

now we can import and use package of importLocalRepo

![img_5.png](blog/java/img/importLocalRepo6.png)


# **import repository by maven from NEXUS**

Example in:
- importNexusServer
- demo


install nexus on docker

    docker pull sonatype/nexus3

![img_7.png](blog/java/img/importLocalRepo8.png)

**run on cmd**

    docker run -d -p 8081:8081 --name nexus sonatype/nexus3

![img_9.png](blog/java/img/importLocalRepo10.png)

Access `http://localhost:8081/`

![img_10.png](blog/java/img/importLocalRepo11.png)

access to get password

![img_11.png](blog/java/img/importLocalRepo12.png)

![img_12.png](blog/java/img/importLocalRepo13.png)

sign-in to web
![img_13.png](blog/java/img/importLocalRepo14.png)



## Config to deploy jar to Nexus
Copy `settings.xml` to .m2 folder

    C:\Users\{user}\.m2

trong file này ta sẽ config về link đến server nexus và authentication để upload và download file từ server về


![img_16.png](blog/java/img/importLocalRepo17.png)

Edit file with authorization

![img_17.png](blog/java/img/importLocalRepo18.png)


![img_14.png](blog/java/img/importLocalRepo15.png)

![img_15.png](blog/java/img/importLocalRepo16.png)

check maven on Nexus

![img_18.png](blog/java/img/importLocalRepo19.png)

## Import to demo app

![img_20.png](blog/java/img/importLocalRepo20.png)

# **Result**

![img_21.png](blog/java/img/importLocalRepo21.png)




Ref: https://github.com/NamNV2496/importLocalRepository


