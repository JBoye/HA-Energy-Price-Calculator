# Energy Price Calculator
## Installation

1. Setup packages, according to https://www.home-assistant.io/docs/configuration/packages/
2. Add `calculator.yaml` to the packages/ directory
3. Remove the first line `energy_calculator:` in calculator.yaml
4. Reload Home Assistant
5. Go to Helpers and set the new variables. Hint: Search for `calculator`

## Configuration 

### sensor_name

The entity_id of your [EnergiDataService](https://github.com/MTrab/energidataservice)  Sensor. 
Examples:

```
{% set sensor_name = "sensor.elpris" %}
```

```
{% set sensor_name = "sensor.energi_data_service" %}
```

### consumption

The amount of kW used. 
Examples:

Hybrid Electric car charging at 230 volt * 16 amp
```
{% set consumption = 230 * 16 / 1000 %}
```

Washing machine using 1500 watts
```
{% set consumption = 1500 / 1000 %}
```
	

### duration

How long will it run for. 
Examples:

Hybrid Electric car charging 10 kWh battery: (Assuming sensor.car_battery_level is the battery level %)
```
{% set duration = timedelta(hours = (10 / consumption) * (1 - (states("sensor.car_battery_level") | float / 100))) %}
```

Electric car charging 75 kWh battery up to 80%: (Assuming sensor.car_battery_level is the battery level %)
```
{% set duration = timedelta(hours = 0.8 * (75 / consumption) * (1 - (states("sensor.car_battery_level") | float / 100))) %}
```

Washing machine running 2.5 hours
```
{% set duration = timedelta(hours = 2, minutes = 30) %}
```

### start_time 

First possible time to start. Typically:
```
{% set start_time = now() %} 
```

To show all values use:
```
{% set start_time = today_at("00:00") %} 
```

### end_time

Deadline for when the operation should be finished. (For example departure time). 

Maximum 
```
{% set end_time = today_at("00:00") + timedelta(days=2) %} 
```

Tomorrow at 7:30
```
{% set end_time = today_at("7:30") + timedelta(days=1) %} 
```

In 12 hours:
```
{% set end_time = now() + timedelta(hours=12) %} 
```


## Sample Card (in danish)
```
type: vertical-stack
cards:
  - type: entity
    entity: sensor.price_calculator_cheapest_time
    attribute: countdown
    name: 'Start tid:'
    icon: mdi:clock-digital
  - type: entities
    entities:
      - entity: binary_sensor.price_calculator_is_cheapest
        name: Billigst nu
        icon: mdi:currency-usd
      - entity: sensor.price_calculator_price_now
        icon: mdi:cash-100
        name: Pris nu
      - entity: sensor.price_calculator_cheapest_price
        name: Billigste pris
        icon: mdi:cash-100
      - entity: sensor.price_calculator_cheapest_time
        name: Billigste tidspunkt
        icon: mdi:clock
  - type: entities
    entities:
      - entity: input_datetime.calculator_end_time
      - entity: input_datetime.calculator_duration
      - entity: input_number.calculator_kilowatt
        name: Forbrug kW
        icon: mdi:lightning-bolt
    title: Indstilling

```
