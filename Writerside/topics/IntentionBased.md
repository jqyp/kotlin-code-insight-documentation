# IntentionBased

+ Use this factory only when `ModCommandAction.ModCommandBased` cannot be applied.  
  For example, when the action must not be invoked inside a write action.  
  A typical case is quick fixes that launch multi-step refactorings.
+ Actions that cannot be migrated to the ModCommand API may often be recognizable by:
  ```kotlin
  override fun startInWriteAction(): Boolean = false
  ```