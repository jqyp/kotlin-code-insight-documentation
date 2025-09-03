# Intentions

Intentions are actions that a user can apply to change code, such as specifying a variable type or adding an argument name.
They can be considered small refactoring actions.

For more information, see [Intention actions](https://www.jetbrains.com/help/idea/intention-actions.html).

## Intentions vs. inspections

Code inspections also provide quick-fixes, but they have a different purpose.

Intention actions help improve your code or make it more efficient. These are not necessarily errors or warnings but rather improvements, optimizations, or helpful transformations.

Inspections detect problems and suggest fixes based on coding standards, performance concerns, or best practices. For more information, refer to Code inspections.

## Location
+ `kotlin.code-insight.intentions.k2`
+ `kotlin.code-insight.intentions.shared`

## Registration

+ `resources/kotlin.code-insight.intentions.k2.xml`
+ `resources/kotlin.code-insight.intentions.shared.xml`

## Intention class hierarchy

<code-block lang="mermaid">
graph TD
  PBMCA[PsiBasedModCommandAction] --> KPUMCA[KotlinPsiUpdateModCommandAction]
  KPUMCA --> KPUMCA_CB[KotlinPsiUpdateModCommandAction.ClassBased]
  KPUMCA_CB --> KAMCA[KotlinApplicableModCommandAction]
  KAMCA --> KAMCA_S[KotlinApplicableModCommandAction.Simple]
  PBMCA --> PUMCA[PsiUpdateModCommandAction]
</code-block>
