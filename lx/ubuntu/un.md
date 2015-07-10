# [Ubuntu環境下，如何安裝nvm以及nodejs](http://samwhelp.github.io/blog/read/platform/nodejs/install/)

# Ubuntu環境下，如何安裝nvm以及nodejs

## 測試環境

撰寫本文時，測試的環境是

  * 「Xubuntu 14.04 64位元」
  * 「nvm v0.17.2」
  * 「nodejs v0.10.32」

## 安裝步驟

  * 安裝「nvm」。
  * 透過「nvm」安裝「nodejs」。

## 安裝「nvm」。

查詢「[nvm](https://www.google.com.tw/#q=nvm)」，應該第一筆就會找到「[nvm](https://github.com/creationix/nvm)」放在「github」上的「網址」。  
可以直接參考該頁的說明來安裝「nvm」。裡面有一行指令可以快速安裝。  
以下單純討論「Manual install」的方式，因為想要安裝到非預設的資料。

下面這段指令，會用到「[git](http://manpages.ubuntu.com/manpages/trusty/en/man1/git.1.html)」，所以請先確認已經安裝「[git](http://packages.ubuntu.com/trusty/git)」，安裝方式請參考「[這篇](http://samwhelp.github.io/blog/read/git/install/)」。
    
    1
    
    2
    
    3
    
    4
    
    5
    
    mkdir ~/App/Nvm -p
    
    cd ~/App/Nvm/
    
    git clone https://github.com/creationix/nvm.git Git
    
    cd Git
    
    git checkout `git describe --abbrev=0 --tags`

若使用「bash」的話，執行下面這一行
    
    1
    
    $ echo "source ~/App/Nvm/Git/nvm.sh" >> ~/.bashrc

若使用「zsh」的話，執行下面這一行
    
    1
    
    $ echo "source ~/App/Nvm/Git/nvm.sh" >> ~/.zshrc

**上面的動作，就已經把「nvm」安裝完成。**

當你重新登入「shell」。「shell」會執行自己的「profile」，也就是會執行到「source ~/App/Nvm/Git/nvm.sh」這一段。例如我慣用的「Terminal」是「Konsole」。這時候我只要關閉目前的「Konsole」，重新開啟就行了。或是「不重新開啟」，直接開啟新的「Konsole視窗」。

你可以執行，來確認「nvm」是否安裝成功。
    
    1
    
    $ nvm --version

若安裝成功，應該會出現下面的字樣(會依照當下安裝的版本，有所不同，以下請當參考）。
    
    1
    
    0.17.2

執行下面的指令，了解「nvm」有哪些參數可以下。
    
    1
    
    $ nvm help

## 透過「nvm」安裝「nodejs」

了解「nodejs」目前有哪有些版本，可以安裝。
    
    1
    
    $ nvm ls-remote

通常我會到「[nodejs官網](http://nodejs.org/)」首頁，了解目前stable的版本號。你會看到「Current Version: v0.10.32」

所以執行下面的指令，就可以透過「nvm」安裝「nodejs」了。(注意這個方式是安裝「binary package」，而非下載「source package」，編譯後安裝)
    
    1
    
    $ nvm install v0.10.32

或是省略前面的「v」
    
    1
    
    $ nvm install 0.10.32

上面兩個方式，是準確的指定某個版本號。

你也可以使用，下面兩個其中一個指令，安裝「v0.10」系列最新的。
    
    1
    
    $ nvm install v0.10

或
    
    1
    
    $ nvm install 0.10

寫此篇時，「v0.10」系列，最新的是「v0.10.32」，所以上面的指令就會安裝「v0.10.32」。

另外寫此篇時，看官方的文件，有紀錄到可以執行下面的指令
    
    1
    
    $ nvm install stable

不過我試不出來，會出現下面的訊息。
    
    1
    
    Version 'stable' not found - try `nvm ls-remote` to browse available versions.

然後有看到「[一篇文章](http://blog.wu-boy.com/2013/01/node-version-manager/)」，有討論這個問題([1](https://github.com/creationix/nvm/pull/188))([2](https://github.com/creationix/nvm/pull/136))([3](https://github.com/creationix/nvm/pull/74))。  
有興趣可以嘗試上面那篇文章作者改過的這個「[版本](https://github.com/appleboy/nvm)」摟。

當我們透過「nvm」安裝好「nodejs」後，因為可能同時安裝了很多版本的「nodejs」。  
所以我們要透過下面的指令來作切換目前要使用的「nodejs」版本。
    
    1
    
    $ nvm use v0.10.32

  
上面省略「v」和「v0.10」系列的模式，一樣可用。  
  
當然在執行上面的指令前，我們通常會先觀看，目前透過「nvm」裝了哪些版本的「nodejs」。  
  
  

    
    1
    
    $ nvm ls

可能會看到下面的字樣，前面有箭頭的就是目前使用的版本，而列出來的所有項目，則是目前透過「nvm」裝了哪些版本的「nodejs」。
    
    1
    
    2
    
    	v0.10.32
    
    ->	v0.11.14

而為了讓未來重新登入「shell」時，會使用一個預設的「nodejs」版本。

我們可以執行一下的指令，來設定。
    
    1
    
    $ nvm alias default v0.10.32

這時候你再次執行
    
    1
    
    $ nvm ls

就會看到多了「default」那一行。
    
    1
    
    2
    
    3
    
    	v0.10.32
    
    ->	v0.11.14
    
    default -> v0.10.32

這樣未來重新登入「shell」時，就會直接使用「v0.10.32」這個版本的「nodejs」。

你可以執行下面的指令，來觀看目前「nodejs」的版本。
    
    1
    
    $ node -v

會顯示
    
    1
    
    v0.10.32

一樣也可以執行「nvm ls」來觀看。
    
    1
    
    $ nvm ls

或是執行
    
    1
    
    $ nvm current

就會顯示
    
    1
    
    v0.10.32

另外安裝「nodejs」成功了，基本上也同時安裝了「[npm](https://www.npmjs.org/)」。  
你可以確認看看，執行
    
    1
    
    $ npm -v

或
    
    1
    
    $ npm --version

會出現
    
    1
    
    1.4.28

## 使用「nvm」下載「nodejs」的「source package」，編譯後，安裝。

跟上面的安裝「binary package」的指令差不多，只多了個「-s」。
    
    1
    
    $ nvm install -s v0.10.31

注意記得要編譯成功，記得先安裝編譯相依的套件。
    
    1
    
    $ sudo apt-get install build-essential libssl-dev curl git

好拉，安裝「nodejs」完畢後，來去「[NodeSchool](http://nodeschool.io/)」學習吧!
    
    1
    
    $ npm install -g learnyounode
    
    1
    
    $ learnyounode
