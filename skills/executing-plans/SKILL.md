---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. The quality of its work will be significantly higher if run on a platform with subagent support (such as Claude Code or Codex). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create a beads epic with child tasks:
   ```bash
   # Create epic for the plan
   br create --title="[Plan Name]" --type=epic --priority=2
   # Note the epic ID, then create each task as a child
   br create --title="Task 1: [Component Name]" --type=task --parent <epic-id>
   br create --title="Task 2: [Component Name]" --type=task --parent <epic-id>
   # Add inter-task dependencies where order matters
   br dep add <task-2-id> <task-1-id>  # Task 2 blocked by Task 1
   ```

### Step 2: Execute Tasks

Use `br ready` to pick the next unblocked task. For each:
1. Claim it: `br update <id> --status=in_progress`
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Close it: `br close <id> --reason="Completed"`
5. Next task: `br ready` (automatically shows only unblocked tasks)

### Step 3: Complete Development

After all tasks complete and verified:
1. Close the epic: `br epic close-eligible`
2. Announce: "I'm using the finishing-a-development-branch skill to complete this work."
3. **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
4. Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - REQUIRED: Set up isolated workspace before starting
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
