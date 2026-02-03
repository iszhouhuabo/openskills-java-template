---
name: layer-rules
description: Core layered architecture rules. Defines responsibility boundaries and call restrictions for Controller, Service (No-Interface mode), and Mapper.
---

# Layered Architecture Rules (No-Interface Mode)

<architecture_constraints>
    <layer name="Controller (Web)">
        <responsibility>Responsible only for parameter parsing, validation (`@Valid`), and response encapsulation.</responsibility>
        <forbidden>Strictly avoid business logic. Strictly avoid direct calls to Mapper.</forbidden>
        <dependency>Can only depend on Service classes.</dependency>
    </layer>

    <layer name="Service (Business)">
        <definition>
            **No Interface Pattern**: This project adopts a lightweight architecture.
            **Do not** define `IUserService` interfaces; directly define `UserService` classes and add the `@Service` annotation.
            If transactions are needed, add `@Transactional` directly to public methods.
        </definition>
        <responsibility>Business logic orchestration, transaction control, and conversion between Entity and DTO/VO.</responsibility>
        <dependency>Depends on Mapper or other Services.</dependency>
    </layer>

    <layer name="Mapper/Repository (Data)">
        <responsibility>Extends `BaseMapper<T>`, responsible for interacting with MySQL.</responsibility>
        <forbidden>Strictly avoid processing business logic.</forbidden>
    </layer>
</architecture_constraints>