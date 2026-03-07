# BPMN Database Schema Documentation

## Schema Overview

**Name**: BPMN Process Model Database Schema  
**Version**: 3.0  
**Purpose**: Store and manage Business Process Model and Notation (BPMN) process definitions, including flow objects, connecting objects, swimlanes, and artifacts.

## Domain Model Semantics

- A **model** is the top-level container that represents a complete BPMN model (exactly one per instance)
- A **process** is a container for BPMN elements that represents a complete business process (a model can contain multiple processes)
- **Flow objects** (activities, events, gateways) are the main building blocks of a process
- **Connecting objects** (sequence flows, message flows, associations) link flow objects together
- **Swimlanes** (pools, lanes) represent organizational responsibilities
- **Data objects** and **data stores** represent information used in the process
- All BPMN elements inherit from a common base entity (`bpmn_element`)

## Tables

### Table: bpmn_model

**Description**: Represents the BPMN model container metadata (exactly ONE row per model instance).

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| bpmn_model_id | VARCHAR(50) | Unique model identifier | PK, NOT NULL | |
| name | VARCHAR(255) | Name of the model | NOT NULL | |
| version | VARCHAR(20) | Version number of the model | | |
| author | VARCHAR(100) | Author or creator of the model | | |
| creation_date | TIMESTAMP | Model creation date | | |
| modification_date | TIMESTAMP | Date of last modification | | |
| documentation | TEXT | Model-level documentation | | |

**Example Data**:
```
bpmn_model_id | name | version | author | creation_date | modification_date | documentation
------------------------------------------------------------------------------------------------------------------------
20251026_1730_56_001 | Invoice Management System | 1.0 | John Smith | 2025-01-15 10:30:00 | 2025-05-10 14:20:00 | Complete invoice management model with multiple processes
20251026_1845_12_001 | Customer Journey | 2.3 | Sarah Jones | 2025-03-22 09:15:00 | 2025-04-30 11:45:00 | End-to-end customer experience model
```

### Table: bpmn_process

**Description**: Represents a BPMN process that belongs to a model and serves as a container for BPMN elements.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| bpmn_process_id | INTEGER | Unique identifier for the process | PK, AUTO_INCREMENT | |
| bpmn_model_id | VARCHAR(50) | Reference to the model | FK, NOT NULL | bpmn_model.bpmn_model_id |
| name | VARCHAR(255) | Name of the process | NOT NULL | |
| is_executable | BOOLEAN | Indicates whether the process is executable | | |
| documentation | TEXT | Textual documentation of the process | | |

**Example Data**:
```
bpmn_process_id | bpmn_model_id | name | is_executable | documentation
------------------------------------------------------------------------------------------------------------------------
0001 | 20251026_1730_56_001 | Invoice Processing | true | Process for handling invoice approval and payment
0002 | 20251026_1730_56_001 | Invoice Validation | true | Subprocess for validating invoice data
0003 | 20251026_1845_12_001 | Customer Onboarding | true | Process for registering new customers
0004 | 20251026_1845_12_001 | Customer Verification | true | Process for verifying customer identity
```

### Table: bpmn_element

**Description**: The central base entity from which all BPMN elements are derived.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| bpmn_element_id | INTEGER | Unique identifier for the element | PK, AUTO_INCREMENT | |
| name | VARCHAR(255) | Name of the element | | |
| documentation | TEXT | Textual documentation or description of the element | | |
| element_type | VARCHAR(50) | Discriminator indicating the type of element | NOT NULL | |

**Value Domain for element_type**: ["service_task", "user_task", "script_task", "business_rule_task", "subprocess", "call_activity", "event", "gateway", "sequence_flow", "message_flow", "association", "pool", "lane", "data_object", "data_store", "text_annotation", "data_input", "data_output", "data_association"]

**HINWEIS**: Die Werte "activity" und "task" sind KEINE gültigen element_type Werte. Verwende immer den spezifischsten Subtyp (z.B. "user_task" statt "task", "subprocess" statt "activity").

**Example Data**:
```
bpmn_element_id | name | documentation | element_type
------------------------------------------------------
0001 | Approve Invoice | User approves the invoice | user_task
0002 | Start Process | Process starts when new order arrives | event
0003 | Invoice Amount Check | Decides processing path based on amount | gateway
```

### Table: message_definition

**Description**: Defines message types used in Message Events and Message Flows.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| message_definition_id | INTEGER | Unique identifier for the message | PK, AUTO_INCREMENT | |
| name | VARCHAR(255) | Name of the message | NOT NULL | |
| item_id | VARCHAR(50) | Reference to an Item Definition element | | |

**Example Data**:
```
message_definition_id | name | item_id
---------------------------------------
0001 | New Order Request | item_1
0002 | Payment Confirmation | item_2
```

### Table: signal_definition

**Description**: Defines signal types used in Signal Events.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| signal_definition_id | INTEGER | Unique identifier for the signal | PK, AUTO_INCREMENT | |
| name | VARCHAR(255) | Name of the signal | NOT NULL | |
| signal_type | VARCHAR(50) | Type of the signal | | |

**Example Data**:
```
signal_definition_id | name | signal_type
-----------------------------------------
0001 | Process Completed | public
0002 | Timeout Warning | private
```

### Table: error_definition

**Description**: Defines error types used in Error Events.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| error_definition_id | INTEGER | Unique identifier for the error | PK, AUTO_INCREMENT | |
| name | VARCHAR(255) | Name of the error | NOT NULL | |
| error_code | VARCHAR(50) | Error code | | |
| structure_id | VARCHAR(50) | Reference to a data structure | | |

**Example Data**:
```
error_definition_id | name | error_code | structure_id
--------------------------------------------------------
0001 | Invalid Invoice | ERR-001 | struct_1
0002 | Payment Failed | ERR-002 | struct_2
```

### Table: process_element

**Description**: Junction table defining the membership of elements to processes.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| process_element_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_process_id | INTEGER | Reference to the process | FK | bpmn_process.bpmn_process_id |
| bpmn_element_id | INTEGER | Reference to the element | FK | bpmn_element.bpmn_element_id |

**Example Data**:
```
process_element_id | bpmn_process_id | bpmn_element_id
-------------------------------------------------------
0001 | 0001 | 0001
0002 | 0001 | 0002
0003 | 0001 | 0003
```

### Table: activity

**Description**: Represents activities in the BPMN process, such as Tasks or Subprocesses.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| activity_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| activity_type | VARCHAR(50) | Type of activity | NOT NULL | |
| is_multi_instance | BOOLEAN | Indicates whether this is a multi-instance activity | | |
| loop_type | VARCHAR(20) | Type of loop | | |
| is_ad_hoc | BOOLEAN | Indicates whether the activity is ad hoc | | |
| is_compensation | BOOLEAN | Indicates whether the activity is a compensation | | |
| start_quantity | INTEGER | Number of required incoming tokens to start | | |
| completion_quantity | INTEGER | Number of tokens generated upon completion | | |

**Value Domain for activity_type**: ["task", "subprocess", "call_activity"]
**Value Domain for loop_type**: ["standard", "multi_instance_sequential", "multi_instance_parallel", "none"]

**Example Data**:
```
activity_id | bpmn_element_id | activity_type | is_multi_instance | loop_type | is_ad_hoc | is_compensation | start_quantity | completion_quantity
---------------------------------------------------------------------------------------------------------------------------------
0001 | 0001 | task | false | none | false | false | 1 | 1
0002 | 0004 | subprocess | false | none | false | false | 1 | 1
```

### Table: task

**Description**: Specialization of an activity that represents an atomic work step.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| task_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| activity_id | INTEGER | Reference to the activity | FK, UNIQUE | activity.activity_id |
| task_type | VARCHAR(50) | Type of task | NOT NULL | |
| implementation | VARCHAR(50) | Type of implementation | | |

**Value Domain for task_type**: ["user", "service", "send", "receive", "manual", "business_rule", "script"]
**Value Domain for implementation**: ["webservice", "##unspecified", "other"]

**Example Data**:
```
task_id | activity_id | task_type | implementation
--------------------------------------------------
0001 | 0001 | user | ##unspecified
0002 | 0003 | service | webservice
```

### Table: service_task

**Description**: A specialized task that represents a service or application.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| service_task_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| task_id | INTEGER | Reference to the task | FK, UNIQUE | task.task_id |
| operation_id | VARCHAR(50) | Reference to an operation | | |
| implementation_id | VARCHAR(255) | Reference to a specific implementation | | |

**Example Data**:
```
service_task_id | task_id | operation_id | implementation_id
-----------------------------------------------------
0001 | 0002 | operation_1 | com.example.InvoiceService
```

### Table: user_task

**Description**: A specialized task performed by a human user.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| user_task_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| task_id | INTEGER | Reference to the task | FK, UNIQUE | task.task_id |
| implementation | VARCHAR(100) | Implementation details for user interaction | | |
| assignment_expression | TEXT | Expressions for assignment rules | | |

**Example Data**:
```
user_task_id | task_id | implementation | assignment_expression
---------------------------------------------------------
0001 | 0001 | webform | ${role == 'finance-manager'}
```

### Table: script_task

**Description**: A specialized task that executes a script.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| script_task_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| task_id | INTEGER | Reference to the task | FK, UNIQUE | task.task_id |
| script | TEXT | The script code to execute | | |
| script_format | VARCHAR(50) | Format or language of the script | | |

**Value Domain for script_format**: ["javascript", "groovy", "python", "java", "other"]

**Example Data**:
```
script_task_id | task_id | script | script_format
------------------------------------------
0001 | 0003 | console.log("Processing invoice"); | javascript
```

### Table: business_rule_task

**Description**: A specialized task that executes business rules.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| business_rule_task_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| task_id | INTEGER | Reference to the task | FK, UNIQUE | task.task_id |
| implementation | VARCHAR(100) | Implementation details for rule execution | | |
| rule_names | TEXT | Names of the rules to execute | | |

**Example Data**:
```
business_rule_task_id | task_id | implementation | rule_names
------------------------------------------------
0001 | 0004 | dmn | discount_rules,shipping_rules
```

### Table: subprocess

**Description**: A composite activity that contains a self-contained process flow.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| subprocess_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| activity_id | INTEGER | Reference to the activity | FK, UNIQUE | activity.activity_id |
| is_transaction | BOOLEAN | Indicates whether the subprocess is treated as a transaction | | |
| triggered_by_event | BOOLEAN | Indicates whether the subprocess is triggered by an event | | |

**Example Data**:
```
subprocess_id | activity_id | is_transaction | triggered_by_event
---------------------------------------------------
0001 | 0002 | false | false
0002 | 0004 | true | false
0003 | 0005 | false | true
```

### Table: call_activity

**Description**: An activity that references and executes another process.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| call_activity_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| activity_id | INTEGER | Reference to the activity | FK, UNIQUE | activity.activity_id |
| bpmn_process_id_reference | INTEGER | Reference to the called process | FK | bpmn_process.bpmn_process_id |

**Example Data**:
```
call_activity_id | activity_id | bpmn_process_id_reference
----------------------------------
0001 | 0006 | 0002
```

### Table: event

**Description**: Represents events in the BPMN process (Start, End, Intermediate events).

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| event_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| event_type | VARCHAR(50) | Type of event | NOT NULL | |
| event_definition_type | VARCHAR(50) | Type of event definition | | |
| is_interrupting | BOOLEAN | Indicates whether the event is interrupting | | |

**Value Domain for event_type**: ["start", "end", "intermediate_catch", "intermediate_throw", "boundary"]
**Value Domain for event_definition_type**: ["message", "timer", "error", "signal", "none", "conditional", "escalation", "compensation", "link", "terminate"]

**Example Data**:
```
event_id | bpmn_element_id | event_type | event_definition_type | is_interrupting
----------------------------------------------------------------
0001 | 0002 | start | message | true
0002 | 0005 | end | none | null
0003 | 0006 | intermediate_catch | timer | null
0004 | 0007 | boundary | error | true
```

### Table: message_event_definition

**Description**: Defines a message-based event.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| message_event_definition_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| event_id | INTEGER | Reference to the event | FK, UNIQUE | event.event_id |
| message_definition_id | INTEGER | Reference to a message definition | FK | message_definition.message_definition_id |
| operation_ref | VARCHAR(50) | Reference to an operation | | |

**Example Data**:
```
message_event_definition_id | event_id | message_definition_id | operation_id
---------------------------------------------------------
0001 | 0001 | 0001 | operation_1
```

### Table: timer_event_definition

**Description**: Defines a time-based event.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| timer_event_definition_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| event_id | INTEGER | Reference to the event | FK, UNIQUE | event.event_id |
| time_date | TEXT | A specific date (ISO-8601) | | |
| time_duration | TEXT | A time duration (ISO-8601) | | |
| time_cycle | TEXT | A recurring time interval (ISO-8601) | | |

**Example Data**:
```
timer_event_definition_id | event_id | time_date | time_duration | time_cycle
---------------------------------------------------------
0001 | 0003 | null | PT1H | null
0002 | 0005 | 2025-06-01T09:00:00Z | null | null
0003 | 0006 | null | null | R3/PT10H
```

### Table: error_event_definition

**Description**: Defines an error-based event.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| error_event_definition_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| event_id | INTEGER | Reference to the event | FK, UNIQUE | event.event_id |
| error_definition_id | INTEGER | Reference to an error definition | FK | error_definition.error_definition_id |

**Example Data**:
```
error_event_definition_id | event_id | error_definition_id
-------------------------------------
0001 | 0004 | 0001
```

### Table: signal_event_definition

**Description**: Defines a signal-based event.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| signal_event_definition_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| event_id | INTEGER | Reference to the event | FK, UNIQUE | event.event_id |
| signal_definition_id | INTEGER | Reference to a signal definition | FK | signal_definition.signal_definition_id |

**Example Data**:
```
signal_event_definition_id | event_id | signal_definition_id
--------------------------------------
0001 | 0007 | 0001
```

### Table: sequence_flow

**Description**: Defines the control flow between activities, events, and gateways.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| sequence_flow_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| source_bpmn_element_id | INTEGER | Reference to the source element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| target_bpmn_element_id | INTEGER | Reference to the target element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| is_default | BOOLEAN | Indicates whether this is the default flow from a gateway | | |
| condition_expression | TEXT | Condition expression for conditional flows | | |

**Example Data**:
```
sequence_flow_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | is_default | condition_expression
--------------------------------------------------------------------------------------------------
0001 | 0009 | 0002 | 0001 | false | null
0002 | 0010 | 0001 | 0003 | false | null
0003 | 0011 | 0003 | 0012 | true | null
0004 | 0012 | 0003 | 0013 | false | ${amount > 1000}
```

### Table: gateway

**Description**: Represents decision and branching points in the process flow.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| gateway_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| gateway_type | VARCHAR(50) | Type of gateway | NOT NULL | |
| gateway_direction | VARCHAR(50) | Direction of gateway | | |
| sequence_flow_id | INTEGER | Reference to a sequence flow used as default | FK | sequence_flow.bpmn_element_id |

**Value Domain for gateway_type**: ["exclusive", "inclusive", "parallel", "event_based", "complex"]
**Value Domain for gateway_direction**: ["converging", "diverging", "mixed"]

**Example Data**:
```
gateway_id | bpmn_element_id | gateway_type | gateway_direction | sequence_flow_id
------------------------------------------------------------------
0001 | 0003 | exclusive | diverging | 0003
0002 | 0008 | parallel | converging | null
```

### Table: message_flow

**Description**: Defines the message flow between pools or participants.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| message_flow_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| source_bpmn_element_id | INTEGER | Reference to the sending element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| target_bpmn_element_id | INTEGER | Reference to the receiving element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| message_definition_id | INTEGER | Reference to a message definition | FK | message_definition.message_definition_id |

**Example Data**:
```
message_flow_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | message_definition_id
----------------------------------------------------------------------------------------
0001 | 0014 | 0015 | 0016 | 0002
```

### Table: association

**Description**: Connects information and artifact elements with BPMN base elements.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| association_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| source_bpmn_element_id | INTEGER | Reference to the source element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| target_bpmn_element_id | INTEGER | Reference to the target element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| association_direction | VARCHAR(20) | Direction of association | | |

**Value Domain for association_direction**: ["none", "one", "both"]

**Example Data**:
```
association_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | association_direction
----------------------------------------------------------------------------------------
0001 | 0017 | 0001 | 0018 | none
```

### Table: pool

**Description**: Represents a participant in the BPMN process.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| pool_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| bpmn_process_id | INTEGER | Reference to the contained process | FK | bpmn_process.bpmn_process_id |
| is_closed | BOOLEAN | Indicates whether the pool is closed | | |

**Example Data**:
```
pool_id | bpmn_element_id | bpmn_process_id | is_closed
--------------------------------------------
0001 | 0019 | 0001 | false
0002 | 0020 | 0003 | true
```

### Table: lane

**Description**: Represents a subdivision within a pool, often for roles or organizational units.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| lane_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| pool_id | INTEGER | Reference to the parent pool | FK, NOT NULL | pool.bpmn_element_id |

**Example Data**:
```
lane_id | bpmn_element_id | pool_id
------------------------------------
0001 | 0021 | 0019
0002 | 0022 | 0019
```

### Table: lane_element

**Description**: Junction table defining the membership of elements to lanes.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| lane_element_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| lane_bpmn_element_id | INTEGER | Reference to the lane | FK | lane.bpmn_element_id |
| bpmn_element_id | INTEGER | Reference to the contained element | FK | bpmn_element.bpmn_element_id |

**Example Data**:
```
lane_element_id | lane_bpmn_element_id | bpmn_element_id
----------------------------
0001 | 0021 | 0001
0002 | 0021 | 0002
0003 | 0022 | 0024
```

### Table: data_object

**Description**: Represents data used in the process.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| data_object_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| is_collection | BOOLEAN | Indicates whether this is a collection of data objects | | |
| state | VARCHAR(100) | State of the data object | | |

**Example Data**:
```
data_object_id | bpmn_element_id | is_collection | state
------------------------------------------
0001 | 0026 | false | draft
0002 | 0027 | true | approved
```

### Table: data_store

**Description**: Represents a persistent data storage.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| data_store_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| capacity | INTEGER | Capacity of the data store | | |
| is_unlimited | BOOLEAN | Indicates whether the data store has unlimited capacity | | |

**Example Data**:
```
data_store_id | bpmn_element_id | capacity | is_unlimited
-------------------------------------------
0001 | 0028 | 1000 | false
0002 | 0029 | null | true
```

### Table: text_annotation

**Description**: Allows adding text comments to the diagram.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| text_annotation_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| text | TEXT | The content of the text annotation | | |

**Example Data**:
```
text_annotation_id | bpmn_element_id | text
----------------------------------------------
0001 | 0030 | Critical step - requires manager approval
```

### Table: data_input

**Description**: Represents input data for an activity or process.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| data_input_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| name | VARCHAR(255) | Name of the input element | | |
| is_collection | BOOLEAN | Indicates whether this is a collection of data | | |

**Example Data**:
```
data_input_id | bpmn_element_id | name | is_collection
---------------------------------------
0001 | 0031 | Invoice Document | false
0002 | 0032 | Supporting Documents | true
```

### Table: data_output

**Description**: Represents output data from an activity or process.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| data_output_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| name | VARCHAR(255) | Name of the output element | | |
| is_collection | BOOLEAN | Indicates whether this is a collection of data | | |

**Example Data**:
```
data_output_id | bpmn_element_id | name | is_collection
---------------------------------------
0001 | 0033 | Approved Invoice | false
0002 | 0034 | Payment Records | true
```

### Table: data_association

**Description**: Connects activities with data elements and defines the data flow.

| Column | Data Type | Description | Constraints | References |
|--------|-----------|-------------|------------|------------|
| data_association_id | INTEGER | Unique identifier | PK, AUTO_INCREMENT | |
| bpmn_element_id | INTEGER | Reference to the base element | FK, UNIQUE | bpmn_element.bpmn_element_id |
| source_bpmn_element_id | INTEGER | Reference to the source data element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| target_bpmn_element_id | INTEGER | Reference to the target data element | FK, NOT NULL | bpmn_element.bpmn_element_id |
| transformation_expression | TEXT | Expression for data transformation | | |
| assignment_expression | TEXT | Expression for data assignment | | |

**Example Data**:
```
data_association_id | bpmn_element_id | source_bpmn_element_id | target_bpmn_element_id | transformation_expression | assignment_expression
------------------------------------------------------------------------------------------------------------------------
0001 | 0035 | 0031 | 0001 | null | ${invoice = input}
0002 | 0036 | 0024 | 0033 | ${output = transform(result)} | null
```

## Key Relationships

### Process to Elements
- **Type**: many-to-many
- **Junction Table**: process_element
- **Description**: Defines which elements belong to which processes

### Element Specialization
- **Type**: one-to-one (inheritance)
- **Example**: bpmn_element → activity → task → service_task
- **Description**: Specialized elements inherit from more general elements

### Process Flow
- **Type**: one-to-many
- **Example**: source_element → sequence_flow → target_element
- **Description**: Sequence flows connect source and target elements defining the process execution path

### Pool to Lanes
- **Type**: one-to-many
- **Example**: pool → lane(s)
- **Description**: A pool can contain multiple lanes representing different responsibilities

### Lanes to Elements
- **Type**: many-to-many
- **Junction Table**: lane_element
- **Description**: Elements can be assigned to lanes representing organizational responsibilities

### Data Flow
- **Type**: one-to-many
- **Example**: source_element → data_association → target_element
- **Description**: Data associations connect activities with data objects/stores

## Validation Rules

1. Every process must have at least one start event and one end event
2. Sequence flows cannot have the same element as both source and target
3. Every task must be connected with at least one incoming and one outgoing sequence flow (except start/end events)
4. Pools must either reference a process or contain lanes
5. Elements assigned to a lane must belong to the same process as the lane's pool
6. For any message flow, let:
   - `source_pool` = the pool containing the source element (or the source itself if it's a pool)
   - `target_pool` = the pool containing the target element (or the target itself if it's a pool)
   
   The constraint `source_pool ≠ target_pool` must be satisfied.
   
   **Implementation Notes**:
   - A message flow is invalid if both its source and target elements belong to the same pool
   - An element "belongs to" a pool if it is either:
     - The pool itself (when the pool acts as a message sender/receiver)
     - Contained within the pool (either directly or through lanes)
   - This rule ensures that message flows only represent communication between different pools (participants)
7. All IDs are now numeric integers with AUTO_INCREMENT

## Sample Process Instance

A complete minimal process instance would include:
- A process
- At least one start and one end event
- At least one activity (task)
- Sequence flows connecting the events and activities
- At least one pool containing the process elements

This documentation provides a comprehensive overview of the BPMN database schema with all information required to understand the structure, relationships, and constraints necessary to create valid BPMN process instances.