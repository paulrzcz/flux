---
name: story-decomposition
description: "Analyze Jira stories, fetch requirements from Confluence, examine source code, and decompose stories into actionable tasks created as linked Jira issues. When an agent needs to: (1) Break down a story into implementation tasks, (2) Create task breakdown from user story, (3) Plan work for a Jira story, or (4) Decompose complex work into manageable pieces. Uses story content, requirements documentation, and code analysis to create comprehensive task breakdowns."
---

# Story Decomposition

## Keywords
story decomposition, break down story, task breakdown, implementation plan, story analysis, Jira tasks, decompose work, story planning, task creation, story to tasks, breakdown work, analyze story, implementation tasks, story requirements

## Overview

Transform Jira stories into structured, actionable implementation tasks by analyzing story details, fetching requirements from Confluence, and examining relevant source code. This skill creates comprehensive task breakdowns with proper Jira issue linking, ensuring traceability from requirements to implementation.

**Use this skill when:** You have a Jira story that needs to be broken down into smaller, manageable implementation tasks with proper requirements analysis and code context.

---

## Core Workflow

**CRITICAL: Always follow this exact sequence:**

1. **Fetch Story Details** → Get the Jira story content and context
2. **Extract Confluence Links** → Find and fetch requirements documentation
3. **Analyze Source Code** → Search repository for relevant code context
4. **Decompose Story** → Break down into logical implementation tasks
5. **Present Breakdown** → Show user the planned tasks and approach
6. **Create Tasks** → Generate Jira issues with proper linking
7. **Provide Summary** → Present all created items with relationships

---

## Step 1: Fetch Story Details

When triggered with a Jira story key or URL, obtain to complete story context by using Atlassian MCP.


### Extract Key Information:
- **Summary and Description**: Core story details
- **Acceptance Criteria**: Specific requirements and outcomes
- **Comments**: Additional context and clarifications
- **Labels and Components**: Technical categorization
- **Linked Issues**: Dependencies or related work
- **Attachments**: Supporting documents or images

### Identify Confluence Links:
Parse the story content for:
- Direct Confluence page links
- Wiki references
- Documentation mentions
- Requirements specifications

---

## Step 2: Fetch Requirements Documentation

For each Confluence link identified in the story:

### Extract Page Information:
```
getConfluencePage(
  cloudId="...",
  pageId="[PAGE_ID]",
  contentFormat="markdown"
)
```

### Additional Confluence Searches:
If the story mentions requirements but no direct links:
```
searchConfluenceUsingCql(
  cloudId="...",
  cql="text ~ '[story keywords]' AND type = page AND space = 'REQ' OR space = 'DOCS'"
)
```

### Parse Requirements Content:
Extract and categorize:
- **User Stories Narrative**: As-a/I-want/So-that format
- **Business Requirements**: Business rules and constraints
- **Functional Requirements**: What to system should do
- **Non-Functional Requirements**: Performance, security, usability
- **Technical Specifications**: Architecture and integration requirements
- **Wireframes/Mockups**: UI/UX specifications
- **API Specifications**: Integration details and contracts

### Note Version and Date:
Check if requirements are current:
- "Last updated: [date]"
- "Version: [version]"
- Note outdated specifications and suggest verification

---

## Step 3: Analyze Source Code Context

Search the repository for relevant code implementation context:

### Primary Code Search:
Use the story keywords to find related files:
```
// Search for files containing story keywords
// Look in common locations: src/, lib/, components/, services/, api/
// Consider file extensions: .js, .ts, .py, .java, .go, .rb, .php
```

### Search Strategy:
1. **Feature/Module Names**: Search for main feature components
2. **Technical Terms**: Find relevant classes, functions, modules
3. **Data Models**: Look for database schemas, DTOs, entities
4. **API Endpoints**: Search related controllers, routes, handlers
5. **UI Components**: Find relevant views, components, templates

### Code Analysis Focus:
- **Existing Implementation**: What's already built
- **Related Components**: Code that will be affected
- **Architecture Patterns**: How new code should integrate
- **Database Schemas**: Current data structures
- **API Contracts**: Existing endpoints and patterns
- **Testing Patterns**: Current test structure and approach

### If Code Search is Limited:
```
I found limited code context for "[feature]". This could be:
- A brand new feature
- Using different terminology in code
- Documentation-driven development approach

Could you provide:
1. Specific file paths that might be relevant?
2. The main service/module name?
3. Any existing similar implementations I can reference?
```

---

## Step 4: Decompose Story into Tasks

Now create to logical breakdown based on all gathered information:

### Task Analysis Framework:

#### Aspect-Based Decomposition:
- **Backend/API Work**: Services, endpoints, business logic
- **Frontend/UI Work**: Components, views, user interactions
- **Database/Data Work**: Schemas, migrations, data processing
- **Integration Work**: Third-party services, APIs, messaging
- **Testing Work**: Unit tests, integration tests, E2E tests
- **Documentation Work**: Technical docs, user guides, API docs
- **Infrastructure/DevOps**: Deployments, configuration, monitoring

#### Size and Granularity:
- Target 3-10 tasks per story (avoid over-granularity)
- Each task should be 1-3 days of work
- Tasks should be independently implementable when possible
- Include both technical and non-technical work

#### Dependency Mapping:
- Identify prerequisite work (database before API)
- Note parallel work opportunities (frontend + backend)
- Mark sequential dependencies
- Consider integration points between tasks

### Determine Issue Types:

#### Use "Story" Tasks When:
- New user-facing features or functionality
- User experience improvements
- Product enhancements
- Customer-visible changes
- Keywords: "user can", "add ability to", "new feature", "enable"

#### Use "Task" Issues When:
- Technical implementation without direct user impact
- Infrastructure or DevOps work
- Refactoring or optimization
- Configuration or setup
- Documentation or tooling
- Keywords: "implement", "setup", "configure", "optimize", "refactor"

#### Use "Bug" Issues When:
- Fixing existing problems or defects
- Resolving errors or incorrect behavior
- Addressing performance issues
- Correcting data inconsistencies
- Keywords: "fix", "resolve", "bug", "issue", "error", "broken"

#### Use "Sub-task" Type When:
- Project uses Epic → Story → Sub-task hierarchy
- Very granular breakdown is needed
- Tasks are clearly nested under stories

### Task Content Structure:

For each planned task:
- **Clear, Actionable Summary**: Use verb-first naming
- **Implementation Scope**: What specifically needs to be done
- **Technical Details**: Relevant code context and patterns
- **Acceptance Criteria**: Testable requirements
- **Dependencies**: What needs to be completed first

---

## Step 5: Present Breakdown to User

Before creating anything, show the user your complete analysis:

### Presentation Format:
```
## Story Analysis: [Original Story Title]

**Original Story:** [PROJECT]-[NUMBER] - [Story Summary]
**Status:** [Current Status]

### Requirements Summary:
Based on the story and [X] Confluence pages:
- [Key requirement 1]
- [Key requirement 2]
- [Key requirement 3]

### Code Context Identified:
- Found relevant code in: [modules/files]
- Architecture pattern: [pattern name]
- Database impact: [schema changes needed]
- Integration points: [external services/APIs]

### Proposed Task Breakdown (X tasks):

**Implementation Tasks:**
1. [Story] Design and implement [feature] database schema
2. [Task] Create [feature] API endpoints with authentication
3. [Story] Build [feature] UI components with validation
4. [Task] Integrate [service] for [feature] functionality
5. [Task] Add error handling and logging for [feature]
6. [Story] Write unit and integration tests for [feature]
7. [Task] Update documentation and deployment guides

**Dependencies:**
- Tasks 2-3 depend on task 1 (database schema)
- Task 4 can be done in parallel with tasks 2-3
- Task 7 depends on tasks 2-6 completion

**Estimated Timeline:** [Timeline based on task count]

Shall I create these tasks and link them to the original story?
```

### Wait for User Confirmation:
This allows the user to:
- Adjust the task breakdown
- Add missing requirements
- Modify task priorities
- Change issue types
- Request more or less granularity

---

## Step 6: Create Tasks and Link to Story

After user approval, proceed with task creation:

### Get Project Information:
```
getJiraProjectIssueTypesMetadata(
  cloudId="...",
  projectIdOrKey="PROJECT"
)
```

### Create Each Task:
For each task in the planned breakdown:
```
createJiraIssue(
  cloudId="...",
  projectKey="PROJECT",
  issueTypeName="[Story/Task/Bug]",
  summary="[Task Summary]",
  description="[Task Description Template]",
  additional_fields={
    "priority": {"name": "Medium"},
    "labels": ["auto-generated", "story-decomposition"]
  }
)
```

### Task Description Template:
```markdown
## Context
This task is part of the decomposition of [PROJECT]-[NUMBER] - [Original Story Title].

### Original Story Requirements:
[Key requirement 1]
[Key requirement 2]
[Key requirement 3]

## Task Details
**This task specifically covers:**
- [Specific implementation details]
- [Technical approach]
- [Components/modules to work on]

### Code Context:
- Located in: [directory/file]
- Related to: [existing components]
- Follows pattern: [architectural pattern]

### Technical Requirements:
- [Technical requirement 1]
- [Technical requirement 2]
- [Technical requirement 3]

## Acceptance Criteria
- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]
- [ ] [Testable criterion 3]
- [ ] Code follows existing patterns and conventions
- [ ] Tests written and passing
- [ ] Documentation updated

## Notes
- This task was auto-generated from story decomposition
- Dependencies: [list any task dependencies]
- Related files: [relevant file paths]

**Related Story:** [PROJECT]-[NUMBER]
```

### Link Tasks to Original Story:
For each created task, create the link relationship:
```
createIssueLink(
  cloudId="...",
  inwardIssueKey="[NEW_TASK_KEY]",
  outwardIssueKey="[ORIGINAL_STORY_KEY]",
  linkTypeName="is part of"
)
```

### Track Created Tasks:
Maintain a list of all created tasks for the summary:
- Task key and summary
- Issue type
- Creation status (success/failure)
- Link status (created/failed)

---

## Step 7: Provide Summary

After all tasks are created and linked, present a comprehensive summary:

### Summary Format:
```
✅ Story Decomposition Complete!

**Original Story:** [PROJECT]-[NUMBER] - [Original Story Title]
https://[site].atlassian.net/browse/[PROJECT]-[NUMBER]

**Created Tasks ([X] tasks):**

1. [PROJECT]-[A] - [Task 1 Summary]
   Issue Type: [Story/Task/Bug]
   https://[site].atlassian.net/browse/[PROJECT]-[A]

2. [PROJECT]-[B] - [Task 2 Summary]
   Issue Type: [Story/Task/Bug]
   https://[site].atlassian.net/browse/[PROJECT]-[B]

3. [PROJECT]-[C] - [Task 3 Summary]
   Issue Type: [Story/Task/Bug]
   https://[site].atlassian.net/browse/[PROJECT]-[C]

[Continue for all tasks...]

**Requirements Source:**
- Story: [PROJECT]-[NUMBER]
- Confluence pages: [X] pages analyzed
- Code context: [Y] files/modules examined

**Task Relationships:**
- All tasks linked "is part of" [PROJECT]-[NUMBER]
- Dependencies documented in task descriptions

**Next Steps:**
1. Review created tasks for accuracy
2. Assign tasks to team members
3. Add estimations if your team uses them
4. Prioritize tasks within the story
5. Start implementation work
```
---

## Final Notes

This skill is designed to:
- **Save time** by automating routine story breakdowns
- **Ensure consistency** in task creation and linking
- **Improve traceability** from requirements to implementation
- **Reduce human error** in manual task creation
- **Scale productivity** by handling repetitive decomposition work

The key to success is:
1. **Thorough analysis** of story, requirements, and code context
2. **Clear communication** with users about planned breakdowns
3. **Consistent task creation** with proper linking and relationships
4. **Quality focus** over quantity when creating tasks
5. **Adaptability** to different project structures and needs

Remember: Every project is different. The skill should adapt to the specific context, patterns, and needs of each team while maintaining the core principles of good story decomposition.
