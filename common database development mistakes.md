**9. Chuẩn hóa thiếu**

[Chuẩn hóa cơ sở dữ liệu](http://en.wikipedia.org/wiki/Database_normalization) về cơ bản là quá trình tối ưu hóa thiết kế cơ sở dữ liệu hoặc làm thế nào bạn tổ chức dữ liệu của bạn vào các bảng.

Chỉ trong tuần này tôi đã lướt qua một số code, nơi mà ai đó đã tách một mảng và chèn nó vào một trường duy nhất trong cơ sở dữ liệu. Chuẩn hóa điều đó sẽ xử lý phần tử của mảng đó như một hàng riêng biệt trong bảng con (mối quan hệ một - nhiều).

Điều này cũng được đưa ra trong [Phương pháp tốt nhất để lưu trữ danh sách ID người dùng:](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi đã nhìn thấy trong các hệ thống khác mà danh sách được lưu trữ trong một mảng PHP tuần tự.

Tuy nhiên, sự thiếu chuẩn hóa lại có nhiều hình thức.

Tìm hiểu thêm:

- [Chuẩn hóa: Bao nhiêu là đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Tại sao bạn cần một CSDL chuẩn hóa? ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hóa quá mức**

Điều này có thể xem như một mâu thuẫn với điều ở trên nhưng chuẩn hóa, như những thứ khác, là một công cụ. Đó là một phương tiện để kết thúc và không phải là một kết thúc và của chính nó. Tôi nghĩ nhiều Developer quên điều này và bắt đầu thay đổi một "phương tiện" thành "sự kết thúc". Unit testing là một ví dụ điển hình của điều này

Tôi đã làm việc mỗi tuần 1 lần trên hệ thống mà có độ phân cấp cực lớn cho các Client như sau:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

như vậy bạn đã phải join khoảng 11 bảng với nhau trước khi bạn có thể nhận được bất kỳ dữ liệu có ý nghĩa. Đó là một ví dụ điển hình về quá trình chuẩn hóa quá nhiều.

Thêm đó, chú ý và xem xét không chuẩn hóa có thể có những lợi ích rất lớn nhưng bạn phải cẩn thận khi làm việc này.

Thêm nữa:

- [Tại sao chuẩn hóa quá mức CSDL lại là một điều xấu](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Chuẩn hóa CSDL bao nhiêu là đủ?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [Khi nào không cần chuẩn hóa CSDL](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Có lẽ chuẩn hóa lại là điều không bình thương](http://www.codinghorror.com/blog/archives/001152.html)
- [Nguyên do của các cuộc tranh luận về việc chuẩn hóa CSDL nằm ở code](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)


**11. Sử exclusive arcs**

Một exclusive arc là một sai lầm phổ biến mà một bảng được tạo ra với hai hoặc nhiều khóa ngoại, trong đó một và chỉ một trong số chúng có thể không có giá trị.  **Sai lầm lớn.** đây là điều mà nó trở nên khó khăn hơn nhiều để duy trì tính toàn vẹn dữ liệu. Sau tất cả, ngay cả khi với các ràng buộc tham chiếu, không có gì ngăn ngừa hai hoặc nhiều hơn các khóa ngoại này được thiết lập (mặc dù kiểm tra ràng buộc phức tạp).

Nguồn [Hướng dẫn Thực tiễn về Thiết kế Cơ sở dữ liệu Quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> Chúng tôi đã chỉ ra rõ ràng chống lại việc xây dựng exclusive arc bất cứ lúc nào, vì lý do tốt đó mà họ có thể được lúng túng để viết code và gây ra những khó khăn về bảo trì.

**12. Không phân tích hiệu suất của các truy vấn**

Chủ nghĩa thực dụng thống trị tối cao, đặc biệt trong môi trường cơ sở dữ liệu. Nếu bạn đang tuân thủ các nguyên tắc đến mức chúng đã trở thành một thói quen thì bạn có thể rất dễ phạm sai lầm. Lấy ví dụ của các truy vấn gộp từ phía trên. Phiên bản gộp có thể trông "hợp lí" nhưng hiệu suất của nó là không tốt. Một so sánh hiệu suất đã chấm dứt cuộc tranh luận (nhưng nó không) nhưng thêm một điểm là: việc đưa quá nhiều view thông báo xấu ngay trong vị trí đầu tiên là ngu ngốc, thậm chí là nguy hiểm.

**13. Quá phụ thuộc vào UNION ALL và đặc biệt cấu trúc UNION**

Một UNION trong câu lệnh SQL đơn giản chỉ ghép các tập dữ liệu đồng nhất, có nghĩa là chúng có cùng kiểu và số cột. Sự khác biệt giữa chúng là UNION ALL là một kết nối đơn giản và nên được ưa thích hơn bất cứ nơi nào có thể trong khi UNION sẽ ngầm làm một DISTINCT để loại bỏ các bản sao trùng lặp.

UNIONs, như DISTINCT, có vai trò của riêng chúng. Có các ứng dụng hợp lệ. Nhưng nếu bạn thấy mình đang làm rất nhiều trong số họ, đặc biệt là trong các truy vấn phụ, sau đó có thể bạn thấy đang làm gì sai. Đó có thể là bạn đang xây dựng truy vấn kém hoặc mô hình dữ liệu được thiết kế kém buộc bạn phải làm những việc như vậy.

UNION, đặc biệt khi được sử dụng trong joins hoặc các truy vấn phụ phụ thuộc, có thể làm tê liệt cơ sở dữ liệu. Cố gắng tránh chúng bất cứ khi nào có thể.

**14. Sử dụng điều kiện OR trong truy vấn**

Chúng có vẻ vô hại. Nhưng sau tất cả, AND lại ổn. OR cũng nên được như vậy phải không? Sai rồi. Về cơ bản một điều kiện AND **hạn chế** dữ liệu cài đặt trong khi một điều kiện OR **kích thích** điều này nhưng không phải là cách để cho phép chúng tối ưu hóa. 
Đặc biệt khi các điều kiện OR khác nhau có thể giao cắt, do đó buộc trình tối ưu hóa có hiệu quả để một hoạt động DISTINCT trong kết quả.

Tồi tệ:

... WHERE a = 2 OR a = 5 OR a = 11

Tốt hơn:

... WHERE a IN (2, 5, 11)

Bây giờ, trình tối ưu hoá SQL của bạn có thể biến truy vấn đầu tiên thành thứ hai. Nhưng nó có thể không. Chỉ cần không làm điều đó.

**15. Không thiết kế mô hình dữ liệu để có giải pháp hiệu suất cao**

Đây là một điểm khó để thể lường. Nó thường được theo dõi hiệu quả của nó. Nếu bạn thấy mình đang biết các câu truy vấn cho các việc tương đối đơn giản hay các truy vấn để tìm các thông tin tương đối đơn giản nhưng không hiệu quả, thì bạn chắc chắn đang có mô hình dữ liệu tệ.

Trong một số điểm này tóm tắt tất cả những thứ trước đó nhưng đó là một câu chuyện cảnh giác hơn mà làm những thứ như tối ưu hóa truy vấn thường được thực hiện đầu tiên khi nó nên được thực hiện thứ hai. Trước hết bạn phải đảm bảo bạn có một mô hình dữ liệu tốt trước khi cố gắng để tối ưu hóa hiệu suất. Như Knuth đã nói:

> Tối ưu hóa sớm là nguồn cơ của mọi rắc rối.

**16. Sử dụng Database Transactions sai cách**

Tất cả dữ liệu thay đổi cho một quá trình cụ thể nên là **atomic**. **I.e**. Nếu hoạt động thành công, nó đầy đủ. Nếu nó không thành công,dữ liệu không bị thay đổi. - Không nên có những thay đổi "xong một nửa".


Lý tưởng thì cách tốt nhất để đạt được điều này là thiết kế toàn bộ hệ thống nên cố gắng hỗ trợ toàn bộ thay đổi dữ liệu qua từng câu lệnh INSERT/UPDATE/DELETE đơn. Trong trường hợp này, không có xử lý transaction đặc biệt nào cần thiết cả, vì engine cơ sở dữ liệu của bạn nên làm điều này tự động.

Tuy nhiên, nếu bất kỳ quy trình nào yêu cầu nhiều lệnh được thực hiện như một đơn vị để giữ dữ liệu ở trạng thái nhất quán, thì cần Transaction Control thích hợp cho cần thiết

- Bắt đầu một giao dịch trước câu lệnh đầu tiên.
- Commit giao dịch sau câu lệnh cuối cùng.
- Trong bất kì hoàn cảnh nào, hãy phục hồi giao dịch. Đừng quên skip/abort tất cả câu lệnh theo lỗi đó.

Cũng nên chú ý cẩn thận đến các subtelties của lớp kết nối cơ sở dữ liệu của bạn, và công cụ cơ sở dữ liệu tương tác về vấn đề này.

**17. Chưa nắm rõ mô hình 'set-based'**

 Ngôn ngữ SQL tuân theo mô hình cụ thể phù hợp với các loại vấn đề cụ thể. Mặc dù có nhiều phần mở rộng cung cấp cụ thể, cuộc đấu tranh ngôn ngữ đẻ giải quyết vấn đề này là không đáng kể trong ngôn ngữ như Java, C#, Delphi v.v...


Sự thiếu hiểu biết này thể hiện theo một vài cách.
- Áp dụng quá nhiều thủ tục hay bắt buộc logic trong CSDL
- Việc sử dụng không phù hợp hoặc quá mức của các con trỏ. Đặc biệt là khi một câu truy vấn đơn nhất là đủ.
-Giả định không đúng kích hoạt một lần mỗi hàng bị ảnh hưởng trong cập nhật nhiều hàng.

Xác định phân chia trách nhiệm rõ ràng, và cố gắng sử dụng công cụ thích hợp để giải quyết từng vấn đề.