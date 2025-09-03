# KotlinQuickFixFactory.ModCommandBased

`KotlinQuickFixFactory.ModCommandBased` is preferred, as it produces fixes based on the `ModCommandAction` API.

Below is the hierarchy of classes that might be used with KotlinQuickFixFactory.ModCommandBased:

<code-block lang="mermaid">
graph TD
  ModCommandAction --> PsiBasedModCommandAction
  PsiBasedModCommandAction --> PsiUpdateModCommandAction
  PsiBasedModCommandAction --> KotlinPsiUpdateModCommandAction
  KotlinPsiUpdateModCommandAction --> KotlinPsiUpdateModCommandAction.ElementBased
</code-block>

## PsiUpdateModCommandAction

+ Updates only the file that contains the starting element and may also adjust the editor state (caret, selection, highlighting, etc.).
+ Can be constructed in two ways:
    + **Element-based** – `PsiUpdateModCommandAction(@NotNull E element)`\
      Common for compiler-diagnostic quick fixes, as `diagnostic.psi` or a related PSI element can be passed directly.
    + **Class-based** – `PsiUpdateModCommandAction(@NotNull Class<E> elementClass)`.\
      Finds an element of the specified type at the caret offset.
+ `invoke` operates on a writable, non-physical copy of the starting element.
    + Use `updater` to perform editor-like operations—moving the caret, selecting elements, highlighting, creating templates, showing
      messages, and more (see `ModPsiUpdater`).
    + It’s not expected to call `udpater.getWritable(…)` in `invoke`: the action is intended to modify only the current file and already
      starts from a writable, non-physical copy of the element.

## KotlinPsiUpdateModCommandAction.ElementBased

+ Extends `PsiUpdateModCommandAction` with a prepared context.
  + The context is computed under a `KaSession` in the factory and stored for later use in `invoke`.
+ Store `SmartPsiElementPointers` in the context rather than raw PSI references.
+ `invoke` runs in the background on a writable, non-physical copy of the element.
    + For cross-file edits, obtain writable copies via `updater.getWritable(…)`.
    + If the context stores `SmartPsiElementPointers` to elements in the same file as the starting element, those pointers still resolve
      to physical PSI; convert them to writable non-physical PSI with `updater.getWritable(…)` before editing.

## PsiBasedModCommandAction

+ Use when edits to PSI and editor-state operations available through `ModPsiUpdater` are insufficient.
+ Supports high-level operations such as creating or deleting files, presenting a chooser for a subsequent action, updating IDE/project
  settings, and more (see `ModCommand`).
+ Can be constructed in two ways:
    + **Element-based** – `PsiUpdateModCommandAction(@NotNull E element)`\
      Common for compiler-diagnostic quick fixes, as `diagnostic.psi` or a related PSI element can be passed directly.
    + **Class-based** – `PsiUpdateModCommandAction(@NotNull Class<E> elementClass)`.\
      Finds an element of the specified type at the caret offset. 
+ Note that if you need to bind to several elements, you still need to save additional elements into fields by yourself, preferably using  
  smart pointers. It’s up to you to check the validity of these pointers.
  Also note that the element you get inside perform() is a physical element, and you cannot change it directly.

## ModCommandAction

+ Use in the same cases as `ModCommandAction` but when it is no needed to start with an element.
