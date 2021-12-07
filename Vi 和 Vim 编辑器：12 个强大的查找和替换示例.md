Vim 通常被称为文本编辑器，而不是文本创建器。为什么 ？因为我们花费大量时间编辑现有文本而不是创建新文本。在文本编辑中，文本/模式替换成为至关重要的部分。

在本文中，让我们回顾一下如何在Vi 和 Vim 编辑器中执行基本和高级文本和模式替换功能。使用 12 个非常实用且功能强大的文本替换示例来解释这些功能。

vim 编辑器中文本替换的语法：

:[range]s[ubstitute]/{pattern}/{string}/[flags] [count]

以下是三个可能的标志。

[c]确认每次替换。
[g]替换该行中出现的所有内容。
[i]忽略模式的大小写。
示例 1. 用整个文件中的另一个文本替换所有出现的文本
这是 Vi 编辑器中文本替换的基本用法。当您希望将特定文本替换为整个文件中的另一个文本时，您可以使用以下序列。

:%s/old-text/new-text/g

%s – 指定所有行。将范围指定为 '%' 意味着在整个文件中进行替换。
g – 指定行中的所有出现。使用 'g' 标志，您可以替换整行。如果未使用此 'g' 标志，则仅替换该行中第一次出现的位置。
示例 2. 在一行中用另一个文本替换一个文本
当您希望以不区分大小写的方式将特定文本替换为一行中的另一个文本时。不指定范围意味着仅在当前行中进行替换。使用 'i' 标志，您可以使替代搜索文本不区分大小写。

:s/I/We/gi
示例 3. 用行范围内的另一个文本替换一个文本
使用范围，您只能使替换中受影响的行范围。指定 1, 10 作为范围意味着只在第 1 – 10 行进行替换。

:1,10s/helo/hello/g
示例 4. 通过视觉选择行将一个文本替换为另一个文本
您还可以通过视觉选择这些行来选择特定的行。在命令模式下按 CTRL + V，使用导航键选择要替换的文件部分。按 ':' 它将自动形成为 :'<,'> 然后你可以使用正常的替代作为


:'<,'>s/helo/hello/g
示例 5. 将一个文本替换为另一个文本仅第 1 个 X 行
在替换中使用计数，如果在替换中指定计数 N 则表示从光标当前位置开始在 N 行中进行替换。从当前行开始替换 4 行。

:s/helo/hello/g 4
示例 6. 仅替换整个单词而不是部分匹配
让我们假设您只想将下面提到的原始文本中的整个单词“his”更改为“her”。如果您进行标准替换，除了将他更改为她之外，还会将 This 更改为 Ther，如下所示。

标准替换
Original Text: This is his idea

:s/his/her/g

Translated Text: Ther is her idea

全词替换
Original Text: This is his idea

:s/\<his\>/her/

Translated Text: This is her idea
注意：：您应该用 < 和 > 将单词括起来，这将强制替换仅搜索完整单词而不是任何部分匹配。

示例 7. 使用正则表达式将 word1 或 word2 替换为新单词
在下面的示例中，它将翻译任何出现的 good 或 nice 将替换为 awesome。

Original Text: Linux is good. Life is nice.

:%s/\(good\|nice\)/awesome/g

Translated Text: Linux is awesome. Life is awesome.

您还可以通过指定正则表达式来进行替换。以下示例将 hey 或 hi 替换为 hai。请注意，这不会替换“他们”、“这个”等词。

:%s/\<\(hey\|hi\)\>/hai/g

\< - 字边界。
\| – “逻辑或”（在这种情况下嘿或嗨）
示例 8. Vim 编辑器中的交互式查找和替换
您可以使用替换中的“c”标志执行交互式查找和替换，这将要求确认进行替换或跳过它，如下所述。在这个例子中，Vim 编辑器将全局查找单词 'awesome' 并将其替换为 'wonderful'。但它只会根据您的输入进行替换，如下所述。

:%s/awesome/wonderful/gc

replace with wonderful (y/n/a/q/l/^E/^Y)?
y - 将替换当前突出显示的单词。替换后它会自动突出显示与搜索模式匹配的下一个单词
n – 不会替换当前突出显示的单词。但它会自动突出显示与搜索模式匹配的下一个单词
a – 将自动替换所有符合搜索条件的突出显示的词。
l - 这将仅替换当前突出显示的单词并终止查找和替换工作。
示例 9. 用其行号替换所有行。
当字符串以 '\=' 开头时，应将其计算为表达式。使用“line”函数我们可以获得当前的行号。通过结合这两个功能，替换对所有行进行行编号。

:%s/^/\=line(".") 。“。 “/G

注意：这与“:set number”不同，它不会将行号写入文件。但是当您使用此替换时，您将在文件中永久使用这些行号。

示例 10. 将特殊字符替换为其等效值。
用 $HOME 变量值替换 ~ 。

Original Text: Current file path is ~/test/

:%s!\~!\= expand($HOME)!g

Translated Text: Current file path is /home/ramesh/test/
您可以使用 expand 函数来使用所有可用的预定义和用户定义的变量。

示例 11. 在插入新项目时更改编号列表中的序列号
假设您在文本文件中有一个如下所示的编号列表。在本例中，假设您想在第 2 条之后添加一个新行。为此，您应该相应地更改所有其他文章的数量。

vi / vim tips & tricks series
Article 1: Vi and Vim Editor: 3 Steps To Enable Thesaurus Option
Article 2: Vim Autocommand: 3 Steps to Add Custom Header To Your File
Article 3: 5 Awesome Examples For Automatic Word Completion Using Ctrl-X
Article 4: Vi and Vim Macro Tutorial: How To Record and Play
Article 5: Tutorial: Make Vim as Your C/C++ IDE Using c.vim Plugin
Article 6: How To Add Bookmarks Inside Vim Editor
Article 7: Make Vim as Your Bash-IDE Using bash-support Plugin
Article 8: 3 Powerful Musketeers Of Vim Editor ? Macro, Mark and Map
Article 9: 8 Essential Vim Editor Navigation Fundamentals
Article 10: Vim Editor: How to Correct Spelling Mistakes Automatically
Article 11: Transfer the Power of Vim Editor to Thunderbird for Email
Article 12: Convert Vim Editor to Beautiful Source Code Browser

第 3 篇文章“使用 perl-support.vim 插件使 Vim 成为您的 Perl IDE”。所以当你想
增加它时，你要把“第3条”改为“第4条”，将“第4条”改为“第5条”，将“第12条”改为“第13条”。

这可以通过以下 vim 替换命令来实现。

:4,$s/\d\+/\=submatch(0) + 1/

范围：4,$ – 第 4 行到最后一行。
搜索模式 - \d\+ - 数字序列
要替换的模式 - \=submatch(0) + 1 - 获取匹配的模式并将其加 1。
标志 - 因为没有标志，默认情况下它只替换第一次出现。

执行substitute语句后文件会变成这样，可以
添加第3条。

vi / vim tips & tricks series
Article 1: Vi and Vim Editor: 3 Steps To Enable Thesaurus Option
Article 2: Vim Autocommand: 3 Steps to Add Custom Header To Your File
Article 4: 5 Awesome Examples For Automatic Word Completion Using Ctrl-X
Article 5: Vi and Vim Macro Tutorial: How To Record and Play
Article 6: Tutorial: Make Vim as Your C/C++ IDE Using c.vim Plugin
Article 7: How To Add Bookmarks Inside Vim Editor
Article 8: Make Vim as Your Bash-IDE Using bash-support Plugin
Article 9: 3 Powerful Musketeers Of Vim Editor ? Macro, Mark and Map
Article 10: 8 Essential Vim Editor Navigation Fundamentals
Article 11: Vim Editor: How to Correct Spelling Mistakes Automatically
Article 12: Transfer the Power of Vim Editor to Thunderbird for Email
Article 13: Convert Vim Editor to Beautiful Source Code Browser
注意：检查替换将 3 改为 4、4 改为 5 等。现在我们可以添加一个新行作为第 3 条提及它，而无需进行任何手动更改。

示例 12. 替换以大写开头的句子。（即标题案例整个文件）。
在格式化文档时，标题大小写也是一件重要的事情。它可以通过替换轻松完成。

:%s/\.\s*\w/\=toupper(submatch(0))/g

\.\s*\w – 搜索模式 – 文字。( 点 ) 后跟零个或多个空格，以及一个单词字符。
toupper – 将给定的文本转换为大写。
submatch(0) – 返回匹配的模式。
替换前的文字：
Lot of vi/vim tips and tricks are available at hgst.com.cn. reading
these articles will make you very productive. following activities can be
done very easily using vim editor.
        a. source code walk through,
        b. record and play command executions,
        c. making the vim editor as ide for several languages,
        d. and several other @ vi/vim tips & tricks.
替换后的文本（更改以粗体显示）
Lot of vi/vim tips and tricks are available at hgst.com.cn. Reading
these articles will make you very productive. Following activities can be
done very easily using vim editor.
        a. Source code walk through,
        b. Record and play command executions,
        c. Making the vim editor as ide for several languages,
        d. And several other @ vi/vim tips & tricks.
