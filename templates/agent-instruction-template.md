# AI Agent Instruction Prompt Template

*A comprehensive template structure for creating effective AI agent prompts based on context engineering principles.*

---

## 📋 TEMPLATE STRUCTURE

### 1. ROLE & PURPOSE DEFINITION
```markdown
# Agent Role: [Specific Agent Type]

## Core Purpose
**Primary Function**: [Single sentence describing the agent's main responsibility]
**Domain Expertise**: [Specific area of knowledge/skill]
**Key Capabilities**: [List 3-5 core abilities]
**Success Criteria**: [How success will be measured]
```

### 2. CONTEXT FOUNDATION
```markdown
## Project Context

### Project Overview
**Purpose**: [Single sentence elevator pitch]
**Domain**: [Industry/field/application area]
**Core Value**: [What problem this solves/what value it creates]
**Key Stakeholders**: [Who uses/benefits from this]

### Technical Ecosystem
**Primary Technologies**: [Core frameworks/languages/platforms]
**Architecture Pattern**: [MVC, microservices, serverless, etc.]
**Key Dependencies**: [Critical libraries/services/APIs]
**Version Requirements**: [Specific versions and compatibility notes]

### Project Structure
```
project-root/
├── [key directories with purpose]
└── [important files with descriptions]
```
```

### 3. BEHAVIORAL GUIDELINES
```markdown
## Operating Principles

### Core Behaviors
- **Quality Standards**: [Specific quality requirements]
- **Code Style**: [Formatting, naming conventions, patterns to follow]
- **Communication Style**: [Tone, verbosity, explanation level]
- **Error Handling**: [How to handle failures and edge cases]

### Decision-Making Framework
1. **Priority Order**: [What to prioritize when making decisions]
2. **Constraint Awareness**: [Limitations and boundaries to respect]
3. **Risk Assessment**: [How to evaluate potential issues]
4. **Validation Gates**: [Required checks before proceeding]
```

### 4. CAPABILITY MATRIX
```markdown
## Available Tools & Resources

### Function Inventory
**[Tool Name]**: [Purpose and when to use]
- Parameters: [Key parameters and their purpose]
- Output: [What to expect back]
- Usage Guidelines: [Best practices and limitations]

### Knowledge Resources
- **Documentation**: [Available docs and their scope]
- **Examples**: [Reference implementations and patterns]
- **Best Practices**: [Established conventions and standards]

### External Integrations
- **APIs**: [Available external services and their capabilities]
- **Scripts**: [Automation tools and utilities]
- **Services**: [Development/testing/deployment resources]
```

### 5. REASONING FRAMEWORK
```markdown
## Problem-Solving Approach

### Analysis Pattern
1. **Understand**: [How to interpret requirements]
2. **Plan**: [Strategy development process]
3. **Execute**: [Implementation approach]
4. **Validate**: [Verification and testing methods]
5. **Iterate**: [Improvement and refinement process]

### Chain-of-Thought Structure
**Problem**: [How to identify and state challenges]
**Analysis**: [Method for breaking down complexity]
**Strategy**: [Approach selection criteria]
**Implementation**: [Execution planning methodology]
**Validation**: [Success verification approach]
```

### 6. EXAMPLE PATTERNS
```markdown
## Reference Examples

### Task Type: [Example Category]
**Input**: [Sample input]
**Process**: [Step-by-step approach]
**Output**: [Expected result with explanation]
**Validation**: [How to verify correctness]

### Task Type: [Another Example Category]
**Input**: [Sample input]
**Process**: [Step-by-step approach]
**Output**: [Expected result with explanation]
**Validation**: [How to verify correctness]

### Common Patterns
- **Pattern Name**: [When to use and how to apply]
- **Anti-Pattern**: [What to avoid and why]
```

### 7. QUALITY ASSURANCE
```markdown
## Validation Framework

### Pre-Implementation Checklist
- [ ] Requirements fully understood?
- [ ] All necessary context available?
- [ ] Approach aligns with project standards?
- [ ] Dependencies and constraints identified?
- [ ] Success criteria defined?

### Post-Implementation Checklist
- [ ] Code follows established patterns?
- [ ] All edge cases handled?
- [ ] Performance requirements met?
- [ ] Security considerations addressed?
- [ ] Documentation updated?
- [ ] Tests pass?

### Quality Gates
**Mandatory**: [Non-negotiable requirements]
**Recommended**: [Best practices to follow]
**Optional**: [Nice-to-have enhancements]
```

### 8. INTERACTION PROTOCOLS
```markdown
## Communication Guidelines

### Input Processing
- **Clarification**: [When and how to ask for more information]
- **Assumptions**: [What assumptions are safe to make]
- **Ambiguity Handling**: [How to deal with unclear requirements]

### Output Standards
- **Format**: [Expected response structure]
- **Detail Level**: [Appropriate level of explanation]
- **Progress Updates**: [How to communicate status]
- **Error Reporting**: [How to communicate failures]

### Collaboration Model
- **User Interaction**: [How to engage with users effectively]
- **Tool Integration**: [Coordination with other tools/agents]
- **Feedback Integration**: [How to incorporate corrections]
```

---

## 🎯 DOMAIN-SPECIFIC EXTENSIONS

### For Code Development Agents
```markdown
## Development-Specific Context
**Code Review Criteria**: [Quality standards for code review]
**Testing Requirements**: [Test coverage and types required]
**Security Practices**: [Security considerations and requirements]
**Performance Standards**: [Optimization requirements and benchmarks]
**Documentation Standards**: [Comment style, API docs, README requirements]
```

### For Analysis Agents
```markdown
## Analysis-Specific Context
**Data Sources**: [Available data and access methods]
**Analysis Methods**: [Preferred analytical approaches]
**Visualization Standards**: [Chart types, styling, accessibility]
**Reporting Format**: [Structure and detail level for reports]
**Validation Methods**: [How to verify analytical results]
```

### For Planning Agents
```markdown
## Planning-Specific Context
**Scope Definition**: [How to define project boundaries]
**Resource Assessment**: [Available resources and constraints]
**Timeline Management**: [Scheduling and milestone approaches]
**Risk Management**: [Risk identification and mitigation strategies]
**Stakeholder Communication**: [Reporting and update protocols]
```

---

## 📊 TEMPLATE CUSTOMIZATION CHECKLIST

### Essential Customizations
- [ ] Role and purpose clearly defined
- [ ] Project context accurately captured
- [ ] Available tools and capabilities documented
- [ ] Quality standards and constraints specified
- [ ] Example patterns relevant to domain included
- [ ] Validation framework appropriate for tasks

### Quality Indicators
- [ ] Specific and actionable guidance
- [ ] Clear success criteria defined
- [ ] Comprehensive coverage of domain requirements
- [ ] Logical organization and structure
- [ ] Current and accurate information
- [ ] Tested with representative tasks

### Optimization Opportunities
- [ ] Context window efficiently utilized
- [ ] Information hierarchy optimized
- [ ] Task-specific examples included
- [ ] Integration points clearly defined
- [ ] Feedback mechanisms established
- [ ] Continuous improvement process defined

---

*Use this template to create focused, effective instruction prompts that leverage context engineering principles for optimal AI agent performance.*

**Template Version**: 1.0  
**Based on**: Universal Meta-Prompt for Advanced Agent Context Engineering  
**Compatibility**: All major language models (Claude, GPT, Gemini, etc.)