# ISUCON_memo




# SQLチューニング

## B-Tree

ソート処理と木構造によって検索の高速化をするアルゴリズムのこと

B-TreeはDBのインデックスの内部構造となっている -> 実際のDBでは範囲検索やorder byに対応するためにB+Treeを採用している

![B-Tree](./images/B-Tree.png)


### 単一カラムのインデックス

likeでインデックスが効くのは前方一致のみ

![single](./images/single.jpeg)

先頭文字が分からないと検索木を辿る事書きでなくなるためである

```
where name like 'naka%'
```

上記のように検索すればnakajimaの直前の位置まで木構造の動きでたどり着くことができ、
それ以降のデータを取得する動きができる

中間一致や後方一致ではB-Treeのソートが活かせない

また、検索キーに関数や演算を使うとインデックスが効かない

これは関数適用前の値で検索木が形成されているので、検索木を辿ることが出来なくなるから

### 複合インデックス

複数カラムを指定してインデックスを作成しても、定義の第一カラムをもとに検索木を作成する

検索木は第一カラムで作られるため、第二カラムを指定したクエリではインデックスは使わない

第一カラムがカーディナリティが高いカラムを指定した方がよい

カーディナリティはカラムの値の種類の絶対値

- 男と女のような性別についてはカーディナリティが低いという
- 一方顧客番号ならたくさん存在するので、レコード数が多くなりカーディナリティが高いという

カーディナリティの少ないカラムを指定すると一つの枝のリーフノードが多すぎて、全然絞り込めない


![multi](./images/multi.jpeg)

- [SQLアンチパターンとBtreeインデックスの関連性 | BASE PRODUCT TEAM BLOG](https://devblog.thebase.in/entry/2018/12/09/110000)
- [【図解】B-treeを理解し、複合インデックスの順番を正しく作る | Enjoy IT Life](https://nishinatoshiharu.com/overview-multicolumn-indexes)
- [MySQLのEXPLAINの読み方とチューニング時のチェックポイント | Enjoy IT Life](https://nishinatoshiharu.com/explain_overview/)
