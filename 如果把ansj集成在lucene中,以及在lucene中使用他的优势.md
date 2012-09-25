也没什么优势.-_-!

呵呵.目前支持英文时态,复数的统一.同意转换为小写.支持停用词典.<red>未来会增加近义词词典</red>

下面我来介绍如何使用吧.

首先来到这里 https://github.com/ansjsun/ansj_seg/tree/master/contrib .
下载 ansj_lucene_plug.jar ,ansj_seg.jar

ansj_lucene_plug.jar 是lucene插件的jar包
ansj_seg.jar 是核心源码
吧这两个jar加入到环境变量中.当然你要使用lucene需要lucene的jar.我就不废话了,
然后你可以写代码了
<pre><code>
package org.ansj.splitWord.impl;

import java.io.BufferedReader;
import java.io.IOException;
import java.util.HashSet;
import java.util.ResourceBundle;

import love.cq.util.IOUtil;

import org.ansj.lucene3.AnsjAnalysis;
import org.apache.lucene.analysis.Analyzer;
import org.apache.lucene.document.Document;
import org.apache.lucene.document.Field;
import org.apache.lucene.index.CorruptIndexException;
import org.apache.lucene.index.IndexWriter;
import org.apache.lucene.index.IndexWriterConfig;
import org.apache.lucene.queryParser.ParseException;
import org.apache.lucene.queryParser.QueryParser;
import org.apache.lucene.search.IndexSearcher;
import org.apache.lucene.search.TopDocs;
import org.apache.lucene.store.Directory;
import org.apache.lucene.store.RAMDirectory;
import org.apache.lucene.util.Version;

public class LucenePlugTest {
	public static void main(String args[]) throws CorruptIndexException, IOException, ParseException {
		//停用词典
		HashSet<String> hs = new HashSet<String>();
		BufferedReader reader2 = IOUtil.getReader(ResourceBundle.getBundle("library").getString("stopLibrary"), "UTF-8");
		String word = null;
		while ((word = reader2.readLine()) != null) {
			hs.add(word);
		}
		Analyzer analyzer = new AnsjAnalysis(hs);
		Directory directory = null;
		IndexWriter iwriter = null;
		IndexSearcher isearcher = null;

		String text = "我从小java csdn person books就不由自主地认为自己长大以后一定得成为一个象我父亲一样的画家, 可能是父母潜移默化的影响。其实我根本不知道作为画家意味着什么，我是否喜欢，最重要的是否适合我，我是否有这个才华。其实人到中年的我还是不确定我最喜欢什么，最想做的是什么？我相信很多人和我一样有同样的烦恼。毕竟不是每个人都能成为作文里的宇航员，科学家和大教授。知道自己适合做什么，喜欢做什么，能做好什么其实是个非常困难的问题。"
				+ "幸运的是，我想我的孩子不会为这个太过烦恼。通过老大，我慢慢发现美国高中的一个重要功能就是帮助学生分析他们的专长和兴趣，从而帮助他们选择大学的专业和未来的职业。我觉得帮助一个未成形的孩子找到她未来成长的方向是个非常重要的过程。"
				+ "美国高中都有专门的职业顾问，通过接触不同的课程，和各种心理，个性，兴趣很多方面的问答来帮助每个学生找到最感兴趣的专业。这样的教育一般是要到高年级才开始， 可老大因为今年上计算机的课程就是研究一个职业走向的软件项目，所以她提前做了这些考试和面试。看来以后这样的教育会慢慢由电脑来测试了。老大带回家了一些试卷，我挑出一些给大家看看。这门课她花了2个多月才做完，这里只是很小的一部分。"
				+ "在测试里有这样的一些问题："
				+ "你是个喜欢动手的人吗？ 你喜欢修东西吗？你喜欢体育运动吗？你喜欢在室外工作吗？你是个喜欢思考的人吗？你喜欢数学和科学课吗？你喜欢一个人工作吗？你对自己的智力自信吗？你的创造能力很强吗？你喜欢艺术，音乐和戏剧吗？  你喜欢自由自在的工作环境吗？你喜欢尝试新的东西吗？ 你喜欢帮助别人吗？你喜欢教别人吗？你喜欢和机器和工具打交道吗？你喜欢当领导吗？你喜欢组织活动吗？你什么和数字打交道吗？";

		IndexWriterConfig ic = new IndexWriterConfig(Version.LUCENE_32, analyzer);
		// 建立内存索引对象
		directory = new RAMDirectory();
		iwriter = new IndexWriter(directory, ic);
		addContent(iwriter, text);
		iwriter.commit();
		iwriter.close();

		System.out.println("索引建立完毕");

		search(analyzer, directory, "book");
		search(analyzer, directory, "java");
		search(analyzer, directory, "找不到");
	}

	private static void search(Analyzer analyzer, Directory directory, String query) throws CorruptIndexException, IOException, ParseException {
		IndexSearcher isearcher;
		// 查询索引
		isearcher = new IndexSearcher(directory);
		QueryParser tq = new QueryParser(Version.LUCENE_32, "text", analyzer);
		TopDocs hits = isearcher.search(tq.parse(query), 5);
		System.out.println("共找到:" + hits.totalHits + "条记录!");
		for (int i = 0; i < hits.scoreDocs.length; i++) {
			int docId = hits.scoreDocs[i].doc;
			Document document = isearcher.doc(docId);
			System.out.println(document);
		}
	}

	private static void addContent(IndexWriter iwriter, String text) throws CorruptIndexException, IOException {
		Document doc = new Document();
		doc.add(new Field("text", text, Field.Store.YES, Field.Index.ANALYZED));
		iwriter.addDocument(doc);
	}
}

</code></pre>


注意你在运行时候不出意外会出现 <red>用户自定义词典:library/userLibrary/userLibrary.dic, 没有这个文件!</red>

这是用户自定义词典的没有找到.如果你需要此功能.就按照路径加入用户自定义词典.用户自定义词典的格式是 word[tab]nature[tab]freq

example: 孙健	nr	100