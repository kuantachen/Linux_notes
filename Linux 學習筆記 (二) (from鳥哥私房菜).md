# Linux 學習筆記 (二) (from鳥哥私房菜)

## 指令回傳值
- 請將『變數』、『指令』、『數學計算式』、『純文字』、『保持 $ 功能』等寫入下方的空格中
```
var=${變數}
var=$(指令)
var=$((數學計算式))
var="保持 $ 功能"
var='純文字'
var=`指令`
```

## 連續指令下達
- 直接透過分號(;)隔開每個指令即可
```
列出目前日期,目前系統資訊,核心資訊
date; uptime; uname -r
```

## 具有指令相依性的 && 與 ||
```
command1 && command2
當 command1 執行回傳為0時(成功)，command2 才會執行，否則不執行。

command1 || command2
當 command1 執行回傳為非0時(失敗)，command2 才會執行，否則不執行。
```
- 假設需要一個指令來說明某個檔名是否存在:
```
ls -d /etc && echo exist || echo non-exist
# 會輸出結果

ls -d /etc &> /dev/null && echo exist || echo non-exist
# 不輸出結果，只顯示是否exist
```
- 寫成一個 checkfile filename 的腳本
```
vim /usr/local/bin/checkfile

    #!/bin/bash
    ls -d ${1} &> /dev/null && echo exist || echo non-exist
```
```
chmod a+x /usr/local/bin/checkfile
```

## 使用 test 及判斷式確認回傳值

## 指令執行資料流動
1. 標準輸入(stdin): 代碼為 0 ```< 或 <<```
2. 標準輸出(stdout): 代碼為 1 ```> 或 >>```
3. 標準錯誤輸出(stderr): 代碼為 2 ```2> 或 2>>```

- 1> ：以覆蓋的方法將『正確的資料』輸出到指定的檔案或裝置上；
- 1>>：以累加的方法將『正確的資料』輸出到指定的檔案或裝置上；
- 2> ：以覆蓋的方法將『錯誤的資料』輸出到指定的檔案或裝置上；
- 2>>：以累加的方法將『錯誤的資料』輸出到指定的檔案或裝置上；

## standard input
```
1. 直接輸入 cat 指令可以打字
2. 使用 ctrl+d 離開 cat 結束輸入
3. 將字串直接轉存為 mycat.text

cat >> mycat.txt
123456

cat mycat.txt
123456
```

## 常見的管線命令:
- cut ：裁切資料，包括透過固定符號或者是固定字元位置
- grep ：擷取特殊關鍵字的功能
- awk '{print $N}' ：以空白為間隔，印出第 N 個欄位的項目
- sort ：進行資料排序
- wc ：計算資料的行數、字數、字元數
- uniq ：資料依行為單位，進行重複資料的計算
- tee ：將資料轉存一份到檔案中
- split ：將資料依據行數或容量數切割成數份

## grep 指令應用
- ``` -n ``` = 顯示行號
- ``` -v ``` = invert, 反向搜尋關鍵字
- 例: 觀察開機流程產生的問題，使用 ```dmesg``` 。只是這個指令輸出的資訊量非常龐大。若用戶僅須知道 eth0 這張網路卡的相關資訊，就可以使用如下的方式查詢：
```
dmesg | grep -n -i eth0
```
- 承上，如果我們還需要知道該行之前的 4 行以及之後的 3 行，以了解這行前後文的話，則可以這樣處理：
```
dmesg | grep -n -A 3 -B 4 -i eth0
```

## sed 工具的使用
```
sed 's/舊字串/新字串/g' 檔案內容
```
- sed 可以擷取出特定的關鍵行數，例:想要取出10-15行的/etc/passwd的內容
```
cat -n /etc/passwd | sed -n '10,15p'
```

## 基礎 shell script 撰寫與執行
```
1. 請先宣告使用的 script 為 bash ；
2. 請先說明程式的功能 (需要用 # 註解)
3. 請先說明程式的撰寫者 (需要用 # 註解)
4. 秀出『 This script will show your account messages 』
5. 秀出『 The 'id' command output is: 』
6. 秀出 id 這個指令的結果
7. 秀出『 your user's home is: ${HOME} 』的結果
8. 秀出『 your history record: ${HISTSIZE} 』的結果
9. 秀出『 your command aliases: 』
10. 顯示出 alias 的結果
11. 顯示出家目錄的檔名結果
```
```
mkdir bin
cd bin
vim myid.sh

!#/bin/bash
# This script will show your account messages
echo "The 'id' command output is: "
id
echo "your user's home is: ${HOME}"
echo "your history record: ${HISTSIZE}"
echo "your command aliases: "
alias
echo " your home directory is: "
ll ~
```
- 直接以 bash 或 sh 去執行腳本
```
sh myid.sh
```
- 使用 source 或 . 來執行腳本


## 以對談式腳本及外帶參數計算 pi
- 以 read 做為對談式腳本的設計依據
- 以 ${1} 等做為外帶參數的設計依據


## Linux 帳號之 UID 與 GID
```
id範圍            該ID使用者特性

0                root
1~200            由distributions自行建立的系統帳號
201~999          若使用者有系統帳號需求會使用這邊的UID
1000~            給一般使用者使用
```
- Linux 的帳號資料記錄在 /etc/passwd 當中，這個檔案的內容中，以冒號 (:) 為區隔，共有七個欄位，每個欄位的意義為：
```
1. 帳號名稱：就是登入的帳號名稱
2. 密碼：已經挪到 /etc/shadow 中，因此在此都是 x
3. UID：如上說明的 UID 資訊
4. GID：為使用者『初始群組』的 ID
5. 使用者資訊說明欄
6. 家目錄所在處
7. 預設登入時所取得的 shell 名稱
```
- /etc/shadow 以冒號 (:) 分隔成 9 欄位，各欄位功能為：
```
1. 帳號名稱
2. 密碼：目前大多使用 SHA 的加密格式來取代舊的 md5，因為密碼的長度較長，比較不容易被破解。
3. 最近更動密碼的日期：主要以 1970/01/01 累積來的總日數
4. 密碼不可被更動的天數：修改好密碼後，幾天內不能變更之意，0 代表沒限制
5. 密碼需要重新變更的天數：修改好密碼後，幾天內一定要變更的意思，0 代表沒限制
6. 密碼需要變更期限前的警告天數：第 3 與第 5 欄位相加後的幾天內，當使用者登入系統時，會警告該修改密碼了
7. 密碼過期後的帳號寬限時間(密碼失效日)：密碼有效日期為『更新日期(第3欄位)』+『重新變更日期(第5欄位)』， 過了該期限後使用者依舊沒有更新密碼，那該密碼就算過期了。(參考底下例題的說明)
8. 帳號失效日期：亦使用 1970/01/01 累加的總日數，這個欄位表示，此帳號在此欄位規定的日期之後，將無法再使用。
9. 系統保留尚未使用
```
- 查詢目前系統的加密機制
```
authconfig --test | grep password

cat /etc/sysconfig/authconfig | grep -i passwd
```
- 使用者的初始群組 (原生家庭) 記載在 /etc/passwd 檔案的第四個欄位，不過該 GID 對應到人類認識的群組名稱就得到 /etc/group 當中查詢。 這個檔案的內容同樣使用冒號 (:) 分隔成四個欄位，內容為：
```
1. 群組名稱
2. 群組密碼：目前很少使用
3. GID
4. 加入此群組的帳號，使用逗號 (,) 分隔每個帳號
```
- 使用者帳號資訊
```
chage -l user
```

## 預設權限 umask

## 主機細部權限規劃: ACL的使用
- ACL (Access Control List, 存取控制列表)
- 主要的目的是在提供傳統的 owner,group,others 的 read,write,execute 權限之外的細部權限設定。ACL 可以針對單一使用者，單一檔案或目錄來進行 r,w,x 的權限規範，對於需要特殊權限的使用狀況非常有幫助。
- ACL 主要可以針對幾個項目來加以控制：
    * 使用者 (user)：可以針對使用者來設定權限；
    * 群組 (group)：針對群組為對象來設定其權限；
    * 預設屬性 (mask)：還可以針對在該目錄下在建立新檔案/目錄時，規範新資料的預設權限；

## 網路設定
- 觀察網路連線
```
ip addr
```
- 查詢網路連線代號
```
nmcli connection show

NAME：即為網路連線代號，接下來我們要處理的任務均是針對此連線代號而來。
UUID：Linux 的裝置識別碼，在系統當中幾乎是獨一無二的存在。
TYPE：網路連線的類型，包括乙太網路、無線網路、橋接等等功能
DEVICE：即是網路介面卡。
```
- 觀察網路連線代號的詳細設定
```
nmcli connection show eth0

connection.autoconnect [yes|no] ：是否於開機時啟動這個連線，預設通常是 yes
ipv4.method [auto|manual] ：自動還是手動設定網路參數
ipv4.dns [dns_server_ip] ：填寫 DNS 伺服器的 IP 位址
ipv4.addresses [IP/Netmask] ：IP 與 netmask 的集合，中間用斜線 / 來隔開
ipv4.gateway [gw_ip] ： gateway 的 IP 位址！

IP4.ADDRESS[1]: 目前運作中的 IPv4 的 IP 位址參數
IP4.GATEWAY: 目前運作中的 IPv4 的通訊閘
IP4.DNS[1]: 目前運作中的 DNS 伺服器 IP 位址
DHCP4.OPTION[XX]: 由 DHCP 伺服器所提供的相關參數
```

## 主機名稱設定
- 主機名稱的觀察方式可以使用 ``` hostname ``` 來觀察
- 若要更改設定，則需要編寫 /etc/hostname，但通常改完都需要 reboot
- 為了解決 reboot 的問題 centOS 提供 ``` hostnamectl ``` 的指令來管理
```
hostnamectl set-hostname www.centos

hostnamectl
```

## 日期與時間設定
```
timedatectl
```
- 也可以透過網路來校正時間。例: 崑山科大提供了 ntp.ksu.edu.tw 這個 NTP 伺服器，因此網路較時可以簡單地進行。
```
ntpdate ntp.ksu.edu.tw
```

## 語系設定
```
localectl

# 若想要讓圖形介面的畫面以台灣中文為主，可以使用下麵方式來處置:
localectl set-locale LANG=zh_TW.utf8
systemctl isolate multi-user.target
systemctl isolate graphical.target
```

## 簡易防火牆管理
```
firewall-cmd --get-default-zone
```

## Linux 檔案的打包與壓縮
- 在 Linux 環境下，常見的壓縮指令有：gzip, bzip2, xz 三種，這三個指令主要的目的為壓縮單一檔案而已。但預設的情況下， 被壓縮的檔案會遺失而僅存在被壓縮完成之後的壓縮檔，除非使用 -c 的選項搭配資料流重導向，方可原始檔案與壓縮檔案同時存在。
### tar
```
tar [-z|j|J] -c|-t|-x [-v] [-f tar 支援的檔名] [filename...]

[-z|j|J] ：是否需要壓縮支援，三個選項分別是 gzip, bzip2, xz 的支援
-c|-t|-x ：實際進行的任務，三個選項分別是打包、查閱資料、解打包
-v ：是否要查閱指令執行過程
[-f tar 支援的檔名] ：使用 -f 來處理 tar 的檔案
```
- *.tar：單純的 tar 並沒有壓縮
- *.tar.gz：支援 gzip 壓縮的 tar 檔案
- *.tar.bz2：支援 bzip2 壓縮的 tar 檔案
- *.tar.xz：支援 xz 壓縮的 tar 檔案
```
打包: tar -Jcv -f etc.tar.xz /etc
查看: tar -Jtv -f etc.tar.xz
```

## Linux 工作排程
- Linux 系統的工作非常的多，管理員總是希望系統可以自行管理自己，這樣維護系統會比較輕鬆。而自動排程的方式有兩種，分別是：
    * 單一執行一次，執行完畢後該工作則被捨棄；
    * 一直循環不停的工作
- 在預設的情況下，Linux 系統提供的上述兩種工作排程，最小的時間解析度為分鐘，最大的時間解析度為一年內。

## 單次工作排程: at
- 單次排程工作必須要啟動 atd 這個『服務』才能夠運作，因此讀者應該先查看系統的 atd 是否有啟動。
```
systemctl status atd
```
- 單次循環工作可使用『 at TIME 』來處理，那個 TIME 為時間格式。最常見的時間格式為：
```
at HH:MM YYYY-MM-DD
at now
at now + 10 minutes
```
```
at 11:00
at> ip addr show &> /home/student/myipshow.txt
at> <EOT>   <== 這裡按下 [ctrl]+d 結束輸入
job 1 at Thu May 12 11:00:00 2016
```
- 查閱 at 的工作列表狀態
```
atq
at -c 1 (看第一個工作排程的內容)
```
## 循環工作排程: crontab
- 循環型的工作排程需要啟動 crond 這個服務才行，請先確認這個服務的狀態。
```
systemctl status crond
```

## Linux 本機軟體管理
```
|  Distribution 代表 | 軟體管理機制 |    使用指令    | 線上升級指令 
|  RedHat / fedora  |  RPM        | rpm, rpmbuild | yum     
|  Debian / Ubuntu  |  DPKG       |     dpkg      | apt-get
```

## RPM 軟體管理程式
- RPM 查詢 (Query)
```
rpm -qa                               <==已安裝軟體
rpm -q[licdR] 已安裝的軟體名稱           <==已安裝軟體
rpm -qf 存在於系統上面的某個檔名          <==已安裝軟體
rpm -qp[licdR] 未安裝的某個檔案名稱      <==查閱RPM檔案
```

## 利用 yum 進行查詢、安裝、升級與移除功能
- 查詢功能: yum [list|info|search|provides|whatprovides] 參數
- 安裝/升級功能: yum [install|update] 軟體

## CentOS 7 登錄檔簡易說明
- 各 Linux distribution 所使用的登錄檔記錄位置大多位於 /var/log，但檔名則不見得相同。CentOS 7 常見的檔名與相關內容對應如下：

    * /var/log/cron：記錄由 crond 這個服務所產生的各項資訊，包括使用者 crontab 的結果。
    * /var/log/dmesg：記錄系統在開機的時候核心偵測過程所產生的各項資訊。
    * /var/log/lastlog：記錄系統上面所有的帳號最近一次登入系統時的相關資訊。
    * /var/log/maillog：記錄郵件往來的資訊，包括由 postfix, devecot 服務所產生的系統資訊
    * /var/log/messages：幾乎系統發生的錯誤訊息 (或者是重要的資訊) 都會記錄在這個檔案中； 如果系統發生莫名的錯誤時，這個檔案是一定要查閱的登錄檔之一。
    * /var/log/secure：只要牽涉到『需要輸入帳號密碼』的軟體，那麼當登入時 (不管登入正確或錯誤) 都會被記錄在此檔案中。

## 登錄檔所需相關服務 (daemon) 與程式
- systemd-journald.service：最主要的訊息收受者，由 systemd 提供的；
- rsyslog.service：主要登錄系統與網路等服務的訊息；
- logrotate：主要在進行登錄檔的輪替功能。

## rsyslog 的設定與運作
- 這個服務的設定檔在 /etc/rsyslog.conf ， 設定內容主要是針對『(1)什麼服務 (2)的什麼等級訊息 (3)需要被記錄在哪裡(裝置或檔案)』

## 程序管理透過 kill 與 signal
- 主要的程序訊號可以使用 ```kill -l``` 或 ```man 7 signal``` 查詢。
- 管理員也能夠使用指令名稱來給予 signal ，直接透過 ``` killall ``` 即可。
```
1	SIGHUP	啟動被終止的程序，可讓該 PID 重新讀取自己的設定檔，類似重新啟動
2	SIGINT	相當於用鍵盤輸入 [ctrl]-c 來中斷一個程序的進行
9	SIGKILL	代表強制中斷一個程序的進行，如果該程序進行到一半， 那麼尚未完成的部分可能會有『半產品』產生，類似 vim會有 .filename.swp 保留下來。
15	SIGTERM	以正常的結束程序來終止該程序。由於是正常的終止， 所以後續的動作會將他完成。不過，如果該程序已經發生問題，就是無法使用正常的方法終止時， 輸入這個 signal 也是沒有用的。
19	SIGSTOP	相當於用鍵盤輸入 [ctrl]-z 來暫停一個程序的進行
```

## systemctl 管理服務的啟動與關閉
```
systemctl [command] [unit]
```
```
start ：立刻啟動後面接的 unit
stop ：立刻關閉後面接的 unit
restart ：立刻關閉後啟動後面接的 unit，亦即執行 stop 再 start 的意思
reload ：不關閉後面接的 unit 的情況下，重新載入設定檔，讓設定生效
enable ：設定下次開機時，後面接的 unit 會被啟動
disable ：設定下次開機時，後面接的 unit 不會被啟動
status ：目前後面接的這個 unit 的狀態，會列出有沒有正在執行、開機預設執行否、登錄等資訊等！
```

## 核心與核心模組
- 目前系統上以載入的模組 ```lsmod ``` ```lsmod | grep xfs```
- 了解模組功能 ```modinfo xfs```


## grub2 設定檔初探
- 核心的載入與設定是由開機管理程式來處理的，而 centOS 7 預設的開機管理程式為 grub2 這一個軟體。該軟體的優點包括有:
    * 認識與支援較多的檔案系統，並且可以使用 grub2 的主程式直接在檔案系統中搜尋核心檔名
    * 開機的時候，可以 自行編輯與修改開機設定項目 ，類似 bash 的指令模式
    * 可以動態搜尋設定檔，而不需要在修改設定檔後重新安裝 grub2。亦即是我們只要修改完 /boot/grub2/grub.cfg 裏頭的設定後，下次開機就生效了！
```
(hd0,1)         # 一般的預設語法，由 grub2 自動判斷分割格式
(hd0,msdos1)    # 此磁碟的分割為傳統的 MBR 模式
(hd0,gpt1)      # 此磁碟的分割為 GPT 模式
```

## 軟體磁碟陣列 (Software RAID)
- 陣列 (RAID) 的目的主要在 『加大容量』、『磁碟容量』、『加快效能』等方面，而根據你注重的面向就得要使用不同磁碟陣列等級了。

## 什麼是 RAID
- Redundant Arrays of Independent Disks, RAID。獨立容錯式磁碟陣列。RAID 可以透過一個技術(軟體或硬體)，將多個較小的磁碟整合成為一個較大的磁碟裝置;而這個較大的磁碟功能可不只是儲存而已，他還有資料保護的功能。整個 RAID 由於選擇的等級 (level) 不同，而使得整合後的磁碟具有不同功能，基本常見的 level 有幾種:

    * RAID-0 (等量模式, stripe, 效能最佳): 兩顆以上的磁碟組成 RAID-0 時，當有 100MB 的資料要寫入，則會將該資料以固定的 chunk 拆解後， 分散寫入到兩顆磁碟，因此每顆磁碟只要負責 50MB 的容量讀寫而已。如果有 8 顆組成時，則每顆僅須寫入 12.5MB ，速度會更快。此種磁碟陣列效能最佳， 容量為所有磁碟的總和，但是不具容錯功能。
    * RAID-1 (映射模式, mirror, 完整備份): 大多為 2 的倍數所組成的磁碟陣列等級。若有兩顆磁碟組成 RAID-1 時，當有 100MB 的資料要寫入， 每顆均會寫入 100MB，兩顆寫入的資料一模一樣 (磁碟映射 mirror 功能)，因此被稱為最完整備份的磁碟陣列等級。但因為每顆磁碟均須寫入完整的資料， 因此寫入效能不會有明顯的提昇，但讀取的效能會有進步。同時容錯能力最佳，但總體容量會少一半。 
    * RAID 1+0: 此種模式至少需要 4 顆磁碟組成，先兩兩組成 RAID1，因此會有兩組 RAID1，再將兩組 RAID1 組成最後一組 RAID0，整體資料有點像底下的圖示: 因此效能會有提昇，同時具備容錯，雖然容量會少一半。




