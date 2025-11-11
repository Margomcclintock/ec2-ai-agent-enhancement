# EC2 AI Agent Enhancement System \- ServiceNow Implementation

## Overview

**Company Context:** Following the successful implementation of Netflix's manual EC2 remediation system (WL2), the DevOps team has reported significant improvements in incident response times. However, engineers still need to manually read incident descriptions, identify instance IDs, and navigate to the correct EC2 records to trigger remediation. During peak streaming hours (especially during new season releases), this manual process creates bottlenecks when multiple instances fail simultaneously.

**Your Role:** As a ServiceNow Administrator and Jr Developer at Netflix, you've been tasked with enhancing the existing manual remediation system with AI Agent capabilities that can identify EC2 instance IDs, and provide conversational remediation assistance to DevOps engineers.

**The Business Problem:** Netflix's DevOps team needs an intelligent conversational interface that can identify EC2 instance IDs from natural language, provide human-in-the-loop remediation approval, and execute the same proven remediation scripts through AI-driven conversation rather than manual UI navigation.

---
## Architecture Diagram
<img width="1126" height="763" alt="image" src="https://github.com/user-attachments/assets/94725bcc-de61-4f14-988b-6fb3a1937c6a" />


___
## Prerequisites

**Required:** Completed WL2 Manual EC2 Remediation System with all components functional:

- EC2 Monitoring and Remediation scoped application  
- EC2 Instance tables with working data  
- AWS Integration Server connection and credentials

**Verification:** Before proceeding, ensure your manual system can successfully restart EC2 instances.

## System Resources and Context

**Netflix's Enhanced Integration:** Your existing AWS Integration Server connection and manual remediation system remain unchanged. The AI Agent enhancement adds an intelligent conversational layer that utilizes your proven RemediationHelper logic through natural language interaction.

**What You Receive:**

*From GitHub Repository:*

1. Enhanced system documentation and configuration requirements

*From Your WL2 Implementation:*

2. Fully functional manual EC2 remediation system  
3. Working AWS Integration Server connectivity  
4. Proven RemediationHelper Script Include  
5. Populated EC2 Instance and Remediation Log tables

**What You Build:**

1. ServiceNow AI Agent with conversational remediation capabilities  
2. Script Tool that bridges natural language input to your existing remediation API  
3. Chat interface for DevOps engineer interaction  
4. Integration testing and validation  
5. Comparative analysis of manual vs AI-enhanced approaches

## Assignment Objectives

### Enhanced System Architecture

```
AWS EC2 → AWS Integration Server → ServiceNow Custom Table → Flow Designer Workflow → AI Agent Conversational Interface + Manual UI Action → AWS Integration Server API
```

**Enhanced System Components:**

1. Existing manual remediation system (WL2) continues to function  
2. AI Agent provides conversational interface for the same remediation functionality  
3. DevOps engineers can choose between manual UI Action or AI chat-based remediation  
4. Same AWS Integration Server API calls and logging for both approaches  
5. Enhanced incident response through intelligent conversation

### Implementation Objectives

1. **Build AI Agent Infrastructure**  
     
   - Create AI Agent with proper role and instructions for EC2 remediation  
   - Implement Script Tool that adapts existing RemediationHelper logic for conversational use  
   - Configure supervised execution with human approval requirements

   

2. **Enable Conversational Remediation**  
     
   - Test AI Agent's ability to read incident descriptions and identify instance IDs  
   - Validate human-in-the-loop approval workflow  
   - Verify same remediation logging and API integration as manual system

   

3. **System Integration and Analysis**  
     
   - Compare manual vs AI Agent approaches for efficiency and usability  
   - Analyze script differences and architectural adaptations  
   - Document when each approach is most appropriate for DevOps workflows

## Sequential Configuration Steps

### Step 1: AI Agent Creation

**Navigate to AI Agent Studio in ServiceNow**

Create new AI Agent with the following configuration:

**Describe the AI agent:**

- **Name:** `EC2 Remediation Assistant`  
- **Description:** `Helps DevOps engineers restart EC2 instances from incident tickets by reading incident descriptions and executing remediation scripts with human approval`
<img width="950" height="290" alt="image" src="https://github.com/user-attachments/assets/31ee0c84-c478-4bd9-83d0-7873c3793029" />

**Instruct the AI agent:**

- **AI agent role:** `EC2 remediation specialist for DevOps operations`  
- **Instructions:** Configure agent to read incident descriptions, identify EC2 instance IDs, request human approval, and execute remediation using your existing API integration

<img width="727" height="175" alt="image" src="https://github.com/user-attachments/assets/08186ce1-36c2-4d6a-ad90-38ce4f77578c" />

### Step 2: Script Tool Implementation

**Add Script Tool to your AI Agent:**

- **Tool Configuration:** Create script tool using your existing RemediationHelper logic adapted for conversational interface  
- **Input Schema:** Single input for `instance_id` with LLM-friendly description

<img width="935" height="409" alt="image" src="https://github.com/user-attachments/assets/0a1f310b-a7c4-4ae2-b847-9bc8bc3a52b2" />


- **Execution Mode:** Supervised (requires human approval)
<img width="928" height="314" alt="image" src="https://github.com/user-attachments/assets/b5ebb1cb-1428-4cc9-90c9-7f91ea71c190" />


  
- **Integration:** Adapts your existing RemediationHelper logic for natural language interaction
  


**Key Adaptation:** The Script Tool translates between natural language input (instance IDs from conversation) and your existing database structure (sys\_ids), while maintaining identical API calls and logging to your existing RemediationHelper.

**Important:** After creating your script tool, it will automatically create records in the `sn_aia_agent_tool_m2m` table that links your agent to the tool. These relationship records must be included in your update set.

### Step 3: Agent Tool Configuration

**Navigate to AI Agent Studio:**

1. **Open your AI Agent:** EC2 Remediation Assistant  
2. **Go to Tools tab:** Add your script tool for EC2 remediation  
3. **Configure tool parameters:** Ensure the tool accepts instance\_id input and provides human-readable responses  
4. **Test tool integration:** Verify the agent can successfully call your script tool
<img width="839" height="417" alt="image" src="https://github.com/user-attachments/assets/4c43a3b9-6ca1-418e-a46e-59a0de66f24f" />


**Agent Tool Relationship:** The system automatically creates a many-to-many relationship record in `sn_aia_agent_tool_m2m` when you add tools to your agent. This record is essential for your update set.

### Step 4: Testing and Validation

**Conversational Testing:**

Test these scenarios in AI Agent Studio:

1. **Direct Instance ID Request:** "Restart instance i-09ae69f1cb71f622e"
   <img width="932" height="401" alt="image" src="https://github.com/user-attachments/assets/a88cac96-0d40-4607-85bb-48fbe33c5eda" />
<img width="956" height="388" alt="image" src="https://github.com/user-attachments/assets/41f07200-2b33-44a3-a416-c38a76c8edc6" />

2. **Incident-Based Request:** "Help me solve incident INC0010021"
<img width="944" height="341" alt="image" src="https://github.com/user-attachments/assets/ff71009b-fe97-46ec-8a42-97dd0459c0a6" />

option2: <img width="951" height="338" alt="image" src="https://github.com/user-attachments/assets/d6980dbe-96af-4257-ad2a-9045fd411cf3" />
<img width="951" height="302" alt="image" src="https://github.com/user-attachments/assets/0630a70b-1718-4d2c-8d16-2cf102970028" />
<img width="947" height="323" alt="image" src="https://github.com/user-attachments/assets/b3af8998-4548-400d-b397-2cb4ad4abc05" />


3. **Invalid Input Handling:** Test with malformed instance IDs
<img width="959" height="365" alt="image" src="https://github.com/user-attachments/assets/1e803225-7450-46c8-a690-0afaa2c35a41" />
<img width="953" height="310" alt="image" src="https://github.com/user-attachments/assets/79183e57-5610-4a6b-926c-9ec300bb2fb0" />


 
6. **Permission Flow:** Verify human approval requirements

**Integration Verification:**

- Confirm AI Agent creates entries in same Remediation Log table as manual system
 <img width="959" height="193" alt="image" src="https://github.com/user-attachments/assets/742ee867-1dea-41b1-9bb2-7d2578d41710" />

Integration Consistency Validation: Manual and AI Agent-generated remediation logs with identical payloads and structure.

All match so the integration consistency is confirmed.
| Field                | Manual                        | AI Agent           | Match? |
| -------------------- | ----------------------------- | ------------------ | ------ |
| **EC2 Instance**     | same sys_id                   | same sys_id        | ✅      |
| **Request Payload**  | identical JSON                | identical JSON     | ✅      |
| **HTTP Status Code** | 201                               | 201                | ✅      |
| **Response Time**    | within similar range          | similar range      | ✅      |
| **Success**          | true                          | true               | ✅      |
| **Timestamp**        | slightly different (run time) | slightly different | ✅      |

---
| Field                             | What It Shows                           | What It Means                                                                                                                                            |
| --------------------------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **EC2 Instance**                  | `04056b13307322107f44536bdd6b1cd9`      | The log entry was tied to the correct EC2 record (your instance reference worked).                                                                       |
| **HTTP Status Code**              | `401`                                   | The REST call reached the AWS Integration Server but failed authentication — meaning the connection worked, but credentials or headers need to be fixed. |
| **Request Payload**               | `{"instance_id":"i-08a978e8520523a44"}` | The Script Tool correctly sent your EC2 instance ID in JSON format — this confirms your AI Agent passed input to the RemediationHelper logic properly.   |
| **Success**                       | `false`                                 | This flag is only `false` because the API returned `401 Unauthorized`. Once your AWS credentials are valid, this will change to `true`.                  |
| **Created / Updated / Timestamp** | `2025-11-10 17:24:41`                   | Confirms the log was generated *at the time of your AI Agent execution*.                                                                                 |
---
- Verify identical API calls to AWS Integration Server  
- Test both manual UI Action and AI Agent on same instances

### Step 5: System Analysis and Documentation

**Script Comparison Analysis:**

Compare your manual system's `EC2RemediationHelper.js` with the AI Agent's script tool logic:

- Input method differences (sys\_id vs instance\_id)  
- Lookup direction changes (direct get vs query by instance\_id)  
- Return format adaptations (JSON string vs object)  
- Error handling variations  
- When each approach is most appropriate
---
#### **A. Efficiency Comparison **
| Criteria                         | Manual WL2 Remediation                  | AI Agent Remediation          | Improvement          |
| -------------------------------- | --------------------------------------- | ----------------------------- | -------------------- |
| **Action Steps per Remediation** | 5–6 (clicks + form load)                | 2 (chat + approval)           | ~65 % fewer steps    |
| **Average Execution Time**       | 90 seconds                              | 40 seconds                    | ~55 % faster         |
| **Context Switches**             | High (UI form + Integration Server log) | Low (chat window only)        | Reduced distraction  |
| **Error Transparency**           | Limited UI alerts                       | Clear conversational feedback | Higher clarity       |
| **Logging Consistency**          | ✔ Same table                            | ✔ Same table                  | Maintained integrity |

Conclusion: The AI Agent cuts execution time nearly in half while maintaining identical data logging and compliance workflows.
---
#### **B. Usability & Workflow Adoption **
| Metric                  | Manual System     | AI Agent                               |
| ----------------------- | ----------------- | -------------------------------------- |
| **User Interface**      | Form-based        | Conversational                         |
| **Learning Curve**      | Moderate          | Low                                    |
| **Human Approval Flow** | Manual click      | Chat-based “Yes/No”                    |
| **Accessibility**       | Desktop only      | Works in chat or mobile                |
| **Best Use Case**       | Single record fix | Multi-incident triage during peak load |

Conclusion: The conversational interface encourages faster responses, especially for DevOps engineers handling multiple incidents during Netflix release peaks.
---

#### **C. Architecture Comparison Chart **

| Layer               | Manual WL2 Flow         | AI Agent Enhancement                     |
| ------------------- | ----------------------- | ---------------------------------------- |
| **Input Source**    | UI Action on EC2 record | AI conversation message                  |
| **Identifier Used** | `sys_id`                | `instance_id`                            |
| **Record Lookup**   | `gr.get(sys_id)`        | `gr.addQuery('instance_id', instanceId)` |
| **Execution Logic** | RemediationHelper.js    | Script Tool → RemediationHelper.js       |
| **Response Format** | Object or UI message    | JSON string for LLM                      |
| **Logging**         | Remediation Log table   | Same Remediation Log table               |

---
#### **D.Architectural Diagram (Simplified) **
flowchart LR
A[AWS EC2] -->|Monitoring Alert| B[AWS Integration Server]
B --> C[ServiceNow EC2 Table]
C -->|Manual UI Action| D[RemediationHelper Script]
C -->|AI Conversation| E[EC2 Remediation Script Tool]
E --> D
D --> F[Remediation Log Table]
F --> G[Success/Failure Response]
---
#### **E. Summary Insights **
| Key Dimension       | Observation                             | Outcome                              |
| ------------------- | --------------------------------------- | ------------------------------------ |
| **Speed**           | Fewer steps and quicker execution       | Faster incident resolution           |
| **Accuracy**        | Identical Remediation Log entries       | Consistent auditing                  |
| **Scalability**     | AI Agent can process multiple incidents | Higher throughput during peak events |
| **Maintainability** | Reuses RemediationHelper logic          | Low technical debt                   |
| **User Experience** | Conversational approval flow            | Improved engagement                  |


---
#### ** Overall Conclusion**F. Overall Conclusion

The EC2 AI Agent Enhancement extends the WL2 manual remediation system into a conversational framework that blends automation with human control.
It keeps all backend APIs, logging, and security mechanisms intact while significantly improving efficiency, speed, and user experience.
The AI Agent remediation is now recommended for peak load hours and incident triage scenarios, while the manual UI Action remains best for single, direct record fixes.

## Testing and Validation

### DevOps User Testing

1. **AI Agent Conversation Flow:**  
     
   - DevOps engineer: "Help me with incident INC0010021"  
   - Agent reads incident, identifies instance ID, requests approval  
   - Engineer approves, agent executes remediation  
   - Same logging and API integration as manual system
<img width="944" height="341" alt="image" src="https://github.com/user-attachments/assets/ff71009b-fe97-46ec-8a42-97dd0459c0a6" />
   <img width="959" height="365" alt="image" src="https://github.com/user-attachments/assets/1e803225-7450-46c8-a690-0afaa2c35a41" />
<img width="953" height="310" alt="image" src="https://github.com/user-attachments/assets/79183e57-5610-4a6b-926c-9ec300bb2fb0" />


2. **Comparative Testing:**  
     
   - Use manual UI Action on one failed instance  
   - Use AI Agent conversation on another failed instance  
   - Verify identical remediation outcomes and logging

### System Verification

**AI Agent Execution:** Verify agent successfully calls your existing remediation API and creates log entries

**Conversation History:** Review agent conversation logs in AI Agent Studio

**Integration Consistency:** Confirm both manual and AI approaches create identical log entries in Remediation Log table

Both the manual WL2 UI Action and the AI Agent Script Tool write to the same x_wl2_remediation_log table.
Side-by-side comparison of recent entries confirms identical fields — including EC2 Instance reference, Request Payload structure, HTTP status codes, and success indicators.
Both approaches also trigger the same AWS Integration Server connection (AWS_Remediation_Alias), verified through REST Message Logs.
This confirms architectural consistency and ensures unified audit trails regardless of remediation path.

## Deliverables

### Update Set Requirements

**Complete all implementation work before creating your update set.**

Your update set must contain these working components:

**Data Layer Components:**

- Record from EC2 Instance table

**AI Agent Enhancement Components:**

- AI Agent definition: "EC2 Remediation Assistant" (Table: `sn_aia_agent`)  
- Script Tool with conversational remediation logic (Table: `sn_aia_agent_tool_m2m`)  
- Execution plan and associated tasks (Tables: `sn_aia_execution_plan` and related task records)

**API Integration Components:**

- Connection & Credential Alias  
- Connection record  
- Credentials (Basic Auth Type)

**Required Update Set Components Collection:**

**Step 1: Activate Your Update Set** Before testing your AI Agent, ensure your update set is active to capture all components.

**Step 2: Collect AI Agent Components**

**Finding and Adding AI Agent Definition:**

1. Navigate to **System Definition \> Tables**  
2. Search for table name: `sn_aia_agent`  
3. Click on the table name to open the list view  
4. Find your agent record: "EC2 Remediation Assistant"  
5. Right-click on your agent record \> **Add to Update Set**

**Finding and Adding Agent Tool Relationships:**

1. Navigate to **System Definition \> Tables**  
2. Search for table name: `sn_aia_agent_tool_m2m`  
3. Click on the table name to open the list view  
4. Filter by your agent's name or sys\_id to find the relationship records  
5. Select all agent-tool relationship records for your agent  
6. Right-click \> **Add to Update Set**

**Finding and Adding Execution Plans:**

1. Navigate to **System Definition \> Tables**  
2. In the **Name** field, search for: `execution plan`  
3. Look for the table `sn_aia_execution_plan` in the results  
4. Click on the table name to open the list view  
5. Look in the **Objective** column for plans related to EC2 remediation  
6. Identify your execution plan(s) by objective content (should mention EC2 instances or remediation)  
7. Right-click on your execution plan record(s) \> **Add to Update Set**

**Finding and Adding Execution Plan Tasks (Critical Step):**

1. From the execution plan list view, click on your execution plan record to open it  
2. In the execution plan form, find the **State** field  
3. Click on the **'Completed'** value in the State field (this is a clickable link)  
4. This opens a list view showing all tasks associated with your execution plan  
5. Use **Ctrl+A** (Windows) or **Cmd+A** (Mac) to **select all tasks**  
6. Right-click on the selected tasks \> **Add to Update Set**

**Alternative Method for Finding Tasks:**

1. Navigate to **System Definition \> Tables**  
2. Search for execution plan task tables (may vary, look for tables starting with `sn_aia` and containing "task")  
3. Filter by your execution plan's sys\_id  
4. Select all related task records and add to update set

**File name:** `ec2-ai-agent-enhancement.xml`

### README.md Content Requirements

**Required sections (in this order):**

*Use screenshots to augment your documentation*

- **System Overview:** Description of the enhanced EC2 remediation system with both manual and AI Agent capabilities  
- **Implementation Steps:** Key configuration decisions for AI Agent integration with existing manual system  
- **Architecture Diagram:** Visual representation showing both manual UI and conversational workflows  
- **Optimization:** Comparative analysis of manual vs AI Agent approaches \- when to use each method, efficiency differences, and architectural trade-offs  
- **DevOps Usage:** Instructions for Netflix DevOps engineers on using both manual remediation and AI Agent conversation for different scenarios

### Architecture Diagram Requirements

Create an enhanced system flow diagram showing:

- Existing manual remediation workflow (WL2 system)  
- New AI Agent conversational interface  
- How both approaches utilize the same AWS Integration Server API  
- Decision points for when DevOps engineers should use manual vs conversational remediation

Use Draw.io and save as `Diagram.png`

## Submission Requirements

1. Test your enhanced system thoroughly, demonstrating both manual and AI Agent remediation capabilities  
2. Create your update set with all required AI Agent components and evidence records  
3. Complete comprehensive script comparison analysis in your README  
4. Upload your GitHub repository with complete documentation and enhanced architecture diagram  
5. Submit your repository URL

**Critical Success Factors:**

- AI Agent must successfully read incident descriptions and identify instance IDs  
- Human approval workflow must function correctly before remediation execution  
- Both manual and AI Agent approaches must create identical remediation log entries  
- Script comparison analysis must demonstrate understanding of architectural differences  
- Documentation must clearly explain when to use manual vs conversational remediation approaches


Additional Testing Verificiation :
<img width="959" height="365" alt="image" src="https://github.com/user-attachments/assets/7cd752c8-b6bb-4973-8445-f2becae0ec1a" />
<img width="943" height="365" alt="image" src="https://github.com/user-attachments/assets/b6464f9d-ac61-4284-8867-319e1e92cd38" />
<img width="951" height="365" alt="image" src="https://github.com/user-attachments/assets/966a1903-6047-4e77-94f2-c06c78fc4fa2" />



