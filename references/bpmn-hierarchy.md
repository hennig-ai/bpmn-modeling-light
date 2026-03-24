# BPMN Schema Inheritance Hierarchy

## Inheritance Relationships (bpmn_element Hierarchy)

All tables in this hierarchy reference `bpmn_element.bpmn_element_id` (directly or indirectly).

| parent | child | valid as element_type? |
|--------|-------|------------------------|
| bpmn_element | activity | NO (always use subtype) |
| bpmn_element | event | YES |
| bpmn_element | gateway | YES |
| bpmn_element | sequence_flow | YES |
| bpmn_element | message_flow | YES |
| bpmn_element | association | YES |
| bpmn_element | pool | YES |
| bpmn_element | lane | YES |
| bpmn_element | data_object | YES |
| bpmn_element | data_store | YES |
| bpmn_element | text_annotation | YES |
| bpmn_element | data_input | YES |
| bpmn_element | data_output | YES |
| bpmn_element | data_association | YES |
| activity | task | NO (always use subtype) |
| activity | subprocess | YES |
| activity | call_activity | YES |
| task | service_task | YES |
| task | user_task | YES |
| task | script_task | YES |
| task | business_rule_task | YES |
