# 嵌入式慣性導航系統 EGI-M320 操作說明
- 嵌入式慣性導航系統 EGI-M320 操作說明
    - [簡介](##簡介)
    - [搭配線材](##搭配線材)
    - [外觀及接口設置](##外觀及接口設置)
    - [系統 Directory Structure](##系統-Directory-Structure)
    - [操作流程](##操作流程)
        - [Step 1. Power及線路連接](#Step-1.-Power及線路連接)
        - [Step 2. 使用Tera Term進行指令操作](#Step-2.-使用Tera-Term進行指令操作)
        - [Step 3. Tera Term設置](#Step-3.-Tera-Term設置)
        - [Step 4. EGI-M320 開機登入](#Step-4.-EGI-M320-開機登入)
        - [Step 5. 等待時間同步](#Step-5.-等待時間同步)
        - [Step 6. 開始資料收集](#Step-6.-開始資料收集)
        - [Step 7. 結束資料收集](#Step-7.-結束資料收集)
        - [Step 8. 資料匯出](#Step-8.-資料匯出)
        - [Step 9. 重新開始資料收集](#Step-9.-重新開始資料收集)
        - [Step 10. 結束關機](#Step-10.-結束關機)
    - [資料格式](##資料格式)
## 簡介
EPSON IMU在近期得到了使用者廣泛的在自動化精密農業機械、智能建築機械和無人車應用，並以優異的性能和質量贏得了良好的聲譽。EPSON M-G320之應用領域屬於自動化精密農業機械、智能建築機械和無人車應用。
## 搭配線材
一套完整EGI-M320需搭配以下物件:
| 由本團隊提供之物件 | 數量 |
| --------------- | --- |
| EGI-M320        | 1個 |
| Power(電池or電供) | 1個 |
| EGI-M370線     | 1條 |
| GNSS天線     | 1個    |

| 本團隊未提供之物件 | 數量 |
| ------------ | ------ |
| RS-232 to USB | 1條 |

![320線材](https://hackmd.io/_uploads/B1MaltDu0.png)

## 外觀及接口設置
![320接口](https://github.com/CC4534534/EGI-M320/blob/3114765518915d263ef48aaff08e408046074d06/320%E6%95%99%E5%AD%B8%E5%9C%96%E7%89%87/320%E6%8E%A5%E5%8F%A3.jpg)

## 系統 Directory Structure
```
root
├── .bash_history
├── .bashrc
├── .cache
├── .config
├── GNSSINS
│   ├── epson_decoder.py
│   ├── logs
│      ├── EPSON320-流水號.bin
│   ├── save_log.py
├── .gnupg
├── .lesshst
├── .local
├── .profile
├── .python_history
├── .selected_editor
├── .vim
├── .viminfo
├── CLIENT 
```
## 操作流程
### Step 1. Power及線路連接
- Power: 12V，利用電池供電，連接至Power接口
- 天線:連接至GPS ANT
### Step 2. 使用Tera Term進行指令操作
Tera Term，是一款開放原始碼的遠程客戶端操作軟體，一開始是由日本物理學家寺西高開發並發布的，之後是由TeraTerm Project在BSD許可證下進行維護支持。
該軟體支持的通信協議有SSH、telnet、序列埠通信，僅支持Microsoft Windows操作系統。
 - 官網下載:[Tera Term Open Source Project](<https://teratermproject.github.io/index-en.html>)
### Step 3. Tera Term設置
![image](https://hackmd.io/_uploads/SyWVPCVKC.png)
開啟Tera Term，點選Setup > Serial Port。
- 在Port設置COM port，可藉由查看裝置管理員得知，如下圖。
![image](https://hackmd.io/_uploads/ry9Bw04Y0.png)

- 在Speed設置Baud rate為115200
### Step 4. EGI-M320 開機登入
上電後如果有連接電腦會出現開機與登入畫面，如果有連電腦沒有畫面就按Enter
![image](https://hackmd.io/_uploads/H1AvDRNt0.png)
沒有連接電腦也會自動登入與收資料，只是沒有螢幕顯示
### Step 5. 等待時間同步
開機後會自動開始時間同步(GNSS透空度佳時，此過程約1~3分鐘，如果有連接電腦可以輸入：
1. ```cd GNSSINS/logs```
2. ```ls -al```
![螢幕擷取畫面 2024-08-01 140619](https://hackmd.io/_uploads/SJQRLidKA.png)

這時會出現logs內的檔案，可以看最新的EPSON320-流水號.bin檔案大小有沒有變大來判斷有無時間同步完開始收資料，有成功的話多次輸入```ls -al```可以發現檔案會變大，上圖紅框的資料顯示0代表尚未開始收資料，如果有收到的話數字會從1024、2048...開始變大，代表成功收到資料。
### Step 6. 開始資料收集
開機後完成時間同步後即開始資料收集
### Step 7. 結束資料收集
直接斷電或在命令視窗輸入：```shutdown now```
即完成資料收集，資料為EPSON320-流水號.bin
### Step 8. 資料匯出
先確定有連接DATA接口，由RS-232匯出資料
重新開機後，在Tab file內選change directory以選擇資料傳輸目的地
![螢幕擷取畫面 2024-07-29 173149](https://hackmd.io/_uploads/rktib1StR.png)
到傳輸你要的檔案的資料夾，並輸入下方指令：
1. ```$ cd your_file_directory // cd GNSSINS/logs```
2. ```$ sz your_filename // sz EPSON320-流水號.bin```

點選file -> Transfer -> ZMODEM -> Receive，等待資料匯出完畢
![螢幕擷取畫面 2024-07-29 173234](https://hackmd.io/_uploads/SkEaZJHKA.png)
### Step 9. 重新開始資料收集
斷電後重新上電就是一個新的資料收集過程，要再收一次可由[Step 4.](#Step-4.-EGI-M320-開機登入)開始

## 資料格式
收完資料後會產出：EPSON320-流水號.bin->需要進行轉檔
