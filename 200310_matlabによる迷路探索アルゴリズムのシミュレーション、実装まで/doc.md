<!--
{"id":"26006613533178194","title":"matlabnによる迷路探索アルゴリズムのシミュレーション、実装まで（予告）","categories":["マイクロマウス","matlab"],"draft":"no"}
-->
こんにちは、おかやんです。

先日、mathworks様からマウサー向けにmatlabライセンスが発行されました。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">マイクロマウス関係者の皆さま！<br><br>2020年大会に向けて新しいMATLAB・Simluinkライセンスが発行可能になりました！<br><br>MathWorksのマイクロマウスページにある［ソフトウェアの利用を申し込む］からお申し込みください。<a href="https://t.co/VLqkiFhD63">https://t.co/VLqkiFhD63</a><a href="https://twitter.com/hashtag/%E3%83%9E%E3%82%A4%E3%82%AF%E3%83%AD%E3%83%9E%E3%82%A6%E3%82%B9?src=hash&amp;ref_src=twsrc%5Etfw">#マイクロマウス</a> <a href="https://twitter.com/hashtag/MATLAB?src=hash&amp;ref_src=twsrc%5Etfw">#MATLAB</a> <a href="https://t.co/C82fJJA8xN">pic.twitter.com/C82fJJA8xN</a></p>&mdash; MATLAB Japan (@MATLAB_jp) <a href="https://twitter.com/MATLAB_jp/status/1230671152944562176?ref_src=twsrc%5Etfw">February 21, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

私は昨年からこのライセンスをいただいているのですが、未だマウスの大会に参加できていません。申し訳ございません。今年は出ます。

昨年のライセンスは、

- マウスの同定 
- スラロームシミュレータの作成
- **迷路画像から迷路情報を抽出**
- **迷路探索シミュレータの作成**
- **迷路探索ロジックのC言語化**

といったことに活かさせていただきました。次回から数回に分けて下の三つについて、紹介していこうと思います。

ちなみに、作成したシミュレータは以下のような動作をします。
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">ブログ埋め込み用<br><br>こんな感じの迷路探索シミュレータを作成しました。<br>現状ではシンプルな足立法のみ実装してあります。 <a href="https://t.co/In8LWBHVar">pic.twitter.com/In8LWBHVar</a></p>&mdash; おかやん (@okayannnnnnnnnn) <a href="https://twitter.com/okayannnnnnnnnn/status/1237387652778700802?ref_src=twsrc%5Etfw">March 10, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


githubにてソースは公開します。
ただ、現状でREADMEすら整備していませんので、使い方はわからないかと思います。
細々と更新していきますので、しばらくお待ちください。
[githubはこちら][1]

[1]:https://github.com/okada-y/micromouse_maze_simlator.git
