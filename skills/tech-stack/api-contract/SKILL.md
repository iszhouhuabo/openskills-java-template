---
name: api-contract
description: Web interface contract, including DTO/VO definitions and exception handling.
---

# API Design Contract

<api_rules>
    <controller>
        <rule>Use `@RestController` and `@RequestMapping`.</rule>
        <rule>
            HTTP Method semantics must be accurate:
            - GET: Query
            - POST: Create/Complex Query
            - PUT: Full Update
            - DELETE: Delete
        </rule>
        <rule>If existing Controller interfaces use `@Log` to mark logs, follow the pattern and add it; otherwise, do not add it.</rule>
    </controller>

    <size_format>
        <rule>
            All interfaces must return a unified wrapper class `HttpResult<T>`.
            Fields include: `code` (int), `msg` (String), `data` (T).
            Check the specific success and failure methods in the `HttpResult<T>` entity for usage.
        </rule>
    </size_format>

    <exception_handling>
        <rule>
            Prohibit try-catch in Controllers.
            Exceptions should be thrown uniformly and captured/transformed by a `@RestControllerAdvice` global exception handler.
        </rule>
    </exception_handling>
</api_rules>