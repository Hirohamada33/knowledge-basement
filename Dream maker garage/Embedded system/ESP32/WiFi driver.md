Refer to [Wifi Driver](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-guides/wifi.html) documentation. 
##### Useful features
- The Espressif-specific ESP-NOW protocol and Long Range mode, which supports up to **1 km** of data traffic
- Up to 20 MBit/s TCP throughput and 30 MBit/s UDP throughput over the air
- Multiple antennas
- Channel state information
- Sniffer 

##### Suggestion
Generally, the most effective way to begin your own Wi-Fi application is to select an example which is similar to your own application, and port the useful part into your project.
Generally, it is easy to write code in "sunny-day" scenarios, such as `WIFI_EVENT_STA_START` and `WIFI_EVENT_STA_CONNECTED`. The hard part is to write routines in "rainy-day" scenarios, such as `WIFI_EVENT_STA_DISCONNECTED`. **Good handling of "rainy-day" scenarios is fundamental to robust Wi-Fi applications.**


##### Error code
The error code can be categorised into:
- No errors, e.g., `ESP_OK` means that the API returns successfully.
- Recoverable errors, such as `ESP_ERR_NO_MEM`.
	For recoverable errors, in which case you can write a recoverable-error code. For example, when `esp_wifi_start()` returns `ESP_ERR_NO_MEM`, the recoverable-error code `vTaskDelay` can be called in order to get a microseconds' delay for another try.
- Non-recoverable, non-critical errors.
	For non-recoverable, yet non-critical errors, in which case printing the error code is a good method for error handling.
- Non-recoverable, critical errors.
	For non-recoverable and also critical errors, in which case "assert" may be a good method for error handling. For example, if `esp_wifi_set_mode()` returns `ESP_ERR_WIFI_NOT_INIT`, it means that the Wi-Fi driver is not initialised by `esp_wifi_init()` successfully. You can detect this kind of error very quickly in the application development phase.


##### Initialisation
There are two ways of initialisation:
1. Explicitly set all fields of the parameter.
2. Use get API to get current configuration first, then set application specific fields. 

Initialising or getting the entire structure is very important, because most of the time the value 0 indicates that the default value is used.


##### Workflow
![[截圖 2025-01-08 下午2.40.29.png]]
The Wi-Fi driver can be considered a black box that knows nothing about high-layer code, such as the TCP/IP stack, application task, and event task. The application task (code) generally calls [Wi-Fi driver APIs](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-reference/network/esp_wifi.html) to initialize Wi-Fi and handles Wi-Fi events when necessary. Wi-Fi driver receives API calls, handles them, and posts events to the application.

Wi-Fi event handling is based on the [esp_event library](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-reference/system/esp_event.html). Events are sent by the Wi-Fi driver to the [default event loop](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-reference/system/esp_event.html#esp-event-default-loops). Application may handle these events in callbacks registered using [`esp_event_handler_register()`](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-reference/system/esp_event.html#_CPPv426esp_event_handler_register16esp_event_base_t7int32_t19esp_event_handler_tPv "esp_event_handler_register"). Wi-Fi events are also handled by [esp_netif component](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-reference/network/esp_netif.html) to provide a set of default behaviors. For example, when Wi-Fi station connects to an AP, `esp_netif` will automatically start the DHCP client by default.


##### Event description
`WIFI_EVENT_SCAN_DONE`
`WIFI_EVENT_STA_START`
`WIFI_EVENT_STA_STOP`
`WIFI_EVENT_STA_DISCONNECTED`
`IP_EVENT_STA_GOT_IP`
`WIFI_EVENT_STA_BEACON_TIMEOUT`


##### Development process 
![[Pasted image 20250108151815.png]]


###### Wi-Fi/LwIP Init Phase
s1.1: The main task calls `esp_netif_init()` to create an LwIP core task and initialise LwIP-related work.
s1.2: The main task calls `esp_event_loop_create()` to create a system Event task and initialise an application event's callback function. In the scenario above, the application event's callback function does nothing but relaying the event to the application task.
s1.3: The main task calls `esp_netif_create_default_wifi_ap()` or `esp_netif_create_default_wifi_sta()` to create default network interface instance binding station or AP with TCP/IP stack.
s1.4: The main task calls `esp_wifi_init()` to create the Wi-Fi driver task and initialise the Wi-Fi driver.
s1.5: The main task calls OS API to create the application task.

###### Wi-Fi Configuration Phase


