---
name: java-code-review
description: Java代码审查技能，通过比较两个git分支进行自动化代码审查
---

# Java代码审查技能

## 🎯 适用场景
- PR/CR代码审查
- 分支合并前检查
- 代码质量评估
- 安全风险排查


## 执行流程

1. **获取变更**
使用git命令进行两个分支差异代码的比对，获取差异文件

如果有必要，可以直接查看原始的文件，获取更完整的内容进行变更的检查判断

代码差异可以参数命令，如：
   ```bash
   git diff --name-only target..source -- '*.java'
   ```

3. **分析差异,进行审查**
   - 逐个文件检查问题
   - 审查可参考 [checklist.md](./reference/checklist.md)

4. **生成报告**
   - 创建Markdown格式报告
   - 包含统计和修复建议


## 审查规则
- [高危] 事务管理问题
- [高危] 安全漏洞
- [中危] 性能问题  
- [低危] 代码规范

## 报告格式
```markdown
# 代码审查报告

## 📊 统计概览
- 分支对比: {target_branch} → {source_branch}
- 变更文件: {file_count}个
- 发现问题: {issue_count}个

## 🚨 高危问题
{high_issues}

## ⚠️ 中危问题  
{medium_issues}

## 💡 建议
{recommendations}
```

<problem-desc-example>
【问题分类】事务管理
【风险等级】高
【问题描述】事务内包含 Feign 调用，可能导致长事务
【代码位置】OrderService.createOrder() 第 42 行
【改进建议】将 inventoryService.deduct() 移出 @Transactional，使用编程式事务单独控制订单创建
</problem-desc-example>