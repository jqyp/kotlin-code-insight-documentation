# Inspections

Inspections detect problems and suggest fixes based on coding standards, performance concerns, or best practices.

For more information, see [Code inspections](https://www.jetbrains.com/help/idea/code-inspection.html).

## Location

+ `kotlin.code-insight.inspections.k2`
+ `kotlin.code-insight.inspections.shared`

## Registration

+ `resources/kotlin.code-insight.inspections.k2.xml`
+ `resources/kotlin.code-insight.inspections.shared.xml`

## Guidelines
See [UI Guidelines → Writing UI Texts → Inspections](https://plugins.jetbrains.com/docs/intellij/inspections.html)
for conventions on how to write inspection-related texts shown in the IDE.

## Inspection class hierarchy

<code-block lang="mermaid">
graph TD
  LocalInspectionTool --> KotlinApplicableInspectionBase
  LocalInspectionTool --> AbstractKotlinInspection
  KotlinApplicableInspectionBase --> KotlinApplicableInspectionBase.Simple
</code-block>

## Choosing the right base class

Inspections should be implemented using the most specific class possible.  
Whenever possible, quick-fixes should be implemented using the ModCommand API,
since it provides safer PSI modifications and automatically supports intention preview.

1. [KotlinApplicableInspectionBase.Simple](KotlinApplicableInspectionBase-Simple.md) – preferred when exactly one quick-fix is needed (ModCommand-based).
2. [KotlinApplicableInspectionBase](KotlinApplicableInspectionBase.md) – for none or multiple quick-fixes (ModCommand-based).
3. [AbstractKotlinInspection](AbstractKotlinInspection.md) – use when quick-fixes cannot be `KotlinModCommandQuickFix`.
4. [LocalInspectionTool](LocalInspectionTool.md) – lowest-level fallback.