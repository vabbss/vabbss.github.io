---
bố cục: bài
title: DIY a Server Sauce · Phiên bản TurboMini
các thẻ: [Máy ​​chủ, PHP]
---

Bạn có dám tính phí cho những vấn đề có thể giải quyết trong một câu nói? <! - more ->

# Nguyên nhân
Tôi đã từng sử dụng nước sốt Máy chủ để gửi báo hàng ngày cho tôi hoặc cho tôi biết liệu Raspberry Pi có khởi động bình thường hay không. Lý do sử dụng nó là API khá tiện lợi và tôi thường sử dụng WeChat rất nhiều và thật tốt khi có thể đẩy thông tin trực tiếp qua WeChat.
Sau này, một phiên bản Turbo đã được thêm vào Server sauce, nhưng tôi đã sử dụng phiên bản thông thường khá tốt, vì vậy tôi không quan tâm lắm. Kết quả là, thông báo nào đã được gửi hôm nay, nói rằng dịch vụ của họ có thể bị tạm ngừng? Điều mình ghét nhất là dịch vụ không ổn định, thậm chí có người dịch vụ không ổn định thì cũng không nên làm dịch vụ gì cả, làm xong cũng không có hại gì, LeanCloud như trước cũng không hoạt động được nên mình cũng [Tôi đã tự viết một Blog Counter] (/ 2019/06/22 / counter.html).
Sau đó, tôi xem qua phiên bản Server Sauce · Turbo của họ. Tốt lắm, họ vẫn tính phí 8CNY / tháng là hơi quá. Sau khi đọc nó, tôi nói không, cái này quá đắt, nhưng tôi muốn xem nó sẽ tốn bao nhiêu tài nguyên.
Trước đây, tôi đã xem họ nổ tung và nói rằng tôi yêu cầu 5kw mỗi tháng. Tôi đã nói vấn đề với nó là gì. Học viện Huahuo của tôi yêu cầu hàng trăm triệu yêu cầu mỗi tháng và nó không tốn vài đô la. Tôi dám đến ra và thổi nó ra cho 5kw? Dám bắt đầu một lớp học? Vì vậy, hôm nay tôi sẽ xem xét mức độ phức tạp của cái gọi là "cấu hình hơi phức tạp" này.

# Cố gắng để làm điều đó
Tôi đã đọc tài liệu về giao diện WeChat trên tài khoản thử nghiệm, nó có vẻ không quá phức tạp, tôi đoán vậy và có thể làm được nhiều nhất trong một câu! Sau đó, tôi đã cố gắng viết nó bằng PHP.
Mã cuối cùng như sau:
```php
<?php
$appid='appID';
$secret='appsecret';
$userid='微信号（OpenID）';
$template_id='模板ID';
$title='标题';
$content='内容';
file_get_contents('https://api.weixin.qq.com/cgi-bin/message/template/send?access_token='.json_decode(file_get_contents('https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid='.$appid.'&secret='.$secret),true)['access_token'], false, stream_context_create(array('http' => array('method'=>'POST','header'=>"Content-Type: application/json;charset=utf-8",'content'=>'{"touser":"'.$userid.'","template_id":"'.$template_id.'","data":{"title": {"value":"'.$title.'"},"content": {"value":"'.$content.'"}}}'))));
```
Viết xong và test thì thấy hiệu quả khá tốt, gần giống với mã số test nước sốt máy chủ, tất nhiên là không có trang web nào mở được, tất nhiên là bạn có thể làm được, rất đơn giản chỉ cần chèn một URL. Nếu bạn muốn Để biết thêm các hiệu ứng đặc biệt, bạn cũng có thể truy cập [Tài liệu giao diện mẫu] (https://developers.weixin.qq.com/doc/offiaccount/Message_Management/Template_Message_Interface.html).
Về các hạn chế, nó tốt hơn so với nước sốt Máy chủ. Về lý thuyết, mã của tôi có thể được gửi 2000 lần một ngày, chủ yếu là do giao diện access_token chỉ có thể được sử dụng 2000 lần một ngày, nhưng nếu access_token có thể được lưu vào bộ nhớ cache thì về mặt lý thuyết, nó có thể được gửi 100.000 lần một ngày. Nó tốt hơn nhiều so với 1000 lần sốt Máy chủ rác.

# Làm thế nào để lấy các tham số?
Mã tôi đã viết tương thích với Nước sốt máy chủ, vì vậy nó có thể được sử dụng trực tiếp bằng cách làm theo hướng dẫn cấu hình của họ, nhưng một số người thậm chí có thể không biết Nước sốt máy chủ là gì và tôi sẽ không rút lưu lượng truy cập cho họ để không lãng phí giá trị quý giá của họ. tài nguyên máy chủ.
Việc cần làm rất đơn giản, trước tiên hãy mở [trang ứng dụng] (https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login), sau đó quét mã để đăng nhập, và bạn sẽ có thể Xem trang quản lý số kiểm tra.
Điều đầu tiên chúng ta thấy là appID và appsecret, vì vậy chúng ta có hai tham số. Đối với hai mã còn lại, tiếp tục cuộn xuống và tìm mã QR của số kiểm tra, sau khi quét và làm theo mã đó bằng WeChat, ID WeChat của chính bạn sẽ xuất hiện. Tất nhiên, đây không phải là ID WeChat thật, nó tương đương với chỉ là một mã nhận dạng, vì vậy tham số thứ ba Cũng có nó. Tiếp theo là thông số thứ 4. Tìm giao diện thông báo mẫu, bấm để thêm mẫu thử nghiệm, nhập tiêu đề thông báo đẩy hoặc bất cứ thứ gì bạn thích, điền nội dung:
```
{% raw %}{{title.DATA}}
{{content.DATA}}{% endraw %}
```
Sở dĩ điền theo cách này là để tương thích với Server sauce, tất nhiên bạn cũng có thể tự đổi mã và điền thêm thứ khác. Nhưng nếu bạn không muốn thay đổi mã và thêm chữ ký vào cuối, thì không có vấn đề gì, ví dụ:
```
{% raw %}{{title.DATA}}
{{content.DATA}}{% endraw %}
--By Mayx
```
Bằng cách này, chúng tôi cũng nhận được ID mẫu tham số thứ tư, vì vậy mã trên sẽ có thể được sử dụng bình thường.
Một điều cần lưu ý là vì những lý do không thể giải thích được, có thể do appecret lấy được lần đầu tiên sau khi quét mã bị sai, nếu mã không hoạt động bình thường, bạn có thể refresh trang quản lý số kiểm tra để xem có không. thay đổi và nhập cái mới nhất nếu có. appsecret.

# Phần kết luận
Tôi nghĩ rằng với tư cách là một nhà phát triển, tôi nên tự mình thực hiện loại công việc đơn giản này. Không cần phải trả tiền cho những nhà phát triển không thể sử dụng các chức năng mà không có sự tài trợ. Tôi nghĩ vì tài trợ được gọi là tính phí., Đó là một hoạt động kinh doanh. Đừng dùng từ tài trợ để trợ cấp cho hành vi tư bản của bạn.
