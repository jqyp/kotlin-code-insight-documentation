# Quick fixes

Quick-fixes are actions provided by the plugin for errors and warnings reported by the compiler.

## Location
+ `kotlin.code-insight.fixes.k2`
+ `kotlin.fir.frontend-independent`

## Registration

Register factories for compiler diagnostics in [`KotlinK2QuickFixRegistrar`](https://github.com/JetBrains/intellij-community/blob/25a174000aa36aad04dcbc083d2a465d079a3329/plugins/kotlin/code-insight/fixes-k2/src/org/jetbrains/kotlin/idea/k2/codeinsight/fixes/KotlinK2QuickFixRegistrar.kt#L19).

## Quick-fix factories

There are two ways to register a quick-fix factory:

1. `registerFactory(factory)`
    - where `factory` is [](KotlinQuickFixFactory.md):
        - `KotlinQuickFixFactory.ModCommandBased`, which is preferable as the one that produces a list of `ModCommandActions`:
            - Use `PsiUpdateModCommandAction` if no element context is required.
            - Use `KotlinPsiUpdateModCommandAction.ElementBased` if an element context is required.
        - `KotlinQuickFixFactory.IntentionBased`, which produces a list of `IntentionAction`.

2. `registerPsiQuickFixes(diagnosticClass, factories)`
    - `factories` are pure PSI-based quick-fix factories that do not make Analysis API calls.
      They can be created using `quickFixesPsiBasedFactory`.
    - You should use this way only in cases of reusing quick fixes for K2 that have already been implemented for K1.
