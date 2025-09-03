# PsiBasedModCommandAction

+ Use when edits to PSI and editor-state operations available through `ModPsiUpdater` are insufficient.
+ Supports high-level operations such as creating or deleting files, presenting a chooser for a subsequent action, updating IDE/project
  settings, and more (see `ModCommand`).
+ Constructed via class-based `PsiBasedModCommandAction(@NotNull Class<E> elementClass)` super-constructor which finds an
  element of the specified type at the caret offset.
+ Note that if you need to bind to several elements, you still need to save additional elements into fields by yourself,
  preferably using smart pointers. It’s up to you to check the validity of these pointers.
  Also note that the element you get inside `perform()` is a physical element, and you cannot change it directly.