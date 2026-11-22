---
marp: true
size: 16:9
paginate: true
lang: zh-TW
author: Denny Huang（黃一晉）
title: 軟體物料清單（SBOM）介紹
description: 廠商交付之後，我們要看什麼？臺北市政府開源指引培訓。
---

<script>
if (window.location.hostname === 'denny.one') {
  window.dataLayer = window.dataLayer || [];
  window.gtag = function () { window.dataLayer.push(arguments); };
  window.gtag('js', new Date());
  window.gtag('config', 'G-SJL6EQRW3V');
  const script = document.createElement('script');
  script.async = true;
  script.fetchPriority = 'low';
  script.src = 'https://www.googletagmanager.com/gtag/js?id=G-SJL6EQRW3V';
  document.head.appendChild(script);
}
</script>

# 軟體物料清單（SBOM）

## 廠商交付之後，我們要看什麼？

Denny Huang（黃一晉）

臺北市政府開源指引培訓
2026.12.09／12.11

<!--
時間：0:00–0:30，共 30 秒。
「上午已經看過指引和檢核表。接下來，我們真的打開一份 SBOM，看看收到檔案以後可以做什麼。」
-->

---

# Denny Huang

- SITCON 學生計算機年會 共同發起人
- COSCUP 開源人年會、HITCON 台灣駭客年會 長期志工
- 曾任雷亞遊戲 資料分析團隊主管

[About me](https://denny.one/)

<!--
時間：0:30–1:00，共 30 秒。
從自己的開源社群與軟體實務經驗切入，接到今天的工作情境。
來源：講者提供之參考簡報，自介頁。
https://github.com/denny0223/20261120-google-cloud/blob/gh-pages/slide.md
-->

---

# 「系統交付了，SBOM 也附上了」

假設你正在辦理一套便民預約系統的交付。

| 收到的東西 | 廠商的說明 |
| --- | --- |
| 預約系統 2.4 | 這次交付的版本 |
| 一份 SBOM JSON | 已經產製完成 |
| 格式檢查結果 | 通過 |

**接下來，你會先看什麼？**

<!--
時間：1:00–5:00，共 4 分鐘。第一次短互動。
明說這是教學假設情境，不是特定機關的真實採購案。
先給 30 秒思考，請兩位學員各說一件會先確認的事。若沒有人發言，可以提示：「你會先看檔名？版本？還是請廠商說明？」
不急著判對錯，記住大家的問題，後面讀樣本時逐一回扣。
收束：「檔案收到了。接下來要知道，裡面寫的內容能幫我們確認哪些事。」
-->

---

# 一套系統裡，還有哪些人的程式？

以預約系統為例：

| 看到的功能 | 可能用到的元件 |
| --- | --- |
| 預約規則 | 廠商開發的程式 |
| 網頁與登入 | 網頁框架、驗證套件 |
| 報表下載 | PDF、試算表套件 |

**套件本身，也可能使用其他套件。**

<!--
時間：5:00–6:30，共 1 分 30 秒。
表格為假設情境，不表示任何實際機關系統使用了特定套件。
用「你直接選用的套件，還可能帶進別的套件」說明直接與間接相依。先建立直覺，稍後再看真實依賴資料。
來源：CycloneDX SBOM 功能說明。
https://cyclonedx.org/capabilities/sbom/
-->

---

# 什麼時候會用到這份清單？

| 工作情境 | 想知道的事 |
| --- | --- |
| 系統交付 | 這個版本包含哪些元件？ |
| 接到漏洞通知 | 我們有用到嗎？版本是多少？ |
| 更新或維護交接 | 哪些元件換了？後續由誰處理？ |

授權資訊，也能成為後續合規審查的線索。

<!--
時間：6:30–8:30，共 2 分鐘。
漏洞通知可以假設為「某套件的某個版本出現問題」，不必展開具體事件或編造 CVE。
強調知道元件名稱與版本，可以開始確認影響；是否真的受到影響、要採什麼處置，還需要技術判讀。
授權欄位提供線索，仍需配合實際使用、修改、散布方式與合規資料。
來源：CycloneDX SBOM 功能說明；資安院 SBOM 與 OSV 工具教材；主辦提供之會後版 v2 草案，伍、三及 FAQ10、FAQ11。
https://cyclonedx.org/capabilities/sbom/
https://material.nics.nat.gov.tw/material/maintainable/Guide_to_SBOM_and_OSV_Tools/
-->

---

# SBOM：軟體物料清單

Software Bill of Materials

- 記錄軟體元件、版本與相依關係
- 使用有明確規則的格式，讓工具能讀取、比對

**讀這份清單時，要一起確認它對應的軟體與範圍。**

<!--
時間：8:30–10:00，共 1 分 30 秒。
JSON 是一種以欄位和值表示資料的文字格式。今天只要能找到欄位、讀懂值，就能開始判讀。
SBOM 可以由不同階段的資料產生，涵蓋範圍也可能不同。不能只因為檔名叫 SBOM，就推論已完整盤點所有環境。
來源：CycloneDX SBOM 功能說明與 1.6 規格。
https://cyclonedx.org/capabilities/sbom/
https://cyclonedx.org/docs/1.6/json/
-->

---

# SPDX 與 CycloneDX

兩種常見的國際標準

| 標準 | 今天使用的公開樣本 |
| --- | --- |
| CycloneDX | 官方工具的 Laravel 示範檔，1.6／JSON |
| SPDX | Python 官方發布檔，2.3／JSON |

交付前，先約定**標準、版本與需要的內容**。

<!--
時間：10:00–12:00，共 2 分鐘。
標準規定欄位意義與結構，JSON 是資料表示方式；兩者分開說。
這裡的 1.6 與 2.3 是本次樣本採用的格式版本，不宣稱是最新版本，也不表示它們就是臺北市所有採購案的指定版本。
選擇時要看機關與廠商工具支援、實際交付需要，以及契約或適用公告。兩種標準都能用於 SBOM，這堂課不作功能優劣排名。
來源：SPDX 官方規格頁；CycloneDX 1.6 規格；兩份公開樣本。
https://spdx.dev/use/specifications/
https://cyclonedx.org/docs/1.6/json/
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
https://www.python.org/ftp/python/3.14.0/python-3.14.0-amd64.exe.spdx.json
-->

---

# DEMO：打開一份真的 SBOM

[CycloneDX 官方工具產製範例](https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json)

| 先找什麼？ | 本次樣本 |
| --- | --- |
| 描述的專案 | `cyclonedx-php-composer-demo` |
| 專案版本 | `dev-master` |
| 元件清單 | `components`，62 筆 |

接著對照原始欄位。

<!--
時間：12:00–13:00，共 1 分鐘。
先展示這頁中文摘要，再開啟來源檔。教材包內保留同一份原始 JSON：materials/laravel-7.12.0.cdx-1.6.json。
這是官方工具儲存庫中的產製示例，有真實元件與相依關係；它不是臺北市預約系統的資料，也不是一份保證可驗收的範本。
62 是頂層 components 陣列筆數，不包含另列於 metadata.component 的根專案。數量不能證明涵蓋完整。
網路無法開啟時，接著用後面幾頁的實際摘錄帶讀。
來源與固定 commit：
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
-->

---

# 三個版本，各自在說什麼？

| 原始欄位 | 值 | 意義 |
| --- | --- | --- |
| `specVersion` | `1.6` | CycloneDX 格式版本 |
| 最外層的 `version` | `1` | 這份 BOM 的文件版本 |
| `metadata.component.version` | `dev-master` | 被描述的專案版本 |

**要核對交付版本，先找對欄位。**

<!--
時間：13:00–15:00，共 2 分鐘。
在原始檔分別搜尋 specVersion 與 metadata，讓學員看到 version 這個字會出現在不同位置。
示範檔的 metadata.component.name 為 cyclonedx-php-composer-demo，version 為 dev-master。
dev-master 是樣本提供的版本標示。若要對應實際交付物，還要有廠商的版本或建置對照，不能自己把資料夾名稱 laravel-7.12.0 當成根專案版本。
來源：同一份固定樣本；CycloneDX 1.6 的 specVersion、version、metadata.component 定義。
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
https://cyclonedx.org/docs/1.6/json/
-->

---

# 元件名稱之外，還要看版本

`components` 中的一筆，摘錄：

```json
{
  "group": "laravel",
  "name": "framework",
  "version": "v7.30.6",
  "purl": "pkg:composer/laravel/framework@v7.30.6"
}
```

這裡記錄的是 **`laravel/framework`，版本 `v7.30.6`**。

<!--
時間：15:00–17:00，共 2 分鐘。
先看名稱與版本，再介紹 purl 是讓工具辨識元件的識別資訊；不要求學員記住完整語法。
這段保留原始值，只選出與本頁判讀有關的欄位，不能把摘錄單獨當作完整 SBOM 送驗。
同一筆原始資料也有 licenses，標示 MIT 且 acknowledgement 為 declared。這是資料中的授權宣告，不能直接當成整個系統的授權合規結論。
來源：固定樣本中 bom-ref 為 laravel/framework-7.30.6.0 的元件。
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
-->

---

# 這個專案，怎麼用到這個元件？

從樣本的 `dependencies` 摘出一條相依路徑：

| 層次 | 元件 | 版本 |
| --- | --- | --- |
| 根專案 | `cyclonedx-php-composer-demo` | `dev-master` |
| 直接相依 | `laravel/laravel` | `v7.12.0` |
| 再往下一層 | `laravel/framework` | `v7.30.6` |

**只看直接選用的套件，可能漏掉後面的相依元件。**

<!--
時間：17:00–20:00，共 3 分鐘。
表格依原始 dependencies 的 ref／dependsOn 整理，這是其中一條路徑，不是完整相依圖。
在原始 JSON 搜尋 dependencies，再找根專案的 bom-ref。根專案指向 laravel/laravel-7.12.0.0，該元件再指向 laravel/framework-7.30.6.0 等元件。
說明 ref 是這份資料裡用來連接元件的識別值，不要求學員背欄位。
如果學員問為什麼有 7.12.0 和 7.30.6：它們是不同元件的版本，不能合併成一個「Laravel 版本」。
來源：固定樣本的 components 與 dependencies。
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
-->

---

# 誰產生的？什麼時候產生的？

`metadata.tools` 中的工具紀錄：

```json
{
  "vendor": "cyclonedx",
  "name": "cyclonedx-php-composer",
  "version": "in-dev"
}
```

這份樣本**沒有 `metadata.timestamp` 產製時間欄位**。

<!--
時間：20:00–22:00，共 2 分鐘。
工具紀錄有助追溯，但工具名稱不等於實際交付責任人。還要知道由誰產製、由誰交付及保留哪些佐證。
in-dev 也沒有提供固定工具版本或 commit；樣本沒有 timestamp，不能用檔案下載時間或 GitHub 顯示的更新時間代替產製時間。
這些資訊是否必填，要分清標準的結構要求與本案交付要求。下一段會用檢查器呈現這個差異。
來源：固定樣本完整 metadata；CycloneDX 1.6 metadata 定義。
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
https://cyclonedx.org/docs/1.6/json/
-->

---

# 現在，可以問得更具體了

- `dev-master` 對應哪一個交付版本？
- 這份清單何時產製，使用哪個工具版本？
- 清單涵蓋哪些交付內容？有哪些沒有包含？

**有疑問時，把欄位和值一起帶進問題。**

<!--
時間：22:00–24:00，共 2 分鐘。
回扣開場學員提出的問題，指出現在已有檔案與欄位可以對照。
這三個問題是把公開樣本放進假設交付情境後的示範，不是指控上游專案違反不存在的採購契約。
轉場：「接下來看看，哪些問題可以交給工具檢查，哪些還需要人補上說明。」
依據：前面讀取的固定樣本與教學假設情境。
-->

---

# DEMO：線上檢查

[sbomify SBOM Checker](https://sbomify.com/tools/sbom-checker/)

1. 開啟 **Paste JSON instead**
2. 貼上完整的公開範例
3. 按 **Check pasted SBOM**

先看 **Schema**，再讀它實際檢查的項目。

<!--
時間：24:00–27:00，共 3 分鐘。
示範用的是完整公開樣本，不是投影片上的摘錄。講師預先開好官方樣本與檢查器兩個分頁。
網站也支援選擇 JSON 檔案。可使用教材包內的 materials/laravel-7.12.0.cdx-1.6.json。
Schema 可以解釋成「這個格式的欄位與結構規則」。先看格式版本是否被正確辨識，再看結構檢查。
現場若無法使用網站，直接接下一頁的固定樣本結果與原始摘錄，不在課堂排除網路問題。
本堂只使用公開教材。機關實際資料應依核准工具及資料處理規定辦理。
來源：檢查器官方使用介面及說明。
https://sbomify.com/tools/sbom-checker/
-->

---

# 格式通過了，還缺什麼？

本次 Laravel 樣本

| 檢查項目 | 看到的結果 |
| --- | --- |
| CycloneDX 1.6 Schema | 通過 |
| 專案版本 | `dev-master` |
| 產製工具版本 | `in-dev` |
| 產製時間 | 未提供 |

**格式檢查通過，仍要核對本案需要的資訊。**

<!--
時間：27:00–28:30，共 1 分 30 秒。
這份固定樣本先前在 sbomify 線上檢查通過，2026-11-23 另以 CycloneDX 官方 1.6 JSON Schema 本機復核，結構錯誤為 0。
timestamp 在這個 Schema 中不是必填，因此缺少它不會單獨造成這項結構驗證失敗；採購案仍可能要求產製時間。
線上工具顯示的其他評分與檢查規範，要看清適用目的，不能直接當成臺北市驗收標準。
來源：固定樣本及官方 Schema。
https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json
https://cyclonedx.org/schema/bom-1.6.schema.json
https://sbomify.com/tools/sbom-checker/
-->

---

# 這份 Python SBOM，檢查出什麼？

[Python 3.14.0 Windows x64 的 SBOM](https://www.python.org/ftp/python/3.14.0/python-3.14.0-amd64.exe.spdx.json)

SPDX 2.3 Schema 檢查發現欄位值不符規則：

| 檔案中的值 | Schema 接受的值 |
| --- | --- |
| `PACKAGE_MANAGER` | `PACKAGE-MANAGER` |

**先辨識錯誤在哪裡，再確認如何補正。**

<!--
時間：28:30–30:00，共 1 分 30 秒。
這是固定的歷史發布樣本，不是「最新 Python」。資料內 creationInfo.created 為 2025-10-07T11:38:34Z。
先前線上檢查出現 21 個 Schema 問題。本教材於 2026-11-23 使用 SPDX 官方 v2.3 Schema 本機復核，也得到 21 個錯誤，都是 externalRefs.referenceCategory 使用 PACKAGE_MANAGER，不符合列舉值 PACKAGE-MANAGER。
只用一個欄位示範看懂錯誤，不逐一修 21 處。教材提供原始發布檔。
這項結果是該檔案與指定 Schema 的結構差異，不能推論 Python 本身不安全。
來源：Python 原始發布檔；SPDX 官方 v2.3 JSON Schema。
https://www.python.org/ftp/python/3.14.0/python-3.14.0-amd64.exe.spdx.json
https://raw.githubusercontent.com/spdx/spdx-spec/v2.3/schemas/spdx-schema.json
-->

---

# 檢查結果，回答了哪一個問題？

| 想確認什麼？ | 要看什麼？ |
| --- | --- |
| 格式符合規則 | 標準、版本、檢查結果 |
| 資料可以追溯 | 交付者、工具、時間與產製方式 |
| 對得上交付內容 | 軟體版本、產物對應與涵蓋範圍 |
| 安全與授權風險的處理情況 | 另外的分析、聲明與處置紀錄 |

<!--
時間：30:00–32:00，共 2 分鐘。
這是課堂判讀架構，不是機關新訂的驗收等級。
來源可信、格式正確、內容對應與風險處理是不同問題。一個工具可能檢查其中幾項，但要依實際功能與規則確認，不能只看總分或顏色。
授權合規聲明是草案另列的交付資料，不由 SBOM 的 license 欄位取代。
來源：主辦提供之會後版 v2 草案，伍、三（二）至（四）、FAQ10 至 FAQ12；檢查器功能說明。
https://sbomify.com/tools/sbom-checker/
-->

---

# SBOM 從哪裡產生？

| 產製時使用的資料 | 可以看到什麼？ |
| --- | --- |
| 套件清單、鎖定檔 | 宣告或解析出的套件與版本 |
| 建置流程 | 當次建置使用的元件資訊 |
| 安裝包、容器映像等產物 | 工具能辨識的已打包內容 |

**先問「從哪裡產生」，才知道還要補看哪裡。**

<!--
時間：32:00–34:00，共 2 分鐘。
鎖定檔可解釋成開發工具記下的具體套件版本。建置可解釋成把程式與依賴整理成可交付產物的過程。
不同工具支援的語言、套件管理系統與產物分析方式不同。來源清單、建置紀錄與交付產物各有可見範圍，沒有單一方式能在所有系統保證完整。
可提到本次 Laravel 範例使用 Composer 生態的工具；Syft、Trivy 等工具也有產製方式，操作細節放在延伸閱讀。
來源：CycloneDX 1.6 lifecycles 定義；資安院工具教材。
https://cyclonedx.org/docs/1.6/json/
https://material.nics.nat.gov.tw/material/maintainable/Guide_to_SBOM_and_OSV_Tools/
-->

---

# 清單涵蓋到哪裡？

- 直接、間接使用的第三方元件
- 開源與專有元件
- 前端、後端與交付產物的涵蓋範圍
- 建置期元件的標示，以及未納入部分的說明

本案交付範圍，應能和清單對照。

<!--
時間：34:00–35:30，共 1 分 30 秒。
專有元件也可能出現在交付物中，不能只盤點開源套件。
這裡依主辦提供的會後版 v2 草案 FAQ11 講解：交付或部署軟體的直接、間接第三方元件，包括開源與專有元件；只供建置且未隨交付物提供的工具可另行標示，實際使用但未納入的部分須說明原因及範圍。
作業系統、容器基底、外部服務等邊界也應對照契約與部署情境，不在課堂一概宣告所有項目均為本案必填。
資料來源：主辦提供之「臺北市政府發展與導入開源技術評估指引」（草案）會後版 v2，FAQ11。仍為草案狀態。
-->

---

# 有產製工具，還有哪些工作？

- 盤點舊系統與缺少紀錄的元件
- 補齊來源、版本與授權資訊
- 接進每次建置、交付的流程
- 系統更新後，持續產製與複核

**估工作量時，要一起看後續怎麼維持。**

<!--
時間：35:30–37:00，共 1 分 30 秒。
說明成本來源，不提供沒有依據的價格。能在既有建置流程產製的專案，和來源不明、長期未維護的系統，準備工作可能差很多。
廠商需要明確的交付範圍、頻率與補正方式才能估價。機關也需要接收、判讀及保存的工作安排。
這是工作拆解與估算考量，不是宣稱每案都會發生相同成本。
依據：資安院工具教材所列工具辨識差異；主辦提供之 v2 草案對維護與更新的要求。
https://material.nics.nat.gov.tw/material/maintainable/Guide_to_SBOM_and_OSV_Tools/
-->

---

# 指引草案列了哪些交付資訊？

依主辦提供的會後版 v2 草案

- 持續維護的國際標準機器可讀格式
- 版本資訊、產製工具與時間
- 另出具開源授權合規聲明

具體版本、欄位與程序，對照適用公告及本案契約。

<!--
時間：37:00–38:30，共 1 分 30 秒。
銜接上午已介紹的指引與檢核表，不重講完整政策背景。
文件仍標示草案，這頁如實呈現主辦提供的版本，不能說已是全市一體適用的正式命令。
「持續維護的格式」與「清單跟著軟體更新」是兩件事。前者涉及標準版本的維護與工具支援，後者在 FAQ10 及檢核表更新項目另有說明。
來源：主辦提供之「臺北市政府發展與導入開源技術評估指引」（草案）會後版 v2，正文伍、三（二），正文第 5 頁；FAQ10、FAQ11。
-->

---

# 「請提供 SBOM」，還差哪些約定？

| 約定項目 | 要說清楚的內容 |
| --- | --- |
| 交付範圍與時點 | 哪個版本、哪些內容、何時交付 |
| 交付資料 | 格式版本、欄位與對應證據 |
| 檢查與補正 | 誰確認、何時回覆、如何補正 |
| 後續更新 | 哪些變更要重交、由誰接收 |

<!--
時間：38:30–40:00，共 1 分 30 秒。
這張是協助形成採購需求的討論清單，實際文字仍應放入本案適用的採購文書與契約，依機關分工審查。
同時約定交付時點，例如測試或驗收所需版本，以及正式交付後的維護更新；時點依個案決定，不虛構統一的期限。
談可追溯性時，可以視本案需要要求版本對照、建置紀錄、交付產物雜湊等證據。不要把一個未約定的技術選項直接說成所有案件強制義務。
依據：主辦提供之 v2 草案，伍、三、檢核表及 FAQ10、FAQ11；本課程的工作拆解。
-->

---

# 問廠商時，可以這樣問

> 請說明這份 SBOM 對應的交付版本，
> 並提供可對照交付產物的資訊。

> 請補充產製時間、工具版本與涵蓋範圍。
> 未納入的部分，請說明原因及後續處理方式。

詢問時，一起附上**檔案、欄位與待確認事項**。

<!--
時間：40:00–42:00，共 2 分鐘。
以上是教學用的詢問範例，實際契約文字依個案處理。
以 Laravel 樣本為例，把 dev-master、in-dev、缺少 timestamp 這些具體觀察帶進問題，比只寫「請補齊合格 SBOM」更容易得到可核對的回覆。
如資料採不同版本命名，先請對方說明對照方式。若確實錯用舊版清單，再依約補正。
提醒要約定可執行的回覆與補正時點，並保留原始交付資料及後續版本。
-->

---

# 這個問題，由誰接著處理？

分工示意，依機關實際安排調整

| 角色 | 接續處理的工作 |
| --- | --- |
| 專案承辦 | 核對交付項目、整理疑問、追蹤回覆 |
| 廠商 | 說明產製方式、提供證據、補正與更新 |
| 資訊／資安人員 | 確認技術範圍、判讀影響與處置 |
| 採購／履約與主管 | 依契約辦理、協調責任與資源 |

<!--
時間：42:00–43:30，共 1 分 30 秒。
這是功能分工，不代表每個機關都有四個獨立單位。兼辦人員可能同時擔任多種角色。
帶去詢問的資料應包含檔案、交付版本、檢查結果與待確認事項，讓技術人員能接續判讀。
實際驗收權責依機關與契約辦理；這張不指定新的法定責任或假設已存在統一支援平台。
來源：資安署 FAQ 關於委外廠商評估的機關分工說明；本課程工作情境。
https://web.moda.gov.tw/ACS/laws/faq/06/642
-->

---

# 系統更新，清單也要跟著走

| 系統的變化 | 接續確認 |
| --- | --- |
| 修補、升版或替換元件 | 依約更新 SBOM |
| 收到新的清單 | 核對版本、範圍與檢查結果 |
| 移交維護工作 | 一併交接清單、紀錄與未結事項 |

**新舊版本都留得住，後續才查得回來。**

<!--
時間：43:30–45:00，共 1 分 30 秒。
回到開場的便民預約系統：交付 2.4 後又修補到 2.5，如果仍只有 2.4 的清單，遇到通知時就無法直接對照現在的系統。
清單更新時點、保留方式與接收人由契約及機關流程約定，不在這裡提出一體適用的天數。
新舊版本留存是追溯工作的建議，不能擴張為未經確認的法定保存年限。
依據：主辦提供之 v2 草案 FAQ10 與檢核表的軟體更新項目；課程工作建議。
-->

---

# 系統是 2.4，清單卻寫 2.3

假設交付情境

| 項目 | 收到的資訊 |
| --- | --- |
| 本次交付系統 | `2.4` |
| SBOM 內的主要軟體版本 | `2.3` |
| 格式檢查 | 通過 |

**你能確認什麼？還要請廠商提供什麼？**

<!--
時間：45:00–47:00，共 2 分鐘。第二次短互動。
先給 30 秒獨立思考，再請兩位學員簡短回答。追問一句：「這個問題接下來由誰確認、由誰追蹤？」
本題重點在交付對應，而不是背標準欄位。不要先把版本標示不同判成一律退件，也不要因格式通過就直接接受。
案例為教學假設，並非真實機關事件。
-->

---

# 先問清楚，版本怎麼對應

已知：這份檔案通過了格式檢查。

待確認：它是否描述本次交付的 `2.4`。

> 請提供版本對照與交付產物的對應證據。
> 若清單未更新，請依約補正。

承辦追蹤回覆，由適當人員確認技術內容。

<!--
時間：47:00–48:30，共 1 分 30 秒。
對照學員回答：若能區分已知與未知、提出具體證據要求、說出接續處理角色，就已達到這個案例的學習成果。
不同版本名稱可能來自產品、套件、建置或發布命名差異，因此要先確認對照。不預設有問題就停機，也不預設立即驗收通過。
正式履約與驗收處理依機關程序及本案契約辦理。
-->

---

# 回到你手上的案子

- 這份清單，對得上哪個交付版本？
- 它怎麼產生，包含了哪些範圍？
- 檢查結果回答了什麼，還缺哪些證據？
- 下一次更新時，誰提供、誰確認？

<!--
時間：48:30–50:00，共 1 分 30 秒。
請學員把開場想到的檢查事項，和現在這四個問題對照。
收束：「下次收到 SBOM，希望你能指出一個具體欄位、一個待確認的問題，並把事情交給能接著處理的人。」
課後的範例、格式規格與技術教材連結在附錄。若主辦提供已確認的支援窗口，可以口頭補充實際聯絡路徑。
-->

---

# Q&A

你手上的案子，卡在哪一步？

交付、判讀、補正，或後續更新？

[Denny Huang](https://denny.one/)

<!--
時間：50:00–60:00，共 10 分鐘。
優先處理與本課程工作情境有關的問題。實際案件可先確認系統與交付範圍，不請學員公開機關未核准揭露的資料。
涉及未定案的政策、特定案件驗收權責或法律結論時，清楚區分已確認規定、契約內容與需要主辦或權責單位確認的部分。
正文到此結束。後面為延伸閱讀與問答時可使用的附錄。
-->

---

# 附錄：公開案例與線上工具

- [主要案例：CycloneDX PHP Composer 的 Laravel 示範檔](https://github.com/CycloneDX/cyclonedx-php-composer/blob/defcbfed77054ae9659eb6092422456bb24c1114/demo/laravel-7.12.0/results/bom.1.6.json)
- [對照案例：Python 3.14.0 Windows x64 SBOM](https://www.python.org/ftp/python/3.14.0/python-3.14.0-amd64.exe.spdx.json)
- [較小的範例：SPDX 官方 Maven 示範檔](https://github.com/spdx/spdx-examples/blob/08a3552c7a5348f79ae996196a5c4f99e068607e/software/example8/spdx2.3/examplemaven-0.0.1.spdx.json)
- [線上檢查：sbomify SBOM Checker](https://sbomify.com/tools/sbom-checker/)

<!--
附錄，不計入正文授課時間。
主要案例與對照案例的完整原始 JSON 隨教材包提供，檔案來源與授權見 materials/SOURCE_NOTES.txt。
公開範例使用固定版本，供閱讀與比較；不表示其中的軟體版本適合新系統採用。
-->

---

# 附錄：想進一步了解產製方式

- [資安院：SBOM 開源工具使用說明](https://material.nics.nat.gov.tw/material/maintainable/Guide_to_SBOM_and_OSV_Tools/)
- [CycloneDX：工具與產製方式](https://cyclonedx.org/tool-center/)
- [SPDX：工具](https://spdx.dev/use/spdx-tools/)

先對照自己的語言、建置流程與交付產物，再選工具。

<!--
附錄，不計入正文授課時間。
工具操作與版本可能更新，使用前讀官方目前文件及安全公告。
評估工具要看辨識範圍、證據與流程整合。單看元件數較多，不能證明內容更準確或完整。
本課程的共同作業不要求安裝工具；有實作需求的資訊或廠商人員可由這些來源延伸。
-->

---

# 附錄：資訊人員可以再往下問

- SBOM 如何對應建置紀錄與交付產物？
- 元件識別資訊與相依關係是否可追溯？
- 無法辨識、排除與只在建置時使用的部分？
- 產製、檢查與更新能否接進既有流程？

<!--
附錄，不計入正文授課時間。
可以依個案討論 commit、建置編號、產物雜湊、Package URL、簽章或來源證明等證據。這些機制各自回答不同問題，不把其中任何一項宣稱為完整性或安全性的充分保證。
對已知漏洞的影響判斷，還要看實際版本、使用方式、可達性與處置證據，避免只憑元件名稱下結論。
依據：CycloneDX 規格與主辦提供之 v2 草案的交付範圍要求。
https://cyclonedx.org/docs/1.6/json/
-->

---

# 附錄：規格與指引來源

- [SPDX 官方規格](https://spdx.dev/use/specifications/)
- [CycloneDX 1.6 JSON Reference](https://cyclonedx.org/docs/1.6/json/)
- [SPDX 2.3 JSON Schema](https://raw.githubusercontent.com/spdx/spdx-spec/v2.3/schemas/spdx-schema.json)
- 主辦提供之《臺北市政府發展與導入開源技術評估指引》
  （草案）會後版 v2：伍、三、檢核表、FAQ10–12

<!--
附錄，不計入正文授課時間。
各頁的事實來源、範例位置與必要限制另列於講者備註。
公開樣本維持原始內容，第三方檔案的來源、授權與雜湊記錄於教材包 materials/SOURCE_NOTES.txt 及所附授權文件。
主辦草案的交付版本、欄位與適用程序，以後續確認的正式文件及本案契約為準。
-->
