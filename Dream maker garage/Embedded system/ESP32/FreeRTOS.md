Refer to [FreeRTOS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/freertos.html), [FreeRTOS (IDF)](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/freertos_idf.html)
FreeRTOS is an open source RTOS (real-time operating system) kernel that is integrated into ESP-IDF as a component. ESP-IDF provides different implementations of FreeRTOS in order to support SMP (Symmetric Multiprocessing) on multi-core ESP chips.


#### Background Tasks
During startup, ESP-IDF and the FreeRTOS kernel automatically create multiple tasks that run in the background (listed in the the table below).
![[截圖 2025-01-10 下午3.32.12.png]]\