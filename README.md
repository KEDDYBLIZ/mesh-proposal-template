# Mesh Proposal Template

A decentralized proposal management system built on Stacks for transparent collective decision-making and governance.

## Overview

Mesh Proposal enables decentralized communities to:
- Create and manage collaborative proposals
- Track proposal lifecycle through an immutable on-chain record
- Implement role-based participation and voting
- Ensure transparent and accountable decision-making processes
- Coordinate multi-stage proposal workflows

## Architecture

Mesh Proposal uses a modular governance structure:

```mermaid
graph TD
    A[Proposal] --> B[Stages]
    A --> C[Participants]
    A --> D[Voting Mechanisms]
    B --> E[Review Stages]
    E --> F[Voting Requirements]
    E --> G[Approval Thresholds]
```

Core components:
- Proposals: The foundational unit of collective decision-making
- Stages: Structured phases for proposal progression
- Participants: Members with defined governance roles
- Voting Mechanisms: Transparent and configurable voting systems
- Review Stages: Iterative evaluation processes

## Contract Documentation

### Main Contract: mesh-proposal.clar

#### Status Constants
- `STATUS-PENDING` (1)
- `STATUS-IN-PROGRESS` (2)
- `STATUS-COMPLETED` (3)
- `STATUS-DELAYED` (4)
- `STATUS-CANCELLED` (5)

#### Role Constants
- `ROLE-ADMIN` (1)
- `ROLE-MEMBER` (2)
- `ROLE-VIEWER` (3)

## Getting Started

### Prerequisites
- Clarinet installed
- Stacks wallet for deployment/interaction

### Usage Examples

1. Create a new proposal:
```clarity
(contract-call? .mesh-proposal create-proposal 
    "Community Initiative" 
    "Proposal for ecosystem improvement" 
    u1000)
```

2. Add proposal participant:
```clarity
(contract-call? .mesh-proposal add-participant 
    u1 
    'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM 
    u2)
```

3. Create proposal stage:
```clarity
(contract-call? .mesh-proposal create-stage 
    u1 
    "Initial Review" 
    "Community feedback phase" 
    u500)
```

## Function Reference

### Project Management

```clarity
(create-project (name (string-ascii 100)) (description (string-utf8 500)) (deadline uint))
(add-team-member (project-id uint) (member principal) (role uint))
(create-milestone (project-id uint) (name (string-ascii 100)) (description (string-utf8 500)) (deadline uint))
```

### Task Management

```clarity
(create-task (project-id uint) (milestone-id uint) (name (string-ascii 100)) (description (string-utf8 500)) (deadline uint) (dependencies (list 10 uint)))
(assign-task (project-id uint) (milestone-id uint) (task-id uint) (assignee principal))
(update-task-status (project-id uint) (milestone-id uint) (task-id uint) (new-status uint))
```

### Query Functions

```clarity
(get-project (project-id uint))
(get-milestone (project-id uint) (milestone-id uint))
(get-task (project-id uint) (milestone-id uint) (task-id uint))
(get-tasks-by-assignee (project-id uint) (assignee principal))
(get-upcoming-deadlines (project-id uint) (blocks-window uint))
```

## Development

### Testing
1. Clone the repository
2. Install dependencies: `clarinet install`
3. Run tests: `clarinet test`

### Local Development
1. Start local chain: `clarinet console`
2. Deploy contract
3. Interact using clarity console

## Security Considerations

1. Access Control
- Only authorized team members can perform actions
- Role-based permissions enforce access levels
- Admin privileges required for sensitive operations

2. Data Validation
- Deadline validation ensures future dates
- Status transitions are validated
- Dependencies are checked before task completion

3. Limitations
- Maximum 10 dependencies per task
- String length limits on names and descriptions
- List size constraints for scalability