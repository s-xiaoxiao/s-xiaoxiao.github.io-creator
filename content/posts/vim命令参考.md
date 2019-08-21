---
title: "vim命令参考笔记"
date: 2019-08-20T10:08:36+08:00
draft: false
---

切记！切记！ 一切命令皆在英文状态输入！
在命令行内输入 vimtutor 自带的 vim 教程

# vim 的基本操作

#### 进入：vi or vim 一> enter

vi 文件名 enter or vim 文件名 enter 进入文件编辑

#### 退出：ESC 一> : 一> q 一> enter 即可

-   <strong>q!<strong> 强制退出：丢失文件的改动
-   <strong>wq</strong> 保存退出

#### 编辑：在普通模式下

-   <strong>i</strong> &在当前光标下,即会进入编辑模式/插入模式
-   <strong>I</strong> 则会在行的开头进行编辑模式
    <br >
-   <strong>a</strong> 在当前下一个光标,即会进入编辑模式/插入模式
-   <strong>A</strong> 则会在行尾的进行编辑模式

#### 移动：

-   数字 w (number word) 移动几个单词开始处
-   数字 e (number )移动几个单词的结尾处

#### 删除：

-   在普通模式下 x 就会删除当前光标后面一个
-   dd 删除一行(可选参数：number delete delete 意味着连续删除几行)
-   dw (delete a word) 删除单词 or 一串中文字符,须在单词 or 一串中文字符的起始处,删除后会到一下单词起始处
-   db (delete back)删除单词 or 一串中文字符,须在单词 or 一串中文字符的结尾处
-   di+() or {} (delete in () or {}) 删除 {} or () 内的字符,须在 () or {} 内
-   da+() or {} (delete at () or {}) 删除包括 () or {} 的字符,须在 () or {} 内
-   dit (delete in tag 删除标签内的内容)
-   dat (delete at tag 删除标签包含内容)
-   d\$ 从当前光标删到航末尾
-   de 从当前光标删到单词结尾
-   d number w (delete number word) 删除多个单词

#### 删除修改:

-   ci+ t or () or {} or <> (change in tag or () or {} or <>) 删除<> </> 包含的内容并且进入 insert 模式
-   ca+ t or () or {} or <> (change at tag or () or {} or <>) 删除标签包含你内容，并且进入 insert 模式

#### 撤销：在普通模式下 按住 u 即会撤销上一次修改

-   number u (number undo 撤销多次)
-   U 可撤销整行的修改

#### 还原：在普通模式下 按住 ctrl+r 就会还原上一次撤销

-   number ctrl+r (可多次还原上一次修改)

#### 复制：按 v 进入复制选取模式,选择 按 y 可复制

#### 粘贴：在普通模式下,p 可直接到下方粘贴。

-   P 是粘贴到上方

#### 快速上下翻页

-   ctrl+d (向下翻半页)
-   ctrl+u (向上翻半页)

#### 光标移动

-   k (上行键)
-   l (右移)
-   j (下行键)
-   h (左移)

## vim 有四种模式

1. 按 ESC (处于正常模式 normal)
2. 按 i (处于编辑模式 insert)
3. 按 : (处于命令行模式)
4. 按 v (处于复制选取模式)

## 基本单词

-   quit (退出)
-   write/read (写/读)
-   yank (复制)
-   paste (粘贴)
-   delete (删除)
-   change (修改)
-   line (一行)
-   find (查找)
-   word (单词)
-   forward/backward (前进/后退)
-   up/down (上/下)
-   insert/append (插入/附加)
-   undo (撤销)
-   redo (还原)
