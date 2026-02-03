---
name: java-style
description: Java coding style guide, including naming conventions, imports, Lombok usage, and comment requirements.
---

# Java Coding Style Guide

<style_rules>
    <naming>
        <rule>Class names use `UpperCamelCase` (e.g., UserLoginController).</rule>
        <rule>Method and variable names use `lowerCamelCase` (e.g., getUserInfo).</rule>
        <rule>Constant names use `UPPER_SNAKE_CASE` (e.g., MAX_RETRY_COUNT).</rule>
        <rule>MyBatis Plus entity classes are recommended to map as closely as possible to database table names, or use `@TableName`.</rule>
        <rule>MyBatis Plus entity attributes should include `@TableId`, `@TableField`, and use table field comments as Doc comments.</rule>
        <rule>When referencing `Classes` from other packages, check if the corresponding package has been imported; if not, import it.</rule>
    </naming>

    <lombok_usage>
        <rule>
            Entity classes (Entity/DTO/VO) must use Lombok annotations to simplify code:
            `@Data` (or @Getter/@Setter), `@ToString`, `@NoArgsConstructor`, `@AllArgsConstructor`, `@Builder`.
        </rule>
    </lombok_usage>

    <comments>
        <rule>
            Classes and complex public methods must include Javadoc (`/** ... */`).
            Complex logic within methods uses single-line comments (`//`).
        </rule>
        <rule>
            **Mandatory Reading**. Generated code must be marked at the beginning of the modification with the comment `AI Assistant IN [Current Time]` + `Comment explaining the changes`.
            This does not include CLASS header comments and is only valid for the modified lines of code.
        </rule>
    </comments>
</style_rules>