# KotlinApplicableInspectionBase.Simple

Designed for inspections that assume exactly one quick-fix, implemented as a `KotlinModCommandQuickFix<E>` suitable
when the fix involves PSI edits and/or editor-state operations available through `ModPsiUpdater`.

## Hierarchy

Inherits from [](KotlinApplicableInspectionBase.md).

## Members

`fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): KtVisitor<*, *>`
: See [
`VisitorWrappers.kt`](https://github.com/JetBrains/kotlin/blob/d2f834966243699198bc3c3163a1ae3b9cf88d02/compiler/psi/psi-utils/src/org/jetbrains/kotlin/psi/VisitorWrappers.kt).

`fun isApplicableByPsi(element: E): Boolean`
: + Checks whether the inspection is applicable to an element by PSI only.
: + Must not use the Analysis API due to performance concerns.

`fun getApplicableRanges(element: E): List<TextRange>`
: + Returns the text ranges within the given `element` where the inspection can be applied.
<note>
Returned ranges must be relative to the given <code>element</code>,
i.e., if the whole element is applicable, it should return <code>[0, element.length)</code>.
</note>

: + See [
`ApplicabilityRange`](https://github.com/JetBrains/intellij-community/blob/a97277d13173a4d5288adf460f122b3a9d56cf7c/plugins/kotlin/code-insight/api/src/org/jetbrains/kotlin/idea/codeinsight/api/applicators/ApplicabilityRange.kt#L15)
and [
`ApplicabilityRanges`](https://github.com/JetBrains/intellij-community/blob/b9f16041831650a905057a53da5aa19a748a5099/plugins/kotlin/code-insight/impl-base/src/org/jetbrains/kotlin/idea/codeinsights/impl/base/applicators/ApplicabilityRanges.kt#L14).

`fun KaSession.prepareContext(element: E): C?`
: + Prepares an Analysis API context on a physical PSI that is provided for `invoke`.
: + Must return `null` if the inspection is not applicable by analysis.
: + The prepared context must not store `KaSession` itself, raw `PsiElement` (use `SmartPsiElementPointer` instead),
or Analysis-API objects such as `KaSymbol`, `KaCall`, or `KaType` (store types as rendered strings via
`KaType.render(...)`).
: + Guaranteed to be executed from a read action.

`fun getProblemDescription(element: E, context: C): @InspectionMessage String`
: + Returns a description of the problem.
<warning>
Don’t use a fix description instead of a problem description.
</warning>
: + See also [Editor messages](https://plugins.jetbrains.com/docs/intellij/inspections.html#editor-messages).

`fun createQuickFix(element: E, context: C): KotlinModCommandQuickFix<E>`
: + Creates a quick-fix, used for PSI changes and editor updates via `ModPsiUpdater`.
