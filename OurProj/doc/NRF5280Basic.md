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
  


    ```c
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

- `ble_uuid_t`

    Bluetooth Low Energy UUID type, encapsulates both 16-bit and 128-bit UUIDs
    ```c
    /** @brief . */
    typedef struct
    {
        uint16_t    uuid; /**< 16-bit UUID value or octets 12-13 of 128-bit UUID. */
        uint8_t     type; /**< UUID type, see @ref BLE_UUID_TYPES. If type is @ref BLE_UUID_TYPE_UNKNOWN, the value of uuid is undefined. */
    } ble_uuit_t
    ```

- `ble_lbs_s`

    LED button service structure , including `characteristic` and `uuid` and `service handle` , `characteristic handle` and `Event handler`

    ```c
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

- `ble_gatts_attr_t`



    ```c
    typedef struct
    {
    ble_gap_conn_sec_mode_t read_perm;       /**< Read permissions. */
    ble_gap_conn_sec_mode_t write_perm;      /**< Write permissions. */
    uint8_t                 vlen       :1;   /**< Variable length attribute. */
    uint8_t                 vloc       :2;   /**< Value location, see @ref BLE_GATTS_VLOCS.*/
    uint8_t                 rd_auth    :1;   /**< Read authorization and value will be requested from the application on every read operation. */
    uint8_t                 wr_auth    :1;   /**< Write authorization will be requested from the application on every Write Request operation (but not Write Command). */
    } ble_gatts_attr_md_t;

    ```

- `ble_gap_conn_sec_mode_t`



    ```c
        **@brief GAP connection security modes.
    *
    * Security Mode 0 Level 0: No access permissions at all (this level is not defined by the Bluetooth Core specification).\n
    * Security Mode 1 Level 1: No security is needed (aka open link).\n
    * Security Mode 1 Level 2: Encrypted link required, MITM protection not necessary.\n
    * Security Mode 1 Level 3: MITM protected encrypted link required.\n
    * Security Mode 1 Level 4: LESC MITM protected encrypted link using a 128-bit strength encryption key required.\n
    * Security Mode 2 Level 1: Signing or encryption required, MITM protection not necessary.\n
    * Security Mode 2 Level 2: MITM protected signing required, unless link is MITM protected encrypted.\n
    */
    typedef struct
    {
    uint8_t sm : 4;                     /**< Security Mode (1 or 2), 0 for no permissions at all. */
    uint8_t lv : 4;                     /**< Level (1, 2, 3 or 4), 0 for no permissions at all. */

    } ble_gap_conn_sec_mode_t;
    ```

- `ble_gatts_char_md_t`

```c
/**@brief GATT Characteristic metadata. */
typedef struct
{
  ble_gatt_char_props_t       char_props;               /**< Characteristic Properties. */
  ble_gatt_char_ext_props_t   char_ext_props;           /**< Characteristic Extended Properties. */
  uint8_t const              *p_char_user_desc;         /**< Pointer to a UTF-8 encoded string (non-NULL terminated), NULL if the descriptor is not required. */
  uint16_t                    char_user_desc_max_size;  /**< The maximum size in bytes of the user description descriptor. */
  uint16_t                    char_user_desc_size;      /**< The size of the user description, must be smaller or equal to char_user_desc_max_size. */
  ble_gatts_char_pf_t const  *p_char_pf;                /**< Pointer to a presentation format structure or NULL if the CPF descriptor is not required. */
  ble_gatts_attr_md_t const  *p_user_desc_md;           /**< Attribute metadata for the User Description descriptor, or NULL for default values. */
  ble_gatts_attr_md_t const  *p_cccd_md;                /**< Attribute metadata for the Client Characteristic Configuration Descriptor, or NULL for default values. */
  ble_gatts_attr_md_t const  *p_sccd_md;                /**< Attribute metadata for the Server Characteristic Configuration Descriptor, or NULL for default values. */
} ble_gatts_char_md_t;

```


- `ble_gatt_hvx_params_t`
```c
/**@brief GATT HVx parameters. */
typedef struct
{
  uint16_t          handle;             /**< Characteristic Value Handle. */
  uint8_t           type;               /**< Indication or Notification, see @ref BLE_GATT_HVX_TYPES. */
  uint16_t          offset;             /**< Offset within the attribute value. */
  uint16_t         *p_len;              /**< Length in bytes to be written, length in bytes written after return. */
  uint8_t const    *p_data;             /**< Actual data content, use NULL to use the current attribute value. */
} ble_gatts_hvx_params_t;
```