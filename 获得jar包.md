#下载jar包

## 下载地址

>可以从 [https://mvnrepository.com/artifact/org.ansj/ansj_seg](https://mvnrepository.com/artifact/org.ansj/ansj_seg)下载你需要的jar包

## 下载步骤

>1. 让分词运行需要两个jar，分别是 [nlp-lang](https://mvnrepository.com/artifact/org.nlpcn/nlp-lang) 和 [ansj_seg](https://mvnrepository.com/artifact/org.ansj/ansj_seg) 的jar 。建议每次下载保证这两个jar是最新的。
2. 然后将jar导入到classpath中。应该不用多说了吧。你懂的

#用maven导入


2. 在dependencies标签中粘贴如下:(其实version 以最新的为标准.)

````xml
	<dependencies>
		....
		<dependency>
			<groupId>org.ansj</groupId>
			<artifactId>ansj_seg</artifactId>
			<version>5.1.6</version>
		</dependency>
		....
	</dependencies>
````



### 注意

1. jar的编译版本是jdk1.6 。
<p></p>
2. 如果是1.1之后的版本。拥有两个jar ansj_seg-[version].jar **标准版*** 和  ansj_seg-[version]-**min**.jar **简洁版**。</br>
   <p>这两个jar的区别，标准版本。比简洁版本。大的多的多。标准版一般是40m左右。简洁版大约是4m左右。</p>
   <p>标准版提供了NlpAnalysis 方式。也就是具有更加的新词发现以及新词识别的能力。</p>
   <p>但是为了部署成本。以及内存耗费。如果不是用来作自然语言处理的童鞋。建议用简洁版本。绿色环保。</p>
   
   
