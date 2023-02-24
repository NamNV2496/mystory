# Agile scrum / waterfall


Đây là 1 điều vô cùng quan trọng nếu muốn tham gia lĩnh vực lập trình. Đa phần các JD bây giờ đều yêu cầu biết và đã từng làm việc vs agile scrum (mới) hoặc waterfall


## **Waterfall là gì?**

Phương pháp luận của mô hình Waterfall tuân theo 1 quy trình tuần tự, tuyến tính. Đây là mô hình phổ biến nhất của vòng đời phát triển hệ thống (SDLC) dành cho phát triển phần mềm và các dự án IT


![img.png](blog/java/img/agile1.png)

**Ưu điểm:**

- Mô hình Waterfall phù hợp nhất với các dự án đơn giản, không có thay đổi. Các bước tuần tự khiến cho việc áp dụng mô hình dễ dàng hơn và các tài liệu trong dự án cũng sẽ được mô tả chi tiết hơn.

- Dễ sử dụng và quản lý: Mô hình Waterfall thực hiện theo các công đoạn tuần tự, giống nhau cho mỗi dự án, vì thế dễ hiểu và dễ áp dụng. Đối với các dự án tương tự nhau, team không cần được training trước khi bắt đầu làm việc trong dự án. Waterfall cũng tương đối cứng nhắc, mỗi công đoạn sẽ có 1 danh sách các sản phẩm, nhờ vậy sẽ dễ dàng hơn trong việc quản lý các sản phẩm và việc review sản phẩm.

- Mô hình có kỷ luật: Mỗi công đoạn trong mô hình Waterfall đều có điểm bắt đầu và điểm kết thúc, từ đó dễ dàng chia sẻ tiến độ công việc với các bên liên quan. Nếu team tập trung và công đoạn tìm hiểu yêu cầu khách hàng và thiết kế trước khi tiến hành coding thì có thể giảm được các rủi ro không kịp deadline.

- Đòi hỏi cao về tài liệu: Waterfall yêu cầu cần có tài liệu cho mỗi công đoạn, nhờ vậy đảm bảo được việc hiểu yêu cầu và logic của chương trình tốt hơn. Ngoài ra các tài liệu của dự án có thể được sử dụng tiếp trong các dự án khác, hoặc cung cấp cho các bên liên quan khi cần confirm chi tiết về 1 công đoạn nào đó.

**Nhược điểm:**

- Điểm yếu lớn nhất của Waterfall chính là làm thế nào để quản lý các thay đổi. The biggest drawback of Waterfall is how it handles change. Team không thể đảo giữa các công đoạn, ngay cả khi có thay đổi xảy ra.
Khi dự án đi đến công đoạn chạy test và phát hiện ra việc bị thiếu 1 chức năng trong yêu cầu của khách hàng, thì việc quay lại để sửa rất đắt đỏ và khó thực hiện.

- Sản phẩm được deliver muộn: Dự án sẽ cần hoàn thành 2 đến 4 công đoạn trước khi công đoạn coding thực sự bắt đầu, vì thế các bên liên quan sẽ phải đợi đến các giai đoạn cuối để thể thấy được sản phẩm.

- Việc thu thập chính xác các yêu cầu ngay từ đầu là 1 thử thách. Ở 1 trong số các công đoạn đầu tiên trong dự án, team cần trao đổi với khách hàng và các bên liên quan để làm rõ các yêu cầu. Tuy nhiên, sẽ rất khó khăng để xác định chính xác họ muốn gì ngay từ đầu dự án. Trong nhiều trường hợp, khách hàng thậm chí vẫn chưa biết họ muốn gì ở giai đoạn này.


+) Có 7 công đoạn cơ bản khi áp dụng mô hình Waterfall


**Conception:** Công đoạn này có thể bắt đầu với 1 ý tưởng. Đây là giai đoạn đánh giá ban đầu của dự án: tại sao cần phát triển dự án? lợi ích đem lại là gì? và các ước lượng ban đầu về cost.

**Initiation:** Khi ý tưởng đã thành hình, bạn cần thuê team dự án, định nghĩa mục tiêu, phạm vị, mục đích và sản phẩm của dự án.

**Requirement Gathering and Analysis:** Các yêu cầu được tập hợp và phân tích để đánh giá tính khả thi. Tất cả thông tin cần được lưu và tài liệu requirement specification document.

**Design:** Các thiết kế tạo trong công đoạn này được dùng làm input cho công đoạn coding. Các yêu cầu của dự án cần được study, phân tích và thiết kế xem sẽ xử lý thế nào trong hệ thống. Mục tiêu của team là để hiểu những action cần thực hiện và sản phẩm sẽ có hình dung như thế nào.

**Implementation/Coding:** Giai đoạn tạo coding, tất cả các nội dung trong công đoạn design cần được chuyển đổi thành ngôn ngữ lập trình.

**Testing:** Sau khi coding kêt thúc, phần mềm cần được test xem có lỗi không. Sau khi việc test kết thúc, chương trình được chuyển cho khách hàng. Ở 1 số dự án sẽ thực hiện user acceptance testing (UAT) trước khi chương trình được deploy lên môi trường hoạt động thực sự.

**Maintenance:** Khi khách hàng sử dụng hệ thống, họ có thể tìm thấy những vấn đề chưa ổn. Team phát triển cần thay đổi chương trình để chương trình chạy hiệu quả.


## **Agile là gì?**

Agile là mô hình phát triển phần mềm linh hoạt, dựa trên phương thức lặp (iterative) và tăng trưởng (incremental). Nói một cách đơn giản, agile hoạt động theo flow sau:

`Phát triển chức năng → Test → Release sản phẩm cho khách hàng → Khách hàng phản hồi lại → Thay đổi và test lại → Release sản phẩm`

Một vòng lặp của agile thường ngắn, từ 1,2 tuần đến 1 tháng. Trong agile, các yếu tố teamwork, tính trách nhiệm và communicate face-to-face được khuyến khích. Stakeholder và đội phát triển sẽ làm việc cùng nhau để điều chỉnh sản phẩm dần mapping với mong muốn của khách hàng.


**12 nguyên tắc là kim chỉ nam và thước đo để quyết định một quy trình phát triển phần mềm có là Agile hay không?**

- Làm thỏa mãn khách hàng nhanh và liên tục bằng cách delivery các phần mềm có giá trị
- Welcome change spec trong cả vòng đời. Agile giúp tăng tính cạnh tranh của sản phẩm khi luôn có các bước cập nhật nhanh chóng
- Deliver sản phẩm thường xuyên, từ khoảng 2 tuần đến vài tháng, với chu kì ngắn.
- Những người hiểu biết về business và đội dự án cần làm việc với nhau hằng ngày
- Xây dựng dự án xoay quanh các cá nhân, tăng motivated cho team bằng cách cung cấp cho họ môi trường làm việc cởi mở, và tin họ sẽ hoàn thành công việc.
face-to-face luôn là cách truyền đạt thông tin nhanh và hiệu quả nhất
- Thước đo chính của process là một software chạy được
- Quy trình Agile khuyến khích phát triển bền vững và nhịp độ phát triển liên tục
- Liên tục quan tâm đến kĩ thuật và thiết kế để cải tiến sự linh hoạt
- Sự đơn giản là cần thiết – thay vì tuân theo một quy tắc cứng nhắc, đơn giản hóa cách truyền đạt thông, làm task
- Team tự tổ chức cách thức làm việc, đối ứng với các vấn đề gặp phải
- Thích ứng thường xuyên với sự thay đổi

**Ưu điểm:**

- Change luôn được chấp nhận: với vòng đời phát triển trong thời gian ngắn, việc đối ứng change request hay các yêu cầu mới trở nên đơn giản.Luôn luôn có cơ hội để tinh chỉnh và sắp xếp lại công việc tồn đọng, cho phép các nhóm giới thiệu các thay đổi cho dự án chỉ trong vài tuần.
- End-goal can be unknown: Agile rất thích hợp với project thì mà mục tiêu cuối cùng chưa được clear. Xét về mặt quy trình phát triển, mục tiêu đặt ra trở nên nhẹ nhàng hơn và đội phát triển có thể dễ dàng đáp ứng được việc spec tăng dần
- Delivery nhanh và có chất lượng cao: Breaking down project vào các vòng lặp thời gian ngắn cho phép team hướng đến việc phát triển, tesing chất lượng cao. Bug được tìm ra và đối ứng một cách nhanh chóng.
- Sự tương tác mạnh mẽ trong team: Agile đề cao sự tương tác thường xuyên trong team, đặc biệt là face-to-face. Việc này đem lại sự gắn kết và mọi người có thể hiểu được tránh nhiệm của bản thân trong dự án.
- Khách hàng luôn được lắng nghe: khách hàng có nhiều cơ hội để trải nghiệm sản phẩm nhanh chóng, xác nhận và đưa ra các feedback nhanh chóng. Ngoài ra, việc đồng hành với dự án, giúp tăng lòng tin của họ về sản phẩm cuối
- Continuous improvement: Agile giúp cho product trưởng thành qua từng vòng lặp nhờ feedback của khách hàng và nỗ lực của team member. Mỗi bài học rút ra sẽ giúp ích cho vòng lặp sau


**Nhược điểm:**

- Độ chi tiết của Planning sẽ kém: Agile sẽ khó để đặt ra được một mốc delivery cố định. Bởi vì agile dựa trên delivery thời gian ngắn, các task đôi khi lặp lại dẫn đến một vài mục tiêu delivery không được đảm bảo deadline. Thêm nữa, việc add thêm một sprint có thể kéo dài plan của cả dự án.
- Team phải ngon: team agile thường nhỏ, và đòi hỏi team member phải ở mức highly skill, đảm bảo yếu tố chuyên môn để có thể "tin tưởng"
- Time được commit từ đội phát triển: cách tiếp cận Agile đôi khi gây mất thời gian hơn cách thực hiện thông thường, rủi ro của nó đến từ việc bất hơp lý hay xung đột trong quá trình làm việc, khi lượng đầu công việc mà team member phải tự điều chỉnh tăng lên
- Document bị xem nhẹ: thông thường, đội phát triển chỉ quan tâm các tài liệu tổng thể và các requirement để có thể phát triển nhanh phần mềm, các loại tài liệu khác bị xem nhẹ. Việc này giúp cho việc quản lý thông tin trở nên khó khăn, và đôi khi khó có thể chấp nhận với khách hàng khó tính như khách hàng Nhật.
- Final product can be very different: việc thay đổi liên tục trong quá trình có thể khiến cho dự án dần mất đi mục đích ban đầu, dẫn đến mục tiêu khác đem đến tính rủi ro cao hơn.

![img.png](blog/java/img/agile2.png)

**Planning:** Team ngồi với nhau để xác định các chức năng, chia nhỏ chúng vào các phase trên cơ sở xem xét tính khả thi, độ ưu tiên và feedback từ khách hàng.
**Requirements analysis:** phân tích yêu cầu, giai đoạn này đòi hỏi các cuộc meeting liên tục với sự tham gia của managers, stakeholders, and users. Đội dự án cần tập hợp và đề xuất ra cách để quản lý, định lượng
**Design:** design để có view về những gì sẽ phát triển, chiến lược test của được phát triển từ phase này
Implementation, coding or development: implement và test các chức năng.
**Testing:** test để đảm bảo sản phẩm thực sự giải quyết được customer needs và matching user stories. Unit testing, integration testing, system testing, and acceptance testing là các hình thức test được khuyến khích áp dụng.
**Deployment:** Sau khi test, một sản phẩm chạy được sẽ được đưa đến tay khách hàng. Khách hàng sẽ dùng thử và đưa ra feedback cùng các yêu cầu mới. Đội dự án cần xác định là yêu cầu và thay đổi plan cho phù hợp

## **Scrum là gì?**


Scrum là 1 dạng của mô hình Agile và là framework phổ biến nhất khi thực hiện mô hình Agile. Scrum là mô hình phát triển phần mềm lặp đi lặp lại. Những khoảng lặp cố định, thường kéo dài 1, 2 tuần được gọi là sprint. Cuối mỗi sprint, các stakeholder và team member sẽ cùng ngồi lại với nhau và lên kế hoạch cho giai đoạn tiếp theo.

Scrum quy định những roles, trách nhiệm và những loại meeting cố định. Ví du, scrum quy quy định các công đoạn của từng sprint cần bao gồm Sprint planning, daiy stand-up, sprint demo và tổng kêt sprint. Trong suốt mỗi sprint, team sử dụng các công cụ như task board hay burndown charts để nhìn tiến độ, và nhận các phản hồi.


**Ưu điểm của Scrum**


- Tình trạng dự án được thể hiện rõ ràng: Với việc thực hiện họp hàng ngày, thường là đứng họp, cả team sẽ biết được ai đang làm gì, tránh được những hiểu nhầm, sự không rõ ràng. Các vấn đề trong dự án được chỉ ra hàng ngày và team có thể giải quyết trước khi chúng trở nên nghiêm trọng.

- Tăng tính trách nhiệm của cả team: Sẽ không có 1 PM chỉ ra team scram cần làm gì, khi nào. Thay vào đó, team sẽ tập hợp và quyết định những công việc cả team sẽ hoàn thành trong mỗi sprint. Các thành viên làm việc và hỗ trợ lẫn nhau, tăng sự hợp tác và tạo động lực để mỗi thành viên trong team làm việc độc lập.

- Dễ dàng thích ứng với các thay đổi: Với những sprint ngắn và phản hồi tức thì từ các bên, sẽ dễ dàng hơn để xử lý các thay đổi. Ví dụ, nếu team tìm ra 1 user story mới trong 1 sprint, họ có thể dễ dàng thêm chức năng này và sprint tiếp theo trong backlog refinement meeting (cuộc họp để đưa ra và sàng lọc các yêu cầu tồn đọng chưa giải quyết)

- Tiết kiệm chi phí: Communication thường xuyên và ngay lập tức đảm bảo được team nhận thức tất cả các vấn đề, thay đổi sớm khi phát sinh, giúp giảm chi phí và tăng chất lượng công việc. Increased cost savings: Constant communication ensures the team is aware of all issues and changes as soon as they arise, helping to lower expenses and increase quality. Bằng cách coding và test các nhóm chức năng nhỏ, team sẽ nhận được những phản hồi liên tiếp và những lỗi có thể được sửa sớm hơn, trước khi chúng trở nên rất tốn chi phí để sửa lại.


**Nhược điểm của Scrum**

- Không kiểm soát được scope phát triển: Nhiều dự án Scrum gặp phải vấn đề này do thiếu thống nhất về ngày kết thúc dự án. Vì không có ngày hoàn thành dự án, khách hàng hay các bên có thể sẽ không dừng việc add thêm nhiều chức năng mới.

- Team cần có kinh nghiệm, trình độ cao và commitment cao: Với những role và trách nhiệm cố định, cả team cần phải quen thuộc với những quy định của Scrum. Vì trong team Scrum, không có role chuyên biệt, nên các thành viên trong team đều cần có kinh nghiệm kỹ thuật. Cả team cần cam kết tham gia Scrum meeting hàng ngày, và ở trong team suốt cả dự án.

- Scrum Master không thích hợp có thể làm hỏng mọi thứ: Scrum Master rất khác với PM. Về căn bản Scrum Master phải là người có quyền cao hơn team, người này cần tin tưởng team mà họ quản lý và không bao giờ chỉ đạo team cần làm gì. Nếu Scrum Master cố gắng kiểm soát team, dự án sẽ thất bại.

- Việc define task không rõ ràng sẽ dẫn đến sự thiếu chính xác: Chi phí dự án và timelineness của dự án sẽ không chĩnh xác nếu các task không được xác định rõ ràng. Nếu mục tiêu ban đầu của dự án không rõ ràng, việc plan sẽ trở nên khó khăn và các sprint có thể sẽ tốn nhiều thời gian hơn dự tính ban đầu.


**Các vai trò trong Scrum**

- **Product Owner:** Đây là người biết rõ về việc dự án sẽ tạo ra sản phẩm gì và truyền đạt lại cho team. Product Owner chủ yếu chú trọng và những yêu cầu về nghiệp vụ và thị trường, phân loại công việc cần làm. Người này sẽ xây dựng và quản lý product backlog (những yêu cầu tồn đọng của dự án), hướng dẫn về việc những chức năng nào cần được xử lý tiếp theo, và làm việc với team, cũng nhưng các stakeholder để đảm bảo các bên hiểu được những nội dung trong product backlog. Product Owner không phải là PM. Thay vì quản lý tiến độ và tình trạng dự án, công việc của project owner là tăng động lực của team để hướng đến mục tiêu cuối của dự án.

- **Scrum Master:** thường được cho là người hướng dẫn của team, Scrum Master hỗ trợ team làm tốt nhất công việc của họ. Người này tổ chức các meeting, xử lý các vấn đề, và làm việc với Product Owner để đảm bảo product backlog sẵn sàng cho sprint tiếp theo. Scrum Master cũng cần đảm bảo team đang thực hiện đúng theo quy trình Scrum. Tuy không có quyền hơn các member khác, nhưng người này lại có quyền với quy trình dự án. Ví dụ, Scrum Master không thể bảo mọi người làm gì, nhưng có thể đưa ra những đề xuất về thời gian của 1 sprint.

- **Scrum Team:** Scrum Team thường gồm có 5 hay 7 thành viên. Mọi người làm việc và hỗ trợ lẫn nhau. Không giống như các team truyền thống, không có những vai trò chuyên biệt như programmer, designer hay tester. Mọi người cần hoàn thiện tất cả các công việc cùng lúc. Scrum team đảm nhiệm việc lên kế hoạch cho mỗi sprint, họ quyết định bao nhiêu công việc họ có thể hoàn thành trong mỗi sprint.


![img.png](blog/java/img/agile3.png)

- **Product backlog:** Product Owner và Scrum Team họp để phân loại mức độ ưu tiên cho những nội dung trong product backlog (Những công việc trong product backlog đến từ user stories và các yêu cầu). product backlog không phải là list các công việc cần hoàn thành mà nó giống như 1 list các chức năng mong muốn của 1 sản phẩm. Team phát triển sau đó sẽ đưa ra các công việc dựa trên product backlog để hoàn thành trong mỗi sprint.

- **Sprint planning:** Trước mỗi sprint, Product Owner sẽ đưa ra những nội dung ưu tiên trong project backlog. Team sẽ lựa chọn công việc nào họ có thể hoàn thành trong sprint đó, và đưa những công việc này từ product backlog sang sprint backlog (List những task cần hoàn thiện trong sprint).

- **Backlog refinement/grooming:** Vào giai đoạn của của 1 sprint, team và Product Owner tổ chức họp để đảm bảo nội dung backlog cho sprint tiếp theo. Team có thể bỏ đi những user story không liên quan, tạo story mới, đánh giá độ ưu tiên của các story hoặc chi user story thành những task nhỏ hơn. Mục tiêu của công đoạn "chải chuốt" này là để đảm bảo backlog chỉ bao gồm những nội dung liên quan và chi tiết đúng với mục tiêu của dự án.

- **Daily Scrum meetings:** Họp hàng ngày là cuộc họp ngắn, kéo dài 15 phút và thường là họp đứng. Trong meeting, member sẽ nói về mục tiêu và những vấn đề họ gặp phải. Scrum meeting được tổ chức hàng ngày trong suốt sprint, đảm bảo team đang trong tầm kiểm soát.

- **Sprint review meeting:** Khi kết thúc mỗi sprint, team sẽ trình bày về công việc họ đã hoàn thiện trong cuộc họp này.Cuôc họp này nên có demo, chứ không phải là 1 báo cáo thuyết trình PowerPoint.

- **Sprint retrospective meeting:** Cũng ở giai đoạn kết thúc mỗi sprint, team sẽ phản ánh những điểm tốt khi thực hiện scrum và nói về những điểm cần thay đổi cho sprint tiếp theo. Team cũng có thể nói về những điểm tốt, những điểm chưa tốt, cũng những những điều họ có thể làm khác đi trong suốt mỗi sprint.


### **Tools, Artifacts, and Methods in Scrum**


- **Scrum board:** You can visualize your sprint backlog with a Scrum task board. The board can have different forms; it traditionally involves index cards, Post-It notes, or a whiteboard. The Scrum board is usually divided into three categories: to do, work in progress, and done. The Scrum Team needs to update the board throughout the entire sprint. For example, if someone comes up with a new task, she would write a new card and put it in the appropriate column.

- **User stories:** A user story describes a software feature from the customer’s perspective. It includes the type of user, what they want, and why they want it. These short stories follow a similar structure: as a <type of user>, I want to so that I can <achieve some goal.> The development team uses these stories to create code that will meet the requirements of the stories.

- **Burndown chart:** A burndown chart represents all outstanding work. The backlog is usually on the vertical axis, with time along the horizontal axis. The work remaining can be represented by story points, ideal days, team days, or other metrics. A burndown chart can warn the team if things aren’t going according to plan and helps to show the impact of decisions.

- **Large-Scale Scrum (LeSS):** If you want to scale elements of Scrum to hundreds of developers, the Large-Scale Scrum (LeSS) framework helps extend the rules and guidelines without losing the core of Scrum. The principles are taken directly from Scrum, however focuses on scaling up without adding additional overhead (like adding more roles, artifacts, or processes).

- **Timeboxing:** A timebox is a set period of time during which a team works towards completing a goal. Instead of letting a team work until the goal is reached, the timebox approach stops work when the time limit is reached. Time-boxed iterations are often used in Scrum and Extreme Programming.

- **Icebox:** Any user stories that are recorded but not moved to development are stored in the icebox. The term “icebox” was created by Pivotal Tracker, an Agile project management tool.

- **Scrum vs RUP:** While both Scrum and Rational Unified Process (RUP) follow the Agile framework, RUP involves more formal definition of scope, major milestones, and specific dates (Scrum uses a project backlog instead of scope). In addition, RUP involves four major phases of the project lifecycle (inception, elaboration, construction, and transition), whereas Scrum dictates that the whole “traditional lifecycle” fits into one iteration.

- **Lean vs Scrum:** Scrum is a software development framework, while Lean helps optimize that process. Scrum’s primary goal is on the people, while Lean focuses on the process. They are both considered Agile techniques, however Lean introduces two major concepts: eliminating waste and improving flow.




=> bài viết đi copy từ nguồn: https://viblo.asia/p/waterfall-vs-agile-vs-scrum-part-3-scrum-la-gi-m68Z0wndKkG


## Tóm lại là: mô hình phát triển rất quan trọng vì chúng ta dựa vào đó để làm việc. Outsourcing hay product thì đều có nơi áp dụng cái agile/ waterfall. Ví dụ như Fsoft thì gần như đã chuyển qua agile, agile của fsoft khá tối ưu và gọn nhẹ (tiệm cận nhất) để tiết kiệm chi phí. Nhưng ko phải tất cả các dự án đều có mô hình agile gọn nhẹ. Có những dự án thì lai agile và waterfall
## Điểm khác biệt rõ ràng là: 
## - waterfall thì cần CA, SRS final để thực hiện làm và làm đúng ko có sự sáng tạo sai thì về sau khi testing phát hiện sẽ sửa. Sản phẩm phải hoàn thành hết mới golive chứ ko thể golive từng phần
## - agile thì chỉ cần ý tưởng, mô tả đầu vào đầu ra rồi dev có thể tự vẽ vời ra cho phù hợp, đẩy sức sáng tạo lên tối đa. Và hơn nữa nếu trong quá trình develop phát hiện ra lỗi hay cải thiện thì hoàn toàn có thể sửa đổi để golive từng phần. => Agile sẽ cần dev cứng và có kinh nghiệm hơn!

