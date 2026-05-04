---
name: todo-delete
description: When the user wants to delete a task or todo item. Use this skill whenever the user says "delete task", "remove task", "delete todo", "删除任务", "删除待办", or any request to delete something from their todo list. This skill helps users find and delete tasks by ID or by listing tasks for selection.
---

# Todo-Delete Skill

Helps users delete existing tasks from their todo list.

## Input

User's request to delete a task (can be in Chinese or English). The user may provide:
- A specific task ID to delete
- A request to list tasks first for selection

## Process

### Step 1: Determine the Task to Delete

If the user provides a specific task ID, proceed to Step 3.
If the user does not specify an ID, proceed to Step 2.

### Step 2: List Tasks for Selection

Call `mcp__todo-mcp__list_tasks` to get all available tasks.
Present the tasks in a table format:

```
## Available Tasks

| ID | Title | Status | Priority |
|----|-------|--------|----------|
| {id} | {title} | {status} | {priority} |
```

Ask the user to specify which task ID to delete.

### Step 3: Get Task Details

Call `mcp__todo-mcp__get_task` with the task ID to confirm the task exists and display its details.

### Step 4: Confirm Deletion

Present the task details and ask for confirmation:

```
## Confirm Delete

**Task ID:** {id}
**Title:** {title}
**Status:** {status}
**Priority:** {priority}

Please confirm: Are you sure you want to delete this task? (yes/no)
```

Wait for user confirmation before proceeding.

### Step 5: Delete the Task

Upon confirmation, call `mcp__todo-mcp__delete_task` with the task ID.

### Step 6: Report Result

Report the deletion result:

- **Success:** "Task '{title}' (ID: {id}) has been deleted successfully."
- **Failure:** "Failed to delete task. Error: {error_message}"

## Priority Guidelines

- **high**: Deadline imminent, critical path, blocks others
- **medium**: Normal priority, no specific urgency
- **low**: Nice to have, can be deferred
