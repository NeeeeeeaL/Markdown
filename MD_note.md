# Markdown Learning Notes

- [Markdown Learning Notes](#markdown-learning-notes)
  - [1. Headings](#1-headings)
  - [2. Styling text](#2-styling-text)
  - [3. Quoting](#3-quoting)
  - [4. Links](#4-links)
  - [5. Lists](#5-lists)
  - [6. Using emoji](#6-using-emoji)
  - [7. Attachments](#7-attachments)

## 1. Headings

<!-- 
# The largest heading
## The second largest heading
### The third largest heading
#### The third smallest heading
##### The second smallest heading
###### The smallest heading 
-->

---
## 2. Styling text

| Style                 | Synax          | Example          |
| --------------------- | -------------- | ---------------- |
| Bold(加粗)            | ** ** or __ __ | **NeaL**, __Yu__ |
| Italic(斜体)          | * * or _ _     | *NeaL*, _Yu_     |
| Strikethrouth(删除线) | ~~ ~~          | ~~shit~~         |
| Bold and italic       | ** ** and _ _  | **I am _NeaL_**  |

---
## 3. Quoting 

+ Quoting text

    In the words of Abraham Lincoln:
    > Pardon my French

+ Quoting code

    + Use `git status` to list all new or modified files that haven't yet been commited.
    + Some basic Git commands are:

    ```
    git status
    git init
    git commit
    ```
    
 ---
## 4. Links

+ Nomal links

    The site is [GitHub](https://www.github.com)
+ Section links

    <!-- Not clear -->
+ Relative links

    [Contrabution guidelines for this project (Markdown - PPT)](PPT_test.md) 
    <!-- 此时已包含此路径c:\Users\Adam0\Documents\Markdown\，因此不必在文档前加此路径 -->

---
## 5. Lists
+ Normal lists( '-' or '*' )
    - George Washington
    * Adam Young
    - NeaL Yu

+ Nested lists
    1. First list item
       - First nested list item 
         - Second nested list item

+ Task lists
- [x] Finish my daily tasks
- [ ] push my commits to github
- [ ] open a pull request

---
## 6. Using emoji
You can add emoji to your writing by typing `:EMOJICODE:`.

Eg:
@NeaL :+1: This PR looks great - it's ready to merge! :smile:
<!-- It seems like VScode does not support emoji like this! -->
<!-- But there is always a way to fix it!  -->
Emoji can be copide and pasted in [Emoji Homepage](http://emojihomepage.com/)

That's it! 🚵😁

## 7. Attachments
+ VScode 插件： Markdown All in One

    - 快捷键：
  
        | 快捷键   | 操作                    |
        | -------- | ----------------------- |
        | Ctrl + B | 加粗                    |
        | Ctrl + I | 斜体                    |
        | Alt + S  | 删除线                  |
        | Alt + C  | 勾选/取消任务清单项目   |
        | Ctrl + M | 开启 LaTex 数学公式编写 |
        
    - 功能：
      - 自动生成/更新目录(Create/Update contents)
      - 自动格式化表格(Format Document)
      - 图片等本地素材自动补全路径(不好使😅😅)
