#简介
EverEdit有着优异的语法着色引擎，EE的着色引擎可以高亮现存的绝大多数的编程语言。在EE中有Region和Item两个概念，Region表示着不同的区块。而Item则代表着这些区块中不同的部分。更为难能可贵的是EE的Region支持空匹配，也就是说您可以指定某个特定的位置用来表达Region的起始或者结束，比如行头和行尾甚至于某个特定的列，从而可以完美的高亮markdown等语言的文件。同时Item还支持捕获分组，可以极大地提高定义语法分析文件的效率

在EE的安装目录的[syntax]文件夹中已包含数个语法着色引擎文件，您可以参照它们写出更加优美正确的着色引擎！
 
#颜色定义
在您定义语法文件的时候，一定会想到要把不同的部分显示为不同的颜色。EE内置多种颜色，赶快让您的文件多彩起来吧！
注意：颜色的具体设定是在主题里面指定的。

``` 
Const COLOR_DEFAULT       =0
Const COLOR_COMMENT1      =1
Const COLOR_COMMENT2      =2
Const COLOR_STRING1       =3
Const COLOR_STRING2       =4
Const COLOR_TAG           =5
Const COLOR_MACRO         =6
Const COLOR_URL           =7
Const COLOR_EMAIL         =8
Const COLOR_NUMBER        =9
Const COLOR_FOUND         =10
Const COLOR_PAIR          =11
Const COLOR_FUNCTION      =12
Const COLOR_VAR           =13
Const COLOR_SUBLAN        =14
Const COLOR_OPERATOR      =15
Const COLOR_WORD1         =16
Const COLOR_WORD2         =17
Const COLOR_WORD3         =18
Const COLOR_WORD4         =19
Const COLOR_HIGHLIGHT1    =20
Const COLOR_HIGHLIGHT2    =21
Const COLOR_HIGHLIGHT3    =22
Const COLOR_HIGHLIGHT4    =23
Const COLOR_HIGHLIGHT5    =24
Const COLOR_HIGHLIGHT6    =25
Const COLOR_HIGHLIGHT7    =26
Const COLOR_HIGHLIGHT8    =27
Const COLOR_IGNORE        =29
Const COLOR_CONCEAL       =30
```

#COLOR_IGNORE和COLOR_CONCEAL
凡是被指定为COLOR_IGNORE模式的字符串，它的前景色将和主界面的背景色一致，看起来好像不存在一样。只有被选择的时候，才会看到！而COLOR_CONCEAL则是一个比较有意思的模式，被指定为COLOR_CONCEAL的字符仅当处于选区或者当前行的时候，才是可见的！借助这个特性，可以实现出很多非常有用的功能！
 
#脚本系统
###SyntaxItem

```
//函数
void Capture(int group, int state);
//属性
string Name;//get,set
```

###WordItem

```
//属性
bool AutoCase;//get,set
string Name;//get,set
```

###SyntaxRegion

```
//函数
bool AddSnippet(string strName, bool bCase=true)
void AddItem(SyntaxItem item );
void AddWord(WordItem item );
void AddRegion(SyntaxRegion child );
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
void CommentBlock(string strOn, string strOff);
void CommentLine(string strText);
void SetPairs(string strText);
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );

//属性
void TransRegion;//set
string Name;//get;set
```

####重要函数说明
```
bool AddSnippet(string strName, bool bCase=true)
添加snippet到该Region。strName为snippet的文件名称，因所有的snippet文件必须放到snippet文件夹，所以只需要提供snippet的文件名称即可。bCase:snippet在匹配的时候是否区分大小写。
```

```
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
指定语法文件的折叠方法。strFold和strUnFold指示了折叠和反折叠的正则表达式。bFCase/bUFCase:是否匹配大小写
```

```
void CommentBlock(string strOn, string strOff);
块注释的文本。主要用于CS+/的调用。比如c++中，可以这么设置 cpp.CommentBlock "/*", “*/"
```

```
void CommentLine(string strText);
行注释的文本。用户C+/的调用。比如c++中，可以这么设置 cpp.CommentLine "//"
```

```
void SetPairs(string strText);
指示该Region可用的配对字符。strText:包含所有的配对字符。配对字符必须连续的放在一块定义，很显然strText的长度应为偶数。
```

```
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
定义用于缩进和反缩进的正则表达式。
```

```
TransRegion
当该区域的文本结束时，该Region默认情况下将会自动跳转到父Region；如果定义了TransRegion那么将会跳转到Region。
```

```
Name
该Region的名称。
```
###Parser

```
//函数
bool AddSnippet(string strPathName );
void AddItem(SyntaxItem item );
void AddWord(WordItem item );
void AddRegion(SyntaxRegion child);
SyntaxRegion CopyRegion(SyntaxRegion pCopy);
SyntaxItem CreateItem(int state, string strMatch, bool bCase, bool bToRight=false);
WordItem CreateWord(int state, string strMatch, bool bCase, string strDelimiters="");
SyntaxRegion CreateRegion(int state, string strBegin, string strEnd, bool bCase, bool bToRight=false );
SyntaxRegion CreateStringRegion(int state, string strChar, string strEscape, bool mline );
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
void FoldAnyText(int nLevel, string strText);
void SetPairs(string strText);
void CommentBlock(string strOn, string strOff);
void CommentLine(string strText);
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );
 
//属性
string Name;//get,set
string FoldingMethod;//get,set
string WordChars;//get,set
SyntaxRegion DefaultRegion;//get
```

####重要函数说明
Parser默认情况下包含了一个DefaultRegion。部分针对Default Region函数的使用可以参考SyntaxRegion的说明。

```
SyntaxRegion CopyRegion(SyntaxRegion pCopy);
复制一个Region。用于某两个不相关的Region可能包含类似的代码。该函数较耗费资源。
```

```
SyntaxItem CreateItem(int state, string strMatch, bool bCase, bool bToRight=false);
创建正则表达式描述的匹配规则。
state:颜色值。
strMatch:正则描述的匹配文本
bCase:是否区分大小写
bToRight:如果该匹配正好在行末，那么背景色是否延伸到窗口右侧。
```

```
WordItem CreateWord(int state, string strMatch, bool bCase, string strDelimiters="");
创建关键字匹配。理论上CreateItem可以代替CreateWord，但CreateWord的效率更高且可以自动完成和自动纠正大小写。

strMatch:以空格分割的关键字字符串。
bCase:是否区分大小写
strDelimiters:默认的情况下strMatch所匹配的文本只包括字母数字和下划线，
strDelimiters则表示哪些特殊字符可以被当作一个词，比如中划线-等。
```

```
SyntaxRegion CreateRegion(int state, string strBegin, string strEnd, bool bCase, bool bToRight=false );
创建一个正则表达式描述的Region。

strBegin/strEnd:描述该区域开始或者结束的正则表达式
bCase:strBegin/strEnd描述的正则是否区分大小写
bToRight:如果该Region在行末,那么扩展背景色到右侧窗口

注意:被+两个加号+包围起来的文本表示这不是一个正则表达式，就是普通的文字匹配(效率更高)。
```

```
SyntaxRegion CreateStringRegion(int state, string strChar, string strEscape, bool mline );
创建字符串匹配。
strChar:字符串指示字符，通常为"或者'。
strEscape:转义字符，通常为\
mline:字符串是否跨行。
```

```
void FoldAnyText(int nLevel, string strText);
正则表达式描述的较为宽松的折叠。
nLevel:折叠的层级
strText:正则表达式
```

```
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );
详情参考calltip说明
```

```
Name
默认区域的命名
```

```
FoldingMethod
该语法文件的折叠方法。取值为syntax/indent/anytext
```

```
WordChars
该语法文件可以被认为是单词的特殊字符。当鼠标双击或者其它操作选择词汇时将会使用该设置。
```

#实例1:创建C++着色器
以C语言为例，描述一下如何去定义一个新的语法描述文件！那么，首先让我们看一下C语言中常见的语法元素:
 
* 单行注释
* 多行注释
* 字符串
* 关键字
 
绝大多数的语法文件均是由上面几个非常简单的元素构成。好了，开始我们的自定义吧！建立一个新的文件mycpp。mac并把它放到EE的syntax目录下，好让EE可以找到它。为了使用颜色值，我们可以把EE已经为大家定义好的cosnt。mac包含进来，当然您也可以直接把该文件的内容粘贴到这个文件当中。

```
Include( ".\const.mac" )
```

创建一个自定义的Parser，命名为cpp。

```
Set cpp=Parser.CreateParser()
```

在创建完自定义Parser之后,我们就要开始为之添加各种各样的元素了，Come On!

**创建单行注释匹配**：单行注释很显然是一个Region，而不是一个Item，因为它有开始和结束的标志！那就是以//开始，以行尾结束！

``` 
Set regionLine=cpp.CreateRegion( COLOR_COMMENT1, "+//+", "$", True )
```

**注意**：这儿我们使用了++, 其中被+包围了, 表示这儿为了提高效率不使用正则表达式进行匹配! 结束标志我们把它定义成$, 明确表示匹配到行尾, 不然的话只会匹配//了! 
 
**创建多行注释匹配**：

```
Set regionBlock=cpp.CreateRegion( COLOR_COMMENT1, "+/*+", "+*/+", True )
```

**创建字符串匹配**：

``` 
Set regionString=cpp.CreateStringRegion( COLOR_STRING1, """", "\", False )
```

**创建关键字匹配**：

```
Set itemWord=cpp.CreateItem(COLOR_WORD1, "\b(int|float|double|char|void|
for|while|if|else|return|break|continue)\b", True)
```

**注意**：为了更好的创建关键字匹配, EE增加了**CreateWord**函数, 比如上文中的关键字匹配, 也可以这么写，效率更高：

``` 
Set itemWord2=cpp.CreateWord(COLOR_WORD1, "int float double char void for while if else return break continue", True)
```

**把所有创建的匹配加入到主区域中**：

``` 
cpp.AddRegion( regionLine )
cpp.AddRegion( regionBlock )
cpp.AddRegion( regionString )
cpp.AddItem( itemWord )
```

到这儿为止我们就自定义了一个简单的C语言着色引擎，看看成品吧！

```
Include( ".\const.mac" )
Set cpp=Parser.CreateParser()
Set regionLine=cpp.CreateRegion( COLOR_COMMENT1, "+//+", "", True )
Set regionBlock=cpp.CreateRegion( COLOR_COMMENT1, "+/*+", "+*/+", True )
Set regionString=cpp.CreateStringRegion( COLOR_STRING1, """", "\", False )
Set itemWord=cpp.CreateItem(COLOR_WORD1, "\b(int|float|double|char|void|for|while|if|else|return|break|continue)\b", True)
cpp.AddRegion( regionLine )
cpp.AddRegion( regionBlock )
cpp.AddRegion( regionString )
cpp.AddItem( itemWord )
```

别着急，还剩下最后一步。那就是让EE可以识别它！打开语法着色对话框，新建一文件类型为MyCpp，并选择语法文件为刚刚定义的mycpp.mac！
 
#实例2：细化C++着色器
在实例1中我们学习了如何创造一个自定义的着色引擎, 那么让我们来进一步丰富这个引擎吧!首先, 我想让注释中所有大写的TODO都显示为一种特殊的颜色! 注意仅仅是注释中, 其它的部分不变!
 
**定义TODO匹配**:

```
Set itemTodo=cpp.CreateItem(COLOR_HIGHLIGHT1, "\bTODO\b", True)
```

**把TODO匹配加入到单行和多行注释中**:

```
regionLine.AddItem( itemTodo )
regionBlock.AddItem( itemTodo )
```

这样我们就可以仅仅在注释中显示高亮为COLOR_HIGHLIGHT1颜色的TODO文字了！EE同样为代码折叠提供了简单方便的调用接口。比如当遇到{时折叠一次，遇到}时反折叠一次，那么可以简单的像下面书写：

```
cpp.FoldText "\{", False, "\}", False
```

**自动缩进**：假设在mycpp中有这样一种缩进：当您在行尾是EEA的时候，输入回车时，自动缩进一次；输入的行满足含有EverEdit时，自动反缩进一次

``` 
cpp.IndentText "EEA$ ", False, "EVEREDIT", False
```

**括号匹配**：我们定义[],{},(),"",''是匹配的字符。

``` 
cpp.SetPairs "[]{}()""""''"
```

**可以被快捷键调用的注释**:

``` 
cpp.CommentLine "//"
cpp.CommentBlock "/*", "*/"
```

到这儿为止，我们为mycpp添加了TODO高亮;添加了缩进和反缩进，添加了括号匹配和代码折叠，同时还添加了可以被快捷键或者菜单命令调用的注释和反注释的匹配！EverEdit以其强大的功能，为您提供了大量的自定义特性！赶快动手试一下吧！
