---
title: 上傳圖片到 AWS S3，並允許公開讀取
date: 2023-04-26 11:37:51
tags:
- AWS
- Laravel
- Storage
- S3
category:
- 學習筆記
index_img: img/uploadToS3.jpeg
banner_img: img/uploadToS3.jpeg
---

# S3 允許公開讀取

{% note warning%}
> [封鎖對 Amazon S3 儲存體的公開存取權](https://docs.aws.amazon.com/zh_tw/AmazonS3/latest/userguide/access-control-block-public-access.html)

由於 AWS S3 預設建議停用 S3 存取控制清單 (ACL），所以新建立的儲存貯體，只能透過「儲存貯體政策」去設定讀取權限。

也就是如果你新創的「儲存貯體」如果沒有啟用 ACL （已經建好的儲存貯體也沒辦法再啟用 ACL 了）， 那 laravel storePubliclyAs() 就等同於沒用
{% endnote %}

## 停用 ACL 的做法 
![](https://i.imgur.com/oMPgEza.png)

1. 停用「帳戶」所有「封鎖公開存取」
![](https://i.imgur.com/L3K2jQ7.png)
2. 停用「儲存貯體」的所有「封鎖公開存取」
![](https://i.imgur.com/ZvZBYmO.png)

3. 設定「儲存貯體政策」
```json=1
{
    "Version": "2012-10-17",
    "Id": "Policy1680494149129",
    "Statement": [
        {
            "Sid": "PublicRead",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::sojtest1/*"
        }
    ]
}
```
4. laravel 程式碼
```php=
Route::post('upload', function (\Illuminate\Http\Request $request) {
    $image = $request->file('image');
    $extensionName = $image->getClientOriginalExtension();
    $fileName = now()->timestamp . ".$extensionName";
    return $image->storeAs('soj/test', $fileName);
});
```


## 啟用 ACL 的做法

{% note info %}
請先確認 ACL 是否啟用
![](https://i.imgur.com/QHf8MxO.png)
{% endnote %}

1. 停用「帳戶」所有「封鎖公開存取」
![](https://i.imgur.com/L3K2jQ7.png)
2. 停用「儲存貯體」的所有「封鎖公開存取」
![](https://i.imgur.com/ZvZBYmO.png)
3. 不用設定「儲存貯體政策」，程式碼使用 `storePubliclyAs()` 就可以
```php=
Route::post('upload', function (\Illuminate\Http\Request $request) {
    $image = $request->file('image');
    $extensionName = $image->getClientOriginalExtension();
    $fileName = now()->timestamp . ".$extensionName";
    return $image->storePubliclyAs('soj/test', $fileName);
});
```