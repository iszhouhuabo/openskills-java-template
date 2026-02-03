---
name: rule
description: 'General project R&D standards, architecture rules, and core practices.'
---
# Corporate Code Redlines

This file defines globally applicable absolute redlines. These rules must be followed regardless of which Skill is loaded.

<critical_rules>
    <security>
        <rule id="SEC-01" level="blocker">
            **Hardcoding keys is prohibited**. Passwords, Secret Keys, and Tokens are strictly forbidden in code.
            Must use `@Value("${...}")` or a configuration center.
        </rule>
        <rule id="SEC-02" level="blocker">
            **SQL Injection Prevention**.
            1. Using `${param}` in Mybatis XML is strictly forbidden; `#{param}` must be used.
            2. When using Mybatis Plus Wrapper, strictly avoid concatenating SQL like `apply("id = " + id)`; placeholders must be used: `apply("id = {0}", id)`.
        </rule>
        <rule id="SEC-03" level="blocker">
            **Prohibit regenerating existing `Mapper` or `Service` classes**.
            First, check if the required Mapper/Service already exists in the project. If it does, modify the existing class directly instead of generating a new file.
        </rule>
    </security>

    <logging>
        <rule id="LOG-01" level="critical">
            **Prohibit the use of System.out**. Slf4j (`log.info`, `log.error`) must be used.
        </rule>
        <rule id="LOG-02" level="critical">
            **Exception Logging Standards**. When catching exceptions, stack trace information must be recorded: `log.error("Error message", e)`.
            Strictly avoid swallowing exceptions (Empty Catch Block) or only printing `e.getMessage()`.
        </rule>
    </logging>

    <performance>
        <rule id="PERF-01" level="major">
            **Prohibit database queries in loops**.
            Strictly avoid calling Mapper/Service for database queries within `for/foreach` loops.
            Collect ID lists outside the loop, use `listByIds` or `in` queries, then assemble data in memory using a Map.
        </rule>
    </performance>
</critical_rules>