---
parent: 開箱
---

# [教學]锐角云（AAC）安裝FreeNas教學

## 前言

最近在社團看到只要兩百多人民幣的小電腦

簡單來說，就是在虛擬幣圈錢底下的產物

用貴貴的價格買一台主機來開機掛著

就會分你虛擬幣

可以在市場交易賺錢

.

想當然，機器沒賣幾個月就跑了

詳細可以看這篇[文章](https://www.chainnews.com/articles/515926413232.htm)

.

公司倒不倒無所謂

反正有便宜的機器可以玩了

有夠心動下了兩台

最後真的拿到了

開心

.

工商一下[附上連結](https://osslab.tv/shop/acute_angle_mini_pc/)

最便宜那批已經賣完了QQ，但目前的價格以玩具來說還算OK

.

[社團內](https://www.facebook.com/groups/wtfhack)有折扣碼可以打八折，目前(2020/7/8)不確定不確定有沒有

.

拿到後當然不是當文書機

不才電腦已經很多了

打算一台拿來裝`FreeNas`跑`Container`或`CI server`

另外一台灌`Batocera`當遊戲機

.

## 正文

到[FreeNas](https://www.freenas.org/download/)官網下載，用[Rufus](https://rufus.ie/)把作業系統安裝到USB上

開機時按`delete`進到`bios`

修改一下開機順序

應該可以順利`freeNas`安裝前選單畫面

.

但如果用預設選項卡在`Controller timeout`的錯誤

.

解決方法 :

可以參考[這篇文章](https://forums.freebsd.org/threads/sdhci_pci0_slot0-controller-timeout.59077/)解決

1. 看到畫面時輸入`3`進入`options`在輸入`2`
2. 接下來應該可以在下面輸入指令
3. 輸入
    > set hint.sdhci_pci.0.disabled="1"
4. 可以輸入`Show`指令看看有沒有被加在開機設定內，還有有沒有打錯字
5. 輸入`boot`用目前設定好的參數來開機

應該就可以順利跑到安裝畫面

.

安裝方式就跟其他網路教學差不多

但目前有遇到兩個問題 :

1. 如果全選磁區好像會有無法擦除的錯誤，不才最後懶得管了
2. 格式化磁區的時間爆幹久(體感時間10分鐘以上)，會以為程序是不是掛了

.

接著就會出現安裝的進度點點，安裝好後會重新開機，記得把USB拔掉

.

開機進入安裝好的系統也要做跟剛剛一樣的動作

第一次會跑比較久

最後螢幕就會出現local可以進去的IP

.

用溜覽器輸入IP後就可以開始玩了

.

然後記得順便用`SSH`進去`NAS`把`/boot/device.hints`裡面的開機設定[修改一下](https://forums.freebsd.org/threads/sdhci_pci0_slot0-controller-timeout.59077/)

`FreeNas`如何使用`SSH`可以參考[這篇影片](https://www.youtube.com/watch?v=UDiFAuNVrTU)，但記得先看完再修改，因為影片不會一次把設定修改過程全說完

.

然後`SSH` Client 建議使用[putty](https://www.putty.org/)，用`瀏覽器內建的`和`windows內建的`沒辦法順利使用`vi`編輯

`SSH`簡單來說可以讓你用指令的方式遠端操作`FreeNas`內的`FreeBSD`系統

至於在linux下如何切換目錄，和編輯檔案(`vi`)，就自己爬文吧

以上

.

修改完後記得把`SSH`關掉，不然其他人可以用這漏洞連上你的`NAS`幹些你不想發生的事情

然後重開機後開機引導就不需要重新設定了

.

開機時間有點長，兩~三分鐘

進入`NAS`後閒置`CPU`使用率大概在1%左右

`記憶體`系統吃掉`1.5G`

還有`6G`可以用

選單操作速度都還算很快

之後再拿來看要不要拿來跑`CI server`還是 `minecraft server`

.

以上

.

# 結論

一台拿去當`NAS`了

還剩一台

.

然後最近公司要搬家了

希望新的辦公室的座位能有網路孔

.