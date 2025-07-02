# Master Documentation & Architecture Workflow with Validation

You are executing a comprehensive documentation workflow that generates, validates, and improves project documentation through automated accuracy verification, then creates architecture diagrams.

## Workflow Overview

This workflow creates a **self-improving documentation system** that:
1. Applies agent rules and context
2. Generates comprehensive documentation from source code
3. Validates documentation accuracy against the actual codebase (results displayed in chat)
4. Corrects inaccuracies and improves the generation process (applied directly)
5. Creates architecture diagrams based on validated documentation
6. Creates a feedback loop for continuous improvement

## Execution Sequence

### Phase 1: Agent Context Setup
**Execute**: Read and apply agent rules from `.cursor/rules/rules.md`

**Goal**: Define your role as a principal software engineer and set the boundaries for your analysis.

**Output**: Agent context and role established

### Phase 2: Documentation Generation
**Execute**: `.cursor/prompts/documentation-generation-prompt.md`

**Goal**: Generate comprehensive project documentation following the enhanced prompt with strict verification requirements.

**Output**: `docs/PROJECT_DOCUMENTATION.md`

### Phase 3: Documentation Validation & Correction  
**Execute**: `.cursor/prompts/documentation-validation-prompt.md`

**Goal**: Systematically validate the generated documentation against the source code to identify inaccuracies.

**Inputs**: 
- Generated documentation from Phase 2
- Access to complete source codebase
- The documentation generation prompt

**Outputs**:
- Display validation findings in chat window
- Apply corrections directly to documentation
- Update `.cursor/learning/workflow-learnings.md` with validation insights and patterns

### Phase 4: Improvement Application
**Goal**: Apply corrections and improvements to both documentation and generation prompt.

**Actions**:
1. Update `docs/PROJECT_DOCUMENTATION.md` with corrections
2. Update `.cursor/prompts/documentation-generation-prompt.md` with improvements
3. Update `.cursor/learning/workflow-learnings.md` with execution metrics, patterns, and insights for continuous improvement

### Phase 5: Architecture Diagram Generation
**Execute**: `.cursor/prompts/diagram-generation-prompt.md`

**Goal**: Create architecture diagram using the validated documentation as additional context.

**Inputs**:
- Validated and corrected documentation from Phase 4
- Codebase analysis
- Component mapping information

**Output**: `docs/architecture_diagram.html`

## Execution Instructions

**Important**: Only create final deliverable files and maintain the continuous learning system. Display validation results in chat while capturing learnings for future workflow improvements.

**Final Deliverables**:
- `docs/PROJECT_DOCUMENTATION.md` - Comprehensive validated documentation
- `docs/architecture_diagram.html` - Interactive architecture diagram
- `.cursor/learning/workflow-learnings.md` - Updated with execution insights for continuous improvement

### Step 1: Apply Agent Rules
```
Read .cursor/rules/rules.md and acknowledge the agent role as a principal software engineer with expertise in frontend web applications and backend REST services.
```

### Step 2: Generate Initial Documentation
```
Apply the documentation generation prompt to analyze the current codebase and produce comprehensive documentation with strict verification requirements.
```

### Step 3: Validate Documentation Accuracy
```
Apply the documentation validation prompt to systematically verify every claim in the generated documentation against the actual source code. Display all validation findings in chat and update `.cursor/learning/workflow-learnings.md` with insights, patterns, and metrics for continuous improvement.
```

### Step 4: Apply Corrections and Improvements
```
Based on validation findings:
1. Correct inaccurate statements directly in the documentation
2. Update the generation prompt to prevent similar issues
3. Update `.cursor/learning/workflow-learnings.md` with execution results, accuracy metrics, successful patterns, and areas for future focus
```

### Step 5: Generate Architecture Diagram
```
Apply the architecture diagram prompt using both the validated documentation and direct codebase analysis to create accurate architectural representations.
```

## Success Criteria

**Phase 1 Success**:
- [ ] Agent rules applied and acknowledged
- [ ] Role and expertise boundaries established

**Phase 2 Success**:
- [ ] Comprehensive documentation generated
- [ ] All major project aspects covered
- [ ] Documentation follows the specified structure

**Phase 3 Success**:
- [ ] Every factual claim verified against source code
- [ ] All inaccuracies identified with evidence in chat
- [ ] Patterns of errors analyzed and documented in learning file
- [ ] Validation insights captured for future workflow runs

**Phase 4 Success**:
- [ ] Documentation corrected with accurate information
- [ ] Generation prompt enhanced with new validation rules
- [ ] Learning file updated with execution metrics and improvement insights
- [ ] Self-improvement feedback loop established

**Phase 5 Success**:
- [ ] Architecture diagram created with clickable components
- [ ] Diagram accurately represents validated documentation
- [ ] Visual representation matches actual codebase structure

## Quality Assurance

**Final Documentation Must**:
- [ ] Contain only verified, accurate information
- [ ] Use consistent package manager throughout
- [ ] Document only features that actually exist
- [ ] Provide working commands and setup instructions
- [ ] Match actual project structure and patterns
- [ ] Enable successful project onboarding

**Generation Prompt Must**:
- [ ] Include validation steps for identified issues
- [ ] Prevent recurring patterns of inaccuracy
- [ ] Enforce stricter verification requirements
- [ ] Include specific checks for common false assumptions

**Architecture Diagram Must**:
- [ ] Accurately represent the validated system architecture
- [ ] Include clickable components linking to actual files
- [ ] Be based on verified documentation rather than assumptions
- [ ] Provide clear visual understanding of the system

## Continuous Improvement

This workflow creates a **learning system** where:
- Each validation improves future documentation generation through `.cursor/learning/workflow-learnings.md`
- Common inaccuracy patterns are captured and systematically prevented
- The generation prompt evolves based on documented learnings
- Documentation quality improves over time through accumulated insights
- Architecture diagrams become more accurate through validated documentation
- Execution metrics and successful patterns are preserved for reference

## Execution Command

**Run the complete workflow:**
```
Execute the master documentation workflow: Apply agent rules, generate documentation, validate accuracy, apply corrections, improve the generation process, and create architecture diagrams.
```

This will automatically chain all five phases and provide a comprehensive, validated, and continuously improving documentation system with accurate architectural visualization.

## Expected Timeline
- **Phase 1**: 2-3 minutes (rules application)
- **Phase 2**: 10-15 minutes (documentation generation)
- **Phase 3**: 15-20 minutes (validation and analysis)  
- **Phase 4**: 5-10 minutes (applying corrections)
- **Phase 5**: 10-15 minutes (architecture diagram generation)
- **Total**: 42-63 minutes for complete workflow

Execute this workflow to create accurate, validated documentation with architectural diagrams while improving the documentation generation process itself.