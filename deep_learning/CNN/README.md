# CNN（Convolutional Neural Network）
## 概要
　畳み込みニューラルネットワーク（CNN :Convolutional Neural Network）は、ディープラーニングのなかで最もポピュラーであり、基礎的な手法である。  
　畳み込みニューラルネットワークは主に画像認識で用いられ、その圧倒的な精度ゆえにディープラーニングが注目される1つの要因を作り出した手法でもある。  
　畳み込みネットワークによる画像認識は応用範囲が広く、自動運転、監視カメラ、オンラインショッピングの商品検索など、さまざまな分野で活用が進んでいる。  
　層の数が多いニューラルネットワークを、うまく学習させるためのアイディアとして、タスクに応じた結合構造を予めネットワークに組み込むことで、結合重みの自由度を減らし、学習を容易かつ、処理負荷を下げるといものがあるが、畳み込みニューラルネットワークはこのアイディアを世襲している。  

### 視覚認識と畳み込みネットワーク  
　畳み込みネットワークは人間の視覚をモデルに考案されており、一般的なニューラルネットワークとは異なる特殊な構造をもつ。  

　人間が物体を見る際に生じる経過を追ってみると、最初に物体から反射された光が目の奥の網膜に像を結ぶ。その後、視神経を通じて脳に刺激が達し、物体が何であるかを認識する。そのとき、物体の像全体を一度に把握するのではなく、ある限定された領域ごとに像をスキャンするように認識する。この限定された領域を、「**局所受容野**」と呼ぶ。  
　局所受容野の光の刺激は、電気信号に変換されて脳に達し、そこで視覚認識に関係するニューロンが反応する。このニューロンには2種類あることが知られており、それぞれ「**単純型細胞**」 「**複雑型細胞**」と名づけられている。  
　単純型細胞は、ある特定の形状に反応する細胞である。さまざまな形状に反応する単純型細胞があり、それらが連携して活動することで複雑な形状の物体を認識する。  
　一方の複雑型細胞は、形状の空間的なずれを吸収するような働きをする。単純型細胞だけだと、ある形状の位置がずれると別の形状と見なすが、複雑型細胞は空間的な位置ずれを吸収し、同一形状と見なせるように働く。
![image](https://www.imagazine.co.jp/wp-content/uploads/2018/07/086-090_16ISno13_kiso_deep_zu001.jpg)  
　畳み込みネットワークはこの2つの細胞の働きを模倣するように考案されており、単純型細胞に対応する「**畳み込み層**」、複雑型細胞に対応する「**プーリング層**」というコンポーネントが用意されている。　

### 畳み込みニューラルネットワークの構造  
　最も基本的な畳み込みネットワークの形は、下図のようになる。最初の層が入力層で、画像の1ピクセルが1ニューロンに対応する（図の1マスが1ニューロンに相当）。その次に単純型細胞に相当する畳み込み（Conv）層、複雑型細胞に相当するプーリング（Pooling）層が続く。

![image](https://www.imagazine.co.jp/wp-content/uploads/2018/07/086-090_16ISno13_kiso_deep_zu002.jpg)  
　プーリング層のあとは、「**全結合層**」と「**出力層**」が接続される。  
　全結合層は、プーリング層からの出力をまとめるために置かれる。構造的にはニューラルネットワークの中間層と同様である。  
　出力層は各分類ラベルの判定確率を出力するもので、これもニューラルネットワークの出力層と同様である。  
　通常のニューラルネットワークとの違いは、畳み込み層とプーリング層の部分にある。

## 畳み込みニューラルネットワークの特徴  
　CNNには、**畳み込み**（Convolution）と**位置不変性** (Translation Invariance) 、 **合成性** (Compositionality)という3つの注目に値すべき点がある。
### 畳み込み（convolution）
　畳み込みは行列に対するオペレータとして考えておくと分かりやすい。  
- 例  
　入力画像を数字で表し、これを簡略化して2値に圧縮する。このような行列にフィルタを適用し畳み込み演算をする。  
　元の入力画像に対し、左上から右下まで要素ごとに掛け合わせていく。このようにしてフィルタをスライドさせていくことからsliding windowと呼ばれることもある。
![image](https://deepage.net/img/convolutional_neural_network/animated_convolution.gif)  
　こうして計算されたデータは特徴マップと呼ばれる。

### 合成性
　CNNはそれぞれの構成要素を理解すると、パズルのように組み合わせてつくることができるようになる。  
　各層は次の層へと意味のあるデータを順に受渡していく。層が進むにつれて、ネットワークはより高レベルな特徴を学習していける。  
### 移動不変性
　畳み込みの例で見たとおり、局所領域からフィルタを通して検出していくので物体の位置のズレに頑健になる。  
　つまり、特徴を検知する対象が入力データのどこにあっても検知することができる。これを移動不変性という。  
　回転や拡大・縮小に対する不変性はある程度は維持できるものの、それほど頑健ではないので、データ拡張でそういったデータを増やして学習するなど工夫が必要だ。  

## 畳み込みニューラルネットワークの構成要素
畳み込みニューラルネットワークは層と活性化関数といくつかのパラメータの組み合わせで出来上がっている。CNNはこの構成要素の知識さえあれば理解できるようになる。
### ゼロパディング（zero padding）
![image](https://deepage.net/img/convolutional_neural_network/zero_padding.jpg)  
　ゼロパディングは上図のように、入力の特徴マップの周辺を0で埋める。こうすることで以下のようなメリットがある。  
- 端のデータに対する畳み込み回数が増えるので端の特徴も考慮されるようになる  
- 畳み込み演算の回数が増えるのでパラメーターの更新が多く実行される  
- カーネルのサイズや、層の数を調整できる  
　畳み込み層とプーリング層で出力サイズは次第に小さくなるので、ゼロパディングでサイズを増やしたりすると層の数を増やすことができる。

### ストライド
ストライドは畳み込みの適用間隔のことで、図で表すとわかりやすい。入力データ4×4、適用フィル2×2に対してストライドを1とすると以下のようになる。  
![image](https://deepage.net/img/convolutional_neural_network/stride1.gif)  
これがストライド2になると2つ間隔でフィルタが進むようになる。  
![image](https://deepage.net/img/convolutional_neural_network/stride2.gif)  
　図より、ストライドを変えると出力の特徴マップのサイズが変わることが分かる。  
　出力サイズの高さ、幅を
![\begin{align*}
O_h, O_w
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AO_h%2C+O_w%0A%5Cend%7Balign%2A%7D%0A)  

として、フィルタサイズの高さと幅を  
![\begin{align*}
F_h, F_w
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AF_h%2C+F_w%0A%5Cend%7Balign%2A%7D%0A)  

そして、パディング*P*を、ストライド*S*を とすると、  
![\begin{align*}
O_h = \frac{H + 2P - F_h}{S} +1\\
O_w = \frac{W + 2P - F_w}{S} +1
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AO_h+%3D+%5Cfrac%7BH+%2B+2P+-+F_h%7D%7BS%7D+%2B1%5C%5C%0AO_w+%3D+%5Cfrac%7BW+%2B+2P+-+F_w%7D%7BS%7D+%2B1%0A%5Cend%7Balign%2A%7D%0A)  

である。この計算式を使って入力サイズ4×4、フィルタサイズ2×2、パディング0、ストライド2の場合を計算してみると、  
![\begin{align*}
O_h = \frac{4 + 0 - 2}{2} +1 = 2 \\
O_w = \frac{4 + 0 - 2}{2} +1 = 2
\end{align*}
](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+%5Cbegin%7Balign%2A%7D%0AO_h+%3D+%5Cfrac%7B4+%2B+0+-+2%7D%7B2%7D+%2B1+%3D+2+%5C%5C%0AO_w+%3D+%5Cfrac%7B4+%2B+0+-+2%7D%7B2%7D+%2B1+%3D+2%0A%5Cend%7Balign%2A%7D%0A)  

となり、先程のアニメーションと同様の結果となる。  
### 畳み込み（convolution）層
- 畳み込みとは  
　


## プーリング層について


## 有名な畳み込みネットワーク
### LeNet
　1998年に、Yann LeCunによって考案された初の畳み込みネットワークである。  
　構造としては、畳み込み層とプーリング層のセットを2回繰り返すのが特徴である。近年開発されたものと比較すると、層が浅く単純であるが、MNISTの手書き文字画像では99％以上の精度を出せる。判別カテゴリが少ない場合などは、十分に実用に堪えうる。最初に畳み込みネットワークを適用する際には、このネットワークから試してみるのもよい。

### AlexNet
　2012年に、ImageNetコンペティションで優勝したトロント大学SuperVisionチームの開発したネットワークである。  
　論文の筆頭著者Alex Krizhevskyの名前から、AlexNetと名づけられている。  
　畳み込み層5層にプーリング層3層という、LeNetと比較するとかなり深い層構造になっている。1400万以上のカラー画像を1万カテゴリに分類するというコンペティション向けに開発されたため、高い認識能力を誇るが、学習時にチューニングすべきパラメータ数も多く、実装には高いスペックをもつハードウェアが必要となる。

### VGG
　2014年に、ImageNetコンペティションで優勝したオックスフォード大学のチームが開発したネットワークである。  
　チーム名であるVisual Geometry Groupの頭文字から、VGGと呼ばれている。  
　通常、畳み込み層の局所受容野は5×5ニューロン程度で構成されることが多い。しかしVGGでは、局所受容野を3×3と小さくする代わりに、畳み込み層を増加させる方法を採用している。構成する層の数に応じて、VGG-11やVGG-16などと呼ばれることが多い。
