# 切片免疫室點貨系統

手機開網頁就能盤點：有安全量的品項填數量，低於安全量自動列入叫貨；沒有安全量的品項只要勾選「足夠／需叫貨」。盤點完可一鍵複製叫貨清單，或直接寄 Email。

## 一、放上 GitHub Pages

1. GitHub 新建一個 repository（例如 `ihc-inventory`）。
2. 上傳 `index.html` 和這份 `README.md`。
3. Repository → **Settings → Pages** → Source 選 `Deploy from a branch`，Branch 選 `main`、資料夾 `/ (root)` → Save。
4. 約 1 分鐘後網址會出現：`https://<你的帳號>.github.io/ihc-inventory/`，用手機開啟後可「加入主畫面」當 App 用。

> ⚠️ 免費版 GitHub Pages 是公開網頁，知道網址的人都看得到品項清單（看不到大家的盤點數量，那只存在各自手機裡）。

## 二、設定 EmailJS（免費方案每月 200 封）

1. 到 <https://www.emailjs.com> 註冊。
2. **Email Services → Add New Service** → 選 Gmail（或 Outlook），登入要拿來寄信的帳號 → 記下 **Service ID**。
3. **Email Templates → Create New Template**，設定：
   - **To Email**：`{{to_email}}`
   - **Subject**：`{{subject}}`
   - **Content**：
     ```
     盤點日期：{{date}}
     盤點人：{{counter}}
     需叫貨：{{order_count}} 項

     {{message}}
     ```
   - 儲存後記下 **Template ID**。
4. **Account → General** 找到 **Public Key**。
5. （建議）在 EmailJS 帳號的安全性設定中，若有網域白名單選項，只允許 `https://<你的帳號>.github.io`，避免別人拿你的 key 亂寄信。
6. 打開 `index.html`，找到最上方這段填入：
   ```js
   const EMAIL_CONFIG = {
     publicKey:  '你的 Public Key',
     serviceId:  '你的 Service ID',
     templateId: '你的 Template ID',
     toEmail:    'a@xxx.org, b@xxx.org'   // 收件人，可多人
   };
   ```
7. 存檔、重新上傳到 GitHub 即可。

沒設定 EmailJS 也能用，「複製」按鈕照常可以貼到 LINE 或信件。

## 三、日常使用

- 上方分頁切廠牌，每個分頁下方顯示「已點 / 總數」。
- 「未點」按鈕：只顯示還沒點到的品項；「需叫貨」：只顯示要叫的。
- 需叫貨的品項會出現「叫貨數量」，預設＝安全量 − 現有量（勾選類預設 1），可用 ＋／− 調整。
- 按「完成盤點」→ 填盤點人 → 複製或寄出。清單文字可以先手動修改。

## 四、更新品項

- **單台裝置小改**：直接在頁面上「新增／編輯」，只會存在該手機瀏覽器。
- **全部人一起更新**：改 `index.html` 裡的 `DEFAULT_ITEMS`，並把 `DATA_VERSION` 改成新日期，重新上傳。大家下次開啟會自動換成新清單（各自在頁面上做過的品項修改會被覆蓋）。
