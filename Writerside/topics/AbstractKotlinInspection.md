# AbstractKotlinInspection

Preferred in cases where the quick-fix cannot be implemented as a `KotlinModCommandQuickFix`.  
For example, when the action must not be invoked inside a write action—common in quick-fixes that launch multi-step
refactorings.  
Also used when PSI edits and editor-state operations available through `ModPsiUpdater` are not sufficient, and
operations such as creating or deleting files, presenting a chooser for a subsequent action, updating IDE/project
settings, and more are required (see `ModCommand`).

## Hierarchy

Inherits from [](LocalInspectionTool.md).

## Members

`fun buildVisitor(holder: ProblemsHolder, isOnTheFly: Boolean): PsiElementVisitor`
: + Provides a PSI visitor for the inspection.
: + Created visitor must not be recursive.
: + Visitor created must be thread-safe.
: + If the inspection should not run in the given context, return `PsiElementVisitor#EMPTY_VISITOR`
: + `holder` is where problems are registered.
: + `isOnTheFly` is true when the inspection runs in the editor (not in batch mode).
: + See [
`VisitorWrappers.kt`](https://github.com/JetBrains/kotlin/blob/d2f834966243699198bc3c3163a1ae3b9cf88d02/compiler/psi/psi-utils/src/org/jetbrains/kotlin/psi/VisitorWrappers.kt).
