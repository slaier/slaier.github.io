---
weight: 1
title: "Editor"
---

## Sed

`sed` is a stream editor. It can perform basic text transformations on an input stream (a file or input from a pipeline).

### Common Sed Commands

*   **Substitute (s)**: Replaces occurrences of a pattern with another string.
    ```bash
    sed 's/old_string/new_string/' filename # Replaces the first occurrence on each line
    sed 's/old_string/new_string/g' filename # Replaces all occurrences on each line
    sed 's/old_string/new_string/gi' filename # Replaces all occurrences case-insensitively
    sed -i 's/old_string/new_string/g' filename # In-place replacement
    ```

*   **Delete (d)**: Deletes lines that match a pattern.
    ```bash
    sed '/pattern/d' filename # Deletes lines containing 'pattern'
    sed '1d' filename # Deletes the first line
    sed '1,5d' filename # Deletes lines from 1 to 5
    sed '$d' filename # Deletes the last line
    ```

*   **Print (p)**: Prints lines that match a pattern.
    ```bash
    sed -n '/pattern/p' filename # Prints only lines containing 'pattern'
    sed -n '1,5p' filename # Prints lines from 1 to 5
    ```
    The `-n` option suppresses the default output, so only the explicitly printed lines are shown.

*   **Insert (i)**: Inserts text before a line.
    ```bash
    sed '1i\
    This is a new line inserted at the beginning.' filename
    ```

*   **Append (a)**: Appends text after a line.
    ```bash
    sed '$a\
    This is a new line appended at the end.' filename
    ```

*   **Transform (y)**: Transforms characters.
    ```bash
    sed 'y/abc/ABC/' filename # Transforms 'a' to 'A', 'b' to 'B', and 'c' to 'C'
    ```

### Complex Sed Commands

* Removes colors.
    ```bash
    sed -r "s/\x1B\[([0-9]{1,3}(;[0-9]{1,2};?)?)?[mGK]//g" filename
    ```
