# trailing whitespace

### Git 的 core.autocrlf 參數

Git 的 core.autocrlf 參數到底該設為 true 還是 false，網路上有不少討論，而且意見分歧。我第一次用 Git，也有這個疑問。爬文之後，我的理解是這樣：
基本上，如果你的專案會同時在不同的作業系統（如 Windows、Linux）上存取，最好是設定為 true。這表示每次 checkin 時，Git 會將純文字類型的檔案中的所有 CRLF 字元轉換為 LF，也就是版本庫中的換行符號一律存成 LF；在 checkout 時，則會將 LF 轉換成目前作業系統的換行符號，例如在 Windows 上面就是轉成 CRLF。這種設定有兩個風險：

1. 若檔案中同時包含 LF 和 CRLF，那麼經過一進一出的轉換之後，就會造成與原始檔案內容不同。這等於是篡改或破壞了原始檔案。
2. 若 Git 將某個二進位檔案誤判為純文字檔（機率不高，但確實有可能），也會破壞檔案的內容。

由於我自己的專案並沒有跨 OS 平台存取的需要，所以將 autocrlf 設為 false，也就是告訴 Git：別幫我轉換 CRLF 字元。
**2013-10-18 更新**：Git 換行字元設定所可能引發的災難，我寫在另一篇筆記：[Git 在 Windows 平台的換行字元](http://huan-lin.blogspot.com/2013/10/git-windows.html)。
**對特定 repository 設定－方法 1**
如欲對特定 reqository 設定 autocrlf，指令是：
$ git config core.autocrlf false
上述指令是僅針對目前這個 repository 所做的設定。你可以在它的根目錄下的 .git\\config 檔案中找到 core.autocrlf 的設定項目。
**對特定 repository 設定－方法 2**
用文字編輯器修改 repository 根目錄下的 .gitattributes 檔案，修改 text 屬性。例如：
**\* text=auto**
這是預設值，亦即如果沒有另外指定哪些檔案類型要採用何種換行策略，就會使用此預設值：auto。也就是說，預設讓 git 自動處理換行字元的轉換。這個屬性的其他設定值以及範例可以看這裡：[Dealing with line endings](https://help.github.com/articles/dealing-with-line-endings)。
在經歷一次慘痛的教訓之後，我的建議是把 auto 關掉（不要自動轉換）。也就是把上面那行改成這樣：
**\* -text**
注意：.gitattributes 檔案必須 commit 至檔案庫中；其設定會蓋掉 core.autocrlf 參數的設定。
**全域設定**
如果要更改全域設定，可將先前方法 1 的指令加上 --global 參數，如下：
$ git config --global core.autocrlf false
全域組態檔所在的目錄就是環境變數 %HOMEPATH% 指向的目錄，例如 C:\\Users\\<使用者帳戶>，在此目錄下有個名為 .gitconfig 的檔案，就是全域組態檔。剛安裝好 Git 時，可能不會有這個檔案，但你只要執行過任何 git config --global 指令來修改參數，就會產生 .gitconfig 檔案。
**Re-normalizing Repository**
一旦修改了 core.autocrlf 或 .gitattributes 的設定，就必須對 repository 做一次性的重新正規化，以確保前後端 repository 的全部檔案都使用一致的換行字元。作法同樣看這篇：[Dealing with line endings](https://help.github.com/articles/dealing-with-line-endings)。
