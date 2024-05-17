**專案演示網站:** http://realcool.top:9090/


#### 一、專案介紹
基於區塊鏈Hyperledger Fabric V2.5的農產品溯源/商品/通用溯源應用模板，部署簡單，附壓測工具tape、區塊鏈瀏覽器，文件詳細。可以快速使用本系統建立自己的溯源系統，幫助想法快速落地。採用的技術堆疊：Fabric V2.5、Gin、Vue.js、Mysql。

![專案系統架構圖](https://truetechlabs-1259203851.cos.ap-shanghai.myqcloud.com/picgo319183738-60337eb8-1799-435f-b0a5-8ac61761aa28.png)
![多類型使用者註冊](https://truetechlabs-1259203851.cos.ap-shanghai.myqcloud.com/picgo202404151236356.jpg)
![農產品上鍊](https://truetechlabs-1259203851.cos.ap-shanghai.myqcloud.com/picgo202404151238978.png)
![農產品溯源](https://truetechlabs-1259203851.cos.ap-shanghai.myqcloud.com/picgo202404151238196.png)
![區塊鏈瀏覽器視覺化](https://truetechlabs-1259203851.cos.ap-shanghai.myqcloud.com/picgo202404151239574.png)

##### 專案位址：
**Github**:
[https://github.com/TrueTechLabs/fabric-trace](https://github.com/TrueTechLabs/fabric-trace)

**Gitee**:
[https://gitee.com/real__cool/fabric-trace](https://gitee.com/real__cool/fabric-trace)

##### 建立視訊位址：
[https://www.bilibili.com/video/BV1Ar421H7TK](https://www.bilibili.com/video/BV1Ar421H7TK)

##### 附完整一刀未剪的搭建影片:
示範使用臨時伺服器，建議使按年付費價格划算一些，具體購買方式請參考下方搭建步驟。
[https://www.bilibili.com/video/BV1mF4m1P7Go](https://www.bilibili.com/video/BV1mF4m1P7Go)。

##### 專案文件地址(部分內容需要訂閱欄位）：
[https://blog.csdn.net/qq_41575489/category_12075943.html](https://blog.csdn.net/qq_41575489/category_12075943.html)

##### 專案程式碼講解與修改前端課程：
[B站：Fabric V2.5通用溯源專案解說與二次開發課程](https://www.bilibili.com/cheese/play/ss15923?bsource=link_copy)


#### 二、版權聲明
本專案基於Apache License 2.0開源協議，在個人的科學研究、學習範圍內可自由使用，請附上專案連結。如有商業需求或合作需求，請填寫收集表：[【騰訊文檔】本專案搭建服務或商務合作意願收集](https://docs.qq.com/form/page/DQ1hIck5OQkNGQXF2)
。如果您不同意本聲明請不要使用本項目。

#### 三、專案特點
本專案採用Hyperledger Fabric V2.5，屬於目前最新的Fabric版本，具有較好的效能與穩定性，呼叫鏈碼使用fabric-gateway模式，是目前版本的推薦方式。內建了tape壓測工具，可以方便的對區塊鏈網路進行壓測；內建了區塊鏈瀏覽器，可以方便地查詢交易資訊。
專案結構清晰，程式碼註解詳細，方便二次開發。結合了mysql實現帳戶註冊登入功能，更符合真實業務場景。

#### 四、專案背景
區塊鏈技術的出現，為溯源系統的建置提供了新的思路。區塊鏈技術的不可篡改性、去中心化、可追溯等特點，使得區塊鏈技術成為溯源系統的理想選擇。本計畫基於Hyperledger Fabric V2.5，實現了一個農產品溯源系統。在本區塊鏈系統中，有5個內建的角色：種植者、工廠、駕駛者、商店、消費者。其中種植者、工廠、駕駛、商店可以將資訊上鍊，消費者有資訊溯源權限。上述可以上鍊資訊的角色各可以輸入5個農產品的屬性，方便二次開發。本專案的目標是作為Fabric V2.5下的一個通用溯源模板。

#### 五、搭建步驟
> 若部分內容與影片不一致請以本文檔為準

 強烈建議依照步驟使用雲端伺服器搭建本系統，虛擬機器的問題較多。點擊此連結直達新人活動頁面：[https://curl.qcloud.com/Sjy0zKjy](https://curl.qcloud.com/Sjy0zKjy) 點擊首單特惠，**購買2核4G或以上的伺服器* *，199/年（價格經常會調整），如果後續準備做程式開發可以用新用戶優惠買三年的，安裝Ubuntu20.04系統。

**嚴格按照以下步驟操作，以下步驟已經經過上百人次的驗證，如果遇到報錯請仔細檢查是否遺漏某個步驟：**


1. 安裝docker

『`bash
#下載docker
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
#新增目前使用者到docker使用者群組
sudo usermod -aG docker $USER
newgrp docker
#配置docker映像加速
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://punulfd2.mirror.aliyuncs.com"]
}
EOF

#重啟docker
sudo systemctl daemon-reload
sudo systemctl restart docker
```

2. 安裝開發使用的go、node、jq

『`bash
#下載二進位包
wget https://golang.google.cn/dl/go1.19.linux-amd64.tar.gz
#將下載的二進位套件解壓縮至 /usr/local目錄
sudo tar -C /usr/local -xzf go1.19.linux-amd64.tar.gz
mkdir $HOME/go
#將以下內容新增至環境變數 ~/.bashrc
export GOPATH=$HOME/go
export GOROOT=/usr/local/go
export PATH=$GOROOT/bin:$PATH
export PATH=$GOPATH/bin:$PATH
#更新環境變數
source ~/.bashrc
#設定代理
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct

#下載nvm安裝腳本
wget https://gitee.com/real__cool/fabric_install/raw/main/nvminstall.sh
#安裝nvm；螢幕輸出內容新增環境變數
chmod +x nvminstall.sh
./nvminstall.sh
# 將環境變數寫入.bashrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
# 更新環境變數
source ~/.bashrc
# 安裝node16
nvm install 16
#換源
npm config set registry https://registry.npmmirror.com

#安裝jq
sudo apt install jq
```



3. 克隆本項目

『`bash
git clone https://gitee.com/real__cool/fabric-trace
```

4. 啟動區塊鏈部分。在fabric-trace/blockchain/network目錄下:

『`bash
# 僅在首次使用執行：下載Fabric Docker映像。如果拉取速度過慢或失敗請檢查是否完成docker換源，或更換一個其他的映像源再試。
./install-fabric.sh -f 2.5.6 d
```
『`bash
# 啟動區塊鏈網絡
./start.sh
```
 **如果在啟動區塊鏈網路時遇到報錯可以嘗試:**
『`bash
# 執行清理所有的容器指令：
docker rm -f $(docker ps -aq)
```
**然後再重新啟動區塊鏈網路**

5. 啟動後端 在fabric-trace/application/backend目錄下： 執行： `go run main.go`

6. 修改後端IP，將下列檔案中的IP：`119.45.247.29`，換成自己雲端服務的IP。
『`bash
fabric-trace/application/web/.env.development
fabric-trace/application/web/.env.development
fabric-trace/application/web/src/router/index.js
```
或使用application/replaceip.sh腳本根據指引修改IP，在fabric-trace/application目錄下
『`bash
./replaceip.sh
```

7. 新開一個窗口，啟動前端 在fabric-trace/application/web目錄下： 執行：

『`bash
# 僅在首次運行執行：安裝依賴
npm install
```

『`bash
# 啟動前端
npm run dev
```
8. 在騰訊雲端輕量應用程式伺服器防火牆頁面，放行TCP埠`8080,9090,9528`
![防火牆配置](https://truetechlabs-1259203851.cos.ap-shanghai.myqcloud.com/picgo202404151240899.png)
9. 在瀏覽器中開啟：http://雲端伺服器IP:9528 即可看到前端頁面。
10. 使用tape對專案進行壓力測試
根據blockchain/chaincode/chaincode/smartcontract.go中的合約函數的簽名，編寫壓測的參數，需要修改的內容是tape目錄下的yaml檔案中的args。 args第一個參數是函數名，後面的參數是函數的參數。例如：
```yaml
args:
# 函數名
- RegisterUser
# userID string
- 1
#userType string
- randomString8
# realInfoHash string
- randomString8
```
執行`./tape --config config_register.yaml -n 1`即可完成使用者1的註冊，然後可以將農產品上鍊操作與取得使用者資訊函數進行壓測。更多的壓測案例可以根據合約函數的簽名進行修改。
附農產品上鍊操作與取得使用者資訊函數進行壓測操作指令：

『 bash
./tape --config config_invoke.yaml -n 100
./tape --config config_query.yaml -n 100
```

#### 六、關閉專案與重新運行步驟
##### 關閉項目：
1. 前端（`npm run dev`介面）與後端（`go run main.go`介面：

使用鍵盤組合鍵：`ctrl+c`

2. 區塊鏈部分：

在`fabric-trace/blockchain/network`目錄`./stop.sh`，此步驟會清理所有的區塊鏈、Mysql中的資料。

##### 開發模式啟動專案：
1. 在`fabric-trace/blockchain/network`目錄
`./start.sh` 如果遇到報錯可以執行以下指令後再試：
執行清理所有的容器指令：
`docker rm -f $(docker ps -aq)`
2. 在`fabric-trace/application/backend`目錄下： 執行： `go run main.go`
3. 在`fabric-trace/application/web`目錄下： 執行：
`npm run dev`
4. 在http://伺服器IP:9528打開

##### 生產環境部署專案(後台運行，存取速度更快)
1. 在`fabric-trace/blockchain/network`目錄
`./start.sh` 如果遇到報錯可以執行以下指令後再試：
執行清理所有的容器指令：
`docker rm -f $(docker ps -aq)`
2. 在`fabric-trace/application`目錄下： 執行： `./start.sh`
3. 在http://伺服器IP:9090打開

注意：此方式部署專案會在背景執行，如果後續遇到連接埠號碼佔用可以嘗試關閉佔用9090連接埠號碼的進程，可以參考：
[解決連接埠佔用 bind:address already in use](https://blog.csdn.net/qq_41575489/article/details/137434008?spm=1001.2014.3001.5501)

#### 七、本項目相關的後續計畫：

1. 本專案目前不夠完美，將持續維護，歡迎給專案點亮Star與B站三連，非常感謝！與B站官方合作，本計畫代碼講解與二次開發課程已發布，限時特價219元，支持試看: [B站：Fabric V2.5通用溯源項目講解與二次開發課程](https://www .bilibili.com/cheese/play/ss15923?bsource=link_copy),購買課程將贈送[《Fabric專案學習筆記》](https://blog.csdn.net/qq_41575489/article/details/128637560)中所有與本項目相關的資料。購買課程與訂閱專欄均配套社群，方便交流與答疑。
2. 支援Docker方式部署，簡化部署步驟
3. 改善農產品資訊上鍊流程，需依照角色順序上鍊

#### 八、特別提示：
1. 區塊鏈瀏覽器有時候會出現無法存取的情況，可以嘗試重新啟動瀏覽器容器。
2. 為了減少使用者執行本專案時的難度，區塊鏈目錄的start.sh腳本在啟動區塊鏈時同時會清理掉所有的歷史資料！如果重啟機器後不希望清理原來的資料啟動區塊鏈，可以使用指令：`docker start $(docker ps -aq)`啟動所有節點

#### 如果無法依照步驟執行項目
上述部署步驟已上百人次驗證並順利完成，如果您通過上述步驟未能運行項目，請檢查環境是否與本項目要求的一致，任何修改或遺漏步驟都可能引起項目不能正常運行，請嚴格按照視頻與文章步驟再次嘗試，[常見問題與解決方案清單參考](https://blog.csdn.net/qq_41575489/article/details/137886728)。若還是有問題請在[B站專案搭建影片](https://www.bilibili.com/video/BV1Ar421H7TK)評論區查看其他人的留言是否有相同的問題，如果還是沒有解決請在影片下評論問題並附上第一個遇到的報錯，如果問題不夠明確，我們也很難幫助您。購買[B站：Fabric V2.5通用溯源專案講解與二次開發課程](https://www.bilibili.com/cheese/play/ss15923?bsource=link_copy)或訂閱[《Fabric專案學習筆記》] (https://blog.csdn.net/qq_41575489/article/details/128637560)可以加入配套社群，方便本計畫交流與答案。




## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=TrueTechLabs/fabric-trace&type=Date)](https://star-history.com/#TrueTechLabs/fabric-trace&Date)
