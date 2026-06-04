# Contribution 1: Integrate Google Cloud Storage for storage backend

**Contribution Number:** 1  
**Student:** Hayzie Chu  
**Issue:** [https://github.com/Portabase/portabase/issues/266]  
**Status:** [Phase I] [Complete]  

---

## Why I Chose This Issue

I chose this issue because it aligns with my interest in Cloud and familiarity with TypeScript and React. The goal is to extend the codebase to support a new Cloud provider, which matches my beginner-level experience in Cloud and would be a good opportunitity to hone my skils. Additionally, I haven't used TypeScript and React in a while, so this would be a good callback and try to incorporate it with something new. The project itself is a database application, which also appeals to me as I plan to 

The issue is clearly defined in the original codebase, with specific files destinations for each feature requirement and a complete task checklist, making it an especially good first issue to get started on working with open source.

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

I folowed the Development Setup guide on the project's README. The guide did not clearly indicate or have steps for the Docket and postgres setup and only references running make up, which internally assumes a running Postgres instance. This initially caused a migration error on start up with an unclear error (Error 1 from make). I resolved this by re-reading the entrypoint script (docker/entrypoints/app-dev-entrypoint.sh) and the docker-compose.yml, then manually starting the Postgres container with docker compose up -d before running make up.

### Steps to Reproduce

1. Clone and cd into the repo
2. Start up Docker
3. Install dependencies: pnpm install
4. Environment configuration: cp .env.example .env
5. Start postgres: docker compose up -d
6. Start in development mode: make up
7. Go to http://localhost:8887
8. Log in using the provided example default account, navigate to Storage/Channels, press "Add Storage Channel".


### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit](https://github.com/hayzie-chu/portabase/commit/ae9b82deeccec7e96d26672b4b18b4385d618033)
- **My findings:** Google Cloud Storage is currently shown as "Coming soon!" and is not selectable. No backend provider, schema, or form exists for it.
- **Screenshots/logs:**
<img width="490" height="319"  alt="Screenshot 2026-06-04 at 1 25 04 PM" src="https://github.com/user-attachments/assets/52906614-e30f-483c-9202-123cb3f93b73" />
<img width="490" height="319" alt="Screenshot 2026-06-04 at 1 21 59 PM" src="https://github.com/user-attachments/assets/e36e3357-483e-4e63-923a-a0159c56b975" />

---

## Solution Approach

### Analysis

The Google Cloud Storage option is only a placeholder UI feature with no implementation in place yet. 

### Proposed Solution

[High-level description of your fix approach]
Implement GCS end-to-end following the existing provider pattern for working providers: add a backend provider using the @google-cloud/storage SDK, create a Zod schema for configuration validation, build a React form component for the UI, and wire everything into the existing provider registry and form renderer. Finally, change the preview: true flag so GCS becomes selectable.


### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Google Cloud Storage needs to be a fully functional storage provider in Portabase. It is currently a placeholder. The implementation should match the interface and patterns of existing providers and include UI screenshots in create and edit mode as PR validation.

**Match:** [What similar patterns/solutions exist in the codebase?]
The existing s3.ts and google-drive.ts providers define the interface to implement (ping, get, upload, delete). Their corresponding .schema.ts and .form.tsx files define the pattern for validation and UI. The index.ts provider registry and storage.tsx/common.tsx helpers show how to wire a new provider into the system.

**Plan:** [Step-by-step implementation plan]
1. Read existing providers (s3.ts, google-drive.ts) and their schemas/forms to understand the exact interface and field patterns
2. Add google-cloud-storage to StorageProviderKind in src/features/storages/types.ts
3. Create src/features/storages/providers/google-cloud-storage.ts implementing ping, get, upload, delete using @google-cloud/storage
4. Register the new provider in src/features/storages/providers/index.ts
5. Create the Zod validation schema at google-cloud-storage.schema.ts (fields: bucketName, projectId, keyFileContents or similar credential field)
6. Create the React form component at google-cloud-storage.form.tsx using the schema
7. Add the GCS case to channel-form.schema.ts
8. Remove preview: true from the GCS entry in storage.tsx
9. Add the google-cloud-storage case to renderChannelForm in common.tsx

**Implement:** [Link to working branch](https://github.com/hayzie-chu/portabase/tree/feature/google-cloud-storage)

**Review:** 
*  Follows the same interface as existing providers
*  TypeScript compiles without errors
*  Form renders correctly in create and edit mode
*  Removed preview: true  so the option is selectable
*  No hardcoded credentials or secrets
*  Follows project code style and file naming conventions

**Evaluate:** 
I will manually test and document if the feature works.
1. Run the dev environment, navigate to Add Storage Channel, confirm GCS appears as a selectable option, and select the GCS option
2. Fill in the form in create mode and save, ensure the form appears and works similarly to other providers.
3. Reopen the form in edit mode.
4. Take screenshots of all three states for the PR.
5. [Optional] Test against a real GCS bucket with a service account key to confirm ping and upload work end-to-end.

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
