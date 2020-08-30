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




### SERVICE and Characteristic 

- `sd_ble_gatts_service_add(uint8_t type, ble_uuid_t const *p_uuid, uint16_t *p_handle)`

    Add a service to `attribute table` 

    - paramters 
        - type       primary service or seconday service 
        - p-uuid     pointer to service UUID 
        - p_handle   pointer to a 16-bit word where assign handle will be stored 


- `sd_ble_gatts_characteritic_add()`

    Add a `characteristic` on a `service` 
  


``` 
    typedef struct
    {
        ble_lbs_led_write_handler_t led_write_handler; /**< Event handler to be called when the LED Characteristic is written. */
    } ble_lbs_init_t;
```


- `sd_ble_uuid_vs_add(ble_uuid128_t const *p_vs_uuid, uint8_t *p_uuid_type)` 

    Take the uuid to a ble table for using it  later 

    - parameters

        `p_vs_uuid` : base UUID
        `p_uuid_type` : UUID's type 

- `ble_uuit_t`

    Bluetooth Low Energy UUID type, encapsulates both 16-bit and 128-bit UUIDs
    ```
    /** @brief . */
    typedef struct
    {
        uint16_t    uuid; /**< 16-bit UUID value or octets 12-13 of 128-bit UUID. */
        uint8_t     type; /**< UUID type, see @ref BLE_UUID_TYPES. If type is @ref BLE_UUID_TYPE_UNKNOWN, the value of uuid is undefined. */
    } ble_uuit_t
    ```

- `ble_lbs_s`

    LED button service structure , including `characteristic` and `uuid` and `service handle` , `characteristic handle` and `Event handler`

    ```
    #define BLE_LBS_DEF(_name)                                                                          \
    static ble_lbs_t _name;                                                                             \
    NRF_SDH_BLE_OBSERVER(_name ## _obs,                                                                 \
                        BLE_LBS_BLE_OBSERVER_PRIO,                                                     \
                        ble_lbs_on_ble_evt, &_name)
    struct ble_lbs_s
    {
        uint16_t                    service_handle;      /**< Handle of LED Button Service (as provided by the BLE stack). */
        ble_gatts_char_handles_t    led_char_handles;    /**< Handles related to the LED Characteristic. */
        ble_gatts_char_handles_t    button_char_handles; /**< Handles related to the Button Characteristic. */
        uint8_t                     uuid_type;           /**< UUID type for the LED Button Service. */
        ble_lbs_led_write_handler_t led_write_handler;   /**< Event handler to be called when the LED Characteristic is written. */
    };

    ```