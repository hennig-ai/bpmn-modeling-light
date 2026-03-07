# Event Detail-Tabellen (KEINE bpmn_elements!)

Diese Tabellen erweitern `event` über `event_id`, haben aber **KEINE eigene `bpmn_element_id`**.
Sie sind daher **KEINE gültigen `element_type` Werte**.

| parent | child | Spezialisierung über |
|--------|-------|---------------------|
| event | message_event_definition | event.event_definition_type='message' |
| event | timer_event_definition | event.event_definition_type='timer' |
| event | error_event_definition | event.event_definition_type='error' |
| event | signal_event_definition | event.event_definition_type='signal' |
