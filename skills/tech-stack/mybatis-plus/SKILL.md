---
name: mybatis-plus
description: MyBatis Plus best practices and SQL security standards. Includes mandatory LambdaWrapper usage and Service CRUD standards.
---

# MyBatis Plus & MySQL Guidelines

<orm_rules>
    <basic_usage>
        <rule>
            Service classes must extend `ServiceImpl<Mapper, Entity>` to directly use CRUD methods provided by MP.
        </rule>
        <rule>
            Mapper interfaces must extend `BaseMapper<Entity>`.
        </rule>
    </basic_usage>

    <query_wrapper>
        <rule type="mandatory">
            **LambdaWrapper must be used** (`LambdaQueryWrapper`, `LambdaUpdateWrapper`).
            **Strictly prohibit** using the regular `QueryWrapper` with hardcoded string field names ("user_name") to prevent refactoring omissions.
        </rule>
        <example_correct>
            // Good
            lambdaQuery().eq(User::getName, "Alice").list();
        </example_correct>
        <example_wrong>
            // Bad
            query().eq("name", "Alice").list();
        </example_wrong>
    </query_wrapper>

    <sql_spec>
        <rule>
            For complex SQL (joins involving more than 3 tables), it is recommended to write manual XML instead of forcing concatenation with Wrappers.
        </rule>
        <rule>
            Logical deletion must use MP's `@TableLogic`.
        </rule>
    </sql_spec>
</orm_rules>