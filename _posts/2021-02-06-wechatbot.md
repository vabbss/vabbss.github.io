---
bố cục: bài
title: Tạo một chatbot WeChat đơn giản
tags: [WeChat, trò chuyện, rô bốt, PHP]
---
Tôi cảm thấy rằng API có vẻ khá thú vị <! - more ->

# Nguyên nhân
Hai ngày trước, tôi [đã tạo Server Sauce · TurboMini Edition] (/ 2021/02/02 / serverchan.html) và cảm thấy rằng API của tài khoản chính thức WeChat có vẻ khá thú vị. Nói chung, nó không phức tạp lắm. và nó là vô ích. Những điều rất lạ, và tài liệu khá rõ ràng, vẫn còn rất tốt.
Vì vậy, gần đây tôi bắt đầu đọc các tài liệu mở WeChat. Thực tế, sau khi tôi viết xong Phiên bản Server Sauce · TurboMini, tôi đã nghĩ rằng nhiều người có một tính năng trên QQ cho phép robot gửi ảnh miễn là họ nói điều gì đó. . Tôi nghĩ điều này có vẻ hơi thú vị, vì tôi thường sử dụng WeChat hơn. Vì có một nền tảng tốt như tài khoản thử nghiệm, thì tôi nên phát triển một số chức năng như vậy.
Tôi đã dành một ngày để đọc toàn bộ tài liệu và sau đó viết chương trình. Tuy nhiên, một điều khủng khiếp đã xảy ra, đó là thời gian cần thiết cho phản hồi thụ động của WeChat phải trong vòng 5 giây, nếu không sẽ báo lỗi. Tuy nhiên, hãy để máy chủ chuyển tiếp Hình ảnh. Rất mất thời gian và tôi đang sử dụng không gian ảo không có rác ở nước ngoài và kết nối giữa Trung Quốc và Internet rất kém khiến chương trình không thể trả lời trong vòng 5 giây.
Không thể nào, tôi đã dành một ngày để viết một cái gì đó, tôi phải tưới một bài báo! Vì vậy, tôi nghĩ về nó, chỉ cần viết nó là một bot trò chuyện, điều đó rất đơn giản, như Istval trên blog của tôi, tôi sử dụng một bot trò chuyện (với các API làm sẵn, mọi thứ đều dễ dàng thực hiện). Vì vậy, tôi đã thay đổi mã một chút và thay đổi rô bốt chia sẻ hình ảnh thành rô bốt trò chuyện.

# Mã số
```php
<?php
$appid=微信appID;
$secret=微信appsecret;
$appkey=图灵机器人APIkey;
function checkSignature()
{
    $signature = $_GET["signature"];
    $timestamp = $_GET["timestamp"];
    $nonce = $_GET["nonce"];
	
    $token = 'mayx';
    $tmpArr = array($token, $timestamp, $nonce);
    sort($tmpArr, SORT_STRING);
    $tmpStr = implode( $tmpArr );
    $tmpStr = sha1( $tmpStr );
    
    if( $tmpStr == $signature ){
        return true;
    }else{
        return false;
    }
}
if(checkSignature()){
if($_GET["echostr"]){
echo $_GET["echostr"];
}else{
$content = file_get_contents("php://input");
$p = xml_parser_create();
xml_parse_into_struct($p, $content, $vals, $index);
xml_parser_free($p);
if($vals[$index['MSGTYPE'][0]]['value'] == 'text'){
echo '<xml>
  <ToUserName><![CDATA['.$vals[$index['FROMUSERNAME'][0]]['value'].']]></ToUserName>
  <FromUserName><![CDATA['.$vals[$index['TOUSERNAME'][0]]['value'].']]></FromUserName>
  <CreateTime>'.time().'</CreateTime>
  <MsgType><![CDATA[text]]></MsgType>
  <Content><![CDATA['.json_decode(file_get_contents('https://www.tuling123.com/openapi/api', false, stream_context_create(array('http' => array('method' => 'POST','header' => 'Content-type:application/x-www-form-urlencoded','content' => http_build_query(array('key' => $appkey,'info' => $vals[$index['CONTENT'][0]]['value'],'userid' => $vals[$index['FROMUSERNAME'][0]]['value'])))))),true)['text'].']]></Content>
</xml>';
}
}
}else{
echo 'error';
}
```

# Hướng dẫn
Tương tự như [bài viết trước] (/ 2021/02/02 / serverchan.html), bạn cũng cần truy cập [ứng dụng] (https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t = sandbox / login) là mã số thử nghiệm, nhưng có một điểm khác biệt, đó là lần này bạn cần cấu hình thông tin cấu hình giao diện URL là địa chỉ mà chương trình này có thể truy cập trên Internet và Token là mayx. tại sao? Bởi vì tôi đã viết nó như thế này trong đoạn mã ... Nếu bạn muốn thay đổi, bạn có thể thay đổi biến tương ứng thành giá trị yêu thích của bạn, tóm lại, chỉ cần đảm bảo rằng cả hai bên đều giống nhau.
Giao diện được cấu hình sau khi gửi nhưng vẫn chưa kết thúc, để sử dụng robot, bạn phải đăng ký [Turing Robot] (http://www.turingapi.com/), dù sao thì không thể viết được một cuộc trò chuyện của chính bạn Robot, đòi hỏi quá nhiều tài nguyên. Giờ đây, rô bốt Turing dường như phải có tên thật để có thể sử dụng nó, vì vậy sẽ không có vấn đề gì đối với những người hòa nhập với Internet gặp phải loại vấn đề này.
Sau khi đăng ký rô bốt, hãy dán trực tiếp APIKey vào mã và sau đó toàn bộ mã có thể chạy bình thường và bây giờ bạn có thể trò chuyện với rô bốt của mình.

# Mã tạm thời lỗi thời
```php
define('MULTIPART_BOUNDARY', '--------------------------'.microtime(true));

$file_contents = file_get_contents(json_decode(file_get_contents('https://www.pixiv.net/ajax/illust/'.json_decode(file_get_contents('https://api.loli.st/pixiv/'),true)['illust_id'].'/pages'),true)['body'][0]['urls']['regular'], false, stream_context_create(array('http' => array('method' => 'GET','header' => "referer: https://www.pixiv.net/"))));

$context = stream_context_create(array(
    'http' => array(
          'method' => 'POST',
          'header' => 'Content-Type: multipart/form-data; boundary='.MULTIPART_BOUNDARY,
          'content' => "--".MULTIPART_BOUNDARY."\r\n".
            "Content-Disposition: filename=\"image.png\"\r\n".
            "Content-Type: image/png\r\n\r\n".
            $file_contents."\r\n".
            "--".MULTIPART_BOUNDARY."--\r\n"
    )
));

echo '<xml>
  <ToUserName><![CDATA['.$vals[$index['FROMUSERNAME'][0]]['value'].']]></ToUserName>
  <FromUserName><![CDATA['.$vals[$index['TOUSERNAME'][0]]['value'].']]></FromUserName>
  <CreateTime>'.time().'</CreateTime>
  <MsgType><![CDATA[image]]></MsgType>
  <Image>
    <MediaId><![CDATA['.json_decode(file_get_contents('https://api.weixin.qq.com/cgi-bin/media/upload?access_token='.json_decode(file_get_contents('https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid='.$appid.'&secret='.$secret),true)[access_token].'&type=image', false, $context),true)['media_id'].']]></MediaId>
  </Image>
</xml>';
```
Thực ra đoạn mã này không hoạt động, nhưng không đạt yêu cầu, không có cách nào tải hình lên máy chủ WeChat kịp thời, và cũng không có cách nào trả lời kịp thời ... Có thể nếu có điều kiện tốt , mã này có thể chạy ...…
Tôi cũng đã thử rằng nếu nó không phải là hình ảnh trên Pixiv, nhưng hình ảnh trên một máy chủ nhỏ và nhanh, thì mã này có thể chạy.

# Kế hoạch thay thế
Sau khi mình đọc tài liệu thì thấy dường như không có giao diện nào để chủ động gửi thông tin đến người dùng, chỉ bị động, thì vấn đề 5 giây chắc là không thể giải quyết được ... Nhưng mình nghĩ nếu bạn sử dụng giao diện dịch vụ khách hàng thì dường như không có hạn chế như vậy. Dù sao, hãy để tôi thử nó sau.
Ngoài ra, tôi cũng nghĩ đến một số lựa chọn:
1. Tải ảnh lên máy chủ WeChat đều đặn hàng ngày và chỉ gửi ID khi cần thiết, không tải ảnh lên khi được yêu cầu.
2. Thiết lập 2 lệnh, một để tải lên máy chủ WeChat và lệnh kia để truy xuất. Tuy nhiên, vấn đề là ID không dễ truyền và có thể phải cache, thực ra cái trên cũng cần cache.
3. Tạo một định dạng đồ họa và gửi nó đều đặn hàng ngày, giống như một tờ báo hàng ngày

Tôi đã nghĩ về rất nhiều cho đến nay, ngủ nhiều hơn và sau đó suy nghĩ về nó từ từ ~
