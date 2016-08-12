Java项目开发中，常常需要在编码文件上面加上一些版权声明或者类注释，如果文件很多，手工去添加或者修改，会很麻烦。可以利用工具满足我们的要求。
#一、版权声明
可以使用Jautodoc。将jautodoc的plugin和feature目录对应copy到eclipse目录中，并且带-clean参数重新启动一次eclipse（以后不再需要该参数）。到eclipse的window->preference菜单中，java- jautodoc，configure project specific settings中，选择add file header，并点【edit】按钮，输入版权信息，保存。
以后要添加版权时，按alt+shift+J，就可以了。
###类似的版权信息如下：
    /**
    *@项目名称: ${project_name} 
    
    *@文件名称: ${file_name}
      *@Date: ${date}
    *@Copyright: ${year} www.xxx.com Inc. All rights reserved.
    *注意：本内容仅限于xxx公司内部传阅，禁止外泄以及用于其他的商业目的
    */
其中${project_name},${file_name}和${date}、${year} 是内置的变量，在编辑模板的时候，输入$之后，会有很多类似的变量提示显示出来。
#二、类注释，方法注释等
可以直接使用eclipse的code style功能。
 
类注释在window-reference-java-code style-code templates-comments，选中types，然后点击【edit】按钮，输入类注释信息，保存。

以后要添加版权时，按Ctrl+alt+J(快捷方式可以在Window-preference-general-keys查看和修改)，就可以了。

####类似的类注释如下：
    /**
    *@Project: ${project_name}
    *@Author: Vin
    *@Date: ${date}
    *@Copyright: ${year} www.xxx.com Inc. All rights reserved.
    */
同样，这里的${date}也是预定义的变量 。

window-preference-java-code style-code templates-comments，选中types，可以编辑类的注释，
window-preference-java-code style-code templates-comments，选中files，可编辑类文件的注释
window-preference-java-code style-code templates-comments，选中methods，可编辑方法的注释
等等。

在window-preference-java-code style-code templates-code，编辑new class files，加入

    ${filecomment}
    ${package_declaration}
    ${typecomment}
    ${type_declaration}

可以在新建一个类文件的时候自动加入前面设置的文件注释和类注释。
方法注释的操作与类注释的操作类似。可以自己研究。

最后，要使用这些注释，在window-preference-java-code style，将Automatically add comments for new methods and types选上才可应用到开发中。
或者将光标放在需要加注释的方法或类里按Alt+Shift+J键 