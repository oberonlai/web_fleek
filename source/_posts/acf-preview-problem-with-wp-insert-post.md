---
title: ACF Preview Problem with wp_insert_post()
tags: []
id: '539'
categories:
  - - PHP
date: 2020-01-08 21:28:12
---

![](https://oberonlai.blog/wp-content/uploads/2020/01/CleanShot-2020-01-08-at-21.10.57.jpg)

狀況是這樣：寫了 ajax 前台發文並能切換文章狀態，當 wp\_insert\_post() 發文後，如果是使用草稿狀態的話 preview link 會無法正常顯示內容，但切換成發佈狀態是可以正常顯示，而且 acf 的 欄位都有進去，但如果又切換成草稿，則會不顯示任何內容，

必須要去後台點選預覽按鈕或是儲存草稿後，preview link 才能正常顯示，插入文章的寫法如下：

```
/**
  * 建立新文章
  */
if($is_new_post === 'new'){
  $author_id = $_POST['author_id'];
  $my_post = array(
    'post_title'    => $post_title,
    'post_name'   => $post_title,
    'post_status'   => $post_status, // 取得 select 值
    'post_author'   => $author_id,
    'post_category' => $post_category
  );
  $new_post_id = wp_insert_post( $my_post );
  $post_id = $new_post_id;
  update_field('storage',$storage_field,$new_post_id);
  wp_save_post_revision($new_post_id);
}
```

然後預覽按鈕的 URL 是這樣組：

```
document.querySelector('#btnPreview a').href = `${insert_params.siteurl}?p=${jsonData.postId}&preview=true`
```

一直以為是不開給作者進 wp-admin 的權限所導致的，或是 Gutenberg 造成的錯誤，爬了文查到這是 wp\_insert\_post 的機制造成的 -> [https://support.advancedcustomfields.com/forums/topic/saved-drafts-not-displaying-custom-fields/page/2/](https://support.advancedcustomfields.com/forums/topic/saved-drafts-not-displaying-custom-fields/page/2/)

> Could not figure out what was causing it but when i remove preview=true from the uri it works again.

嘗試了在預覽網址後面不要帶 preview 的參數，結果可以順利預覽了。