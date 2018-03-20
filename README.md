# Tiles custom state card for [Home Assistant](https://home-assistant.io)

![preview](https://raw.githubusercontent.com/c727/home-assistant-tiles/master/docs/preview.png)

## Requirements
- [Home Assistant 0.66+](https://home-assistant.io/)
- [Modern web browser](https://caniuse.com/#feat=css-grid)

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
```yaml
homeassistant:
  customize:
    input_boolean.dummy_tiles:
      custom_ui_state_card: state-card-tiles
      config:
        {{ grid config }}
        entities:
          - entity: input_boolean.switch1
            {{ entity config }}

input_boolean:
  dummy_tiles:
  switch1:
 ```

### Grid config
Option | Value | Default | Description
--- | --- | --- | ---
columns | (integer) \| auto-fit | 3 | number of columns
column_width | (float)(CSS unit) | 1fr | width for each column, >=30px
row_height | (float)(CSS unit) | 100px | height for each row, >=30px
gap | (float)(CSS unit) | 4px | gap between columns and rows
color | (CSS color) | var(--primary-color) | color for none-toggle tiles
color_on | (CSS color) | var(--google-green-500) | on color for toggle tiles
color_off | (CSS color) | var(--google-red-500) | off color for toggle tiles
text_color | (CSS color) | #FFF | text + icon color for none-toggle tiles
text_color_on | (CSS color) | (text_color) | on text + icon color for toggle tiles
text_color_off | (CSS color) | (text_color) | off + icon text color for toggle tiles
text_size | (float)(CSS unit) | 1em | text size
text_align | center \| left \| right | center | text align
text_uppercase | true \| false | true | uppercase text
text_sec_color | (CSS color) | (text_color) | secondary text color for none-toggle tiles
text_sec_color_on | (CSS color) | (text_color_on) | on secondary text color for toggle tiles
text_sec_color_off | (CSS color) | (text_color_off) | off secondary text color for toggle tiles
text_sec_size | (float)(CSS unit) | 1em | secondary text size
icon_size | (float)(CSS unit) | 24px | icon size

### Entity config
Option | Value | Default | Description
--- | --- | --- | ---
entity | (HA entity_id) | **REQUIRED** | Home Assistant enity_id
column | (integer) | auto | column position
column_span | (integer) | 1 | column span
row | (integer) | auto | row position
row_span | (integer) | 1 | row span
more_info | (HA entity_id) | - | default behaviour for non-toggles, show more-info-card instead of toggle action
service | (domain).(service) | default | use a custom service for the action
data | (JSON) \| (YAML) | { entity_id: entity_id } | service data
color | (CSS color) | (inherit) | color for none-toggle tiles
color_on | (CSS color) | (inherit) | on color for toggle tiles
color_off | (CSS color) | (inherit) | off color for toggle tiles
icon | (set):(icon) | - | icon
icon_template | (JavaScript) | - | JavaScript, return set:icon
icon_size | (float)(CSS unit) | (inherit) | icon size
label | (string) | - | primary label
label_state | (HA entity_id) | - | label text from entity state
label_template | (JavaScript) | - | JavaScript, return a string
text_color | (CSS color) | (inherit)| text color + icon for none-toggle tiles
text_color_on | (CSS color) | (inherit) | on text + icon color for toggle tiles
text_color_off | (CSS color) | (inherit) | off text + icon color for toggle tiles
text_size | (float)(CSS unit) | (inherit) | text size
text_align | center \| left \| right | (inherit) | text align
text_uppercase | true \| false | (inherit) | uppercase text
label_sec | (string) | - | secondary label
label_sec_state | (HA entity_id) | - | label text from entity state
label_sec_template | (JavaScript) | - | JavaScript, return a string
text_sec_color | (CSS color) | (inherit) | secondary text color for none-toggle tiles
text_sec_color_on | (CSS color) | (inherit) | on secondary text color for toggle tiles
text_sec_color_off | (CSS color) | (inherit) | off secondary text color for toggle tiles
text_sec_size | (float)(CSS unit) | (inherit) | secondary text size
style_template | (JavaScript) | - | JavaScript, return CSS code

Also check the example configuration.

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
      data_template:
        title: Update available
        message: "A new version for [Tiles custom state card](https://github.com/c727/home-assistant-tiles#changelog) is available: {{ states('sensor.sc_tiles_latest_version') }}"
        notification_id: tiles_update_info
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
For more panel features check the example in the [configuration file](https://github.com/c727/home-assistant-tiles/blob/master/configuration.yaml#L172).

![panel_multi](https://raw.githubusercontent.com/c727/home-assistant-tiles/master/docs/panel_multi.png)

## Templates
JavaScript functions to compute custom values. Examples:
```yaml
# label based on state
label_template: "if (state < 10) return '<10'; else return '>=10';"

# icon based on state
icon_template: "if (state === 'on') return 'mdi:power-plug'; else return 'mdi:power-plug-off'"

# background image based on sate
style_template: "return 'background-image: url(\"/local/' + state + '.png\");'"
```

Variable | Description | Example
--- | --- | ---
state | state of the entity | state
attributes | attribute of the entity | attributes['brightness']
entities | state of another entity | entities['input_boolean.switch1'].state
entities | attribute of another entity | entities['light.floor1'].attributes.brightness

![templates](https://raw.githubusercontent.com/c727/home-assistant-tiles/master/docs/templates.png)

## Changelog
Version: 20180320
```
-fixed text colors
-added service to entity config to run any other service like 'remote.send_command' (in combination with data:)
-code improvements
```
Version: 20180319
```
-added text_sec_color, text_sec_color_on, text_sec_color_off
-fix for version code
```
Version: 20180314.1
```
-removed text_sec_color
```
Version: 20180314
```
-added tiles version to dev info panel (HA 0.66+)
-added device_tracker to sensors
-changed minimum tile size to 30px
-added text_uppercase
```
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
