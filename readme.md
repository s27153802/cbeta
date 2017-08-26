# cbeta 大藏经(大正藏)开源阅读计划

因为cbeta是用C++写的，我这里没有正版windows，所以一直是把cbeta光盘中的xml拷贝下来，自己处理之后阅读。
最近cbeta的xml格式发生了变化。所以重写了我的程序。随便共享出来给有需要的人。

## 主要目的

1. 是能在线阅读大藏经,所以主要精力放在排版阅读上面

## cbeta存在的问题
1. 使用图片显示悉昙字,蘭扎字，使用 组字式显示异体字,显示效果差
2. 光盘版是C++写的，Windows only
3. 光盘版不稳定，容易死机，速度也不理想
4. 原始xml文件的质量存在问题，需要做一个清洗

## 技术方式和预期效果

1. 速度.目前来看相比CBETA速度快上很多。完全不消耗服务器资源。对浏览器的消耗也非常小。速度非常快。CBETA在多处有卡顿的现象。原因不明，阅读体验不佳
目前唯一发现有些微卡顿的是' 胎藏梵字真言上卷', (T18/T18n0854_001.xml),用时0.5秒，原因是使用了太多的图片和文件比较大所导致;优化速度后最大的文件'朝鮮佛教通史'(B31n0170_003.xml)可以在1秒之内处理完毕
2. 代码非常少。修改部署容易
3. 正确的显示经文
4. 简体字与繁体字都可以正常搜索内容
5. 自制的悉昙字字体已经正常, 使用Unicode10.0的码表来显现, ttf文件只有9k大小
6. 整体程序非常简单。只有一个tei.xsl文件和一个tei.css文件作为显示效果。而阅读效果比较好。修改简单方便，适合长期阅读藏经使用。

## 技术方式

1. 使用xslt来处理xml, 然后使用在xml中直接嵌入xslt的方式, 就可以直接在浏览器中阅读了。
如果希望自己使用的人, 可以自己搭建一个nginx服务器。将静态文件指向xml和tei.xsl,tei.css文件所在目录即可

2. 使用python来处理其他事情, 以及动态生成xslt文件， 主要是生成目录

3. 尽量少的使用js控制， 对搜索引擎友好

4. 在线直接阅读xml文件. 不单独生成html，占用空间小。其他格式文件也可以直接在线生成

## 使用说明

1. 首先把cbeta中的xml文件拷贝出来到一个目录比如TMP，然后拷贝static/tei.xsl和static/tei.css两个文件到TMP目录下的stylesheet目录。
2. 启动nginx服务器，设置静态目录为TMP。一个不错的阅经环境就设置完成了

## 文件列表
1. static/tei.xsl 主体程序
2. static/tei.css css效果文件
3. static/siddham.ttf 符合unicode10.0的悉昙体字库
4. static/siddham.sfd 符合unicode10.0的悉昙体字库的fontforg文件，可以根据这个文件继续修改字库
5. static/siddham.woff 符合unicode10.0的悉昙体字库,可以通过webfont方式使用悉昙体字库，以便读者不用安装字库即可阅读悉昙体
6. terms.txt  佛教词汇大全，目前搜集了不到8万词汇，用来给藏经分词用的, 以便全文检索使用
7. w_normal.txt 制作的组合字表,相比原光盘的更加清晰, 用来清洗xml文档
8. w_norm2.txt 制作的未知组合字表, 需要填写
9. yoga 目录,打算后期为瑜伽师地论做现代化标点

## 浏览器兼容性
1. firefox、opera、chrome、safari、搜狗浏览器、IOS手机等主流浏览器都没有发现任何问题
2. Edge 12、ie11在处理xslt失败之后使用了一个空的xslt处理xml,估计ms具有特殊的xslt格式
3. ie8, 360浏览器卡死，无法使用
4. links2无法显示汉字， w3m和lynx可以正常显示汉字，但是xml则只显示xml文件，没有处理。
5. 因为导航需要探测文件存在与否，所有出现文件不存在的警告是正常的

## 工作到一半的时候发现已经有了
http://cbetaonline.dila.edu.tw/zh/T1579_011
但是技术没有公开, 而且还有显示问题

## TODO
1. 目前只能阅读， 目录还没有加上   # DONE!
2. 使用whoosh方案做全文搜索，支持复杂的查询语法, 使查询变得更加简单
3. 做一个组字式的替换表，以便在阅读中显示正常汉字
4. 删除悉昙字的图片，使用Unicode10.0的字来显现。目前字体做了一半，不会做那种可以移动的符号
5. 删除蘭扎字的图片，暂时设想使用悉昙字的变体来实现
6. 删除异体字和组字式
7. 删除g标签中的错误
8. 对悉昙体叠辅音的支持，暂时没空做了

# 查询使用的语法
1. 关键字是AND、OR、NOT。搜索域是title、author, 可以随意组合,使用()
2. 例如搜索阿含经中的一段话,使用如下语句:  比丘集讲堂 AND title:阿含经

## 目前存在的问题
1.搜索生成的索引文件太大了, 接近4G，查找一次使用时间太长，需要15～20秒。搜索结果不理想
2.异体字的注释目前使用了一段js，不知道是不是有办法去掉

## 联系方式
可以通过wenping_zhao@126.com与我联系使用中的问题

