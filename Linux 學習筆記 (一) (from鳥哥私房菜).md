# Linux 學習筆記 (一) (from鳥哥私房菜)

## 
1. 最純種的UNIX: BSD(Berkeley Software Distribution), SystemV
2. GNU: GNU is NOT Unix, 非洲牛羚, GNU的核心=Hurd, https://www.gnu.org/
## Linux Kernel
- 2.6.x: 偶數版，為穩定版，適用於商業套件上。
- 2.5.x: 奇數版，為發展測試版，提供工程師一些先進開發的功能。

1. Android 的版本搭配的 Linux 核心版本為何？
- Android的目標是Linux內核的4.4、4.9或是4.14版本。
2. 由 Linux kernel 官網的『Releases』相關說明，找出現階段的 Linux Mainline, Stable, Longterm 版本各有哪些？ mainline: 5.17-rc4, stable: 5.16.9, longterm: 5.15.23
## Linux Distribution
- RPM 軟體管理: 商業公司 - RHEL, SuSE // 社群單位 - Fedora, CentOS, OpenSuSE
- DPKG 軟體管理: 商業公司 - Ubuntu // 社群單位 - Debian, B2D
## 離開系統與關閉系統
- 檢察系統上的用戶狀態: ```w```
- USER欄位為登入的使用者
- TTY: 終端機，通常為 tty1~tty6。但 tty1 使用圖形介面登入時，會顯示 ```:0```
- WHAT: 終端機目前使用的指令為何。
- 關機指令: ```poweroff``` ```halt``` ```shutdown -h now``` ```systemctl poweroff```

## 文字模式指令下達方式 (--help 兩個-)
EX: 
- 小時:分鐘: ```date +%H/%M```
- 顯示兩天以前: ```date --date='2 days ago' +%Y/%m/%d```
- 西元年-月-日 小時:分鐘: ```date +'%F %H:%M'``` (利用引號去加空格)
- 顯示月曆: ```cal```

## 身分切換 ```su -``` 的使用

## 語系功能變換
- ```LANG=en_US.utf8``` (zh_TW.utf8, zh_TW.big5)
- ```echo ${LANG}``` = 查閱目前語系
- 純文字模式 (tty2~tty6) 情況下，資料量太大，沒有滑鼠滾輪可以利用， 此時可以使用哪些組合按鈕來顯示之前的螢幕畫面？ ``` command + 上下鍵```
- 若想要讓命令提示字元出現在第一行 (螢幕最上方)，可以輸入那一個指令來清空畫面？ ```clear```

## 常見的熱鍵與組合按鍵
- ```tab``` 命令/檔名/變數名稱補齊
- ```ctrl+c``` 中斷一個運作中的指令
- ```shift+pageup``` ```shift+pagedown``` 上下移動螢幕畫面
- 我想要知道，到底有多少變數是由 H 開頭的？如何使用 echo 去查閱？
```
echo $H 按tab
```

## 線上求助方式
- ```bc``` 小算盤
- ```man``` 手冊頁
```
方向鍵上下: 向文件前後移動一行
g: go to fisrt line
G: go to last line
q: quit
/keyword: search for keyword
n: downsearch the keyword
N: upsearch the keyword
```
- man page
```
代號    代表內容
1	使用者在shell環境中可以操作的指令或可執行檔
2	系統核心可呼叫的函數與工具等
3	一些常用的函數(function)與函式庫(library)，大部分為C的函式庫(libc)
4	裝置檔案的說明，通常在/dev下的檔案
5	設定檔或者是某些檔案的格式
6	遊戲(games)
7	慣例與協定等，例如Linux檔案系統、網路協定、ASCII code等等的說明
8	系統管理員可用的管理指令
9	跟kernel有關的文件
```
- 我們知道 passwd 有兩個地方存在，一個是設定檔 /etc/passwd，一個是變更密碼的指令 /usr/bin/passwd， 如何分別查詢兩個資料的 man page 呢？
```
# /etc/passwd

man 5 passwd

# /usr/bin/passwd

man 1 passwd
```

## Linux 目錄樹系統簡介 - 檔案階層是架構(Filesystem Hierarchy Standard, FHS)
```
/bin        主要放置一般用戶可操作的指令
/sbin       主要放置系統管理員可操作的指令
            這兩個資料目前都是連結檔，分別連結到 /usr/bin, /usr/sbin 當中
            
/boot	    與開機有關的檔案，包括核心檔案 / 開機管理程式與設定檔
/dev	    是 device 的縮寫，放置裝置檔，包括硬碟檔、鍵盤滑鼠終端機檔案等

/etc	    一堆系統設定檔，包括帳號、密碼與各式服務軟體的設定檔大多在此目錄內
/home       一般帳號的家目錄預設放置位置
/root       系統管理員的家目錄！

/lib        系統函式庫與核心函式庫，/lib 包含核心驅動程式。
/lib64      而其他軟體的函式庫若為 64 位元，則使用 /lib64 目錄內的函式庫檔案。 
            這兩個目錄目前也都是連結到 /usr/lib, /usr/lib64 內。

/proc       將記憶體內的資料做成檔案類型，放置於這個目錄下，連同某些核心參數也能手動調整。
/sys        跟 /proc 類似，只是比較針對硬體相關的參數方面。

/usr        是 usr 不是 user 喔！是 unix software resource 的縮寫，與 Unix 程式有關。從 CentOS 7 開始， 系統相關的所有軟體、服務等，均放置在這個目錄中了！因此不能與根目錄分離。
/var        是一些變動資料，系統運作過程中的服務資料、暫存資料、登錄資料等等。
/tmp        一些使用者操作過程中會啟用的暫存檔，例如 X 軟體相關的資料等等。
```
```
/media      主要是系統上臨時掛載使用的裝置(如隨插即用 USB)之慣用目錄
/mnt        主要是使用者或管理員自行暫時手動掛載的目錄
/opt        optional 的意思，通常是第三方協力廠商所開發的軟體放置處
/run        系統進行服務軟體運作管理的功能，CentOS 7以後，這個目錄也放在記憶體當中了！
/srv        通常是給各類服務 (service) 放置資料使用的目錄
```

## 觀察目錄本身的參數
```
ll dirname        顯示該目錄底下的內容
ll -d dirname     顯示該目錄本身的資訊
```

## 刪除檔案
```
rm
-r: 刪除目錄
-f: 不詢問直接刪除
```

## 觀察檔案類型
```
file
ex: file /etc/passwd /usr/bin/passwd
```

## 大量建置空白檔案的方法
```
touch test{1,2,3,4}
touch test{1..10} (test1 ~ test10)
```
- 我需要在 /dev/shm/testing 目錄下建立名為 mytest_XX_YY_ZZ 的檔案，其中 XX 為 jan, feb, mar, apr 四個資料， YY 為 one, two, three 三個資料，而 ZZ 為 a1, b1, c1 三個資料，如何使用一個指令就建立出上述的 36 個檔案？
```
mkdir /dev/shm/testing
cd /dev/shm/testing
touch mytest_{jan,feb,mar,apr}_{one,two,three}_{a1,b1,c1}
```
- 我需要在 /dev/shm/student/ 目錄下，建立檔名為 4070C001 到 4070C050 的檔案，如何使用一個指令來完成這 50 個檔案的建置？
```
mkdir /dev/shm/student
cd /dev/shm/student
touch 4070C0{01..50}
```

## 檔案內容查詢
```
cat: 將檔案內容全部列出
head: 預設只列出檔案最前面10行
tail: 預設只列出檔案最後面10行
-n: 加上行號
```

## 檔案屬性與權限修改方式
```
chmod: change file permission
chown: change file owner
chgrp: change file group
```
- 使用 chmod 搭配數字修改權限
```
r = read = 2^2 = 4
w = write = 2^1 = 2
x = execute = 2^0 = 1
r--r--r-- = 444
rw------- = 600
rw-r--r-- = 644
rw-rw-rw- = 666
rwx------ = 700
rwxr--r-- = 744
rwxr-xr-x = 755
rwxrwxrwx = 777
```
- 使用 chmod 搭配符號修改權限
```
chmod    u(user)    +(加入)    r    檔案或目錄
         g(group)   -(減去)    w
         o(other)   =(設定)    x
         a(all)

e.g. chmod u=rwx,g=rw,o=r filename
    => chmod 761 filename
```

## 其他屬性修改
- 檔案時間
```
touch -t 05051200 filename
```
- 檔名
```
mv filename newfilename
```

## 觀察程序的工具指令
- 僅觀察 bash 自己相關的程序
```
ps -l

F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000  1685  1684  0  80   0 - 29011 wait   pts/0    00:00:00 bash
0 R  1000  4958  1685  0  80   0 - 34343 -      pts/0    00:00:00 ps
```
- F (flag)：代表程序的總結旗標，常見為 4 代表 root
- S (stat)：狀態列，主要的分類項目有：
    * R (Running)：該程式正在運作中；
    * S (Sleep)：該程式目前正在睡眠狀態(idle)，但可以被喚醒(signal)。
    * D ：不可被喚醒的睡眠狀態，通常這支程式可能在等待 I/O 的情況(ex>列印)
    * T ：停止狀態(stop)，可能是在工作控制(背景暫停)或除錯 (traced) 狀態；
    * Z (Zombie)：僵屍狀態，程序已經終止但卻無法被移除至記憶體外。
- UID/PID/PPID：代表『此程序被該 UID 所擁有/程序的 PID 號碼/此程序的父程序 PID 號碼』
- C：代表 CPU 使用率，單位為百分比；
- PRI/NI：Priority/Nice 的縮寫，代表此程序被 CPU 所執行的優先順序，數值越小代表該程序越快被 CPU 執行。
- ADDR/SZ/WCHAN：都與記憶體有關，ADDR 是 kernel function，指出該程序在記憶體的哪個部分，如果是個 running 的程序，一般就會顯示『 - 』 / SZ 代表此程序用掉多少記憶體 / WCHAN 表示目前程序是否運作中，同樣的， 若為 - 表示正在運作中。
- TTY：登入者的終端機位置，若為遠端登入則使用動態終端介面 (pts/n)；
- TIME：使用掉的 CPU 時間，注意，是此程序實際花費 CPU 運作的時間，而不是系統時間；
- CMD：就是 command 的縮寫，造成此程序的觸發程式之指令為何。

## 觀察全系統程序
```
pstree -A
ps aux
top: 觀察程序的動態資訊，每5秒更新一次。
```

## SBIT 的功能與觀察
- 當使用者對於此目錄具有 w,x 權限，亦即具有寫入權限時;
- 當使用者在該目錄下建立檔案或目錄時，僅有自己與 root 才有權力刪除該檔案。

## SUID/SGID/SBIT 權限的設定
```
SUID: chmod u+s filename
SGID: chmod g+s filename
SBIT: chmod o+t filename
=============================================
若使用數字:
SUID = 4
SGID = 2
SBIT = 7

e.g. chmod 1777 filename => drwxrwxtwt
```

## 認識 Linux 檔案系統
1. EXT2 (新版為 EXT4)
2. XFS 適合大容量磁碟，格式化非常快。
- 兩種都必須符合 inode 與 block 等檔案系統使用的特性。

## 系統與使用者的 shell
- 所有合法的 shell 都在 /etc/shells 這個檔案內，內容如下:
    * /bin/sh (已經被 /bin/bash 所取代)
    * /bin/bash (Linux 預設的 shell)
    * /bin/tcsh (整合 C Shell，提供更多功能)
    * /bin/csh (已經被 /bin/tcsh 所取代)

- 列出目前執行檔(確認目前的shell)
```
echo $0
```

- 寫入在 /etc/shells 內有個名為 /sbin/nologin 的 shell，就是給系統帳號預設使用的不可互動的合法 shell 。
```
1. 許多系統預設要執行的軟體，例如 mail 的郵件分析、WWW 的網頁回應等等，系統不希望該軟體使用 root 的權限，因為擔心網路軟體會被惡意人士所攻擊。 因此系統就會依據該軟體的特性給予『系統帳號』，這些系統帳號就是有特殊的任務(執行某軟體)而產生的，並不是要讓一般用戶透過該帳號來登入系統互動。 因此系統帳號通常就是使用 /sbin/nologin 作為預設 shell

2. 某些伺服器的帳號，例如郵件伺服器、FTP 伺服器等，這些伺服器的帳號本收就在收發 email 或者是傳輸檔案而已，這些帳號無須登入系統來取得互動 shell， 因此這些帳號就不需要可互動的 shell，此時就能給 /sbin/nologin。
```

## 變數設定規則
1. 變數與變數內容以一個等號『=』來連結
2. 等號兩邊不能直接接空白字元
3. 變數名稱只能是英文字母與數字，但是開頭字元不能是數字
4. 變數內容若有空白字元可使用雙引號『"』或單引號『'』將變數內容結合起來
5. 承上，雙引號內的特殊字元如 $ 等，可以保有原本的特性
6. 承上，單引號內的特殊字元則僅為一般字元 (純文字)
7. 可用跳脫字元『 \ 』將特殊符號(如 [Enter], $, \, 空白字元, ' 等)變成一般字元
8. 在一串指令的執行中，還需要藉由其他額外的指令所提供的資訊時，可以使用反單引號『`指令`』或 『$(指令)』。
9. 若該變數為擴增變數內容時，則可用 "$變數名稱" 或 ${變數} 累加內容
10. 若該變數需要在其他子程序執行，則需要以 export 來使變數變成環境變數
11. 通常大寫字元為系統預設變數，自行設定變數可以使用小寫字元，方便判斷 (純粹依照使用者興趣與嗜好)
12. 取消變數的方法為使用 unset ：『unset 變數名稱』

## 區域/全域變數
- 區域變數: 變數只能在目前這個 shell 當中存在，不會被子程序所沿用。
- 全域變數: 變數會儲存在一個共用的記憶體空間，可以讓子程序繼承使用。

## 使用 kill 管理程序
```
vim &
jobs -l
kill 836       # 不能生效
kill -9 836    # 可以生效

kill -9 PID
```





