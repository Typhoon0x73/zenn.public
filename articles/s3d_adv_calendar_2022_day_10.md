---
title: "Siv3D用リソース埋め込み補助ツールを作った"
emoji: "🧰"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["siv3d", "cpp", "tool"]
published: true
published_at: 2022-12-10 00:00
---  
  
# はじめに  
  
初めまして🤟  
私はしがない一般モバイルゲームエンジニアです。  
この記事はSiv3Dのアドベントカレンダーの10日目の記事です。  
余裕をぶちかましてゲームしてたので、  
時間が無くなりハードコーディング多めです。ご了承ください。。  
  
私がツールを作るときに考えていることと、ツールの内容についての記事です。  

ツールのリポジトリはこちら  
https://github.com/Typhoon0x73/s3d_built-in_resource_supporter  

ツールダウンロードはこちら  
https://github.com/Typhoon0x73/s3d_built-in_resource_supporter/releases/tag/alpha_2022_12_09  
  
# きっかけ  
開発が楽になるように環境改善していきたいと考えたのが始まりでした。  
最初はコンソール上で動くものを作っていたのですが、  
コマンドを打つ手間を考えると手動で埋め込み操作したほうが楽なのでは、、、  
となったのでGUI作ればええやんってことで作りました。
  
# OpenSiv3Dを選んだ理由  
昨今のGUIライブラリは非常に潤沢です。(いい時代ですねぇ)  
私が迷ったのは以下の項目ですが、まだまだほかにもたくさんあります。  
・自作フレームワークにのっけたImGuiを使う  
・Qtを使う  
・OpenSiv3Dを使う  
・C#を勉強する  
・Pythonを勉強する  
これらで迷っていた時、OpenSiv3D ver 0.6.6 より、  
GUI周りのアップデートが入るとのことと、  
OpenSiv3D用のツールだしOpenSiv3Dを使うかという流れで選びました。
  
# ツールを作るときに考えていること
まず、大前提として、ツールを使う人がいます。  
開発者自身もそうですし、公開した場合には不特定多数のユーザーが出現します。  
これについてはゲームを造る上でも共通して言えますね。  
使うのは人間ということで人間が操作に迷わないような設計を心がけます。

## デメリットを超えるメリットの供給  
自作ツールを使ってもらう以上、使い方を覚えるという作業(デメリット)が発生します。  
ですので、それ以上のメリットを供給しないとツールとして不必要です。  
開発環境の改善って難しい、、  
(今回作ったこのツールはまだゴミ段階です)  

# Siv3D用リソース埋め込み補助ツールの紹介  
## このツールができること  
Siv3DのWindowsプロジェクトでは、  
リソースを埋め込むために`Resource.rc`とプロジェクトへ追加、追記する必要があります。  
詳細は、[[Siv3Dリソースの埋め込み方法](https://zenn.dev/reputeless/books/siv3d-documentation/viewer/tutorial-resource)]こちらをご覧ください。  
これをGUIで操作し追加できるようにします。  

## 開発環境  
opensiv3d ver 0.6.6  
Windows 10 pro  
MicrosoftVisualStudio 2022 v17.4

## 機能紹介  
・埋め込むリソースの追加  
・埋め込んだリソースの削除  
・エンジン任意のリソースON/OFF  
  
# 使い方  
## 埋め込むリソースの追加  
  
起動したら、リソース埋め込みを行いたいプロジェクトファイル(*.vcxproj)を開きます。  
![open_project](/images/s3d_built-in_supporter_gif/open_project.png)  
プロジェクトが開けたら、`User`タブを選択している状態で  
`edit`メニューの`regist resource`か、  
`regist resource`ボタンを押下します。  
追加したいリソースがあるフォルダまで移動し、リソースを選択しましょう。  
![how_to_use](/images/s3d_built-in_supporter_gif/sample_0.gif)
サンプルでの`PNG`となっているのがタグとなり、  
`Resource Files\example`はフィルタの項目となります。  
`file`メニューからsaveを選択することで`Resource.rc`に追記され、プロジェクトにも追加されます。  

## 埋め込んだリソースの削除  
追加できるのであれば最低でも削除の機能が欲しいですよね。  
(タグの削除方法は手動で`Resource.rc`を触る他ありません...)  
  
リソースの削除はタグを選択し、削除したいリソースを選択すると、  
`regist resource`ボタンの右に🗑️ボタンが出現し、  
押下することで削除できます。  
(`edit`メニューの`erase resource`からでも削除できます。)  
![how_to_use](/images/s3d_built-in_supporter_gif/sample_1.gif)  
ちなみに、`undo` `redo` も存在します。  
Ctrl+Zで元に戻す、  
Ctrl+Yでやり直しのショートカットも仕込んでます。😎  
ここの仕組みはツールの醍醐味ではありますが、それだけで記事書けちゃいそうなのでまた次回。  

## エンジン任意のリソースON/OFF  
`Engine`タブからエンジンに組み込まれているリソースの選択ができます。  
どれをON/OFFすればいいかのガイドラインは[こちら](https://zenn.dev/reputeless/articles/article-minimum)で、  
`help`メニューの`engine resource built-in cancel guidline`からも飛べます。  
![how_to_use](/images/s3d_built-in_supporter_gif/sample_2.gif)  
OFFにして`save`すると、   
`Resource.rc`でコメントアウト化され、埋め込みから省かれます。  
元に戻す時はONの状態で`save`すると、コメントインされて再度埋め込まれます。  
(Shiftを押しながらスクロールすると水平スクロールが可能です。)  

ソースコードではフィルタやタグの追加、移動、削除などの機能がありますが、  
GUI上での実装が間に合わず、、、  
今後私の頑張りで追加されていくはずです。  

# 余談  
C++26以降で、プリプロセッサディレクティブに#embedが追加される？提案[[P1967R9](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p1967r9.html)]があるようです。  
埋め込みリソースの形が変わってくるのか変わらないのか、、  
それまでに血反吐をはいて柔軟に対応できるよう改善、リファクタしなければ、、  
  
# 終わりに  
今後も保守していくために一度リファクタリングを行います。  
私は不器用で設計してもうまくいかないので、  
作って壊して作り直すを繰り返し、保守できる形を目指します。  
この記事、ソース、ツールが誰かの参考になれば幸いです。  
(ツールを使っていただけると尚ハッピーです)  

💪💪💪来年もSiv3Dとともに成長し発展していくぞ～！！💪💪💪  