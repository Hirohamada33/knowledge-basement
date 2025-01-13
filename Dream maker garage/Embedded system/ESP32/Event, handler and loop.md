Refer to [Event Loop Library](https://docs.espressif.com/projects/esp-idf/en/v5.3.2/esp32/api-reference/system/esp_event.html) documentation. 

The **event loop library** in ESP-IDF provides a mechanism for components to communicate through events. Here's a breakdown:

1. **Event Declaration and Registration**:
  - Components can declare specific events (e.g., "Wi-Fi connected" or "Button pressed").
  - Other components or the application can register handlers (functions) that execute when  these events occur.
2. **Loose Coupling**:  
  - Components don't need to directly call each other. Instead, they interact through the event system, making them loosely coupled.
  - This reduces dependencies between components and improves modularity.
3. **No Application Involvement**:
   - The application doesn't need to act as a middleman between components.
   - For instance, a "Wi-Fi connected" event can trigger logging or network initialisation without explicit instructions in the main application.
4. **Simplified Event Processing**:
   - The library serialises event handling, ensuring that only one handler processes an event at a time.
   - Handlers are executed in a deferred context (usually a FreeRTOS task), which avoids blocking the main application or interrupt context.


##### Terminology
- **Events**
  An event indicates an important occurrence, such as a successful Wi-Fi connection to an access point.
- **Event loop**
  The bridge between events and event handlers. 
- **Event handler**
  The event handlers registered to the event loop respond to specific types of events.

##### Flow
1. The user defines a function that should run when an event is posted to a loop. This function is referred to as the event handler, and should have the same signature as `esp_event_handler_t`.
2. An event loop is created using `esp_event_loop_create()`, which outputs a handle to the loop of type `esp_event_loop_handle_t`. Event loops created using this API are referred to as user event loops. There is, however, a special type of event loop called the default event loop which is discussed in default event loop.
3. Components register event handlers to the loop using `esp_event_handler_register_with()`. Handlers can be registered with multiple loops, see notes on handler registration.
4. Event sources post an event to the loop using `esp_event_post_to()`.
5. Components wanting to remove their handlers from being called can do so by unregistering from the loop using `esp_event_handler_unregister_with()`.
6. Event loops that are no longer needed can be deleted using `esp_event_loop_delete()`.

```c
// 1. Define the event handler
void run_on_event(void* handler_arg, esp_event_base_t base, int32_t id, void* event_data)
{
    // Event handler logic
}

void app_main()
{
    // 2. A configuration structure of type esp_event_loop_args_t is needed to
    // specify the properties of the loop to be created. A handle of type
    // esp_event_loop_handle_t is obtained, which is needed by the other APIs to 
    // reference the loop to perform their operations.
    esp_event_loop_args_t loop_args = {
        .queue_size = ...,
        .task_name = ...
        .task_priority = ...,
        .task_stack_size = ...,
        .task_core_id = ...
    };

    esp_event_loop_handle_t loop_handle;

    esp_event_loop_create(&loop_args, &loop_handle);

    // 3. Register event handler defined in (1). MY_EVENT_BASE and MY_EVENT_ID 
    // specify a hypothetical event that handler run_on_event should execute 
    // when it gets posted to the loop.
    esp_event_handler_register_with(loop_handle, MY_EVENT_BASE, MY_EVENT_ID, run_on_event, ...);

    ...

    // 4. Post events to the loop. This queues the event on the event loop. At 
    // some point, the event loop executes the event handler registered to the 
    // posted event, in this case, run_on_event. To simplify the process, this 
    // example calls esp_event_post_to from app_main, but posting can be done 
    //from any other task (which is the more interesting use case).
    esp_event_post_to(loop_handle, MY_EVENT_BASE, MY_EVENT_ID, ...);

    ...

    // 5. Unregistering an unneeded handler
    esp_event_handler_unregister_with(loop_handle, MY_EVENT_BASE, MY_EVENT_ID, run_on_event);

    ...

    // 6. Deleting an unneeded event loop
    esp_event_loop_delete(loop_handle);
}
```

For WiFi-specific event loop, it uses `default_event_loop`. Refer to [[WiFi driver]]. 


