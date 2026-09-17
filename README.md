# 6G 雙軌語意通訊系統 (Dual-Track Semantic Communication System for 6G IoT)

本專案為 6G 物聯網環境下基於大型語言模型 (LLM) 之端到端雙軌語意通訊系統。透過結合「全域滑動視窗 (Global Sliding Window, GSW)」與「語意解碼器 (Semantic Decoder)」，系統能在極端惡劣的通訊通道 (AWGN BPSK, Eb/N0 = 1.0 ~ 5.0 dB) 中，實現高精準度的感測器數值修復與特徵對齊。

## 系統亮點 (Key Features)
* **雙軌解碼架構 (Dual-Track Decoding)**：以 `Threshold = 0.70` 作為最佳門檻值，完美平衡邊緣算力消耗與解碼精準度。
* **抗極端雜訊 (Extreme Noise Robustness)**：在 Eb/N0 = 3.0 dB 時，相對誤差控制在0.09以下，JSON 格式輸出成功率維持 100%，感測器命中率達 96.3%，有效抑制推論幻覺。
* **跨通道泛化能力 (Zero-Shot Generalization)**：訓練集採用 BSC 通道，測試集採 BPSK+AWGN 通道，證明模型無須重新訓練即可適應異質通道。

## 儲存庫內容 (Repository Contents)
本專案依據實驗與系統資料流，將程式碼模組化為以下三個核心階段：
* `00_Instruction_Dataset_Generation.ipynb`：SFT 訓練資料集生成，負責將原始感測器數值轉換為 LLM 專用之 Alpaca JSON 指令格式。
* `01_SFT_Model_Training.ipynb`：模型微調訓練程式碼，包含資料預處理與 LoRA 權重更新。
* `02_E2E_Semantic_Communication_Pipeline.ipynb`：端到端通訊測試管線，涵蓋發射端特徵轉換、AWGN 通道雜訊模擬、接收端 GSW/LLM 雙軌解碼，以及最終的效能視覺化圖表生成。
* `6G_Semantic_Communication_Report.pdf`：專題完整成果報告，包含系統架構設計與詳盡之消融實驗分析。

## 模型權重 (Model Weights)
本系統使用之核心語意解碼器已透過 LoRA 進行微調並量化匯出為 `.gguf` 格式，以利於邊緣設備推論。
* **模型下載連結**：[點擊此處下載 Semantic-Decoder.gguf](<https://drive.google.com/file/d/1l8PohzopGh7uBCrxcNcpG21UQjfaeauI/view?usp=drive_link>)

## 執行環境 (Requirements)
* Python 3.8+
* [Ollama](https://ollama.com/) (需於本地端背景執行 `ollama serve`)
* 套件：`pandas`, `numpy`, `matplotlib`, `transformers`, `datasets`
