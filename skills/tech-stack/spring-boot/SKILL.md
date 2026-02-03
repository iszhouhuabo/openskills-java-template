---
name: spring-boot
description: Spring Boot framework standards. Includes Bean management, Dependency Injection (DI), and JSON processing rules.
---

# Spring Boot Guidelines

<framework_rules>
    <dependency_injection>
        <rule type="mandatory">
            **Constructor Injection Preference**.
            Lombok's `@RequiredArgsConstructor` combined with `private final` fields is recommended.
            **Strictly prohibit** using `@Autowired` for field injection.
        </rule>
        <example>
            @Service
            @RequiredArgsConstructor
            public class UserServiceImpl { 
                private final UserMapper userMapper;
            }
        </example>
    </dependency_injection>

    <json_handling>
        <rule>
            Use `LocalDateTime` uniformly for time formats.
            Global Jackson serialization must be configured, with the output format unified as `yyyy-MM-dd HH:mm:ss`.
        </rule>
    </json_handling>
</framework_rules>