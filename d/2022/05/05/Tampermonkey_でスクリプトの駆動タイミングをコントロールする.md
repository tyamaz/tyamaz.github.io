---
title: Tampermonkey_でスクリプトの駆動タイミングをコントロールする
aliases:
  - Tampermonkey_でスクリプトの駆動タイミングをコントロールする
tags:
  - d/2022/05/05
---

- Tampermonkey v4.16

Chrome でユーザースクリプトを駆動する Tampermonkey の話

`@run-at` という記述を追加するとスクリプトを発火させるタイミングをコントロールできる。

デフォルトは `// @run-at document-idle` で、所謂ブラウザの `DOMContentLoaded` イベントの発火後に挿入される。

このような機構があるため、自力でコントロールしてもタイミングが合わないということがおきる。

[Tampermonkey • Documentation](https://www.tampermonkey.net/documentation.php#_run_at)

