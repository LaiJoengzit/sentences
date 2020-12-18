# 粵語對話語料

本倉庫用來蒐集粵語對話語料庫嘅原始數據，數據主要係粵語短句，以日常生活對話爲主，**本倉庫唔蒐集長篇粵文**。

短句語料喺 NLP 可以有好多種用途，包括但唔限於：

1. 詞性標註 POS tagging
1. 機械翻譯 Machine Translation
1. 情感分析 Sentiment Analysis
1. 命名實體識別 Name Entity Recognition
1. 傾偈機械人 Chatbot
1. 語音識別 ASR
1. 語音合成 TTS
1. 錯別字改正 Misspelling Correction

考慮到有咁多用途，蒐集句子嗰陣需要儘可能保留數據源嘅信息，包括但唔限於：

1. 粵語片區（使唔使區分到地點，例如廣府片區分廣州同香港）
1. 句子提供人性別、年齡段
1. 用於 Chatbot 嘅句子可能要記埋使用場景，而且要區分對話段落

喺蒐集數據嗰陣，要考慮以下問題：

1. 因爲語料有可能會用嚟做錯別字改正，所以之後可以專門分出一份錯別字語料，入面全部都係錯別字。格式另作討論，可參考[GitHub Typo Corpus](https://github.com/mhagiwara/github-typo-corpus)
1. 有錯別字就要有正字，我哋要確定一套正字法，而且要考慮埋唔同片區
1. 對於 TTS 訓練數據，需要一份比較規範性（prescriptive）、純正嘅語料，所以我哋需要確定一套專用於 TTS 嘅粵語標準語

## 數據格式

鑑於 POS tagging、MT、NER、ASR、TTS 等任務都唔需要對話段落信息，只需要區分粵語片區，所以數據儲存格式可以唔使考慮呢啲任務。我哋首先要保留一份原始數據，即數據源提供嘅，完全冇經過任何修改嘅版本，按照片區同段落分類。而呢個數據因爲係對話類型，所以應該用對話語料格式儲存，參考 [chatterbot-corpus](https://github.com/gunthercox/chatterbot-corpus)。

問題：
1. 如果對話語料要區分片區、話題、人口背景幾個信息，啲數據量可能會好雜亦都太少，係唔係應該合併一啲條件唔作區分？例如所有片區都可以放埋一嚿，唔理廣州香港。
2. 對於對話語料，使唔使修正用字？大部分原數據嘅語氣詞用字都係唔啱嘅，我哋需唔需要將所有語氣詞都按照我哋標準用字改過來？
1. 數據嘅 TN(Text Normalization)要做幾多，譬如 emoji、單詞大細寫、空格、特殊符號點樣處理。

## 工作流程

現階段以蒐集同清理數據爲主，原數據全部放喺 `raw` 路徑入面嘅 csv 文件度。唔同用途嘅數據另外開路徑存放。蒐集源數據嘅同時都可以爲唔同嘅任務做清理，例如整理成對話語料後放落 `dialogue` 路徑下面。

### Common Voice 句子清理規範

[How-to](https://commonvoice.mozilla.org/sentence-collector/#/how-to)

加入 Common Voice 嘅句子要排除版權問題，而且要做下面嘅規範化清洗：

1. 冇數字：句子入面唔可以有「2049」，要轉成「二零四九」或者「兩千零四十九」
2. 冇縮寫：一啲英文縮寫譬如「ICE」既可以讀成 /ai si i/ 又可以讀成 /ais/，會造成唔一致
3. 冇標點：標點符號同埋 emoji 都應該刪除
4. 外國文字：呢點需要另外討論，因爲香港粵語有大量英文詞，所以可能無法避免
5. 長度：mozilla 要求唔可以超過 14 個詞，呢個有待討論
6. 句子必須冇語病、冇錯別字而且可讀。