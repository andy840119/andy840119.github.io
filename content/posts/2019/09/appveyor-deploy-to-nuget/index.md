---
title: '[教學] 把github 的專案自動部署到nuget上'
date: 2019-09-01
tags: [CI, 教學]
---

## 前言 : 

最近在寫專案

想說趁機試一下如何把自動化佈署

把`.netStandard`的專案部屬到`nuget`上

.

結果找了好久發現大家教學寫得都不一樣

三洨

.

找了找最後覺得[這篇](https://dev.to/joaofbantunes/creating-a-cicd-pipeline-for-a-net-library-part-1---intro-2j61?fbclid=IwAR2P8wr3x75airwMQLkpyFC73DZFDXF1l1JZna_XfvG_7L2mdXa45kQo674)寫得最好

就整理一下心得好了

.

## 佈署方式 :

簡單來說，CI(持續整合) CD(自動佈署)有好幾種方法

最基本的兩種

1. CI/CD 供應商(例如`Appveyor`)會提供自己的設定檔和格式，通常是`appveyor.yml `   
   可以在裡面設定一些參數   
   優點是快速上手，通常自動測試設定很快就完成了   
   但缺點是不同的自動化平台的設定方式會有些差別

2. 叫CI跑自己設定好的腳本    
   雖然說多了幾個不明所以的檔案會有點礙眼   
   但如果哪天要從`appveyor`換到`Tr@vis CI`，只要讓`travis.yml`跑自己寫好的腳本就好了   
   .   
   還有本地也可以執行腳本，不需要等到push後才知道跑出來的結果

.

內建的參數有點玩膩了(並沒有

就來玩玩看用腳本自動佈署的方式好了

.

## 執行流程

先簡單說明一下執行過程好了，以`Appveyor`為例

在跑CI/CD時

.

執行的流程大概會是 : 

1. `Appveyor`會抓取專案內的`appveyor.yml`設定

2. 然後讓`Appveyor`執行`build.ps1`   
   並且帶入參數，例如`目前執行的branch`，還有`nuget上傳的金鑰`等等

3. `build.ps1`再執行`build.cake`，會把剛剛的那些參數傳過去

4. `build.cake`會定義一些流程，然後可以根據`不同參數`執行`不同動作`   
   假如在`master`上，就會自動佈署   
   如果是`develop`或是其他Branch，就只跑測試就好
   
.

所以說，跑腳本可以做的事情，會比`appveyor.yml`上面設定參數的自由度更高

.

## 腳本(build.cake)

這邊的說明方式是直接把整份`build.cake`直接翻開來加上註解做說明

也可以從[這邊](https://github.com/osu-Karaoke/LyricMaker/blob/develop/build.cake)看

.

首先，先產生`build.ps1`檔

詳細的方法可以參考[這邊](https://dotblogs.com.tw/mileslin/2016/04/23/124859)

.

自動產生出來的`build.ps1`不用動他

.

接下來新增`build.cake`

然後複製[這邊](https://github.com/osu-Karaoke/LyricMaker/blob/develop/build.cake)的內容過去

然後在修改成自己想要的樣子

.

這邊簡單說明一下這份`build.cake`工作方式是

平常PR時跑測試

如果在`master`上做事或是有東西合併進來，就佈署一份到`nuget.org`上

.

如果大家要做的事情也差不多

就只要修改`上面的參數名稱`就好了

```csharp
// 這邊不用管他，簡單來說當作要引用nuget的感覺
#tool "nuget:?package=coveralls.io&version=1.4.2"
#addin Cake.Git
#addin nuget:?package=Nuget.Core
#addin "nuget:?package=Cake.Coveralls&version=0.9.0"

// 因為要丟到nuget上所以using這個package
using NuGet;


// 預設要執行的項目
var target = Argument("target", "Default");

// 產生出來的nuget要放這個目錄
var artifactsDir = "./artifacts/";

// 方案位置，要改成自己的方案名稱
var solutionPath = "./LyricMaker.sln";

// 要包成nuget的專案，要改成自己的方案名稱
var project = "./LyricMaker/LyricMaker.csproj";

// 測試目錄，和對應需要測試的方案名稱，也是要改
var testFolder = "./LyricMaker.Tests/";
var testProject = testFolder + "LyricMaker.Tests.csproj";

// CodeCoverage清單，但不才暫時沒需要所以有註解調
var coverageResultsFileName = "coverage.xml";

// 從丟進來的參數中取得目前Branch名稱
var currentBranch = Argument<string>("currentBranch", GitBranchCurrent("./").FriendlyName);

// 判斷是不是master
var isReleaseBuild = string.Equals(currentBranch, "master", StringComparison.OrdinalIgnoreCase);

// 預設是Release
var configuration = "Release";

// 從丟進來的參數中取得nuget金鑰和codecoverage(目前沒用到)的金鑰
var nugetApiKey = Argument<string>("nugetApiKey", null);
var coverallsToken = Argument<string>("coverallsToken", null);

// nuget.org網址 
var nugetSource = "https://api.nuget.org/v3/index.json";

// 定義任務，把空間清乾淨
Task("Clean")
    .Does(() => {
        if (DirectoryExists(artifactsDir))
        {
            DeleteDirectory(
                artifactsDir, 
                new DeleteDirectorySettings {
                    Recursive = true,
                    Force = true
                }
            );
        }
        CreateDirectory(artifactsDir);
        DotNetCoreClean(solutionPath);
    });

// 定義任務，載入nuget
Task("Restore")
    .Does(() => {
        DotNetCoreRestore(solutionPath);
    });

// 定義任務，建置
Task("Build")
    .IsDependentOn("Clean")
    .IsDependentOn("Restore")
    .Does(() => {
        DotNetCoreBuild(
            solutionPath,
            new DotNetCoreBuildSettings 
            {
                Configuration = configuration
            }
        );
    });

// 定義任務，跑測試
Task("Test")
    .Does(() => {
        var settings = new DotNetCoreTestSettings
        {
            ArgumentCustomization = args => args.Append("/p:CollectCoverage=true")
                                                .Append("/p:CoverletOutputFormat=opencover")
                                                //.Append("/p:CoverletOutput=./" + coverageResultsFileName)
        };
        DotNetCoreTest(testProject, settings);
        //MoveFile(testFolder + coverageResultsFileName, artifactsDir + coverageResultsFileName);
    });

// 定義任務，上傳跑任務時產生的Code coverage結果
Task("UploadCoverage")
    .IsDependentOn("Test")
    .Does(() =>
    {
        CoverallsIo(artifactsDir + coverageResultsFileName, new CoverallsIoSettings()
        {
            RepoToken = coverallsToken
        });
    });

// 定義任務，包成nuget
Task("Package")
    .Does(() => {
        var settings = new DotNetCorePackSettings
        {
            OutputDirectory = artifactsDir,
            NoBuild = true
        };
        DotNetCorePack(project, settings);
    });

// 定義任務，把nuget上傳
Task("Publish")
    .IsDependentOn("Package")
    .Does(() => {
        var pushSettings = new DotNetCoreNuGetPushSettings 
        {
            Source = nugetSource,
            ApiKey = nugetApiKey
        };

        var pkgs = GetFiles(artifactsDir + "*.nupkg");
        foreach(var pkg in pkgs) 
        {
            if(!IsNuGetPublished(pkg)) 
            {
                Information($"Publishing \"{pkg}\".");
                DotNetCoreNuGetPush(pkg.FullPath, pushSettings);
            }
            else {
                // 如果看到這個訊息，代表專案的nuget版本號在nuget.org上已經有了
                Information($"Bypassing publishing \"{pkg}\" as it is already published.");
            }
            
        }
    });

// Check nuget package is published
private bool IsNuGetPublished(FilePath packagePath) {
    var package = new ZipPackage(packagePath.FullPath);

    var latestPublishedVersions = NuGetList(
        package.Id,
        new NuGetListSettings 
        {
            Prerelease = true
        }
    );

    return latestPublishedVersions.Any(p => package.Version.Equals(new SemanticVersion(p.Version)));
}

// 定義任務，這個任務要完成編譯和測試
Task("BuildAndTest")
    .IsDependentOn("Build")
    .IsDependentOn("Test");

// 定義任務，這個任務要完成編譯，測試和上傳結果
Task("CompleteWithoutPublish")
    .IsDependentOn("Build")
    .IsDependentOn("Test");
    //.IsDependentOn("UploadCoverage");


if(isReleaseBuild)
{
    // 定義完成任務
    // 玩成任務就是執行編譯，測試和佈署到nuget
    Information("Release build");
    Task("Complete")
        .IsDependentOn("Build")
        .IsDependentOn("Test")
        //.IsDependentOn("UploadCoverage")
        .IsDependentOn("Publish");
}
else
{
    // 定義完成任務
    // 完成任務就只需要跑編譯和測試
    Information("Development build");
    Task("Complete")
        .IsDependentOn("Build")
        .IsDependentOn("Test");
        //.IsDependentOn("UploadCoverage");
}

// 定義預設任務是 完成任務
Task("Default")
    .IsDependentOn("Complete");

// 執行預設任務
RunTarget(target);
```

## 腳本(appveyor.yml)

接下來新增`build.cake`

然後複製[這邊](https://github.com/osu-Karaoke/LyricMaker/blob/develop/build.cake)的內容過去

也是在修改成自己想要的樣子

.

也是說明一下專案格式

這樣比較好懂

```yml
# 建置版本
version: "{build}"

# 使用的環境
image: Visual Studio 2017

# 定義只有在那些Branch上Appveyor才做事
# 因為不才希望丟Pull Request也要跑CI，所以就註解掉了
#branches:
#  only:
#    - master
#    - develop

# 環境
environment:
  DOTNET_CLI_TELEMETRY_OPTOUT: 1
  DOTNET_SKIP_FIRST_TIME_EXPERIENCE: 1
  NUGET_API_KEY:

    # 金藥需要加密，下面會說明
    secure: SCmDP3tOoIfO4zexfzu07xjBnv1KDMNDljU1iKZqU1VCTwyZ8pcAi5aLa5SMmwzh

# Appveyor要做的事情就是執行build.ps1，build.ps1就會執行build.cake
# 然後帶入參數
build_script:
- ps: .\build.ps1 --currentBranch=$env:APPVEYOR_REPO_BRANCH --nugetApiKey=$env:NUGET_API_KEY --coverallsToken=$env:COVERALLS_TOKEN

# 把內建的測試和佈署關掉
test: off #tests handled by cake script
deploy: off #deploy handled by cake script
```

.

在把Nuget自動上傳到`nuget.org`時，會需要一組金鑰

取得金鑰的教學在[這邊](https://docs.microsoft.com/zh-tw/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli#acquire-your-api-key)可以看到

.

直接把取得到的金鑰放在`appveyor.yml`

會讓所有取得金鑰的人

能夠直接用金鑰佈署專案，相當危險，所以需要加密

.

需要在[這邊](https://ci.appveyor.com/tools/encrypt)

把從`nuget.org`拿到的金鑰貼上後

會得到(比較長)加密後的金鑰

.

這組(比較長)加密後的金鑰

就可以任意丟在網路上沒問題了

.

## 後言

在摸了半天，知道整個運作流程後

就沒那麼難了

.

大家加油ㄅ

.
