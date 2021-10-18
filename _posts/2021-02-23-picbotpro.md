---
bố cục: bài
title: Nhanh chóng làm một robot hình ảnh WeChat
các thẻ: [WeChat, hình ảnh, Pixiv, rô bốt, PHP]
---

Tối ưu hóa thực sự phức tạp ... <! - more ->

# Nguyên nhân
Cách đây một thời gian, mình có làm [Robot gửi ảnh] (/ 2021/02/19 / picbot.html), sau khi làm xong mình sẽ đưa cho nhóm bạn cùng trải nghiệm, nhưng tiếc là code thực sự không có. Tốt. Trước hết, có hai vấn đề. Thứ nhất là số lần WeChat có thể lấy `access_token` bị giới hạn. Phiên bản mã đầu tiên của tôi được sử dụng để lấy` access_token` mỗi khi nó được gọi, vì vậy số lần sẽ hết sớm, và sau đó tôi đã cải thiện nó một chút. Sau một thời gian, tôi thiết lập bộ nhớ cache, kết quả là tôi sử dụng ít giao diện hơn khi kiểm tra ... Nó quá ngu ngốc ... Tiếp theo là gì? Do đó, tôi thấy rằng có hai nơi để lấy `access_token` trong mã ngày hôm nay và bộ nhớ cache hoàn toàn vô dụng ...
Nói tóm lại, các vấn đề ở trên cuối cùng cũng được giải quyết, và có một vấn đề khác là nguồn ảnh của tôi là [API Lolicon] (https://api.lolicon.app/setu), và Giới hạn cuộc gọi là 300 lần / ngày, Thành thật mà nói, số này chỉ đủ cho một người, nhưng nếu có nhiều người, như số thử nghiệm có thể chứa tối đa 100 người, thì mỗi người sẽ chỉ có 3 cơ hội gọi mỗi ngày .
Làm thế nào để giải quyết vấn đề về số lượng cuộc gọi? Điều đầu tiên tôi nghĩ đến là lưu kết quả vào bộ nhớ cache.

# Giải quyết vấn đề quá ít lệnh gọi API
Bởi vì về cơ bản không có thông tin thay đổi cho hình ảnh, thực sự không có vấn đề gì nếu kết quả của mỗi lần có thể được lưu vào bộ nhớ cache. Vậy là cứ làm đi, mình mở một kho riêng [pixiv-index] (https://github.com/Mabbs/pixiv-index) để lưu kết quả đã cache, mã cụ thể là ở kho này, mỗi ngày gọi API đó cho đến khi số lần được sử dụng hết.
Xem xét rằng hình ảnh gốc không cần thiết trong hầu hết các trường hợp, hình ảnh trong API này chỉ là hình thu nhỏ với chiều dài hoặc chiều rộng tối đa là 1200px.
Cách sử dụng cũng rất đơn giản, giống như PHP, bạn có thể viết nó như sau:
```php
<?php
$raw=json_decode(file_get_contents("https://mabbs.github.io/pixiv-index/index.json"),true);
echo file_get_contents('https://mabbs.github.io/pixiv-index/data/'.$raw[rand(0,count($raw)-1)]);
```
Mặc dù vấn đề đã được giải quyết nhưng tôi đã tìm thấy một lỗ hổng rất lớn. Ý định ban đầu khi thiết kế tập lệnh này là nghĩ rằng nó có rất nhiều dữ liệu để tôi gọi, nhưng tôi thấy rằng mình đã nhầm. Tôi đã không đọc kỹ tài liệu của họ. trước đây. Sau đó, tôi phát hiện ra rằng những bức ảnh tôi muốn chỉ có 3361 bức, quá ít và tổng số bức ảnh chỉ có 17.285 (ngay cả khi dữ liệu của trạm đó đang tăng với tốc độ rất chậm) ...
Tôi chỉ không bận tâm tìm kiếm nó ở nơi khác, và bởi vì tác giả API nói rằng những bức ảnh đã được anh ấy lựa chọn cẩn thận, tôi đã viết kịch bản của kho đặc biệt, và cũng đã tìm hiểu về Github Action ... ~~ (Mặc dù nó thực sự là Cái đã sao chép [cái để gia hạn tài khoản của nhà phát triển] (https://github.com/wangziyingwen/AutoApiSecret) kho đó lol) ~~

# Mã mới
Sau khi giải quyết những vấn đề đó, tôi đã tối ưu hóa nó một chút và loại bỏ chức năng của chatbot để không làm cho API của rô bốt Turing trở nên khó hiểu.
```php
<?php
$appid='微信appID';
$secret='微信appsecret';
$token='和配置的Token配置一致即可';
$source='https://i.pximg.net';

ini_set('session.gc_maxlifetime', 7200);
ignore_user_abort(true);
set_time_limit(0);
session_id('Storage');
session_start();
if(!json_decode(file_get_contents('https://api.weixin.qq.com/cgi-bin/get_api_domain_ip?access_token='.$_SESSION['access_token']),true)['ip_list']){
$_SESSION['access_token']=json_decode(file_get_contents('https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid='.$appid.'&secret='.$secret),true)['access_token'];
}
if($_GET["upap"]){
define('MULTIPART_BOUNDARY', '--------------------------'.microtime(true));

$retry=3;
while(!$picdata||$retry<=0){
$raw=json_decode(file_get_contents("https://mabbs.github.io/pixiv-index/index.json"),true);
$picdata=file_get_contents($source.json_decode(file_get_contents('https://mabbs.github.io/pixiv-index/data/'.$raw[rand(0,count($raw)-1)]),true)['url'], false, stream_context_create(array('http' => array('method' => 'GET','header' => "referer: https://www.pixiv.net/"))));
$retry-=1;
}

$context = stream_context_create(array(
    'http' => array(
          'method' => 'POST',
          'header' => 'Content-Type: multipart/form-data; boundary='.MULTIPART_BOUNDARY,
          'content' => "--".MULTIPART_BOUNDARY."\r\n".
            "Content-Disposition: filename=\"image.jpg\"\r\n".
            "Content-Type: image/jpg\r\n\r\n".
            $picdata."\r\n".
            "--".MULTIPART_BOUNDARY."--\r\n"
    )
));

file_get_contents('https://api.weixin.qq.com/cgi-bin/message/custom/send?access_token='.$_SESSION['access_token'] , false, stream_context_create(array('http' => array('method' => 'POST','header' => 'Content-type: application/json;charset=utf-8','content' => '{
    "touser":"'.$_GET["openid"].'",
    "msgtype":"image",
    "image":
    {
      "media_id":"'.json_decode(file_get_contents('https://api.weixin.qq.com/cgi-bin/media/upload?access_token='.$_SESSION['access_token'].'&type=image', false, $context),true)['media_id'].'"
    }
}'))));
exit();
}

$timestamp=$_GET["timestamp"];
$nonce=$_GET["nonce"];
$tmpArr=array($token, $timestamp, $nonce);
sort($tmpArr, SORT_STRING);
if( sha1(implode($tmpArr)) == $_GET["signature"] ){
if($_GET["echostr"]){
echo $_GET["echostr"];
}else{
 
//  加载XML内容
$content = file_get_contents("php://input");
$p = xml_parser_create();
xml_parse_into_struct($p, $content, $vals, $index);
xml_parser_free($p);
if($vals[$index['MSGTYPE'][0]]['value'] == 'text'){
if($vals[$index['CONTENT'][0]]['value'] == '来点色图'){

echo '<xml>
  <ToUserName><![CDATA['.$vals[$index['FROMUSERNAME'][0]]['value'].']]></ToUserName>
  <FromUserName><![CDATA['.$vals[$index['TOUSERNAME'][0]]['value'].']]></FromUserName>
  <CreateTime>'.time().'</CreateTime>
  <MsgType><![CDATA[text]]></MsgType>
  <Content><![CDATA[开始发起请求，请耐心等待]]></Content>
</xml>';

file_get_contents('https://'.$_SERVER['HTTP_HOST'].$_SERVER['PHP_SELF'].'?upap=1&openid='.$vals[$index['FROMUSERNAME'][0]]['value'], false, stream_context_create(array('http' => array('timeout' => 0.5))));

}else{
echo 'success';
}
}
}
}else{
echo 'error';
}
```
Cập nhật 2021.02.26: Có vẻ như một số hình ảnh trong thư viện đã bị xóa, vì vậy để cải thiện tỷ lệ trả lời thành công, 3 lần thử lại đã được thêm vào.

# cách sử dụng?
Tôi không cần phải nói chi tiết, chỉ cần đọc đoạn văn trong các bài viết trước về robot WeChat. Ở đây mình đã xóa 2 thông số và thêm 2 thông số nữa, một là Token, bạn có thể điền bất cứ thứ gì mình muốn, miễn là cấu hình trong số kiểm tra giống nhau. Cái còn lại là nguồn, đó là máy chủ hình ảnh của Pixiv, nếu máy chủ back-end ở nước ngoài thì bạn không phải lo lắng về điều đó, nếu bạn ở Trung Quốc, bạn cần đổi nó thành `https: // i. pixiv.cat` để chống tạo hoặc nếu Có các dịch vụ chống tạo khác và không có vấn đề gì khi tạo một dịch vụ với CloudFlare Worker.

# kết thúc
API Lolicon thực sự không dễ sử dụng, nhưng mình lười giải quyết quá nên đã nhờ em trai mình làm công việc thu thập danh sách hàng ngày của Pixiv, cùng xem lại xem hiệu quả như thế nào, nếu không được thì thôi. , Tôi sẽ đi và nghiên cứu tất cả các loại booru. Đó là một loại trạm hình ảnh và sử dụng những hình ảnh đó cũng là một lựa chọn tốt.
