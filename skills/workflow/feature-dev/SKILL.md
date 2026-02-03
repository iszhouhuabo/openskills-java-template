---
name: feature-dev
description: Standard feature development workflow SOP. Defines steps from database design, Entity/Mapper creation, DTO definition to Service/Controller implementation and self-check.
---

# Feature Development Workflow

<workflow_steps>
    <step sequence="1" name="Database Design">
        Design table structure. If the table does not exist, provide SQL DDL.
        Ensure it includes base fields: `id` (PK), `create_time`, `update_time`, `deleted` (logical delete).
    </step>
    
    <step sequence="2" name="Entity & Mapper">
        Create Entity class (using `@TableName`, `@TableId`).
        Create Mapper interface (extending `BaseMapper`).
    </step>
    
    <step sequence="3" name="DTO Definition">
        Define Request DTO (input parameters) and Response VO (output parameters) required for frontend interaction.
        Do not use Entity directly as a Controller parameter.
    </step>
    
    <step sequence="4" name="Service Implementation">
        Create Service class (extending `ServiceImpl`, annotated with `@Service`).
        Write business logic, prioritizing `lambdaQuery()` / `lambdaUpdate()`.
    </step>
    
    <step sequence="5" name="Controller Implementation">
        Create Controller and inject Service.
        Write interface methods and add Swagger annotations (`@Operation`).
    </step>

    <step sequence="6" name="Self Check">
        After the functional development is complete, re-check according to standards to confirm if there are any non-compliant areas. If so, fix them.
        Checkpoints include: naming conventions, code style, exception handling, logging, security, performance, etc.
        Finally, output a feature modification brief.
    </step>
</workflow_steps>