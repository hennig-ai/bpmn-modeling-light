# BPMN Schema Inheritance Hierarchy

## Inheritance Relationships (bpmn_element Hierarchie)

Alle Tabellen in dieser Hierarchie referenzieren `bpmn_element.bpmn_element_id` (direkt oder indirekt).

| parent | child | element_type verwendbar? |
|--------|-------|--------------------------|
| bpmn_element | activity | NEIN (immer Subtyp verwenden) |
| bpmn_element | event | JA |
| bpmn_element | gateway | JA |
| bpmn_element | sequence_flow | JA |
| bpmn_element | message_flow | JA |
| bpmn_element | association | JA |
| bpmn_element | pool | JA |
| bpmn_element | lane | JA |
| bpmn_element | data_object | JA |
| bpmn_element | data_store | JA |
| bpmn_element | text_annotation | JA |
| bpmn_element | data_input | JA |
| bpmn_element | data_output | JA |
| bpmn_element | data_association | JA |
| activity | task | NEIN (immer Subtyp verwenden) |
| activity | subprocess | JA |
| activity | call_activity | JA |
| task | service_task | JA |
| task | user_task | JA |
| task | script_task | JA |
| task | business_rule_task | JA |