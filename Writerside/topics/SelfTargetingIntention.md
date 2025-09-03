# SelfTargetingIntention

Use this class only when the intention cannot be implemented using the ModCommand API.  
For example, when the intention must not be invoked inside a write action.  
A typical case is quick-fixes that launch multi-step refactorings.  
Use when applicability can be determined for the whole PSI element.

+ `startInWriteAction()`
    + Indicates whether the action must be invoked inside a write action.
    + Typically, you will override it as:
    ```kotlin
    override fun startInWriteAction(): Boolean = false
    ```
because `SelfTargetingIntention` are usually used when the ModCommand API—where actions already start in a write
action—cannot be used.
+ `isApplicableTo(element, caretOffset)`
    + Checks whether it is applicable to an element at caret offset.
+ `applyTo(element, editor)`
    + Performs changes over a physical PSI.