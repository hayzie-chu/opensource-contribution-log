# Contribution 1: Integrate Google Cloud Storage for storage backend

**Contribution Number:** 1  
**Student:** Hayzie Chu  
**Issue:** [https://github.com/Portabase/portabase/issues/266]  
**Status:** [Phase I] [Complete]  

---

## Why I Chose This Issue

I chose this issue because it aligns with my interest in Cloud and familiarity with TypeScript and React. The issue is also clearly defined with specific files and task checklist, as well as being a "good first issue".

---

## Understanding the Issue

### Problem Description

As a tool for backing up and restoring database instances, it would benefit from having support for Google Cloud storage as a storage provider.

### Expected Behavior

The tool should show Google Cloud as an available option for providers, and have Google Cloud configuration in create and edit mode.

### Current Behavior

There is no current support and integration with Google Cloud as a provider.

### Affected Components

Relevant files: 
src/features/storages/providers/index.ts
src/features/storages/types.ts
src/components/wrappers/dashboard/admin/channels/channel/channel-form/providers/storages/forms/google-cloud-storage.schema.ts
src/components/wrappers/dashboard/admin/channels/channel/channel-form/providers/storages/forms/google-cloud-storage.form.tsx
src/components/wrappers/dashboard/admin/channels/channel/channel-form/channel-form.schema.ts
src/components/wrappers/dashboard/admin/channels/helpers/storage.tsx
src/components/wrappers/dashboard/admin/channels/helpers/common.tsx

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
