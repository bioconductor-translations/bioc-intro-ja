---
source: Rmd
title: データの可視化
teaching: 60
exercises: 60
---



::::::::::::::::::::::::::::::::::::::: objectives

- ggplotを使って散布図、箱ひげ図、折れ線グラフなどを作成する。
- ユニバーサルプロット設定を行う。
- ファセットとは何かを説明し、ggplotでファセットを適用する。
- 既存のggplotプロットの美学（軸ラベルや色を含む）を修正する。
- データフレーム内のデータから複雑でカスタマイズされたプロットを作成。

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Rによる可視化

::::::::::::::::::::::::::::::::::::::::::::::::::



> このエピソードは、Data Carpentriesの_Data Analysis and
> Visualisation in R for Ecologists_レッスンに基づいています。

## データの可視化

必要なパッケージをロードすることから始める。 \*\*ggplot2`**は、 **tidyverse`\*\*パッケージに含まれています。


``` r
library("tidyverse")
```

ワークスペースにまだない場合は、前回の
レッスンで保存したデータをロードします。


``` r
rna <- read.csv("data/rnaseq.csv")
```

Data Visualization Cheat
Sheet](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-visualization.pdf)
は `ggplot2` の基本からより高度な機能までをカバーし、
パッケージで利用可能な多くのデータ表現
の概要を理解するためのリマインダとしてだけでなく、手助けになるでしょう。 Thomas Lin Pedersen氏による以下のビデオ
チュートリアル（[パート1](https://www.youtube.com/watch?v=h29g21z0a68)と
[2](https://www.youtube.com/watch?v=0m4yywqNPVY)）
も非常に参考になる。

## ggplot2\`によるプロット

ggplot2`は、データフレーム内のデータから複雑な
プロットを簡単に作成できるプロットパッケージである。 どの変数をプロットするか、どのように表示するか、
、一般的なビジュアル・プロパティを指定するための、よりプログラム的な
。 
`ggplot2\`を支える理論的基盤は、_Grammar of Graphics_ (@Wilkinson:2005)である。 この
アプローチを使用すると、基礎となるデータが変更された場合、または棒グラフから散布図に変更することを決めた場合に
、最小限の変更で済む。 これは、
、最小限の調整で出版品質のプロットを作成するのに役立ちます
、微調整。

ggplot2`に関する本（@ggplot2book）があり、
。 第3版は現在準備中で、 [オンラインで自由に利用できる](https://ggplot2-book.org/)。 
の `ggplot2\` ウェブページ([https://ggplot2.tidyverse.org](https://ggplot2.tidyverse.org))に十分なドキュメントがあります。

ggplot2`の関数は、'long'形式のデータ、つまり、
すべての次元を表す列と、すべてのオブザベーションを表す行を持つ。 よく構造化されたデータ
、 `ggplot2\`で図を作成する時間を大幅に節約できる。

ggplotグラフィックスは、新しい要素を追加することによって段階的に構築される。 この方法で
レイヤーを追加すると、プロットの広範な柔軟性と
カスタマイズが可能になる。

> Grammar of Graphicsの背後にある考え方は、同じ3つのコンポーネントからすべての
> グラフを構築できるということである：(1)データセット、(2)座標系、
> 、(3)ジオム、つまりデータ点を表す視覚的マーク [^three_comp_ggplot2] である。

[^three_comp_ggplot2]: 出典[データ可視化チートシート](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-visualization.pdf).

ggplotを構築するために、
、さまざまなタイプのプロットに使用できる以下の基本テンプレートを使用する：

```
ggplot(data =<DATA>, mapping = aes(<MAPPINGS>)) +<GEOM_FUNCTION>()
```

- ggplot()`関数を使用し、`data\`引数を使用して特定の**data
 frame**にプロットをバインドする。


``` r
ggplot(data = rna)
```

- (`aes`)関数を使用して)**マッピング**を定義する。
 、プロットする変数を選択し、
 、x/yの位置や、
 、サイズ、形、色などの特徴として、グラフでどのように表示するかを指定する。


``` r
ggplot(data = rna, mapping = aes(x = expression))
```

- 追加 '**geoms**' - ジオメトリ、つまりプロット内の
 データのグラフ表現（点、線、棒）。 ggplot2\`は多くの
 、様々なジオムを提供している：

 ```
 * 散布図やドットプロットなどには `geom_point()` を使用する。
 *
 * `geom_boxplot()` ボックスプロット！
 * トレンドライン、時系列など。
 ```

プロットにgeom(etry)を追加するには `+` 演算子を使います。
`geom_histogram()` をまず使ってみよう：


``` r
ggplot(data = rna, mapping = aes(x = expression)) +
  geom_histogram()
```

``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="fig/40-visualization-rendered-first-ggplot-1.png" style="display: block; margin: auto;" />

ggplot2`パッケージの`+`は特に便利で、
、既存の`ggplot\`オブジェクトを修正することができる。 つまり、
、プロット・テンプレートを簡単に設定し、
、さまざまなタイプのプロットを便利に調べることができる：


``` r
# Assign plot to a variable
rna_plot <- ggplot(data = rna,
                   mapping = aes(x = expression))

# Draw the plot
rna_plot + geom_histogram()
```

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

ヒストグラムを描画するときに表示される自動メッセージにお気づきでしょう：


``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

geom_histogram()`の引数`bins`または`binwidth\` を変更して、
ビンの数または幅を変更する。

:::::::::::::::  solution

## ソリューション


``` r
# change bins
ggplot(rna, aes(x = expression)) +
    geom_histogram(bins = 15)
```

<img src="fig/40-visualization-rendered-unnamed-chunk-5-1.png" style="display: block; margin: auto;" />

``` r
# change binwidth
ggplot(rna, aes(x = expression)) +
    geom_histogram(binwidth = 2000)
```

<img src="fig/40-visualization-rendered-unnamed-chunk-5-2.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

データが右に偏っていることがわかる。 より対称的な分布にするために、
log2変換を適用することができる。 ここでは、
、0に等しい式の値に対して返される`-Inf`値
を避けるために、小さな定数値（`+1`）を追加していることに注意してください。


``` r
rna<- rna %>%
  mutate(expression_log = log2(expression + 1))
```

ここで対数変換した式のヒストグラムを描いてみると、
の分布は確かに正規分布に近くなっている。


``` r
ggplot(rna, aes(x = expression_log)) + geom_histogram()
```

``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="fig/40-visualization-rendered-second-ggplot-1.png" style="display: block; margin: auto;" />

これからは対数変換した発現値を扱うことにする。

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

この変換を視覚化するもう1つの方法は、オブザベーションのスケール
を考えることである。 たとえば、
プロットの空間でオブザベーションをよりよく分布させるために、軸のスケール
を変更する価値があるかもしれません。 軸のスケールの変更は、
他のコンポーネントの追加/変更と同様に（すなわち、
コマンドをインクリメンタルに追加することによって）行われます。 このように変更してみてほしい：

- `scale_x_log10()` を参照。 前のグラフと比較してみよう。
 、警告メッセージが表示されるようになったのはなぜですか？

:::::::::::::::  solution

## ソリューション


``` r
ggplot(data = rna,mapping = aes(x = expression))+
  geom_histogram() +
  scale_x_log10()
```

``` warning
Warning in scale_x_log10(): log-10 transformation introduced infinite values.
```

``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

``` warning
Warning: Removed 507 rows containing non-finite outside the scale range
(`stat_bin()`).
```

<img src="fig/40-visualization-rendered-unnamed-chunk-6-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

**注**\*。

- ggplot()`関数で設定したものはすべて、あなたが追加したgeom
   レイヤーで見ることができます（つまり、これらはグローバルなプロット設定です）。 この
   には、`aes()\`で設定したx軸とy軸のマッピングが含まれる。
- また、`ggplot()`関数でグローバルに定義された
 マッピングとは別に、与えられたジオムに対するマッピングを指定することもできます。
- 新しいレイヤーを追加するために使われる`+`記号は、_前の_レイヤーを含む
 行の最後に置かなければなりません。 その代わりに、`+`記号が
 新しいレイヤーを含む行の先頭に追加された場合、
 `ggplot2` は新しいレイヤーを追加せず、エラー
 メッセージを返す。


``` r
# これはレイヤーを追加するための正しい構文です
rna_plot +
  geom_histogram()

# これは新しいレイヤーを追加せず、エラーメッセージを返します
rna_plot
  + geom_histogram()
```

## 反復的にプロットを構築する

ここでは、2つの連続変数と
`geom_point()`関数を使って散布図を描きます。 このグラフは、時間8と時間0、時間4と時間0を比較した発現のlog2倍変化
。
この目的のために、まず対数変換した
発現値の平均値を遺伝子ごと、時間ごとに計算する必要がある。次に、
、時間8と時間0の間の平均対数発現と、
、時間4と時間0の間の平均対数発現を差し引くことにより、対数倍変化を計算する。 ここでは、後で遺伝子を表現するために使用する遺伝子
バイオタイプも含めていることに注意されたい。
フォールドの変化を `rna_fc.` という新しいデータフレームに保存する。


``` r
rna_fc <- rna %>% select(gene, time,
                         gene_biotype, expression_log) %>%
  group_by(gene, time, gene_biotype) %>%
  summarize(mean_exp = mean(expression_log)) %>%
  pivot_wider(names_from = time,
              values_from = mean_exp) %>%
  mutate(time_8_vs_0 = `8` - `0`, time_4_vs_0 = `4` - `0`)
```

``` output
`summarise()` has grouped output by 'gene', 'time'. You can override using the
`.groups` argument.
```

新しく作成されたデータセット `rna_fc` を使って ggplot を作成することができる。
ggplot2\`でプロットを作成するのは、通常、反復的なプロセスである。
まず、使用するデータセットを定義し、軸を配置し、
ジオムを選択する：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point()
```

<img src="fig/40-visualization-rendered-create-ggplot-object-1.png" style="display: block; margin: auto;" />

次に、このプロットからより多くの情報を抽出するために、プロットを修正し始める。
例えば、オーバープロットを避けるために透明度（`α`）を加えることができる：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point(alpha = 0.3)
```

<img src="fig/40-visualization-rendered-adding-transparency-1.png" style="display: block; margin: auto;" />

また、すべてのポイントに色を付けることもできる：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point(alpha = 0.3, color = "blue")
```

<img src="fig/40-visualization-rendered-adding-colors-1.png" style="display: block; margin: auto;" />

あるいは、プロット中の各遺伝子を異なる色にするために、
、引数**color**の入力としてベクトルを使うこともできる。 ggplot2`は、ベクトルの異なる値に対応する異なる
。 以下は、
`gene_biotype\`を使った例である：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point(alpha = 0.3, aes(color = gene_biotype))
```

<img src="fig/40-visualization-rendered-color-by-gene_biotype1-1.png" style="display: block; margin: auto;" />

また、
`ggplot()`関数で提供されるマッピングの中で直接色を指定することもできる。 これはどのジオムレイヤーでも見ることができ、
のマッピングは `aes()` で設定したx軸とy軸によって決定される。


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0,
                                color = gene_biotype)) +
  geom_point(alpha = 0.3)
```

<img src="fig/40-visualization-rendered-color-by-gene_biotype2-1.png" style="display: block; margin: auto;" />

最後に、`geom_abline()`
：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0,
                                color = gene_biotype)) +
  geom_point(alpha = 0.3) +
  geom_abline(intercept = 0)
```

<img src="fig/40-visualization-rendered-adding-diag-1.png" style="display: block; margin: auto;" />

ジオムレイヤーを `geom_point` から
`geom_jitter` に変更しても、色は `gene_biotype` によって決定されることに注意してください。


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0,
                                color = gene_biotype)) +
  geom_jitter(alpha = 0.3) +
  geom_abline(intercept = 0)
```

<img src="fig/40-visualization-rendered-color-by-gene_biotype3-1.png" style="display: block; margin: auto;" />



:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

散布図は、小規模なデータセットの探索に役立つツールである。 rna_fc\`
データセットのような多数のオブザベーションを持つ
データセットの場合、点のオーバープロットは散布図の制限となりうる。
このような設定を扱うための1つの戦略は、
の観測値を六角形にビニングすることである。 プロット空間は六角形にテッセレーションされている。 それぞれの
六角形は、
その境界内に入るオブザベーションの数に基づいて色が割り当てられる。

- ggplot2`で六角ビニングを使用するには、まずRパッケージ
 `hexbin\`をCRANからインストールしてロードする。

- そして、`geom_hex()`関数を使ってhexbin図を作成する。

- 散布図と比較して、六角形のビン
 プロットの相対的な長所と短所は何か？ 上記の散布図（
 ）を調べ、作成した六角形のビンプロットと比較する。

:::::::::::::::  solution

## ソリューション


``` r
install.packages("hexbin")
```


``` r
library("hexbin")

ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_hex() +
  geom_abline(intercept = 0)
```

<img src="fig/40-visualization-rendered-unnamed-chunk-10-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

今学んだことを使って、`rna`データセットから`sample`に対する`expression_log`
の散布図を作成し、
異なる色で時間を表示する。 このようなデータを表示するのは良い方法ですか？

:::::::::::::::  solution

## ソリューション


``` r
ggplot(data = rna, mapping = aes(y = expression_log, x = sample)) +
    geom_point(aes(color = time))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-11-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## ボックスプロット

ボックスプロットを使って、各サンプル内の遺伝子発現の分布（
）を可視化することができる：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_boxplot()
```

<img src="fig/40-visualization-rendered-boxplot-1.png" style="display: block; margin: auto;" />

boxplotにポイントを追加することで、
の測定数とその分布をよりよく知ることができる：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_jitter(alpha = 0.2, color = "tomato") +
  geom_boxplot(alpha = 0)
```

<img src="fig/40-visualization-rendered-boxplot-with-points-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

ボックスプロットレイヤーがジッターレイヤーの前にあることに注目してほしい。
、ボックスプロットをポイントの下に配置するために、コードのどこを変更する必要がありますか？

:::::::::::::::  solution

## ソリューション

この2つのジオムの順番を入れ替えるべきだ：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample))+
  geom_boxplot(alpha = 0) +
  geom_jitter(alpha = 0.2, color = "tomato")
```

<img src="fig/40-visualization-rendered-boxplot-with-points2-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

X軸の値がまだ正しく
読めないことにお気づきかもしれない。 ラベルの向きを変え、
縦と横に重ならないように調整しよう。
90度の角度を使ってもいいし、
斜め向きのラベルに適切な角度を見つけるために実験してもいい：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample))+
  geom_jitter(alpha = 0.2, color = "tomato") +
  geom_boxplot(alpha = 0) +
  theme(axis.text.x = element_text(angle = 90, hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-boxplot-xaxis-rotated-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

感染期間（
）に応じて、箱ひげ図上のデータ点に色を付ける（`time`）。

_ヒント:_ `time`のクラスをチェックする。
`time` のクラスをggplotマッピングで整数から因数に直接変更することを検討する。
、Rのグラフの作り方が変わるのはなぜですか？

:::::::::::::::  solution

## ソリューション


``` r
# time as integer
ggplot(data = rna,
         mapping = aes(y = expression_log,
                       x = sample))+
  geom_jitter(alpha = 0.2, aes(color = time))+
  geom_boxplot(alpha = 0) +
  theme(axis.text.x = element_text(angle = 90, hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-boxplot-color-time-1.png" style="display: block; margin: auto;" />

``` r
# time as factor
ggplot(data = rna,
         mapping = aes(y = expression_log,
                       x = sample))+
  geom_jitter(alpha = 0.2, aes(color = as.factor(time)))+
  geom_boxplot(alpha = 0) +
  theme(axis.text.x = element_text(angle = 90, hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-boxplot-color-time-2.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

箱ひげ図は便利な要約だが、
分布の_形_を隠してしまう。 例えば、分布が二峰性であれば、
、ボックスプロットではそれを見ることはできない。 箱ひげ図に代わるものとして、（点の密度の）形状を描くバイオリン
プロットがある。

- geom_violin()`を参照してください。 
   引数 `fill\` の時間に従ってヴァイオリンにフィルを入れる。

:::::::::::::::  solution

## ソリューション


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample))+
  geom_violin(aes(fill = as.factor(time)))+
  theme(axis.text.x = element_text(angle = 90, hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-12-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

- ヴァイオリンのプロットを修正し、ヴァイオリンを `sex` で埋める。

:::::::::::::::  solution

## ソリューション


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample))+
  geom_violin(aes(fill = sex))+
  theme(axis.text.x = element_text(angle = 90, hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-13-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 線プロット

時刻8と
時刻0を比較し、log fold変化が最も大きかった10の遺伝子について、感染期間ごとの平均発現量を計算してみよう。 まず、遺伝子を選択し、選択した10遺伝子を含む`sub_rna`と呼ばれる`rna`
のサブセットを作成する必要がある。次に、
データをグループ化し、各グループ内の平均遺伝子発現を計算する必要がある：


``` r
rna_fc<- rna_fc %>% arrange(desc(time_8_vs_0))

genes_selected <- rna_fc$gene[1:10]

sub_rna<- rna %>%
    filter(gene %in% genes_selected)

mean_exp_by_time<- sub_rna %>%
  group_by(gene,time) %>%
    summarize(mean_exp = mean(expression_log))
```

``` output
`summarise()` has grouped output by 'gene'. You can override using the
`.groups` argument.
```

``` r
mean_exp_by_time
```

``` output
# A tibble: 30 × 3
# Groups:   gene [10]
   gene   time mean_exp
   <chr> <int>    <dbl>
 1 Acr       0     5.07
 2 Acr       4     5.54
 3 Acr       8     7.31
 4 Aipl1     0     3.70
 5 Aipl1     4     3.89
 6 Aipl1     8     6.56
 7 Bst1      0     3.20
 8 Bst1      4     3.77
 9 Bst1      8     5.22
10 Chil3     0     4.00
# ℹ 20 more rows
```

X軸に感染期間（
）、Y軸に平均発現をとって折れ線グラフを作成することができる：


``` r
ggplot(data = mean_exp_by_time, mapping = aes(x = time, y = mean_exp))+
  geom_line()
```

<img src="fig/40-visualization-rendered-first-time-series-1.png" style="display: block; margin: auto;" />

残念なことに、これはうまくいかない。というのも、
の全遺伝子のデータをまとめてプロットしたからである。 各遺伝子に対して線を引くようにggplotに指示する必要がある。
、`group = gene`を含むようにesthetic関数を修正する：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp, group = gene))+
  geom_line()
```

<img src="fig/40-visualization-rendered-time-series-by-gene-1.png" style="display: block; margin: auto;" />

色をつければ、プロットの中で遺伝子を区別できるようになる（
`color`を使えば、自動的にデータをグループ化することもできる）：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp, color = gene))+
  geom_line()
```

<img src="fig/40-visualization-rendered-time-series-with-colors-1.png" style="display: block; margin: auto;" />

## ファセット

ggplot2\`には_faceting_と呼ばれる特別なテクニックがあり、
、データセットに含まれる
、1つのプロットを複数の(サブ)プロットに分割することができる。 こ れ ら の異な る サブプ ロ ッ ト は、 同 じ プ ロ パテ ィ
を継承 し ます （軸の限界、目盛り、 ...）。 直接比較しやすくするためだ。
、これを用いて各遺伝子について時間軸に沿った折れ線グラフを作成する：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp))+ geom_line() +
  facet_wrap(~ gene)
```

<img src="fig/40-visualization-rendered-first-facet-1.png" style="display: block; margin: auto;" />

ここでは、X軸とY軸はすべてのサブプロットで同じスケールになっている。、Y軸のスケールを自由に設定できるように`scales`を修正することで、このデフォルトの動作を変更することができる：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y")
```

<img src="fig/40-visualization-rendered-first-facet-scales-1.png" style="display: block; margin: auto;" />

ここで、各プロットの線をマウスの性別で分けたい。
そのためには、
、遺伝子、時間、性別でグループ化したデータフレームの平均発現を計算する必要がある：


``` r
mean_exp_by_time_sex<- sub_rna %>%
  group_by(gene, time, sex) %>%
    summarize(mean_exp = mean(expression_log))
```

``` output
`summarise()` has grouped output by 'gene', 'time'. You can override using the
`.groups` argument.
```

``` r
mean_exp_by_time_sex
```

``` output
# A tibble: 60 × 4
# Groups:   gene, time [30]
   gene   time sex    mean_exp
   <chr> <int> <chr>     <dbl>
 1 Acr       0 Female     5.13
 2 Acr       0 Male       5.00
 3 Acr       4 Female     5.93
 4 Acr       4 Male       5.15
 5 Acr       8 Female     7.27
 6 Acr       8 Male       7.36
 7 Aipl1     0 Female     3.67
 8 Aipl1     0 Male       3.73
 9 Aipl1     4 Female     4.07
10 Aipl1     4 Male       3.72
# ℹ 50 more rows
```

`color` を使ってさらに分割することで、ファセット化されたプロットを作ることができる（1つのプロット内で）：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y")
```

<img src="fig/40-visualization-rendered-facet-by-gene-and-sex-1.png" style="display: block; margin: auto;" />

通常、背景が白いプロットは印刷したときに読みやすくなる。
、関数 `theme_bw()` を使って背景を白に設定することができる。
さらに、グリッドを削除することもできる：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank())
```

<img src="fig/40-visualization-rendered-facet-by-gene-and-sex-white-bg-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

、各染色体の
平均発現量が感染期間を通じてどのように変化するかをプロットする。

:::::::::::::::  solution

## ソリューション


``` r
mean_exp_by_chromosome<- rna %>%
  group_by(chromosome_name, time) %>%
  summary(mean_exp = mean(expression_log))

ggplot(data = mean_exp_by_chromosome, mapping = aes(x = time,
                                y = mean_exp))+
  geom_line() +
  facet_wrap(~ chromosome_name, scales = "free_y")
```

``` error
Error in `fortify()`:
! `data` must be a <data.frame>, or an object coercible by `fortify()`,
  or a valid <data.frame>-like object coercible by `as.data.frame()`.
Caused by error in `.postvalidate_data_frame_like_object()`:
! `as.data.frame(data)` must preserve dimensions.
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

facet_wrap`ジオメトリは、1ページにきれいに収まるように、プロットを任意の数の
次元に抽出する。 一方、
`facet_grid` ジオメトリでは、
数式表記（`rows ~ columns`; `.\`
は、1つの行または列のみを示すプレースホルダとして使用できます）によって、プロットの配置方法を明示的に指定することができます。

先ほどのプロットを修正し、男性と女性の平均遺伝子発現
が経時的にどのように変化したかを比較してみよう：


``` r
# 1列、行ごとのファセット
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = gene))+
  geom_line() +
  facet_grid(sex ~ .)
```

<img src="fig/40-visualization-rendered-mean-exp-time-facet-sex-rows-1.png" style="display: block; margin: auto;" />


``` r
# 1行、列ごとのファセット
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = gene))+
  geom_line() +
  facet_grid(. ~ sex)
```

<img src="fig/40-visualization-rendered-mean-exp-time-facet-sex-columns-1.png" style="display: block; margin: auto;" />

## ggplot2\`テーマ

`ggplot2` には、プロットの背景を白に変更する `theme_bw()` に加えて、
手早く視覚化の見た目を変更するのに便利なテーマがいくつか用意されている。
テーマの全リストは[https://ggplot2.tidyverse.org/reference/ggtheme.html](https://ggplot2.tidyverse.org/reference/ggtheme.html)で入手できる。
theme_minimal()`や`theme_light()`は人気があり、`theme_void()\`
は新しい手作りのテーマを作る出発点として役に立つ。

ggthemes](https://jrnold.github.io/ggthemes/reference/index.html)
パッケージは様々なオプション（Excel 2003
テーマを含む）を提供します。 `ggplot2` extensions
website](https://exts.ggplot2.tidyverse.org/) は、追加の
テーマを含む `ggplot2` の機能を拡張する
パッケージのリストを提供しています。

## カスタマイズ

時間別、遺伝子別の平均発現のファセット・プロットに戻ろう。
性別に色分けされている。

ggplot2\`のカンニング
シート](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-visualization.pdf),
を見て、プロットを改善する方法を考えてみてください。

ここで、軸の名前を
'time' や 'mean_exp' よりも情報量の多いものに変更し、図にタイトルを追加する：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  labs(title = "感染期間別の平均遺伝子発現",
       x = "感染期間（日単位）",
       y = "平均遺伝子発現")
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-right-labels-1.png" style="display: block; margin: auto;" />

軸にはより情報量の多い名前が付けられているが、フォントサイズを大きくすることで読みやすさは
：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  labs(title = "感染期間別の平均遺伝子発現",
       x = "感染期間（日単位）",
       y = "平均遺伝子発現") +
  theme(text = element_text(size = 16))
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-right-labels-xfont-size-1.png" style="display: block; margin: auto;" />

プロットのフォントを変更することも可能です。
Windowsをお使いの場合は、 をインストールする必要があるかもしれません。

さらに、X軸とY軸のテキストの色、
グリッドの色などをカスタマイズできる。 例えば、
`legend.position`を`"top"`に設定することで、凡例を一番上に移動させることもできる。


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  labs(title = "感染期間別の平均遺伝子発現",
       x = "感染期間（日単位）",
       y = "平均遺伝子発現") +
  theme(text = element_text(size = 16),
        axis.text.x = element_text(color = "royalblue4", size = 12),
        axis.text.y = element_text(color = "royalblue4", size = 12),
        panel.grid = element_line(color="lightsteelblue1"),
        legend.position = "top")
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-theme-1.png" style="display: block; margin: auto;" />

作成した変更がデフォルトのテーマよりも気に入った場合は、
、オブジェクトとして保存して、作成した他の
プロットに簡単に適用することができます。 以下は、
以前に作成したヒストグラムを使った例です。


``` r
blue_theme <- theme(axis.text.x = element_text(colour = "royalblue4",
                                               size = 12),
                    axis.text.y = element_text(colour = "royalblue4",
                                               size = 12),
                    text = element_text(size = 16),
                    panel.grid = element_line(colour="lightsteelblue1"))

ggplot(rna, aes(x = expression_log)) +
  geom_histogram(bins = 20) +
    blue_theme
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-right-labels-xfont-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## チャレンジ

これらの情報を手に入れたら、
、この練習で作成したプロットのいずれかを改良するか、
、あなた自身の美しいグラフを作成してください。 RStudio ggplot2
を参考にしてください。 いくつかアイデアを挙げてみよう：

- 線の太さを変えてみてください。
- 伝説の名前を変える方法はありますか？
 そのラベルについてはどうだろう？ (ヒント:
 `scale_` で始まる ggplot 関数を探す)
- 別のカラーパレットを使用するか、線の
 カラーを手動で指定してみてください（
 [http://www.cookbook-r.com/Graphs/Colors_(ggplot2)/](https://www.cookbook-r.com/Graphs/Colors_\(ggplot2\)/) 参照）。

:::::::::::::::  solution

## ソリューション

例えば、このプロットに基づくと


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex))+
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank())
```

<img src="fig/40-visualization-rendered-unnamed-chunk-15-1.png" style="display: block; margin: auto;" />

以下のようなカスタマイズが可能です：


``` r
# change the thickness of the lines
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line(size=1.5) +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank())
```

``` warning
Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
ℹ Please use `linewidth` instead.
This warning is displayed once every 8 hours.
Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
generated.
```

<img src="fig/40-visualization-rendered-unnamed-chunk-16-1.png" style="display: block; margin: auto;" />

``` r
# change the name of the legend and the labels
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  scale_color_discrete(name = "Gender", labels = c("F", "M"))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-16-2.png" style="display: block; margin: auto;" />

``` r
# using a different color palette
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  scale_color_brewer(name = "Gender", labels = c("F", "M"), palette = "Dark2")
```

<img src="fig/40-visualization-rendered-unnamed-chunk-16-3.png" style="display: block; margin: auto;" />

``` r
# manually specifying the colors
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  scale_color_manual(name = "Gender",  labels = c("F", "M"),
                     values = c("royalblue", "deeppink"))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-16-4.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## プロットの構成

ファセッティングは、1つのプロットを複数のサブプロットに分割するのに最適なツールです。
しかし、
複数の独立したプロット、すなわち異なる
変数、あるいは異なるデータフレームに基づくプロットを含む1つの図を作成したい場合があります。

、まず2つのプロットを作成する：

最初のグラフは、染色体ごとにユニークな遺伝子の数を数えている。
、まず`chromosome_name`のレベルを並べ替え、
、染色体ごとにユニークな遺伝子をフィルタリングする必要がある。 また、読みやすくするために、Y軸のスケールを
log10スケールに変更した。


``` r
rna$chromosome_name <- factor(rna$chromosome_name,
                               levels = c(1:19,"X","Y"))

count_gene_chromosome <- rna %>% select(chromosome_name, gene) %>%
  distinct() %>% ggplot() +
  geom_bar(aes(x = chromosome_name), fill = "seagreen",
           position = "dodge", stat = "count") +
  labs(y = "log10(n genes)", x = "chromosome") +
  scale_y_log10()

count_gene_chromosome
```

<img src="fig/40-visualization-rendered-sub1-1.png" style="display: block; margin: auto;" />

以下では、
`legend.position` を `"none"` に設定することで、凡例を完全に削除することもできる。


``` r
exp_boxplot_sex <- ggplot(rna, aes(y=expression_log, x = as.factor(time),
                 color=sex)) +
   geom_boxplot(alpha = 0) +
  labs(y = "Mean gene exp",
       x = "time") + theme(legend.position = "none")

exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-sub2-1.png" style="display: block; margin: auto;" />

patchwork\*\*](https://github.com/thomasp85/patchwork)パッケージ
は、`+` を使って図形を組み合わせるエレガントなアプローチを提供し、
図形を（通常は横に並べて）並べます。 より具体的には、`|`
は明示的に横に並べ、`/`は
上に重ねる。


``` r
install.packages("patchwork")
```


``` r
library("patchwork")
count_gene_chromosome + exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-patchworkplot1-1.png" style="display: block; margin: auto;" />

``` r
## or count_gene_chromosome | exp_boxplot_sex
```


``` r
count_gene_chromosome / exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-patchwork2-1.png" style="display: block; margin: auto;" />

`plot_layout` と組み合わせることで、最終的なコンポジションのレイアウトをさらにコントロールし、より複雑なレイアウトを作成することができる：


``` r
count_gene_chromosome + exp_boxplot_sex + plot_layout(ncol = 1)
```

<img src="fig/40-visualization-rendered-patchwork3-1.png" style="display: block; margin: auto;" />


``` r
count_gene_chromosome +
 (count_gene_chromosome + exp_boxplot_sex) +
 exp_boxplot_sex +
 plot_layout(ncol = 1)
```

<img src="fig/40-visualization-rendered-patchwork4-1.png" style="display: block; margin: auto;" />

最後のプロットは `|` と `/` のコンポーザーを使っても作成できる：


``` r
count_gene_chromosome /
 (count_gene_chromosome | exp_boxplot_sex) /
 exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-patchwork5-1.png" style="display: block; margin: auto;" />

パッチワークの詳細については、
[ウェブページ](https://patchwork.data-imaginist.com/)、またはこちらの
[ビデオ](https://www.youtube.com/watch?v=0m4yywqNPVY)をご覧ください。

もう一つのオプションは \*\*gridExtra`** パッケージで、
の別々の ggplot を `grid.arrange()\` を使って一つの図にまとめることができます：


``` r
install.packages("gridExtra")
```


``` r
library("gridExtra")
grid.arrange(count_gene_chromosome, exp_boxplot_sex, ncol = 2)
```

<img src="fig/40-visualization-rendered-gridarrange-example-1.png" style="display: block; margin: auto;" />

引数 `ncol` と `nrow` は単純な
の配置を作るのに使われるが、それに加えて、より複雑な
のレイアウトを作る ためのツールもある。

## プロットのエクスポート

プロットを作成したら、お好きな
形式でファイルに保存できます。 RStudioの**Plot**ペインのExportタブでは、
のプロットが低解像度で保存されます。これは多くのジャーナルでは受け入れられませんし、
ではポスター用にうまく拡大縮小できません。

代わりに `ggsave()` 関数を使います。この関数を使うと、
の適切な引数（`width`, `height`, `dpi`）を調整することで、プロットの
次元と解像度を簡単に変更することができます。

作業ディレクトリに `fig_output/` フォルダがあることを確認してください。


``` r
my_plot <- ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  labs(title = "Mean gene expression by duration of the infection",
         x = "Duration of the infection (in days)",
         y = "Mean gene expression") +
  guides(color=guide_legend(title="Gender")) +
  theme_bw() +
  theme(axis.text.x = element_text(colour = "royalblue4", size = 12),
        axis.text.y = element_text(colour = "royalblue4", size = 12),
        text = element_text(size = 16),
        panel.grid = element_line(colour="lightsteelblue1"),
        legend.position = "top")
ggsave("fig_output/mean_exp_by_time_sex.png", my_plot, width = 15,
       height = 10)

# This also works for grid.arrange() plots
combo_plot <- grid.arrange(count_gene_chromosome, exp_boxplot_sex,
                           ncol = 2, widths = c(4, 6))
ggsave("fig_output/combo_plot_chromosome_sex.png", combo_plot,
       width = 10, dpi = 300)
```

注意: パラメータ `width` と `height` は、保存されたプロットのフォントサイズ
も決定します。



## その他のビジュアライゼーション用パッケージ

ggplot2\`は非常に強力なパッケージで、我々の_tidy
data_と_tidy tools_パイプラインにうまくフィットする。 Rには他にも無視できない可視化パッケージ
がある。

### ベースグラフィック

Rに付属するデフォルトのグラフィックス・システムは、しばしば_ベースR
グラフィックス_と呼ばれ、シンプルで高速である。 これは_画家またはキャンバスの
モデル_に基づいており、異なる出力がそれぞれ
互いに直接重ね合わされる（図@ref(fig:paintermodel)を参照）。 これは`ggplot2`（および後述する`lattice`）との基本的な
の違いである。
は専用のオブジェクトを返し、それは画面上またはファイル上にレンダリングされ、
は更新することさえできる。


``` r
par(mfrow = c(1, 3))
plot(1:20, main = "First layer, produced with plot(1:20)")

plot(1:20, main = "A horizontal red line, added with abline(h = 10)")
abline(h = 10, col = "red")

plot(1:20, main = "A rectangle, added with rect(5, 5, 15, 15)")
abline(h = 10, col = "red")
rect(5, 5, 15, 15, lwd = 3)
```

<div class="figure" style="text-align: center">
<img src="fig/40-visualization-rendered-paintermodel-1.png" alt="Successive layers added on top of each other."  />
<p class="caption">Successive layers added on top of each other.</p>
</div>

もうひとつの主な違いは、ベース・グラフィックスのプロット関数は、入力タイプに基づいて、
、_正しい_ことをしようとする。 これは、
データフレームしか入力として受け付けない`ggplot2`で、プロットをビットごとに構築する必要があるのとは、
異なる。


``` r
par(mfrow = c(2, 2))
boxplot(rnorm(100),
        main = "rnorm(100) の Boxplot")
boxplot(matrix(rnorm(100), ncol = 10),
        main = "matrix(rnorm(100), ncol = 10) の Boxplot")
hist(rnorm(100))
hist(matrix(rnorm(100), ncol = 10))
```

<div class="figure" style="text-align: center">
<img src="fig/40-visualization-rendered-plotmethod-1.png" alt="Plotting boxplots (top) and histograms (bottom) vectors (left) or a matrices (right)."  />
<p class="caption">Plotting boxplots (top) and histograms (bottom) vectors (left) or a matrices (right).</p>
</div>

、`plot`、`hist`、 `boxplot`、...のような1行のコードと1つの関数で非常に素早く作成できる。
しかし、デフォルトは必ずしも最も魅力的なものではありません。
、図形のチューニングは、特に複雑になると（例えばファセットを生成するために
）、時間がかかり面倒になります。

### 格子パッケージ

lattice`** パッケージは `ggplot2` と似ているが、
データフレームを入力として使い、グラフィカルオブジェクトを返し、ファセットをサポートする。
しかし、`lattice\`はグラフィックの文法に基づいておらず、
より複雑なインターフェイスを持っている。

lattice\`パッケージの良いリファレンスは@latticebookだ。

:::::::::::::::::::::::::::::::::::::::: keypoints

- Rによる可視化

::::::::::::::::::::::::::::::::::::::::::::::::::
