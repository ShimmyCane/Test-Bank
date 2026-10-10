# Test Bank Question JSON Schema

## Purpose

Each test-bank batch is the source of truth for its own question content. Keep question text, choices, answer keys, explanations, difficulty, and subject tags in the batch JSON. The separate knowledge database defines the subject/topic coverage map and links to questions by source file and ID; it does not copy question records.

This keeps the test-bank workflow simple:

1. Read and write quiz content in the relevant batch JSON.
2. Associate a question with one or more stable knowledge-map nodes using `knowledge_node_ids`.
3. Use the knowledge database to refine topic coverage, hierarchy, mastery areas, and importance flags.
4. Review coverage and links when question batches change or questions are culled.

## Question object

Each JSON file contains an array of question objects. Preserve the established batch structure and fields:

| Field | Type | Purpose |
|---|---|---|
| `id` | string | Question ID, unique within a source file (for example, `CASP-001`). IDs may repeat in alternate batch files. |
| `category` | string | Broad test-bank category. |
| `subcategory` | string | More specific subject area, mapped to a knowledge-map node where applicable. |
| `topic` | string | Subject or scope description; retain for existing test-bank use. |
| `difficulty` | number | Relative difficulty, normally 1–5. |
| `source_authority` | string | Governing source/authority stated for the question; verify before relying on it. |
| `question_text` | string | Question prompt. |
| `choices` | array of strings | Multiple-choice options. |
| `correct_answer` | string | Correct option text. |
| `explanation` | string | Why the keyed answer is correct and which facts/authority control. |
| `traps` | array of strings | Common misconceptions or distractors the item tests. |
| `knowledge_node_ids` | array of strings | Stable IDs of the leaf topic and its ancestor nodes in the knowledge map. |

Optional provenance fields such as `source_chat_id` and `source_chat_title` may be included when a question is derived from a selected conversation. Do not replace source provenance with a knowledge-map link: they serve different purposes.

## Knowledge-map linkage

- Use the stable `id` from a node in `CASp/CASp Knowledge Database.json`'s recursive `taxonomy`.
- Store the leaf node ID and its ancestors in the question's `knowledge_node_ids` array. Add other branches only when a question genuinely covers multiple distinct topics.
- Knowledge-map leaf nodes have `test_bank_links`: entries containing a relative `source_file` path and the question IDs in that file linked directly to the topic. Parent-topic coverage is derived recursively from its descendants, so the same ID lists are not copied up the tree.
- This is a bidirectional reference, not duplicated question content. The bank file owns the question; the knowledge map owns the topic hierarchy.
- Use IDs, not topic names, as the durable link. Names can be refined without breaking references.
- When topics are reorganized, update question `knowledge_node_ids` and the corresponding knowledge-map links together. When questions are removed or IDs change, remove or correct the stale map links.
- A newly added knowledge node can have no linked questions yet. That is useful for identifying content gaps; do not fabricate a question just to fill the map.
- Question editing, culling, answer revisions, and difficulty changes belong in the relevant test-bank JSON—not in the knowledge-map HTML.

## Example

```json
[
  {
    "id": "CASP-001",
    "category": "Code framework and governing standards",
    "subcategory": "Authority and code-edition selection",
    "topic": "CBC / ADA applicability",
    "difficulty": 2,
    "source_authority": "CBC 11B / ADA (as applicable)",
    "question_text": "Identify the governing accessibility standard before applying technical requirements.",
    "choices": [
      "Determine the applicable standard and adopted code edition.",
      "Assume one standard applies to every facility."
    ],
    "correct_answer": "Determine the applicable standard and adopted code edition.",
    "explanation": "Authority and project scope control which technical provisions apply.",
    "traps": [
      "Assuming overlapping standards are interchangeable."
    ],
    "knowledge_node_ids": [
      "topic-1",
      "topic-1-authority-and-code-edition-selection"
    ]
  }
]
```

The example node ID must exist in the current knowledge database. Treat it as a linkage example; validate exact question claims against controlling sources before use.

## Validation expectations

Before accepting a batch update:

1. Confirm the file parses as JSON and its root value is an array.
2. Confirm each question has a non-empty `id`, prompt, choices, answer, and category.
3. Confirm every `knowledge_node_ids` value resolves to a node in the knowledge database.
4. Confirm knowledge-map source paths and IDs resolve to questions in the named batch JSON.
5. Check that there are no unintended duplicate IDs inside a single file.
6. Review topic coverage independently of question volume. A node with no linked questions is a visible coverage gap, not an error to conceal.
