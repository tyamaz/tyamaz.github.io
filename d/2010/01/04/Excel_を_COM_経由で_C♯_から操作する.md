---
title: Excel を COM 経由で C♯ から操作する
aliases:
  - Excel_を_COM_経由で_C♯_から操作する
tags:
  - d/2010/01/04
  - n/PGM/MS-Office/Excel/2003
  - n/PGM/CSharp/2.0
---

- ※古い情報です
- 2010-01-04
- WindowsXP Home
- Excel 2003
- VS2005
- C#2.0

使う前の前準備
================================================================================
参照の追加
--------------------------------------------------------------------------------
参照設定を追加してやる。`comタブ` の中から

```
Microsoft Excel 11.0 Object Library
```

を追加

サンプルコード
================================================================================
まずクラスを Excel 関係を使えるように名前空間を `using`

```csharp
using Excel = Microsoft.Office.Interop.Excel;
```

`Excel` って名前で使えるようにエイリアスを作る。


```csharp
Excel.Application xlsApp = new Excel.Application();
xlsApp .Visible = true;
Excel.Workbooks xlsBooks = xlsApp.Workbooks;
Excel.Workbook xlsBook = xlsBooks.Add(string.Empty);
Excel.Sheets xlsSheets = xlsBook .Worksheets;
Excel.Worksheet xlsSheet = (Excel.Worksheet)xlsSheets[1];
Excel.Range xlsCells = (Excel.Range)s.Cells;
Excel.Range xlsCell = (Excel.Range)xlsCells[1, 1];
xlsCell.Value2 = "aaaa";
            
xlsBook .SaveAs("C:\\hoge.xls",
                Type.Missing,
                Type.Missing,
                Type.Missing,
                Type.Missing,
                Type.Missing,
                Excel.XlSaveAsAccessMode.xlExclusive,
                Type.Missing,
                Type.Missing,
                Type.Missing,
                Type.Missing,
                Type.Missing);

xlsBook.Close(Type.Missing, Type.Missing, Type.Missing);
xlsApp.Quit();

System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsCell);
System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsCells);
System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsSheet);
System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsSheets);
System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsBook);
System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsBooks);
System.Runtime.InteropServices.Marshal.ReleaseComObject(xlsApp);
xlsCell = null;
xlsCells = null;
xlsSheet = null;
xlsBook = null;
xlsBooks = null;
xlsApp = null;
```

`Type.Missing` というのがずらずら並ぶのは引数の省略ができないから

プロセス残り対策
================================================================================
いろんなところでかかれてますが `COM` 経由で Excel を制御すると、普通の感覚で使うと、エクセルのプロセスが残ってしまいます。

[Excel VB プロセス - Google 検索](http://www.google.co.jp/search?q=Excel+VB+%E3%83%97%E3%83%AD%E3%82%BB%E3%82%B9&lr=lang_ja&ie=utf-8&oe=utf-8&aq=t&rls=org.mozilla:ja:official&client=firefox-a) とかで検索すると、その対策がたくさんでてきます。対策は↓のようになる。

- `new` した `Excel.Application` オブジェクトは `Quit` メソッドを呼び出して明示的に終了する
- `Book` は明示的に `close` する
- `new` と型宣言と定数とキャスト以外では `Excel.` から始まる名前から処理を呼び出さない
  - つまり名前空間頼りの暗黙クラスメソッド呼び出しとかを避ける
- プロパティやメソッド経由でオブジェクトを操作しない
  - サンプルコードでもわかるように、コレクションを一旦変数に格納するという面倒な操作をやっている。このように使うために経由する参照はすべて変数に一旦取り込んでプログラム側から制御可能な状態にしておく
- Excelがらみのオブジェクトは使い終わったら `ReleaseComObject` によって `COM` の参照を開放する
  - ここで開放するために経由する参照を変数で掴んでおく必要がある。
- `COM` の参照を掴んでいた変数に `null` を突っ込んで GC で処理させる
- 明示的に変数に `null` を突っ込んで変数からの参照も開放する。

このようにコーディングするとプロセスは残らない。

設計
================================================================================
`COM` 関係の操作は値を直接操作するような処理は非常に面倒なので、必要な値をマッピングする総合的なラッパーを一個作って、値は別で保持し、保存時に一気に反映させるのがいいような気がする。

逆のマッピングを考えるとちょっとメンドイけど保存だけならこれでもいいか


ブックを開く
================================================================================
```csharp
Excel.Application xlsApp = new Excel.Application();
Excel.Workbooks xlsBooks = xlsApp.Workbooks;
Excel.Workbook xlsBook = xlsBooks .Open("C:\\hoge.xls",
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing,
                                        Type.Missing);
```

VBAマクロが動かないように開く
================================================================================
ブックに `Openイベント` にフックされていると、開いた瞬間に何かがおきてプログラム側から開いた場合にいろいろ不具合が起きたりします。特にダイアログが開くとかは結構メンドイ

なのでマクロつきのブックを開くときはイベントを停止させたいですね。

```csharp
Excel.Application hoge = new Excel.Application();
hoge.EnableEvents = false;
```

`EnableEvents` に `false` を突っ込んでから開くとマクロが動きません

※これは2003とかからだけかもしれません　2000とかではだめかも。



