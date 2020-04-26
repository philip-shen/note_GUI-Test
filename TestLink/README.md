# Purpose  
Take note of TestLink  

# Table of Contents  

# 

[テスト工程の管理をするツール、TestLinkについて ](https://qiita.com/mima_ita/items/ed56fb1da1e340d397b9)  
## TestLinkの環境作成  
[TestLinkの環境作成](https://qiita.com/mima_ita/items/ed56fb1da1e340d397b9#testlink%E3%81%AE%E7%92%B0%E5%A2%83%E4%BD%9C%E6%88%90)  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F47856%2Fae024b25-aa46-392e-6925-53436ccb5198.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=5d4dc6732d344a9edd447c3d956a8f16)  


[TestLink で LDAP 認証を行う|へっぽこプログラマーの備忘録 2017/10/24](http://kuttsun.blogspot.com/2017/10/testlink-ldap.html)  
[RedmineとTestlinkの連携 03/25 2011](http://in.shappi.org/article/192485671.html)  
[TestLinkの使い方メモ　テストプロジェクトとテスト項目の作成 2010/06/19(土)](https://symfoware.blog.fc2.com/blog-entry-443.html)  
[脱Excel！ Redmineでアジャイル開発を楽々管理 (1/5) 2009/04/14](https://www.atmarkit.co.jp/ait/articles/0904/14/news117.html)  
[脱Excel！ TestLinkでアジャイルにテストをする (2/6) 2009/10/23](https://www.atmarkit.co.jp/ait/articles/0910/23/news110_2.html)  

[オープンソースのテスト管理ツール TestLinkをさくらのレンタルサーバーに設置する 2011/12/2](https://nantekottai.com/2011/12/02/testlink-the-open-source-test-managament-tool/)  
[TestLink1.9.10へのバージョンアップ時の調査 Jul 10, 2014](https://qiita.com/mima_ita/items/f2837a91c9492a2b9e72)  
[【2019年版】macOS 10.14 Mojaveの docker で TestLink をインストールと設定する Nov 19, 2019](https://qiita.com/shimizumasaru/items/a1de689866c3ee2a4de5)  
[【2019年版】Docker Bitnami/TestLink を設定する Nov 19, 2019](https://qiita.com/shimizumasaru/items/6c7b3f55a2dd63d5252b)  
## 9. 参考：最終的な custom_config.inc.php の設定例  
[9. 参考：最終的な custom_config.inc.php の設定例](https://qiita.com/shimizumasaru/items/6c7b3f55a2dd63d5252b#9-%E5%8F%82%E8%80%83%E6%9C%80%E7%B5%82%E7%9A%84%E3%81%AA-custom_configincphp-%E3%81%AE%E8%A8%AD%E5%AE%9A%E4%BE%8B)

```
<?php

// 参考資料： TestLink インストールマニュアルや、各種設定ドキュメントは、
// /opt/bitnami/testlink/docs 配下に存在する

// path設定
$tlCfg->log_path = '/opt/bitnami/testlink/logs/';
$g_repositoryPath = '/opt/bitnami/testlink/upload_area/';

// TestLinkセキュリティ上の弱点が存在する場合に警告する方法は、
//  * 'SCREEN'：ログイン画面にメッセージを表示する（デフォルト）
//  * 'FILE'：警告ファイルを作成、ログイン画面にメッセージを表示する。
//  * 'SILENT'：警告ファイルを作成、ログイン画面にメッセージを表示しない。
$tlCfg->config_check_warning_mode = 'SCREEN';

// smtp設定
$g_smtp_host = 'smtp.gmail.com';
$g_tl_admin_email = 'mymailaddress@gmail.com';
$g_from_email = 'mymailaddress@gmail.com';
$g_return_path_email = 'mymailaddress@gmail.com';
$g_smtp_username = 'mymailaddress@gmail.com';
$g_smtp_password = 'mymailpassword12345678';
$g_smtp_port = '587';
$g_smtp_connection_mode = 'tls';

// デフォルト言語を日本語にする
$tlCfg->default_language = 'ja_JP';

// レポートで表示される TestLink ロゴを自社ロゴに変更する
$tlCfg->document_generator->company_logo = 'logo.png';

// ロゴマークの高さを指定する
$tlCfg->document_generator->company_logo_height = '50';

// グラフのレポートのシステムフォントを IPA P ゴシック(Ver.003.03)に設定する
$tlCfg->charts_font_path = "/usr/share/fonts/truetype/ipagp00303/ipagp.ttf";

// TestLink にアップロードするファイルサイズ制限を拡大する。ただし(多くの場合のPHPデフォルト値の)２MBを超える場合は、同時に php.ini の upload_max_filesize の設定も変更すること
$tlCfg->import_max_size = '100000000'; // in bytes
$tlCfg->repository_max_filesize = 100; //MB

// レポートとメトリクスで表示される会社名を設定する
$tlCfg->document_generator->company_name = '自社名';
$tlCfg->document_generator->company_copyright = 'Copyright© ' . date('Y') . '自社名';
$tlCfg->document_generator->confidential_msg = '本資料は、自社名の許可無く対外的に参照・配布しないようお願い申し上げます。';

// 実行済みのテストケースの編集を可能にする
$tlCfg->testcase_cfg->can_edit_executed = ENABLED;

// 実行済みのテストケースの削除不可にする(初期値：削除可能)
$tlCfg->testcase_cfg->can_remove_executed = DISABLED;

// ユーザをテストケースにアサインしなくても、テスト実行およびレポートを出力する
$tlCfg->exec_cfg->exec_mode->tester='all';

// テスト実行時にアサインに関わらず、テストケースを全て表示する
$tlCfg->exec_cfg->user_filter_default='none';

// テストの実行で、実行履歴を表示する
$tlCfg->exec_cfg->history_on = TRUE;

// sessionInactivityTimeoutを延長する
$tlCfg->sessionInactivityTimeout = 86400;

// 管理者による新規アカウント登録に限定する
$tlCfg->user_self_signup = FALSE;

?>
```

# Troubleshooting


# Reference


* []()
![alt tag]()  

# h1 size

## h2 size

### h3 size

#### h4 size

##### h5 size

*strong*strong  
**strong**strong  

> quote  
> quote

- [ ] checklist1
- [x] checklist2

* 1
* 2
* 3

- 1
- 2
- 3
