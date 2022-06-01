---
title: Flex 3.0 MXML と ActionScript の分離
aliases:
  - Flex3.0_MXMLとActionScriptの分離
tags:
  - d/2009/02/28
  - n/PGM/Flex/3.0
  - n/PGM/ActionScript
---

- 注 非常に古い情報
- 2009年02月28日

MXMLソースとActionScriptソースの分離
================================================================================
HTML + Javascript 同様に分離して書くことができます。

分離前
--------------------------------------------------------------------------------

```xml
<?xml version="1.0" ?>
<mx:Application xmlns:mx="http://www.adobe.com/2006/mxml">
  <mx:Script>
    <![CDATA[
      import mx.controls.Alert;
      private function sayHoge():void{
        Alert.show("hoge");
      }
    ]]>
  </mx:Script>
  <mx:Button id="hogeButton" label="hoge" click="sayHoge();"/>
</mx:Application>
```

分離後
--------------------------------------------------------------------------------
やり方は簡単でJavascriptの場合と同じです。


```xml
<?xml version="1.0" ?>
<mx:Application xmlns:mx="http://www.adobe.com/2006/mxml">
  <mx:Script source="sayhoge.as" />
  <mx:Button id="hogeButton" label="hoge" click="sayHoge();"/>
</mx:Application>
```


```actionscript
import mx.controls.Alert;
private function sayHoge():void{
    Alert.show("hoge");
}
```

こんな風に Scriptタグにsourceを指定します。

