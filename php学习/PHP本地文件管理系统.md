# **软件开发流程【了解】**

## **软件开发基本流程【了解】**	

> 网站开发制作流程：任何一个系统的开发都是一件非常复杂的事情，涉及的工作也都是方方面面的。一个大型的软件产品的开发，通常需要一个甚至多个团队合作。一个软件产品从开发到最终验收上线，通常有以下几个步骤：      
  
1.需求分析
* 相关系统分析员向用户初步了解需求，然后用相关的工具软件列出要开发的系统的大功能模块，每个大功能模块有哪些小功能模块，对于有些需求比较明确相关的界面时，在这一步里面可以初步定义好少量的界面。
* 系统分析员深入了解和分析需求，根据自己的经验和需求用WORD或相关的工具再做出一份文档系统的功能需求文档。这次的文档会清楚列出系统大致的大功能模块，大功能模块有哪些小功能模块，并且还列出相关的界面和界面功能。
* 系统分析员向用户再次确认需求。    

2.概要设计    
* 首先，开发者需要对软件系统进行概要设计，即系统设计。概要设计需要对软件系统的设计进行考虑，包括系统的基本处理流程、系统的组织结构、模块划分、功能分配、接口设计、运行设计、数据结构设计和出错处理设计等，为软件的详细设计提供基础。

3.详细设计    
* 在概要设计的基础上，开发者需要进行软件系统的详细设计。在详细设计中，描述实现具体模块所涉及到的主要算法、数据结构、类的层次结构及调用关系，需要说明软件系统各个层次中的每一个程序(每个模块或子程序)的设计考虑，以便进行编码和测试。应当保证软件的需求完全分配给整个软件。详细设计应当足够详细，能够根据详细设计报告进行编码。

4.编码    
* 在软件编码阶段，开发者根据《软件系统详细设计报告》中对数据结构、算法分析和模块实现等方面的设计要求，开始具体的编写程序工作，分别实现各模块的功能，从而实现对目标系统的功能、性能、接口、界面等方面的要求。在规范化的研发流程中，编码工作在整个项目流程里最多不会超过1/2，通常在1/3的时间，所谓磨刀不误砍柴功，设计过程完成的好，编码效率就会极大提高，编码时不同模块之间的进度协调和协作是最需要小心的，也许一个小模块的问题就可能影响了整体进度，让很多程序员因此被迫停下工作等待，这种问题在很多研发过程中都出现过。编码时的相互沟通和应急的解决手段都是相当重要的，对于程序员而言，bug永远存在，你必须永远面对这个问题，大名鼎鼎的微软，可曾有连续三个月不发补丁的时候吗？从来没有！

5.测试    
* 测试编写好的系统。交给用户使用，用户使用后一个一个的确认每个功能。软件测试有很多种：按照测试执行方，可以分为内部测试和外部测试；按照测试范围，可以分为模块测试和整体联调；按照测试条件，可以分为正常操作情况测试和异常情况测试；按照测试的输入范围，可以分为全覆盖测试和抽样测试。以上都很好理解，不再解释。总之，测试同样是项目研发中一个相当重要的步骤，对于一个大型软件，3个月到1年的外部测试都是正常的，因为永远都会有不可预料的问题存在。完成测试后，完成验收并完成最后的一些帮助文档，整体项目才算告一段落，当然日后少不了升级，修补等等工作，只要不是想通过一锤子买卖骗钱，就要不停的跟踪软件的运营状况并持续修补升级，直到这个软件被彻底淘汰为止。

6.软件交付
* 在软件测试证明软件达到要求后，软件开发者应向用户提交开发的目标安装程序、数据库的数据字典、《用户安装手册》、《用户使用指南》、需求报告、设计报告、测试报告等双方合同约定的产物。
《用户安装手册》应详细介绍安装软件对运行环境的要求、安装软件的定义和内容、在客户端、服务器端及中间件的具体安装步骤、安装后的系统配置。
《用户使用指南》应包括软件各项功能的使用流程、操作步骤、相应业务介绍、特殊提示和注意事项等方面的内容，在需要时还应举例说明。

7.验收
* 用户验收：用户按照需求对功能进行验收。

8.维护
* 根据用户需求的变化或环境的变化，对应用程序进行全部或部分的修改。

> **总结**：软件开发的标准流程是非常复杂的，因为一个号的软件产品从需求到产品上线，这个中间涉及的环节是非常多的，需求也是不断的变化，要根据实际的需求进行很多调整。而且在整个开发过程中，涉及的产物也非常多，有需求文档、需求确认文档、概要设计文档、详细设计文档、原型图、ER图、开发文档、产品使用文档、运维报告等等。

****

## **Web开发基本流程【了解】**
> **定义**：Web开发也属于应用软件开发，本质上应该遵循软件产品的开发流程，只是Web应用（网站）作为一种比较简单的软件开发，为了达到快速上线占领市场，会把流程做到简化一些。  
  
1.需求分析：要做什么，能否实现？完成功能确认    
2.原型设计：简单效果展示，完成效果确认    
3.开发设计：将原型设计的效果使用技术实现
* 开发语言
* 开发团队
* 开发模式
* 系统架构
* 开发周期

4.软件测试：测试网站各项功能是否能够满足需求
* 单元测试
* 集成测试
* 白盒测试
* 黑盒测试

5.网站部署：将系统部署到对应的服务器环境，实现上线服务    
6.运维：检测系统运行状况，随时处理出现的异常

> **总结**：其实国内的很多开发，尤其是小公司，并没有开发流程这一套，更多的时候就是给定一个需求，然后讨论一下时间节点，中间开发的时候有个过程报告，最后就实现了产品的交互。但是按照流程开发的话，能够让整个开发过程变得有序进行，而且过程透明，利于监控进度。因此，如果企业没有特别追求产品上线周期的时候，尽量按照开发规范来设计网站。

****

# **PHP本地文件管理系统【掌握】**

> **思考**：现在有这么一个需求，想有一个网站，能够在网站里去输入对应的路径（HTML是客户端运行，因此不支持直接获取客户端路径），就能够看到对应的路径下的所有文件信息，并且能够实现增删改查操作呢？

> **引入**：这种需求本质上是不能支持客户操作的，因为PHP只能操作本地的文件，没有技术能够去收集客户端电脑的文件信息（太不安全）。因此，这种需求可以考虑是实现一个写好的专门用户本地电脑上文件管理的功能。

## **前端显示设计**
> **定义**：所谓前端显示设计，就是设计一个最终显示的效果界面，这种界面就是静态的，利用HTML、CSS和JavaScript完成

1.功能分析：没有直接获取路径的表单，因此需要一个能够让用户输入数据并且提交的表单
```HTML
<form action="获取列表的PHP文件" method="GET">
输入目录：<input type="text" name="dir" value=""/>
<input type="submit" name="submit" value="提交"/>
</form>
```
2.功能分析：需要给用户显示所有的对应目录的文件信息，可以使用表格完成。表格中应该有序号列、文件名列（缩进）、文件类型列和操作列（增删改三项操作）
```HTML
<table border=1>
    <p sytle="text-align:center">XXX目录文件列表</p>
    <tr>
        <th>序号</th>
        <th>文件名字</th>
        <th>文件类型</th>
        <th>操作</th>
    </tr>
    <tr>
        <td>1</td>
        <td>index.php</td>
        <td>文件</td>
        <td><a href="">删除</a>|<a href="#">重命名</a></td>    
    </tr>
    <tr>
        <td>2</td>
        <td>Web</td>
        <td>文件夹</td>
        <td><a href="#">新增</a>|<a href="#">删除</a>|<a href="#">重命名</a></a></td>    
    </tr>
</table>
```
3.功能分析：新增文件表单需要用户指定是文件夹还是文件操作：注意文件操作是带路径的，因此需要提供要操作的具体目录
```HTML
<h1>显示要新增文件的目录</h1>
<form action="新增文件的PHP文件" method="POST">
<input type="hidden" name="dir" value="要新增文件的目录">
<input type="text" name="filename" value=""/>
文件夹<input type="radio" name="type" value="1"/>
文件<input type="radio" name="type" value="2"/>
<input type="submit" name="submit" value="提交"/>
</form>
```
4.功能分析：重命名文件就是显示文件本身的名字，然后给定对应的新名字：注意修改文件需要提供路径和原来文件的名字
```HTML
<h1>显示要重命名的文件路径以及名字</h1>
<form action="修改名字的PHP文件" method="POST">
<input type="hidden" name="dir" value="原文件路径"/>
<input type="hidden" name="filename" value="原文件名字"/>
新名字：<input type="text" name="newname" value=""/>
<input type="submit" name="submit" value="提交修改"/>
</form>
```

> **总结**：虽然本地文件操作在显示的时候已经列出了所能操作的功能，但是实际上因为新增和重命名操作需要涉及到新数据提交，因此需要对应的新的表单文件。

****

> **引入**：前端界面已经涉及好，那么主要要看后台的实现了。后台的实现可以从需求一步一步去实现即可。因为当前功能比较简单，可以考虑将所有的文件放到一个文件夹下面。

## **后端PHP功能实现【掌握】**

1.显示功能：用户第一次访问的界面应该是输入目标的表单：然后提交给后台，后台PHP文件进行处理
* 确定后台显示目录所有文件的PHP脚本名字：getfile.php
* 修改用户表单提交对象action为getfile.php
* getfile.php接收用户提交的目录：$_GET['dir']
* 判定路径的有效性：无效提示路径无效并返回路径提交表单
* 调用函数获取所有的文件信息（已经封装好的另外一个PHP文件file.php），得到数组
* 加载显示所有文件的表单文件
* 在显示文件的表单数据中，显示所有的文件（.和..没有必要）

2.删除功能：删除是在显示的基础上，增加删除链接，然后点击实现删除
* 修改显示列表中对应的删除a标签的href属性，指向后台删除文件的PHP文件delete.php（携带路径和文件名字）
* 增加delete.php文件
* delete.php接收用户要删除的文件信息（原路径），并判断文件路径是否正确
* 判断文件类型，执行删除操作
* 提示删除结果，并返回到原显示目录文件的PHP文件：getfile.php

3.新增功能：新增是在显示的基础上，对应的文件夹才有新增功能。新增功能通常分为两个部分：第一个部分是显示新增表单，第二部分是提交新增内容    
第一部分：显示新增表单
* 修改显示表单中新增a标签的href属性，指向后台新增文件的PHP文件add.php（携带路径和名字）
* 在add.php中接收对应的路径信息和文件夹名字信息
* 加载新增表单文件
* 在HTML中显示必要的信息

第二部分：新增提交
* 修改表单要提交对象action为create.php
* 在create.php文件中，接收用户要新增的文件信息，包括路径，文件夹名字和新文件类型，文件名字
* 判定有效性
* 根据类型创建文件或者文件夹
* 提示用户结果，并跳转到原文件显示PHP文件：getfile.php

4.修改功能：修改也是在显示的基础上，实现文件的重命名。编辑操作通常也分为两个部分：第一个部分显示原来信息，第二个部分是提交修改    
第一部分：显示修改表单
* 修改显示列表中对应的重命名a标签的href属性，指向后台修改文件的PHP文件edit.php（携带路径和名字）
* 在edit.php中接收数据信息：原路径，原文件名字
* 加载编辑文件的模板文件
* 在模板文件中显示对应的信息

第二部分：执行重命名操作
* 修改重命名表单的action为PHP重命名文件rename.php
* 在rename.php中接收相关信息，包括路径、原文件名字和新文件名字
* 判断路径的有效性
* 执行重命名操作
* 将重命名结果提示给用户，并返回到到原文件显示PHP文件：getfile.php

> **总结**：其实所有的功能开发最终的代码实现无外乎就是各种增删改查，只是开发的时候会根据不同业务作出不同的判断逻辑，以保证代码的正确执行以及功能的实现。