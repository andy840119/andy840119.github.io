---
parent: asp.net mvc
---

# Asp.net 專案產生 CopyRefAssembliesToPublishDirectory 錯誤

## 前言

最近被認識的人問到

給我原本薪水的1.5倍

然後過去他們那邊工作

.

如果只是跳槽的話

不才當然馬上去問老闆明天能不能不要來了

.

但很不巧的

是做博弈

.

有點好奇

如果可以增加`一點五倍`的薪水，大家會改去做`博弈`(的遊戲)嗎?

.

雖然站在不才的角度來想

不才只是提供你遊戲玩

恩，娛樂的一種

雖然不太好

.

嘛，就跟垃圾食物一樣嘛

雖然傷身體但大家都愛吃

.

但單把進來玩的人當羊宰

說真的有實在有點過意不去

而且會把手弄髒

.

後來和朋友聊到後

他提到了課金手游....

.

:thinking: :thinking: :thinking:

.

究竟`賭博`

和`課金手游`

到底有什麼不同呢

為啥不才會排斥博弈

卻不會排斥做手游

.

## 正文


![img](https://1.bp.blogspot.com/-ePcASqSiFAQ/XS20_l3LpTI/AAAAAAAAZng/h-u4UdUtisA7y5GpMjEWF0uYFlLT4WvugCLcBGAs/s1600/1.png)


回到正題

在使用`Azure`的時候有時候會因為懶惰

直接把檔案按下右鍵`Publish`到`Azure`上

.

在某些程度上不才用得挺開心的

尤其在修改`.cshtml`時

真是方便(菸

.

但很不巧地今天踢到了鐵板

出現了這個畫面

.

![img](https://1.bp.blogspot.com/-FoFUcemmLYc/XS20SXTV0lI/AAAAAAAAZnU/jamCURDpASwiuZ4Y9q94eCfPpy991xc5wCLcBGAs/s1600/2.png)

恩，上面簡單是說

把`CopyRefAssembliesToPublishDirectory`打開

.

但不才試過根本沒用RRR

為啥只是在cshtml加上css然後按下publish就會變成這樣

.

後來發現

在有些專案中

例如這次不才改的[grandnode](https://grandnode.com/)

.

有些`.cshtml`是直接以`檔案`形式丟到azure上面的server

例如`前台`的`佈景主題`

.

另外有些則是包在`dll`內

例如`Area`裡面後台管理上的`.cshtml`

.

......

.

![img](https://1.bp.blogspot.com/-DyhUMYEXmyA/XS2z_kekhzI/AAAAAAAAZnM/lplzxpxfluMeWduh31cRyes-fVCKAit2ACLcBGAs/s1600/3.png)

解決方式就是進到server內

把剛才丟上去的檔案(`.cshtml`)砍掉

就正常了

.

## 結論 

白生氣了

.

