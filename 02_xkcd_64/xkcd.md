# xkcd

flagというファイルをまず作成しないと正常に動作しない。

適当にflagを作る。

```
echo 'flag{flag}' > flag
```

実行すると、入力が受け付けられる。


## 解法

リバーシングしていくと、
入力した文字列を
`strtok`と言う関数で少しずつ調べている。

結果、
入力の始めから`?`までの文字列が
`SERVER, ARE YOU STILL THERE`
と一致し、
続く`"`までの文字列が` IF SO, REPLY `と一致する場合次の処理に進み、
続く`"`までの文字列をXとして、その次の`(`と`)`で囲まれた中の数字とXの文字長が一致する場合、Xを出力すると言うプログラムであった。

入力は以下のようにすれば良い

```
SERVER, ARE YOU STILL THERE? IF SO, REPLY "XXX" (3) 
```

ここまではわかったが、どう攻撃するのかが分からなかった。

初めに`flag`ファイルを読み込んでいるアドレスが`0x6b7540`であり、
文字列Xを保存するアドレスが`0x6b7340`であるので、
その差`0x200`の分だけ文字列を入力することで、Xとflagを繋げ、一つの文字列とすることができるらしい。
つまり、512文字だけ入力し、()内には512 + (flagの文字長)の数字を入れておけば良い。

今回作ったflagは`flag{flag}`で10文字であるので以下のように入力すれば良い。

```
SERVER, ARE YOU STILL THERE? IF SO, REPLY "[X*512]" (522) 
```