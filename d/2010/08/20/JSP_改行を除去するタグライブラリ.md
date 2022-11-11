---
title: JSP 改行を除去するタグライブラリ
aliases:
  - JSP_改行を除去するタグライブラリ
tags:
  - d/2010/08/20
  - n/PGM/Java/JSP/v2.0
---

- 2010-08-20
- JSP2.0

隙間対策で適当に作ったメモ
================================================================================
改行をスペースとして判断されるとレイアウトにくずれていしまう対策だけどコードは改行いれて書きたいよってことで。

フィルタでやれという話もあるがね

メイン
================================================================================

```java
import java.io.IOException;
import java.io.StringWriter;

import javax.servlet.jsp.JspException;
import javax.servlet.jsp.tagext.SimpleTagSupport;

public class RejectEolTag  extends SimpleTagSupport  {
    @Override
    public void doTag() throws JspException, IOException {
        StringWriter sw = new StringWriter();
        getJspBody().invoke(sw);
        String body = sw.toString().trim();
        body = body.replaceAll("\n", "");
        body = body.replaceAll("\r", "");
        body = body.replaceAll("> +<", "><");
        getJspContext().getOut().println(body);
    }
}
```



