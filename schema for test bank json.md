# Test Bank JSON Schema

A test bank is either an array of question objects or an object with a `questions` array. Keep question wording, choices, answer keys, explanations, and topic metadata in the bank JSON. The quiz page supports four answer formats:

| `question_type` | Required answer data | How it is answered |
|---|---|---|
| `multiple_choice` | `choices` with at least two strings | Select one choice |
| `true_false` | `choices` containing true/false values, such as `["True", "False"]` | Select True or False |
| `fill_in_blank` | A text `correct_answer`; `choices` is not needed | Type a response |
| `select_keyword` | `choices` or `word_bank` with at least two strings | Select one keyword |

## Question fields

```json
{
  "id": "CASP-001",
  "question_type": "multiple_choice",
  "category": "Code framework and governing standards",
  "subcategory": "Authority and code-edition selection",
  "topic": "CBC / ADA applicability",
  "difficulty": 2,
  "source_authority": "CBC 11B / ADA (as applicable)",
  "question_text": "Which standard governs this project?",
  "choices": [
    "Determine the applicable standard and adopted code edition.",
    "Assume one standard applies to every facility."
  ],
  "correct_answer": "Determine the applicable standard and adopted code edition.",
  "accepted_answers": [],
  "explanation": "Project scope and authority determine the applicable requirements.",
  "traps": [],
  "knowledge_node_ids": ["topic-1", "topic-1-authority-and-code-edition-selection"]
}
```

`id`, `question_text`, and `correct_answer` are required. `category` and `choices` should be provided for selection questions. `choices` may also be named `options`; for keyword questions, `word_bank` or `keywords` can supply the choices. For fill-in-the-blank, `accepted_answers` may list additional valid text responses; matching ignores case and extra surrounding or repeated whitespace.

`question_type` is recommended. Supported values (including common spellings) are `multiple_choice`, `true_false`, `fill_in_blank`, and `select_keyword`. If omitted, the quiz makes a rough inference: two True/False choices are treated as true/false; `word_bank`/`keywords` implies keyword selection; prompts with a blank marker or questions with no choices are treated as fill-in-the-blank; other choice questions default to multiple choice. Explicitly label types when inference may be ambiguous.

## Knowledge-map links

- Use stable node IDs from the relevant knowledge database in `knowledge_node_ids`.
- Include the leaf topic and its ancestor IDs where applicable.
- When question IDs, source files, or topic links change, update the corresponding knowledge-map `test_bank_links` as well.
- A topic with no linked questions is a visible coverage gap; do not add a question only to fill the map.

## Validation

Before accepting a batch, confirm it parses as JSON, has a non-empty question array, and each question has a unique non-empty `id`, non-empty prompt and answer, and the required choices for its type. Verify that answer keys belong to the choices for selection questions and that all knowledge-node IDs resolve in the knowledge database.
