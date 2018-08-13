# Linter Rules

This chapter includes the static code analysis rules which we are using in our projects.

## iOS

We use SwiftLint as Linter with its default rules and some configurations.

SwiftLint details can be found on: https://github.com/realm/SwiftLint 

In addition to default rules, our configurations to default rules and additional rules can be seen below. 
These rules should be added to project in a "**.swiftlint.yml**" file.

    disabled_rules:
       - trailing_whitespace
       - cyclomatic_complexity
       - redundant_string_enum_value

    opt_in_rules:
       - attributes
       - array_init
       - closure_end_indentation
       - closure_spacing
       - contains_over_first_not_nil
       - convenience_type
       - discouraged_object_literal
       - empty_count
       - empty_string
       - explicit_enum_raw_value
       - explicit_init
       - fallthrough
       - fatal_error_message
       - first_where
       - force_unwrapping
       - function_default_parameter_at_end
       - implicit_return
       - implicitly_unwrapped_optional
       - literal_expression_end_indentation
       - modifier_order
       - multiline_arguments
       - multiline_function_chains
       - multiline_parameters
       - operator_usage_whitespace
       - overridden_super_call
       - prohibited_super_call
       - required_enum_case
       - sorted_first_last
       - switch_case_on_newline
       - trailing_closure
       - unavailable_function
       - unneeded_parentheses_in_closure_argument
       - vertical_parameter_alignment_on_call
       - yoda_condition

     excluded:
       - Pods

     line_length: 120
     function_body_length: 100
     type_body_length: 400
     file_length: 1000
     type_name:
       min_length: 1
       max_length: 60
     identifier_name:
       min_length: 1
       max_length: 60 

All rules and explanations with examples can be found in: https://github.com/realm/SwiftLint/blob/master/Rules.md 

## Android

We combine four tools for static code analysis

For Java,
  * [Checkstyle](http://checkstyle.sourceforge.net/)
    * Up to date ruleset for CheckStyle is [here](https://github.com/Hipo/android-base-project/blob/master/config/quality/checkstyle.xml)
  * [PMD](https://pmd.github.io/)
    * We are using the default ruleset for now

For Kotlin,
  * [Detekt](https://arturbosch.github.io/detekt/)
    * Up to date ruleset for Detekt is [here](https://github.com/Hipo/android-base-project/blob/master/config/quality/detekt-config.yml)
  * [Ktlint](https://ktlint.github.io/)
    * We are using the default ruleset for now
