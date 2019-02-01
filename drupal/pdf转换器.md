## 程序运行

### 首先要安装python3的PyMuPDF库

直接在命令行中运行一下即可

```
pip3 install PyMuPDF
```



### 要在paths.txt里面写路径（程序运行前要先修改路径）

在paths.txt里面填写要转换的图片路径，和保存pdf的路径，比如

```
picsPath = /home/bruce/workspace_all/workspace_for_pycharm/pics2pdf/pics
storePath = /home/bruce/Desktop/drupal
```

> 1. picsPath就是要转换的图片路径，这个路径的类似下图
>
>    ![](https://ws1.sinaimg.cn/large/891f7782gy1fxxbow9ruvj207t02zweb.jpg)
>
>    其中，pics这个文件夹中可以包含文件夹pdf1和pdf2.jpeg
>
> 2. storePath就是要保存的文件路径，保存程序运行后保存结果为：
>
> ![](https://ws1.sinaimg.cn/large/891f7782gy1fxxbr3tkruj208x04y3yl.jpg)

### 运行程序

```bash
python3 main.py
```

