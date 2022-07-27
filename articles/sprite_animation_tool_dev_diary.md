---
title: "スプライトアニメーション作成ツールを制作中"
emoji: "📘"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [gamedev, cpp, tools, OpenSiv3D, animation]
published: true
---
ゲーム制作でぜひ使われたいツールを恥ずかしながらも公開。
https://github.com/Typhoon0x73/sample-opengl-for-imgui/releases
現在　アルファ版

# スプライトアニメーション作成ツール
![sample](/images/sprite_animation_tools_gif/sample_00.gif)  

使用させていただいた画像アセット
https://szadiart.itch.io/2d-soulslike-character

## 開発環境  
OS : windows 10 pro  
IDE : visual studio 2019, 2022  
開発言語 : c++17  
グラフィックライブラリ : OpenGL + imgui  
自分のPCでしか動かしていないのでもしかしたら動かない可能性も...  
もしどうしても動かしたいけど動かない方は連絡ください。できる限り対応します。  

## 動機  
OpenSiv3D実装会参加してみたいけど、なにすればいいかわからない...  
せや！昔作ったアニメーションツールをOpenSiv3D向けにリワークしよけ！  

ということです。  

# 使い方  
現在、アルファ版ということで、全部手打ちです。(シェアしてモチベ上げ優先した)  
詳しい使い方は **ベータ or マスタアップ後** にドキュメント書きます。  
ここからはOpenSiv3Dで導入する方法を書きます。  

## ダウンロード  
上記のgithubリンクから`animake.zip`をダウンロードします。
![how_to_use](/images/sprite_animation_tools_gif/how_to_use_00.png)  

ダウンロードした解凍してフォルダを開きます。  
解凍したフォルダ内にある`animake.exe`がツール本体です。
`animake.exe` ・・・ ツール本体。
`Resources/` ・・・ 素材、サンプルなどのデータが入ってます。
`src/` ・・・ スプライトアニメーションのソースコード入ってます。
:::message alert
※ResourcesフォルダのSprite.fragとSprite.vertを消すと何も描画されなくなります
:::

## データを作る  
簡単にアニメーションを作る手順を紹介。  
### 1. `animake.exe`の起動。  
![](/images/sprite_animation_tools_gif/sample_01.gif)  

### 2. 画像の追加
texturesウィンドウのaddボタンを押下します。
起動した`animake.exe`と同じフォルダの  
`Resources/2D_SL_Knight_v1.0/Idle.png`と`Run.png`を開きます。
(別々にaddボタンを押して開いてもOK)
![](/images/sprite_animation_tools_gif/sample_02.gif)

### 3. パターンの追加  
editorウィンドウのcreateボタンを押下します。  
今回は、複製して作っていくので**パターン数は 1 で作成**します。  
editorウィンドウの各パラメータを設定。  
**texture:2D_SL_Knight_v1.0\Idle.png**,
**width:128**, **height:64**, **refresh:0.08**
![](/images/sprite_animation_tools_gif/sample_03.gif)  

### 4. パターンの複製  
patternsウィンドウのduplicateボタンを押下します。
複製されたパターンを選択し、オフセットをずらして設定していきます。  
has loopedチェックを入れるとアニメーションがループ設定になります。
:::message  
この時点ではまだviewにでません  
:::  
![](/images/sprite_animation_tools_gif/sample_04.gif)　　
複製して設定を下記のパラメータに設定します。  
|pattern N | offset x | offset y |  
|:---------|:---------|:---------|  
|0|0|0|  
|1|128|0|  
|2|0|64|  
|3|128|64|  
|4|0|128|  
|5|128|128|  
|6|0|192|  
|7|128|192|  

### 5. アニメーション名の変更  
animationsウィンドウのanimation nameを`animation_0`から`idle`に編集します。  
変更した名前を下のリストから選択すると animation viewウィンドウに表示されます。

![](/images/sprite_animation_tools_gif/sample_05.gif)  

### 6. アニメーションの追加  
animationsウィンドウのduplicateボタンを押下します。
animaiton nameを`idle_copy`から`run`に編集します。
![](/images/sprite_animation_tools_gif/sample_06.gif)  

patternsウィンドウの各パターンを選択し、  
editorウィンドウのtextureを  
**2D_SL_Knight_v1.0\Idle.png**から**2D_SL_Knight_v1.0\Run.png**へ  
すべて変更します。手動です。~~操作感最悪です←~~  
![](/images/sprite_animation_tools_gif/sample_07.gif)  

### 7. ファイルの保存  
fileメニューから`save as`を押下します。  
現在`save`も機能しますが、何のリアクションも起こさないので  
不安であれば`save as`を使用してください。  
保存場所は任意ですが、使用した画像付近か  
.spaを保存するフォルダをResources/に作ると良いと思います。  
説明では画像と同フォルダに保存します。(名前はplayer.spaとしました)  
![](/images/sprite_animation_tools_gif/sample_08.gif)  

ここまででデータの作成が完了しました。  
fileメニューのopenからいつでも編集ができます。  

## OpenSiv3Dに読み込ませる  
OpenSiv3Dについてはこちらを...  
https://siv3d.github.io/ja-jp/  

簡単に説明すると、簡単にC++でゲーム作れるライブラリです。  
ドキュメントが整備されていてリファレンスも見やすいので好きです。  
animakeの対応バージョンはv0.6.4です。  
常に最新に対応するよう努力します。  

### プロジェクトの準備  
さてプロジェクトを準備します。  
プロジェクトの作成方法は公式ドキュメントをご覧ください。
https://siv3d.github.io/ja-jp/download/windows/  

animake本体はWindowsオンリーですが、  
読み込む部分はリトルエンディアンであれば対応しています。  
(ビッグエンディアンについては今後対応したいと考えています...)  

ダウンロードしたanimakeフォルダの
`src/`から３ファイル全てをOpenSiv3Dプロジェクトに追加します。  
先ほど作ったファイルと対応する素材もOpenSiv3Dプロジェクトフォルダに移動します。  
追加後のOpenSiv3Dプロジェクトのフォルダ配置は下記のようになります。  
```
OpenSiv3D_project/  
|--App/  
|  |--Resources/2D_SL_Knight_v1.0/  
|     |--Idle.png   
|     |--Run.png  
|     `--player.spa  
|--SpriteAnimation.cpp  
|--SpriteAnimation.h
`--SpriteAnimationCommon.h   
```  

### OpenSiv3D側のコード
```cpp  
# include <Siv3D.hpp> // OpenSiv3D v0.6.4

// スプライトアニメーション関連のクラスをまとめたヘッダ
# include "SpriteAnimation.h"

void Main()
{
	// リソースフォルダにパス変更
	FileSystem::ChangeCurrentDirectory(U"Resources");

	// 画像配列
	std::vector<Texture> textures;

	// ファイルパス配列
	std::vector<std::string> texture_file_paths;

	// スプライトアニメーション制御インスタンスファイルを指定して読み込む
	spa::SpriteAnimationController sprite_animation("2D_SL_Knight_v1.0/player.spa", &texture_file_paths);

	// 読み込んだファイルパス配列から画像を読み込む
	for (const auto& texture_file_path : texture_file_paths)
	{
		textures.push_back(Texture(Unicode::FromUTF8(texture_file_path)));
	}
	// 再生するアニメーションを数字指定
	sprite_animation.changeAnimation(0, false);

	// ゲームループ
	while (System::Update())
	{
		// 1キー押下でidleアニメーションに切り替え
		if (Key1.down())
		{
			sprite_animation.changeAnimation("idle");
		}
		// 2キー押下でrunアニメーションに切り替え
		if (Key2.down())
		{
			sprite_animation.changeAnimation("run");
		}

		// スプライトアニメーションの更新
		/**/
		sprite_animation.update(Scene::DeltaTime());
		/*/
		sprite_animation.update(Scene::DeltaTime() * 2.0); // アニメーション速度変更可能
		/**/

		// 現在再生しているパターン情報を取得
		const auto& pattern = sprite_animation.currentPattern();
		if (pattern)
		{
			// レイヤーの数だけデータを回す
			for (const auto& layer : pattern->m_LayerArray)
			{
				// 画像番号を取得して読み込んだ画像番号が存在するかチェック
				const auto& imageNo = layer.second.m_ImageNo;
				if (imageNo < 0 || static_cast<std::size_t>(imageNo) >= textures.size())
				{
					continue;
				}
				// 各パラメータ
				const auto& offsetX     = layer.second.m_OffsetX;
				const auto& offsetY     = layer.second.m_OffsetY;
				const auto& width       = layer.second.m_Width;
				const auto& height      = layer.second.m_Height;
				const auto& drawOffsetX = layer.second.m_DrawOffsetX;
				const auto& drawOffsetY = layer.second.m_DrawOffsetY;
				// 画像切り抜き用矩形
				const Rect srec{ offsetX, offsetY, width, height };
				// 画像番号から選択し、指定矩形で切り抜き描画
				textures[imageNo](srec).draw(drawOffsetX, drawOffsetY);
			}
		}
	}
}

```
ちゃんと描画されていれば成功です！
![](/images/sprite_animation_tools_gif/sample_09.gif)  

## 余談
.spaファイルの中身はただのバイナリです。
Common.hにデータ詳細書いているので独自でデータ読み取りたい場合はご覧ください。  
OpenSiv3Dに読み込ませましたが、
DxLibにも矩形切り抜き描画があるので使えます。  
ベータ or マスタアップ以降サンプル追加は考えています。  

# 最後に  
animakeブランチで開発を継続していきます。  
要望などあればissues書いていただけると嬉しいです。  
https://github.com/Typhoon0x73/sample-opengl-for-imgui/tree/animake  

この記事のコメントでも大丈夫です。  
モチベーション維持のためアルファ版公開しているので是非意見ください！！  