# Contribution 2: Update Agent Counter 

**Contribution Number:** 2
**Student:** Nhi Chu (Hayzie)
**Issue:** [https://github.com/Portabase/portabase/issues/271]
**Status:** [Phase II] [In progress]

---

## Why I Chose This Issue

I chose this issue because it's on the same repo that I have been contributing to, so I am familiar with the codebase and the tech stack used. 
This would also be a good practice on Javascript and full-stack development, especially on real-world debugging problems.
The issue is also clearly described and seems to be related to the files I have worked with in my previous contribution. 
It also has a clear way to double check whether my contribution fixes the problem visually, by replicating the problem.

---

## Understanding the Issue

### Problem Description

The Agent count display is not updated correctly in the case where an user deletes an existing agent. 

### Expected Behavior

The Agent counter should decrement whenever the user deletes an agent on the UI and display the updated count.

### Current Behavior

The counter stays the same, and still counts the deleted agent. 

### Affected Components

app/(customer)/dashboard/home/page.tsx
app/(customer)/dashboard/(admin)/agents

---

## Reproduction Process

### Environment Setup

The same process as [https://github.com/hayzie-chu/opensource-contribution-log/blob/main/Portabase-Google-Cloud.md]

### Steps to Reproduce

1. Clone and cd into the repo
2. Start up Docker
3. Install dependencies: pnpm install
4. Environment configuration: cp .env.example .env
5. Start postgres: docker compose up -d
6. Start in development mode: make up
7. Go to http://localhost:8887
8. Log in using the provided example default account
9. Navigate to Administration/Agents on the left panel
10. Add agent(s)
11. Navigate back to Home Dashboard to check the number of Agents (should be correct)
12. Navigate back to Agents page and delete the client
13. Navigate back to Home Dashboard to check the number of Agents (should be incorrect and has not changed)

### Reproduction Evidence

- **Commit showing reproduction:** N/A because already using the reproduction from [previous forked commit](https://github.com/hayzie-chu/opensource-contribution-log/blob/main/Portabase-Google-Cloud.md), and the issue is UI-only
- **Screenshots/logs:** 
<img width="1470" height="956" alt="Screenshot 2026-06-26 at 2 28 49 PM" src="https://github.com/user-attachments/assets/00408eed-a31b-4611-80ac-ab270794298e" />
<img width="1470" height="956" alt="Screenshot 2026-06-26 at 2 29 04 PM" src="https://github.com/user-attachments/assets/5591e06a-bedc-460b-945e-0414c095113a" />
<img width="1470" height="956" alt="Screenshot 2026-06-26 at 2 28 55 PM" src="https://github.com/user-attachments/assets/135f36d9-5d1b-4a9d-863b-46472533ce1c" />

- **My findings:** The count is only incorrect when deleting agents, not for adding. For example if you have 2 total agents both active, and delete 1, it still says 2 agents. And after that, if you add a new agent, it still increases by 1, resulting in the total count of 3. So the issue isn't some stale counter state, but that it's counting deleted agents as active.


---

## Solution Approach

### Analysis

The root cause is the dashboard count query has no isArchived filter. When an agent is deleted, in the database, the row isn't removed but it just sets isArchived to true and add deletedAt.

### Proposed Solution

I will look through the codebase to identify how the agent is created, stored, and counted, as well as how these information is displayed. Then I will check the counting logic and identify where the issue lies. If it's not some minor error but a major decision and logic change, I will communicate with the maintainers to discuss and get approval first.

### Implementation Plan

**Understand:** The count of Agents for each account is not decremented when an Agent is deleted, and still counts it as active.

**Match:** On the same dashboard, there are counts for Projects, Organizations, etc. display as well. I can look into this as guides for the display and count logic reference. Additionally, there are other pages and functions that works with the Agents database.

**Plan:** [Step-by-step implementation plan]
1. Check out all relevant, matching logics for counting, display, and Agents.
2. Implement a filter on the RoutePage function, following other functions working with the Agents database.
3. Test the counting logic in different cases.

**Implement:** [Link to your branch/commits as you work](https://github.com/hayzie-chu/portabase/tree/agent-count)

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]
Yes, and I have commented and been given permission to work on this on the issue.

**Evaluate:** [How will you verify it works?]
I will start up the server from my local machine and then perform a few Agents deletion/creation test cases manually to check the count is correctly updated.

---

## Testing Strategy
The project doesn't have equivalent/relevant testing files and ci flows for this change, as it's a specific logic just for UI display and not a standalone function and doesn't impact functionality.

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week 1 Progress

I recreated and tracked down the source of the issue through exploring the codebase and comparing it to similar counting displays and cross-checked other functions working with the Agents database. Found that agents/page.tsx:19 and services/agent.ts:40 both filter isArchived for counting Agents, only the Dashboard doesn't.

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** portabase/app/(customer)/dashboard/home/page.tsx
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
