---
title: "增加KaTeX的引用形式"
date: 2022-02-23T17:11:33+08:00
tags: ["Hugo"]
categories: ["建站笔记"]
---

本站使用主题Beautiful Hugo,其自带数字格式化引擎$\KaTeX{}$，但$\KaTeX{}$不支持两个$括住公式的语法来描述数学公式，这就导致与平常习惯及其不相符，降低码字效率

<!--more-->

为此查阅资料后，在Beautiful Hugo的issue栏找到了相关的问题与解法，只需要修改`layouts/partials/footer.html`

```html
-  {{- end -}}
-  <script> renderMathInElement(document.body); </script>
+  {{- end  }}
+  <script>
+    renderMathInElement(
+      document.body,
+      {
+        delimiters: [
+          {left: "$$", right: "$$", display: true},
+          {left: "$", right: "$", display: false},
+          {left: "\\begin{equation}", right: "\\end{equation}", display: true},
+          {left: "\\begin{align}", right: "\\end{align}", display: true},
+          {left: "\\begin{alignat}", right: "\\end{alignat}", display: true},
+          {left: "\\begin{gather}", right: "\\end{gather}", display: true},
+          {left: "\\(", right: "\\)", display: false},
+          {left: "\\[", right: "\\]", display: true}
+        ]
+      }
+    );
+  </script>
```

上面的代码块-开头的部分指删去，+开头的部分指增加

这块代码自定义了引用$\KaTeX{}$时的分隔字符，因此就能够使用两个$来括住公式