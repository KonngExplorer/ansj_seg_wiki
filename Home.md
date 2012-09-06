Welcome to the ansj_seg wiki!

上面是系统打的呵呵..欢迎使用ansj分词


我不废话了..直接上干货吧.演示下分词的调用方式,当然用他之前强烈建议你吧内存调大(不是让你买内存去).不得不说.他是个内存消耗大户.

不会调内存????其实我也不会.呵呵
在eclipse中Run As--> Run Configurations--> Arguments 找到个框框 VM Arguments写上:-Xms1024m -Xmx1024m

如果你空闲内存连1G都木有.有两个办法.1.买内存or换机器 2.删除用户词典(强烈不建议)


1.这是一个标注的调用方式
`package org.ansj.splitWord.impl;

import java.io.IOException;
import java.io.StringReader;

import org.ansj.domain.Term;
import org.ansj.splitWord.Analysis;
import org.ansj.splitWord.analysis.ToAnalysis;

/**
 * 标注的分词方式,这里面的流你可以传入任何流.除了流氓
 * @author ansj
 *
 */
public class Demo {
	public static void main(String[] args) throws IOException {
		Analysis udf = new ToAnalysis(new StringReader("Ansj中文分词是一个真正的ict的实现.并且加入了自己的一些数据结构和算法的分词.实现了高效率和高准确率的完美结合!"));
		Term term = null ;
		while((term=udf.next())!=null){
			System.out.print(term.getName()+" ");
		}
	}
}`

2.这是一个简易的调用方式
`
package org.ansj.splitWord.impl;

import java.io.IOException;
import java.io.StringReader;
import java.util.List;

import org.ansj.domain.Term;
import org.ansj.splitWord.Analysis;
import org.ansj.splitWord.analysis.ToAnalysis;

/**
 * 最最最简单的分词调用方式
 * @author ansj
 *
 */
public class SimpleDemo {
	public static void main(String[] args) throws IOException {
		List<Term> paser = ToAnalysis.paser("Ansj中文分词是一个真正的ict的实现.并且加入了自己的一些数据结构和算法的分词.实现了高效率和高准确率的完美结合!");
		System.out.println(paser);
	}
}

`


3.如何做词性标注,词性标注是需要在分词结果后调用词性标注.下面写一个简单的方式.有针对文件的词性标注特殊的处理办法.不要着急

`
package org.ansj.splitWord.impl;

import java.io.IOException;
import java.util.List;

import org.ansj.domain.Term;
import org.ansj.splitWord.analysis.ToAnalysis;
import org.ansj.util.recognition.NatureRecognition;

/**
 * 词性标注
 * @author ansj
 *
 */
public class NatureDemo {
	public static void main(String[] args) throws IOException {
		List<Term> terms = ToAnalysis.paser("Ansj中文分词是一个真正的ict的实现.并且加入了自己的一些数据结构和算法的分词.实现了高效率和高准确率的完美结合!");
		new NatureRecognition(terms).recogntion() ;
		System.out.println(terms);
	}
}

`


以上这些结果你会看到
[ansj/en, 中文/nz, 分/q, 词/n, 是/v, 一个/m, 真正/d, 的/uj, ict/en, 的/uj, 实现/v, ./m, 并且/c, 加入/v, 了/ul, 自己/r, 的/uj, 一些/m, 数据结构/userDefine, 和/c, 算法/n, 的/uj, 分词/n, ./m, 实现/v, 了/ul, 高/a, 效率/n, 和/c, 高/a, 准确率/n, 的/uj, 完美/a, 结合/v, !/null]

完毕手工.