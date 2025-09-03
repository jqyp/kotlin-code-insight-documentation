# KotlinApplicableInspectionBase

Intended for inspections that assume **none or multiple quick-fixes**, implemented as a `KotlinModCommandQuickFix<E>`
suitable when the fix involves PSI edits and/or editor-state operations available through `ModPsiUpdater`.

## Hierarchy

Inherits from [](LocalInspectionTool.md).   

## Members

For descriptions of the methods below, see [KotlinApplicableInspectionBase.Simple](KotlinApplicableInspectionBase-Simple.md#members)

| Methods |
|---------|
|`fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): KtVisitor<*, *>`
|`fun isApplicableByPsi(element: E): Boolean`
|`fun getApplicableRanges(element: E): List<TextRange>`
|`fun KaSession.prepareContext(element: E): C?`
|`fun getProblemDescription(element: E, context: C): @InspectionMessage String`


`fun InspectionManager.createProblemDescriptor(element: E, context: C, rangeInElement: TextRange?, onTheFly: Boolean): ProblemDescriptor`
: + Override to provide a custom `ProblemDescriptor`.
: + For inspections with a single quick-fix and no custom descriptor logic, use [](KotlinApplicableInspectionBase-Simple.md) instead.