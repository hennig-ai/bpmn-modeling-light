# Event Detail Tables (NOT bpmn_elements!)

These tables extend `event` via `event_id`, but have **NO own `bpmn_element_id`**.
Therefore they are **NOT valid `element_type` values**.

| parent | child | specialization via |
|--------|-------|-------------------|
| event | message_event_definition | event.event_definition_type='message' |
| event | timer_event_definition | event.event_definition_type='timer' |
| event | error_event_definition | event.event_definition_type='error' |
| event | signal_event_definition | event.event_definition_type='signal' |
