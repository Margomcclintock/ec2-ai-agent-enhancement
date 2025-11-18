# EC2 AI Agent Enhancement System \- ServiceNow Implementation

## Overview

**Company Context:** Following the successful implementation of Netflix's manual EC2 remediation system (WL2), the DevOps team has reported significant improvements in incident response times. However, engineers still need to manually read incident descriptions, identify instance IDs, and navigate to the correct EC2 records to trigger remediation. During peak streaming hours (especially during new season releases), this manual process creates bottlenecks when multiple instances fail simultaneously.

**Your Role:** As a ServiceNow Administrator and Jr Developer at Netflix, you've been tasked with enhancing the existing manual remediation system with AI Agent capabilities that can identify EC2 instance IDs, and provide conversational remediation assistance to DevOps engineers.

**The Business Problem:** Netflix's DevOps team needs an intelligent conversational interface that can identify EC2 instance IDs from natural language, provide human-in-the-loop remediation approval, and execute the same proven remediation scripts through AI-driven conversation rather than manual UI navigation.

---
## Architecture Diagram


<img width="1171" height="799" alt="image" src="https://github.com/user-attachments/assets/b7206e4d-a4cd-40f5-ad72-14134ed9de2e" />




___
## Prerequisites

### Related Repository
This project extends [EC2 Remediation System](https://github.com/Margomcclintock/ec2-remediation-system) to introduce AI-driven enhancements for Netflix’s EC2 incident management.

To ensure compatibility, the WL2 Manual EC2 Remediation System needs to be fully configured and functional, including:

-  The EC2 Monitoring and Remediation-scoped application
-  EC2 Instance tables populated with valid data
-  A working AWS Integration Server connection and credentials
  -The EC2 Monitoring and Remediation-scoped application

**Verification:** Before proceeding,it is necessary for the manual system to successfully restart EC2 instances.

---
## System Resources and Context

**Netflix's Enhanced Integration:** The existing AWS Integration Server connection and manual remediation system remained unchanged. The AI Agent enhancement adds an intelligent conversational layer that utilizes the prior RemediationHelper logic through natural language interaction.

**Enhanced System Components:**

1. Existing manual remediation system (WL2) continues to function  
2. AI Agent provides a conversational interface for the same remediation functionality  
3. DevOps engineers can choose between manual UI Action or AI chat-based remediation  
4. Same AWS Integration Server API calls and logging for both approaches  
5. Enhanced incident response through intelligent conversation

### Implementation Objectives

1. **Build AI Agent Infrastructure**  
     
   - Created AI Agents with proper roles and instructions for EC2 remediation  
   - Implemented Script Tool that adapted existing RemediationHelper logic for conversational use  
   - Configured supervised execution with human approval requirements

   

2. **Enable Conversational Remediation**  
     
   - Tested the AI Agent's ability to read incident descriptions and identify instance IDs  
   - Validate the human-in-the-loop approval workflow  
   - Verified the same remediation logging and API integration as the manual system

   

3. **System Integration and Analysis**  
     
   - Compared manual vs AI Agent approaches for efficiency and usability  
   - Analyzed script differences and architectural adaptations  
   - Documented when each approach is most appropriate for DevOps workflows

## Sequential Configuration Steps

### Step 1: AI Agent Creation

**Navigate to AI Agent Studio in ServiceNow**

Created a new AI Agent with the following configuration:

**Describe the AI agent:**

- **Name:** `EC2 Remediation Assistant`  
- **Description:** `Helps DevOps engineers restart EC2 instances from incident tickets by reading incident descriptions and executing remediation scripts with human approval`
<img width="950" height="290" alt="image" src="https://github.com/user-attachments/assets/31ee0c84-c478-4bd9-83d0-7873c3793029" />

**Instruct the AI agent:**

- **AI agent role:** `EC2 remediation specialist for DevOps operations`  
- **Instructions:** Configure the agent to read incident descriptions, identify EC2 instance IDs, request human approval, and execute remediation using your existing API integration

<img width="727" height="175" alt="image" src="https://github.com/user-attachments/assets/08186ce1-36c2-4d6a-ad90-38ce4f77578c" />

### Step 2: Script Tool Implementation

**Added Script Tool to the AI Agent:**

- **Tool Configuration:** Created a script tool using the existing RemediationHelper logic adapted for a conversational interface  
- **Input Schema:** Single input for `instance_id` with LLM-friendly description

<img width="935" height="409" alt="image" src="https://github.com/user-attachments/assets/0a1f310b-a7c4-4ae2-b847-9bc8bc3a52b2" />


- **Execution Mode:** Supervised (requires human approval)
<img width="928" height="314" alt="image" src="https://github.com/user-attachments/assets/b5ebb1cb-1428-4cc9-90c9-7f91ea71c190" />


  
- **Integration:** Adapts the existing RemediationHelper logic for natural language interaction
  


**Key Adaptation:** The Script Tool translates between natural language input (instance IDs from conversation) and the existing database structure (sys\_ids), while maintaining identical API calls and logging to the existing RemediationHelper.

**Important:** After creating the script tool, it will automatically create records in the `sn_aia_agent_tool_m2m` table that links the agent to the tool. 

### Step 3: Agent Tool Configuration

**Navigate to AI Agent Studio:**

1. **Open the AI Agent:** EC2 Remediation Assistant  
2. **Go to Tools tab:** Add the script tool for EC2 remediation  
3. **Configure tool parameters:** Ensured the tool accepts instance\_id input and provides human-readable responses  
4. **Test tool integration:** Verify the agent can successfully call your script tool
<img width="839" height="417" alt="image" src="https://github.com/user-attachments/assets/4c43a3b9-6ca1-418e-a46e-59a0de66f24f" />


**Agent Tool Relationship:** The system automatically creates a many-to-many relationship record in `sn_aia_agent_tool_m2m` when you add tools to your agent. 

### Step 4: Testing and Validation

**Conversational Testing:**

Tested these scenarios in AI Agent Studio:

1. **Direct Instance ID Request:** "Restart instance i-09ae69f1cb71f622e"
   <img width="932" height="401" alt="image" src="https://github.com/user-attachments/assets/a88cac96-0d40-4607-85bb-48fbe33c5eda" />
<img width="956" height="388" alt="image" src="https://github.com/user-attachments/assets/41f07200-2b33-44a3-a416-c38a76c8edc6" />

2. **Incident-Based Request:** "Help me solve incident INC0010021"
<img width="944" height="341" alt="image" src="https://github.com/user-attachments/assets/ff71009b-fe97-46ec-8a42-97dd0459c0a6" />

 **Permission Flow:** Verify human approval requirements
<img width="959" height="365" alt="image" src="https://github.com/user-attachments/assets/1e803225-7450-46c8-a690-0afaa2c35a41" />
<img width="953" height="310" alt="image" src="https://github.com/user-attachments/assets/79183e57-5610-4a6b-926c-9ec300bb2fb0" />


 


## Integration Verification

 <img width="959" height="193" alt="image" src="https://github.com/user-attachments/assets/742ee867-1dea-41b1-9bb2-7d2578d41710" />
 
The Remediation Logs table captures each remediation request executed by the AI Agent. The  Manual and AI Agent-generated remediation logs with identical payloads and structure. The chart below explains what each field represents and how to interpret the results.

| Field | What It Shows | What It Means |
|-------|---------------|----------------|
| **EC2 Instance** | `04056b13307322107f44536bdd6b1cd9` | The log is linked to the correct EC2 Instance record in ServiceNow. This confirms the Script Tool located the correct record and passed the reference properly. |
| **HTTP Status Code** | `201` | AWS received the remediation request and accepted it. A 201 response means the remediation action was successfully created and AWS is processing it. |
| **Request Payload** | `{"instance_id":"i-08a978e8520523a44"}` | The instance ID was extracted by the AI Agent and sent in the correct JSON format. This verifies the LLM extraction and Script Tool formatting were accurate. |
| **Response Time** | Milliseconds | The amount of time AWS took to respond. This helps measure the speed and performance of the remediation workflow. |
| **Success** | `true` | The remediation request completed successfully. This confirms authentication, headers, credentials, and the AWS Integration Server connection are all functioning correctly. |
| **Timestamp** | `2025-11-10 17:24:41` | The exact time the remediation action was executed. This provides a clear timeline of agent activity and remediation events. |

## Interpretation

When a remediation is triggered, a new log entry captures the status and payload. A 201 status code combined with a success value of true confirms the full automation pipeline is working as expected, from LLM extraction to AWS API execution.

### Step 5: Comparison Analysis 

The AI Agent cuts execution time nearly in half while maintaining identical data logging and compliance workflows.
The conversational interface encourages faster responses, especially for DevOps engineers handling multiple incidents during Netflix release peaks. The EC2 AI Agent Enhancement extends the WL2 manual remediation system into a conversational framework that blends automation with human control.
It keeps all backend APIs, logging, and security mechanisms intact while improving efficiency, speed, and user experience. The AI Agent remediation is recommended for peak load hours and incident triage scenarios, while the manual UI Action remains best for single, direct record fixes.




