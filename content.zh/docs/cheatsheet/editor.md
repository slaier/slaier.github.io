---
weight: 1
title: "编辑器"
---

## Sed

`sed` 是一个流编辑器。它可以在输入流（文件或来自管道的输入）上执行基本的文本转换。

### 常用 Sed 命令

*   **替换 (s)**: 将模式的出现替换为另一个字符串。
    ```bash
    sed 's/old_string/new_string/' filename # 替换每行的首次出现
    sed 's/old_string/new_string/g' filename # 替换每行的所有出现
    sed 's/old_string/new_string/gi' filename # 不区分大小写地替换所有出现
    sed -i 's/old_string/new_string/g' filename # 就地替换
    ```

*   **删除 (d)**: 删除匹配模式的行。
    ```bash
    sed '/pattern/d' filename # 删除包含 'pattern' 的行
    sed '1d' filename # 删除第一行
    sed '1,5d' filename # 删除从 1 到 5 的行
    sed '$d' filename # 删除最后一行
    ```

*   **打印 (p)**: 打印匹配模式的行。
    ```bash
    sed -n '/pattern/p' filename # 仅打印包含 'pattern' 的行
    sed -n '1,5p' filename # 打印从 1 到 5 的行
    ```
    `-n` 选项抑制默认输出，因此只显示明确打印的行。

*   **插入 (i)**: 在行前插入文本。
    ```bash
    sed '1i\
    这是在开头插入的新行。' filename
    ```

*   **追加 (a)**: 在行后追加文本。
    ```bash
    sed '$a\
    这是在末尾追加的新行。' filename
    ```

*   **转换 (y)**: 转换字符。
    ```bash
    sed 'y/abc/ABC/' filename # 将 'a' 转换为 'A'，'b' 转换为 'B'，'c' 转换为 'C'
```
