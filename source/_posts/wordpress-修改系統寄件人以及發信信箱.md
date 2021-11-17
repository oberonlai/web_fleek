---
title: WordPress 修改系統寄件人以及發信信箱
tags: []
id: '68'
categories:
  - - WordPress
date: 2019-02-02 13:41:30
---

在預設情況下，發送系統信的寄件人名稱會是 WordPress 以及 wordpress@domain.com，在 function.php 加入以下兩個 hook 來進行修改

```
// 修改發信電子郵件
function wpb_sender_email( $original_email_address ) {
  return ‘service@domain.com';
}
// 修改寄件人名稱
function wpb_sender_name( $original_email_from ) {
  return ‘Hello~';
}
add_filter( 'wp_mail_from', 'wpb_sender_email' );
add_filter( 'wp_mail_from_name', 'wpb_sender_name' );
```