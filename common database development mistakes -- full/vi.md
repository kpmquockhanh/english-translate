## Đâu là những lỗi phổ biến hay mắc phải của những nhà phát triển ứng dụng trong quá trình phát triển cơ sở dữ liệu.

_source https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ mục thích hợp**

Đây là 1 vấn đề tương đối đơn giản những vẫn xảy ra hàng ngày. Các khóa ngoại nên có các chỉ mục. Nếu bạn đang sử dụng 1 trường trong mệnh đề WHERE bạn nên (có lẽ) có chỉ mục trên nó. Các chỉ mục thường nên gồm nhiều cột dựa trên câu truy vấn bạn thực hiện.

**2. Không sử dụng các ràng buộc tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi nhưng nếu nó hỗ trợ ràng buộc tham chiếu-- tức là tất cả các khóa ngoại được đảm bảo cho 1 thực thể tổn tại -- bạn nên sử dụng nó.

Rất hay thấy lỗi như thế này trên cơ sở dữ liệu MySQL. Tôi không nghĩ rằng MyISAM hỗ trợ nó. Nhưng InnoDB thì có. Bạn sẽ thấy những người sử dụng MyISAM hay những cơ sở dữ liệu khác hỗ trợ InnoDB nhưng không sử dụng nó

Thêm nữa:

- [Các ràng buộc như NOT NULL hay Khóa Ngoại quan trọng như thế nào khi ta luôn luôn kiểm soát đầu vào cơ sở dữ liệu với php](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Khóa ngoại có thực sự cần thiết trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Khóa ngoại có thực sự cần thiết trong thiết kế cơ sở dữ liệu?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng khóa tự nhiên thay vì (kỹ thuật) khóa chính đại diện*

Khóa tự nhiên là các khóa dựa trên các dữ liệu có nghĩa bên ngoài mà (có vẻ) độc nhất. 1 vài ví dụ phổ biến là các code sản xuất, mã bang 2-chữ-cái (US), số bảo mật xã hội và vân vân. Khóa đại diện hay kĩ thuật khóa chính tuyệt đối không có ý nghĩa bên ngoài hệ thống. Nó được phát minh hoàn toàn là để định nghĩa các thực thể và là các trường tự tăng  (SQL Server. MySQL, khác nưã) hoặc chuỗi(đặc biệt nhất là (Oracle) 

Theo ý kiến của tôi, bạn nên **luôn luôn** sử dụng khóa đại diện. Vấn đề này tới từ những câu hỏi sau đây

- [Bạn thích khóa chính của bạn như thế nào?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [Cách tốt nhất để thực hành khóa chính trên các bảng?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Nên dùng khóa chính loại nào trong trường hợp này.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Khóa đại diện và khóa thường/kinh tế](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Chúng ta có nên có 1 trường khóa chính?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây phần nào là 1 chủ đề gây tranh cãi khi không được chấp nhận rộng rãi. Trong khi bạn cố gắng tìm ra 1 người nghĩ rằng khóa thường trong 1 vài trường hợp thì ok, bạn sẽ không thấy bất kỳ sự chỉ trích nào dành cho khóa đại diện cũng như cho rằng nó không cần thiết, Đây là 1 nhược điểm nhỏ nếu bạn hỏi tôi


Nhớ rằng, thậm chí [ các quốc gia không còn tồn tại][countries can cease to exist](http://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ, Nam Tư)
**4. Viết các câu truy vấn được yêu cầu Để DISCTINCE hoạt động**

Bạn sẽ thường thấy nó trong các câu truy vấn khởi tạo ORM. Nhìn log đầu ra trong Hibernate và bạn sẽ thấy tất cả các câu truy vấn bắt đầu với:

SELECT DISTINCT ...

Sẽ mất 1 lục để đảm bảo bạn không tạo ra các dòng bị trùng và dẫn đến các đối tượng bị trùng. Bạn sẽ thỉnh thoảng thấy vài người làm như vậy. Nếu bạn thấy điều này quá nhiều thì thực sự đáng báo động đấy.Không phải do DISTINCE tệ háy không có các ứng dụng hợp lệ. Nó tốt (cả 2 mặt) nhưng nó không phải đại diện hoặc tạm thời để viết các câu truy vấn đúng.

Từ [Tại sao tôi ghét DISCTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Where things start to go sour in my opinion is when a developer is building substantial query, joining tables together, and all of a sudden he realizes that it **looks** like he is getting duplicate (or even more) rows and his immediate response...his "solution" to this "problem" is to throw on the DISTINCT keyword and **POOF** all his troubles go away.
> Mọi thứ bắt đầu dến trong tâm trí tôi khi 1 nhà phát triển bắt đầu viết câu truy vấn đáng kể, nối các bảng lại với nhau, và bỗng nhiên anh ý nhận ra nó **trông** như kiểu ông ý đang lấy các dòng trùng nhau(thậm chí nhiều hơn) và câu trả lời ngay lập tức của anh ý ..... "cách giải quyết" của anh ý cho "vấn đề" này là ném từ khóa DISTINCE và **ném** tất cả các rắc rối đi.

**5. Thích các phép gộp hơn và phép nối**
Một trong những lỗi phổ biến của các nhà phát triển ứng dụng cơ sở dữ liệu là không nhận ra phép hợp(ví dụ như mệnh đề GROUP BY) tốn kém hơn thế nào so với các phép nối.

Để cho bạn 1 ý tưởng về điều này phổ biến rộng rãi như thế nào, tôi đã viết về chủ đề này vài lần ở đây và đã bị vote down rất nhiều. Ví dụ:

Từ [câu lệnh SQL  - “join” với “group by và having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> câu truy vấn đầu tiên:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> thời gian truy vấn: 0.312 s

> câu truy vấn thứ 2:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> thơi gian truy vấn: 0.016 s

> Đúng vậy. Phiên bản nối tôi đề xuất **nhanh gấp 2 lần phiên bản nhóm.**

**6. Không đơn giản hóa các câu truy vấn phức tạp qua view**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ viewiew nhưng chúng đều cóó thể đơn giản hóa các câu truy vấn nếu sử dụng 1 cách khôn ngoan. Ví dụ, trong 1 dự án tôi sử dụng [generic Party model](http://www.tdan.com/view-articles/5014/) cho CRM. Đấy là 1 kỹ thuật mô hình rất mạnh và linh hoạt có thể sử dụng nhiều phép nối. Trong mô hình này có :

- **Party**: con người và các tổ chức;
- **Party Role**: các việc mà các nhóm làm , như nhân viên và nhà tuyển dụng;
- **Party Role Relationship**: các luật liên quan thế nào với các luật khác.

Ví dụ:

- Ted là 1 Person, là 1 tập con Party;
- Ted có nhiều luật, 1 trong số chúng là Employee;
- Intel là 1 tổ chức, là 1 tập con cửa Party;
- Intel có nhiều luật, 1 trong số chúng là Employer;
- Intel tuyển Ted, nghĩa là có quan hệ giữa các luật tương ứng của chúng.

Do vậy, có 5 bảng nối để kếtết nối Ted với các nhà tuyển dụng của anh ý.Giả định rằng các nhân viên là Persons(không phải các tổ chức) và cung cấp các view hỗ trợ
CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và bỗng nhiên bạn có 1 view rất đơn giản các dữ liệu bạn muốn những ở mô hình dữ liệu linh hoạt cao.

**7. Không lọc đầu vào**

Đây là 1 chuyện lớn đấy. Bây giờ tôi thích PHP nhưng nếu bạn không muốn biết bạn đang làm gì nó thực sự dễ dàng trong việc tạo các site có thể bị tấn công. Không cái nào tổng kết điều này tốt hơn là [câu chuyện về BOobby Tables bé nhỏ](http://xkcd.com/327/).

Dữ liệu được cung cấp bởi người dùng qua URLs, fỏm data **và cookies** nên luôn luôn được coi là không tốt và cần được lọc. Hãy đảm bảo rằng bạn đang lấy được những gì bạn mong muốn

**8. Không sử dụng các lệnh được chuẩn bị**

Các câu lệnh chuẩn bị là khi bạn biên dịch 1 truy vấn khuyết dữ liệu sử dụng trong các lệnh thêm, cập nhật và 

mệnh đề WHERE  và sẽ cung cấp dữ liệu cho nó sau. Ví dụ:

SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

or

SELECT * FROM users WHERE username = :username

dựa trên nền tảng của bạn.

Tôi đã từng thấy các cơ sở dữ liệu bị phá hủy vì điều này. Thông thường, mỗi lần bất kỳ 1 cớ sở dữ liệu hiện đại nào gặp 1 câu truy vấn mới, nó phải biên dịch nó. Nếu nó gặp 1 câu truy vấn đã nhìn thấy trước đó, thì cơ sở dữ liệu có cơ hội cache câu trúy vấn đã biên dịch và thực hiện nó. Bằng cách thực hiện câu truy vấn nhiều lần bạn đang chó phép cơ sở dữ liệu để tìm kiếm và tối ưu thích hợp ( ví dụ, bằng cách ghim các câu truy vấn đã biên dịch trong bộ nhớ)

Sử dụng các câu lệnh được chuẩn bị cũng sẽ cho bạn thống kê ý nghĩa về việc các câu truy vấn cụ thể được sử dụng với tần suất nào.

Các câu lệnh được chuẩn bị cũng bảo vệ bạn tốt hơn với cách tấn công SQL injection
