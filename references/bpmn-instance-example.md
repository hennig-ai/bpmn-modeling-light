# BPMN Integer Schema - Corrected Minimal Example

## bpmn_model
| bpmn_model_id | name | version | author | creation_date | modification_date | documentation |
|---------------|------|---------|--------|---------------|-------------------|---------------|
| 20251026_1730_56_001 | Simple Invoice Model | 1.0 | AI System | 2025-05-24T10:00:00Z | 2025-05-24T10:00:00Z | Demonstration model for invoice handling showing all BPMN element types |

## bpmn_process
| bpmn_process_id | bpmn_model_id | name | is_executable | documentation |
|-----------------|---------------|------|---------------|---------------|
| 001 | 20251026_1730_56_001 | Simple Invoice Process | true | Main process for invoice handling |

## bpmn_element
| bpmn_element_id | name | documentation | element_type |
|-----------------|------|---------------|--------------|
| 001 | Process Start | Invoice processing starts | event |
| 002 | Message Start | Alternative start via message | event |
| 003 | Review Invoice | User reviews the invoice | user_task |
| 004 | Invoice Validation | Subprocess for validation | subprocess |
| 005 | Amount Check | Checks invoice amount | gateway |
| 006 | Auto Process | Automated processing for small amounts | service_task |
| 007 | Calculate Total | Script to calculate totals | script_task |
| 008 | Apply Rules | Business rule application | business_rule_task |
| 009 | External Check | Calls external validation service | call_activity |
| 010 | Approval Gateway | Merges approval paths | gateway |
| 011 | Process End | Invoice processing completed | event |
| 012 | Error End | Process ends with error | event |
| 013 | Start to Review | Flow from start to review task | sequence_flow |
| 014 | Message to Review | Flow from message start to review | sequence_flow |
| 015 | Review to Validation | Flow to validation subprocess | sequence_flow |
| 016 | Validation to Gateway | Flow to amount check | sequence_flow |
| 017 | Small Amount Flow | Flow for amounts < 1000 | sequence_flow |
| 018 | Large Amount Flow | Flow for amounts >= 1000 | sequence_flow |
| 019 | Auto to Calculate | Flow to calculation | sequence_flow |
| 020 | Calculate to Rules | Flow to business rules | sequence_flow |
| 021 | Rules to Merge | Flow to merge gateway | sequence_flow |
| 022 | External to Merge | Flow from external check | sequence_flow |
| 023 | Merge to End | Flow to process end | sequence_flow |
| 024 | Error Flow | Flow to error end | sequence_flow |
| 025 | Invoice Department | Department handling invoices | pool |
| 026 | Finance Team | Finance team lane | lane |
| 027 | Management | Management lane | lane |
| 028 | Invoice Document | Invoice data object | data_object |
| 029 | Invoice Archive | Storage for processed invoices | data_store |
| 030 | Processing Note | Important processing information | text_annotation |
| 031 | Invoice Input | Input invoice data | data_input |
| 032 | Processed Invoice | Output processed invoice | data_output |
| 033 | Input Association | Associates input with review task | data_association |
| 034 | Output Association | Associates output with process | data_association |
| 035 | Note Association | Links annotation to gateway | association |
| 036 | Status Update | Message flow for status | message_flow |
| 037 | Timeout Timer | Timer for process timeout | event |
| 038 | Completion Signal | Signal when process completes | event |
| 039 | Standard Loop Task | Task with standard loop | user_task |
| 040 | MI Sequential Task | Task with sequential multi-instance loop | script_task |
| 041 | MI Parallel Task | Task with parallel multi-instance loop | service_task |
| 042 | Message Catch | Intermediate catch for message | event |
| 043 | Conditional Catch | Intermediate catch for condition | event |
| 044 | Escalation Catch | Intermediate catch for escalation | event |
| 045 | Compensation Catch | Intermediate catch for compensation | event |
| 046 | Link Catch | Intermediate catch for link | event |
| 047 | Terminate End | Process terminates immediately | event |
| 048 | Inclusive Split | Inclusive gateway | gateway |
| 049 | Parallel Split | Parallel gateway | gateway |
| 050 | Event-Based GW | Event-based gateway | gateway |
| 051 | Complex GW | Complex gateway | gateway |

## message_definition
| message_definition_id | name | item_id |
|----------------------|------|---------|
| 001 | Invoice Received | item_1 |
| 002 | Status Update Message | item_2 |

## signal_definition
| signal_definition_id | name | signal_type |
|---------------------|------|-------------|
| 001 | Process Completed | public |

## error_definition
| error_definition_id | name | error_code | structure_id |
|--------------------|------|------------|--------------|
| 001 | Validation Failed | ERR-001 | struct_1 |

## process_element
| process_element_id | bpmn_process_id | bpmn_element_id |
|-------------------|-----------------|-----------------|
| 001 | 001 | 001 |
| 002 | 001 | 002 |
| 003 | 001 | 003 |
| 004 | 001 | 004 |
| 005 | 001 | 005 |
| 006 | 001 | 006 |
| 007 | 001 | 007 |
| 008 | 001 | 008 |
| 009 | 001 | 009 |
| 010 | 001 | 010 |
| 011 | 001 | 011 |
| 012 | 001 | 012 |
| 013 | 001 | 013 |
| 014 | 001 | 014 |
| 015 | 001 | 015 |
| 016 | 001 | 016 |
| 017 | 001 | 017 |
| 018 | 001 | 018 |
| 019 | 001 | 019 |
| 020 | 001 | 020 |
| 021 | 001 | 021 |
| 022 | 001 | 022 |
| 023 | 001 | 023 |
| 024 | 001 | 024 |
| 025 | 001 | 025 |
| 026 | 001 | 026 |
| 027 | 001 | 027 |
| 028 | 001 | 028 |
| 029 | 001 | 029 |
| 030 | 001 | 030 |
| 031 | 001 | 031 |
| 032 | 001 | 032 |
| 033 | 001 | 033 |
| 034 | 001 | 034 |
| 035 | 001 | 035 |
| 036 | 001 | 036 |
| 037 | 001 | 037 |
| 038 | 001 | 038 |
| 039 | 001 | 039 |
| 040 | 001 | 040 |
| 041 | 001 | 041 |
| 042 | 001 | 042 |
| 043 | 001 | 043 |
| 044 | 001 | 044 |
| 045 | 001 | 045 |
| 046 | 001 | 046 |
| 047 | 001 | 047 |
| 048 | 001 | 048 |
| 049 | 001 | 049 |
| 050 | 001 | 050 |
| 051 | 001 | 051 |

## activity
| activity_id | bpmn_element_id | activity_type | is_multi_instance | loop_type | is_ad_hoc | is_compensation | start_quantity | completion_quantity |
|-------------|-----------------|---------------|-------------------|-----------|-----------|-----------------|----------------|---------------------|
| 001 | 003 | task | false | none | false | false | 1 | 1 |
| 002 | 004 | subprocess | false | none | false | false | 1 | 1 |
| 003 | 006 | task | false | none | false | false | 1 | 1 |
| 004 | 007 | task | false | none | false | false | 1 | 1 |
| 005 | 008 | task | false | none | false | false | 1 | 1 |
| 006 | 009 | call_activity | false | none | false | false | 1 | 1 |
| 007 | 039 | task | false | standard | false | false | 1 | 1 |
| 008 | 040 | task | true | multi_instance_sequential | false | false | 1 | 1 |
| 009 | 041 | task | true | multi_instance_parallel | false | false | 1 | 1 |

## task
| task_id | activity_id | task_type | implementation |
|---------|-------------|-----------|----------------|
| 001 | 001 | user | ##unspecified |
| 002 | 003 | service | webservice |
| 003 | 004 | script | ##unspecified |
| 004 | 005 | business_rule | ##unspecified |
| 005 | 007 | user | ##unspecified |
| 006 | 008 | script | ##unspecified |
| 007 | 009 | service | ##unspecified |

## service_task
| service_task_id | task_id | operation_id | implementation_id |
|-----------------|---------|--------------|-------------------|
| 001 | 002 | operation_1 | com.example.InvoiceService |
| 002 | 007 | operation_2 | com.example.LoopService |

## user_task
| user_task_id | task_id | implementation | assignment_expression |
|--------------|---------|----------------|----------------------|
| 001 | 001 | webform | ${role == 'finance-clerk'} |
| 002 | 005 | webform | ${role == 'manager'} |

## script_task
| script_task_id | task_id | script | script_format |
|----------------|---------|--------|---------------|
| 001 | 003 | total = amount * (1 + taxRate); return total; | javascript |
| 002 | 006 | for item in items: process(item) | python |

## business_rule_task
| business_rule_task_id | task_id | implementation | rule_names |
|----------------------|---------|----------------|------------|
| 001 | 004 | dmn | invoice_approval_rules,discount_rules |

## subprocess
| subprocess_id | activity_id | is_transaction | triggered_by_event |
|---------------|-------------|----------------|-------------------|
| 001 | 002 | false | false |

## call_activity
| call_activity_id | activity_id | bpmn_process_id_reference |
|------------------|-------------|---------------------------|
| 001 | 006 | 001 |

## event
| event_id | bpmn_element_id | event_type | event_definition_type | is_interrupting |
|----------|-----------------|------------|----------------------|-----------------|
| 001 | 001 | start | none | ##!empty!## |
| 002 | 002 | start | message | ##!empty!## |
| 003 | 011 | end | none | ##!empty!## |
| 004 | 012 | end | error | ##!empty!## |
| 005 | 037 | boundary | timer | true |
| 006 | 038 | intermediate_throw | signal | ##!empty!## |
| 007 | 042 | intermediate_catch | message | ##!empty!## |
| 008 | 043 | intermediate_catch | conditional | ##!empty!## |
| 009 | 044 | intermediate_catch | escalation | ##!empty!## |
| 010 | 045 | intermediate_catch | compensation | ##!empty!## |
| 011 | 046 | intermediate_catch | link | ##!empty!## |
| 012 | 047 | end | terminate | ##!empty!## |

## message_event_definition
| message_event_definition_id | event_id | message_definition_id | operation_id |
|----------------------------|----------|----------------------|--------------|
| 001 | 002 | 001 | ##!empty!## |
| 002 | 007 | 001 | ##!empty!## |

## timer_event_definition
| timer_event_definition_id | event_id | time_date | time_duration | time_cycle |
|--------------------------|----------|-----------|---------------|------------|
| 001 | 005 | ##!empty!## | PT2H | ##!empty!## |

## error_event_definition
| error_event_definition_id | event_id | error_definition_id |
|--------------------------|----------|---------------------|
| 001 | 004 | 001 |

## signal_event_definition
| signal_event_definition_id | event_id | signal_definition_id |
|---------------------------|----------|---------------------|
| 001 | 006 | 001 |

## sequence_flow
| sequence_flow_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | is_default | condition_expression |
|------------------|-----------------|------------------------|------------------------|------------|---------------------|
| 001 | 013 | 001 | 003 | false | ##!empty!## |
| 002 | 014 | 002 | 003 | false | ##!empty!## |
| 003 | 015 | 003 | 004 | false | ##!empty!## |
| 004 | 016 | 004 | 005 | false | ##!empty!## |
| 005 | 017 | 005 | 006 | false | ${amount < 1000} |
| 006 | 018 | 005 | 009 | false | ${amount >= 1000} |
| 007 | 019 | 006 | 007 | false | ##!empty!## |
| 008 | 020 | 007 | 008 | false | ##!empty!## |
| 009 | 021 | 008 | 010 | false | ##!empty!## |
| 010 | 022 | 009 | 010 | false | ##!empty!## |
| 011 | 023 | 010 | 011 | false | ##!empty!## |
| 012 | 024 | 004 | 012 | false | ${validationFailed} |

## gateway
| gateway_id | bpmn_element_id | gateway_type | gateway_direction | sequence_flow_id |
|------------|-----------------|--------------|-------------------|------------------|
| 001 | 005 | exclusive | diverging | ##!empty!## |
| 002 | 010 | exclusive | converging | ##!empty!## |
| 003 | 048 | inclusive | diverging | ##!empty!## |
| 004 | 049 | parallel | diverging | ##!empty!## |
| 005 | 050 | event_based | diverging | ##!empty!## |
| 006 | 051 | complex | diverging | ##!empty!## |

## message_flow
| message_flow_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | message_definition_id |
|-----------------|-----------------|------------------------|------------------------|-----------------------|
| 001 | 036 | 003 | 025 | 002 |

## association
| association_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | association_direction |
|----------------|-----------------|------------------------|------------------------|-----------------------|
| 001 | 035 | 030 | 005 | none |

## pool
| pool_id | bpmn_element_id | bpmn_process_id | is_closed |
|---------|-----------------|-----------------|-----------|
| 001 | 025 | 001 | false |

## lane
| lane_id | bpmn_element_id | pool_id |
|---------|-----------------|---------|
| 001 | 026 | 025 |
| 002 | 027 | 025 |

## lane_element
| lane_element_id | lane_bpmn_element_id | bpmn_element_id |
|-----------------|---------------------|-----------------|
| 001 | 026 | 003 |
| 002 | 026 | 006 |
| 003 | 026 | 007 |
| 004 | 026 | 008 |
| 005 | 027 | 009 |

## data_object
| data_object_id | bpmn_element_id | is_collection | state |
|----------------|-----------------|---------------|-------|
| 001 | 028 | false | received |

## data_store
| data_store_id | bpmn_element_id | capacity | is_unlimited |
|---------------|-----------------|----------|--------------|
| 001 | 029 | 10000 | false |

## text_annotation
| text_annotation_id | bpmn_element_id | text |
|-------------------|-----------------|------|
| 001 | 030 | Check amount threshold: < 1000 = automatic, >= 1000 = manual approval |

## data_input
| data_input_id | bpmn_element_id | name | is_collection |
|---------------|-----------------|------|---------------|
| 001 | 031 | Invoice Document | false |

## data_output
| data_output_id | bpmn_element_id | name | is_collection |
|----------------|-----------------|------|---------------|
| 001 | 032 | Approved Invoice | false |

## data_association
| data_association_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | transformation_expression | assignment_expression |
|--------------------|-----------------|------------------------|------------------------|---------------------------|----------------------|
| 001 | 033 | 031 | 003 | ##!empty!## | ${invoice = input} |
| 002 | 034 | 011 | 032 | ##!empty!## | ${output = processedInvoice} |