##启发式评估
- 启发式评估 -- 为什么以及如何？
- 设计启发（1）
- 设计启发（2）

##启发式评估 -- 为什么以及如何？

多种评估方式：

- 经验：评价实际的用户
- 常规：模型和公式计算测量
- 自动化：软件策略
- 批评：专业知识和启发式反馈

什么时候得到批评设计？

- 用户测试前。不要让用户浪费在小问题上。
- 在重新设计前。批评可以让你学到改进的地方。
- 当你知道有问题时，但是你需要证据。
- 在发布之前。

Begin Review with a Clear Goal

**启发式评估：**

- 由Jakob Nielsen开发
- 在一个设计中帮助找到可用性问题
- Small set评估检查用户界面
	- 独立检查符合可用性原则
	- 不同的评价者会找到不同的问题
	- 评估者只在之后沟通
- 可以在工作界面或草图上执行

Ten Design Heuristics

**评估过程：**

- 通过多次设计步骤
	- 检查细节、工作流和结构
	- 参考列表的可用性原则
	- 其它所想到的
- 什么原则？
	- Nielsen的“heuristics”
- 使用violations来重设计或解决问题

为什么要多个评估者？

- 没有一个评估者可以找到所有问题
- 一些人会比另一些找到更多

Heuristics和用户测试的比较：

- Heuristics Evaluation更快
	- 每个评估者1-2小时
- HE的结果预解释
- 用户测试更精确
	- 考虑真实的用户和任务
	- HE可能会遗漏问题，并且找到“false positives”
- 有价值的替代方法
	- 找到不同的问题
	- 不浪费参与者

Heuristic Evaluation阶段：

- Pre-evaluation training
- Evaluation
- Severity rating
- Debriefing

How-to: Heuristic Evaluation

- At least two passes for each evaluator
	- first to get feel for flow and scope of system
	- second to focus on specific elements
- If system is walk-up-and-use or evaluators are domain experts, no assistance needed
	- otherwise might supply evaluators with scenarios
- Each evaluator produces list of problems
	- explain why with reference to heuristic or other information
	- be specific and list each problem separately
- Why separate listings for each violation?
	- risk of repeating problematic aspect
	- may not be possible to fix all problems
- Where problems may be found
	- single location in UI
	- two or more locations that need to be compared
	- problem with overall structure of UI
	- something is missing
		- ambiguous with early prototypes; clarify in advance
		- sometimes features are implied by design docs and just haven’t been “implemented” – relax on those

Severity Ratings：

- 0 - don’t agree that this is a usability problem
- 1 - cosmetic problem 
- 2 - minor usability problem
- 3 - major usability problem; important to fix
- 4 - usability catastrophe; imperative to fix

Debriefing

- Conduct with evaluators, observers, and development team members
- Discuss general characteristics of UI
- Suggest potential improvements to address major usability problems
- Dev. team rates effort to fix
- Brainstorm solutions
