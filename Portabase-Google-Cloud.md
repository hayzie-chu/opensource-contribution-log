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

Google Cloud Storage should appear as a fully selectable provider in the "Add Storage Channel" dialog. Users should be able to configure a GCS bucket (providing credentials such as a service account key, bucket name, and project ID) in both create and edit modes, and Portabase should be able to ping, upload, get, and delete objects from that bucket.

### Current Behavior

Google Cloud Storage is shown in the provider list as "Coming soon!" and cannot be selected or configured. There is no backend provider implementation, no validation schema, and no UI form for it.

### Affected Components

Relevant files: 
src/features/storages/providers/google-cloud-storage.ts (to be created)
src/features/storages/providers/index.ts
src/features/storages/types.ts
src/components/wrappers/dashboard/admin/channels/channel/channel-form/providers/storages/forms/google-cloud-storage.schema.ts (to be created)
src/components/wrappers/dashboard/admin/channels/channel/channel-form/providers/storages/forms/google-cloud-storage.form.tsx (to be created)
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
The original application is not yet fully functional and doesn't require unit or integration test, just manual checking through running the application and interacting with the UI.

### Manual Testing
1. Run the dev environment, navigate to Add Storage Channel, confirm GCS appears as a selectable option, and select the GCS option
2. Fill in the form in create mode and save, ensure the form appears and works similarly to other providers.
3. Reopen the form in edit mode.
4. Take screenshots of all three states for the PR.
5. [Optional] Test against a real GCS bucket with a service account key to confirm ping and upload work end-to-end.


---

## Implementation Notes

### Week 1 Progress

[What you built this week, challenges faced, decisions made]
Implemented Google Cloud Storage as a fully functional storage provider in Portabase end-to-end.

What I built:

- Backend provider: src/features/channel/storages/google-cloud-storage/index.ts: implemented ping, get, upload, delete (plus copy, which the ProviderHandler interface requires) using the @google-cloud/storage SDK. ping round-trips a ping.txt object (save → download → delete) to verify credentials + bucket access, mirroring pingS3.
- Config + validation: Zod schema (google-cloud-storage.schema.ts) and a types.ts holding GoogleCloudStorageConfig, with fields projectId, bucketName, clientEmail, privateKey.
- UI form: google-cloud-storage.form.tsx (private key uses a multi-line Textarea; other fields use Input).
- Wiring: registered the handler in storages/index.ts, added the discriminated-union case in channel-form.schema.ts, added the renderChannelForm case in channels-helpers.tsx, added google-cloud-storage to the StorageProviderKind type, and removed preview: true from the provider registry so it's selectable.
- Database: extended the provider_storage_kind Postgres enum and generated migration 0058_tense_tana_nile.sql.

Challenges faced:

- The issue's listed file paths (src/features/storages/providers/..., src/components/wrappers/dashboard/admin/channels/...) did not exist in the current repo — the structure had been refactored. Spent the first chunk of time mapping the real layout by grepping for stable symbols (StorageProviderKind, renderChannelForm, the existing s3/google-drive providers) instead of trusting the paths. Real backend providers live in src/features/channel/storages/.
- The provider key was inconsistent: the UI placeholder used gcs, but the type system, schema literals, and renderer needed to agree. Standardized on google-cloud-storage everywhere (safe since the placeholder was never functional).
- tsc surfaced an error the issue's plan never mentioned: the provider column is a Postgres pgEnum, so adding the value to the TS union wasn't enough — it required a schema change + drizzle-kit migration. Found the precedent in migration 0032 (which had added google-drive the same way) and matched it.
- React "uncontrolled → controlled input" warning when filling the form: create-mode resets the form to {enabled: true}, so config fields start undefined. Fixed by making each field controlled from mount with value={field.value ?? ""}.

Decisions made:

- Used the folder structure (google-cloud-storage/index.ts + sibling files) matching the google-drive/ provider, rather than the flat single-file s3.ts style — keeps related files grouped.
- Took clientEmail + privateKey as separate fields (passed to the SDK as credentials) rather than a key-file path, since config is stored in the DB and the app can't rely on a filesystem key file. Normalize escaped newlines (\\n → \n) so a key pasted from a service-account JSON works.
- ensureBucket does not auto-create the bucket (unlike uploadS3) as GCS bucket names are globally unique and creation needs a location, so it errors clearly if the bucket is missing instead.


### Code Changes

- **Files added:** 
src/features/channel/storages/google-cloud-storage/index.ts 
src/features/channel/storages/google-cloud-storage/google-cloud-storage.form.tsx 
src/features/channel/storages/google-cloud-storage/google-cloud-storage.schema.ts
src/features/channel/storages/google-cloud-storage/types.ts
src/db/migrations/0058_tense_tana_nile.sql (+ snapshot/journal)

- **Files modified:** 
src/features/storages/storages.types.ts — StorageProviderKind
src/features/channel/storages/index.ts — handler registration
src/features/channel/channel-form.schema.ts — discriminated-union case
src/features/channel/channels-helpers.tsx — renderChannelForm case
src/features/channel/channels-storage-helper.tsx — removed preview: true, renamed gcs → google-cloud-storage
src/db/schema/12_storage-channel.ts — provider_storage_kind enum
package.json / pnpm-lock.yaml — added @google-cloud/storage

- **Key commits:** [working UI, not yet tested](https://github.com/hayzie-chu/portabase/commit/3765b8b27462bb3d2558202b4b714a1b76faa1db)
- **Approach decisions:** [Why you chose certain approaches]
Used the folder structure (google-cloud-storage/index.ts + sibling files) matching the google-drive/ provider, rather than the flat single-file s3.ts style — keeps related files grouped. Took clientEmail + privateKey as separate fields (passed to the SDK as credentials) rather than a key-file path, since config is stored in the DB and the app can't rely on a filesystem key file. Normalize escaped newlines (\\n → \n) so a key pasted from a service-account JSON works. ensureBucket does not auto-create the bucket (unlike uploadS3) as GCS bucket names are globally unique and creation needs a location, so it errors clearly if the bucket is missing instead.

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted](https://github.com/Portabase/portabase/pull/316)

**PR Description:** [Draft or final PR description - much of the content above can be adapted]
# Validation Requirements screenshots:
- “Add Storage Channel” dialog showing Google Cloud Storage as an available provider
<img width="1470" height="798" alt="dialog-providers" src="https://github.com/user-attachments/assets/6a8a3cc1-a43a-4db2-b627-81c986f0a75b" />

- Google Cloud Storage configuration form in create mode
<img width="744" height="751" alt="create-mode-default" src="https://github.com/user-attachments/assets/06f9798a-10cf-4be6-85af-8606f5970c40" />
<img width="597" height="700" alt="create-mode-filled" src="https://github.com/user-attachments/assets/4182d7c7-69b5-44d1-9bb2-cd630beeeefe" />

- Google Cloud Storage configuration form in edit mode
<img width="575" height="728" alt="edit-mode" src="https://github.com/user-attachments/assets/276be101-f48f-46fb-900c-9d0f5afc4826" />
<img width="1468" height="798" alt="after-changed" src="https://github.com/user-attachments/assets/dba0f488-7fc7-4a04-899c-84fbc8db81ad" />

___________________________
Note: the ping/upload path hasn't been tested against a real GCS bucket yet, but the UI is fully functional

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** Awaiting review

---

## Learnings & Reflections

### Technical Skills Gained

- How a provider-plugin architecture is wired across layers in a Next.js + Drizzle codebase: backend handler → registry Record<Kind, Handler> → Zod discriminated union → React form → form renderer.
- Drizzle pgEnum workflow: changing an enum requires drizzle-kit generate to emit an ALTER TYPE ... ADD VALUE migration; the TS type alone isn't enough.
- The @google-cloud/storage SDK: authenticating with in-memory credentials (vs. a key file), streaming uploads via createWriteStream, and generating signed URLs with getSignedUrl.
- The react-hook-form controlled-input pattern (value={field.value ?? ""}) and why the uncontrolled→controlled warning happens.

### Challenges Overcome
- Stale documentation in a large repo: the issue's file paths were wrong. Learned to navigate by searching for stable symbols/patterns rather than trusting given paths, and to trace one working example (S3) end-to-end as a template.
- A hidden requirement the plan missed: the DB enum migration. The TypeScript compiler caught it — I used tsc --noEmit to find every place the new provider value needed to be accepted, then traced the error into the Drizzle schema.
- Distinguishing my errors from pre-existing ones: when tsc showed errors in channel-form.tsx, I git stash-ed my work and re-ran the typecheck to confirm those errors already existed on the branch and weren't introduced by my change.

### What I'd Do Differently Next Time

- Run tsc --noEmit and a build before writing the UI layer — the DB enum requirement would have surfaced immediately instead of after the wiring was done.
- Establish a baseline (tsc on a clean tree) at the very start, so distinguishing new vs. pre-existing errors is instant.
- Make small, descriptive commits as I go rather than one WIP commit — easier for a mentor/reviewer to follow the progression.
