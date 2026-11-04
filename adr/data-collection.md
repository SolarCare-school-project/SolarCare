# Data collection

**Status:** proposed

## Context

[Why SolarCare collects data, and who uses it: house owner, support, management, development.]

## Decision

### Collected from the solar system

SolarCare subscribes only to the topics below. A topic it does not subscribe to never reaches the database.

https://www.opendtu.solar/firmware/mqtt_topics/#general-topics

| Topic | What it is | Seen by |
| --- | --- | --- |
| `[serial]/0/power` | Power the inverter produces now | House owner, support engineer, software engineer, regional technician, support team lead |
| `[serial]/0/yieldday` | Energy produced today | House owner, support engineer, software engineer, regional technician |
| `[serial]/0/yieldtotal` | Energy produced since the inverter was installed | House owner, support engineer, software engineer, regional technician |
| `[serial]/0/temperature` | Inverter temperature | House owner, support engineer, software engineer, regional technician |
| `[serial]/status/reachable` | Whether the gateway can reach the inverter | House owner, support engineer, software engineer, regional technician, support team lead |
| `[serial]/status/producing` | Whether the inverter is producing | House owner, support engineer, software engineer, regional technician |
| `[serial]/status/last_update` | When the reading was measured | House owner, support engineer, software engineer, support team lead |
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
| | | |

GDPR?

### Never collected

- [Data SolarCare will not collect, and why.]

GDPR?

### Retention

[How long each kind of data is kept.]

GDPR?

## Consequences

- [What this makes possible.]
- [Privacy and GDPR impact.]

## Open questions

## Open questions

- How often should readings be collected?
- How long should measurements be kept?
- What does the fuller log contain?
- What does the website collect from the browser, such as cookies or usage analytics?
- Should the regional technician see last update, so they know how old a reading is?
- Where do the customer details come from: the house owner registers in SolarCare, the regional company sends them, or another company system?
- Should company management see anonymised totals across all homes?
- Which solar equipment will SolarCare support? OpenDTU is only a candidat for compatible Hoymiles inverters.
