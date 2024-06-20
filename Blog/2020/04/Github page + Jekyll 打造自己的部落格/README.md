# Github page + Jekyll 打造自己的部落格

## 前言

到新公司也差不多一個月了

前面都在啃`Python` 和 `Angular(JS)`

啃到都快起肖了

.

最近終於把最困難的作業寫完

可以開始~~偷懶~~繼續寫文章了

.

然後自從去年開始改使用`Markdown`打文章，發現比那智障`Html`好用一百倍後

就想著用`Github page`架設部落格

.

之後打算`Github page`專門打文件，教學相關的文章

[舊的部落格](http://blog.udn.com/andy840119)打開箱文之類的

沒辦法，舊的部落格SEO還是做得不錯

一時之間不好整個砍掉QAQ

.

## Github page架設目標

最終的目標是，會分成兩個專案(repo)

[主題專案](https://github.com/andy840119/jekyll-tameowder-theme)裡面只有在Jekyll產生網頁時會用到的資源，例如網頁上方的Header圖片，背景圖片等等

[文章專案](https://github.com/andy840119/andy840119.github.io)裡面只有`md`檔和`圖片檔`，不會有自動產生的`html`，感覺像是網站的資料庫

.

在文章專案那邊，如果有新增文章到`master`分支

就會自動觸發`github`裡面的`自動部署(CD)`，抓取`_config.yml`裡面指定的`主題`

然後把專案內所有`md`檔和`圖片檔`編譯成`html`靜態網頁

最後就可以在網址上看到

例如不才的[github repo](https://github.com/andy840119/andy840119.github.io)

最後就會呈現成[這種效果](https://andy840119.github.io/)

.

當然主題用公版很輕鬆，啥都不用改

只是身為一個廢文寫手還是想要在`github page`上擁有自己客製化過的主題

所以這篇會簡單介紹幾個部分: 

- 如何使用自己的主題
- 如何在本地把文章內容跑起來
- 如何在本地把主題內容跑起來
- 快速開發的小技巧

.

如果可以的話，最終目標還是想要
- 根據`readme.md`目錄自動產生`Navigation`
- Google Analyze(用來偷偷查看到底有多少人造訪)
- 文章下方留言板
- SEO優化(當然是自動的，或是可以手動指定SEO關鍵字)
- 每一篇文章或是總文章的`Counter`
- 切出左邊的`Navigation`方便坎入`Discord`或是`Facebook`的Frame進去
- 反正舊的部落格的東西希望這邊大部分都要有就是了

## 如何跑自己的主題

首先先在`Github`下建立一個專案(Repo)

名稱取`<你的github user name>.github.io`

例如`andy840119.github.io`

.

建立一個`readme.md`(或是已經有了就不用建立)

然後到設定內，把`Github page`選項打開

然後選一個主題，應該就會發現專案內多了`_config.yml`

.

把`_config.yml`打開

把裡面的`theme: jekyll-theme-slate`

改成`remote_theme: andy840119/jekyll-tameowder-theme`

儲存，合併到`master`

等一小段時間

到`https://<你的github user name>.github.io`看

就會發現主題變了，\變成不才的主題/

.

`remote_theme`命名規則是:

`remote_theme: <github user name>/<repo name>`

可以把[不才的](https://github.com/andy840119/jekyll-tameowder-theme)，或是[其他人的](https://github.com/pages-themes/slate)專案

複製到自己的`github`帳號下

就可以開始修改自己的主題了喔

.

## 如何在本地把文章內容跑起來

簡單來說

可以看看打出來的`markdown`

會實際被編譯成怎麼樣的`Html`出來

.

雖然說用`Visual studio code`有`markdown previewer`可以看

但實際上的Html還是有可能會因為一些標籤跑版

.

建議使用`Mac` + `Visual studio code`

`Windows 在安裝 Jekyll Server 可能很難搞`

.

先在專案內加上`.gitignore`，然後新增以下內容

```bash
# 在local跑起來後會產生這個資料夾，不要讓裡面的資料上版控
_site/

# 在跑Gemfile時會產生這檔案，也不要上版控
Gemfile.lock
```

之後照著[Github上面這篇文章](https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll)跑


如果沒問題在`http://localhost:4000/`應該就可以看到跟在`github page`一模一樣的東西了

測試過在Github上的`Remote theme`也會自動抓下來

.

## 如何在本地把主題內容跑起來

如果要開發自己的主題，在`Local`跑起來一台機器總是比上傳後看效果快

.

如果剛剛有把`jekyll` server跑起來

要記得關掉，不然port會衝突

.

跑起來也很簡單

切換到`主題專案`的目錄

執行`bundle exec jekyll serve`應該就OK了

.

如果想要用`Chrome debug`

可以參考[這篇文章](https://emous.github.io/2019/04/05/UseVisualStudioCodeToWriteWithJekyll/)建立`.vscode/launch.json`

不過還是要`bundle exec jekyll serve`打指令把Server跑起來後，才能用Chrome偵錯

.

