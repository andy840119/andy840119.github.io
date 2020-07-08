---
parent: 開箱
---

# [教學]锐角云（AAC）安裝Batocera教學

## 前言

趁著剛好有點空閒

把沒寫完的文章補一下

繼[上一篇](../aac-install-freenas/README.md)把其中一台安裝FreeNas後

這次把剩下的主機安裝`Batocera`

.

## 正文

先簡單介紹一下各個名詞

`RetroArch`，`EmulationStation`，`RetroPie`之間的關係是什麼，可以看[這篇文章](https://3q.9527.tw/83)，講得挺清楚的

.

然而除了[RetroPie](https://retropie.org.uk/)外

也有很其他人做出把模擬器直接打包成作業系統，像是

- [RecalBox](https://www.recalbox.com/)
- [Lakka](http://www.lakka.tv/)
- [Batocera](https://batocera.org/)

.

大概簡單說一下這幾家的差異 :

- RetroPie : 只能安裝在樹莓派
- RecalBox : 能安裝在`x86`的機器上  
             缺點是不支援`macbook`和目前的`aac`主機，感覺`uefi`支援度不夠好
- Lakka : 沒有使用上`EmulationStation`，介面沒那麼好看
- Batocera : 和`RecalBox`差不多，但作業系統更肥大，換來的是支援度更好

.

一開始測試`RecalBox`沒辦法在這次的`AAC`主機上跑

只能換成`Batocera`

.

過程很簡單

下載，使用用[Rufus](https://rufus.ie/)把作業系統安裝到USB上，開機按`delete`修改成USB開機，就能進入到系統了

.

wifi ???

藍芽 ???

.

如果要安裝在內部的`SSD`的話

可以參考[這傢伙](https://www.youtube.com/watch?v=NyQC6ZGbSpY)的做法，雖然有點繞圈子就是了

.

## 結論

機子都用完了

QQ

.