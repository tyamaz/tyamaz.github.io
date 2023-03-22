
---
title: ActionScript 3.0 プロパティの定義
aliases:
  - ActionScript_3.0_プロパティの定義
tags:
  - d/2008/11/20
  - n/PGM/ActionScript/v3.0
---

- 2008年11月20日
- ActionScript3.0

オリジナルのプロパティの定義
================================================================================
getとsetというキーワードを使うと定義できる。

```actionscript
package{
    public class Hoge{
        private var _piyo:Number;
        public function Hoge(){
        }
        public function get piyo(){
            return this._piyo;
        }
        public function set piyo(value:Number){
            this._piyo = value;
        }
        
    }
}
```

