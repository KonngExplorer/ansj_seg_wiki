```
package org.ansj.demo;

import java.io.File;

import org.ansj.library.UserDefineLibrary;

import love.cq.domain.Forest;
import love.cq.domain.Value;
import love.cq.library.Library;

/**
 * 重新加载用户自定义辞典的两种方式
 * @author ansj
 *
 */
public class ReloadAmbiguityLibrary {
    public static void main(String[] args) throws Exception {
        //从文件中reload
        loadFormFile();
        //通过内存中reload
        loadFormStr();
    }

    private static void loadFormStr() {
        // TODO Auto-generated method stub
        Forest forest = new Forest() ;
        
        Value value = new Value("三个和尚","三个","m","和尚","n") ;
        Library.insertWord(forest, value) ;
      //将新构建的辞典树替换掉旧的。
        UserDefineLibrary.ambiguityForest = forest ;
    }

    private static void loadFormFile() throws Exception {
        // TODO Auto-generated method stub
        //make new forest
        Forest forest = Library.makeForest("new_Library_Path") ;

        //将新构建的辞典树替换掉舊的。
        UserDefineLibrary.ambiguityForest = forest;
    }
}
```