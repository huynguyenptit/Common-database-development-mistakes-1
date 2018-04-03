## Những sai lầm phổ biến của các nhà phát triển ứng dụng trong phát triển cơ sở dữ liệu?

_source https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ số thích hợp**

Đây là một điều khá đơn giản nhưng vẫn xảy ra bất kì lúc nào. Khóa ngoài nên có các indexes. Nếu bạn đang sử dụng một trường trong một lệnh

WHERE bạn nên (có thể) có một chỉ mục trên đó. Các chỉ mục như vậy thường bao gồm nhiều cột dựa trên các truy vấn bạn cần thực hiện.

**2. Không thực thi tất cả tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi nhưng nếu cơ sở dữ liệu của bạn hỗ trợ tất cả tham chiếu--có nghĩa là tất cả các khóa ngoài được đảm bảo trỏ đến một thực thế tồn tại--bạn nên sử dụng nó.

Rất phổ biến để thấy sự thất bại này trên cơ sở dữ liệu MySQL. Tôi không tin rằng MyISAM hỗ trợ nó. InnoDB làm được. Bạn sẽ tìm thấy những người đang sử dụng MyISAM hoặc những người đang sử dụng InnoDB nhưng dù thế nào đi nữa cũng không sử dụng nó.

Thêm nữa:

- [Những ràng buộc quan trọng như NOT NULL và FOREIGN KEY nếu tôi sẽ luôn luôn kiểm soát đầu vào cơ sở dữ liệu của tôi với php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Khóa ngoài thực sự cần thiết trong một thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Khóa ngoài thực sự cần thiết trong một thiết kế cơ sở dữ liệu?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng các khóa chính tự nhiên chứ không phải là để đại diện (kỹ thuật)**

Các khóa tự nhiên là các khoá dựa trên dữ liệu bên ngoài có ý nghĩa đó (có vẻ là) duy nhất.Các ví dụ phổ biến là mã sản phẩm, mã bang 2-chữ-cái (US), **social security number** và vv.... Các khóa chính thay thế hoặc kỹ thuật chính là những khoá hoàn toàn không có ý nghĩa bên ngoài hệ thống. Chúng được sinh ra hoàn toàn để xác định thực thể vàthường là các trường tự động gia tăng (SQL Server, MySQL, khác) hoặc các trình tự (nhất là Oracle).

Theo tôi, bạn nên **luôn luôn** sử dụng các khóa đại diện. Vấn đề này đã đưa ra trong những câu hỏi sau:

- [Bạn thích khóa chính của mình như thế nào?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [Cách luyện tập tốt nhất cho các khóa chính trong các bảng?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Bạn sử dụng định dạng khóa chính nào trong trường hợp này.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Khóa đại diên vs khóa tự nhiên/vai trò](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Tôi có nên dành riêng trường khóa chính?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là một chủ đề gây nhiều tranh cãi mà bạn sẽ không nhận được ý kiến chung.Trong khi bạn có thể tìm thấy một số người, những người nghĩ rằng các khóa tự nhiên là trong một số tình huống OK, bạn sẽ không tìm thấy bất kỳ lời chỉ trích nào khóa đại diện khác không cần thiết. Đó là một nhược điểm nhỏ nếu bạn hỏi tôi.

Nhớ lại, ngay đến [các quốc gia có thể không còn tồn tại](http://en.wikipedia.org/wiki/ISO_3166-1) (Ví dụ từ Yugoslavia).

**4. Viết các truy vấn yêu cầu

DISTINCT hoạt động**

Bạn thường thấy điều này trong các truy vấn ORM-generated. Nhìn vào đầu ra log từ Hibernate và bạn sẽ thấy tất cả các truy vấn bắt đầu với:

SELECT DISTINCT ...

Đây là một chút của một shortcut để đảm bảo bạn không trả lại các hàng trùng lặp và do đó nhận được các đối tượng trùng lặp. Đôi khi bạn cũng thấy mọi người đang làm việc này. Nếu bạn nhìn thấy nó quá nhiều đó là một flag red thực sự. Không phải cái đó

DISTINCT là xấu hoặc không có ứng dụng hợp lệ. Nó tốt (cả 2 mặt) nhưng nó không phải đại diện hoặc tạm thời để viết các câu truy vấn đúng.

Từ [Tại sao tôi gét DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Trường hợp mọi thứ bắt đầu trở nên tôi tệ theo ý kiến ​​của tôi là khi một nhà phát triển đang xây dựng truy vấn quan hệ, ghép bảng lại với nhau, và đột nhiên anh ta nhận ra nó **trông** như ông đang bị trùng lặp (hoặc thậm chí nhiều hơn) các hàng và phản ứng tức thời của ông..."giải pháp" của ông đối với "vấn đề" này là ném vào từ khóa DISTINCT và **POOF** tất cả những rắc rối của ông đi.

**5. Khuyến khích tập hợp các ghép nối**

Một sai lầm phổ biến khác của nhà phát triển ứng dụng cơ sở dữ liệu  là không nhận ra phép hợp(ví dụ như mệnh đề GROUP BY) tốn kém hơn thế nào so với các phép nối.

Để cung cấp cho bạn một ý tưởng về sự phổ biến rộng rãi này là, Tôi đã viết về chủ đề này nhiều lần ở đây và được downvoted rất nhiều cho nó. Ví dụ:

Từ [SQL statement - “join” vs “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> Truy vấn đầu tiên:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Thời gian truy vấn: 0.312 s

> Truy vấn thứ 2:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Thời gian truy vấn: 0.016 s

> Chuẩn luôn. Ghép nối Tôi đề xuất ** nhanh gấp 2 lần nhóm.**

**6. Không đơn giản hóa các truy vấn phức tạp thông qua các view**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu đều hỗ trợ views nhưng đối với những người làm, họ có thể rất đơn giản hóa các truy vấn nếu được sử dụng một cách thận trọng. Ví dụ,trên một dự án tôi sử dụng một [generic Party model](http://www.tdan.com/view-articles/5014/) cho CRM. Đây là một kỹ thuật mô hình rất mạnh mẽ và linh hoạt nhưng có thể dẫn đến nhiều ghép nôi. Trong mô hình này đã có:

- **Party**: người và tổ chức;
- **Party Role**: những điều mà các bên đã làm, ví dụ như Employee and Employer;
- **Party Role Relationship**: làm thế nào những vai trò liên quan đến nhau.

Ví dụ:

- Ted là 1 Person, là 1 tập con Party;
- Ted có nhiều luật, 1 trong số chúng là Employee;
- Intel là 1 tổ chức, là 1 tập con cửa Party;
- Intel có nhiều luật, 1 trong số chúng là Employer;
- Intel tuyển Ted, nghĩa là có quan hệ giữa các luật tương ứng của chúng.

Vì vậy, có năm table tham gia để liên kết Ted **to his employer**.Bạn giả định tất cả nhân viên là Người (không phải tổ chức) và cung cấp các helper view:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và đột nhiên bạn có một cái nhìn rất đơn giản về dữ liệu mà bạn muốn nhưng trên một mô hình dữ liệu có tính linh hoạt cao.

**7.  Không cải thiện đầu vào**

Đây là một cái rất lớn. Bây giờ tôi thích PHP nhưng nếu bạn không biết những gì bạn đang làm rất dễ dàng để tạo các trang web dễ bị tấn công. Không có gì kết luận nó tốt hơn so với [story of little Bobby Tables](http://xkcd.com/327/).

Dữ liệu được cung cấp bởi người dùng bằng các URL, dữ liệu biểu mẫu **và cookie** phải luôn được coi là không tốt và cần được lọc. Đảm bảo bạn đang nhận được những gì bạn mong đợi.

**8.Không sử dụng các câu lệnh được chuẩn bị**

Báo cáo chuẩn bị là khi bạn biên dịch một truy vấn trừ dữ liệu được sử dụng trong chèn, cập nhật và 

WHERE clauses và sẽ cung cấp dữ liệu cho nó sau. Ví dụ:

SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

or

SELECT * FROM users WHERE username = :username

tùy thuộc vào nền tảng của bạn.

Tôi đã từng thấy các cơ sở dữ liệu bị phá hủy vì điều này. Về cơ bản,mỗi lần bất kỳ cơ sở dữ liệu hiện đại gặp một quer mới nó phải biên dịch. Nếu nó gặp một truy vấn nó được nhìn thấy trước, bạn đang cho cơ sở dữ liệu cơ hội để cache truy vấn biên dịch và kế hoạch thực hiện. Bằng cách làm các truy vấn rất nhiều bạn đang cho cơ sở dữ liệu cơ hội tìm kiếm và tối ưu thích hợp (ví dụ, bằng cách ghim truy vấn đã biên dịch trong bộ nhớ).

Sử dụng các câu lệnh chuẩn bị cũng sẽ cung cấp cho bạn số liệu thống kê có ý nghĩa Sử dụng các câu lệnh chuẩn bị cũng sẽ cung cấp cho bạn số liệu thống kê có ý nghĩa.

Các câu lệnh được chuẩn bị cũng sẽ giúp bạn chống lại các cuộc tấn công SQL injection tốt hơn.

**9. Chuẩn hóa thiếu**

[Chuẩn hóa cơ sở dữ liệu](http://en.wikipedia.org/wiki/Database_normalization) về cơ bản là quá trình tối ưu hóa thiết kế cơ sở dữ liệu hoặc làm thế nào bạn tổ chức dữ liệu của bạn vào các bảng.

Chỉ trong tuần này tôi đã lướt qua một số code, nơi mà ai đó đã tách một mảng và chèn nó vào một trường duy nhất trong cơ sở dữ liệu. Chuẩn hóa điều đó sẽ xử lý phần tử của mảng đó như một hàng riêng biệt trong bảng con (mối quan hệ một - nhiều).

Điều này cũng được đưa ra trong [Phương pháp tốt nhất để lưu trữ danh sách ID người dùng:](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi đã nhìn thấy trong các hệ thống khác mà danh sách được lưu trữ trong một mảng PHP tuần tự.

Tuy nhiên, sự thiếu chuẩn hóa lại có nhiều hình thức.

Tìm hiểu thêm:

- [Chuẩn hóa: Bao xa thì đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [Thiết kế SQL: Tại sao bạn cần chuẩn hóa cơ sở dữ liệu ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hóa quá nhiều**

Điều này có vẻ như là mâu thuẫn với điều bên trên nhưng chuẩn hóa, giống như nhiều thứ, nó là một công cụ . **It is a means to an end and not an end in and of itself**. Tôi nghĩ rằng nhiều nhà phát triển quên điều này và bắt đầu nghiên cứu **"means" as an "end"**. Đơn vị kiểm nghiệm là một ví dụ điển hình của việc này.

Tôi đã từng làm việc trên một hệ thống mà đã phân cấp rõ ràng cho khách hàng và đã gặp một cái gì đó như:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

như vậy bạn đã phải join khoảng 11 bảng với nhau trước khi bạn có thể nhận được bất kỳ dữ liệu có ý nghĩa. Đó là một ví dụ điển hình về quá trình chuẩn hóa quá nhiều.

Thêm đó, chú ý và xem xét không chuẩn hóa có thể có những lợi ích rất lớn nhưng bạn phải cẩn thận khi làm việc này.

Thêm nữa:

- [Vì sao chuẩn hóa quá nhiều lại là điều tồi tệ](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Làm thế nào để có thể chuẩn hóa trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [Điều gì khi không chuẩn hóa cơ sở dữ liệu SQL của bạn ](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Có thể Chuẩn hoá không phải bình thường](http://www.codinghorror.com/blog/archives/001152.html)
- [Nguồn gốc của tất cả các cuộc thảo luận chuẩn hóa cơ sở dữ liệu về mã hoá Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Sử exclusive arcs**

Một exclusive arc là một sai lầm phổ biến mà một bảng được tạo ra với hai hoặc nhiều khóa ngoại, trong đó một và chỉ một trong số chúng có thể không có giá trị.  **Sai lầm lớn.** đây là điều mà nó trở nên khó khăn hơn nhiều để duy trì tính toàn vẹn dữ liệu. Sau tất cả, ngay cả khi với các ràng buộc tham chiếu, không có gì ngăn ngừa hai hoặc nhiều hơn các khóa ngoại này được thiết lập (mặc dù kiểm tra ràng buộc phức tạp).

Nguồn [Hướng dẫn Thực tiễn về Thiết kế Cơ sở dữ liệu Quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> Chúng tôi đã chỉ ra rõ ràng chống lại việc xây dựng exclusive arc bất cứ lúc nào, vì lý do tốt đó mà họ có thể được lúng túng để viết code và gây ra những khó khăn về bảo trì.

**12. Không phân tích hiệu suất về các truy vấn mọi lúc**

Chủ nghĩa thực dụng thống trị tối cao, đặc biệt trong cơ sở dữ liệu rộng lớn. Nếu bạn đang tuân thủ các nguyên tắc đến mức họ đã trở thành một thói quen sau đó bạn có thể đã sai lầm. Lấy ví dụ của các truy vấn gộp từ phía trên. Phiên bản gộp có thể trông "tuyệt" nhưng hiệu suất của nó là không tốt. Một so sánh hiệu suất đã chấm dứt cuộc tranh luận (nhưng nó không) nhưng thêm một điểm là: việc đưa quá nhiều view thông báo xấu ngay trong vị trí đầu tiên là ngu ngốc, thậm chí là nguy hiểm.

**13. Quá phụ thuộc vào UNION ALL và đặc biệt cấu trúc UNION**

Một UNION trong câu lệnh SQL đơn giản chỉ ghép các tập dữ liệu đồng nhất, có nghĩa là chúng có cùng kiểu và số cột. Sự khác biệt giữa chúng là UNION ALL là một kết nối đơn giản và nên được ưa thích hơn bất cứ nơi nào có thể trong khi UNION sẽ ngầm làm một DISTINCT để loại bỏ các bản sao trùng lặp.

UNIONs, như DISTINCT, có vai trò của riêng chúng. Có các ứng dụng hợp lệ. Nhưng nếu bạn thấy mình đang làm rất nhiều trong số họ, đặc biệt là trong các truy vấn phụ, sau đó có thể bạn thấy đang làm gì sai. Đó có thể là bạn đang xây dựng truy vấn kém hoặc mô hình dữ liệu được thiết kế kém buộc bạn phải làm những việc như vậy.

UNION, đặc biệt khi được sử dụng trong joins hoặc các truy vấn phụ phụ thuộc, có thể làm tê liệt cơ sở dữ liệu. Cố gắng tránh chúng bất cứ khi nào có thể.

**14. Sử dụng điều kiện OR trong truy vấn**

Điều này có vẻ vô hại. Sau cùng, ANDs là ổn. OR nên cũng ổn chứ? Sai bét rồi. Về cơ bản một điều kiện AND hạn chế tập dữ liệu, trong khi một điều kiện OR tăng lên nó nhưng không phải trong một cách mà đáng để tối ưu hoá. Đặc biệt khi các điều kiện  OR khác nhau có thể giao nhau khiến chó việc tối ưu hóa hiệu quả với các phép DISTINCT trong kết quả

Bad:

... WHERE a = 2 OR a = 5 OR a = 11

Better:

... WHERE a IN (2, 5, 11)

Bây giờ, trình tối ưu hoá SQL của bạn có thể biến truy vấn đầu tiên thành thứ hai. Nhưng nó có thể không. Chỉ cần không làm điều đó.

**15. Không thiết kế mô hình dữ liệu để có giải pháp hiệu suất cao**

Đây là một điểm khó để thể lường. Nó thường được theo dõi hiệu quả của nó. Nếu bạn thấy mình đang biết các câu truy vấn cho các việc tương đối đơn giản hay các truy vấn để tìm các thông tin tương đối đơn giản nhưng không hiệu quả, thì bạn chắc chắn đang có mô hình dữ liệu tệ.

Trong một số điểm này tóm tắt tất cả những thứ trước đó nhưng đó là một câu chuyện cảnh giác hơn mà làm những thứ như tối ưu hóa truy vấn thường được thực hiện đầu tiên khi nó nên được thực hiện thứ hai. Trước hết bạn phải đảm bảo bạn có một mô hình dữ liệu tốt trước khi cố gắng để tối ưu hóa hiệu suất. Như Knuth đã nói:

> Tối ưu hóa sớm là gốc rễ của tất cả các điều tệ hại

**16. Sử dụng Database Transactions sai cách**

Tất cả dữ liệu thay đổi cho một quá trình cụ thể nên là **atomic**. **I.e**. Nếu hoạt động thành công, nó đầy đủ. Nếu nó không thành công,dữ liệu không bị thay đổi. - Không nên có những thay đổi "xong một nửa".

Lý tưởng thì cách tốt nhất để đạt được điều này là thiết kế toàn bộ hệ thống nên cố gắng hỗ trợ toàn bộ thay đổi dữ liệu qua từng câu lệnh INSERT/UPDATE/DELETE đơn. Trong trường hợp này, không có xử lý transaction đặc biệt nào cần thiết cả, vì engine cơ sở dữ liệu của bạn nên làm điều này tự động.

Tuy nhiên, nếu bất kỳ quy trình nào yêu cầu nhiều lệnh được thực hiện như một đơn vị để giữ dữ liệu ở trạng thái nhất quán, thì cần Transaction Control thích hợp cho cần thiết

- Begin a Transaction trước đầu tiên.
- Commit the Transaction sau cuối cùng.
- Trên bất kỳ lỗi nào, hãy Hoàn tác Giao dịch. Và rất NB! Đừng quên bỏ qua / hủy bỏ tất cả các câu lệnh theo sau lỗi.

Cũng nên chú ý cẩn thận đến các subtelties của lớp kết nối cơ sở dữ liệu của bạn, và công cụ cơ sở dữ liệu tương tác về vấn đề này.

**17. Chưa nắm rõ mô hình 'set-based'**

 Ngôn ngữ SQL tuân theo mô hình cụ thể phù hợp với các loại vấn đề cụ thể. Mặc dù có nhiều phần mở rộng cung cấp cụ thể, cuộc đấu tranh ngôn ngữ đẻ giải quyết vấn đề này là không đáng kể trong ngôn ngữ như Java, C#, Delphi v.v...

Sự thiếu hiểu biết này thể hiện theo một vài cách.

- Áp đặt quá nhiều các phương pháp và logic bắt buộc không phù hợp trên cơ sở dữ liệu
- Sử dụng con trỏ không phù hợp hoặc 1 cách quá đáng. Đặc biệt khi câu truy vấn đơn đầy đủ.
- Giả định sai rằng trigger thực hiện ngay khi mỗi dòng bị ảnh hưởng trong cập nhiều đa-dòng

Xác định phân chia trách nhiệm rõ ràng, và cố gắng sử dụng công cụ thích hợp để giải quyết từng vấn đề.