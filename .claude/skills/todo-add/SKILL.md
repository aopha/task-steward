---
name: todo-add
description: When the user wants to add a new task or todo item. Use this skill whenever the user says "add a task", "create a todo", "new task", "我要添加任务", "新增任务", or any request to add something to their todo list. This skill analyzes the user's input text, extracts task information, matches it to existing goals, and creates the task using the todo-mcp API.
---

# Todo-Add Skill

Analyzes user input to create a structured task with AI-suggested details.

## Input

User's raw text describing a task (can be in Chinese or English).

## Process

### Step 1: List Existing Goals

Call `mcp__todo-mcp__list_goals` to get all available goals for goal_id matching.

### Step 2: Analyze User Input

From the user's input text, extract or infer:

| Field | How to Determine                                                                                                   |
|-------|--------------------------------------------------------------------------------------------------------------------|
| **goal_id** | Match task topic to existing goals by title/description relevance. If no match, use `null`                         |
| **title** | Extract the main action/subject from input                                                                         |
| **description** | Summarize or restate the input as a proper description                                                             |
| **ai_suggestion** | Provide 2-4 concrete steps or advice for completing this task                                                      |
| **tags** | Classify from: `["开发", "设计", "测试", "写作", "研究", "规划", "会议", "培训", "投资", "学习", "生活"]`            |
| **status** | Always `doing`                                                                                                     |
| **priority** | Infer from urgency/importance in input: `"high"` if urgent or important, `"medium"` if normal, `"low"` if can wait |
| **assignee** | `"AI"` if task can be completed by AI alone, otherwise `"本人"`                                                      |
| **progress** | Always `"0%"`                                                                                                      |
| **due_date** | Do not fill (leave empty)                                                                                          |
| **completed_at** | Do not fill (leave empty)                                                                                          |
| **executor** | Do not fill (leave empty)                                                                                          |
| **created_at** | Do not fill (use database default)                                                                                 |

### Step 3: Output Analysis Results

Present the filled fields in this format:

```
## Task Analysis

| Field | Value |
|-------|-------|
| goal_id | {id} |
| title | {title} |
| description | {description} |
| ai_suggestion | {suggestion} |
| tags | {tags} |
| status | doing |
| priority | {priority} |
| assignee | {assignee} |
| progress | 0% |

### Goal Match
{reasoning for goal_id selection, or "No matching goal found"}
```

### Step 4: Create the Task

Call `mcp__todo-mcp__create_task` with all the filled fields (excluding fields marked as "do not fill").

### Step 5: Confirm

Report the created task's ID and title.

## Tag Options Reference

- **开发**: Coding, programming, software development
- **设计**: Design work, UI/UX, architecture design
- **测试**: Testing, QA, verification
- **写作**: Writing, documentation, content creation
- **研究**: Research, exploration, proof of concept
- **规划**: Planning, strategy, roadmapping
- **会议**: Meetings, reviews, discussions
- **培训**: Training, learning, education
- **投资**: Investment, financial management, asset allocation
- **学习**: Learning, studying, skill development
- **生活**: Daily life, personal matters, lifestyle

## Priority Guidelines

- **high**: Deadline imminent, critical path, blocks others
- **medium**: Normal priority, no specific urgency
- **low**: Nice to have, can be deferred

## AI vs 本人 Assignment

- **AI**: The task can be completed by AI without human intervention (e.g., code generation, text writing, simple analysis)
- **本人**: Requires human judgment, creativity, or access AI doesn't have
