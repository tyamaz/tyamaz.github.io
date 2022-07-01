---
title: ActionScript 3.0 裏で通信する
aliases:
  - ActionScript_3.0_裏で通信する
tags:
  - d/2009/01/09
  - n/PGM/ActionScript/3.0/Ope/ネットワーク操作
---


- 2009年01月19日


```actionscript
import flash.net.URLLoader;
import flash.net.URLLoaderDataFormat;
import flash.net.URLRequest;
import flash.system.Security;

//....中略

Security.loadPolicyFile('http://localhost/crossdomain.xml');
this.ld = new URLLoader(new URLRequest("http://localhost/hoge.txt"));
this.ld.dataFormat = URLLoaderDataFormat.TEXT;
this.ld.addEventListener(Event.COMPLETE, onComplete);
```


```actionscript
public function onComplete(e:Event){
    trace((eval(this.ld.data)).a);		
}
```


