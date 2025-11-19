**C. 半專有／標準外（僅限你真的能改晶片/韌體）**

如果你**真的**能下到 Baseband/MAC 時序層面（常見於研究平台/私有模組），還能做更激進的事，但要注意**法規與互通性**：

- **縮短（甚至繞過）CSMA/CA 的感測/退避**、做 **Pseudo-TDMA**（例如用週期性 CTS-to-Self/Null 幀長時間設 NAV），把空口切片分配給己方鏈路。這會**破壞與旁站互通、且可能違反地方法規**（CCA/ED/DFS 都有硬性要求），需極度謹慎。[mrn-cciew](https://mrncciew.com/2014/10/26/cwap-802-11-ctrl-rtscts/?utm_source=chatgpt.com)[Cisco](https://www.cisco.com/c/en/us/support/docs/wireless-mobility/80211/213882-radar-detection-in-dynamic-frequency-sel.pdf?utm_source=chatgpt.com)
- **固定更嚴的 PHY 參數**（自定前導、交織深度、FEC、碼率），或**在 MAC 上關閉/改寫某些控制流程**（例如把 ACK/重傳做成自定批回饋），本質就不是通用 Wi-Fi 了──**效能可能最好，但等於專用協定**。

**實作藍圖（你可直接照這個原型跑）**

1. **頻譜/通道**：5GHz 非 DFS 固定通道、**20/40 MHz**；AP/STA 鎖在該通道，禁自動選台與 DFS。[Cisco](https://www.cisco.com/c/en/us/support/docs/wireless-mobility/80211/213882-radar-detection-in-dynamic-frequency-sel.html?utm_source=chatgpt.com)
2. **速率**：AP/STA **固定 MCS**（根據鏈路裕度抓一個穩態 MCS），啟 **Short GI**。[docs.silabs.com](https://docs.silabs.com/wifi91xrcp/latest/wifi91xrcp-developers-guide-wifi-configuration/802-11-ax-wifi-data-rate?utm_source=chatgpt.com)[community.fortinet.com](https://community.fortinet.com/t5/FortiAP/Technical-Tip-Short-Guard-Interval/ta-p/312867?utm_source=chatgpt.com)
3. **QoS/MAC**：串流封包標記到 **AC_VO/VI**；**AIFSN/CWmin/CWmax 下修**；視需要給 **TXOP**；**RetryLimit 下修**；**A-MPDU 上限縮小或關閉**；**（好環境）No-ACK**。[support.huawei.com+1](https://support.huawei.com/enterprise/en/doc/EDOC1100276722/ba03c3ed/configuring-wmm?utm_source=chatgpt.com)[support.mangocomm.com](https://support.mangocomm.com/docs/wlan-user-guide-v2/mac/low_dcf.html?utm_source=chatgpt.com)[CSDN](https://blog.csdn.net/m0_56952740/article/details/135134960?utm_source=chatgpt.com)
4. **11ax（若可）**：AP 以 **Trigger-based UL OFDMA** 給相機固定 RU/週期，讓 uplink 沒有爭用。[Cisco Blogs](https://blogs.cisco.com/networking/wi-fi-6-ofdma-resource-unit-ru-allocations-and-mappings?utm_source=chatgpt.com)[Airheads Community](https://community.arubanetworks.com/blogs/antar1/2020/10/19/why-is-ofdma-a-magical-feature-in-80211ax-standard?utm_source=chatgpt.com)
5. **保護**：多機共場時開 **CTS-to-Self/RTS/CTS**（或靠 OFDMA 分 RU），避免彼此踩空口。[CSDN](https://blog.csdn.net/tiantian_xiao/article/details/134422816?utm_source=chatgpt.com)

**你可能想知道的限制**

- **真正零爭用/零抖動**，在標準 Wi-Fi 的 DCF/EDCA 模式下**做不到**；要麼靠 **11ax 的排程 uplink**，要麼走**專有鏈路**。[Cisco](https://www.cisco.com/c/en/us/td/docs/wireless/controller/9800/17-9/config-guide/b_wl_17_9_cg/m_ofdma_11ax.html?utm_source=chatgpt.com)
- **DFS 通道**可能觸發 **CAC/跳頻與停傳**，任何低延遲系統都應避免

**C. 半專有／標準外（僅限你真的能改晶片/韌體）**

如果你**真的**能下到 Baseband/MAC 時序層面（常見於研究平台/私有模組），還能做更激進的事，但要注意**法規與互通性**：

- **縮短（甚至繞過）CSMA/CA 的感測/退避**、做 **Pseudo-TDMA**（例如用週期性 CTS-to-Self/Null 幀長時間設 NAV），把空口切片分配給己方鏈路。這會**破壞與旁站互通、且可能違反地方法規**（CCA/ED/DFS 都有硬性要求），需極度謹慎。[mrn-cciew](https://mrncciew.com/2014/10/26/cwap-802-11-ctrl-rtscts/?utm_source=chatgpt.com)[Cisco](https://www.cisco.com/c/en/us/support/docs/wireless-mobility/80211/213882-radar-detection-in-dynamic-frequency-sel.pdf?utm_source=chatgpt.com)
- **固定更嚴的 PHY 參數**（自定前導、交織深度、FEC、碼率），或**在 MAC 上關閉/改寫某些控制流程**（例如把 ACK/重傳做成自定批回饋），本質就不是通用 Wi-Fi 了──**效能可能最好，但等於專用協定**。

**實作藍圖（你可直接照這個原型跑）**

1. **頻譜/通道**：5GHz 非 DFS 固定通道、**20/40 MHz**；AP/STA 鎖在該通道，禁自動選台與 DFS。[Cisco](https://www.cisco.com/c/en/us/support/docs/wireless-mobility/80211/213882-radar-detection-in-dynamic-frequency-sel.html?utm_source=chatgpt.com)
2. **速率**：AP/STA **固定 MCS**（根據鏈路裕度抓一個穩態 MCS），啟 **Short GI**。[docs.silabs.com](https://docs.silabs.com/wifi91xrcp/latest/wifi91xrcp-developers-guide-wifi-configuration/802-11-ax-wifi-data-rate?utm_source=chatgpt.com)[community.fortinet.com](https://community.fortinet.com/t5/FortiAP/Technical-Tip-Short-Guard-Interval/ta-p/312867?utm_source=chatgpt.com)
3. **QoS/MAC**：串流封包標記到 **AC_VO/VI**；**AIFSN/CWmin/CWmax 下修**；視需要給 **TXOP**；**RetryLimit 下修**；**A-MPDU 上限縮小或關閉**；**（好環境）No-ACK**。[support.huawei.com+1](https://support.huawei.com/enterprise/en/doc/EDOC1100276722/ba03c3ed/configuring-wmm?utm_source=chatgpt.com)[support.mangocomm.com](https://support.mangocomm.com/docs/wlan-user-guide-v2/mac/low_dcf.html?utm_source=chatgpt.com)[CSDN](https://blog.csdn.net/m0_56952740/article/details/135134960?utm_source=chatgpt.com)
4. **11ax（若可）**：AP 以 **Trigger-based UL OFDMA** 給相機固定 RU/週期，讓 uplink 沒有爭用。[Cisco Blogs](https://blogs.cisco.com/networking/wi-fi-6-ofdma-resource-unit-ru-allocations-and-mappings?utm_source=chatgpt.com)[Airheads Community](https://community.arubanetworks.com/blogs/antar1/2020/10/19/why-is-ofdma-a-magical-feature-in-80211ax-standard?utm_source=chatgpt.com)
5. **保護**：多機共場時開 **CTS-to-Self/RTS/CTS**（或靠 OFDMA 分 RU），避免彼此踩空口。[CSDN](https://blog.csdn.net/tiantian_xiao/article/details/134422816?utm_source=chatgpt.com)

**你可能想知道的限制**

- **真正零爭用/零抖動**，在標準 Wi-Fi 的 DCF/EDCA 模式下**做不到**；要麼靠 **11ax 的排程 uplink**，要麼走**專有鏈路**。[Cisco](https://www.cisco.com/c/en/us/td/docs/wireless/controller/9800/17-9/config-guide/b_wl_17_9_cg/m_ofdma_11ax.html?utm_source=chatgpt.com)
- **DFS 通道**可能觸發 **CAC/跳頻與停傳**，任何低延遲系統都應避免
  
**可落地的參數清單**（EDCA、TXOP、MCS、聚合上限、Retry、OFDMA RU 配置）

# PHY（Baseband）層：怎麼把符號時間壓短、啟動快、穩定收

1. **調變/波形**
    
    - **OFDM/SC-OFDM 皆可**。若在 5.8 GHz 空曠場域，建議 OFDM 搭**高副載波間距**（像 60/120 kHz 級）→ **符號更短、前導更短、鎖定更快**。
        
    - **CP（保護區）**依場景實測設計（室外 LOS 常可 ≤0.8 µs）。CP 不要過長，省掉無謂開銷。
        
    - **PAPR/PA 線性**：若受限於小型 PA，選 **π/4-QPSK 或低階 QAM**；追穩定而非高 MCS。
        
2. **前導/同步**
    
    - **超短前導**：2–3 個 OFDM symbol 完成定時/頻偏/信道估測（長訓練另放在入網時或低頻率穿插）。
        
    - **梳狀導頻 + 決策導向（decision-directed）追蹤**，減少每包必須的導頻量。
        
    - **頻偏容忍**：設計較寬的捕獲範圍（coarse CFO）＋快速精校，避免長時間鎖頻。
        
3. **編碼/交織**
    
    - **短區塊 FEC**：**短幀 LDPC / Polar / TBCC**（例如 256–1024 bit）＋**低迭代數**解碼；避免長交織。
        
    - **碼率選擇**：寧可 **低碼率（更多冗餘）**，換取**一次成功**，比依賴回傳重傳更穩、更低尾延遲。
        
    - **交織**：只做**頻域輕交織**（跨少量子載波），**禁止長時域交織**。
        
4. **分集/多天線**
    
    - **接收端 MRC**（多天線合併）或 **Alamouti STBC**（2Tx-1Rx）提升一次成功率；延遲增加極小、值回票價。
        
    - **頻率/空間冗餘**：允許**同包在鄰近子頻重發一次（packet replication）**，接收端做選擇合併/軟合併。
      
      
    # MAC 層：從「爭用」改成「排程」

1. **時分多工（TDMA/TDD）**
    
    - **固定長度時隙**：例如超幀 5 ms；其中 **上行影像 4.2 ms → 下行控制/回饋 0.6 ms → 保留 0.2 ms**。
        
    - **單播保留**：每台空端都拿到**固定週期、固定大小**的 uplink 配額（像 1–2 個 RU 的概念），**零爭用**。
        
2. **去 ACK / 去 ARQ**
    
    - **無傳統 per-packet ACK**；改成**週期性摘要回饋**（例如每 N 幀回報 CRC 統計/鏈路質量）。
        
    - 若必須回復資料，採 **物理層 HARQ-like**（同一時隙內 **Chase 合併 / 增量冗餘**），**禁止高層 ARQ** 造成排隊。
        
3. **固定化一切可變因素**
    
    - **固定 MCS、固定發射功率**（根據最壞路徑設計 3–6 dB 裕度）；**關閉自適應碼率**避免抖動。
        
    - **固定封包長度**（例如每包 0.2–0.5 ms 空口時間），**固定節拍送出**，便於端到端管線化。
        
4. **極輕量控制面**
    
    - 下行控制時隙攜帶 **時鐘校準、功率指示、頻偏補償、編碼檔位切換令牌**（僅在必要時變更）。
        
    - **快切頻**（需要時）：設計**幀邊界切換表**，在保留間隙內一拍完成重設 LO／PLL（避免類 Wi-Fi 的掃描/DFS 開銷）。
      
      
    # 頻譜與 RF 策略

- **固定非 DFS 通道**（例如 5.725–5.850 GHz 區間內可用頻點），法規允許下**鎖定中心頻率與 10/20/40 MHz 帶寬**。
    
- **通道規劃**：多機共場時以 **20 MHz 隔離**切割，每機固頻；或做**兩路頻率冗餘**（主/備同送，接收端選擇合併）。
    
- **EIRP/濾波器**：小型機載 PA 線性是延遲穩定性的「暗樑」；寧可保守功率 + 好濾波 + 好天線，換更少重傳。
  
  # 參考數值（方便你抓量級）

- **帶寬**：20 MHz OFDM、Δf=120 kHz → 符號 ≈ 8.3 µs（含短 CP）；一個「資料包」用 20 個符號 ≈ **0.17 ms**。
    
- **前導**：2 個符號 ≈ **16–20 µs**；每包全空口時間 ≈ **0.19–0.22 ms**（未計 FEC 冗餘）。
    
- **FEC**：短區塊 LDPC，R≈1/2；BLER 設計在目標 SNR 下 **<10⁻⁵**，務求「一次到」。
    
- **超幀**：例：**5 ms 週期**；影像上行 4.2 ms ≈ 可塞 **~20–24 包**；控制 0.6 ms；保留 0.2 ms。
    
- 以上僅為**量級**，實際需依硬體時脈與 PA、頻率規畫調整。
  
  # 快速起跑的最小可行組合（MVP）

- **20 MHz OFDM、Δf≈120 kHz、CP≈0.8 µs**
    
- **QPSK + R=1/2 LDPC（短區塊，≤512 bit）**
    
- **前導 2 symbol、導頻梳狀**
    
- **TDD 超幀 5 ms：UL 4.2 / DL 0.6 / Gap 0.2**
    
- **無 ACK / 無 ARQ、1× 包複本（可選）**
    
- **Rx 2 天線 MRC；Tx 1 天線**
    
- **固定頻點、固定功率、固定封包長度**
    

這套在實務上就能把**空口內延遲**壓到**亞毫秒級（每包）**、**超幀級 5–10 ms 程度的端端穩定延遲**；更進一步可藉由縮短超幀/提升時鐘來再降。

---

如果你告訴我**預計頻段、法規限制（EIRP/DFS）、目標距離/遮擋、天線與功耗、是否要多機共場**，我直接幫你把**參數表（數字級）**開出來，包含：副載波間距、FFT 長度、CP、前導設計、FEC 區塊長/碼率、時隙配置、包長、是否複本、MRC/STBC 選擇與**可達延遲/BLER 預估**。