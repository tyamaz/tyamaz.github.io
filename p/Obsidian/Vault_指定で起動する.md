---
title: Obsidian / Vault 指定で起動する
---

Vault(保管庫)指定で起動する方法。

通常はこのような場合はコマンドラインの起動オプションを指定するのが普通だが、Obsidian は `Obsidian URI` なるものを使うようだ。

- [Using obsidian URI \- Obsidian Help](https://help.obsidian.md/Advanced+topics/Using+obsidian+URI)


```
obsidian://action?param1=value&param2=value
```

このように指定するっぽい。

今回は保管庫指定で起動なので、

```
obsidian://open?path=G:\マイドライブ\hoge\piyo\fuga\obsidian\myvault
```

となる。

はて、こいつをどこに書くのか？

Windows の場合は Obsidian を一度起動すると、カスタムプロトコルハンドラに登録されるというのだ。

調べるとこいつはブラウザからアプリケーションを起動する仕組みのようだ。

つまりブラウザで URL を指定するように起動できればいい。

デスクトップ等を右クリックして新規作成からショートカットを作って↑の URI を指定すればいい。



