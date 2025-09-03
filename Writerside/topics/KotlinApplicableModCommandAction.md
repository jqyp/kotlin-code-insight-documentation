# KotlinApplicableModCommandAction

`fun isApplicableByPsi(element: E): Boolean`
: + Checks whether the intention is applicable to an element by PSI only.
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
: + Must return `null` if the intention is not applicable by analysis.
: + The prepared context must not store `KaSession` itself or Analysis-API objects such as `KaSymbol`, `KaCall`,
or `KaType` (store types as rendered strings via `KaType.render(...)`).
<note>If you store context elements from the same file as the starting element, raw PSI is sufficient,
whereas elements from other files must be stored as <code>SmartPsiElementPointer</code>.
<code>prepareContext</code> is first called on the physical element to check whether the intention is applicable,
but the context is recomputed on a writable, non-physical copy of that element right before <code>invoke</code>.
</note>
: + Guaranteed to be executed from a read action.

`fun invoke(actionContext: ActionContext, element: E, elementContext: C, updater: ModPsiUpdater)`
: + `actionContext` — ModCommand action context.
: + `element` — writable, non-physical PSI.
: + `elementContext` — Analysis-API context.
: + Use `updater` (see `ModPsiUpdater`) to perform editor-like actions like moving caret or highlighting an element.

`fun getPresentation(context: ActionContext, element: E): Presentation?`
: + Provides the whole presentation, including an action text, icon, priority, and highlighting in the editor.
: + The default implementation of `getPresentation` is `Presentation.of(familyName)`.
: + To customize a `ModCommandAction`’s priority, use `Presentation#withPriority`.
Do **not** use the `HighPriorityAction` and `LowPriorityAction` marker interfaces,
as they have no effect on `ModCommandAction`s.  
In addition to an icon, you can also specify a **"Fix all"** option and the ranges to highlight in the current file.

## KotlinApplicableModCommandAction.Simple

A special case of `KotlinApplicableModCommandAction` for intentions that do not require the Analysis API.
