直接看例子.这个功能其实在tree-split中就已经存在的.

说明:

1..删除只能删除用户自定义词典的词.

2.对了分词默认把大写都转换为小写了.所以添加新词的时候要求必须是小写.
<pre><code>
public class DynamicWordDemo {
	public static void main(String[] args) {
		//增加新词,中间按照'\t'隔开
		UserDefineLibrary.insertWord("ansj中文分词	userDefine	1000") ;
		List<Term> terms = ToAnalysis.paser("我觉得ansj中文分词是一个不错的系统!我是王婆!") ;
		new NatureRecognition(terms).recogntion() ;
		System.out.println("增加新词例子:"+terms);		
		//删除词语,只能删除.用户自定义的词典.
		UserDefineLibrary.removeWord("ansj中文分词") ;
		terms = ToAnalysis.paser("我觉得ansj中文分词是一个不错的系统!我是王婆!") ;
		new NatureRecognition(terms).recogntion() ;
		System.out.println("删除用户自定义词典例子:"+terms);
	}
</pre></code>