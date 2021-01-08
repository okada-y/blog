<!--
{"id":"26006613675772574","title":"matlabによるマイクロマウス迷路シミュレータの作成と実装","categories":["マイクロマウス","matlab"],"updated":"2021-01-08T20:37:28+09:00","draft":"no"}
-->

こんにちは、おかやんです。  

前回のブログからほぼ一年が立とうとしていました。

裏では一応ほそぼそマウスやっていたんですがね、、、なかなかブログに手が伸びず・・・。

進捗を出しながらそれを文章化してというのを習慣化するというのは、思ったよりずいぶんと大変なことなんだなと実感するばかりです。

今回重い腰があがったのは、2020全日本（オンライン）大会に向けて、マウスのほうがひと段落つきそうになったからです。このブログも一区切りつかせとこうと。
マウスはこんな感じ。
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">ほそぼそ進捗<br><br>全日本どっちのルート走る気かバレちゃう <a href="https://t.co/vXsjtXDIJw">pic.twitter.com/vXsjtXDIJw</a></p>&mdash; おかやん (@okayannnnnnnnnn) <a href="https://twitter.com/okayannnnnnnnnn/status/1347383497913028609?ref_src=twsrc%5Etfw">January 8, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>




## 初めに
前回投稿した[予告][1]（詐欺）にて、以下内容を書くと宣言していました。

[1]:https://okayan.hatenablog.com/entry/2020/03/11/001815

- **迷路画像から迷路情報を抽出**
- **迷路探索シミュレータの作成**
- **迷路探索ロジックのC言語化**

ひとつづつ書いていこうと思います。詳細需要ない気もするので、概要まで。

### 迷路画像から迷路情報を抽出

先駆者が多数いるであろう"迷路の画像からの壁のデータ抽出をmatlabでやってみてた”です。
私が作成したスクリプトもとあるマウサーの手段を大いに参考にしています。

動作としては、図左のような画像から、壁の位置を取得し、図右のように出力させます。
![スクリーンショット 2021-01-08 135831.jpg](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20210108/20210108140540.jpg)

主な流れとしては、画像を取り込んで、二値化(imbinarize)し、
黒色のピクセルの数のピーク(findpeaks)から、迷路が記載されている範囲、柱の数(迷路のサイズ)の情報を抽出します。
![スクリーンショット 2021-01-08 141917.jpg](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20210108/20210108142133.jpg)

得られた柱の位置から、各柱の間の黒色ピクセルの数を取得し、
壁の有無を判定します。画像サイズが一定でないと、有無を判定するための閾値調整が必要になってしまいますが、画像サイズに対する割り合いを閾値とすると結構うまく動いてくれています。

動作には以下ツールボックスのライセンスが必要になります。

Signal Processing Toolbox,Image Processing Toolbox


### 迷路探索シミュレータの作成

作成した迷路シミュレータの動作はこんな感じ

[![](https://img.youtube.com/vi/CQBdrrpzLow/0.jpg)](https://www.youtube.com/watch?v=CQBdrrpzLow)

このシミュレータは

- 足立法でゴールし帰ってくるor全面探索or歩数最短経路上のみ探索
- 探索結果からの最短ルート選定（斜めor直進）

といった動作をシミュレーションできます。基本的には世に転がってるシミュレータと大差ないと思いますので、ロジックについては書きませんが、以下の本や多数の競技者のブログが参考になりました。

[asin:4274507114:detail]

今回あえてmatlabでシミュレータをつくったのは、この後紹介するC言語化が簡単にできそうだったのがメインの理由ですが、
可視化もしやすそうだなと思ったのも理由の一つです。
可視化する際に詰まったところを少し紹介します。

#### matlabをつかったアニメーション作成
matlabは簡単にいい感じのグラフを書いてくれます。
例えば、plotやsurfといった関数がよく使われるかと思いますが、
今回のアニメーションもこれらを用いています。

具体的には、
plot、surfで出力されたオブジェクトをマウスの探索状況等から都度更新することでアニメーションにしています。

このオブジェクトという概念の理解が私にはハードルとなっていました。

コードのイメージとしては
```
%マウス本体を示すオブジェクト作成

h = hgtransform('Parent',gca);%現在の軸(axes)にtransformオブジェクトを作成
b = plot(x,y),'ob','MarkerFaceColor','b','Parent',h);%transformオブジェクトを親としてplotでラインオブジェクト（青●）を生成

%迷路探索処理

%探索経過に応じた変換行列の作成
m = makehgtform(～～～);

%変換行列をtransformオブジェクトに適用
h.Matrix = m;

%描画を更新
drawnow
```
であったり、
```
%探索状況に応じたsurfオブジェクトを作成
search_surf = surf(x,y,z,'EdgeColor','none','FaceColor','flat','FaceAlpha',0.4);

%迷路探索処理

%探索経過に応じてsurfオブジェクトを変換
 z = 探索状況; 
 search_surf.ZData = z; 

%描画を更新
drawnow

```

といった感じです。基本的にはオブジェクトを生成、探索、変換、描画更新の流れです。ここらへんはmatlabマニュアルが充実しているので、
hgtransformやsurfあたりから検索してみるといいかもしれません。


### C言語化と実装
さて、本題、な気がする。

マイクロマウスを始めたきっかけであるマイクロマウス競技者へのmatlabのライセンス配布。このライセンス普通に購入するhomeライセンスと比較し、つよつよな点がある。

__MATLABCORDER__　が入っているのである。

これはつまりmatlabで作成した迷路探索ロジックをそのままC言語化し、マウスに突っ込めるのである。強い。

ということでつかってみた。
使い方というより突っかかった点のほうが需要ありそうなのでそちらを紹介する。

#### 型定義

matlabでは普通型定義をしない。特に指定なければdouble型となる。

これはCと比較するととても楽で便利だが、C言語化するとそのままdouble型で実装される。
容量や処理時間が許容できるものであれば無視してもいい気がするが、
そこらのフラグがdoubleでのさばっているのは精神衛生上善くない。そこで、以下のように都度指定する。

```
    %ローカル変数宣言
    wall_write = uint8(zeros(1,4));%壁情報書き込み用バッファ(N,E,S,W)
    serch_write = uint8(zeros(1,4));%探索情報書き込み用バッファ(N,E,S,W)
    %壁センサAD値格納変数
    wall_sensor_front = int16(0);
    wall_sensor_right = int16(0);
    wall_sensor_left = int16(0);

```
めんどくさかった、とても。。。
もはやCで書いている気分になった。何のためにmatlabで書いているんだと。これ対策あったら教えてほしいです。またやるかどうかは抜きとして。
Simlinkでも同じような処理するし、ここは仕方ない気もする。


#### シミュレータ、実装時の動作切り分け
シミュレーション(matlab)とC言語（実装時）の動作を切り分けたいシーンがでてくる。たとえば壁の有無の判断の際、シミュレーションは既存データを参照するのみだが、実装時は壁センサの値から判断する。

そういったとき、以下のようにcordertargetを使用することで切り分けることができる。
```
%右壁判定
if coder.target('MATLAB')
    %for matlab
    %既知の迷路情報をもとに、壁の有無を判定
    if bitand(maze_serial(current_y,current_x),bitshift(uint8(1),rem(current_dir+1,4)) )
        wall_write(uint8(1),rem(current_dir+1,4)+uint8(1)) = wall.wall;
    end
else
    %for Cgen
    %センサ値取得
    wall_sensor_right = coder.ceval('m_get_right_sensor');
    %センサ値をもとに、壁の有無を判定
    if int16(wall_sensor_right) > int16(wall_sensor_right_th)
        %壁情報取得
        wall_write(uint8(1),rem(current_dir+1,4)+uint8(1)) = wall.wall;
        %壁フラグセット
        wall_flg = bitor(wall_flg,2,'uint8');
    end
end

```
これでシミュレータの動作とは別に実装時のコードを記述できる。注意点としては実装時のみ適用される部分のコードは、matlab上でデバックできない。使用は最低限の範囲としたほうがいいと思われる。

#### C言語との連結
ほかの人がどうやっているのか知りたいテーマ。

どこまでをシミュレータ側で記述して実装側でどこまで書くか。

私はフラグ等の管理はシミュレータ側で実施し、Cで記述したマウスを動かす関数を適宜呼び出す形をとっています。
```
if ~coder.target('MATLAB')
    coder.ceval('m_move_front',start_flg,wall_flg,uint8(move_dir_property.straight));
end

```
ここで m_move_front は固定の距離だけまっすぐ進むという関数です。中身は以下のようになっていて、C側で記述している関数を呼び出すのみとなっています。

```
//前進
void m_move_front(unsigned char start_flg,unsigned char wall_flg,unsigned char move_dir_property){  
//     c側で記述した動作関数を記述すること
	move_front (start_flg,wall_flg,move_dir_property);
}
```
この形の問題点としては、シミュレータ動作には特に関係のない動作関数用のフラグをシミュレータで管理することで、各フラグのデバッグが不便になってしまっているという点です。おすすめの手段あれば教えていただきたいです。

#### 関数の特殊化について

これはそんなに気にする必要もない気がするのですが、コード生成するとたまに

```
hogehoge1()
hogehoge2()
hogehoge3()
```
といったように、作った覚えのない連番の関数が生成されています。
これは、関数の特殊化によるもので、入力引数のパターンが固定であったりすると、入力引数が固定化された関数をいくつか用意してくれる見たいです。動作はおよそ問題ない気がしますが、生成コードの可読性が悪くなる（そもそも読むのが間違い？）ので、ちょっと対策する。

```
search_adachi(current_x,current_y,current_dir,maze_row_size,maze_col_size,maze_wall,maze_wall_search,coder.ignoreConst(new_goal),coder.ignoreConst(new_goal_size),...
        start_flg,stop_flg,goal_after_flg,adachi_search_mode.goal);
```
上のコードのようにcoder.ignoreConst(入力引数)とすればよい。
これはこれでmatlabがわの可読性が落ちるのだが…。



### おわりに
つらつらと書いてしまいました。

内容としても薄くなってしまったような・・・・。正直一年ぐらい前の内容もあってどこでどう躓いたかわすれているものがほとんどなんですよね。都度書くのが大事。今年は頑張りましょう。

あそこもうちょっと丁寧に書いてとか、間違っているとかもしあればコメントいただけると幸いです。

それでは。








