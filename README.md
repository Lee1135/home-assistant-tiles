# Tiles custom state card for [Home Assistant](https://home-assistant.io)

![preview](https://raw.githubusercontent.com/c727/home-assistant-tiles/master/docs/preview.png)

## Requirements
[Modern web browser](https://caniuse.com/#feat=css-grid)

## Installation
* Download `/www/custom_ui/state-card-tiles.html` and `/www/custom_ui/state-card-tiles_es5.html` to `<config-dir>/www/custom_ui/`
* Add it to your `configuration.yaml`:
```yaml
frontend:
  extra_html_url:
    - /local/custom_ui/state-card-tiles.html
  extra_html_url_es5:
    - /local/custom_ui/state-card-tiles_es5.html
```

## Configuration
All customization keys are optional.
```yaml
homeassistant:
  customize:
    input_text.dummy_tiles:
      custom_ui_state_card: state-card-tiles
      config:
        columns: 3
        column_width: 75px
        row_height: 75px
        gap: 4px
        color: blue
        color_on: green
        color_off: red
        text_color: "#FFF"
        text_color_on: "#FFF"
        text_color_off: "#FFF"
        text_size: 1em
        text_sec_color: "#FFF"
        text_sec_size: 1em
        text_align: center | left | right
        icon_size: 24px
        entities:
          - entity: input_boolean.switch1 # required
            data: {value: right} # service data for scripts
            more_info: histroy_grah.temperature # fire more-info instead of service, default for sensors
            icon: mdi:power
            icon_template: "if (state === 'on') return 'mdi:volume-high'; else return 'mdi:volume-low'" # JavaScript
            label: S1
            label_state: input_boolean.switch2   
            label_template: "if (state < 10) return '<10'; else return '>=10';" # JavaScript
            label_sec: (Switch 1)
            label_sec_state: input_boolean.switch2
            label_sec_template: "if (state < 10) return '<10'; else return '>=10';" # JavaScript
            column: 2
            column_span: 2
            row: 2
            row_span: 2
            image: /local/test.png
            style_template: "if (state === 'on') return 'background-image: url(\"/local/on.png\");'; else return 'background-image: url(\"/local/off.png\");'" # JavaScript, return CSS here
            color: blue
            color_on: green
            color_off: red
            text_color: "#FFF"
            text_color_on: "#FFF"
            text_color_off: "#FFF"
            text_size: 1em
            text_sec_color: "#FFF"
            text_sec_size: 1em
            text_align: center
            icon_size: 24px
 ```
 
Also check the sample configuration.

## Update info
```yaml
sensor:
  - platform: rest
    name: SC Tiles latest version
    resource: https://raw.githubusercontent.com/c727/home-assistant-tiles/master/LATEST_VERSION

automation:
  - alias: SC Tiles update info
    trigger:
      platform: state
      entity_id: sensor.sc_tiles_latest_version
    action:
      service: persistent_notification.create
      data:
        title: Update available
        message: A new version for [Tiles custom state card](https://github.com/c727/home-assistant-tiles#changelog) is available.
```

## Run as panel
* Also download `/panels/tiles.html` to `<config-dir>/panels/tiles.html`
* Add it to your `configuration.yaml`:
```yaml
panel_custom:
  - name: tiles
    sidebar_title: Tiles
    sidebar_icon: mdi:view-dashboard
    url_path: tiles
    config:
      entities:
        - entity: input_boolean.switch1
          label: Switch 1
```

## Using templates
`state` state of the entity

`attributes['brightness']` attribute of the entity

`entities['input_boolean.switch1'].state` state of another entity

`entities['light.floor1'].attributes.brightness` attribute of another entity

## Changelog
Version: 20180308
```
-fix dialog for old HA releases
```
Version: 20180228
```
-fixed on-click
```
Version: 20180226.1
```
-added scene to stateless domains
```
Version: 20180226
```
-added binary_sensor and scene support
-every domain can use more_info now
```
Version: 20180221
```
-added icon_template and style_template
-fixed label_sec_template
```
Version: 20180219
```
-added support for sensor domain: label = state, tap shows more-info-card
-added more_info to define another entity's card when using sensor domain
-label_template: JS function with sate, attributes[] and entities[] as parameters
-added label_sec, label_sec_state, label_sec_template: shows a 2nd label
-added text_size, text_sec_color, text_sec_size, text_align, icon_size (global + per entity)
```
Version 20180209.1
```
-changed minimum button size to 50px x 50 px
```
Version 20180209:
```
-added support for more domains
```
Version 20180204:
```
-added support for python scripts
```
Version 20180120:
```
-added ES5 and ES6 version
-changed default column width to 1fr
-changed background image to no-repeat and center
-added color, color_on, color_off (global + per entity)
-added text_color, text_color_on, text_color_off (global + per entity)
-added label_state: state from any entity_id as label text
```
