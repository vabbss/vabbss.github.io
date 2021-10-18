---
bố cục: bài
title: Ghi một trò chơi bảo mật thông tin ngoại tuyến
các thẻ: [CTF, thâm nhập, bảo mật thông tin]
---

Tôi vẫn cảm thấy rằng việc thâm nhập vào thực tế rất thú vị ... <! - more ->

# Nguyên nhân
Trong những năm gần đây, tôi đã trải qua [CTF trực tuyến] (/ 2019/12/16 / ctf.html) và [trở thành người tổ chức CTF] (/ 2020/11/24 / createctf.html), nhưng vì nhiều lý do khác nhau, tôi chưa tham gia cuộc thi offline an toàn thông tin kiểu này. Gần đây, một trường học ở Tứ Xuyên đã tổ chức một cuộc thi ngoại tuyến về bảo mật thông tin, và trường chúng tôi đã yêu cầu tôi và các bạn cùng lớp tham gia.
Trong nhóm của chúng tôi, ngoại trừ tôi, những người khác đều chuyên nghiệp về bảo mật thông tin. Tôi nên làm gì với tư cách là một người kỹ thuật mạng😂? Nhưng dù sao đi nữa, tôi đã [tìm thấy lỗ hổng] (/ 2021/05/15 / lỗ hổng.html), [công cụ trang web tự tạo] (/ 2019/07/01 / mabbs.html) và tôi cũng đang trong quá trình này bảo trì trang web [phải Có hiểu biết nhất định về tấn công và phòng thủ mạng] (/ 2019/02/17 / break.html), cộng với kinh nghiệm trên, ít nhất tôi vẫn có thể so sánh với các chuyên gia an toàn thông tin về mặt kiến ​​thức.

# Kinh nghiệm và quá trình
Cuộc thi này có hai phần, phần thứ nhất là CTF và phần thứ hai là thâm nhập mạng nội bộ. Ngoài ra, sự khác biệt giữa ngoại tuyến và trực tuyến là mỗi đội sẽ cung cấp máy bay không người lái riêng biệt. Tôi nghe nói rằng Flag có vẻ như có một số câu hỏi độc đáo. Một điểm khác biệt lớn nữa là các máy tính ngoại tuyến không được phép kết nối Internet và tín hiệu điện thoại di động cũng bị chặn. Tuy nhiên, người ta nói rằng chỉ các tín hiệu dưới 4G và tín hiệu WiFi 2.4G và tín hiệu Bluetooth dường như bị che chắn. Mạng di động 5G và WiFi 5G không bị ảnh hưởng. Ngoài ra, có thể cung cấp một máy tính có kết nối Internet để kiểm tra dữ liệu trong thời gian giới hạn . Vì vậy, tôi chỉ không thể hiểu hoạt động này. Ngoài ra, tôi đã bí mật đặt mật khẩu của mạng trong khuôn viên trường của họ thông qua các nhân viên xã hội, vì vậy tôi thực sự có thể kết nối Internet bình thường.
## Phần CTF
Mặc dù tôi đã chơi CTF nhưng thực sự thì tôi vẫn chưa biết gì về CTF, tôi không hiểu về phần Crypto và cũng không biết sử dụng công cụ gì. Misc chỉ tiếc là tôi có thể làm được và các công cụ là không đủ. Câu hỏi trên web không sao, ít nhất là một chút nhưng tôi vẫn chưa biết SQL injection. Những gì Pwn khác sẽ sử dụng Reverse và những câu hỏi khác về cơ bản không có cơ hội với chúng tôi, dù sao thì những người trong nhóm của chúng tôi sẽ không biết, và cũng không có công cụ.
Trên thực tế, thành thật mà nói, tôi chưa thấy CTF nào được sử dụng trong đời thực cho đến bây giờ. Reverse và Web có thể hữu ích. Ai đó có thể sử dụng mã hóa AES bình thường như Crypto để bẻ khóa nó mà không bạo lực không? Và thử nghiệm Misc là gì? Vấn đề phức tạp? Thuật toán mã hóa ban đầu của tôi có lỗ hổng. Ai đó có thể giải quyết nó không?
Tóm lại, CTF không thực sự có nhiều ý nghĩa, nhưng nếu bạn nói vậy, hãy để tôi chia sẻ một số câu hỏi thú vị mà tôi có thể đưa ra.
Một trong những câu hỏi thú vị nhất trong câu hỏi web này là MD5 bằng chính nó. Tuy nhiên, có vẻ như công nghệ của con người vẫn chưa tìm ra được giá trị như vậy. Có thể mã là như thế này:
```php
<?php
include 'flag.php';
highlight_file("index.php");
if(isset($_GET["a"])){
  $a = $_GET["a"];
  if($a == md5($a)){
    echo $flag;
  }
}
```
```python
from hashlib import md5
for i in range(1,999999999):
    a = md5(("0e"+str(i)).encode()).hexdigest()
    if a[0:2] == "0e":
        try:
            int(a[2:])
            print(i)
        except:
            aaa = 0
```
Vì thời gian eo hẹp nên tôi chỉ viết tùy tiện thôi, dù sao cũng có tác dụng. Cuối cùng, giá trị đầu tiên được tính là "0e215962017", và sau đó là ổn, vì md5 của nó là "0e291242476940776845150308577824", tương đương với "0 = 0" đối với PHP. Baidu rác rưởi thực sự chỉ có thể tìm thấy một lượng nhỏ kết quả trừ khi bạn trực tiếp tìm kiếm giá trị này. Những người khác thực sự không thể tìm kiếm được ...
Tôi không biết làm thế nào để làm bất kỳ câu hỏi nào khác, vì vậy chúng ta hãy dừng lại ở đây cho CTF.
## Phần thâm nhập mạng nội bộ
Việc thâm nhập Intranet vẫn có một chút thú vị so với CTF, đặc biệt là đối với những người làm kỹ thuật mạng như tôi, ít nhất tất cả các loại kết nối đều tương đối dễ dàng. Và trên thực tế, tôi cũng đã chơi thâm nhập trong thực tế, ví dụ như mạng trường học dễ thâm nhập hơn mạng trong các cuộc thi. Ngoài ra, tôi cũng đã viết một bài về [Kết nối hai mạng bất kỳ] (/ 2021/05/07 / ssh.html), vì vậy phần này sẽ thuận tiện hơn cho tôi.
Tuy nhiên, qua cuộc thi này, tôi vẫn thấy rằng bản thân cuộc thi không có nhiều ý nghĩa, ví dụ như cổng vào mà chúng tôi vào trong cuộc thi là một CMS có tên là beecms, tuy nhiên, ban tổ chức cuộc thi lại không đưa ra mã số cho chúng tôi. đoán một cách mù quáng. Điều này không đúng. Không, ở nhiều nơi trong CMS này, chúng tôi có thể thấy những điểm có thể bị SQL chèn vào, nhưng nhóm của chúng tôi đã thử nhiều cách và không thành công. Cuối cùng, tôi đã tìm thấy [một bài báo] (https://www.anquanke.com/post/id/98574) trên Baidu và nói rằng bạn có thể trực tiếp đăng một chuỗi trên trang chủ:
``
_SESSION [login_in] = 1 & _SESSION [admin] = 1 & _SESSION [login_time] = 99999999999
``
Một thứ như vậy có thể trực tiếp sửa đổi Phiên để bỏ qua đăng nhập 😓 ...... Tôi có thể tìm thấy loại lỗ hổng này mà không cần kiểm tra thứ này không? Cuối cùng, tôi đã tìm thành công 4 cờ trên đó và vào mạng nội bộ tiếp theo.
Trong mạng nội bộ tiếp theo, có hai cơ sở dữ liệu MySQL và một trang web Tomcat, cũng có một trang web với phần quản lý nền được viết trên đó, nhưng không thể nhìn thấy khuôn khổ nào. Theo lời nhắc, Tomcat và cơ sở dữ liệu là mật khẩu yếu, và cuối cùng đoán rằng tên người dùng Tomcat là "tomcat" và mật khẩu là "tomcat123". Vấn đề với cơ sở dữ liệu là chúng tôi có thể đăng nhập thành công, nhưng các công cụ liên quan không thể kết nối với proxy ... Tôi không có bất kỳ công cụ nào vì tôi không bảo mật thông tin. Proxy toàn cầu với các công cụ bị hỏng, Tôi không biết tại sao Nó không thể kết nối ... vì vậy không có cách nào ... Ngoài ra, thực sự rất keo kiệt khi không đưa ra cờ sau khi cơ sở dữ liệu vào. Người ta ước tính rằng cờ nằm ​​trong hệ thống tệp Tôi nghe nói rằng UDF có thể được sử dụng để nâng cao quyền. Tomcat là bàn đạp để vào lớp thứ 2. Sau khi thiết lập hai lớp tác nhân trên đó, tôi có thể vào mạng đó. Trong mạng đó, có một hệ thống PHPOA, một trang Weblogic và một hệ thống phải là Windows7. ~~ Có vẻ như chỉ có cổng SMB được mở, và có vẻ như nó nên được chơi với Eternal Blue ... ~~ (Tôi nghe ông lớn nói rằng có vẻ như đang sử dụng IPC $?) Nhưng cụ thể là vì công cụ của đồng đội không thể kết nối ... không có cách nào.
Không có nhiều thông tin về PHPOA, nhưng tôi vẫn tìm thấy [thông tin liên quan] (https://blog.csdn.net/weixin_39788986/article/details/110226286) thông qua Baidu, nói rằng có một đường dẫn trong `/ ntko / upLoadOfficeFile .php` Lỗ hổng do upload file tùy tiện khiến tôi tê cả người, nếu cái này không cho code hay mạng thì tôi có thể dựa vào tiên tri để biết được lỗ hổng này không?
Mặc dù tôi đã tìm thấy một hướng dẫn trên trang web Weblogic, tôi đã nói rằng tôi có thể sử dụng địa chỉ `/ console / css /% 252e% 252e% 252fconsole.portal` để bỏ qua đăng nhập trực tiếp, nhưng tôi chưa sử dụng nền tảng đó nhiều. Tôi không biết cách triển khai hay cách thay đổi mật khẩu. Mật khẩu nên cuối cùng không thành công ...
Cuối cùng kết quả cũng không tồi, mình làm được 6 câu, cho hơn 1k điểm, còn có máu thứ nhất và máu thứ hai.

# Tổng hợp
Kết thúc trò chơi, kết quả không tồi, đội chúng tôi đã xuất sắc giành giải ba. Nhưng sau cuộc thi này, mình thấy việc thi lấy chứng chỉ trong thực tế sẽ hữu ích hơn CTF, việc thâm nhập vào môi trường thực tế thú vị hơn rất nhiều so với cuộc thi, vì vậy nếu bạn thực sự muốn chơi thì cứ chơi trong thực tế. Tất nhiên, tôi nghĩ cơ hội này cũng thú vị, ở một mức độ nào đó, nó có thể mang lại cho tôi một chút thú vị thông qua phương pháp Đánh Cờ, đây cũng là một trải nghiệm khá hay.
