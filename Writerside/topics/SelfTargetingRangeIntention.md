# SelfTargetingRangeIntention

The same as [](SelfTargetingIntention.md), which it inherits from, but allows restricting applicability to a specific
text range within the element.  
Use when the intention should apply only to part of an element (e.g., an initializer, operator, or keyword), rather 
than the whole PSI element.

applicabilityRange(element: TElement): TextRange?
Returns the text range within the element where the intention is applicable.

## Members

For descriptions of the methods below, see [](SelfTargetingIntention.md)

| Methods |
|---------|
|`startInWriteAction()`
|`isApplicableTo(element, caretOffset)`
|`applyTo(element, editor)`

`applicabilityRange(element: TElement): TextRange?`
: + Returns the text range within the element where the intention is applicable.