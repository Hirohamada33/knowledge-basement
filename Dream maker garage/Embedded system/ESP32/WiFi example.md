Below is the implementation, function breakdown for the WiFi example. 

```c
include "esp_wifi.h" // main library for WiFi driver and functions
include "esp_log.h" // For ESP_LOGI function
include "esp_err.h" // ESP error detection, handling and routine
include "lwip/err.h" // ESP light weight IP library 
include "lwip/sys.h"
include "nvs_flash.h" // non-volatile memory library - used for WIFI config storage
include "freertos/FreeRTOS.h" // event handling libraries
include "freertos/task.h" 
include "esp_event.h"

  
const char *TAG = "WIFI_INIT"; // ESP_LOGI tag

void nvs_init(){
	// WiFi store the relevant info in nvs (key-value pair).
	// nvs_flash_init(); // initialise nvs flash partition
	// A better error handling way to do this is
	esp_err_t mem_err = nvs_flash_init();
	if (mem_err == ESP_ERR_NVS_NO_FREE_PAGES){
		ESP_ERROR_CHECK(nvs_flash_erase());
		mem_err = nvs_flash_init();
	}
	// ESP_ERROR_CHECK(mem_err); probably no need to check again?
	ESP_LOGI(TAG, "NVS initialisation complete");
}

  

static void event_handler(void* arg, 
						  esp_event_base_t event_base,
						  int32_t event_id, 
						  void* event_data){
	static int s_retry_num = 0;
	if(event_base == WIFI_EVENT && event_id == WIFI_EVENT_STA_START){
		esp_wifi_connect();
		ESP_LOGI(TAG, "successfully connected to WiFi");
	} else if (event_base == WIFI_EVENT && event_id == WIFI_EVENT_STA_DISCONNECTED){
		if (s_retry_num < 5) {
			esp_wifi_connect();
			s_retry_num++;
			ESP_LOGI(TAG, "retry to connect to the AP");
		}
		ESP_LOGI(TAG,"connect to the AP fail");
	} else if (event_base == IP_EVENT && event_id == IP_EVENT_STA_GOT_IP) {
		ip_event_got_ip_t* event = (ip_event_got_ip_t*) event_data;
		ESP_LOGI(TAG, "got ip:" IPSTR, IP2STR(&event->ip_info.ip));
		s_retry_num = 0;
	}
}

void wifi_sta_init(){
    // create event loop
    ESP_ERROR_CHECK(esp_event_loop_create_default()); 

    // note that esp32 requires a network layer for implementing a WiFi
    ESP_ERROR_CHECK(esp_netif_init()); // Initialize the underlying TCP/IP stack.
    ESP_LOGI(TAG, "TCP/IP stack initialised"); 
    esp_netif_create_default_wifi_sta(); 
    // Creates default WIFI STA. Provided in separate APIs to facilitate simple startup code for most applications.
    ESP_LOGI(TAG, "Netif WiFi configuration"); 
    // Initialise the WIFI with default settings
    wifi_init_config_t config = WIFI_INIT_CONFIG_DEFAULT(); 
    ESP_ERROR_CHECK(esp_wifi_init(&config));
    ESP_LOGI(TAG, "WIFI initialisation complete"); 


    // event handler 
    esp_event_handler_instance_t instance_any_id; 
    esp_event_handler_instance_t instance_got_ip; 

    ESP_ERROR_CHECK(esp_event_handler_instance_register(WIFI_EVENT, ESP_EVENT_ANY_ID, &event_handler, NULL, &instance_any_id)); 
    ESP_ERROR_CHECK(esp_event_handler_instance_register(IP_EVENT, ESP_EVENT_ANY_ID, &event_handler, NULL, &instance_got_ip)); 

    // Set station mode: 
    wifi_mode_t mode = WIFI_MODE_STA; 
    ESP_ERROR_CHECK(esp_wifi_set_mode(mode));
    wifi_config_t wifi_config = {
        .sta = {
            .ssid = "Liu的iPhone",
            .password = "Turing complete"
        },
    };
    ESP_ERROR_CHECK(esp_wifi_set_config(WIFI_IF_STA, &wifi_config));
    ESP_LOGI(TAG, "Setting SSID and password..."); 
    ESP_ERROR_CHECK(esp_wifi_start()); 
    ESP_LOGI(TAG, "Starting WIFI"); 

    
}

void app_main(){
    nvs_init(); 
    wifi_sta_init(); 
}
```

