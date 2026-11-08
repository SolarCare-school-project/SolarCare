# Data collection

**Status:** proposed

## Context

[Why SolarCare collects data, and who uses it: Solar System owner, support, management, development.]

## Decision

### Collected from the solar system

SolarCare subscribes only to the topics below. A topic it does not subscribe to never reaches the database.

https://www.opendtu.solar/firmware/mqtt_topics/#general-topics

| Topic | What it is | Seen by |
| --- | --- | --- |
| `[serial]/0/power` | Power the inverter produces now | Solar System owner, support engineer, software engineer, regional technician, support team lead |
| `[serial]/0/yieldday` | Energy produced today | Solar System owner, support engineer, software engineer, regional technician |
| `[serial]/0/yieldtotal` | Energy produced since the counter was last reset, in kWh | Solar System owner, support engineer, software engineer, regional technician |
| `[serial]/0/temperature` | Inverter temperature | Solar System owner, support engineer, software engineer, regional technician |
| `[serial]/status/reachable` | Whether the gateway can reach the inverter | Solar System owner, support engineer, software engineer, regional technician, support team lead |
| `[serial]/status/producing` | Whether the inverter is producing | Solar System owner, support engineer, software engineer, regional technician |
| `[serial]/status/last_update` | When the reading was measured | Solar System owner, support engineer, software engineer, support team lead |
| `[serial]/0/efficiency`, `[serial]/0/powerdc` | How much of the panel power the inverter turns into AC power | Support engineer, software engineer, regional technician |
| `[serial]/[1-4]/yieldday`, `[serial]/[1-4]/current` | Energy and current of each panel input | Support engineer, software engineer, regional technician |
| `[serial]/radio/rssi` | Radio signal between the gateway and the inverter | Support engineer, software engineer, regional technician |
| `dtu/status` | Whether the gateway itself is online | Support engineer, software engineer, support team lead |
| `dtu/rssi` | Wi-Fi signal strength of the gateway | Support engineer, software engineer |
| `dtu/uptime` | Time since the gateway last restarted | Support engineer, software engineer |
| `[serial]/status/limit_relative` | The power limit set on the inverter | Support engineer |
| `[serial]/device/fwbuildversion` | Inverter firmware version | Software engineer, regional technician |
| `[serial]/0/voltage`, `[serial]/0/frequency` | Grid voltage and frequency at the inverter | Regional technician |
| `[serial]/[1-4]/power`, `[serial]/[1-4]/voltage` | Power and voltage of each panel input | Regional technician |
| `[serial]/device/hwpartnumber`, `[serial]/device/hwversion` | Inverter part number and hardware version | Regional technician |

### Collected about the customer

| Data | Why it is needed | Who can see it |
| --- | --- | --- |
| Name | To know who the customer is | Solar System owner (own), support engineer and technician for the assigned incident, support team lead for the incident waiting for a decision |
| Email | Login and notifications | Solar System owner (own), support engineer for the assigned incident |
| Address | For the technician visit | Solar System owner (own), technician for the assigned incident |
| Phone number | So the technician can arrange the visit | Solar System owner (own), support engineer and technician for the assigned incident |
| Inverter serial numbers | To link readings to the right house | Support engineer, software engineer |

These details are stored in the SolarCare database so support and the technician can contact the Solar System owner. They are never written to logs. Logs hold IDs, not customer details. See [Logging](logging.md).


### Never collected

- Inverter commands under `cmd/`, such as power on, power off, or a new limit. SolarCare never controls the inverter.
- Gateway memory and the radio packet counters. They describe the gateway, not the solar system.
- `dtu/temperature`. It is the temperature of the gateway board, not the inverter.
- The `ac/` and `dc/` totals across all inverters. SolarCare keeps the reading for each inverter.
- `[serial]/0/current`, `[serial]/0/powerfactor`, and `[serial]/0/reactivepower`. They are grid details for an electrician on site.
- `[serial]/status/limit_absolute`. It repeats `limit_relative` in watts.

OpenDTU does not publish inverter error codes over MQTT, so SolarCare cannot read fault codes this way.


### Retention

[How long each kind of data is kept.]


## Consequences

- [What this makes possible.]
- [Privacy and GDPR impact.]

## Open questions

- How often should readings be collected?
- How long should measurements be kept?
- What the fuller log contains is described in [Logging](logging.md).
- What does the website collect from the browser, such as cookies or usage analytics?
- Should the regional technician see last update, so they know how old a reading is?
- Where do the customer details come from: the Solar System owner registers in SolarCare, the regional company sends them, or another company system?
- Should company management see anonymised totals across all homes?
- Which solar equipment will SolarCare support? OpenDTU is only a candidat for compatible Hoymiles inverters.
