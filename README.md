# Git-github_notes_學習歷程及基礎概念  
### Author:DONG GUAN-CHENG  
***  
這裡將講解大部份最常被使用的Git基本指令。 讓我們能夠去``組態及初始化一個版本控制倉庫(repository)``、``開始及停止追蹤檔案(track)、暫存(stage)及提交(commit)更新``。還會提到如何讓``Git忽略某些檔案``、``如何輕鬆且很快地救回失誤``、``如何瀏覽讀者的專案歷史及觀看各個已提交的更新之間的變更``、以及如何從遠端版本控制倉庫拉``(pull)``更新下來或將更新推``(push)``上去。


## 取得Git版本控制倉庫並提交到github ##

一般可以使用兩種主要的方法取得一個Git版本控制倉庫。 第一種是將現有的專案或者目錄匯入Git。 第二種從其它伺服器複製一份已存在的Git版本控制倉庫。

### (1) 在現有目錄初始化版本控制倉庫 ###

若要開始使用 Git 追蹤現有的專案，只需要進入該專案(以C++ file為例)的目錄並執行：

	$ cd Ｃ++
	$ git init
	Initialized empty Git repository in /Users/dongguan-cheng/Ｃ++/.git/


這個命令會建立名為 `.git` 的子目錄，該目錄包含一個Git版本控制倉庫架構必要的所有檔案。 目前來說，專案內任何檔案都還沒有被追蹤。  
若想要開始對現有的檔案開始做版本控制（除了空的目錄以外），也許應該開始追蹤這些檔案並做第一次的提交。 能以少數的git add命令指定要追蹤的檔案，並將它們提交：  

	$ git add *.python
	$ git add README_bill
	$ git commit -m 'initial project version'
ps:假若為git add . ->.代表新增資料夾下所有檔案  

ex:完整流程

	echo "# knkn" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git branch -M main
	git remote add origin git@github.com:DONG-GUAN-CHENG/knkn.git
	git push -u origin main

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

### (6) 暫存已修改檔案 ###

讓我們修改已被追蹤的檔案。 若修改先前已被追蹤的檔案，名為`billmodefy`，並檢查目前版本控制倉庫的狀態。`billmodify`檔案出現在 “Changes not staged for commit” 下方，代表著這個檔案已被追蹤，而且位於工作目錄的該檔案已被修改，但尚未暫存。 要暫存該檔案，可執行`git add`命令（這是一個多重用途的指令）。現在，再使用 `git add` 將`billmodify`檔案暫存起來，並再度執行`git status`，可發現`billmodify`被列入追蹤並且被暫存  

接下來假設仍需要對`billmodify`做一點修改後才要提交，可再度開啟並編輯該檔案。 然而，當我們再度執行`git status`，會發現`Changes not staged for commit:`下方又會出現`billmodify`  

而現在`billmodify`同時被列在已被暫存及未被暫存。 這表示Git的確在執行`git add`命令後，將檔案暫存起來。 若現在提交更新，最近一次執行`git add`命令時暫存的`billmodify`會被提交。 若在`git add`後修改檔案，需要再度執行`git add`才能將最新版的檔案暫存起來  

### (7) 忽略某些檔案 ###

通常會有一類不想讓Git自動新增，也不希望它們被列入未被追蹤的檔案。 這些通常是自動產生的檔案，例如：記錄檔或者編譯系統產生的檔案。 在這情況下，可建立一個名為`.gitignore`檔案，列出符合這些檔案檔名的特徵。 以下是一個範例：

	$ cat .gitignore
	*.[oa]
	*~

第一列告訴Git忽略任何檔名為`.o`或`.a`結尾的檔案，它們是可能是編譯系統建置的程式碼時產生的目的檔及程式庫。 第二列告訴Git忽略所有檔名為~結尾的檔案，通常被很多文書編輯器，如：Emacs，使用的暫存檔案。 可能會想一併將log、tmp、pid目錄及自動產生的文件等也一併加進來。 依據類推。在要開始開發之前將`.gitignore`設定好，通常是一個不錯的點子。這樣子不會意外地將真的不想追蹤的檔案提交到Git版本控制倉庫。

編寫`.gitignore`檔案的規則如下：

*	空白列或者以#開頭的列會被忽略。
*	可使用標準的Glob pattern。
*	可以/結尾，代表是目錄。
*	可使用!符號將特徵反過來使用。

Glob pattern就像是shell使用的簡化版正規運算式。 星號（`*`）匹配零個或多個字元；`[abc]`匹配中括弧內的任一字元（此例為`a`、`b`、`c`）；問號（`?`）匹配單一個字元；中括孤內的字以連字符連接（如：`[0-9]`），用來匹配任何符合該範圍的字（此例為0到9）。

以下是另一個`.gitignore`的範例檔案：

	# 註解，會被忽略。
	# 不要追蹤檔名為 .a 結尾的檔案
	*.a
	# 但是要追蹤 lib.a，即使上方已指定忽略所有的 .a 檔案
	!lib.a
	# 只忽略根目錄下的 TODO 檔案。 不包含子目錄下的 TODO
	/TODO
	# 忽略build/目錄下所有檔案
	build/
	# 忽略doc/notes.txt但不包含doc/server/arch.txt
	doc/*.txt
	# ignore all .txt files in the doc/ directory
	doc/**/*.txt

### (8) 檢視已暫存及尚未暫存的更動 ###

在某些情況下，`git status`指令提供的資訊就太過簡要。
有的時候我們不只想知道那些檔案被更動，而是想更進一步知道檔案的內容被做了那些修改，這時我們可以使用`git diff`命令。使用它時通常會是為了瞭解兩個問題：目前已做的修改但尚未暫存的內容是哪些？以及將被提交的暫存資料有哪些？儘管`git status`指令可以大略回答這些問題，但`git diff`可顯示檔案裡的哪些列被加入或刪除，以修補檔(patch)方式表達。  

### (9) 提交修改 ###

現在的暫存區域已被更新為我們想要的檔案，再來可開始提交變更的部份。 要記得任何尚未被暫存的新增檔案或已被修改但尚未使用`git add`暫存的檔案將不會被記錄在本次的提交中。 它們仍會以被修改的檔案的身份存在磁碟中。
在這情況下，最後一次執行`git status`，會看到所有已被暫存的檔案，也準備好要提交修改。 最簡單的提交是執行`git commit`：

	$ git commit

執行此命令會叫出指定的編輯器。（由shell的$EDITOR環境變數指定，通常是vim或emacs。）

編輯器會顯示如下文字（此範例為Vim的畫面）：


	# Please enter the commit message for your changes. Lines starting
	# with '#' will be ignored, and an empty message aborts the commit.
	#
	# On branch main
	# Your branch is up to date with 'origin/main'.
	#
	# Changes to be committed:
	#       new file:   .bill.swp
	#       new file:   README888.md
	#
	# Untracked files:
	#       .DS_Store
	#
	~                                                                               
	~                                                                               
	~                                                                               
	~                                                                               
	~                                                                               

	"~/Git-github_notes/.git/COMMIT_EDITMSG" 14L, 318C

可看到預設的提交訊息包含最近一次`git status`的輸出以註解方式呈現，以及螢幕最上方有一列空白列。 可移除這些註解後再輸入提交的訊息，或者保留它們，提醒你現在正在進行提交。（若想知道更動的內容，可傳遞-v參數給`git commit`。如此一來連比對的結果也會一併顯示在編輯器內，方便明確看到有什麼變更。） 當離開編輯器，Git會利用這些提交訊息產生新的提交（註解及比對的結果會先被濾除）。

另一種方式則是在commit命令後方以`-m`參數指定提交訊息來補充說明，如下：

	$ git commit -m "change"
	[main 5ca297d] change
	 2 files changed, 4 insertions(+)
	 create mode 100644 .bill.swp
	 create mode 100644 README888.md

現在已建立第一個提交！ 我們可從輸出的訊息看到此提交、放到哪個分支（`master`）、SHA-1查核碼（`5ca297d`）、有多少檔案被更動，以及統計此提交有多少列被新增及移除。

記得提交記錄放在暫存區的快照。 任何未暫存的仍然保持在已被修改狀態；仍可進行其它的提交，將它增加到歷史。 每一次執行提交，都是記錄專案的快照，而且以後可用來比對或者復原。  

### (10) 直接跳過暫存區域並提交 ###

雖然優秀好用的暫存區域能很有技巧且精確的提交讀者想記錄的資訊，有時候暫存區域也比實際需要的工作流程繁瑣。 若想跳過暫存區域，Git提供了簡易的使用方式。 在`git commit`命令後方加上`-a`參數，Git``自動將所有已被追蹤且被修改的檔案送到暫存區域並開始提交程序``，讓讀者略過`git add`的步驟：

	$ git commit -a-m "change"


### (11) 刪除檔案 ###

要從Git刪除檔案，需要將它從已被追蹤檔案中移除（更精確的來說，是從暫存區域移除），並且提交。 `git rm`命令除了完成此工作外，也會將該檔案從工作目錄移除。 因此以後不會在未被追蹤檔案列表看到它。

若僅僅是將檔案從工作目錄移除 (rm)，那麼在`git status`的輸出，可看見該檔案將會被視為“已被變更且尚未被更新”（也就是尚未存到暫存區域）：

	$ rm README888.md 
	$ git status
	On branch main
	Your branch is ahead of 'origin/main' by 1 commit.
	  (use "git push" to publish your local commits)

	Changes not staged for commit:
	  (use "git add/rm <file>..." to update what will be committed)
	  (use "git restore <file>..." to discard changes in working directory)
		deleted:    README888.md
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
		.DS_Store

	no changes added to commit (use "git add" and/or "git commit -a")

接著，若執行`git rm`，則會將暫存區域內的該檔案移除：

	$ git rm README888.md
	rm 'README888.md'
	$ ls
	LICENSE		README.md
	$ git status
	On branch main
	Your branch is ahead of 'origin/main' by 1 commit.
	  (use "git push" to publish your local commits)

	Changes to be committed:
	  (use "git restore --staged <file>..." to unstage)
		deleted:    README888.md

下一次提交時，該檔案將會消失而且不再被追蹤。 若已更動過該檔案且將它記錄到暫存區域。 必須使用`-f`參數才能將它強制移除。 這是為了避免已被記錄的快照意外被移除且再也無法使用Git復原。

其它有用的技巧是保留工作目錄內的檔案，但從暫存區域移除。 換句話說，或許讀者想在磁碟機上的檔案且不希望Git繼續追蹤它。 這在讀者忘記將某些檔案記錄到`.gitignore`且不小心將它增加到暫存區域時特別有用。 比如說：巨大的記錄檔、或大量在編譯時期產生的`.a`檔案。 欲使用此功能，加上`--cached`參數：

	$ git rm --cached readme.txt

除了檔名、目錄名以外，還可以指定簡化的正規運算式給`git rm`命令。 這意謂著可執行類似下列指令：

	$ git rm log/\*.log

注意星號（`*`）前方的倒斜線（`\`）。 這是必須的，因為Git會在shell以上執行檔案的擴展。 此命令移除log目錄下所有檔名以`.log`結尾的檔案。 讀者也可以執行類似下列命令：

	$ git rm \*~

此命令移除所有檔名以`~`結尾的檔案。

***
## (12) 與遠端協同工作 ##

想要在任何Git控管的專案協同作業，需要瞭解如何管理遠端的版本控制倉庫。 遠端版本控制倉庫是置放在網際網路或網路其它地方中的專案版本。 可設定多個遠端版本控制倉庫，具備唯讀或可讀寫的權限。 與他人協同作業時，需要管理這些遠端版本控制倉庫，並在需要分享工作時上傳或下載資料。
管理遠端版本控制倉庫包含瞭解如何新增遠端版本控制倉庫、移除已失效的版本控制倉庫、管理許多分支及定義是否要追蹤它們等等。 由此開始講述一些常見的遠端管理的技巧。

### (13) 顯示所有的遠端版本控制倉庫 ###

欲瞭解目前已加進來的遠端版本控制倉庫，可執行 `git remote` 命令。 它會列出當初加入遠端版本控制倉庫時指定的名稱。 若目前所在版本控制倉庫是從其它版本控制倉庫複製過來的，至少應該看到 *origin*，也就是 Git 複製版本控制倉庫時預設名字：
	
	$ git remote
	origin

也可以再加上 `-v` 參數，將會在名稱後方顯示其URL：

	$ git remote -v
	origin	https://github.com/DONG-GUAN-CHENG/Git-github_notes.git (fetch)
	origin	https://github.com/DONG-GUAN-CHENG/Git-github_notes.git (push)

若有一個以上遠端版本控制倉庫，此命令會全部列出。 

	$ cd grit
	$ git remote -vgi


這意謂著我可以很容易從這些伙伴的版本控制倉庫取得最新的更新。 要留意的是只有 origin 遠端的 URL 是 SSH。 因此它是唯一我能上傳的遠端的版本控制倉庫。

### (14) 新增遠端版本控制倉庫 ###

在先前章節已提到並示範如何新增遠端版本控制倉庫，這邊會很明確的說明如何做這項工作。 欲新增遠端版本控制倉庫並取一個簡短的名字，執行 `git remote add [shortname] [url]`： 

	$ git remote
	origin
	$ git remote add pb git://github.com/paulboone/ticgit.git
	$ git remote -v
	origin	git://github.com/schacon/ticgit.git
	pb	git://github.com/paulboone/ticgit.git

現在可看到命令列中的 `pb` 字串取代了整個 URL。 例如，若想取得 Paul 上傳的且本地端版本控制倉庫沒有的更新，可執行 `git fetch pb`：

	$ git fetch pb
	remote: Counting objects: 58, done.
	remote: Compressing objects: 100% (41/41), done.
	remote: Total 44 (delta 24), reused 1 (delta 0)
	Unpacking objects: 100% (44/44), done.
	From git://github.com/paulboone/ticgit
	 * [new branch]      master     -> pb/master
	 * [new branch]      ticgit     -> pb/ticgit

現在可在本地端使用 `pb/master` 存取 Paul 的 master 分支。 讀者可將它合併到本地端任一分支、或者建立一個本地端的分支指向它，如果讀者想監看它。

### (15) 從遠端版本控制倉庫擷取或合併(FETCH & PULL) ###

如剛才所示，欲從遠端擷取資料，可執行：

	$ git fetch [remote-name]

此命令到該遠端專案將所有本地端沒有的資料拉下來。 在執行此動作後，應該有參考到該遠端專案所有分支的參考點，可在任何時間點用來合併或監看。
若複製了一個版本控制倉庫，會自動將該遠端版本控制倉庫命令為 *origin*。 因此 `git fetch origin` 取出所有在複製或最後一下擷取後被上傳到該版本控制倉庫的更新。 需留意的是 `fetch` 命令僅僅將資料拉到本地端的版本控制倉庫，``並未自動將它合併進來``，也沒有修改任何目前工作的專案。 得在必要時將它們手動合併進來。

若設定一個會追蹤遠端分支的分支，可使用 `git pull` 命令自動擷取及合併遠端分支到目錄的分支。 這或許是較合適的工作流程。 而且 `git clone` 命令預設情況下會自動設定本地端的 master 分支追蹤被複製的遠端版本控制倉庫的 master 分支。（假設該版本控制倉庫有 master 分支） 執行 `git pull` 一般來說會從當初複製時的來源版本控制倉庫擷取資料並自動試著合併到目前工作的版本。

### (16) 上傳到遠端版本控制倉庫(PUSH) ###

當有想分享出去的專案，可將更新上傳到上游。 執行此動作的命令很簡單：`git push [remote-name] [branch-name]`。 若想要上傳 master 分支到 `origin` 伺服器（再說一次，複製時通常自動設定此名字），接著執行以下命令即可上傳到伺服器：

	$ git push origin master

此命令只有在被複製的伺服器開放寫入權限給使用者，而且同一時間內沒有其它人在上傳。 若在其它同樣複製該伺服器的使用者上傳一些更新後上傳到上游，該上傳動作將會被拒絕。 我們必須先將其它使用者上傳的資料拉下來並整合進來後才能上傳。

### (17) 監看遠端版本控制倉庫 ###

若我們想取得遠端版本控制倉庫某部份更詳盡的資料，可執行 `git remote show [remote-name]`。 若執行此命令時加上特定的遠端名字，比如說： `origin`。 會看到類似以下輸出：

	$ git remote show origin
	* remote origin
	  Fetch URL: https://github.com/DONG-GUAN-CHENG/Git-github_notes.git
	  Push  URL: https://github.com/DONG-GUAN-CHENG/Git-github_notes.git
	  HEAD branch: main
	  Remote 
	  branch:
	    main tracked
	  Local branch configured for 'git pull':
	    main merges with remote main、
	  Local ref configured for 'git push':
	    main pushes to main (up to date)


它將同時列出遠端版本控制倉庫的URL位置和追蹤分支資訊。特別是告訴你如果你在master分支時用`git pull`時，會去自動抓取數據合併到本地的master分支。它也列出所有曾經被抓取過的遠端分支。

當你使用Git更頻繁之後，你或許會想利用 `git remote show` 去看到更多的資訊。

	$ git remote show origin
	* remote origin
	  URL: git@github.com:defunkt/github.git
	  Remote branch merged with 'git pull' while on branch issues
	    issues
	  Remote branch merged with 'git pull' while on branch master
	    master
	  New remote branches (next fetch will store in remotes/origin)
	    caching
	  Stale tracking branches (use 'git remote prune')
	    libwalker
	    walker2
	  Tracked remote branches
	    acl
	    apiv2
	    dashboard2
	    issues
	    master
	    postgres
	  Local branch pushed with 'git push'
	    master:master

這個指令顯示當你執行`git push`會自動推送的哪個分支(最後兩行)。它也顯示哪些遠端分支還沒被同步到本地端(在這個例子是caching)，哪些已同步到本地的遠端分支在遠端已被刪除(libwalker和walker2)，以及當執行`git pull`時會自動被合併的分支。

### (18) 移除或重新命名遠端版本控制倉庫 ###

在新版 Git 中可以用 `git remote rename` 命令修改某個遠端版本控制倉庫在本地的簡稱，舉例而言，想把 `pb` 改成 `paul`，可以執行下列指令：


	$ git remote 
	new
	origin
	$ git remote rename new bill
	$ git remote
	bill
	origin

值得留意的是這也改變了遠端分支的名稱，原來的 `pb/master` 分支現在變成 `paul/master`。

當你為了種種原因想要移除某個遠端，像是換伺服器或是已不再使用某個特別的鏡像，又或是某個貢獻者已不再貢獻時。你可以使用`git remote rm`：

	$ git remote rm paul
	$ git remote
	origin
