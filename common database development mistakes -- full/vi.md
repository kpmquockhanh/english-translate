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

**9. Không đủ độ chuẩn hóa**  

[Chuẩn hóa cơ sở dữ liệu](https://en.wikipedia.org/wiki/Database_normalization) là một tiến trình cơ bản của việc tối ưu hóa thiết kế cơ sở dữ liệu hoặc cách bạn tổ chức tổ chức dữ liệu của bạn trong các bảng. 
Chỉ trong tuần này, tôi đã bắt gặp một vài mã nguồn mà họ đã tách mảng ra và chèn nó vào một trường đơn trong cơ sở dữ liệu. Quá trình chuẩn hóa sẽ coi là thành phần của mảng như là một dòng riêng biệt trong bẳng con. (như trong mối quan hệ một - nhiều).  
Nó cũng được đề cập đến trong [Phương thức tốt nhất cho việc lưu trữ danh sách các ID người dùng](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids)  
> Tôi đã thấy trong những hệ thông khác các danh sách được lưu trữ trong mảng tuần tự PHP  

Nhưng sự thiếu sót của quá trình chuẩn hóa cũng đến từ rất nhiều hình thức.  
Xem thêm:  
* [Chuẩn hóa: Bao xa là đủ xa?](https://www.techrepublic.com/article/normalization-how-far-is-far-enough/)  
* [SQL qua design: Tại sao bạn cần chuẩn hóa cơ sở dữ liệu](http://www.itprotoday.com/microsoft-sql-server/sql-design-why-you-need-database-normalization)  

**10. Chuẩn hóa quá nhiều**  
Có thể thấy rằng nó mâu thuẫn với điểm trước đó nhưng chuẩn hóa, cũng như nhiều thứ khác, cũng là một công cụ. Nó là một cách để kết thúc vào không phải là kết thúc trong chính nó. Tôi nghĩ rằng rất nhiều nhà phát triển đã quên điều đó và bắt đầu coi "ý nghĩa" như là một "thành tựu đạt được". Đơn vị kiểm thử là một ví dụ điển hình cho điều đó.  
Tôi đã làm việc trên một hệ thống mà có hệ thống phân cấp khổng lồ cho khách hàng mà tôi đã làm như thế này:  
Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...  
Như thể là bạn join khoảng 11 bảng khác nhau trước khi bạn có thể lấy bất cứ dữ liệu có ý nghĩa nào. Nó là một ví dụ tốt cho việc chuẩn hóa đi quá xa.  
Một vài điểm khác, việc chuẩn hóa một cách cẩn thận và được lưu tâm có thể mang lại hiệu suất lớn, nhưng bạn phải thật sự cẩn thận khi làm điều đó.  
Xem thêm:  
* [Tại sao chuẩn hóa cơ sở dữ liệu quá nhiều có thể là điều tồi tệ?](https://www.selikoff.net/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)  
* [Bao nhiêu là đủ cho việc chuẩn hóa trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)  
* [Không chuẩn hóa cơ sở dữ liệu SQL của bạn khi nào?](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)  
* [Có thể chuẩn hóa không phải là bình thường](http://www.codinghorror.com/blog/archives/001152.html)  
* [Nguyên nhân các cuộc tranh luận chuẩn hóa cơ sở dữ liệu trên Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)  

**11. Sử dụng tham số duy nhất**  
Tham số duy nhất là lỗi phổ biến trong các bảng được tạo bởi hai hay nhiều khóa ngoại nơi 1 và chỉ 1 trong chúng có thể không null. **Một sai lầm lớn**. Một điều là nó trở lên khó khăn nhiều hơn để duy trì toàn vẹn dữ liệu. Sau tất cả, cũng với sự toàn vẹn các ràng buộc, không điều gì là ngăn cản hai hay nhiều khóa ngoại này được đặt (mặc dù kiểm tra ràng buộc phức tạp)  
Theo [Hướng dẫn thực tiễn cho việc thiết kế quan hệ cơ sở dữ liệu](https://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&ct=result)  
> Chúng tôi đã nhấn mạnh chống lại việc xây dựng tham số duy nhất bất cứ khi nào có thể, vì lý do tốt mà họ có thể khó viết mã và gây ra nhiều khó khăn về bảo trì.  

**12. Không phân tích hiệu suất về các truy vấn trên toàn bộ**  
Theo chủ nghĩa thực dụng tối cao, đặc biệt là trong thế giới cơ sở dữ liệu. Nếu bạn vẫn đang bị bám theo nguyên tắc rằng chúng đã trở nên độc đoán thì bạn thực sự đang mắc sai lầm. Lấy 1 ví dụ về các truy vấn gộp bên trên. Phiên bản gộp có vẻ nhìn "ổn" nhưng hiệu suất của nó thì tệ. Việc so sánh về hiệu suất nên kết thúc cuộc tranh luận (nhưng nó không) nhưng thêm 1 điều rằng : việc đưa quá nhiều view thông báo xấu ngay trong vị trí đầu tiên là ngu ngốc, thậm chí là nguy hiểm.  
**13. Quá phụ thuộc vào UNION ALL và đặc biệt là cấu trúc UNION**  
UNION trong SQL chỉ đơn thuần nối các tập dữ liệu đồng nhất, có nghĩa là chúng có cùng kiểu và số cột. Sự khác biệt giữa chúng là UNION ALL là một sự ghép nối đơn giản và nên được ưu tiên hơn bất cứ khi nào có thể trong khi một UNION ngầm sẽ làm một DISTINCT để loại bỏ các bản sao trùng lặp.  
UNIONs, cũng như DISTINCT, có vai trò của riêng chúng. Đều có các ứng dụng hợp lệ. Nhưng nếu bạn tự tìm hiểu chúng nhiều 1 chút, đặc biệt là trong các truy vấn con, thì chúng thực sự đang làm sai.  Đó có thể là  trường hợp của cấu trúc truy vấn tệ hoặc là 1 mô hình dữ liệu thiết kế kém khiến bạn phải làm những điều này.  
UNIONs, đặc biệt là khi sử dụng trong phép nối hoặc các truy vấn con phụ thuộc, có thể làm hỏng cơ sở dữ liệu. Cố gắng sử dụng chúng nếu có thể.  
**14. Sử dụng điều kiện OR trong truy vấn**  
Điều này có vẻ vô hại. Sau tất cả, AND là ổn.Phép OR cũng ổn phải không? Sai rồi. Về cơ bản, một điều kiện AND hạn chế tập dữ liệu trong khi điều kiện OR phát triển nó nhưng không phải là theo một cách mà nó có thể tự tối ưu hoá. Đặc biệt khi các điều kiện OR khác nhau có thể giao nhau, do đó trình tối ưu hóa có hiệu quả để một hoạt động DISTINCT về kết quả.  
Bad:  
... WHERE a = 2 OR a = 5 OR a = 11  
Better:  
... WHERE a IN (2, 5, 11)  
Hiện tại quá trình tối ưu hóa SQL của bạn có thể có hiệu quả từ truy vấn đầu tiên đến tiếp theo. Nhưng nó có thể không. Chỉ cần không làm điều đó.  
**15. Không thiết kế mô hình dữ liệu để có giải pháp hiệu suất cao**  
Đây là một điểm khó để định lượng. Nó thường được xem xét bởi hiệu ứng của nó.  Nếu bạn thấy mình đang viết các câu truy vấn cho các việc tương đối đơn giản hay các truy vấn để tìm các thông tin tương đối đơn giản nhưng không hiệu quả, thì bạn chắc chắn đang có mô hình dữ liệu tồi.  
Bằng 1 cách nào đó, điều này tổng kết tất cả các điều trước đó nhưng nó có thêm 1 cảnh bảo là những việc như tối ưu truy vấn thường xong đầu tiên trong khi điều này nên được hoàn thành thứ 2. Đầu tiên và cũng là quan trọng nhất là bạn nên chắc rằng bạn có 1 mô hình dữ liệu tốt trước khi cố gắng tối ưu hiệu suất. Và Knuth nói rằng:  
> Tối ưu sớm là gốc rễ của mọi điều xấu.  

**16. Sử dụng Database Transactions không chính xác**  
Tất cả các dữ liệu thay đổi cho tiến trình riêng phải là rất nhỏ. Ví dụ nếu như hoạt động thành công, nó cũng đầy đủ. Nếu như lỗi, dữ liệu sẽ không thay đổi. Không nên có những thay đổi nửa vời.  
Lý tưởng thì cách tốt nhất để đạt được điều này là thiết kế toàn bộ hệ thống nên cố gắng hỗ trợ toàn bộ thay đổi dữ liệu qua từng câu lệnh INSERT/UPDATE/DELETE đơn. Trong trường hợp này, không có xử lý transaction đặc biệt nào cần thiết cả, vì động cơ cơ sở dữ liệu  của bạn nên làm điều này tự động.  
Tuy nhiên, nếu bất kỳ tiến trình nào yêu cầ nhiều câu lệnh được thực hiện như là đơn vị để giữ dữ liệu ở trạng thái thích hợp, khi đó, điều khiển Transaction thích hợp là cần thiết.  
* Bắt đầu Database Transactions trước câu lệnh đầu tiên  
* Cam kết Transaction sau câu lệnh cuối cùng  
* Khi có bất kỳ lỗi nào, tiến hành Rollback Transaction. Và rất quan trọng! Đừng quên bỏ qua/ dừng tất cả các câu lệnh sau các lỗi.  
Cũng nên chú ý cẩn thận đến các subtelties của lớp kết nối cơ sở dữ liệu của bạn, và động cơ cơ sở dữ liệu tương tác trong vấn đề này  
**17. Không hiểu mô hình 'dựa trên cơ sở'**  
Ngôn ngữ SQL tuân theo mô hình cụ thể phù hợp với kiểu cụ thể của vấn đề. Mặc dù có nhiều phần mở rộng cung cấp cụ thể, cuộc đấu tranh ngôn ngữ để giải quyết vấn đề này là không đáng kể trong ngôn ngữ như Java, C#, Delphi, v.v..  
Phần còn lại được biểu hiện trong 1 vài cách:  
* Áp đặt quá nhiều các phương pháp và logic bắt buộc không phù hợp trên cơ sở dữ liệu  
* Sử dụng con trỏ không phù hợp hoặc 1 cách quá đáng. Đặc biệt khi câu truy vấn đơn đầy đủ.  
* Giả định sai rằng trigger thực hiện ngay khi mỗi dòng bị ảnh hưởng trong cập nhật đa-dòng  
Xác định phân chia trách nhiệm rõ ràng và cố gắng sử dụng công cụ thích hợp để giải quyết từng vấn đề.
