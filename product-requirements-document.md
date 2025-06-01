# RepoScribe - Product Requirements Document

A system that automatically detects new features in code repositories and updates documentation to keep it in sync with code changes.

## 1. Product Overview

### 1.1 Product Purpose
RepoScribe is an AI-powered system that monitors code repositories for feature changes, automatically interprets those changes, and ensures that both internal Product Requirements Documents (PRDs) and customer-facing documentation remain synchronized with the actual codebase.

### 1.2 Key Features
- GitHub integration with automatic feature detection
- AI-powered code interpretation and feature summarization
- Internal PRD synchronization and updating
- External documentation synchronization and updating
- Comprehensive administrative configuration

## 2. Functional Requirements

### 2.1 GitHub Integration & Feature Batch Identification
This core component connects to GitHub repositories, monitors code changes, and identifies batches of changes that constitute a new feature.

- System must listen to GitHub webhook events for:
  - Pull Request merges into configured branches
  - Push events (to scan for feature completion tags in commit messages)
- For PR-based detection:
  - Retrieve all commits included in the merged PR to define the feature batch
- For commit tag-based detection:
  - Scan commit messages for predefined [FEATURE_COMPLETE: <identifier>] tags
  - Identify feature batch boundaries (start and end commits)
- Generate cumulative code diff for all changes within a feature batch
- Generate and analyze Abstract Syntax Trees (ASTs) to identify structural changes

### 2.2 Feature Interpretation & Summarization
This component analyzes the code changes from a batch and generates human-readable descriptions of the new features.

- Feed identified structural code changes to an LLM
- Determine if changes constitute a new user-facing technical feature
- Generate concise technical summaries of identified features
- If needed, query the codebase for additional context to enhance understanding
- Support fallback to GitHub Search API if AST analysis is insufficient

### 2.3 Internal Documentation (PRD) Synchronization
This component ensures that internal product documentation stays current with newly implemented features.

- Access and process existing internal PRDs
- Create and maintain vector embeddings of PRD content for efficient semantic search
- Use feature summaries to perform semantic searches against the PRD vector store
- Determine if new features are adequately documented, partially documented, or missing
- For inadequately documented features:
  - Draft new sections or suggest modifications to existing sections
  - Incorporate feature summary and existing context
- Send proposed PRD changes to Product Managers for review and approval

### 2.4 External (Customer-Facing) Documentation Synchronization
This component keeps customer-facing documentation in sync with new features and updated PRDs.

- Access and process customer-facing documentation:
  - Support pre-formatted documentation (e.g., llms.text files)
  - Integrate with web scraping for content retrieval if needed
- Create and maintain vector embeddings of customer documentation
- Perform semantic searches using feature summaries and updated PRD content
- For missing or outdated documentation:
  - Draft new content or suggest modifications
  - Ensure consistency with internal PRDs
- Send proposed documentation changes to Technical Writers for review and publishing

### 2.5 Configuration & Administration
This component provides administrative controls for setting up and monitoring the system.

- GitHub repository configuration:
  - URL, target branches, API tokens/credentials
- Trigger mechanism configuration:
  - Enable/disable PR merge triggers
  - Enable/disable commit tag triggers
  - Define commit tag patterns and extraction rules
- Documentation source configuration:
  - Internal PRD locations/access methods
  - Customer documentation sources
- LLM and vector store configuration:
  - API keys and connection details
- Monitoring dashboard:
  - Activity logs
  - Success/failure records
  - Feature batch identification metrics

## 3. Technical Architecture

### 3.1 System Components
- GitHub Integration Module
- Code Analysis Engine (AST generation and comparison)
- LLM Interface for feature interpretation
- Vector Database for semantic search
- Documentation Access Module (PRD and customer docs)
- Web Scraping Service (optional for customer docs)
- Notification System for approvals
- Administration Console

### 3.2 Data Flow
1. Code changes trigger the system via GitHub webhooks
2. Feature batches are identified and analyzed
3. LLM interprets changes and generates feature summaries
4. Summaries are used to search existing documentation
5. Documentation updates are drafted where needed
6. Updates are sent for approval to relevant stakeholders
7. Approved changes are published to documentation repositories

## 4. User Roles and Interactions
- Administrators
  - Configure system settings
  - Monitor system performance
  - Manage authentication
- Product Managers
  - Review and approve PRD updates
  - Provide feedback on feature interpretations
- Technical Writers
  - Review and publish customer documentation updates
  - Provide feedback on documentation quality
- Developers
  - Add feature completion tags to commits when applicable
  - Review generated feature summaries for accuracy

## 5. Implementation Phases
- Phase 1: GitHub Integration & Feature Detection
  - Implement webhook listeners
  - Develop batch identification logic
  - Build AST generation and analysis
- Phase 2: Feature Interpretation
  - Integrate with LLM service
  - Develop feature summarization capabilities
  - Implement context gathering mechanisms
- Phase 3: Internal Documentation Sync
  - Create vector embedding system
  - Implement semantic search
  - Build PRD update generation
- Phase 4: External Documentation Sync
  - Implement customer doc access methods
  - Develop web scraping capabilities
  - Build customer doc update generation
- Phase 5: Administration & Configuration
  - Create admin console
  - Implement monitoring dashboard
  - Develop configuration interfaces

## 6. Success Metrics
- Accuracy of feature detection (% of features correctly identified)
- Quality of generated summaries (rated by developers and PMs)
- Documentation coverage (% of features properly documented)
- Time savings for Product Managers and Technical Writers
- Reduction in documentation lag (time between feature implementation and documentation)

## 7. Assumptions and Dependencies
- GitHub API access with sufficient permissions
- Access to high-quality LLM service
- Well-structured internal PRDs for effective vectorization
- Consistent commit patterns for tag-based feature detection
- Documentation sources with stable access methods

## 8. Glossary
- Feature Batch: A collection of commits that together implement a new user-facing feature
- AST (Abstract Syntax Tree): A tree representation of the abstract syntactic structure of source code
- PRD (Product Requirements Document): Internal documentation describing product functionality
- LLM (Large Language Model): AI model used for natural language understanding and generation
- Vector Store: Database optimized for storing and querying vector embeddings for semantic search
- Feature Completion Tag: Special marker in commit messages indicating the completion of a feature