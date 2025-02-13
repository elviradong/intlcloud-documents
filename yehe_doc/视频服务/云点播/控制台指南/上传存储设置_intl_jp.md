## シナリオ
VODのアップロードストレージ設定には、カテゴリー管理とストレージリージョンの機能があり、コンソール上でのファイル管理がより便利に行えるようになっています。

## カテゴリー管理の手順
1. [VODコンソール](https://console.cloud.tencent.com/vod/overview)にログインし、左側ナビゲーションバーの【アップロードストレージ設定】>【カテゴリー管理】を選択して、「カテゴリー管理」の画面に入ります。
![](https://main.qcloudimg.com/raw/e9b6b780809c9857bc410c1b6c9a0a02.png)
2. 【カテゴリーの追加】をクリックすると、「カテゴリーの追加」のダイアログボックスがポップアップしますので、カテゴリー名を入力し、【OK】をクリックすれば、追加が完了します。
3. 新しく追加されたカテゴリーは、当該画面のカテゴリーリストの中に表示され、その中で、ターゲットのカテゴリーのレコードに対するリネーム、サブカテゴリーの削除および追加の操作を行うことができます。
	- リネーム：ターゲットのカテゴリー名をクリックすると、名前の右側に編集のアイコンが現れます。このアイコンをクリックすると、カテゴリー名を修正できます。
	- 削除：ターゲットのカテゴリーがある行の【削除】をクリックすると、そのカテゴリーを削除できます。サブカテゴリーが含まれている場合は、先にサブカテゴリーを削除する必要があります。
	- サブカテゴリーの追加：ターゲットのカテゴリーがある行の【サブカテゴリーの追加】をクリックすると、「サブカテゴリーの追加」のダイアログボックスがポップアップします。ここにカテゴリー名を入力し、【OK】をクリックすると、追加が完了します。

>?
>- カテゴリー名に使用できるのは中国語、英語、数字、小括弧です。最大64文字。
>- カテゴリー管理ではまたAPI方式によるファイルのクエリー、変更、削除、関連付けもサポートしています。
>- カテゴリー管理では4つのクラスのカテゴリー構造をサポートしています。このうち、クラス4のカテゴリーにはサブカテゴリーを追加することはできません。
>- カテゴリーを追加した後は、コンソールのメディア資産管理でも、ビデオファイルのカテゴリーの変更が行えます。詳細については、 [ビデオカテゴリーの変更](https://intl.cloud.tencent.com/document/product/266/33893)をご参照ください。
>- カテゴリー管理には、デフォルトで「その他」という名前のカテゴリーが存在しますが、これに対してはリネーム、サブカテゴリーの削除および追加の操作を行うことはできません。カテゴリーが削除された場合に、削除されたカテゴリーに対応するファイルが「その他」カテゴリーに帰属されます。

## ストレージリージョンの手順
1. [VODコンソール](https://console.cloud.tencent.com/vod/overview)にログインし、左側ナビゲーションバーの【アップロードストレージ設定】>【ストレージリージョン】を選択して、「ストレージリージョン」の画面に入ります。
![](https://main.qcloudimg.com/raw/43deae2ce3e366a448daf6fa692b6b80.png)
2. ターゲットのリージョンがある行の状態をオンにするスイッチボタンをクリックし、該当するリージョンをアクティブにします。すでにアクティブになっているリージョンがデフォルトのリージョンとなります。
>?
>- 複数のリージョンを選択してアクティブにすることもできますが、複数あってもデフォルトのリージョンに設定できるのは1つのリージョンのみです。
>- リージョンをアクティブ化する時は、新しいリージョンでの設定が完了するまで、当該リージョンに対する操作を行うことができません。リージョンをアクティブ化した後、5 - 10分経ってからこの設定が有効になり、かつ**一度リージョンを有効化すると無効にすることはできません**。
>- 中国大陸リージョン（北京、天津、重慶、上海など）をアクティブにしたい場合は、[チケットを提出](https://console.cloud.tencent.com/workorder/category)してください。

#### 保存ルール
- その他のリージョンをアクティブにしない場合、すべてのファイルはデフォルトでデフォルトのリージョンに転送されます。
- その他のリージョンをアクティブ化している場合：
	- IPの帰属地**がアクティブ化しているリージョンの近くのアップロード保存範囲内**であるときは、IPに応じて、ファイルを近くでアクティブ化しているリージョンにアップロードします。
	- IPの帰属地が**アクティブ化しているリージョンの近くのアップロード保存範囲にない**ときは、VODがファイルをデフォルトのリージョンにアップロードします。

より詳しいストレージリージョンの内容については、[アップロード品質最適化の実践](https://intl.cloud.tencent.com/document/product/266/33904)をご参照ください。



