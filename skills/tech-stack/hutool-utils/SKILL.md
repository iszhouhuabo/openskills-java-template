---
name: hutool-utils
description: Utility library usage standards. Prioritize Hutool (cn.hutool), prohibit reinventing the wheel or using Apache Commons.
---

<library_rules>
<principle>
<rule type="mandatory">
Hutool First: When involving operations such as strings, collections, dates, JSON, encryption, HTTP requests, etc., you must prioritize using static utility classes provided by Hutool.
Introduction of Apache Commons Lang/Collections or Guava is prohibited unless Hutool cannot satisfy the requirement (extremely rare).
</rule>
<rule>
Do not reinvent the wheel: Manually creating classes like StringUtils, DateUtils, etc., in the project is strictly forbidden. Use Hutool's StrUtil, DateUtil, etc., directly.
</rule>
</principle><mapping>
<description>Common functions and Hutool class mapping table</description>
<item func="String" class="cn.hutool.core.util.StrUtil" example="StrUtil.isBlank(str)" />
<item func="Collection" class="cn.hutool.core.collection.CollUtil" example="CollUtil.isNotEmpty(list)" />
<item func="Map" class="cn.hutool.core.map.MapUtil" example="MapUtil.newHashMap()" />
<item func="Date/Time" class="cn.hutool.core.date.DateUtil" example="DateUtil.now()" />
<item func="JSON" class="cn.hutool.json.JSONUtil" example="JSONUtil.toJsonStr(obj)" />
<item func="Bean Copy" class="cn.hutool.core.bean.BeanUtil" example="BeanUtil.copyProperties(source, target)" />
<item func="Assert" class="cn.hutool.core.lang.Assert" example="Assert.notNull(user, 'User cannot be null')" />
<item func="Encryption" class="cn.hutool.crypto.SecureUtil" example="SecureUtil.md5(str)" />
<item func="ID Generation" class="cn.hutool.core.util.IdUtil" example="IdUtil.getSnowflakeNextId()" />
</mapping>

<examples>
    <example_correct>
        // Good: Using Hutool for null checks and bean copying
        if (StrUtil.isBlank(username)) {
            return Result.fail("Username cannot be empty");
        }
        
        List<String> list = CollUtil.newArrayList();
        
        // Date processing
        String today = DateUtil.today();
        
        // DTO transformation
        UserDTO dto = BeanUtil.copyProperties(userEntity, UserDTO.class);
    </example_correct>
    
    <example_wrong>
        // Bad: Hand-written judgment logic
        if (username == null || username.trim().isEmpty()) { ... }
        
        // Bad: Introducing Apache Commons
        import org.apache.commons.lang3.StringUtils;
        if (StringUtils.isBlank(str)) { ... }
        
        // Bad: Encapsulating utility classes yourself
        public class MyDateUtils { ... }
    </example_wrong>
</examples>
</library_rules>