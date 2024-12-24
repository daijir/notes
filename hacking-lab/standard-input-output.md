# Standard Input and Output

```Bash
//明示的
cp file1 file2

//暗黙的
co -v file1 file2
//output -> 'file1' -> 'file2'
```
この'file1' -> 'file2'は操作者は出力先を指定していない  
出力先はcpが指定している    

暗黙的な入力 -> 標準入力 0  
暗黙的な出力 -> 標準出力 1  暗黙的なエラー出力 -> 標準エラー出力 2    

リダイレクト -> 出力先を変更する仕組み  
```Bash
//上書き
comand > file

//追記
command >> file

