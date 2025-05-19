# therapylang - a programming language based on emotion-driven development

Created by Jasmine Angle :D

## Interpreter Behavior

- The interpreter must track varying state variables, each of which describe varying aspects of one's thoughts (feelings, insights, memories, etc.). Variables are loosely typed, in that a memory can become a feeling, though if this occurs and no other reference exists, then the variable is deemed overwritten.
- Truth values are logically evaluated when a reflection occurs.
- In a similar manner as general cognitive patterns, ruminations persist until a resolution is reached.
- Breakthroughs represent successful program exits. If there is a session that does not have an understood truth, the session remains unresolved and raises an error.

## Feelings (Integers)

Feelings are a type of `variable` that represents emotional intensity as an integer, which can be affected by insights later.

### Declaration

| Phrase | Meaning |
|--------|---------|
|`I acknowledge my <feeling_name> [is at <value>].` | Declares a new feeling with the provided `feeling` name with an optional default value. Feelings can be re-acknowledged, in which case their value is reset. |

#### Example
```
I acknowledge my sadness.
I acknowledge my happiness is at 0.
```

## Insights (Operation Results)

Insights are computed realizations, which are derived from reflecting on feelings, memories, or truths, to eventually provide clarity. Floating point values are supported.

```
I realize <expression> as <insight_name>.
```

### Supported Operations

- Arithmetic: `+`, `-`, `*`, `/`, `//`, `%`
- Comparisons: `==`, `!=`, `>`, `<`, `>=`, `<=`
- Bitwise: `&`, `|`, `^`
- Truth Logic: `and`, `or`, `not`

### Functions

A few functions exist to conveniently work with differing data types.

- `length of <x>`: Retrieves either the number of moments in a memory, or the number of characters in a wound.
- `sum of <x>`: Computes the sum of all feelings in a memory.
- `max of <x>`: Computes the maximum of all feelings in a memory.
- `min of <x>`: Computes the minimum of all feelings in a memory.

## Wounds (Strings)

Wounds represent textual symbolism of psychological pain points, which can be erased or shared with the Therapist. 

### Declaration

| Phrase | Meaning | Example |
|--------|---------|---------|
|`I carry a wound called <wound>.` | Declares a new wound with the provided `wound` name. | `I carry around a wound called abandonment.`
|`I carry a wound called <wound> that says "".` | Declares a new wound with the provided `wound` name and a default string value. | `I carry around a wound called abandonment that says "They left when I needed them the most".`

### Example

```
Session begins.

I carry a wound called core_wound that says "I was always compared to others".
I carry a wound called shame.

I open my wound shame and speak "I failed when it mattered most".
I open my wound shame and whisper ", and I haven't forgiven myself".

My therapist asks: "Would you like to talk about your shame?" => consent.
I realize consent_is_yes as consent == "yes".

When I reflect on consent_is_yes,
I realize:
  I tell my therapist: shame.
Otherwise:
  I choose not to speak of shame.

I let go of core_wound.

I accept that I'm_enough might be true.
I finally understand I'm_enough.
Session ends.
```

### Assignment and Modification

Wounds are mutable in that they can be opened and spoken to, appending the new text to the wound.

### Unspoken Wounds

If there is wound that should be suppressed, or blocked from disclosure to the Therapist, you may choose not to speak of the wound. This only marks the wound as *unspoken*.

| Phrase | Meaning | Example |
|--------|---------|---------|
|`I choose not to speak of <wound>.` | Blocks the wound from disclosure to the Therapist. | `I choose not to speak of abandonment.`
|`I'm ready to speak of <wound>.` | Allows the wound to be disclosed to the Therapist. | `I'm ready to speak of abandonment.`

## Reflections (Conditionals)

Reflections are introspective thoughts, contemplating a variable and acting upon it in varying ways. Reflections evaluate the *truthiness* of a variable, which is defined as follows:
- Feelings and Insights: Non-zero value is true.
- Truths: Direct evaluation to true/false.

```
When I reflect on <feeling|truth>,
I realize:
  <statements>
Otherwise:
  <statements>
```

The `Otherwise` keyword is not required for introspective thoughts that do not require an action upon a truthy reflection. Nested reflections are permitted.

## Memories (Arrays)

Memories are a type of `variable` that represents one's past events through holding moments. Memories are one-indexed to match natural language conventions. Memories are capable of holding nested memories and can hold varying data types.

### Declaration

| Phrase | Meaning | Example |
|--------|---------|---------|
|`I remember <memory> vividly.` | Declares a new memory with the provided `memory` name. | `I remember childhood_trauma vividly.`

### Assignment and Modification

When adding variables to a memory, there are certain conventions followed for pointer or reference copying:

| Type | Handling |
|--------|---------|
| Feelings | The feeling's value is copied into the memory and no reference is preserved. |
| Memories | A reference to the nested memory is copied into the memory. Both the memory name and the item index within the parent memory will refer to the same underlying memory. Re-remembering the original nested memory name will not delete the memory that exists in the parent memory. |

| Phrase | Meaning | Example |
|--------|---------|---------|
|`I recall <variable\|value> and add it to <memory>` | Appends any type of sudden or defined variable to the end of `memory`.  | `I recall sadness and add it to therapy_sessions` |
|`I revisit the <index>th moment in <memory> as <variable>` | Retrieves a moment from the specified `memory` at a given `index` and stores it in `variable`. If `variable` is not defined, it will be created with the appropriate type. | `I revisit the 2nd moment in therapy_sessions as past_sadness` |
|`I choose to forget the <index>th moment in <memory>` | Removes a moment in `memory` at a given `index`. | `I choose to forget the 1st moment in therapy_sessions` |
|`I replace the <index>th moment in <memory> with <variable\|value>` | Overwrites a moment in `memory` at a given `index` with any type of sudden or defined variable. | `I replace the 1st moment in childhood_trauma with "forgiveness"` |

### Example

```
Session begins.

I acknowledge my anxiety.
I remember therapy_sessions vividly.

My anxiety increases by 3.
I recall anxiety_level and add it to therapy_sessions.

My anxiety decreases by 1.
I recall anxiety_level and add it to therapy_sessions.

I revisit the 1st moment in therapy_sessions as first_anxiety.
I tell my therapist: first_anxiety.

I accept that I'm_enough might be true.
I finally understand I'm_enough.
Session ends.
```

## Rumination (Loops)

Loops are supported through rumination and introspective thought, similar to reflections, though requiring a truth.

```
Until I accept <truth>,
I keep thinking:
  <statements>
```

## Letting Go (Variable Deletion)

To delete a variable, therapylang supports letting go of feelings, memories, wounds, and truths. This can be particularly useful for sifting through a memory that holds varying types.

| Phrase | Meaning |
|--------|---------|
|`I let go of <variable>.` | Deletes a `variable` within the current scope. |

## Breakthroughs

To end the session gracefully, a truth must have been understood by the end. A concise means of understanding a truth to end a session is as follows, though any truth that is determined throughout execution is acceptable:

```
I accept that I'm_enough might be true.
I finally understand I'm_enough.
Session ends.
```

## Therapist Interaction (I/O)

Throughout the execution of your program, the Therapist (`STDOUT`) is always available to talk to. The Therapist can also ask you questions (`STDIN`).

| Phrase | Meaning |
|--------|---------|
|`I tell my therapist: <value>` | Discloses any value to the Therapist (`STDOUT`). |
|`My therapist asks: <question> => <insight>` | Prompts a response from you via `STDIN` to give to the Therapist. Your response is stored as a wound. |
