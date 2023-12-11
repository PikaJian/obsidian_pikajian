# Le IoT 想想物聯網: Build Nexus Player BLE Remote Controller (2-2) - 了解 HID over GATT - HOGP - 副本 1

**擷取檔來源:** [Le IoT 想想物聯網: Build Nexus Player BLE Remote Controller (2-2) - 了解 HID over GATT - HOGP](https://thinkingiot.blogspot.com/2015/03/build-nexus-player-ble-remote_4.html)

## 2015年3月4日 星期三

### Build Nexus Player BLE Remote Controller (2-2) - 了解 HID over GATT - HOGP

BLE 使用 GATT, 但為了讓 HID 也可以透過 BLE 來達到, 所以就有了 HID Over GATT Profile (HOGP) 的 SPEC, 目標就是希望 BLE 也可以做到 HID Service, 另外還有 HID Service (HIDS) 說明如何建立一個 HID Service.

相關介紹以及 SPEC 連結請看:
<https://developer.bluetooth.org/TechnologyOverview/Pages/HOGP.aspx>
<https://developer.bluetooth.org/TechnologyOverview/Pages/HIDS.aspx>

本篇使用的圖片都來自於上述的 SPEC.
(All figures in this article are captured from the SPEC listed above. If there is a violation of copyright. Please tell me and I i will remove it)

\==========================================================

**\[HOGP\]**

Note: 其實整理到最後跟 SPEC 也幾乎沒差, 建議直接看 SPEC, 我就當作寫讀書筆記而已, 老實說真的沒什麼重點, 因為他就是 SPEC 是規範, 每一句話都不能放過...

HOGP 是 HID OVER GATT PROFILE SPECIFICATION 的簡寫, 主要說明 HID 如何透過 BLE GATT 的運作方式, 主要定說明相關的 Procedures 以及 Features, 這份 SPEC 是繼承 HID Profile SPEC (所以有些細節要去那邊翻), 但主要是在於 BLE HID 的部分.

Profile roles 如下圖所示:

* (shall) HID Device => 必須是 GATT Server
* (shall) Boot Host => 必須是 GATT Client
* (shall) Report Host => 必須是 GATT Client

![[hogp1.png]]

![[hogp2.png]]

那有一些 Concurrency 限制如下:

* Boot Host 不能同時是 Report Host
* Report Host 不能同時是 Boot Host
* 但 HID Host role 可以同時是 HID Device role

其他限制:

* (shall) HID Device => 必須是 GAP Peripheral role
* (shall) Boot Host => 必須是 GAP Central role
* (shall) Report Host => 必須是 GAP Central role

另外關於多個 instance of services 的部分 (HID Device), Battery service 以及 HID service 可以有多個, 而 Device Info service 和 Scan Parameter service 只可以有一個, 其他無關 HID 的 service 則沒有限制.

**\[HID Device 需求\]**

如下表所示, 必須要有下表列的三個必要的 Services (M), 以及一個 optional 的 Scan Parameter Service.

這些 Service 其實都各有 SPEC 說明 (建議要看一下):

\[4\] HID Service v1.0
\[5\] Battery Service v1.0
\[6\] Device Information Service v1.0
\[7\] Scan Parameters Profile v1.0

然後這份 HOGP 只是針對上面沒寫到的, GATT 需要的補充而已

![[hogp3.png]]

首先是 HID Service 部分:

* (shall) 任何 non HID Service 假如有某個 characteristic value 是在 Report Map characteristic 描述的, 那這 service 就必須被有此 Report Map characteristic 的 HID Service 給 include 到
* (shall) Report Map characteristic 必須包含 Report Reference characteristic descriptor
* (shall) HID Service 不能夠 include 已經被其他 HID Service include 的 service, 避免有衝突的情況
* (shall) HID Service 必須是 Primary Service
* (should) 建議必須包含 UUID AD Types 放 HID Service UUIDs
* (should) 建議必須包含 local name AD type
* (should) 建議必須包含 Appearance AD Type
* 另外就看有沒有需要 Additional Security Requirements

再來是 Battery Service 的部分:

* (shall) 至少要有一個 battery service UUID, 且是 Primary Service
* 另外就看有沒有需要 Additional Security Requirements

再來是 Device Information Service

* (shall) 必須是 Primary Service
* (shall) 必須包含以下幾個 Mandatory Characteristics
	* PnP ID characteristic, 用來讀取 PnP ID 用
* 另外就看有沒有需要 Additional Security Requirements

再來是 Scan Parameter Service

* 另外就看有沒有需要 Additional Security Requirements

**\[HID Host 需求 & Procedures\]**

HID Host 功能就是 observing, connecting to, configuring for notification from, reading from, and writing to, a HID Device.

如下表所示為 Boot Host / Report Host 的需求

![[hogp4.png]]

下表則是其他 GATT Client 會需要的 sub-procedures

![[hogp5.png]]

Scan Parameters Profile Support

* (shall) Report Host 必須支援 Scan Parameter Profile 的功能

Service Discovery - Boot Host

* (shall) 要支援 primary service discovery
* (shall) 可以找到所有的 HID Service
* (may) 可以找到 Device Info Service
* (may) 可以找到 Battery Service

Characteristic Discovery – Boot Host

* (may) 可以做 HID Service Characteristic Discovery if support
* (shall) 要能夠支援用 GATT Read Using Characteristic UUID 來做 boot mode operation 假如不支援 HID Service Characteristic Discovery, 需要可以讀的 characteristic 如 Protocol Mode characteristic, Boot Keyboard Input Report characteristic, Boot Keyboard Output Report characteristic, Boot Mouse Input Report characteristic
	* (may) 可以 discover Protocol Mode Characteristic
	* (may) 可以 discover Boot Keyboard Input Report characteristic
	* (shall) 可以 discover 所有 Boot Keyboard Input Report characteristics 的 Client Characteristic Configuration Descriptor
	* (may) 可以 discover Boot Keyboard Output Report Characteristic
	* (may) 可以 discover Boot Mouse Input Report Characteristic
	* (shall) 可以 discover 所有 Boot Mouse Input Report Characteristic 的 Client Characteristic Configuration Descriptor
* (may) 可以做 Device Information Service Characteristic discovery if support
* (shall) 要能夠支援用 GATT Read Using Characteristic UUID 來讀 PnP ID characteristic 假如不支援  Device Information Service Characteristic discovery
	* (may) 可以 discover PnP ID characteristic
* (may) 可以做 Battery Service Characteristic Discovery if support
* (shall) 要能夠支援用 GATT Read Using Characteristic UUID 來讀 Battery Level characteristic 假如不支援  Battery Service Characteristic Discovery
	* (may) 可以 discover Battery Level Characteristic

Service Discovery – Report Host

* (shall) 要支援 primary service discovery
* (shall) 假如 Report Host 支援的 ATT\_MTU 大於預設的 ATT\_MTU, 在 service discovery 前必須先做 GATT Exchange MTU sub-procedure
* (shall) 可以找到所有的 HID Service
* (shall) 可以找到 Device Info Service
* (may) 可以找到 Battery Service
* (shall) 必須能夠透過 HID Service Report Map characteristic value 找到包含的 Battery Service

Characteristic Discovery – Report Host

* (shall) 支援 HID Service Characteristic Discovery
	* (shall) 支援用 GATT Discover All Characteristic Descriptors 找到 characteristic descriptors
	* (shall) 可以 discover Report Map characteristics
		* (shall) 可以 discover External Report Reference characteristic descriptors (屬於 ReportMap characteristic 的)
	* (shall) 可以 discover Report characteristics
		* (shall) 可以 discover Client Characteristic Configuration Descriptor (屬於 Report characteristic 的)
		* (shall) 可以 discover Report Reference characteristic descriptor (屬於 Report characteristic 的)
	* (shall) 假如 Report Host 支援 Suspend Mode, 可以 discover HID Control Point Characteristic
		* Report Host 在 low power mode / suspend mode 時, 可以用他來送 control commands 給 HID Devices
	* (shall) 可以 discover HID Information Characteristic
	* (may) 可以 discover Protocol Mode characteristic
* (shall) 支援 Device Information Service Characteristic Discovery
	* (shall) 可以 discover PnP ID Characteristic
* (shall) 支援 Battery Service Characteristic Discovery
	* (shall) 可以 discover Battery Level characteristics, 會去 discover 所有的 battery level characteristics, 並找到可以參照 Report Reference characteristic descriptor 和 External Report Reference characteristic descriptor 的 battery level characteristic

**\[Host Behaviors\]**

Report Map Behavior

* 讀的時候會讀到 HID Report Descriptor, 關於 Report Descriptor 必須參考 USB HID 的 SPEC 有詳述
* (shall) Report Host 必須把整個 Report Map 都讀出來, Report Host 可以利用此資訊跟其他 service 的 characteristic 做對應, 就可以知道 Host 和 Device 之間傳輸的資訊是什麼

Report Behavior

* Report characteristic 用來交換 Host 和 Device 之間的 HID Service
* (shall) Report Host 必須把所有的 Report Type 為 Input Report 的 Report characteristic 利用 Client Characteristic Configuration descriptor 開啟 Notifications
* 補充 Report Type 是定義在 Report Reference characteristic descriptor
* (shall) Boot Host 必須忽略 Report characteristic 的 notification

(補充) Translation Layer

HOGP 的目標就是讓 USB-IF HID 資料鈄過 Bluetooth air interface (GATT) 傳輸, 所以 Report Host 其實就必須要在 USB class driver 和 GATT layer 之間做 translation.

之前提到 Report Host 會做 service discovery, characteristic discovery and characteristic descriptor discovery. Report Map characteristic 裡面的 Report ID 以及 Report Type, 以及 Report Reference characteristic descriptors 的資料可以讓 Report Host 讓 GATT characteristic 的資料進出 USB HID class driver, 以及讓 USB HID class driver 的資料進出 GATT characteristic.

Report Map 內的 Report ID 和 Report Type 的組合會是以下其中一種:

* A HID Service Report characteristic and Report Reference characteristic descriptor
* An external service characteristic, 其 UUID 來源是一個 External Report Reference characteristic descriptor. 所有的 External Report Reference characteristic descriptors 會有一個 unique 的值

當資料由 HID device 送給 Report Host, Report ID 會被加在 Report Host 收到的資料上, 然後才丟給 USB HID class driver.

反過來當 Report Host 送資料給 HID device, Report ID 會被 Report Host 移除後才送給 HID Device.

HID Control Point Behavior

* Report Host 利用 HID Control Point characteristic 告知 HID Device, Report Host 進入省電模式 (suspend mode)

HID Information Behavior

* HID Information characteristic 會包含 bcdHID 和 bcountryCode 欄位 (USB HID 定義的), 簡單來說就是 HID 版本以及國碼
* (shall) 當 Host 進入 suspend mode, RemoteWake flag 必須被用來判斷 Report Host 是否可以被此 HID device 喚醒
* 假如 RemoteWake flag 為 FLASE 表示此 HID device 不是 remote wakeup-capable, 所以 Report Host 也不會此 device 移出可以喚醒 report host 的 devices 集合 

Protocol Mode Behavior

* Protocol Mode characteristic 可以被讀寫, 用來讀出或設定 HID service 的 protocol mode
* (shall) Boot Host 必須在連線建立後, 寫入 GATT Server 中所有的 HID Service 的 Protocol Mode characteristic, 值為 Boot Protocol Mode
* Report Host 不用使用此 characteristic

Boot Keyboard Input Report Behavior

* 當 HID device 在 Boot Protocol Mode 當成一個 keyboard 的時候, 就用 Boot Keyboard Input Report characteristic 來和 Boot Host 交換 HID service 的資料
* (shall) 如果 Boot Host 支援 Boot Keyboard Input Report characteristic, 那就得用 Client Characteristic Configuration descriptor 開啟此 characteristic notification
* (shall) Report Host 要忽略 Boot Keyboard Input Report characteristic 的 notification

Boot Keyboard Output Report Behavior

* 當 HID device 在 Boot Protocol Mode 當成一個 keyboard 的時候, 就用 Boot Keyboard Output Report characteristic 來和 Boot Host 交換 HID service  資料 (表示 LED 的狀態)

Boot Mouse Input Report Behavior

* 當 HID device 在 Boot Protocol Mode 當成一個 mouse 的時候, 就用 Boot Mouse Input Report characteristic 來和 Boot Host 交換 HID service 的資料
* (shall) 如果 Boot Host 支援 Boot Mouse Input Report characteristic, 那就得用 Client Characteristic Configuration descriptor 開啟此 characteristic notification
* (shall) Report Host 要忽略 Boot Mouse Input Report characteristic 的 notification

Battery Level Behavior

* Battery Level characteristic 可以直皆被 Host 讀取
* 或是用 Host 用 Client Characteristic Configuration descriptor 開啟此 characteristic notification
* (should) 為了避免耗電, Host 必須盡量降低讀取此 Battery Level characteristic 的次數

PnP ID Behavior

* (shall) Report Host 必須在連線建立時去讀 PnP ID characteristic 並存著之後用
* (may) Boot Host 可以在連線建立時去讀 PnP ID characteristic 並存著之後用
* HID Host 利用 PnP ID 可以去載入對應的 Icon 或是軟體 (PnP ID 可區分到 per-device basis)

**\[Host MISC\]**

Information Sharing between HID Hosts

* (shall) Boot Host 和 Report Host 可以互相分享 bonding 的資訊, 且假如有任一方的 bonding 被刪除, 另一方也要刪除
* (shall) Service changed indication 是由 Report Host 收到時, 需要知會 Boot Host
* (shall) Service changed indication 是由 Boot Host 收到時, 需要知會 Report Host

**\[Connection Establishment / Termination\]**

HID Device Requirements

* Device Discovery
	* (should) 要使用 GAP Limited Discoverable Mode
	* (shall) TGAP(lim\_adv\_timeout) 必須 <= 180 秒
* Connection Procedure for Non-bonded Devices
	* 建議值可以參考下圖 Table 5.1 所示
	* (should) 這些設定要把使用者經驗納入考量
	* (shall) HID Host 設定的 valid connection interval, HID Device 都要接受
	* (should) 當 service discovery 以及 encryption 結束, HID Device 可以用 L2CAP Connection Parameter Update Request 來改 connection parameters
	* (shall) 這過程中 HID Device 要待在 bondable mode
	* (should) 若 Bond 成功, HID Device 要把 HID Host address 寫到 white list 內, 並設定 只有在 White List 內的 device 才可以做 scan & connection request
	* (should) 當連線建立, HID Device 必須等待到 idle connection timeout
	* (should) 若 Client Characteristic Configuration 有設定 Notification 但 Device 沒資料要傳, Device 要等待 idle connection timeout, 讓 Host 有時間再完成設定後結束連線
	* (should) 若 Client Characteristic Configuration 有設定 Notification 但 Device 有資料要傳, 傳完隻料後, 等待 idle connection timeout 後必須做 GAP Terminate Connection procedure
* Device-Initiated Connection Procedure for Bonded Devices
	* (may) 當 Bond 結束後, 若 user 命令或者是有 notification pending, 則 HID Device 可以發起 connection procedure
	* (shall) HID device 必須進入 GAP Undirected Connectable Mode or Directed Connectable Mode
	* (should) HID device 要使用之前設定的 advertising filter policy
	* (should) HID device 要使用建議的 advertising interval (下圖 Table 5.2 所示), 先用 option 1 做 fast connection, 若 connection 沒建立成功, 再用 option 2 以達到省電效果
	* (should) 這些設定要把使用者經驗納入考量
	* (shall) HID Host 設定的 valid connection interval, HID Device 都要接受
	* (shall) 若 connection 建立建立時有 notification pending, 那 HID Device 就得送一到多個 notification 給 HID Host
	* (should) 若 Client Characteristic Configuration 有設定 Notification 但 Device 沒資料要傳, Device 要等待 idle connection timeout, 讓 Host 有時間再完成設定後結束連線
	* (should) 若 Client Characteristic Configuration 有設定 Notification 但 Device 有資料要傳, 傳完隻料後, 等待 idle connection timeout 後必須做 GAP Terminate Connection procedure
* Host-Initiated Connection Procedure for Bonded Devices
	* (may) 當 user 有需要或是有資料 (例如 LED 資訊) 要傳給 HID Device 的時候會由 Host 發起連線建立
	* (shall) HID Device 若希望可以接受 HID Report Hosts 來建立連線, 必須把 HID Information characteristic 內的 NormallyConnectable flag 設成 TRUE
	* (shall) 至少和一個 Report Hosts bonding 但還沒跟任何 Report Host 連線的 HID Device, 必須在 GAP Undirected Connectable Mode
	* (should) HID device 要使用之前設定的 advertising filter policy
	* (should) HID device 要使用建議的 advertising interval (下圖 Table 5.3 所示)
	* (should) 這些設定要把使用者經驗納入考量
	* (shall) HID Host 設定的 valid connection interval, HID Device 都要接受
	* (shall) 若 connection 建立建立時有 notification pending, 那 HID Device 就得送一到多個 notification 給 HID Host
	* (should) 若 Client Characteristic Configuration 有設定 Notification 但 Device 沒資料要傳, Device 要等待 idle connection timeout, 讓 Host 有時間再完成設定後結束連線
	* (should) 若 Client Characteristic Configuration 有設定 Notification 但 Device 有資料要傳, 傳完隻料後, 等待 idle connection timeout 後必須做 GAP Terminate Connection procedure
* Link Loss Reconnection Procedure
	* (should) 若因為 link loss 斷線, HID device 要進入 GAP connectable mode 並嘗試跟 Host 連線, advertising interval 建議用 table 5.2 的設定
	* (may) HID device 也可以等到有資料要送或是有 user 的命令才做
* Idle Connection
	* (may) 當 connection idle 一段時間, HID device 可以做 GAP Terminate Connection procedure
	* (shall) 如果 HID device 支援 Scan Parameters Service 那 HID Host 必須遵造 Scan Parameters Profile 的定義, 把預計的 scanning 行為寫到 Scan Interval Window characteristic 並且 Host 不自己斷線, 等待 Device 斷線
	* (may) HID device 可以利用 Scan Interval Window characteristic 這個 scan parameter, 來決定是否要跟 Host 斷線或是保持連線

![[hogp8.png]]

![[hogp6.png]]

![[hogp7.png]]

Host Requirements

* Device Discovery
	* (should) Host 使用 GAP Limited Discovery Procedure 去 discover HID device
	* (may) 藉由 HID Device 的 UUID AD Type 得知支援的 HID Service 顯示給 user 知道
	* (may) 藉由 Local Name AD Type 顯示名字給 user
	* (may) 藉由 Appearance AD Type 可以顯示不同的 icon (例如鍵盤/滑鼠就是不同 icon)
* Connection Procedure for Non-bonded Devices
	* (may) Host 可能會使用以下 4 種的其中 1 種 procedure 來連線
		* General Connection Establishment Procedure
			* Host 要跟一個到多個 HID Devices 建利連線
			* 會用在還沒有 white list, 由 scan 到要建連線的時候使用
		* Direct Connection Establishment Procedure
			* 跟單一 HID Device 建連線時使用
		* Auto Connection Establishment Procedure
			* Host 用來跟一到多個 HID Device 建立連線
			* HID Devices 必須在 host 的 white list 內
		* Selective Connection Establishment Procedure
			* Host 用來跟一到多個 HID Device 建立連線
			* 會用在有 white list, 由 scan 到要建連線的時候使用, 會使用到 white list
	* (should) Host 使用下圖 5.4 的建議的 scan interval / scan window
	* (shall) 在 connection procedure 過程中, Host 要 bond device, 加速之後的 reconnect
	* (should) 當 bond 成功, Host 要把 device address 加到 white list, 並設定 initiator filter 處理 connectable advertisement packets
	* (should) 若 Client Characteristic Configuration 有設定 Notification, HID Host 要等待 idle connection timeout 才結束連線, 以免 device 有 pending notification 還沒送達
	* (may) 一般來說 device 在資料交換完就會關閉連線, 但也可能會等 idle connection timeout 讓 host 處理完設定後由 host 關閉連線
* Device-Initiated Connection Procedure for Bonded Devices
	* (may) 當 Bond 結束後, 若 user 命令或者是有 notification pending, 則 HID Device 可以發起 connection procedure
	* (may) Host 可能會使用上面 (Connection Procedure for Non-bonded Devices) 提到的 4 種的其中 1 種 connection procedure 來連線
	* (should) HID host 要使用圖 5.5 的 scan interval / scan window
	* (should) Host 要使用合適的 scan interval / scan window 設定達到預期的耗電以及建立連線時間的需求 (彼此互相影響)
	* (should) scan interval / scan window 設定要符合使用者對連線建立時間的預期
	* (should) 若 Client Characteristic Configuration 有設定 Notification, HID Host 要等待 idle connection timeout 才結束連線, 以免 device 有 pending notification 還沒送達
	* (may) 一般來說 device 在資料交換完就會關閉連線, 但也可能會等 idle connection timeout 讓 host 處理完設定後由 host 關閉連線
	* (shall) 連線建立且 bond 狀態確認後 Host 就得開啟加密
	* (shall) 若加密失敗 (例如 bonding 已經不存在), Host 就得重做 bonding, service discovery...等動作, 重新取得資料以免資料已經被改過
* Host-Initiated Connection Procedure for Bonded Devices
	* (may) 當 user 有需要或是有資料 (例如 LED 資訊) 要傳給 HID Device 的時候會由 Host 發起連線建立
	* (may) Host 可能會使用上面 (Connection Procedure for Non-bonded Devices) 提到的 4 種的其中 1 種 connection procedure 來連線
	* (should) HID host 要使用圖 5.6 的 scan interval / scan window, 注意 60ms 是用在假如 Host 還支援其他功能的時候, 用來給 Host 較充裕的時間處理其他功能
	* (may) 若一段時間內連線沒建立, Host 可以結束 GAP connection procedure
	* (should) Host 要使用合適的 scan interval / scan window 設定達到預期的耗電以及建立連線時間的需求 (彼此互相影響)
	* (should) scan interval / scan window 設定要符合使用者對連線建立時間的預期
	* (should) 若 Client Characteristic Configuration 有設定 Notification, HID Host 要等待 idle connection timeout 才結束連線, 以免 device 有 pending notification 還沒送達
	* (may) 一般來說 device 在資料交換完就會關閉連線, 但也可能會等 idle connection timeout 讓 host 處理完設定後由 host 關閉連線
	* (shall) 連線建立且 bond 狀態確認後 Host 就得開啟加密
	* (shall) 若加密失敗 (例如 bonding 已經不存在), Host 就得重做 bonding, service discovery...等動作, 重新取得資料以免資料已經被改過
* Link Loss Reconnection Procedure
	* (should) 若因為 link loss 斷線, HID host 嘗試跟 HID device 做連線, 使用上面 (Connection Procedure for Non-bonded Devices) 提到的 4 種的其中 1 種 connection procedure 來連線
	* (should) 如果 HID device 的 NormallyConnectable flag 設定為 TRUE, Report Host 參數要用 table 5.6 的設定
* Fast Connection Interval
	* (should) 為了避免太長的 service discovery 和加密時間, HID Host 建議使用 5.7 的參數於 connection request 內
		

![[hogp9.png]]![[hogp10.png]]

![[HOGP11.png]]

![[HOGP12.png]]

**\[Security Considerations\]**

Device Security Considerations

* (shall) HID Device 可以使用 Security Mode 1/2/3
* (shall) 所有 HID Service 內的 characteristics 必須使用  Security Mode 1/2/3
* (shall) HID Devices 必須使用 Security Mode 1/2/3 做 Bonding, 所以會有 encryption link
* (should) HID Device 必須送 SM Slave Security Request 告知 HID Host security requirement
* (should) 所以 Device Information Service, Scan Parameters Service 和 Battery Service 的 characteristics 必須用和 HID Service characteristics 相同的 LE Security Mode / Security Level

Host Security Considerations

* (shall) HID Host 必須和 HID Device 做 Bond
* (shall) HID Host 必須支援 Security Mode 1/2/3
* (shall) 來自 HID Device 任何合法的 security mode / security level 組合的 security request, HID Host 都得接受
* (shall) 只有當 HID Host 收到 SM Slave Security Request, 才可以做 encryption key refresh
