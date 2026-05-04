# 1.todo-add

```
开发一个skills，名叫todo-add，用于新增task，用户的输入是一段文本，需要根据用户的输入和task表的字段，把信息补充完整，规则如下：
1. goal_id，从获取goal表获取目标，如果当前任务和目标相关，则设置为对应目标的id
2. title，根据用户的输入的文本得到
3.description，根据用户的输入的文本得到
4.ai_suggestion，你对该任务的建议，或者执行步骤
5.tags，你对该任务的分类["开发" | "设计" | "测试" | "写作" | "预研" | "规划" | "会议" | "培训"]
6.status，'doing'
7.priority，由你根据截至时间和重要性确定["high" | "medium" | "low"]
8.created_at，不填，用数据库默认值
9.due_date，不填
10.completed_at，不填
11.assignee，你评估，该任务可由AI单独完成，如果是则填AI，否则填本人
12.progress，0%
13.executor，不填

然后输出上面的信息，同时调用todo-mcp的create_task接口完成任务创建
```