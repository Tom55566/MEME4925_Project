# 火災警報系統

## 目錄
1. [專案簡介](#專案簡介)
2. [功能介紹](#功能介紹)
3. [系統架構](#系統架構)
4. [安裝與設定](#安裝與設定)
5. [使用說明](#使用說明)
6. [文件目錄結構](#文件目錄結構)
7. [貢獻](#貢獻)
8. [版本歷史](#版本歷史)

## 專案簡介
此火災警報系統是一個基於樹莓派（Raspberry Pi）的嵌入式系統，用於監控環境中的溫度和一氧化碳濃度，並在偵測到火災風險時發出警報。該系統還能通過攝影機捕捉影像，並定期將圖片上傳到伺服器，方便使用者遠程查看現場狀況。

## 功能介紹
- **溫度監測**：使用 DHT22 感測器實時監控環境溫度。
- **一氧化碳濃度監測**：使用 MQ7 感測器監測空氣中一氧化碳濃度。
- **警報觸發**：當偵測到火災風險時，系統會觸發蜂鳴器發出警報聲。
- **遠程監控**：每 2 秒使用樹莓派相機拍攝現場影像，並上傳至指定伺服器供遠程查看。
- **數據同步**：使用分享記憶體（Shared Memory）和系統計時器（sys/timer）來確保各個感測器與設備的數據同步處理。

## 系統架構
本系統由以下幾個主要部分組成：
- **樹莓派（Raspberry Pi）**：作為系統主控單元，負責感測器數據的讀取、警報觸發以及影像捕捉與上傳。
- **DHT22 溫度感測器**：監控環境溫度。
- **MQ7 一氧化碳感測器**：偵測空氣中的一氧化碳濃度。
- **蜂鳴器**：在偵測到火災風險時發出聲音警報。
- **樹莓派攝影機**：定期捕捉現場影像並上傳至伺服器。
- **LED**: 為樹莓派攝影機提供光源。

## 安裝與設定
1. **硬體準備**：
   - BCM2711 樹莓派（Raspberry Pi 4B）
   - DHT22 溫度感測器
   - MQ7 一氧化碳感測器
   - HW508 蜂鳴器
   - 5V LED (White)
   - 5MP 樹莓派相機(OV5647)
  
2.  **設備連接**：
   - 請參照附圖
  
3. **系統建置**：
   - 系統版本: Linux raspberrypi 5.10.103-v7l
   - 系統架構: ARMv7 

4. **軟體安裝**：
   - 啟用樹莓派攝影機：
     ```sh
     sudo raspi-config
     ```
     選擇 "Interface Options" 然後啟用 Camera。
     
   - 更新系統軟體包
     ```sh
     sudo sudo apt update
     sudo sudo apt upgrade
     ```
   - 安裝 ffmpeg
     ```sh
     sudo apt install ffmpeg
     ```
   - 安裝 nvm
     ```sh
     wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
     source ~/.bashrc
     ```
   - 安裝 node.js
     ```sh
     nvm install --lts
     nvm install 18.15.0
     ```

## 使用說明
1. **啟動系統**：
   - 請先進入專案內的module目錄，請執行以下指令來編譯驅動程式：
   make
   - 再來回到進入專案目錄，請執行以下指令來啟動系統：
   ```sh
   sudo bash loadandRun.sh
   ```
   - 若要關閉系統，請按下Ctrl + C
     
   - 若要卸載驅動程式，請執行以下指令：
   ```sh
   sudo bash unloadDriver.sh
   ```
   - 若要單獨安裝驅動程式，請執行以下指令：
   ```sh
   sudo bash loadDriver.sh
   ```
   - 若要單獨啟動程式，請執行以下指令：
   ```sh
   sudo bash run.sh
   ```
   - 系統將開始監控環境中的溫度與一氧化碳濃度，並根據設置觸發警報。
   
2. **啟動伺服器**：
 - 請先進入專案內的excute目錄，請執行以下指令來啟動Node.js：
 ```sh
 sudo nvm use 18.15.0
 sudo ./server.js
 ```
  
3. **遠程監控**：
   - 開啟瀏覽器並訪問伺服器上的指定 URL，即可查看實時影像。

## 文件目錄結構
- `main.c`：主程式文件，負責系統的初始化和運行。
- `module/`：感測器、蜂鳴器及LED相關驅動程式。
- `camera.c`：處理樹莓派相機的程式碼和運行。
- `public/`：靜態文件夾，包含前端頁面。
- `Data/`: 感測器讀取的資料文件。

## 貢獻
歡迎任何形式的貢獻，包括但不限於功能添加、Bug 修復和文檔完善。如果你有任何建議或問題，請提交 Issue 或 Pull Request。


## 版本歷史
- **v1.4**
  - DHT11 溫濕度感測器更新為 DHT22 溫濕度感測器。

- **v1.3**
  - 完成Nodejs伺服器架設，回傳感測器所讀取的資料以及樹莓派攝影機回傳的影像。

- **v1.2**
  - 優化了感測器數據同步處理邏輯，減少了阻塞問題。

- **v1.1**
  - 增加了警報觸發功能，當偵測到火災風險時，系統會觸發蜂鳴器發出警報聲。
  - 完成了 DHT11 溫濕度感測器和 MQ7 一氧化碳感測器的整合。

- **v1.1**
  - 增加了警報觸發功能，當偵測到火災風險時，系統會觸發蜂鳴器發出警報聲。
  - 完成了 DHT11 溫度感測器和 MQ7 一氧化碳感測器的整合。
  - 優化了感測器數據同步處理邏輯，減少了阻塞問題。
  - 完成Nodejs伺服器架設，回傳感測器所讀取的資料以及樹梅派相機回傳的影像。

- **v1.0**
  - 初始版本：設置系統架構並完成基本的環境配置。

## 本系統與asd1999fff共同協作
## asd1999fff:https://github.com/asd1999fff/MEME49_project.git


