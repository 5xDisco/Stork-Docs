# Git pull後 modified異常暴增

## 問題主述
每當pull（或fetch）一個分支新進度至本機，Git即判定所有新檔案（包括多數非本機異動過的）皆為modified，造成後續合併、push前後有大量（假）衝突。

其後，僅能以GUI方法（如SourceTree）繞過強制commit要求。

![](https://i.imgur.com/PhkKajG.png)

## 解決方案

1. pull後執行命令　**git config --global core.autocrlf input**
2. 若無效，找出專案中的「.gitattributes」檔案，將「* text=auto」註解掉。


參見：https://stackoverflow.com/questions/5009096/files-showing-as-modified-directly-after-a-git-clone


###### tags: `問題和解決`