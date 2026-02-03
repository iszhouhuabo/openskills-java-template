---
name: payment-logic
description: Payment and amount calculation rules. Includes precision handling, tax calculation formulas, and currency unit standards.
---

# Domain Knowledge: Payment & Currency

<business_logic domain="payment">
    <currency_handling>
        <rule>
            All amount fields must be stored in the database and code in the **smallest currency unit (cent)**, with the type `Long`.
            **Using `Double` or `Float` for amount calculations is strictly prohibited**; `BigDecimal` or integer arithmetic must be used.
        </rule>
    </currency_handling>

    <tax_calculation>
        <formula>
            Tax = Amount * 0.06 (Default VAT)
            Calculation results must be rounded down (Round Down).
        </formula>
        <code_snippet>
            // Standard calculation method
            long tax = BigDecimal.valueOf(amount)
                .multiply(new BigDecimal("0.06"))
                .setScale(0, RoundingMode.DOWN)
                .longValue();
        </code_snippet>
    </tax_calculation>
</business_logic>