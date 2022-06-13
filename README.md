# 資安期末-Burp Suite

> Burp Suite，或者常簡稱Burp，是一套全面的 Web 應用程序安全測試工具，使用JAVA編寫。
> 

# 一、安裝
![image](https://user-images.githubusercontent.com/105283235/173318818-9fa4592b-1479-4ad4-aacd-fd28fd1cc1fe.png)
輸入email後便可下載
![image](https://user-images.githubusercontent.com/105283235/173319171-38409827-c9b3-4cb9-b1a3-ab75d999d0c1.png)
耶正在跑安裝啦~

# 二、****使用 Burp 代理攔截 HTTP 流量****
> 參考網址：[https://portswigger.net/burp/documentation/desktop/getting-started/intercepting-http-traffic](https://portswigger.net/burp/documentation/desktop/getting-started/intercepting-http-traffic)
> 

根據此網址跟隨步驟進行
## ****攔截請求****

### ****第 1 步：啟動 Burp 的瀏覽器****

Go to the **Proxy > Intercept** tab.

Click the **Intercept is off** button, so it toggles to **Intercept is on.**
![image](https://user-images.githubusercontent.com/105283235/173320445-be6b57d5-9234-4606-afdc-481924eaec08.png)
Click Open Browser.
![image](https://user-images.githubusercontent.com/105283235/173320959-a565155f-4bb8-4dc9-be73-1aab219f22fa.png)

### ****第 2 步：攔截請求****

使用 Burp 的瀏覽器，嘗試訪問`https://portswigger.net`並觀察該站點沒有加載。
![image](https://user-images.githubusercontent.com/105283235/173322362-ee348e69-24de-4bed-bbac-d240f0445a99.png)
但不知道為什麼沒有跑出被攔截的請求QQ
後來有跳出完成註冊帳戶的頁面，以為是因為這個，但完成註冊以後仍未跑出攔截請求QQ
![image](https://user-images.githubusercontent.com/105283235/173326375-681bb08b-4161-45cd-a835-adf957638527.png)
不論是直接搜尋還是網站搜尋都式過了，就是沒有跑出攔截請求Q
但...還是學一下接下來是怎麼做吧

### ****第 3 步：轉發請求****

Click the **Forward**以發送截獲的請求以及任何後續請求，直到頁面加載到 Burp 的瀏覽器中。
![image](https://user-images.githubusercontent.com/105283235/173326221-6dc3789e-a17f-4507-889e-3e361db4762e.png)
來來神奇的來了！點了Forward以後出現了出現了！攔截請求出現了！喔耶

### **第 4 步：關閉攔截**

由於瀏覽器通常發送的請求數量，您通常不想攔截每一個請求。Click the **Intercept is on**，現在它會顯示 **Intercept is off**。
![image](https://user-images.githubusercontent.com/105283235/173326749-9f2fd535-6c5f-4ad6-94ae-9260bd00c5bd.png)
返回瀏覽器並確認您現在可以正常與站點交互。
![image](https://user-images.githubusercontent.com/105283235/173327157-69c87282-30d9-4f46-86dc-0ed64ea7ee0f.png)

### **第 5 步：查看 HTTP 歷史記錄**

在 Burp 中，轉到**Proxy > HTTP history**選項卡。在這裡，您可以看到通過 Burp 代理的所有 HTTP 流量的歷史記錄，即使在攔截被關閉時也是如此。

單擊歷史記錄中的任何條目以查看原始 HTTP 請求以及來自服務器的相應響應。
![image](https://user-images.githubusercontent.com/105283235/173327053-e5be723b-0523-400d-9bb1-98aa62a2e218.png)
這讓您可以正常瀏覽網站，然後研究 Burp 的瀏覽器與服務器之間的交互，這在許多情況下更方便。

# 三、****使用 Burp 代理修改 HTTP 請求****
> 參考網址：[https://portswigger.net/burp/documentation/desktop/getting-started/modifying-http-requests](https://portswigger.net/burp/documentation/desktop/getting-started/modifying-http-requests)
> 

並根據以上網址步驟進行

## **第 1 步：在 Burp 的瀏覽器中訪問易受攻擊的網站**

在 Burp 中，轉到**Proxy > Intercept**選項卡並確保[攔截已關閉](https://portswigger.net/burp/documentation/desktop/tools/proxy/intercept#controls)。

啟動 Burp 的瀏覽器並使用它訪問以下 URL：

[https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-excessive-trust-in-client-side-controls](https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-excessive-trust-in-client-side-controls)

頁面加載後，單擊**Access the lab**。如果出現提示，請登錄您的 portswigger.net 帳戶。幾秒鐘後，您將看到您自己的虛假購物網站實例。
![image](https://user-images.githubusercontent.com/105283235/173329835-544b087b-2ed9-46b5-b948-3a2be096c6b7.png)

## **第 2 步：登錄您的購物賬戶**

在購物網站上，單擊**My account**並使用以下憑據登錄：
**用戶名：**`wiener`
**密碼：**`peter`
請注意，您只有 100 美元的商店信用。
![image](https://user-images.githubusercontent.com/105283235/173330479-d529b64c-594c-4edb-a4e5-5464433168e6.png)

## **第三步：找東西買**

單擊**Home**返回主頁。選擇該選項以查看**Lightweight "l33t" leather jacket**的產品詳細信息。
![image](https://user-images.githubusercontent.com/105283235/173330953-fc62795e-2ee3-4342-99bc-b63ae9e1c568.png)

## **第 4 步：研究添加到購物車功能**

在 Burp 中，轉到**Proxy > Intercept**選項卡並打開攔截。在瀏覽器中，將皮夾克添加到您的購物車以攔截生成的 `POST /cart`請求。
![image](https://user-images.githubusercontent.com/105283235/173331736-f2d84a8f-a0a2-488a-b17f-4a9f22f87c8d.png)
研究截獲的請求，並註意到正文中有一個名為的參數price，它與以美分為單位的商品價格相匹配。
### **筆記**

> 如果瀏覽器在後台執行其他操作， 您最初可能會在**Proxy > Intercept選項卡上看到不同的請求。**在這種情況下，只需單擊 **Forward**，直到您看到`POST /cart`上面屏幕截圖中顯示的請求。
> 

ㄚㄚㄚ這個筆記也是我一開始做遇到的問題啊！本來我的也沒出現`POST /cart`，原來是用這個方法

## **第五步：修改請求**

將該參數的值更改`price`為1，然後單擊“**Forward**”將修改後的請求發送到服務器。
![image](https://user-images.githubusercontent.com/105283235/173332513-aeba886e-492a-464a-90e4-56d93d1d8527.png)
再次關閉攔截，以便任何後續請求都可以不間斷地通過 Burp 代理。

## **第 6 步：利用漏洞**

在 Burp 的瀏覽器中，單擊右上角的購物籃圖標以查看您的購物車。請注意，這件夾克只花了一美分。
![image](https://user-images.githubusercontent.com/105283235/173333207-d1d9e503-77c7-4950-9a2b-7081bf4aa64b.png)
### **筆記**

> 無法通過 Web 界面修改價格。多虧了 Burp Proxy，您才能進行此更改。
> 

單擊“**Place order**”按鈕以極其合理的價格購買夾克。
![image](https://user-images.githubusercontent.com/105283235/173332735-6a86b111-c1d5-470c-9cdf-47cb65bc3293.png)

# 四、心得

經過上述的練習，我發現資安的世界我真的是觸碰太少了，也了解的太少了，甚至沒有想過這麼簡單的一些操作，就可以做出我意想不到的結果，利用阻擋網站修改內容等等，尤其是最後修改價前這個真的很令我驚訝，可能是以前總是只有聽過沒有真的看過、做過，覺得那需要很高深的技巧，但也許也是因為有Burp Proxy啦，但真的沒想過自己有一天跟著操作，雖然大部分內容仍是看不懂，但真的能做出結果，對於程式雖然真的不太了解，但很高興能通過這些比較簡易的操作接觸到這些，也讓我有點小小的成就感啦哈哈哈，之後我也會繼續利用這個網站慢慢地跟著步驟練習，希望能接觸到更多不同的、未嘗試過原本以為很難的事物。
