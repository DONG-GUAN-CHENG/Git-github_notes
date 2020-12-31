# Git-github_notes_學習歷程及基礎概念  
### Author:DONG GUAN-CHENG  
***  
這裡將講解大部份最常被使用的Git基本指令。 讓我們能夠去``組態及初始化一個版本控制倉庫(repository)``、``開始及停止追蹤檔案(track)、暫存(stage)及提交(commit)更新``。還會提到如何讓``Git忽略某些檔案``、``如何輕鬆且很快地救回失誤``、``如何瀏覽讀者的專案歷史及觀看各個已提交的更新之間的變更``、以及如何從遠端版本控制倉庫拉``(pull)``更新下來或將更新推``(push)``上去。


## 取得Git版本控制倉庫 ##

一般可以使用兩種主要的方法取得一個Git版本控制倉庫。 第一種是將現有的專案或者目錄匯入Git。 第二種從其它伺服器複製一份已存在的Git版本控制倉庫。

### (1) 在現有目錄初始化版本控制倉庫 ###

若要開始使用 Git 追蹤現有的專案，只需要進入該專案的目錄並執行：

	$ git init

這個命令會建立名為 `.git` 的子目錄，該目錄包含一個Git版本控制倉庫架構必要的所有檔案。 目前來說，專案內任何檔案都還沒有被追蹤。  
若想要開始對現有的檔案開始做版本控制（除了空的目錄以外），也許應該開始追蹤這些檔案並做第一次的提交。 能以少數的git add命令指定要追蹤的檔案，並將它們提交：  

	$ git add *.python
	$ git add README_bill
	$ git commit -m 'initial project version'

執行完畢大約只需要一分鐘。 現在，已經有個追蹤部份檔案及第一次提交內容的Git版本控制倉庫。

### (2) 複製現有的版本控制倉庫 ###

若想要取得現有的Git版本控制倉庫的複本，那需要使用的命令是 `git clone`。 若熟悉其它版本控制系統，例如：Subversion，應該注意這個命令是複製(clone)，而不是取出特定版本(checkout)。 這一點非常重要，Git取得的是大部份伺服器端所有的資料複本。 該專案歷史中所有檔案的所有版本都在讀者執行過 `git clone` 後拉回來。 事實上，若伺服器的磁碟機損毀，可使用任何一個客戶端的複本還原伺服器為當初取得該複本的狀態（可能會遺失一些僅存於伺服器的攔截程式，不過所有版本的資料都健在)。

可以利用 `git clone [url]`，複製一個版本控制倉庫。 例如：

	$ git clone https://github.com/DONG-GUAN-CHENG/Git-github_notes.git

接下來會有個名為`Git-github_notes`的目錄被建立，並在其下初始化名為`.git`的目錄。 拉下所有存在該版本控制倉庫的所有資料，並取出最新版本為工作複本。 若讀者進入新建立的 `grit` 目錄，會看到專案的檔案都在這兒，且可使用。 若想要複製版本控制倉庫到Git-github_notes以外其它名字的目錄，只需要在下一個參數指定即可：

	$ git clone https://github.com/DONG-GUAN-CHENG/Git-github_notes.git mygit

這個命令做的事大致如同上一個命令，只不過目的目錄名會被更改為mygit。

Git提供很多種協定使用。 上一個範例採用 `git://` 協定，讀者可能會看過 `http(s)://` 或者 `user@server:/path.git` 等使用 SSH 傳輸的協定。  

## (3) 提交更新到版本控制倉庫 ##

現在有一個Git版本控制倉庫，而且有一份已放到工作複本的該專案的檔案。 需要做一些修改並提交這些更動的快照到版本控制倉庫，當這些修改到達想要記錄狀態的情況。

記住工作目錄內的每個檔案可能為兩種狀態的任一種：``追蹤``或者``尚未被追蹤``。  
被追蹤的檔案是最近的快照；它們可被復原、修改，或者暫存。  
未被追蹤的檔案則是其它未在最近快照也未被暫存的任何檔案。 當第一次複製保存器時，讀者所有檔案都是被追蹤且未被修改的。 因為剛取出它們而且尚未做任何修改。

只要編輯任何已被追蹤的檔案。 Git將它們視為被更動的，因為將它們改成與最後一次提交不同。 暫存這些已更動檔案並提供所有被暫存的更新。 

### (4) 檢視檔案的狀態 ###

主要用來檢視檔案的狀態是 ``git status ``命令。 若讀者在複製完複本後馬上執行此命令，會看到如下的文字：

	(base) dongde-MacBook-Pro:Git-github_notes dongguan-cheng $ git status
	On branch main
	Your branch is up to date with 'origin/main'.
	nothing to commit, working tree clean

Wokring directory clean意謂著目前的工作目錄沒有未被追蹤或已被修改的檔案。Git未看到任何未被追蹤的檔案，否則會將它們列出。 最後，這個命令告訴我們目前在哪一個分支(branch)。到目前為止，一直都是master，這是預設的。之後會詳細介紹分支(branch)，目前我們先不考慮它。

假設新增一些檔案到專案，如`README`。 若該檔案先前並不存在，執行 `git status` 命令後，將會看到未被追蹤的檔案，如下：

	$ vim bill
	$ git status
	On branch main
	Your branch is up to date with 'origin/main'.

	Untracked files:
  	(use "git add <file>..." to include in what will be committed)
	.bill.swp

	nothing added to commit but untracked files present (use "git add" to track)


我們可以看到新增的`bill`尚未被追蹤，因為它被列在輸出訊息的 Untracked files 下方。 除非我們明確指定要將該檔案加入提交的快照，Git不會主動將它加入。這樣可以避免加入一些二進位格式的檔案或其它使用者不想列入追蹤的檔案。 不過在這個例子中，我們下一步的確是要將 `bill` 檔案加入追蹤:  

### (5) 把新檔案列入追蹤並暫存 ###

要追蹤新增的檔案，我們可以使用`git add`命令。例如:要追蹤`bill`檔案，可執行：

	$ git add .bill.swp

如此一來，我們重新檢查狀態(status)時，可看到`bill`檔案已被列入追蹤並且已被暫存：

	$ git status
	On branch main
	Your branch is up to date with 'origin/main'.

	Changes to be committed:
  	(use "git restore --staged <file>..." to unstage)
	new file:   .bill.swp

	
因為它被放在Changes to be commited文字下方，我們可得知它已被暫存起來。 若此時提交更新，剛才執行`git add`加進來的檔案就會被記錄在歷史的快照。 這裡可回想一下先前執行`git init`後也有執行過`git add`，開始追蹤目錄內的檔案。`git add`命令可接受檔名或者目錄名。 若是目錄名，Git會以``遞迴(recursive)``的方式會將整個目錄下所有檔案及子目錄都加進來。
