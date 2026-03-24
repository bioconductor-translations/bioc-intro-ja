---
source: Rmd
title: データの可視化
teaching: 60
exercises: 60
---



::::::::::::::::::::::::::::::::::::::: objectives

- ggplotを使用して散布図、箱ひげ図、折れ線グラフなどを作成できる
- プロットの共通設定を統一的に設定できる
- ファセット処理の概念を理解し、ggplotで適切に適用できる
- 既存のggplotプロットの外観要素（軸ラベルや色設定など）を変更可能である
- データフレーム内のデータから、複雑でカスタマイズ性の高いプロットを構築できる

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Rによる可視化

::::::::::::::::::::::::::::::::::::::::::::::::::



> このエピソードは、Data Carpentriesの_Data Analysis and
> Visualisation in R for Ecologists_レッスンに基づいています。

## データの可視化

まず必要なパッケージを読み込みます。**`ggplot2`**は**`tidyverse`**パッケージに含まれています。


``` r
library("tidyverse")
```

まだワークスペースに読み込まれていない場合は、前回のレッスンで保存したデータを読み込みます。


``` r
rna <- read.csv("data/rnaseq.csv")
```

[データ可視化チートシート](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-visualization.pdf)では、`ggplot2`の基本機能から高度な使い方までを網羅しており、参考資料としてだけでなく、パッケージで利用可能な多様なデータ表現方法を把握するのにも役立ちます。Thomas Lin Pedersen氏による以下の動画チュートリアル（[パート1](https://www.youtube.com/watch?v=h29g21z0a68)および[パート2](https://www.youtube.com/watch?v=0m4yywqNPVY)）も非常に参考になります。

## `ggplot2`を使ったプロット作成

`ggplot2`はデータフレーム内のデータから複雑なプロットを簡単に作成できる描画パッケージです。変数の指定方法、表示方法、一般的な視覚的特性など、よりプログラム的なインターフェースを提供します。`ggplot2`を支える理論的基盤は*グラフィックスの文法*（@Wilkinson:2005）です。このアプローチを採用すれば、基礎データが変更されたり、棒グラフから散布図に変更したりする場合でも、最小限の修正で済みます。これにより、調整や微調整を最小限に抑えつつ、出版物に使用できる品質のプロットを作成できます。

`ggplot2`に関する書籍（@ggplot2book）も出版されていますが、内容は古めです。現在第3版が準備中で、[無料でオンライン公開される予定](https://ggplot2-book.org/)です。`ggplot2`の公式ウェブサイト（[https://ggplot2.tidyverse.org](https://ggplot2.tidyverse.org)）には充実したドキュメントが用意されています。

`ggplot2`の関数は、データが「長形式」（各次元ごとに1列、各観測値ごとに1行）になっている場合に最適に動作します。適切に構造化されたデータを用意しておけば、`ggplot2`で図を作成する際に大幅な時間節約になります。

ggplotのグラフィックスは、新しい要素を段階的に追加することで構築されます。このようにレイヤーを追加していく方法により、プロットの表現に非常に柔軟性とカスタマイズ性が生まれます。

>グラフィックスの文法の背後にある考え方は、あらゆるグラフを以下の3つの基本要素から構築できるということです：(1) データセット、(2) 座標系、(3) ジオメトリ（データポイントを表現する視覚的マーク）[^three\_comp\_ggplot2]

[^three\_comp\_ggplot2]: 出典：[データ可視化チートシート](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-visualization.pdf)

ggplotを構築するには、以下の基本的なテンプレートを使用します。これはさまざまな種類のプロットに適用できる汎用的な形式です：

```
ggplot(data = <DATA>, mapping = aes(<MAPPINGS>)) +  <GEOM_FUNCTION>()
```

- `ggplot()`関数を使用し、`data`引数で特定の**データフレーム**にプロットを結合します


``` r
ggplot(data = rna)
```

- **マッピング**を定義します（`aes`関数を使用して、プロットする変数を選択し、グラフ内での表示方法を指定します。例えばx/y座標、サイズ、形状、色などの特性として表示します）


``` r
ggplot(data = rna, mapping = aes(x = expression))
```

- '**ジオメトリ**'を追加します - プロット内のデータの幾何学的表現（点、線、棒など）。`ggplot2`には多くの異なるジオメトリが用意されており、今日は特に一般的な以下のものを使用します：

  ```
  * `geom_point()` 散布図、ドットプロットなどに使用
  * `geom_histogram()` ヒストグラム作成に使用
  * `geom_boxplot()` ボックスプロット作成に使用
  * `geom_line()` トレンドライン、時系列データなどに使用
  ```

プロットにジオメトリを追加するには`+`演算子を使用します。まずは`geom_histogram()`を使ってみましょう：


``` r
ggplot(data = rna, mapping = aes(x = expression)) +
  geom_histogram()
```

``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="fig/40-visualization-rendered-first-ggplot-1.png" alt="ggplot()とgeom_histogram()で生成したexpressionデータのデフォルトヒストグラム" style="display: block; margin: auto;" />

`ggplot2`パッケージの`+`演算子は特に便利で、既存の`ggplot`オブジェクトを変更できます。つまり、プロットテンプレートを簡単に設定し、さまざまな種類のプロットを便利に探索できるため、上記のプロットは以下のようなコードでも生成可能です：


``` r
# プロットを変数に割り当てる
rna_plot <- ggplot(data = rna,
                   mapping = aes(x = expression))

# プロットを描画する
rna_plot + geom_histogram()
```


:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

ヒストグラムを描画する際に表示される自動メッセージにお気づきでしょうか？


``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

`geom_histogram()` の `bins` または `binwidth` 引数を変更することで、ビンの数や幅を調整できます。

:::::::::::::::  solution

## 解答


``` r
# ビン幅の変更
ggplot(rna, aes(x = expression)) +
    geom_histogram(bins = 15)
```

<img src="fig/40-visualization-rendered-unnamed-chunk-5-1.png" alt="ビン幅15の場合（上）とビン幅2000の場合（下）におけるggplot()とgeom_histogram()で生成された発現データのヒストグラム" style="display: block; margin: auto;" />

``` r
# ビン幅の変更
ggplot(rna, aes(x = expression)) +
    geom_histogram(binwidth = 2000)
```

<img src="fig/40-visualization-rendered-unnamed-chunk-5-2.png" alt="ビン幅15の場合（上）とビン幅2000の場合（下）におけるggplot()とgeom_histogram()で生成された発現データのヒストグラム" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

ここでデータが右に歪んでいることが確認できます。より対称的な分布を得るために、
対数変換（log2変換）を適用することができます。なお、式値が0の場合に`-Inf`値が
返されるのを防ぐため、ここでは小さな定数値（`+1`）を追加しています。


``` r
rna <- rna |>
  mutate(expression_log = log2(expression + 1))
```

次に、対数変換した発現量のヒストグラムを作成すると、確かに正規分布に近い分布が得られます。


``` r
ggplot(rna, aes(x = expression_log)) + geom_histogram()
```

``` output
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="fig/40-visualization-rendered-second-ggplot-1.png" alt="事前に計算した発現量の対数値に基づくヒストグラム（ggplot()とgeom_histogram()で生成）" style="display: block; margin: auto;" />

これ以降の分析では、対数変換した発現量の値を用いて作業を進めていきます。


:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

この変換を視覚的に理解する別の方法として、観測値のスケールを考慮する方法があります。例えば、軸のスケールを調整することで、プロット空間内での観測値の分布をより適切に調整できる場合があります。軸のスケール変更は、他のコンポーネントを追加・修正する場合と同様の手順で行います（つまり、段階的にコマンドを追加していく方法です）。以下の変更を試してみてください：

- 変換前の式を対数スケール（log10）で表示します。`scale_x_log10()` 関数を参照してください。以前のグラフと比較してください。なぜ今回は警告メッセージが表示されるのでしょうか？

:::::::::::::::  solution

## 解答


``` r
ggplot(data = rna, mapping = aes(x = expression))+
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

<img src="fig/40-visualization-rendered-unnamed-chunk-6-1.png" alt="発現量の対数に対する ggplot()、geom_histogram()、および scale_x_log10() で生成されたヒストグラム" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

**注意事項**

- `ggplot()`関数内で指定した設定は、後から追加するすべてのジオメトリレイヤーから参照されます（つまり、これらはグローバルなプロット設定となります）。これには`aes()`で設定したx軸・y軸のマッピングも含まれます。
- 特定のジオメトリレイヤーに対しては、`ggplot()`関数で定義したグローバル設定とは独立してマッピングを指定することも可能です。
- 新しいレイヤーを追加する際に使用する`+`記号は、必ず*直前の*レイヤーを含む行の末尾に配置する必要があります。もし新しいレイヤーを含む行の先頭に`+`記号を置いた場合、`ggplot2`は新しいレイヤーを追加せず、エラーメッセージを返します。


``` r
# これはレイヤーを追加する正しい構文です
rna_plot +
  geom_histogram()

# これは新しいレイヤーを追加せず、エラーメッセージを返します
rna_plot
  + geom_histogram()
```

## プロットを段階的に構築する方法

ここでは、2つの連続変数を用いて散布図を作成し、`geom_point()`関数で表現します。このグラフは、時間8時点と時間0時点、および時間4時点と時間0時点における発現量の対数2倍変化を表します。まず、遺伝子ごと・時間ごとに対数変換した発現量の平均値を計算し、次に時間8時点と時間0時点、時間4時点と時間0時点の平均値の対数差を計算して対数2倍変化を求めます。なお、後で遺伝子を分類するために使用する遺伝子のバイオタイプ情報もここで含めます。これらの変化量は新しいデータフレーム`rna_fc`に保存します。


``` r
rna_fc <- rna |> select(gene, time,
                         gene_biotype, expression_log) |>
  group_by(gene, time, gene_biotype) |>
  summarize(mean_exp = mean(expression_log)) |>
  pivot_wider(names_from = time,
              values_from = mean_exp) |>
  mutate(time_8_vs_0 = `8` - `0`, time_4_vs_0 = `4` - `0`)
```

``` output
`summarise()` has grouped output by 'gene', 'time'. You can override using the
`.groups` argument.
```

作成したデータフレーム`rna_fc`を用いてggplotを構築します。`ggplot2`でプロットを作成する際は通常、反復的なプロセスで行います。まず使用するデータセットを定義し、軸を設定し、使用するジオメトリを選択します：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point()
```

<img src="fig/40-visualization-rendered-create-ggplot-object-1.png" alt="上記で計算した対数2倍変化を比較する散布図。すべての点は黒色で表示されます" style="display: block; margin: auto;" />

次に、このプロットをさらに修正してより多くの情報を抽出します。例えば、重なりを避けるための透明度（`alpha`）を追加できます：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point(alpha = 0.3)
```

<img src="fig/40-visualization-rendered-adding-transparency-1.png" alt="上記で計算した対数2倍変化を比較する散布図。すべての点は半透明の黒色で表示されます" style="display: block; margin: auto;" />

すべての点に色を付けることも可能です：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point(alpha = 0.3, color = "blue")
```

<img src="fig/40-visualization-rendered-adding-colors-1.png" alt="上記で計算した対数2倍変化を比較する散布図。すべての点は半透明の青色で表示されます" style="display: block; margin: auto;" />

あるいは、プロット内の各遺伝子を異なる色で表示したい場合は、`color`引数にベクトルを指定します。`ggplot2`はこのベクトルの値に応じて異なる色を自動的に割り当てます。以下は`gene_biotype`で色分けする例です：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_point(alpha = 0.3, aes(color = gene_biotype))
```

<img src="fig/40-visualization-rendered-color-by-gene_biotype1-1.png" alt="上記で計算した対数2倍変化を比較する散布図。点の色は遺伝子のバイオタイプに基づいて色分けされています" style="display: block; margin: auto;" />

`ggplot()`関数内で直接マッピング時に色を指定することもできます。この設定はすべてのジオメトリレイヤーから参照され、`aes()`で設定したx軸・y軸のマッピングに基づいて決定されます。


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0,
                                color = gene_biotype)) +
  geom_point(alpha = 0.3)
```

<img src="fig/40-visualization-rendered-color-by-gene_biotype2-1.png" alt="上記で計算した対数2倍変化を比較する散布図。点の色は遺伝子のバイオタイプに基づいて色分けされています" style="display: block; margin: auto;" />

最後に、`geom_abline()`関数を使用して対角線を追加することも可能です：


``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0,
                                color = gene_biotype)) +
  geom_point(alpha = 0.3) +
  geom_abline(intercept = 0)
```

<img src="fig/40-visualization-rendered-adding-diag-1.png" alt="上記で計算した対数2倍変化を比較する散布図。点の色は遺伝子のバイオタイプに基づいて色分けされています。原点を通る傾き1の黒色の直線がgeom_abline()によって追加されています" style="display: block; margin: auto;" />

注目すべきは、ジオメトリレイヤーを`geom_point`から`geom_jitter`に変更しても、色は依然として`gene_biotype`によって決定される点です。



``` r
ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0,
                                color = gene_biotype)) +
  geom_jitter(alpha = 0.3) +
  geom_abline(intercept = 0)
```

<img src="fig/40-visualization-rendered-color-by-gene_biotype3-1.png" alt="Scatter plot produced by ggplot() and geom_point() comparing the log-foldchanges computed above. Dots are colour-coded based on the gene's biotype. A black line of slope 1 crossing the origin was added by geom_abline()." style="display: block; margin: auto;" />



:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

散布図は小規模なデータセットの探索的分析において有用なツールです。しかし、`rna_fc` データセットのように観測値数が多い場合、点の重なり（オーバープロット）が散布図の限界となることがあります。このような状況に対処するための有効な手法の一つが、観測値を六角形グリッドに分割するヘキサビン法です。

- `ggplot2` でヘキサビン法を使用するには、まず CRAN リポジトリから R パッケージ `hexbin` をインストールし、ロードする必要があります。

- 次に `geom_hex()` 関数を使用することで、ヘキサビンプロットを作成できます。

- ヘキサビンプロットと従来の散布図を比較した場合、それぞれの長所と短所は何でしょうか？上記の散布図と作成したヘキサビンプロットを詳細に比較・検討してください。

:::::::::::::::  solution

## 解答


``` r
install.packages("hexbin")
```


``` r
library("hexbin")

ggplot(data = rna_fc, mapping = aes(x = time_4_vs_0, y = time_8_vs_0)) +
  geom_hex() +
  geom_abline(intercept = 0)
```

<img src="fig/40-visualization-rendered-unnamed-chunk-10-1.png" alt="Scatter plot produced by ggplot() and geom_hexbin() comparing the log-foldchanges computed above shows hexagons coloured based on the underlying dot density. A black line of slope 1 crossing the origin was added by geom_abline()." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

これまでに学んだ知識を活用して、`rna` データセットから `sample` 列を選択し、`expression_log` 値を散布図としてプロットしてください。時間軸は異なる色で表示されるように設定します。このデータ表現方法はこの種のデータを示す上で適切な方法でしょうか？


:::::::::::::::  solution

## 解答


``` r
ggplot(data = rna, mapping = aes(y = expression_log, x = sample)) +
    geom_point(aes(color = time))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-11-1.png" alt="Figures showing a stretch of overlapping points indicating the log of expression + 1 for each sample. The points are coloured with different shades of blue for samples collected at different time points." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 箱ひげ図

箱ひげ図を用いることで、各サンプル内における遺伝子発現量の分布を視覚的に表現することができます：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_boxplot()
```

<img src="fig/40-visualization-rendered-boxplot-1.png" alt="geom_boxplot()によって生成された各サンプルの対数変換発現量+1値の分布を示す箱ひげ図。各箱ひげ図は白色で塗りつぶされています。" style="display: block; margin: auto;" />

箱ひげ図に点プロットを追加することで、測定値の総数とその分布状況をより明確に把握することが可能になります：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_jitter(alpha = 0.2, color = "tomato") +
  geom_boxplot(alpha = 0)
```

<img src="fig/40-visualization-rendered-boxplot-with-points-1.png" alt="geom_boxplot()によって生成された各サンプルの対数変換発現量+1値の分布を示す箱ひげ図と点プロット。各箱ひげ図は半透明で、ジッター処理された点プロットは半透過のトマト色で表示され、箱ひげ図の背面に配置されています。" style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

箱ひげ図のレイヤーがジッタープロットのレイヤーの前面に表示されていることに注目してください。箱ひげ図を点の下に表示されるようにするには、コードをどのように変更すればよいでしょうか？


:::::::::::::::  solution

## 解答

これら2つのジオメトリの順序を入れ替える必要があります：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_boxplot(alpha = 0) +
  geom_jitter(alpha = 0.2, color = "tomato")
```

<img src="fig/40-visualization-rendered-boxplot-with-points2-1.png" alt="Boxplot and dots showing the distribution of log expression + 1 values for each sample, as produced by geom_boxlpot(). Each boxplot is transparent and the jittered dots are semi-transparent tomato-coloured. This time, the boxplots are behind the dots." style="display: block; margin: auto;" />


:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

x軸上の値がまだ適切に読み取れないことにお気づきかもしれません。ラベルの表示方向を変更し、垂直方向と水平方向に調整して重なりが起こらないようにしましょう。90度の角度を使用するか、斜め向きのラベルに適した角度を見つけるために試行錯誤してみてください：


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_jitter(alpha = 0.2, color = "tomato") +
  geom_boxplot(alpha = 0) +
  theme(axis.text.x = element_text(angle = 90,  hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-boxplot-xaxis-rotated-1.png" alt="Boxplot and dots showing the distribution of log expression + 1 values for each sample, as produced by geom_boxlpot(). Each boxplot is transparent and the jittered dots are semi-transparent tomato-coloured. The sample labels are displayed vertically and readable." style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

箱ひげ図のデータポイントに、感染期間（`time`）に応じて色を付けてください。

*ヒント:* `time` 変数のクラスを確認してください。ggplot のマッピング処理内で `time` のクラスを整数型から因子型に直接変更することを検討してください。この変更が R でグラフを作成する方法にどのような影響を与えるのでしょうか？

:::::::::::::::  solution

## 解答


``` r
# time as integer
ggplot(data = rna,
         mapping = aes(y = expression_log,
                       x = sample)) +
  geom_jitter(alpha = 0.2, aes(color = time)) +
  geom_boxplot(alpha = 0) +
  theme(axis.text.x = element_text(angle = 90,  hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-boxplot-color-time-1.png" alt="Boxplot and dots showing the distribution of log expression + 1 values for each sample, as produced by geom_boxlpot(). On the first figure, each boxplot is transparent and the jittered dots are semi-transparent and coloured in different shares of blue. On the second figures, each boxplot is transparent and the jittered dots are semi-transparent and coloured red, green and blue." style="display: block; margin: auto;" />

``` r
# time as factor
ggplot(data = rna,
         mapping = aes(y = expression_log,
                       x = sample)) +
  geom_jitter(alpha = 0.2, aes(color = as.factor(time))) +
  geom_boxplot(alpha = 0) +
  theme(axis.text.x = element_text(angle = 90,  hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-boxplot-color-time-2.png" alt="Boxplot and dots showing the distribution of log expression + 1 values for each sample, as produced by geom_boxlpot(). On the first figure, each boxplot is transparent and the jittered dots are semi-transparent and coloured in different shares of blue. On the second figures, each boxplot is transparent and the jittered dots are semi-transparent and coloured red, green and blue." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

箱ひげ図は分布の概要を把握するのに有用ですが、分布の「形状」そのものは表示しません。例えば、分布が二峰性の場合、箱ひげ図ではその特徴を捉えることができません。箱ひげ図の代替手段として「バイオリンプロット」があり、これはデータ点の密度分布の形状を視覚的に表現します。

- 箱ひげ図をバイオリンプロットに置き換えてください。`geom_violin()`関数を使用します。`fill`引数を用いて、時間軸に沿ってバイオリンプロットを塗り分けてください。

:::::::::::::::  solution

## 解答


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_violin(aes(fill = as.factor(time))) +
  theme(axis.text.x = element_text(angle = 90,  hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-12-1.png" alt="Violin plot showing the distribution of log expression + 1 values for each sample, as produced by geom_violin(). Each boxplot is transparent and the jittered dots are semi-transparent and coloured red, green and blue." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

- バイオリンプロットを修正し、`性別`（sex）ごとにバイオリン部分を塗りつぶすようにしてください。

:::::::::::::::  solution

## 解答


``` r
ggplot(data = rna,
         mapping = aes(y = expression_log, x = sample)) +
  geom_violin(aes(fill = sex)) +
  theme(axis.text.x = element_text(angle = 90,  hjust = 0.5, vjust = 0.5))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-13-1.png" alt="Violin plot showing the distribution of log expression + 1 values for each sample, as produced by geom_violon(). Each violin plot is coloured in red or blue depending on the sex variable." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 折れ線グラフ

感染期間ごとの平均発現量を、時間8時点と時間0時点を比較した際の対数倍率変化が最大となる10遺伝子について計算します。まず対象となる遺伝子を選択し、`rna`データセットからこれら10遺伝子のみを含むサブセット`sub_rna`を作成します。その後、データをグループ化し、各グループ内の遺伝子発現量の平均値を算出します：


``` r
rna_fc <- rna_fc |> arrange(desc(time_8_vs_0))

genes_selected <- rna_fc$gene[1:10]

sub_rna <- rna |>
    filter(gene %in% genes_selected)

mean_exp_by_time <- sub_rna |>
  group_by(gene,time) |>
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

感染期間をx軸、平均発現量をy軸とした折れ線グラフを作成できます：


``` r
ggplot(data = mean_exp_by_time, mapping = aes(x = time, y = mean_exp)) +
  geom_line()
```

<img src="fig/40-visualization-rendered-first-time-series-1.png" alt="geom_line()で生成した折れ線グラフですが、実際のデータが期待通りの傾向を示していません" style="display: block; margin: auto;" />

残念ながらこの手法ではうまくいきません。すべての遺伝子のデータを一括してプロットしているためです。各遺伝子ごとに別々の線を描画するには、美的関数を`group = gene`に変更する必要があります：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp, group = gene)) +
  geom_line()
```

<img src="fig/40-visualization-rendered-time-series-by-gene-1.png" alt="geom_line()で生成した折れ線グラフで、10本の線がそれぞれ異なる遺伝子の時間経過に伴う発現量の増加を示しています" style="display: block; margin: auto;" />

色分けを追加すれば（`color`パラメータを使用すると自動的にデータがグループ化されます）、プロット上で各遺伝子を区別できるようになります：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp, color = gene)) +
  geom_line()
```

<img src="fig/40-visualization-rendered-time-series-with-colors-1.png" alt="geom_line()で生成した折れ線グラフで、10本の色分けされた線がそれぞれ異なる遺伝子の時間経過に伴う発現量の増加を示しています" style="display: block; margin: auto;" />

## 面分割表示

`ggplot2`には*面分割*と呼ばれる特殊な機能があり、データセットに含まれる因子に基づいて1つのプロットを複数のサブプロットに分割できます。これらのサブプロットは同じプロパティ（軸の範囲、目盛りなど）を継承するため、直接比較が容易になります。各遺伝子について時間経過に沿った折れ線グラフを作成する場合に活用しましょう：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp)) + geom_line() +
  facet_wrap(~ gene)
```

<img src="fig/40-visualization-rendered-first-facet-1.png" alt="geom_line()で生成した折れ線グラフで、10個のサブプロット/面分割があり、それぞれが時間経過に伴う発現量の増加を示す1本の線を表示しています。すべてのy軸スケールは同一です" style="display: block; margin: auto;" />

ここではすべてのサブプロットにおいて、x軸とy軸が同じスケールになっています。このデフォルト設定を変更するには、`scales`パラメータを調整してy軸のスケールを自由に設定可能にできます：


``` r
ggplot(data = mean_exp_by_time,
       mapping = aes(x = time, y = mean_exp)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y")
```

<img src="fig/40-visualization-rendered-first-facet-scales-1.png" alt="geom_line()で生成した折れ線グラフで、10個のサブプロット/面分割があり、それぞれが時間経過に伴う発現量の増加を示す1本の線を表示しています。各面分割/遺伝子ごとにy軸スケールが発現量の範囲に合わせて調整されています" style="display: block; margin: auto;" />

次に、各プロット内の線をマウスの性別でさらに分割したいとします。そのためには、`gene`、`time`、`sex`でグループ化したデータフレーム内で平均発現量を計算する必要があります：


``` r
mean_exp_by_time_sex <- sub_rna |>
  group_by(gene, time, sex) |>
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

これで、`color`パラメータを使用して単一プロット内で性別ごとにさらに分割した面分割グラフを作成できます：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y")
```

<img src="fig/40-visualization-rendered-facet-by-gene-and-sex-1.png" alt="geom_line()で生成した折れ線グラフで、10個のサブプロット/面分割があり、それぞれが時間経過に伴う発現量の増加を示す2本の色分けされた線（メスは赤、オスは青）を表示しています" style="display: block; margin: auto;" />

通常、白背景のプロットは印刷時に視認性が高くなります。`theme_bw()`関数を使用して背景を白に設定できます。さらに、グリッドラインも削除可能です：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank())
```

<img src="fig/40-visualization-rendered-facet-by-gene-and-sex-white-bg-1.png" alt="geom_line()で生成した折れ線グラフで、10個のサブプロット/面分割があり、それぞれが時間経過に伴う発現量の増加を示す2本の色分けされた線（メスは赤、オスは青）を表示しています。図の背景が白になっています" style="display: block; margin: auto;" />


:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

これまでに学んだ知識を活用して、感染経過に伴う各染色体の平均発現量の変化を視覚的に表現するプロットを作成してください。


:::::::::::::::  solution

## 解答


``` r
mean_exp_by_chromosome <- rna |>
  group_by(chromosome_name, time) |>
  summarize(mean_exp = mean(expression_log))
```

``` output
`summarise()` has grouped output by 'chromosome_name'. You can override using
the `.groups` argument.
```

``` r
ggplot(data = mean_exp_by_chromosome, mapping = aes(x = time,
                                y = mean_exp)) +
  geom_line() +
  facet_wrap(~ chromosome_name, scales = "free_y")
```

<img src="fig/40-visualization-rendered-mean-exp-chromosome-time-series-1.png" alt="Line plot, as produced by geom_line(), with 21 sub-plots/facets, each showing one line with expression values over time for each chromosome." style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

`facet_wrap`ジオメトリはプロットを任意の数の次元に分割し、1ページにきれいに収まるようにします。一方、`facet_grid`ジオメトリでは、数式表記（`rows ~ columns`; 単一の行または列を示すプレースホルダーとして`.`を使用可）を用いて、プロットの配置方法を明示的に指定できます。

以前のプロットを修正して、性別ごとの遺伝子発現の平均値が時間経過とともにどのように変化したかを比較してみましょう：


``` r
# 1列レイアウト、行単位で分割
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = gene)) +
  geom_line() +
  facet_grid(sex ~ .)
```

<img src="fig/40-visualization-rendered-mean-exp-time-facet-sex-rows-1.png" alt="2つの折れ線グラフが上下に配置され、それぞれ遺伝子ごとに10本の線が色分けされています。上部のサブプロットは女性サンプルの発現値を、下部のサブプロットは男性サンプルの発現値を示しています" style="display: block; margin: auto;" />


``` r
# 1行レイアウト、列単位で分割
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = gene)) +
  geom_line() +
  facet_grid(. ~ sex)
```

<img src="fig/40-visualization-rendered-mean-exp-time-facet-sex-columns-1.png" alt="2つの折れ線グラフが左右に配置され、それぞれ遺伝子ごとに10本の線が色分けされています。左側のサブプロットは女性サンプルの発現値を、右側のサブプロットは男性サンプルの発現値を示しています" style="display: block; margin: auto;" />

## `ggplot2`テーマ

プロットの背景色を白に変更する`theme_bw()`に加え、`ggplot2`には視覚化の外観を素早く変更できる他のテーマも用意されています。利用可能なテーマの完全なリストは[https://ggplot2.tidyverse.org/reference/ggtheme.html](https://ggplot2.tidyverse.org/reference/ggtheme.html)で確認できます。`theme_minimal()`と`theme_light()`は人気の高いテーマで、`theme_void()`は独自のテーマを作成する際の出発点として便利です。

[ggthemes](https://jrnold.github.io/ggthemes/reference/index.html)パッケージでは、Excel 2003テーマを含む多様なオプションが提供されています。[`ggplot2`拡張機能
ウェブサイト](https://exts.ggplot2.tidyverse.org/)では、`ggplot2`の機能を拡張するパッケージの一覧が掲載されており、追加のテーマも含まれています。

## カスタマイズ

時間と遺伝子ごとの平均発現量を性別で分割したプロットに戻りましょう。色分けは性別で行います。

[`ggplot2`チートシート](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-visualization.pdf)を参照し、プロットを改善する方法を考えてみてください。

次に、「時間」と「mean_exp」という単純な軸名をより情報量の多い表現に変更し、図にタイトルを追加しましょう：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  labs(title = "感染期間別の遺伝子発現平均値",
       x = "感染期間（日数）",
       y = "平均遺伝子発現値")
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-right-labels-1.png" alt="白背景の折れ線グラフで、カスタムタイトルと軸ラベルが表示されています" style="display: block; margin: auto;" />

軸名はより情報量が増えましたが、可読性をさらに向上させるためにフォントサイズを大きくすることができます：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  labs(title = "感染期間別の遺伝子発現平均値",
       x = "感染期間（日数）",
       y = "平均遺伝子発現値")  +
  theme(text = element_text(size = 16))
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-right-labels-xfont-size-1.png" alt="白背景の折れ線グラフで、カスタムタイトルと軸ラベルのフォントサイズが拡大されています" style="display: block; margin: auto;" />

なお、プロットのフォントを変更することも可能です。Windows環境では、[**`extrafont`**
パッケージ](https://cran.r-project.org/web/packages/extrafont/index.html)をインストールする必要がある場合があります。

さらに、x軸・y軸のテキスト色、グリッドの色などをカスタマイズできます。例えば、`legend.position`を`"top"`に設定することで凡例を上部に移動させることも可能です。


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  labs(title = "感染期間別の遺伝子発現平均値",
       x = "感染期間（日数）",
       y = "平均遺伝子発現値")  +
  theme(text = element_text(size = 16),
        axis.text.x = element_text(colour = "royalblue4", size = 12),
        axis.text.y = element_text(colour = "royalblue4", size = 12),
        panel.grid = element_line(colour="lightsteelblue1"),
        legend.position = "top")
```

<img src="fig/40-visualization-rendered-mean_exp-time-with-theme-1.png" alt="白背景の折れ線グラフで、カスタムタイトルと軸ラベルのフォントサイズが拡大され、グリッドが青色になっています" style="display: block; margin: auto;" />

作成したカスタマイズテーマがデフォルトテーマよりも優れていると判断した場合、
このテーマをオブジェクトとして保存しておけば、今後作成する他のプロットに簡単に適用できます。以下に、以前に作成したヒストグラムを例に説明します。


``` r
blue_theme <- theme(axis.text.x = element_text(colour = "royalblue4",
                                               size = 12),
                    axis.text.y = element_text(colour = "royalblue4",
                                               size = 12),
                    text = element_text(size = 16),
                    panel.grid = element_line(colour="lightsteelblue1"))

ggplot(rna, aes(x = expression_log)) +
  geom_histogram(bins = 20) +
    blue_theme()
```

``` error
Error in blue_theme(): could not find function "blue_theme"
```

:::::::::::::::::::::::::::::::::::::::  challenge

## 課題

ここまでの情報を踏まえ、この演習で生成されたプロットのいずれかを改善するか、
あるいはあなた自身の美しいグラフを作成するのにさらに5分間取り組んでください。
インスピレーションを得るために、RStudioの[`ggplot2`チートシート](https://github.com/rstudio/cheatsheets/raw/master/data-visualization-2.1.pdf)
を参照してください。以下にいくつかのアイデアをご紹介します：

- 線の太さを変更する方法を試してみてください。
- 凡例の名称を変更する方法はありますか？ラベルの変更はどうでしょう？
  （ヒント：`scale_`で始まる`ggplot`関数を探してみてください）
- 異なるカラーパレットを使用するか、線の色を手動で指定する方法を試してみましょう
  （詳細は[https://www.cookbook-r.com/Graphs/Colors_(ggplot2)/](https://www.cookbook-r.com/Graphs/Colors_(ggplot2)/)をご覧ください）。


:::::::::::::::  solution

## 解答

例えば、以下のプロットに基づいて：


``` r
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank())
```

<img src="fig/40-visualization-rendered-unnamed-chunk-15-1.png" alt="theme_bw()と空白のグリッド上に生成されたシンプルな面グラフ付き折れ線プロット" style="display: block; margin: auto;" />

このプロットは以下の方法でカスタマイズ可能です：


``` r
# 線の太さを変更
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
This warning is displayed once per session.
Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
generated.
```

<img src="fig/40-visualization-rendered-unnamed-chunk-16-1.png" alt="theme_bw()と幅広のグリッド線を使用したシンプルな面グラフ付き折れ線プロット" style="display: block; margin: auto;" />


``` r
# 凡例名とラベルの名称を変更
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  scale_color_discrete(name = "性別", labels = c("F", "M"))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-17-1.png" alt="theme_bw()と名称変更された色ラベルを使用したシンプルな面グラフ付き折れ線プロット" style="display: block; margin: auto;" />


``` r
# 異なるカラーパレットを使用
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  scale_color_brewer(name = "性別", labels = c("F", "M"), palette = "Dark2")
```

<img src="fig/40-visualization-rendered-unnamed-chunk-18-1.png" alt="異なるカラーパレットと名称変更された色ラベルを使用したtheme_bw()と空白のグリッド上のシンプルな面グラフ付き折れ線プロット" style="display: block; margin: auto;" />


``` r
# 色を手動で指定
ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  theme_bw() +
  theme(panel.grid = element_blank()) +
  scale_color_manual(name = "性別",  labels = c("F", "M"),
                     values = c("royalblue", "deeppink"))
```

<img src="fig/40-visualization-rendered-unnamed-chunk-19-1.png" alt="手動で設定した色と名称変更された色ラベルを使用したtheme_bw()と空白のグリッド上のシンプルな面グラフ付き折れ線プロット" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## 複数プロットの配置方法

ファセット機能は1つのプロットを複数のサブプロットに分割する強力なツールですが、時には異なる変数やデータフレームに基づく独立した複数のプロットを1つの図にまとめたい場合もあります。

まず、横に並べて表示したい2つのプロットを作成しましょう：

最初のグラフは各染色体ごとのユニークな遺伝子数をカウントしたものです。まず`chromosome_name`のレベル順序を並べ替え、各染色体ごとのユニークな遺伝子をフィルタリングします。また、読みやすさを向上させるため、y軸のスケールを対数10スケールに変更します。


``` r
rna$chromosome_name <- factor(rna$chromosome_name,
                               levels = c(1:19,"X","Y"))

count_gene_chromosome <- rna |>
  select(chromosome_name, gene) |>
  distinct() |> ggplot() +
  geom_bar(aes(x = chromosome_name), fill = "seagreen",
           position = "dodge", stat = "count") +
  labs(y = "log10(遺伝子数)", x = "染色体") +
  scale_y_log10()

count_gene_chromosome
```

<img src="fig/40-visualization-rendered-sub1-1.png" alt="染色体ごとの遺伝子数を対数10スケールで表示したシンプルなヒストグラム（棒グラフ）" style="display: block; margin: auto;" />

次に、`legend.position`を`"none"`に設定することで、凡例を完全に非表示にします。


``` r
exp_boxplot_sex <- ggplot(rna, aes(y=expression_log, x = as.factor(time),
                 color=sex)) +
   geom_boxplot(alpha = 0) +
  labs(y = "平均遺伝子発現値",
       x = "時間") + theme(legend.position = "none")

exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-sub2-1.png" alt="異なる時間ポイントにおける雌雄別の発現値を示す、透明度を持たせた赤と青の箱ひげ図" style="display: block; margin: auto;" />

[**patchwork**](https://github.com/thomasp85/patchwork)パッケージは、`+`演算子を使用して図を整然と配置する洗練された方法を提供します。具体的には、`|`演算子で図を左右に並べ、`/`演算子で上下に重ねることが可能です。


``` r
install.packages("patchwork")
```


``` r
library("patchwork")
count_gene_chromosome + exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-patchworkplot1-1.png" alt="左側にヒストグラム、右側に箱ひげ図を配置した合成図" style="display: block; margin: auto;" />

``` r
## または count_gene_chromosome | exp_boxplot_sex と記述することも可能
```


``` r
count_gene_chromosome / exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-patchwork2-1.png" alt="上部にヒストグラム、下部に箱ひげ図を配置した合成図" style="display: block; margin: auto;" />

`plot_layout`関数を使えば、さらに詳細なレイアウト制御が可能で、より複雑な配置も実現できます：


``` r
count_gene_chromosome + exp_boxplot_sex + plot_layout(ncol = 1)
```

<img src="fig/40-visualization-rendered-patchwork3-1.png" alt="上部にヒストグラム、下部に箱ひげ図を配置した合成図" style="display: block; margin: auto;" />


``` r
count_gene_chromosome +
 (count_gene_chromosome + exp_boxplot_sex) +
 exp_boxplot_sex +
 plot_layout(ncol = 1)
```

<img src="fig/40-visualization-rendered-patchwork4-1.png" alt="上部にヒストグラム、中央にヒストグラムと箱ひげ図の組み合わせ、左右に並べて表示、下部に箱ひげ図を配置した4プロット構成の図" style="display: block; margin: auto;" />

最後のプロット構成は、`|`と`/`の配置演算子を使っても作成できます：


``` r
count_gene_chromosome /
 (count_gene_chromosome | exp_boxplot_sex) /
 exp_boxplot_sex
```

<img src="fig/40-visualization-rendered-patchwork5-1.png" alt="上記と同様、上部にヒストグラム、中央にヒストグラムと箱ひげ図の組み合わせ、左右に並べて表示、下部に箱ひげ図を配置した4プロット構成の図" style="display: block; margin: auto;" />

`patchwork`についてさらに詳しく知りたい場合は、[公式ウェブサイト](https://patchwork.data-imaginist.com/)や[この解説動画](https://www.youtube.com/watch?v=0m4yywqNPVY)をご覧ください。

別の選択肢として、**`gridExtra`**パッケージを使用する方法もあります。このパッケージでは`grid.arrange()`関数を使って個別のggplotプロットを1つの図に統合できます：


``` r
install.packages("gridExtra")
```


``` r
library("gridExtra")
grid.arrange(count_gene_chromosome, exp_boxplot_sex, ncol = 2)
```

<img src="fig/40-visualization-rendered-gridarrange-example-1.png" alt="左側にヒストグラム、右側に箱ひげ図を配置した合成図" style="display: block; margin: auto;" />

`ncol`と`nrow`引数を使った基本的な配置に加え、[より複雑なレイアウト構成](https://cran.r-project.org/web/packages/gridExtra/vignettes/arrangeGrob.html)を実現するツールも用意されています。

## プロットのエクスポート方法

プロットの作成が完了したら、お好みの形式でファイルに保存できます。RStudioの「プロット」ペインにある「エクスポート」タブを使用すると、低解像度でプロットを保存できますが、多くの学術誌では受理されず、ポスター用の拡大表示にも適さない場合があります。

代わりに、`ggsave()`関数を使用することをお勧めします。この関数では、適切な引数（`width`、`height`、`dpi`）を調整することで、プロットの寸法と解像度を簡単に変更できます。

作業ディレクトリに`fig_output/`フォルダが作成されていることを確認してください。


``` r
my_plot <- ggplot(data = mean_exp_by_time_sex,
       mapping = aes(x = time, y = mean_exp, color = sex)) +
  geom_line() +
  facet_wrap(~ gene, scales = "free_y") +
  labs(title = "感染期間別の平均遺伝子発現量",
         x = "感染期間（日数）",
         y = "平均遺伝子発現量") +
  guides(color=guide_legend(title="性別")) +
  theme_bw() +
  theme(axis.text.x = element_text(colour = "royalblue4", size = 12),
        axis.text.y = element_text(colour = "royalblue4", size = 12),
        text = element_text(size = 16),
        panel.grid = element_line(colour="lightsteelblue1"),
        legend.position = "top")
ggsave("fig_output/mean_exp_by_time_sex.png", my_plot, width = 15,
       height = 10)

# grid.arrange()で作成したプロットも同様に保存可能です
combo_plot <- grid.arrange(count_gene_chromosome, exp_boxplot_sex,
                           ncol = 2, widths = c(4, 6))
ggsave("fig_output/combo_plot_chromosome_sex.png", combo_plot,
       width = 10, dpi = 300)
```

注意：`width`と`height`のパラメータは、保存されるプロットのフォントサイズも決定します。



## 可視化のためのその他のパッケージ

`ggplot2`は非常に強力なパッケージであり、私たちの「クリーンなデータ」と「クリーンなツール」のワークフローに非常によく適合します。Rには他に無視できない可視化パッケージも存在します。

### 基本グラフィックス

Rに標準で付属しているグラフィックスシステム、いわゆる「基本Rグラフィックス」はシンプルで高速です。これは「画家モデル」または「キャンバスモデル」に基づいており、異なる出力が直接重ね合わされます（図@ref(fig:paintermodel)参照）。これは`ggplot2`（および後述する`lattice`）とは根本的な違いがあり、`ggplot2`は画面やファイルにレンダリングされる専用オブジェクトを返すため、さらに更新することも可能です。


``` r
par(mfrow = c(1, 3))
plot(1:20, main = "最初のレイヤー（plot(1:20)で作成）")

plot(1:20, main = "h = 10で追加した水平赤色線")
abline(h = 10, col = "red")

plot(1:20, main = "rect(5, 5, 15, 15)で追加した長方形")
abline(h = 10, col = "red")
rect(5, 5, 15, 15, lwd = 3)
```

<div class="figure" style="text-align: center">
<img src="fig/40-visualization-rendered-paintermodel-1.png" alt="左から右へ、対角線上に20個の空の点、その上に垂直な赤色の線、さらにその上にプロット中央の長方形を重ねた「基本」グラフィックスの比較図"  />
<p class="caption">順次重ね合わされるレイヤー</p>
</div>

もう一つの重要な違いは、基本グラフィックスのプロット関数が入力データの種類に基づいて「適切な」処理を試みる点です。つまり、入力データのクラスに応じて動作を適応させます。これは`ggplot2`とは大きく異なり、`ggplot2`はデータフレームのみを入力として受け付け、プロットを段階的に構築していく必要があります。


``` r
par(mfrow = c(2, 2))
boxplot(rnorm(100),
        main = "rnorm(100)のボックスプロット")
boxplot(matrix(rnorm(100), ncol = 10),
        main = "matrix(rnorm(100), ncol = 10)のボックスプロット")
hist(rnorm(100))
hist(matrix(rnorm(100), ncol = 10))
```

<div class="figure" style="text-align: center">
<img src="fig/40-visualization-rendered-plotmethod-1.png" alt="上部に2つのボックスプロット、下部に2つのヒストグラムを配置した2×2の構成図"  />
<p class="caption">ボックスプロット（上）とヒストグラム（下）をベクトルデータ（左）または行列データ（右）で作成する例</p>
</div>

基本グラフィックスのデフォルト設定は、シンプルで標準的な図を素早く作成する場合に非常に効率的です。`plot`、`hist`、`boxplot`などの単一の関数と1行のコードで迅速に生成できます。ただし、デフォルト設定が常に最も見栄えが良いとは限らず、特に複雑な図（例えばファセット表示を作成する場合など）では、調整に時間がかかり煩雑になることがあります。

### latticeパッケージ

**`lattice`**パッケージは、入力としてデータフレームを使用し、グラフィックオブジェクトを返し、ファセット表示をサポートする点で`ggplot2`と類似しています。ただし、`lattice`はグラフィックス文法に基づいておらず、インターフェースがより複雑です。

`lattice`パッケージに関する優れたリファレンスとして、@latticebookを参照してください。


:::::::::::::::::::::::::::::::::::::::: keypoints

- Rによる可視化

::::::::::::::::::::::::::::::::::::::::::::::::::
