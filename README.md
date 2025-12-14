# Ultimate Fan Project - The 3-Speed Fan Convert Part:2

Using a i2C port expander (**MCP23008**) to add back local control of a 3-Speed Desk Fan and also, 3 LEDs for current speed indication.
Basic wiring example: https://cdn-learn.adafruit.com/assets/assets/000/063/923/medium800/circuitpython_FeatherM0_MCP23008_LED_Button_bb.jpg

UK Source for MCP23008, RS: https://uk.rs-online.com/web/p/i-o-expanders/0403563

ESPHome: Info: https://esphome.io/components/mcp230xx.html

Watch the Live Stream here: https://youtu.be/AIuAeBEvEG4

--- Full edited Tutorial coming soon ---

EspHome Code. Note the use of substitutions for names and entities
```yaml
esphome:
  name: ${name}
  platform: ESP8266
  board: esp01_1m

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

substitutions:
  name: fan_big #Use the same name you called the device: E.G. "fan_big.yaml"

logger:
api:
ota:
  platform: esphome

mcp23008:
  - id: "mcp23008_hub"
    address: 0x20

i2c:
  sda: GPIO1 #Green wire on Tx
  scl: GPIO3 #Blue wire on Rx
  scan: true

binary_sensor:
  - platform: homeassistant
    entity_id: input_boolean.${name}
    id: ${name}
    on_press:
      then:
        switch.turn_on: led3
    on_release:
      then:
        - switch.turn_off: led3
        - switch.turn_off: led3
        - switch.turn_off: led3

  - platform: gpio
    id: button1
    pin:
      mcp23008: mcp23008_hub
      number: 0
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - switch.toggle: led1

  - platform: gpio
    id: button2
    pin:
      mcp23008: mcp23008_hub
      number: 1
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - switch.toggle: led2

  - platform: gpio
    id: button3
    pin:
      mcp23008: mcp23008_hub
      number: 2
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      then:
        - switch.toggle: led3

switch:
  - platform: gpio
    id: relay1
    pin: GPIO12

  - platform: gpio
    name: ${name} 1
    id: led1
    icon: mdi:fan
    pin:
      mcp23008: mcp23008_hub
      number: 5
      mode: OUTPUT
      inverted: true
    interlock: &interlock_group [led1, led2, led3]
    interlock_wait_time: 300ms
    on_turn_on:
      - switch.turn_on: relay1
    on_turn_off:
      - switch.turn_off: relay1

  - platform: gpio
    name: ${name} 2
    id: led2
    icon: mdi:fan
    pin:
      mcp23008: mcp23008_hub
      number: 6
      mode: OUTPUT
      inverted: true
    interlock: *interlock_group
    interlock_wait_time: 300ms

  - platform: gpio
    name: ${name} 3
    id: led3
    icon: mdi:fan
    pin:
      mcp23008: mcp23008_hub
      number: 7
      mode: OUTPUT
      inverted: true
    interlock: *interlock_group
    interlock_wait_time: 300ms

status_led:
  pin:
    number: GPIO13
    inverted: yes
```

---

<div align="center">

### 💖 Support This Project

Found this useful? Want to say thanks and fuel future creations?

**Your support keeps the creativity flowing!** 🍺✨

<table>
  <tr>
    <td align="center">
      <a href="https://www.buymeacoffee.com/3ative">
        <img src="https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Support-yellow.svg?style=for-the-badge&logo=buy-me-a-coffee&logoColor=white" alt="Buy Me A Coffee"/>
        <br/>
        <b>☕ Buy me a Coffee</b>
      </a>
    </td>
    <td align="center">
      <a href="https://www.patreon.com/3ative">
        <img src="https://img.shields.io/badge/Patreon-Become%20a%20Patron-red.svg?style=for-the-badge&logo=patreon&logoColor=white" alt="Patreon"/>
        <br/>
        <b>💖 Join on Patreon</b>
      </a>
    </td>
  </tr>
</table>

**Every contribution helps bring more awesome projects to life!** 🚀

</div>

---
