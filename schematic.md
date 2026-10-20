# Test Bank Project Schematic

## Executive summary

This folder contains a modular learning and assessment system that separates question content from the subject-matter knowledge map. The main product is a browser-based quiz app in [index.html](./index.html), while the actual question content is stored in JSON batches under [Archive](./Archive) and any subject-specific banks under their own folders, and the subject structure lives in a knowledge database file such as [CASp/CASp Knowledge Database.json](./CASp/CASp%20Knowledge%20Database.json).

The overall design is:

- Keep quiz question records in batch JSON files as the source of truth.
- Keep knowledge coverage in a separate taxonomy map with stable node IDs.
- Link questions to topics using `knowledge_node_ids`.
- Maintain reciprocal links in the knowledge database using `test_bank_links`.
- Use the front-end quiz app to deliver study sessions and report mastery gaps.

This is not a single monolithic app; it is a structured content system for learning, question generation, and topic coverage review that can support additional subjects beyond a single domain.

## Folder structure and file roles

### Root folder

- [index.html](./index.html)
  - Browser-based quiz platform.
  - Lets the user choose a question-bank file, start a timed session, answer questions, flag items, and review results.
  - Tracks question selection, timers, wrong answers, flagged items, and mastery by category/subcategory.

- [schema for test bank json.md](./schema%20for%20test%20bank%20json.md)
  - Defines the JSON structure for a question batch.
  - Establishes the rules for question IDs, topic fields, difficulty, authority, explanations, traps, and `knowledge_node_ids` linkage.

- [schematic.md](./schematic.md)
  - This summary and architecture note.

### Subject-specific content folders

- [CASp](./CASp)
  - Example subject implementation with planning and question-bank content.
  - Includes study or learning docs, a knowledge database, and batch question files.

- [Reader Level Test](./Reader%20Level%20Test)
  - Original, child-friendly reading-comprehension practice, beginning with one 15-question school Level L bank.
  - Uses the shared quiz app and stores passage and question content in a bank JSON file.

- [Chinese](./Chinese)
  - Beginner Mandarin practice covering pronunciation and tones, characters and writing, and vocabulary and phrases.
  - Keeps its quiz bank separate from the Chinese learning knowledge map.

- [Trivia](./Trivia)
  - Family-oriented practice covering culture, children’s safety, and general awareness.
  - Keeps its quiz bank separate from the trivia knowledge map.

- [Archive](./Archive)
  - Historical snapshots, prior versions, raw exports, and earlier working materials.

The Chinese and Trivia starter banks are registered in the shared quiz app and load alongside the other included banks. Additional subject areas can be added in parallel as separate folders or banks without changing the core app architecture.

## Core design principle: question bank vs. knowledge map

The strongest architectural idea in this project is the separation between:

1. The question bank
   - Actual questions and answer keys live here.
   - Source of truth for text, choices, explanation, difficulty, and answer metadata.

2. The knowledge map
   - Topic structure and subject coverage live here.
   - Stores stable topics and their relationships.
   - Tracks `test_bank_links` to specific files and question IDs.

This is explicitly stated in the schema and plan documents: the knowledge database should not duplicate question records. It should only organize, refine, and identify coverage gaps.

## Question schema and data model

The schema document defines a standard question object with fields such as:

- `id`
- `category`
- `subcategory`
- `topic`
- `difficulty`
- `source_authority`
- `question_text`
- `choices`
- `correct_answer`
- `explanation`
- `traps`
- `knowledge_node_ids`

The key relationship is:

- `knowledge_node_ids` in each question points to stable IDs in the taxonomy.
- Each knowledge node stores `test_bank_links` to `source_file` + `question_ids`.

This creates a durable two-way connection without copying question content into the map.

## Knowledge database findings

The current example knowledge database is already substantial. It declares:

- `schema_version`: `2.1.0`
- `coverage.test_bank_source_files`: `5`
- `coverage.question_records_at_link_creation`: `408`
- `coverage.topic_count`: `8`
- `coverage.taxonomy_node_count`: `86`
- `coverage.maximum_topic_depth`: `2`
- `coverage.linked_question_count`: `408`

The top-level taxonomy branches in the example structure include:

- code framework and governing standards
- accessible parking and EV charging
- accessible routes and site circulation
- scoping and alteration triggers
- building elements and door/threshold requirements
- plumbing and toilet-room requirements
- housing and federal law comparison
- subject responsibility and legal/regulatory context

The database also includes a list of authorities to verify, reflecting a common requirement across specialized domains: the project treats source verification as essential, not optional.

## Quiz platform behavior

The front-end app is designed around learning flow rather than only exam delivery.

Based on the HTML, it supports:

- bank selection from a directory of question JSON files
- timed quiz session per question
- answer choice selection
- question flagging
- review of explanations after answering
- results screen summarizing wrong answers, flagged questions, and slow responses
- category/subcategory-level mastery and focus areas

This matches the project’s goal of “teaching materials” and “tracking missed items for future development of quiz question batches.”

## Content strategy

The documents describe a reusable study workflow that can support many subject areas.

For reading-comprehension banks, keep an original shared passage at the bank level and link every question to that text. Use answer explanations to model where to look in the passage and how to reason, not just reveal the answer.

### Main learning goals

- Identify the governing standard or framework before technical analysis.
- Distinguish scope from technical requirements.
- Evaluate facility type, context, rules, and jurisdiction.
- Review key technical areas appropriate to the subject domain.

### Question design rules

- Mostly multiple-choice.
- Roughly 50% scenario-based, 50% concise application questions.
- Difficulty scale from 1 to 5.
- Distractors built around common errors: wrong framework, wrong context, wrong scope, wrong technical interpretation.
- Explanations should explain the governing rule, triggering fact, technical result, and misunderstanding.

### Evidence and verification rules

The material documents are careful to separate:

- captured topics
- recollections needing verification
- generated claims needing verification
- conflicting information
- open questions
- process guidance instead of legal or technical requirements

This is a deliberate QA practice: the bank is meant to be evidence-driven and source-grounded, not just a collection of remembered facts.

## Project workflow

The iterative workflow described in the plans is:

1. Define the learning or quiz goal.
2. Review source material and identify verification needs.
3. Build or refine the knowledge hierarchy.
4. Identify gaps, priority topics, misconceptions, and edge cases.
5. Draft or curate questions in the appropriate JSON batch only.
6. Link each question to one or more stable knowledge-node IDs.
7. Update reciprocal links in the knowledge map.
8. Validate JSON structure, IDs, and file references.
9. Review question quality and cull weak items.
10. Export a new version of the map as needed.

## Practical interpretation

The project is best understood as a structured learning platform with these moving parts:

- a quiz app for test delivery
- a content schema for question batches
- a taxonomy map for knowledge organization
- a validation discipline for factual correctness
- an archive system preserving older working versions

The system is intentionally designed to support a single specialized domain first, but the pattern is general enough to accommodate additional subjects with the same source-of-truth model.

## Bottom line

The Test Bank folder is not a single static file set; it is a small content architecture for expert learning and question generation. The central design decision is disciplined separation:

- question batch files hold the actual test items
- the knowledge database holds the coverage model
- the app presents the material to learners
- verification and source discipline protect educational quality

That design is consistent across the root app, the schema specification, the knowledge map, and the training/planning documents.