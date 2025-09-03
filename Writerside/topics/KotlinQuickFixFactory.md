# KotlinQuickFixFactory

There are two functional interfaces that extend `KotlinQuickFixFactory` and produce quick fixes from diagnostics:

+ [KotlinQuickFixFactory.ModCommandBased](ModCommandBased.md) — creates `List<ModCommandAction>` (preferred).
+ [KotlinQuickFixFactory.IntentionBased](IntentionBased.md) — creates `List<IntentionAction>`.

These interfaces are parameterized by a diagnostic type (`KaDiagnosticWithPsi`) and define a single method, 
`createQuickFixes(diagnostic)`, which returns a list of quick-fix actions.
