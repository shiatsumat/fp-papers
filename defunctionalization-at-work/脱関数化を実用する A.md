## <a name="sectionA">A 高階のマッチャーの正当性証明</a>

[§5.3.1](脱関数化を実用する 5#section5-3-1) で与えられた正当性の判断基準を証明するには、以下の性質が、任意の正規表現`r`と文字列（つまり文字のリスト）`s`と文字列からブール値への関数のうち`s`の任意の接尾辞について停止する`k`について、成り立っているということを証明すれば十分である。

* `accept (r, s, k)`の評価が停止し、かつ `accept (r, s, k)` ~> `True` ⇔ `s` ∈ ***L***(`r`)***L***(`k`) である。

ただし、「文字列受容関数」`k`の言語 ***L***(`k`) を集合 {`s` | `k s` ~> `True`} として定義している。

この証明は正規表現`r`における構造的帰納法である。それぞれの場合について、`k`と`s`は、`s`の任意の接尾辞について`k`が停止するという性質を満たしているものとする。

### (i) `r` = `Zero` の場合

`accept (r, s, k)` を評価すると、ただちに`False`となり停止する。

同値性は、`accept (r, s, k)` ~> `False` より左辺が偽であり、***L***(`r`) = ∅ より右辺も偽であり、両辺が偽であることから成立している。

### (ii) `r` = `One` の場合

`k`は`s`自体を含む`s`の任意の接尾辞で停止するので、`accept (r, s, k)`の評価は停止する。

また、`accept (One, s, k)` ≡ `k (s)` ~> `True` は定義より `s` ∈ ***L***(`k`) と等価であり ***L***(`r`)***L***(`k`) = ***L***(`k`) である。

### (iii) `r` = `Char c` の場合

`accept (r, s, k)`の評価は、ただちに`False`を生むか、`k`を`s`のある接尾辞とともに呼び出すので、停止する。

`s` ∈ ***L***(`Char c`)***L***(`k`) = {`[c]`}***L***(`k`) であるとき、またそのときに限り、`s`=`c : s'` かつ `s'` ∈ ***L***(`k`) であるような`s'`が存在する。

定義により、`s'` ∈ ***L***(`k`) は `k (s')` ~> `True` と同値であり、`s` = `c : s'` かつ `k (s')` ~> `True` である条件は、まさに `accept (r, s, k)` ~> `True` であるのだ。

### (iv) `r` = `Sum r1 r2` の場合

帰納法の仮定より`accept (r1, s, k)`と`accept(r2, s, k)`の両者が停止することが知られているので、`accept (r, s, k)`を評価すると停止する。

`s` ∈ ***L***(`r`)***L***(`k`) という条件は、`s` ∈ ***L***(`r1`)***L***(`k`) ∨ `s` ∈ ***L***(`r2`)***L***(`k`) と同値である。

帰納法の仮定より、`s` ∈ ***L***(`r1`)***L***(`k`) ⇔ `accept (r1, s, k)` ~> `True` かつ `s` ∈ ***L***(`r2`)***L***(`k`) ⇔ `accept (r2, s, k)` ~> `True` であり、`accept`の両方の適用は停止する。これらの条件を合わせると、

* `accept (r1, s, k) || accept (r2, s, k)` ~> `True`

が推論され、さらにこれは `accept (r, s, k)` ~> `True` と同値である。

### (v) `r` = `Cat r1 r2` の場合

`s`が`s`の接尾辞であるとき帰納法の仮定より`accept (r2, s', k)`を評価すると停止するということが分かる（`k`は`s'`の任意の接尾辞について停止するので`s`の任意の接尾辞について停止する）ので、`accept (r, s, k)`を評価すると停止する。したがって、関数`\s' -> accept (r2, s', k)`は`s`の任意の接尾辞について停止する。この場合、帰納法の仮定より`accept (r1, s, \s' -> ...)`が停止すると分かる。

また、`s` ∈ ***L***(`r`)***L***(`k`) は `s` ∈ ***L***(`r1`)***L***(`r2`)***L***(`k`) と同値である（言語の連結は結合性がある (associate) のだ）。

`k'`を `\s' -> accept (r2, s', k)` としよう。このとき、帰納法の仮定より、`k'`は`s`の接尾辞である引数に対して停止する。なぜなら`k`自体そうであり、なおかつ `k s'` ~> `True` は `accept (r2, s', k)` ~> `True` と同値であり、帰納法の仮定よりこれはちょうど `s` ∈ ***L***(`r2`)***L***(`k`) のとき成り立つからである。帰納法の仮定では、`s` ∈ ***L***(`r1`)***L***(`k'`) ~> ***L***(`r1`)***L***(`r2`)***L***(`k`) ⇔ `accept (r1, s, k')` ~> `True` であり、これはちょうど `accept (r, s, k)` ~> `True` が成り立つときに成り立つ。

### (vi) `r` = `Star r1` の場合

この場合 `s` ∈ ***L***(`r`)***L***(`k`) は `s` ∈ ***L***(`r1`)<sup>＊</sup>***L***(`k`) と同値であり、`accept (r, s, k)` ~> `True` は `accept_star (r1, s, k)` ~> `True` と同値である。ゆえに、これを示したい。

* `accept_star (r1, s, k)`を評価すると停止し、かつ `s` ∈ ***L***(`r1`)<sup>＊</sup>***L***(`k`) ⇔ `accept_star (r1, s, k)` ~> `True` である

この論理包含を両方向で証明していこう。

#### 〈⇒ の方向〉

（任意の文字列`s`について）以下のことを示したい。

* `s` ∈ ***L***(`r1`)***L***(`k`) ⇒ `accept_star (r1, s, k)` ~> `True`

これを示すには同値な

* ∃n. `s` ∈ ***L***(`r1`)<sup>n</sup>***L***(`k`) ⇒ `accept_star (r1, s, k)` ~> `True`

を n についての累積帰納法 (course-of-values induction)によって示す。

##### (a) n = 0 のとき

`s` ∈ ***L***(`r1`)<sup>0</sup>***L***(`k`) = ***L***(`k`) であり、その場合 `k s` ~> `True` であるので、`accept_star (r1, s, k)` ~> `True` である。

##### (b) n > 0 のとき

`s` ∈ ***L***(`r1`)<sup>n</sup>***L***(`k`) であるので、n がこの性質を満たす最小の数であるか、`s` ∈ ***L***(`r1`)<sup>m</sup>***L***(`k`) を満たす m<n が存在する。

もしそのような m があるならば帰納法の仮定より直ちに `accept_star (r1, s, k)` ~> `True` である。

もし n が最小の数であれば、`s` ∉ ***L***(`k`) であると分かるので、停止すると仮定しているので `k s` ~> `False` が言える。また、`s` ~> `x ++ y`, `x` ∈ ***L***(`r1`)<sup>n</sup>, `y` ∈ ***L***(`k`) であるような`x`,`y`が存在する。 ***L***(`r1`)<sup>n</sup> = ***L***(`r1`)***L***(`r1`)<sup>n-1</sup> なので、またしても `x1` ∈ ***L***(`r1`) かつ `x2` ∈ ***L***(`r1`)<sup>n-1</sup> となるように`x1`を`x = x1 ++ x2`と分けることができる。n は最小なので、`x` ∉ ***L***(`r1`)<sup>n-1</sup> であることが分かり、`x1`が空文字列でないと分かる。

そして `x2 ++ y` ∈ ***L***(`r1`)<sup>n-1</sup>***L***(`k`) であるので、帰納法の仮定から `accept_star (r1, x2 ++ y, k)` ~> `True` が導ける。そして`x1`は空文字列なので、`x2 ++ y` ≠ `s` である。ここで

* `k'` = `\s' -> s/=s' && accept_star (r1, s', k)`

とすると、`x2 ++ y` ∈ ***L***(`k'`) であるので、元々の帰納法の仮定から `accept_star (r1, s, k')` ~> `True` であり、つまり `accept_star (r1, s, k)` ~> `True` である。

#### 〈停止性と ⇐ の方向〉

この場合に対する証明は、`s`の構造における整礎帰納法によるものである。ここで順序は、文字列はその接尾辞以上である（そして真の接尾辞 (proper suffix) よりも真に大きい）というものである。

`accept_star (r1, s, k)` の評価は停止する。なぜなら、`k s` が `k` についての仮定より停止し、さらに `s'` の任意の接尾辞について帰納法の仮定より `accept_star (r1, s', k)` が停止するので、関数`(\s' -> s/= s' && accept_star (r1, s', k))`が`s`の任意の接尾辞について停止し、それゆえに外側の帰納法の仮定より`accept (r1, s, \s' -> ...)`が停止するからである。

さて、`accept_star (r1, s, k)` ~> `True` と仮定しよう。すると次のどちらかになる。

##### (a) `k s` ~>`True` の場合

`s` ∈ ***L***(`k`) ⊂ ***L***(`Star r1`)***L***(`k`) である。

##### (b) `k s` ~>`False` かつ `accept (r1, s, \s' -> ...)` の場合

`k'` を以下のように定義しよう。

* `k'` = `\s' -> s/=s' && accept_star (r1, s', k)`

すると、`k'`は`s`の任意の接尾辞について停止する。`s`自体については偽へと評価され（リストの比較は常に停止する）、すべての真の接尾辞`s'`については帰納法の仮定より `accept_star (r1, s', k)` の評価が停止する。

この場合構造帰納法の仮定より `s` ∈ ***L***(`r1`)***L***(`k'`) であると分かる。

定義によりこれは `s` = `x ++ y`, `x` ∈ ***L***(`r1`), `y` ∈ ***L***(`r2`) であるような `x`,`y` が存在する。（つまり、）`s /= y` ~> `True` かつ `accept_star (r1, y, k)` ~> `True` である。

`y` ≠ `s` であるので、`y`は`s`の真の接尾辞である（そして`x`は空文字列ではない）。その場合、整礎帰納法の仮定より `y` ∈ ***L***(`r1`)<sup>＊</sup>***L***(`k`) であると分かり、以下を証明したことになる。

* `s` = `x ++ y` ∈ ***L***(`r1`)***L***(`r1`)<sup>＊</sup>***L***(`k`) ⊂ ***L***(`r1`)<sup>＊</sup>***L***(`k`)

<div align="right">（証明終わり）</div>
