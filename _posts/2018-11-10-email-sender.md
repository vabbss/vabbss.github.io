---
bố cục: bài
title: Đăng ký miễn phí email hàng ngày của riêng bạn
tags: [miễn phí, email, hàng ngày, trải nghiệm]
---

Vài ngày trước, tôi đã tạo một hệ thống đăng ký email cho chính mình <! - more ->, được tạo bằng PHP. Tôi phải khen PHP ở đây, PHP quả thực là ngôn ngữ tốt nhất trên thế giới, tôi chưa học PHP bao giờ, nhưng tôi chỉ tìm thấy nó trên Baidu
Hệ thống đăng ký thư này có thể được thực hiện bằng cách kết hợp những điều ở trên. Nó vẫn rất tốt và có nhiều không gian PHP miễn phí trên Internet, vì vậy bạn có thể dễ dàng xây dựng một hệ thống đăng ký thư miễn phí cho mình.

# Phương pháp sản xuât
Rất đơn giản, trước tiên hãy truy cập Baidu để tìm kiếm một không gian lưu trữ PHP miễn phí với sendmail và CronTab, sau đó tạo một tệp PHP trên đó, bất kỳ tên nào bạn muốn, miễn là hậu tố là PHP, sau đó đặt như sau mã số
Dán nó, lưu nó, sau đó đặt tác vụ CornTab trên bảng điều khiển máy chủ, đặt nó chạy mỗi ngày một lần và sau đó OK ... Nhân tiện, hãy nhớ thay đổi địa chỉ trong biến `$ to` bên dưới thành địa chỉ của riêng bạn địa chỉ email, nếu không bạn sẽ gửi nó mỗi khi Thư được gửi
Đó là hộp thư của tôi ...

# Mã số
`` PHP
<? php
function curl_post_https ($ url, $ data) {// mô phỏng hàm gửi dữ liệu
    $ curl = curl_init (); // bắt đầu phiên CURL
    curl_setopt ($ curl, CURLOPT_URL, $ url); // địa chỉ được truy cập
    curl_setopt ($ curl, CURLOPT_SSL_VERIFYPEER, 0); // Kiểm tra nguồn của chứng chỉ xác thực
    curl_setopt ($ curl, CURLOPT_SSL_VERIFYHOST, 1); // Kiểm tra xem thuật toán mã hóa SSL có tồn tại từ chứng chỉ hay không
    curl_setopt ($ curl, CURLOPT_FOLLOWLOCATION, 1); // Sử dụng chuyển hướng tự động
    curl_setopt ($ curl, CURLOPT_AUTOREFERER, 1); // tự động đặt Người giới thiệu
    curl_setopt ($ curl, CURLOPT_POST, 1); // Gửi yêu cầu Đăng bài thông thường
    curl_setopt ($ curl, CURLOPT_POSTFIELDS, $ data); // Gói dữ liệu gửi qua Bưu điện
    curl_setopt ($ curl, CURLOPT_TIMEOUT, 30); // Đặt giới hạn thời gian chờ để ngăn vòng lặp vô hạn
    curl_setopt ($ curl, CURLOPT_HEADER, 0); // Hiển thị nội dung của vùng Header trả về
    curl_setopt ($ curl, CURLOPT_RETURNTRANSFER, 1); // Thông tin thu được được trả về dưới dạng một luồng tệp
    $ tmpInfo = curl_exec ($ curl); // Thực hiện thao tác
    if (curl_errno ($ curl)) {
        echo'Errno'.curl_error ($ curl); // Bắt ngoại lệ
    }
    curl_close ($ curl); // Đóng phiên CURL
    $ backdata = json_decode ($ tmpInfo, true);
    return $ backdata ['text']; // trả về dữ liệu, định dạng json
}
hàm w_get () {
        $ url = 'https: //yuri.gear.host/talk.php';
        $ data ['info'] = 'Thời tiết tại một địa điểm';
        $ data ['userid'] = 'Mayx_Mail';
        $ retdata = curl_post_https ($ url, $ data);
        $ data ['info'] = 'Thời tiết ngày mai ở một địa điểm';
        $ retdata = $ retdata. "<br>" .curl_post_https ($ url, $ data);
        $ data ['info'] = 'Thời tiết ở một địa điểm ngày mốt';
        $ retdata = $ retdata. "<br>" .curl_post_https ($ url, $ data);
        return $ retdata; // trả về json
}
hàm xh_get () {
        $ url = 'https: //yuri.gear.host/talk.php';
        $ data ['info'] = 'Kể chuyện cười';
        $ data ['userid'] = 'Mayx_Mail';
        $ retdata = curl_post_https ($ url, $ data);
        return $ retdata; // trả về json
}
hàm xw_get () {
// Mảng danh sách địa chỉ nguồn RSS
$ rssfeed = array ("http://www.people.com.cn/rss/it.xml");
 
for ($ i = 0; $ i <sizeof ($ rssfeed); $ i ++) {// Bắt đầu phân rã
    $ buff = "";
    $ rss_str = "";
    // Mở địa chỉ rss và đọc, nếu đọc không thành công, nó sẽ dừng lại
    $ fp = fopen ($ rssfeed [$ i], "r") hoặc die ("không thể mở $ rssfeed");
    trong khi (! feof ($ fp)) {
        $ buff. = fgets ($ fp, 4096);
    }
    // Đóng mở tệp
    fclose ($ fp);
 
    // Xây dựng trình phân tích cú pháp XML
    $ parser = xml_parser_create ();
    // xml_parser_set_option - Đặt các tùy chọn để phân tích cú pháp XML được chỉ định
    xml_parser_set_option ($ parser, XML_OPTION_SKIP_WHITE, 1);
    // xml_parse_into_struct - Phân tích cú pháp dữ liệu XML thành mảng $ giá trị
    xml_parse_into_struct ($ phân tích cú pháp, $ buff, $ giá trị, $ idx);
    // xml_parser_free - phát hành trình phân tích cú pháp XML được chỉ định
    xml_parser_free ($ parser);
    $ j = 0;
    foreach ($ giá trị dưới dạng $ val) {
        $ tag = $ val ["tag"];
        $ type = $ val ["type"];
        $ value = $ val ["giá trị"];
        // Nhãn được chuyển đổi đồng nhất thành chữ thường
        $ tag = strtolower ($ tag);
 
        if ($ tag == "item" && $ type == "open") {
            $ is_item = 1;
        } else if ($ tag == "item" && $ type == "close") {
            // Xây dựng chuỗi đầu ra
            $ rss_str. = "<a href='".$link."' target=_blank>". $ title. "</a> <br />";
            $ j ++;
            $ is_item = 0;
        }
        // Chỉ đọc nội dung trong thẻ item
        if ($ is_item == 1) {
            if ($ tag == "title") {$ title = $ value;}
            if ($ tag == "liên kết") {$ link = $ value;}
        }
    nếu ($ j == 20) {
        nghỉ;
    }
    }
    // Kết quả đầu ra
    return $ rss_str. "<br />";
}
}
$ to = "mayx@outlook.com, unaayx@139.com";
$ subject = "Mayx hàng ngày";
$ txt = "
<html>
<body>
<h1> Mayx Daily </h1> <hr> Xin chào, hôm nay là ". date (" Y-m-d ").", sau đây là hôm nay hàng ngày: <br> <small>
". file_get_contents (" http://mappi.000webhostapp.com/hitokoto/ ")." </small>
<h2> Dự báo thời tiết </h2> ". w_get ()." <h2> Truyện cười hàng ngày </h2> ". xh_get ()." <h2> Tin tức hôm nay </h2> ". xw_get ()." <hr > <small> ". file_get_contents (" https://api.gushi.ci/all.txt ")." </small> <br> <center> Được tạo bởi <a href = \ "https: // mabbs. github.io \ "> Mayx </a> </center>
</body>
</html>
";
$ headers = "MIME-Phiên bản: 1.0". "\ r \ n".
"Loại nội dung: text / html; charset = utf-8". "\ R \ n".
"Từ: Mayx_Daily <Mayx_Site>";

thư ($ to, $ subject, $ txt, $ headers);
?>
``
(Bản cập nhật 2018.11.12: thêm tin tức hôm nay)
(Bản cập nhật 2018.11.13: giới hạn số lượng mục tin tức ở top 20)

# Tái bút
Thành thật mà nói, tôi giải quyết vấn đề này tốt hơn với Linux Shell. Thật không may, dường như không có dịch vụ lưu trữ đám mây miễn phí trên Internet. Tôi nghe nói rằng Travis-CI dường như có thể làm được điều này, nhưng thành thật mà nói, tiếng Anh của tôi không tốt lắm, vì vậy tôi có thể hiểu các tài liệu ngắn hơn vẫn ổn, ngay cả khi chúng quá dài ...
Nhưng tôi vẫn cố sử dụng Travis-CI để giải quyết vấn đề này, link: [Mayx 日报] (https://mayx.tk/)
Nhân tiện, hộp thư riêng của nhà điều hành có thể được đặt để nhắc nhở SMS, vì vậy nó cũng có thể được sử dụng để gửi tin nhắn văn bản đến điện thoại di động và gửi dự báo thời tiết cho chính bạn mỗi ngày ... Sau đó, trong trường hợp này, hãy thay thế thành phố. trong dự báo thời tiết với chính bạn Thành phố bây giờ!
Nếu bạn không nhận được email, hãy truy cập email spam để tìm nó, sau đó đặt địa chỉ email vào danh sách trắng.
Nếu ai muốn thử tính năng này, bạn có thể để lại lời nhắn bên dưới, và tôi sẽ thêm bạn vào máy chủ của tôi sau khi xác minh được thông qua.
