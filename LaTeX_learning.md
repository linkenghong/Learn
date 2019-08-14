## 数学公式相关

* 数学模式：行内模式（inline） & 行间模式（display，又称独立模式）

    注意：以下方式，markdown中都只支持第一种

    * 行内模式：

        $ ... $ 或 \\( ... \\)
        
        示例：$a^2=b^2+c^2$
        
        公式中间不可有空格，空格用以下方式：

        需求 | 方法 | 效果 | 备注
        ---- | ---- | ---- | ----
        两个quad空格 | a \qquad b | $a \qquad b$ | 两个m的宽度
        quad空格 | a \quad b | $a \quad b$ | 一个m的宽度
        大空格 | a\ b | $a\ b$ | 1/3m宽度
        中等空格 | a\;b | $a\;b$ | 2/7m宽度
        小空格 | a\,b | $a\,b$ | 1/6m宽度
        没有空格 | ab | $ab\,$ |
        紧贴 | a\!b | $a\!b$ | 缩进1/6m宽度

    * 行间模式：

        * 无编号：\$$ ... $$ 或 \\[ ... \\]
        
            示例：

            $$ E=mc^2 $$

        * 有编号：\begin{equation} ... \end{equation}

            示例：
            
            \begin{equation}
            x^n+y^n=z^n
            \end{equation}

* 基本元素：

    * 希腊字母：

    
            



    


