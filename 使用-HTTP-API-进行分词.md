# 摘要
ansj_seg 内置了一个用来提供分词 API 的 HttpServer，类名为 org.ansj.web.AnsjServer。

# 启动服务
在开发环境中，可以通过 mvn 运行分词服务：

mvn exec:java -Dexec.mainClass="org.ansj.web.AnsjServer" -Dexec.args="<端口号>"

在 staging 或生产环境中，运行 org.ansj.webAnsjServer 类即可，如：

java -classpath "ansj_seg.jar" org.ansj.web.AnsjServer <端口号>

# API 调用
假设端口号为 16000。

一个调用例子为：http://127.0.0.1:16009/segment?input=自然语言处理&method=to&nature=true

## input
待分词的文本。

## method
使用的分词方法，可以为 to, nlp 或 base。对应 ansj_seg 的 analysis 中的三种分词方法。

默认为 to。

## nature
是否启用词性标注，true 代表启用，false 代表禁用。

默认为 true。