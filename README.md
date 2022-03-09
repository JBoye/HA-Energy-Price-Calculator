# WIP HA-Nordpool-Calculator

## Configuration 

### sensor_name

The entity_id of your Nordpool Sensor. 
Examples:
```
{% set sensor_name = "sensor.elpris" %}
```
```
{% set sensor_name = "sensor.nordpool_kwh_dk1_dkk_2_00_025" %}
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
{% set duration = timedelta(hours = timedelta(hours = 10 / consumption * states("sensor.car_battery_level") | float)) %}
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
