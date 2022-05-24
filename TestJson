{
    "#comment": "",
    "html": {
      "@lang": "en",
      "head": {
        "meta": [
          {
            "@charset": "utf-8"
          },
          {
            "@http-equiv": "X-UA-Compatible",
            "@content": "IE=edge"
          },
          {
            "@name": "description",
            "@content": "Sample illustrating the use of Web Bluetooth / Notifications."
          },
          {
            "@name": "viewport",
            "@content": "width=device-width, initial-scale=1"
          }
        ],
        "title": "Web Bluetooth / Notifications Sample",
        "script": [
          {
            "@async": "",
            "@src": "https://www.google-analytics.com/analytics.js"
          },
          "// Add a global error event listener early on in the page load, to help ensure that browsers\r\n          // which don't support specific functionality still end up displaying a meaningful message.\r\n          window.addEventListener('error', function(error) {\r\n            if (ChromeSamples && ChromeSamples.setStatus) {\r\n              console.error(error);\r\n              ChromeSamples.setStatus(error.message + ' (Your browser may not support this feature.)');\r\n              error.preventDefault();\r\n            }\r\n          });"
        ],
        "link": [
          {
            "@rel": "icon",
            "@href": "icon.png"
          },
          {
            "@rel": "stylesheet",
            "@href": "../styles/main.css"
          }
        ]
      },
      "body": {
        "img": {
          "@class": "pageIcon",
          "@src": "icon.png"
        },
        "h1": "Web Bluetooth / Notifications Sample",
        "p": [
          {
            "@class": "availability",
            "a": [
              {
                "@target": "_blank",
                "@href": "https://www.chromestatus.com/feature/5264933985976320",
                "#text": "Chrome 48+"
              },
              {
                "@target": "_blank",
                "@href": "https://github.com/googlechrome/samples/blob/gh-pages/web-bluetooth/notifications.html",
                "#text": "View on GitHub"
              },
              {
                "@href": "index.html",
                "#text": "Browse Samples"
              }
            ],
            "#text": [
              "Available in",
              "|",
              "|"
            ]
          },
          {
            "a": {
              "@href": "https://developers.google.com/web/updates/2015/07/interact-with-ble-devices-on-the-web",
              "#text": "Web Bluetooth\r\n  API"
            },
            "#text": [
              "The",
              "lets websites discover and communicate with devices over the\r\nBluetooth 4 wireless standard using the Generic Attribute Profile (GATT). It is\r\ncurrently partially implemented in Android M, Chrome OS, Mac, and Windows 10."
            ]
          },
          {
            "a": [
              {
                "@target": "_blank",
                "@href": "https://play.google.com/store/apps/details?id=io.github.webbluetoothcg.bletestperipheral",
                "#text": "Google\r\nPlay Store"
              },
              {
                "@href": "notifications-async-await.html",
                "#text": "Notifications (Async Await)"
              }
            ],
            "#text": [
              "This sample illustrates the use of the Web Bluetooth API to start and stop\r\ncharacteristic notifications from a nearby Bluetooth Low Energy Device. You may\r\nwant to try this demo with the BLE Peripheral Simulator App\r\nfrom the",
              "and check out the",
              "sample."
            ]
          }
        ],
        "script": [
          "if('serviceWorker' in navigator) {\r\n    navigator.serviceWorker.register('service-worker.js');\r\n  }",
          "window.addEventListener('DOMContentLoaded', function() {\r\n    const searchParams = new URL(location).searchParams;\r\n    const inputs = Array.from(document.querySelectorAll('input[id]'));\r\n\r\n    inputs.forEach(input => {\r\n      if (searchParams.has(input.id)) {\r\n        if (input.type == 'checkbox') {\r\n          input.checked = searchParams.get(input.id);\r\n        } else {\r\n          input.value = searchParams.get(input.id);\r\n          input.blur();\r\n        }\r\n      }\r\n      if (input.type == 'checkbox') {\r\n        input.addEventListener('change', function(event) {\r\n          const newSearchParams = new URL(location).searchParams;\r\n          if (event.target.checked) {\r\n            newSearchParams.set(input.id, event.target.checked);\r\n          } else {\r\n            newSearchParams.delete(input.id);\r\n          }\r\n          history.replaceState({}, '', Array.from(newSearchParams).length ?\r\n              location.pathname + '?' + newSearchParams : location.pathname);\r\n        });\r\n      } else {\r\n        input.addEventListener('input', function(event) {\r\n          const newSearchParams = new URL(location).searchParams;\r\n          if (event.target.value) {\r\n            newSearchParams.set(input.id, event.target.value);\r\n          } else {\r\n            newSearchParams.delete(input.id);\r\n          }\r\n          history.replaceState({}, '', Array.from(newSearchParams).length ?\r\n              location.pathname + '?' + newSearchParams : location.pathname);\r\n        });\r\n      }\r\n    });\r\n  });",
          "var ChromeSamples = {\r\n    log: function() {\r\n      var line = Array.prototype.slice.call(arguments).map(function(argument) {\r\n        return typeof argument === 'string' ? argument : JSON.stringify(argument);\r\n      }).join(' ');\r\n\r\n      document.querySelector('#log').textContent += line + '\\n';\r\n    },\r\n\r\n    clearLog: function() {\r\n      document.querySelector('#log').textContent = '';\r\n    },\r\n\r\n    setStatus: function(status) {\r\n      document.querySelector('#status').textContent = status;\r\n    },\r\n\r\n    setContent: function(newContent) {\r\n      var content = document.querySelector('#content');\r\n      while(content.hasChildNodes()) {\r\n        content.removeChild(content.lastChild);\r\n      }\r\n      content.appendChild(newContent);\r\n    }\r\n  };",
          "if (/Chrome\\/(\\d+\\.\\d+.\\d+.\\d+)/.test(navigator.userAgent)){\r\n    // Let's log a warning if the sample is not supposed to execute on this\r\n    // version of Chrome.\r\n    if (48 > parseInt(RegExp.$1)) {\r\n      ChromeSamples.setStatus('Warning! Keep in mind this sample has been tested with Chrome ' + 48 + '.');\r\n    }\r\n  }",
          "var myCharacteristic;\r\n\r\nfunction onStartButtonClick() {\r\n  let serviceUuid = document.querySelector('#service').value;\r\n  if (serviceUuid.startsWith('0x')) {\r\n    serviceUuid = parseInt(serviceUuid);\r\n  }\r\n\r\n  let characteristicUuid = document.querySelector('#characteristic').value;\r\n  if (characteristicUuid.startsWith('0x')) {\r\n    characteristicUuid = parseInt(characteristicUuid);\r\n  }\r\n\r\n  log('Requesting Bluetooth Device...');\r\n  navigator.bluetooth.requestDevice({filters: [{services: [serviceUuid]}]})\r\n  .then(device => {\r\n    log('Connecting to GATT Server...');\r\n    return device.gatt.connect();\r\n  })\r\n  .then(server => {\r\n    log('Getting Service...');\r\n    return server.getPrimaryService(serviceUuid);\r\n  })\r\n  .then(service => {\r\n    log('Getting Characteristic...');\r\n    return service.getCharacteristic(characteristicUuid);\r\n  })\r\n  .then(characteristic => {\r\n    myCharacteristic = characteristic;\r\n    return myCharacteristic.startNotifications().then(_ => {\r\n      log('> Notifications started');\r\n      myCharacteristic.addEventListener('characteristicvaluechanged',\r\n          handleNotifications);\r\n    });\r\n  })\r\n  .catch(error => {\r\n    log('Argh! ' + error);\r\n  });\r\n}\r\n\r\nfunction onStopButtonClick() {\r\n  if (myCharacteristic) {\r\n    myCharacteristic.stopNotifications()\r\n    .then(_ => {\r\n      log('> Notifications stopped');\r\n      myCharacteristic.removeEventListener('characteristicvaluechanged',\r\n          handleNotifications);\r\n    })\r\n    .catch(error => {\r\n      log('Argh! ' + error);\r\n    });\r\n  }\r\n}\r\n\r\nfunction handleNotifications(event) {\r\n  let value = event.target.value;\r\n  let a = [];\r\n  // Convert raw data bytes to hex values just for the sake of showing something.\r\n  // In the \"real\" world, you'd use data.getUint8, data.getUint16 or even\r\n  // TextDecoder to process raw data bytes.\r\n  for (let i = 0; i < value.byteLength; i++) {\r\n    a.push('0x' + ('00' + value.getUint8(i).toString(16)).slice(-2));\r\n  }\r\n  log('> ' + a.join(' '));\r\n}",
          "document.querySelector('#startNotifications').addEventListener('click', function(event) {\r\n    event.stopPropagation();\r\n    event.preventDefault();\r\n\r\n    if (isWebBluetoothEnabled()) {\r\n      ChromeSamples.clearLog();\r\n      onStartButtonClick();\r\n    }\r\n  });\r\n  document.querySelector('#stopNotifications').addEventListener('click', function(event) {\r\n    event.stopPropagation();\r\n    event.preventDefault();\r\n\r\n    if (isWebBluetoothEnabled()) {\r\n      onStopButtonClick();\r\n    }\r\n  });",
          "log = ChromeSamples.log;\r\n\r\n  function isWebBluetoothEnabled() {\r\n    if (navigator.bluetooth) {\r\n      return true;\r\n    } else {\r\n      ChromeSamples.setStatus('Web Bluetooth API is not available.\\n' +\r\n          'Please make sure the \"Experimental Web Platform features\" flag is enabled.');\r\n      return false;\r\n    }\r\n  }",
          "/* jshint ignore:start */\r\n      (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){\r\n          (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),\r\n        m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)\r\n      })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');\r\n      ga('create', 'UA-53563471-1', 'auto');\r\n      ga('send', 'pageview');\r\n      /* jshint ignore:end */"
        ],
        "form": {
          "input": [
            {
              "@id": "service",
              "@type": "text",
              "@list": "services",
              "@autofocus": "",
              "@placeholder": "Bluetooth Service"
            },
            {
              "@id": "characteristic",
              "@type": "text",
              "@list": "characteristics",
              "@placeholder": "Bluetooth Characteristic"
            }
          ],
          "button": [
            {
              "@id": "startNotifications",
              "#text": "Start notifications"
            },
            {
              "@id": "stopNotifications",
              "#text": "Stop notifications"
            }
          ]
        },
        "datalist": [
          {
            "@id": "services",
            "option": [
              {
                "@value": "alert_notification",
                "#text": "alert_notification"
              },
              {
                "@value": "automation_io",
                "#text": "automation_io"
              },
              {
                "@value": "battery_service",
                "#text": "battery_service"
              },
              {
                "@value": "blood_pressure",
                "#text": "blood_pressure"
              },
              {
                "@value": "body_composition",
                "#text": "body_composition"
              },
              {
                "@value": "bond_management",
                "#text": "bond_management"
              },
              {
                "@value": "continuous_glucose_monitoring",
                "#text": "continuous_glucose_monitoring"
              },
              {
                "@value": "current_time",
                "#text": "current_time"
              },
              {
                "@value": "cycling_power",
                "#text": "cycling_power"
              },
              {
                "@value": "cycling_speed_and_cadence",
                "#text": "cycling_speed_and_cadence"
              },
              {
                "@value": "device_information",
                "#text": "device_information"
              },
              {
                "@value": "environmental_sensing",
                "#text": "environmental_sensing"
              },
              {
                "@value": "generic_access",
                "#text": "generic_access"
              },
              {
                "@value": "generic_attribute",
                "#text": "generic_attribute"
              },
              {
                "@value": "glucose",
                "#text": "glucose"
              },
              {
                "@value": "health_thermometer",
                "#text": "health_thermometer"
              },
              {
                "@value": "heart_rate",
                "#text": "heart_rate"
              },
              {
                "@value": "human_interface_device",
                "#text": "human_interface_device (blocklisted)"
              },
              {
                "@value": "immediate_alert",
                "#text": "immediate_alert"
              },
              {
                "@value": "indoor_positioning",
                "#text": "indoor_positioning"
              },
              {
                "@value": "internet_protocol_support",
                "#text": "internet_protocol_support"
              },
              {
                "@value": "link_loss",
                "#text": "link_loss"
              },
              {
                "@value": "location_and_navigation",
                "#text": "location_and_navigation"
              },
              {
                "@value": "next_dst_change",
                "#text": "next_dst_change"
              },
              {
                "@value": "phone_alert_status",
                "#text": "phone_alert_status"
              },
              {
                "@value": "pulse_oximeter",
                "#text": "pulse_oximeter"
              },
              {
                "@value": "reference_time_update",
                "#text": "reference_time_update"
              },
              {
                "@value": "running_speed_and_cadence",
                "#text": "running_speed_and_cadence"
              },
              {
                "@value": "scan_parameters",
                "#text": "scan_parameters"
              },
              {
                "@value": "tx_power",
                "#text": "tx_power"
              },
              {
                "@value": "user_data",
                "#text": "user_data"
              },
              {
                "@value": "weight_scale",
                "#text": "weight_scale"
              }
            ]
          },
          {
            "@id": "characteristics",
            "option": [
              {
                "@value": "aerobic_heart_rate_lower_limit",
                "#text": "aerobic_heart_rate_lower_limit"
              },
              {
                "@value": "aerobic_heart_rate_upper_limit",
                "#text": "aerobic_heart_rate_upper_limit"
              },
              {
                "@value": "aerobic_threshold",
                "#text": "aerobic_threshold"
              },
              {
                "@value": "age",
                "#text": "age"
              },
              {
                "@value": "aggregate",
                "#text": "aggregate"
              },
              {
                "@value": "alert_category_id",
                "#text": "alert_category_id"
              },
              {
                "@value": "alert_category_id_bit_mask",
                "#text": "alert_category_id_bit_mask"
              },
              {
                "@value": "alert_level",
                "#text": "alert_level"
              },
              {
                "@value": "alert_notification_control_point",
                "#text": "alert_notification_control_point"
              },
              {
                "@value": "alert_status",
                "#text": "alert_status"
              },
              {
                "@value": "altitude",
                "#text": "altitude"
              },
              {
                "@value": "anaerobic_heart_rate_lower_limit",
                "#text": "anaerobic_heart_rate_lower_limit"
              },
              {
                "@value": "anaerobic_heart_rate_upper_limit",
                "#text": "anaerobic_heart_rate_upper_limit"
              },
              {
                "@value": "anaerobic_threshold",
                "#text": "anaerobic_threshold"
              },
              {
                "@value": "analog",
                "#text": "analog"
              },
              {
                "@value": "apparent_wind_direction",
                "#text": "apparent_wind_direction"
              },
              {
                "@value": "apparent_wind_speed",
                "#text": "apparent_wind_speed"
              },
              {
                "@value": "gap.appearance",
                "#text": "gap.appearance"
              },
              {
                "@value": "barometric_pressure_trend",
                "#text": "barometric_pressure_trend"
              },
              {
                "@value": "battery_level",
                "#text": "battery_level"
              },
              {
                "@value": "blood_pressure_feature",
                "#text": "blood_pressure_feature"
              },
              {
                "@value": "blood_pressure_measurement",
                "#text": "blood_pressure_measurement"
              },
              {
                "@value": "body_composition_feature",
                "#text": "body_composition_feature"
              },
              {
                "@value": "body_composition_measurement",
                "#text": "body_composition_measurement"
              },
              {
                "@value": "body_sensor_location",
                "#text": "body_sensor_location"
              },
              {
                "@value": "bond_management_control_point",
                "#text": "bond_management_control_point"
              },
              {
                "@value": "bond_management_feature",
                "#text": "bond_management_feature"
              },
              {
                "@value": "boot_keyboard_input_report",
                "#text": "boot_keyboard_input_report"
              },
              {
                "@value": "boot_keyboard_output_report",
                "#text": "boot_keyboard_output_report"
              },
              {
                "@value": "boot_mouse_input_report",
                "#text": "boot_mouse_input_report"
              },
              {
                "@value": "gap.central_address_resolution_support",
                "#text": "gap.central_address_resolution_support"
              },
              {
                "@value": "cgm_feature",
                "#text": "cgm_feature"
              },
              {
                "@value": "cgm_measurement",
                "#text": "cgm_measurement"
              },
              {
                "@value": "cgm_session_run_time",
                "#text": "cgm_session_run_time"
              },
              {
                "@value": "cgm_session_start_time",
                "#text": "cgm_session_start_time"
              },
              {
                "@value": "cgm_specific_ops_control_point",
                "#text": "cgm_specific_ops_control_point"
              },
              {
                "@value": "cgm_status",
                "#text": "cgm_status"
              },
              {
                "@value": "csc_feature",
                "#text": "csc_feature"
              },
              {
                "@value": "csc_measurement",
                "#text": "csc_measurement"
              },
              {
                "@value": "current_time",
                "#text": "current_time"
              },
              {
                "@value": "cycling_power_control_point",
                "#text": "cycling_power_control_point"
              },
              {
                "@value": "cycling_power_feature",
                "#text": "cycling_power_feature"
              },
              {
                "@value": "cycling_power_measurement",
                "#text": "cycling_power_measurement"
              },
              {
                "@value": "cycling_power_vector",
                "#text": "cycling_power_vector"
              },
              {
                "@value": "database_change_increment",
                "#text": "database_change_increment"
              },
              {
                "@value": "date_of_birth",
                "#text": "date_of_birth"
              },
              {
                "@value": "date_of_threshold_assessment",
                "#text": "date_of_threshold_assessment"
              },
              {
                "@value": "date_time",
                "#text": "date_time"
              },
              {
                "@value": "day_date_time",
                "#text": "day_date_time"
              },
              {
                "@value": "day_of_week",
                "#text": "day_of_week"
              },
              {
                "@value": "descriptor_value_changed",
                "#text": "descriptor_value_changed"
              },
              {
                "@value": "gap.device_name",
                "#text": "gap.device_name"
              },
              {
                "@value": "dew_point",
                "#text": "dew_point"
              },
              {
                "@value": "digital",
                "#text": "digital"
              },
              {
                "@value": "dst_offset",
                "#text": "dst_offset"
              },
              {
                "@value": "elevation",
                "#text": "elevation"
              },
              {
                "@value": "email_address",
                "#text": "email_address"
              },
              {
                "@value": "exact_time_256",
                "#text": "exact_time_256"
              },
              {
                "@value": "fat_burn_heart_rate_lower_limit",
                "#text": "fat_burn_heart_rate_lower_limit"
              },
              {
                "@value": "fat_burn_heart_rate_upper_limit",
                "#text": "fat_burn_heart_rate_upper_limit"
              },
              {
                "@value": "firmware_revision_string",
                "#text": "firmware_revision_string"
              },
              {
                "@value": "first_name",
                "#text": "first_name"
              },
              {
                "@value": "five_zone_heart_rate_limits",
                "#text": "five_zone_heart_rate_limits"
              },
              {
                "@value": "floor_number",
                "#text": "floor_number"
              },
              {
                "@value": "gender",
                "#text": "gender"
              },
              {
                "@value": "glucose_feature",
                "#text": "glucose_feature"
              },
              {
                "@value": "glucose_measurement",
                "#text": "glucose_measurement"
              },
              {
                "@value": "glucose_measurement_context",
                "#text": "glucose_measurement_context"
              },
              {
                "@value": "gust_factor",
                "#text": "gust_factor"
              },
              {
                "@value": "hardware_revision_string",
                "#text": "hardware_revision_string"
              },
              {
                "@value": "heart_rate_control_point",
                "#text": "heart_rate_control_point"
              },
              {
                "@value": "heart_rate_max",
                "#text": "heart_rate_max"
              },
              {
                "@value": "heart_rate_measurement",
                "#text": "heart_rate_measurement"
              },
              {
                "@value": "heat_index",
                "#text": "heat_index"
              },
              {
                "@value": "height",
                "#text": "height"
              },
              {
                "@value": "hid_control_point",
                "#text": "hid_control_point"
              },
              {
                "@value": "hid_information",
                "#text": "hid_information"
              },
              {
                "@value": "hip_circumference",
                "#text": "hip_circumference"
              },
              {
                "@value": "humidity",
                "#text": "humidity"
              },
              {
                "@value": "ieee_11073-20601_regulatory_certification_data_list"
              },
              {
                "@value": "indoor_positioning_configuration",
                "#text": "indoor_positioning_configuration"
              },
              {
                "@value": "intermediate_blood_pressure",
                "#text": "intermediate_blood_pressure"
              },
              {
                "@value": "intermediate_temperature",
                "#text": "intermediate_temperature"
              },
              {
                "@value": "irradiance",
                "#text": "irradiance"
              },
              {
                "@value": "language",
                "#text": "language"
              },
              {
                "@value": "last_name",
                "#text": "last_name"
              },
              {
                "@value": "latitude",
                "#text": "latitude"
              },
              {
                "@value": "ln_control_point",
                "#text": "ln_control_point"
              },
              {
                "@value": "ln_feature",
                "#text": "ln_feature"
              },
              {
                "@value": "local_east_coordinate.xml",
                "#text": "local_east_coordinate.xml"
              },
              {
                "@value": "local_north_coordinate",
                "#text": "local_north_coordinate"
              },
              {
                "@value": "local_time_information",
                "#text": "local_time_information"
              },
              {
                "@value": "location_and_speed",
                "#text": "location_and_speed"
              },
              {
                "@value": "location_name",
                "#text": "location_name"
              },
              {
                "@value": "longitude",
                "#text": "longitude"
              },
              {
                "@value": "magnetic_declination",
                "#text": "magnetic_declination"
              },
              {
                "@value": "magnetic_flux_density_2D",
                "#text": "magnetic_flux_density_2D"
              },
              {
                "@value": "magnetic_flux_density_3D",
                "#text": "magnetic_flux_density_3D"
              },
              {
                "@value": "manufacturer_name_string",
                "#text": "manufacturer_name_string"
              },
              {
                "@value": "maximum_recommended_heart_rate",
                "#text": "maximum_recommended_heart_rate"
              },
              {
                "@value": "measurement_interval",
                "#text": "measurement_interval"
              },
              {
                "@value": "model_number_string",
                "#text": "model_number_string"
              },
              {
                "@value": "navigation",
                "#text": "navigation"
              },
              {
                "@value": "new_alert",
                "#text": "new_alert"
              },
              {
                "@value": "gap.peripheral_preferred_connection_parameters",
                "#text": "gap.peripheral_preferred_connection_parameters"
              },
              {
                "@value": "gap.peripheral_privacy_flag",
                "#text": "gap.peripheral_privacy_flag (blocklisted - write)"
              },
              {
                "@value": "plx_continuous_measurement",
                "#text": "plx_continuous_measurement"
              },
              {
                "@value": "plx_features",
                "#text": "plx_features"
              },
              {
                "@value": "plx_spot_check_measurement",
                "#text": "plx_spot_check_measurement"
              },
              {
                "@value": "pnp_id",
                "#text": "pnp_id"
              },
              {
                "@value": "pollen_concentration",
                "#text": "pollen_concentration"
              },
              {
                "@value": "position_quality",
                "#text": "position_quality"
              },
              {
                "@value": "pressure",
                "#text": "pressure"
              },
              {
                "@value": "protocol_mode",
                "#text": "protocol_mode"
              },
              {
                "@value": "rainfall",
                "#text": "rainfall"
              },
              {
                "@value": "gap.reconnection_address",
                "#text": "gap.reconnection_address (blocklisted)"
              },
              {
                "@value": "record_access_control_point",
                "#text": "record_access_control_point"
              },
              {
                "@value": "reference_time_information",
                "#text": "reference_time_information"
              },
              {
                "@value": "report",
                "#text": "report"
              },
              {
                "@value": "report_map",
                "#text": "report_map"
              },
              {
                "@value": "resting_heart_rate",
                "#text": "resting_heart_rate"
              },
              {
                "@value": "ringer_control_point",
                "#text": "ringer_control_point"
              },
              {
                "@value": "ringer_setting",
                "#text": "ringer_setting"
              },
              {
                "@value": "rsc_feature",
                "#text": "rsc_feature"
              },
              {
                "@value": "rsc_measurement",
                "#text": "rsc_measurement"
              },
              {
                "@value": "sc_control_point",
                "#text": "sc_control_point"
              },
              {
                "@value": "scan_interval_window",
                "#text": "scan_interval_window"
              },
              {
                "@value": "scan_refresh",
                "#text": "scan_refresh"
              },
              {
                "@value": "sensor_location",
                "#text": "sensor_location"
              },
              {
                "@value": "serial_number_string",
                "#text": "serial_number_string (blocklisted)"
              },
              {
                "@value": "gatt.service_changed",
                "#text": "gatt.service_changed"
              },
              {
                "@value": "software_revision_string",
                "#text": "software_revision_string"
              },
              {
                "@value": "sport_type_for_aerobic_and_anaerobic_thresholds",
                "#text": "sport_type_for_aerobic_and_anaerobic_thresholds"
              },
              {
                "@value": "supported_new_alert_category",
                "#text": "supported_new_alert_category"
              },
              {
                "@value": "supported_unread_alert_category",
                "#text": "supported_unread_alert_category"
              },
              {
                "@value": "system_id",
                "#text": "system_id"
              },
              {
                "@value": "temperature",
                "#text": "temperature"
              },
              {
                "@value": "temperature_measurement",
                "#text": "temperature_measurement"
              },
              {
                "@value": "temperature_type",
                "#text": "temperature_type"
              },
              {
                "@value": "three_zone_heart_rate_limits",
                "#text": "three_zone_heart_rate_limits"
              },
              {
                "@value": "time_accuracy",
                "#text": "time_accuracy"
              },
              {
                "@value": "time_source",
                "#text": "time_source"
              },
              {
                "@value": "time_update_control_point",
                "#text": "time_update_control_point"
              },
              {
                "@value": "time_update_state",
                "#text": "time_update_state"
              },
              {
                "@value": "time_with_dst",
                "#text": "time_with_dst"
              },
              {
                "@value": "time_zone",
                "#text": "time_zone"
              },
              {
                "@value": "true_wind_direction",
                "#text": "true_wind_direction"
              },
              {
                "@value": "true_wind_speed",
                "#text": "true_wind_speed"
              },
              {
                "@value": "two_zone_heart_rate_limit",
                "#text": "two_zone_heart_rate_limit"
              },
              {
                "@value": "tx_power_level",
                "#text": "tx_power_level"
              },
              {
                "@value": "uncertainty",
                "#text": "uncertainty"
              },
              {
                "@value": "unread_alert_status",
                "#text": "unread_alert_status"
              },
              {
                "@value": "user_control_point",
                "#text": "user_control_point"
              },
              {
                "@value": "user_index",
                "#text": "user_index"
              },
              {
                "@value": "uv_index",
                "#text": "uv_index"
              },
              {
                "@value": "vo2_max",
                "#text": "vo2_max"
              },
              {
                "@value": "waist_circumference",
                "#text": "waist_circumference"
              },
              {
                "@value": "weight",
                "#text": "weight"
              },
              {
                "@value": "weight_measurement",
                "#text": "weight_measurement"
              },
              {
                "@value": "weight_scale_feature",
                "#text": "weight_scale_feature"
              },
              {
                "@value": "wind_chill",
                "#text": "wind_chill"
              }
            ]
          }
        ],
        "h3": [
          "Live Output",
          "JavaScript Snippet"
        ],
        "div": {
          "@id": "output",
          "@class": "output",
          "div": [
            {
              "@id": "content"
            },
            {
              "@id": "status"
            }
          ],
          "pre": {
            "@id": "log"
          }
        },
        "figure": {
          "@class": "highlight",
          "pre": {
            "code": {
              "@class": "language-js",
              "@data-lang": "js",
              "span": [
                {
                  "@class": "kd",
                  "#text": "var"
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "kd",
                  "#text": "function"
                },
                {
                  "@class": "nx",
                  "#text": "onStartButtonClick"
                },
                {
                  "@class": "p",
                  "#text": "()"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "kd",
                  "#text": "let"
                },
                {
                  "@class": "nx",
                  "#text": "serviceUuid"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "nb",
                  "#text": "document"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "querySelector"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "#service"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ")."
                },
                {
                  "@class": "nx",
                  "#text": "value"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "k",
                  "#text": "if"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "serviceUuid"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "startsWith"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "0x"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": "))"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "serviceUuid"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "nb",
                  "#text": "parseInt"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "serviceUuid"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "}"
                },
                {
                  "@class": "kd",
                  "#text": "let"
                },
                {
                  "@class": "nx",
                  "#text": "characteristicUuid"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "nb",
                  "#text": "document"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "querySelector"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "#characteristic"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ")."
                },
                {
                  "@class": "nx",
                  "#text": "value"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "k",
                  "#text": "if"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "characteristicUuid"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "startsWith"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "0x"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": "))"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "characteristicUuid"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "nb",
                  "#text": "parseInt"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "characteristicUuid"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "}"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "Requesting Bluetooth Device..."
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "nb",
                  "#text": "navigator"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "bluetooth"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "requestDevice"
                },
                {
                  "@class": "p",
                  "#text": "({"
                },
                {
                  "@class": "na",
                  "#text": "filters"
                },
                {
                  "@class": "p",
                  "#text": ":"
                },
                {
                  "@class": "p",
                  "#text": "[{"
                },
                {
                  "@class": "na",
                  "#text": "services"
                },
                {
                  "@class": "p",
                  "#text": ":"
                },
                {
                  "@class": "p",
                  "#text": "["
                },
                {
                  "@class": "nx",
                  "#text": "serviceUuid"
                },
                {
                  "@class": "p",
                  "#text": "]}]})"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "then"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "device"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "Connecting to GATT Server..."
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "k",
                  "#text": "return"
                },
                {
                  "@class": "nx",
                  "#text": "device"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "gatt"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "connect"
                },
                {
                  "@class": "p",
                  "#text": "();"
                },
                {
                  "@class": "p",
                  "#text": "})"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "then"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "server"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "Getting Service..."
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "k",
                  "#text": "return"
                },
                {
                  "@class": "nx",
                  "#text": "server"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "getPrimaryService"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "serviceUuid"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "})"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "then"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "service"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "Getting Characteristic..."
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "k",
                  "#text": "return"
                },
                {
                  "@class": "nx",
                  "#text": "service"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "getCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "characteristicUuid"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "})"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "then"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "characteristic"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "nx",
                  "#text": "characteristic"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "k",
                  "#text": "return"
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "startNotifications"
                },
                {
                  "@class": "p",
                  "#text": "()."
                },
                {
                  "@class": "nx",
                  "#text": "then"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "_"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "> Notifications started"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "addEventListener"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "characteristicvaluechanged"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ","
                },
                {
                  "@class": "nx",
                  "#text": "handleNotifications"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "});"
                },
                {
                  "@class": "p",
                  "#text": "})"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "k",
                  "#text": "catch"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "error"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "Argh!"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "o",
                  "#text": "+"
                },
                {
                  "@class": "nx",
                  "#text": "error"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "});"
                },
                {
                  "@class": "p",
                  "#text": "}"
                },
                {
                  "@class": "kd",
                  "#text": "function"
                },
                {
                  "@class": "nx",
                  "#text": "onStopButtonClick"
                },
                {
                  "@class": "p",
                  "#text": "()"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "k",
                  "#text": "if"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": ")"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "stopNotifications"
                },
                {
                  "@class": "p",
                  "#text": "()"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "then"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "_"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "> Notifications stopped"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "nx",
                  "#text": "myCharacteristic"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "removeEventListener"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "characteristicvaluechanged"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": ","
                },
                {
                  "@class": "nx",
                  "#text": "handleNotifications"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "})"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "k",
                  "#text": "catch"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "error"
                },
                {
                  "@class": "o",
                  "#text": "=>"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "Argh!"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "o",
                  "#text": "+"
                },
                {
                  "@class": "nx",
                  "#text": "error"
                },
                {
                  "@class": "p",
                  "#text": ");"
                },
                {
                  "@class": "p",
                  "#text": "});"
                },
                {
                  "@class": "p",
                  "#text": "}"
                },
                {
                  "@class": "p",
                  "#text": "}"
                },
                {
                  "@class": "kd",
                  "#text": "function"
                },
                {
                  "@class": "nx",
                  "#text": "handleNotifications"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "event"
                },
                {
                  "@class": "p",
                  "#text": ")"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "kd",
                  "#text": "let"
                },
                {
                  "@class": "nx",
                  "#text": "value"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "nx",
                  "#text": "event"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "target"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "value"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "kd",
                  "#text": "let"
                },
                {
                  "@class": "nx",
                  "#text": "a"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "p",
                  "#text": "[];"
                },
                {
                  "@class": "c1",
                  "#text": "// Convert raw data bytes to hex values just for the sake of showing something."
                },
                {
                  "@class": "c1",
                  "#text": "// In the \"real\" world, you'd use data.getUint8, data.getUint16 or even"
                },
                {
                  "@class": "c1",
                  "#text": "// TextDecoder to process raw data bytes."
                },
                {
                  "@class": "k",
                  "#text": "for"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "kd",
                  "#text": "let"
                },
                {
                  "@class": "nx",
                  "#text": "i"
                },
                {
                  "@class": "o",
                  "#text": "="
                },
                {
                  "@class": "mi",
                  "#text": "0"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "nx",
                  "#text": "i"
                },
                {
                  "@class": "o",
                  "#text": "<"
                },
                {
                  "@class": "nx",
                  "#text": "value"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "byteLength"
                },
                {
                  "@class": "p",
                  "#text": ";"
                },
                {
                  "@class": "nx",
                  "#text": "i"
                },
                {
                  "@class": "o",
                  "#text": "++"
                },
                {
                  "@class": "p",
                  "#text": ")"
                },
                {
                  "@class": "p",
                  "#text": "{"
                },
                {
                  "@class": "nx",
                  "#text": "a"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "push"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "0x"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "o",
                  "#text": "+"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": "00"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "o",
                  "#text": "+"
                },
                {
                  "@class": "nx",
                  "#text": "value"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "getUint8"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "nx",
                  "#text": "i"
                },
                {
                  "@class": "p",
                  "#text": ")."
                },
                {
                  "@class": "nx",
                  "#text": "toString"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "mi",
                  "#text": "16"
                },
                {
                  "@class": "p",
                  "#text": "))."
                },
                {
                  "@class": "nx",
                  "#text": "slice"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "o",
                  "#text": "-"
                },
                {
                  "@class": "mi",
                  "#text": "2"
                },
                {
                  "@class": "p",
                  "#text": "));"
                },
                {
                  "@class": "p",
                  "#text": "}"
                },
                {
                  "@class": "nx",
                  "#text": "log"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1",
                  "#text": ">"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "o",
                  "#text": "+"
                },
                {
                  "@class": "nx",
                  "#text": "a"
                },
                {
                  "@class": "p",
                  "#text": "."
                },
                {
                  "@class": "nx",
                  "#text": "join"
                },
                {
                  "@class": "p",
                  "#text": "("
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "s1"
                },
                {
                  "@class": "dl",
                  "#text": "'"
                },
                {
                  "@class": "p",
                  "#text": "));"
                },
                {
                  "@class": "p",
                  "#text": "}"
                }
              ]
            }
          }
        }
      }
    }
  }
