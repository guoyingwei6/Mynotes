---
date: 2023-10-07
categories:
  - Tutorial
slug: markdown-extention-features
---

# 扩展markdown功能


???+ warning
    这里的语法并不是Markdown通用语法，其它主题或软件可能并不支持，慎用！

## content tabs

### Configuration

This configuration enables content tabs, and allows to nest arbitrary content inside content tabs, including code blocks and ... more content tabs! Add the following lines to `mkdocs.yml`:

``` 
markdown_extensions:
  - pymdownx.superfences
  - pymdownx.tabbed:
      alternate_style: true

```
### Usage

#### Grouping code blocks

Code blocks are one of the primary targets to be grouped, and can be considered a special case of content tabs, as tabs with a single code block are always rendered without horizontal spacing:

``` title="Content tabs with code blocks"
=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```
```

=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```

#### Grouping other content

``` title="Content tabs"
=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci

```

=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci

#### Embedded content

``` title="Content tabs in admonition"
!!! example

    === "Unordered List"

        ``` markdown
        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci
        ```

    === "Ordered List"

        ``` markdown
        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci
        ```

``` 

!!! example

    === "Unordered List"

        ``` markdown
        * Sed sagittis eleifend rutrum
        * Donec vitae suscipit est
        * Nulla tempor lobortis orci
        ```

    === "Ordered List"

        ``` markdown
        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci
        ```


## Admonitions

### Demo
mkdocs支持Admonitions，在网页上显示类似警告信息。

=== "渲染效果"
    ???+ note "Admonition"
        Action is the antidote to despair.

=== "源代码"
    ``` 
    ???+ note "Admonition"
        Action is the antidote to despair.
    ```
???+ quote
    - Adding a `+` after the `???` token renders the block expanded

    - 通过缩进控制Tab和Admonition结构


### Supported types

!!! note

    Action is the antidote to despair.

!!! abstract

    Action is the antidote to despair.

??? info

    Action is the antidote to despair.

???+ tip
    Action is the antidote to despair.
    
!!! success

    Action is the antidote to despair.
    
!!! question
    Action is the antidote to despair.
    
???+ warning
    Action is the antidote to despair.
        
!!! failure
    Action is the antidote to despair.
        
!!! danger
    Action is the antidote to despair.
        
!!! bug
    Action is the antidote to despair.
        
!!! example
    Action is the antidote to despair.
            
!!! quote
    Action is the antidote to despair.
    
 