he Wi-Fi libraries provide support for configuring and monitoring the ESP32 Wi-Fi networking functionality. This includes configuration for:
- Station mode (aka STA mode or Wi-Fi client mode). ESP32 connects to an access point.
- AP mode (aka Soft-AP mode or Access Point mode). Stations connect to the ESP32.
- Station/AP-coexistence mode (ESP32 is concurrently an access point and a station connected to another access point).
- Various security modes for the above (WPA, WPA2, WPA3, etc.)
- Scanning for access points (active & passive scanning).
- Promiscuous mode for monitoring of IEEE802.11 Wi-Fi packets.


#### Functions 
Below list the key function for simple WiFi handling. 
Refer to [functions](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_wifi.html#functions) for list of detailed function explanation. 
###### `esp_wifi_init()`
Initialise WiFi Allocate resource for WiFi driver, such as WiFi control structure, RX/TX buffer, WiFi NVS structure etc. This WiFi also starts WiFi task.
```c
esp_err_t esp_wifi_init(const wifi_init_config_t *config)
```
**Note**:
1. This API must be called before all other WiFi API can be called. 
2. If not specified, use `WIFI_INIT_CONFIG_DEFAULT`. If other values are specified, fields can be overwritten. 

Parameters
	**config** -- pointer to WiFi initialised configuration structure; can point to a temporary variable.
Returns
- ESP_OK: succeed
- ESP_ERR_NO_MEM: out of memory
- other

###### `esp_wifi_deinit()`
Deinit WiFi Free all resource allocated in `esp_wifi_init` and stop WiFi task.
```c
esp_err_t esp_wifi_deinit(void)
```
**Note**:
1.  This API should be called if you want to remove WiFi driver from the system

Returns
- ESP_OK: succeed
- `ESP_ERR_WIFI_NOT_INIT`: WiFi is not initialised by `esp_wifi_init()`

###### `esp_wifi_set_mode()`
Set the WiFi operating mode.
```c
esp_err_t esp_wifi_set_mode(wifi_mode_t mode)
```
Parameters
	**mode** -- WiFi operating mode (station, soft-AP, station+soft-AP or NAN, default is `STA`)
Returns
- `ESP_OK`: succeed
- `ESP_ERR_WIFI_NOT_INIT`: WiFi is not initialised by `esp_wifi_init()`
- `ESP_ERR_INVALID_ARG`: invalid argument
- others

###### `esp_wifi_start()`
Start WiFi according to current configuration. 
If mode is `WIFI_MODE_STA`, it creates station control block and starts station. 
If mode is `WIFI_MODE_AP`, it creates `soft-AP` control block and starts `soft-AP`. 
If mode is `WIFI_MODE_APSTA`, it creates `soft-AP` and station control block and starts `soft-AP` and station. 
If mode is `WIFI_MODE_NAN`, it creates `NAN` control block and starts `NAN`.
```c
esp_err_t esp_wifi_start(void)
```
Returns
- `ESP_OK`: succeed
- `ESP_ERR_WIFI_NOT_INIT`: WiFi is not initialised by `esp_wifi_init()`
- `ESP_ERR_INVALID_ARG`: It doesn't normally happen, the function called inside the API was passed invalid argument, user should check if the WiFi related config is correct
- `ESP_ERR_NO_MEM`: out of memory
- `ESP_ERR_WIFI_CONN`: WiFi internal error, station or soft-AP control block wrong
- ESP_FAIL: other WiFi internal errors
###### `esp_wifi_stop()`
Stop WiFi If mode is `WIFI_MODE_STA`, it stops station and frees station control block. 
If mode is `WIFI_MODE_AP`, it stops soft-AP and frees `soft-AP` control block. 
If mode is `WIFI_MODE_APSTA`, it stops station/soft-AP and frees station/soft-AP control block.
If mode is `WIFI_MODE_NAN`, it stops `NAN` and frees `NAN` control block.
```c
esp_err_t esp_wifi_stop(void)
```


###### `esp_wifi_connect()`
Connect WiFi station to the AP.
```c
esp_err_t esp_wifi_connect(void)
```
**Note**:
1. This API only impact `WIFI_MODE_STA` or `WIFI_MODE_APSTA` mode
2. If station interface is connected to an AP, call `esp_wifi_disconnect()` to disconnect.
3. The scanning triggered by `esp_wifi_scan_start()` will not be effective until connection between device and the AP is established. If device is scanning and connecting at the same time, it will abort scanning and return a warning message and error number `ESP_ERR_WIFI_STATE`.
4. This API attempts to connect to an Access Point (AP) only once. To enable reconnection in case of a connection failure, please use the `failure_retry_cnt` feature in the '[wifi_sta_config_t](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_wifi.html#structwifi__sta__config__t)'. Users are suggested to implement reconnection logic in their application for scenarios where the specified AP does not exist, or reconnection is desired after the device has received a disconnect event.

Returns
- `ESP_OK`: succeed
- `ESP_ERR_WIFI_NOT_INIT`: WiFi is not initialised by `esp_wifi_init()`
- `ESP_ERR_WIFI_NOT_STARTED`: WiFi is not started by `esp_wifi_start()`
- `ESP_ERR_WIFI_MODE`: WiFi mode error
- `ESP_ERR_WIFI_CONN`: WiFi internal error, station or soft-AP control block wrong
- `ESP_ERR_WIFI_SSID`: SSID of AP which station connects is invalid

###### `esp_wifi_disconnect()`
Disconnect WiFi station from the AP.
```c
esp_err_t esp_wifi_disconnect(void)
```
Returns
- `ESP_OK`: succeed
- `ESP_ERR_WIFI_NOT_INIT`: WiFi was not initialised by `esp_wifi_init()`    
- `ESP_ERR_WIFI_NOT_STARTED`: WiFi was not started by `esp_wifi_start()`
- `ESP_FAIL`: other WiFi internal errors



