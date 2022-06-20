---
title: Mac を本格的にキーボードで操作する Chrome Profile 編
aliases:
  - Mac_を本格的にキーボードで操作する_Chrome_Profile_編
tags:
  - d/2022/06/13
  - n/PGM/Mac/Monterey
  - n/PGM/browser/Google_Chrome/v102
  - n/PGM/Mac/Karabiner-Elements/v14.4.0
---

- Mac Monterey
- Google Chrome v102
- Karabiner-Elements v14.4.0


仕事では Google Chrome を用途をわけて Profile を使い分けるが、Profile がわかれるとウィンドウが分かれてしまうのでそれを考慮したアプリの切り替えをキーボードで行う必要がある。

アプリの切り替え
================================================================================
アプリを切り替えるにはいくつか方法があるが、無限にあるアプリを無限に消費するわけでなく、特定のアプリをいくつか切り替えるのみなので、キー操作に直接割り当てるのが効率的。

自分は右Optionキーに対して左手の QWERASDF あたりを割り当てることでアプリを切り替える予定にしている。右手のホームポジション自体を右にキー2個分シフトしているので右手の親指でOption キーを常用が可能になっている。

Karabiner-Elements を使ってこのように設定する


```json
        {
            "description":"switch window global",
            "manipulators":[
                {
                    "type":"basic",
                    "from": { "key_code": "q", "modifiers":{ "mandatory": ["option"]} },
                    "to": [
                        {
                            "shell_command": "open -a 'Google Chrome'" 
                        }
                    ],
```

shell の `open` コマンドを名前指定で呼び出すことで Google Chrome をアクティブにする

この例では `option + q` のキーに割り当てている

Profile の切り替え
================================================================================
Profile を切り替えるにはやや工夫が必要になる。

通常 Google .Chrome の操作で Profiel を切り替えるには、上部メニューの「プロファイル」から切り替えたいプロファイルを選ぶことになる。

この上部のメニューに切り替え項目が存在することに注目する。

Mac ではこの上部のメニュー内にある項目にはアプリ関係無くなんでもキーボードショートカットを割り当てることができる。

環境設定→キーボード→ショートカット→アプリケーション で Google Chrome を追加してそのメニューの任意のプロファイルにキー操作を割り当てる。

この割り当てるキーは人間が入力するモノでないので、他の操作とバッティングしないような複雑なモノにするとよい。今回は `Shift + Option + Ctrl + Command + 9` に特定のプロファイルを割り当てた。

このキーを Google Chrome がアクティブになった直後に送り込めばよいことになる。しかしタイミングの関係で最速で送り込んではうまくいかない。

そこで↓のように `to` の指定が動作した後のキーが上がるタイミングで発動させる

```json
                {
                    "type":"basic",
                    "from": { "key_code": "q", "modifiers":{ "mandatory": ["option"]} },
                    "to": [
                        {
                            "shell_command": "open -a 'Google Chrome'" 
                        }
                    ],
                    "to_after_key_up": [
                        {
                            "key_code": "9",
                            "modifiers":[
                                "command","control", "shift", "option"
                            ]
                        }
                    ]
                },
```

これならば確実にアプリが切り替わった後なのでうまく動作する。

このようにしていけば Google Chrome の全プロファイルをキー操作一発で切り替えることができるようになる。




























