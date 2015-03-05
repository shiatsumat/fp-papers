## <a name="sectionB">B 一階のマッチャーの正当性証明</a>

[§5.3.2](脱関数化を実用する 5#section5-3-2) で与えられた正当性の判断基準を証明するために、

* *P<sub>1</sub>*(`r`,`s`,`k`) ≜ `accept (r, s, k)` ~> `True` ⇔ `s` ∈ ***L***(`r`)***L***(`k`)
* *P<sub>2</sub>*(`k`,`s`) ≜ `pop_and_accept (k, s)` ~> `True` ⇔ `s` ∈ ***L***(`r`)***L***(`k`)
* *P<sub>3</sub>*(`r`,`s`,`k`) ≜ `accept_star (r, s, k)` ~> `True` ⇔ `s` ∈ ***L***(`r`)<sup>＊</sup>***L***(`k`)

とし、相互整礎帰納法によってそれぞれの命題に尺度を与え、どの場合の証明も真に小さい命題にのみ依存するようにする。この命題の尺度は自然数の組として与えられ、辞書順で並べられる。

* |*P<sub>1</sub>*(`r`,`s`,`k`)| = (|`s`|, |`r`|<sub>|`s`|</sub> + |`k`|<sub>|`s`|</sub>)

* |*P<sub>2</sub>*(`k`,`s`)| = (|`s`|, |`k`|<sub>|`s`|</sub>)

* |*P<sub>3</sub>*(`r`,`s`,`k`)| = (|`s`|, |`r`|<sub>|`s`|</sub> ( 3|`s`| + 2 ) + |`k`|<sub>|`s`|</sub>)

    * ただし |`s`| は `s` の要素数である。

* |`Zero`|<sub>n</sub> = 1

* |`One`|<sub>n</sub> = 1

* |`Char c`|<sub>n</sub> = 1

* |`Sum r1 r2`|<sub>n</sub> = |`r1`|<sub>n</sub> + |`r2`|<sub>n</sub>

* |`Cat r1 r2`|<sub>n</sub> = |`r1`|<sub>n</sub> + |`r2`|<sub>n</sub> + 2

* |`Star r`|<sub>n</sub> = ( 3n + 2 ) |`r`|<sub>n</sub>

* |`Empty`|<sub>n</sub> = 0

* |`Accept r k`|<sub>n</sub> = |`r`|<sub>n</sub> + |`k`|<sub>n</sub> + 1

* |`AcceptStar s r k`|<sub>n</sub> = |`r`|<sub>min( 3(n+1), 3|`s`| )</sub> + |`k`|<sub>n</sub>

それぞれの場合についての証明は以下の通りである。

### 《*P<sub>1</sub>*(`r`,`s`,`k`)》

#### (i) `r` = `Zero` のとき

`accept (r, s, k)` ~> `True` も `s` ∈ ***L***(`Zero`)***L***(`k`) = ∅ も成り立たないので、同値は成り立つ。

#### (ii) `r` = `One` のとき

`accept (r, s, k)` ~> `True` は `pop_and_accept (k, s)` ~> `True` と同値である。整礎帰納法の仮定より、`pop_and_accept (k, s)` ~> `True` は `s` ∈ ***L***(`k`) と同値であり、***L***(`One`) = {`[]`} であるので、`s` ∈ ***L***(`One`)***L***(`k`) である。

#### (iii) `r` = `Char c` のとき

`accept (r, s, k)` ~> `True` は `s` = `c : s'` かつ `pop_and_accept (k, s')` ~> `True` であることと同値である。その場合、|`s'`| < |`s`| かつ |*P<sub>2</sub>*(`k`,`s'`)| < |*P<sub>1</sub>*(`Char c`,`s`,`k`)| である。帰納法の仮定より `s'` ∈ ***L***(`k`) であるので、`s` = `[c] ++ s'` ∈ ***L***(`Char c`)***L***(`k`) である。

#### (iv) `r` = `Sum r1 r2` のとき

`accept (r, s, k)` ~> `True` は `accept (r1, s, k)` ~> `True` または `accept (r2, s, k)` ~> `True` であることと同値である。|*r*|<sub>n</sub> は常に正であるので、両方とも「より小さい」から、帰納法の仮定より `s` ∈ ***L***(`r1`)***L***(`k`) または `s` ∈ ***L***(`r2`)***L***(`k`) であることと同値であると分かる。さらにこれは `s` ∈ (***L***(`r1`)***L***(`k`)) ∪ (***L***(`r2`)***L***(`k`)) = (***L***(`r1`)∪***L***(`r2`))***L***(`k`) = ***L***(`r`)***L***(`k`) と同じである。

#### (v) `r` = `Cat r1 r2` のとき

`accept (r, s, k)` ~> `True` は `accept (r1, s, Accept r2 k)` ~> `True` と同値である。

順序のより小さい *P<sub>1</sub>*(`r1`,`s`,`Accept r2 k`) により、これが `s` ∈ ***L***(`r1`)***L***(`Accept r2 k`) = ***L***(`r1`)***L***(`r2`)***L***(`k`) = ***L***(`r`)***L***(`k`) と同値であると分かる。

#### (vi) `r` = `Star r1` のとき

`accept (r, s, k)` ~> `True` は `accept_star (r1, s, k)` ~> `True` と同値であると分かる。ゆえに、代わりにただ *P<sub>3</sub>*(`r1`,`s`,`k`) を証明すればよい。

`accept_star (r1, s, k)` ~> `True` は `pop_and_accept (k, s)` ~> `True` または `accept (r1, s, AcceptStar s r1 k)` ~> True であることと同値である。

前者の場合は `s` ∈ ***L***(`k`) ⊂ ***L***(`k`)<sup>＊</sup>***L***(`k`) と同値である。これは *P<sub>2</sub>*(`k`,`s`) が「より小さい」ので、帰納法の仮定より言える。

後者の場合は `s` ∈ ***L***(`r1`)***L***(`AcceptStar s r1 k`) = ***L***(`r1`) ( (***L***(`r1`)<sup>＊</sup>***L***(`k`)) ∖ {`s`} ) と同値である。これも帰納法の仮定より言える。

集合に対する基本演算を使うことで、これら2つの述語の論理和を取るのは、`s` が和集合に入っているということと同値である。つまり言い換えると、`s` ∈ (***L***(`r1`)<sup>0</sup> ∪ (***L***(`r1`) ∖ {`[]`})***L***(`r1`)<sup>＊</sup>)***L***(`k`) = ***L***(`r1`)<sup>＊</sup>***L***(`k`) である。

ただし、*P<sub>3</sub>*(`r1`,`s`,`k`) は *P<sub>1</sub>*(`Star r1`,`s`,`k`) より真に小さくはないが、ここでは *P<sub>3</sub>*(`r1`,`s`,`k`) を仮定していないので問題ない。

### 《*P<sub>2</sub>*(`k`,`s`)》

#### (i) `k` = `Empty` のとき

`s` ∈ ***L***(`k`) であり、これは `s` が空文字列であることと同値であり、まさにこのとき `pop_and_accept (k, s)` ~> `True` が成り立つ。

#### (ii) `k` = `Accept r k'` のとき

`s` ∈ ***L***(`k`) = ***L***(`r`)***L***(`k'`) であり、これは帰納法の仮定より `accept (r, s, k')` ~> `True` と同値である。（*P<sub>1</sub>*(`r`,`s`,`k'`) は *P<sub>2</sub>*(`Accept r k'`,`s`) よりも 1 だけ小さい。）この場合、`pop_and_accept (k, s)` ≡ `accept (r, s, k')` ~> `True` である。

#### (iii) `k` = `AcceptStar s' r k'` のとき

`s` ∈ ***L***(`k`) は `s` ≠ `s'` かつ `s` ∈ ***L***(`r`)<sup>＊</sup>***L***(`k'`) であることと同値である。帰納法の仮定より、`s` ∈ ***L***(`r`)<sup>＊</sup>***L***(`k'`) は `accept_star (r, s, k)` ~> `True` と同値であり、不等式は Haskell のプログラムに反映されているので、`s /= s'` ~> `True` である。これらを合わせると、`s` ∈ ***L***(`k`) が成立することは `pop_and_accept (k, s)` が成立することと同値である。

### 《*P<sub>3</sub>*(`r`,`s`,`k`)》

この場合は *P<sub>1</sub>*(`Star r`.`s`,`k`) の中の部分帰納法と同様に証明される。実際、|*P<sub>3</sub>*(`r`,`s`,`k`)| = |*P<sub>1</sub>*(`Star r`.`s`,`k`)| である。

<div align="right">（証明終わり）</div>
