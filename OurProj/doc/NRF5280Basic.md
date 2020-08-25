# NRFBASIC 






## CODE 


### ERROR handler

- `app_error_fault_handler`

When `APP_ERROR_CHECK(err_code)` called , this func will be called while `DEBUG` is defined 


### EVENT

- `bsp_event_to_button_action_assign()`

The button's action bound a `event`, That means when this button `short cliked or long clicked or pushed` , This event will be triggered

- `bsp_init(BSP_INIT_LEDS|BSP_INIT_BUTTONS, bsp_event_handler)`

Init hardware and register a event handler ,That will be called when the hardware status changed , and the `corresponding event` trigger 


