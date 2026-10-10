# Plan V2: Test Bank and CASp Knowledge Map

## 1. Purpose and direction

The Test Bank project supports different learning experiences, including quizzes for fun, topic learning, exam preparation, and applied knowledge. Each experience may use a different subject-specific bank and format.

The current CASp effort is focused on preparation for the California Certified Access Specialist (CASp) exam and on maintaining a structured map of the knowledge that should be covered.

The knowledge database is **not** a second copy of the question bank. It is a subject-matter coverage map used to organize, refine, and identify gaps in the knowledge base. The CASp batch JSON files remain the sole source of truth for question text, answer choices, answer keys, explanations, and other question content.

## 2. Product decisions captured so far

- Start by identifying the purpose of a quiz or study session: learn a topic, prepare for an exam, consolidate applied knowledge, or play trivia.
- Organize subject mastery before generating or curating questions.
- Use a recursively nested, editable mind map for major topics, subtopics, and specializations.
- Allow topic nodes to be added, removed, nested, expanded/collapsed, and flagged as important.
- Keep the knowledge map useful even when it has no associated questions; an empty node is a coverage gap to consider, not a reason to create a weak question.
- Do not embed or copy question objects into the knowledge database.
- Link topic nodes to existing test-bank JSON files and question IDs. Store the matching stable knowledge-node ID in each linked question object.
- Keep question editing, revisions, and culling in the original test-bank batch JSON files, not in the knowledge-map HTML.
- Expect questions to be culled. The topic hierarchy should remain stable and continue to represent the subject even as bank coverage changes.
- Export knowledge-map edits as a new versioned `CASp Knowledge Database v#.json` file.
- Verify every legal or technical claim against the applicable primary source, adopted code edition, jurisdiction, and project facts.

## 3. CASp mastery framework

Map knowledge around the capabilities a learner should demonstrate.

### A. Recall

- Identify essential facts, definitions, terms, and code references.
- Note frequent confusions and plausible misconceptions.
- Use recall questions when a fact must be retained, but avoid treating memorization as full mastery.

### B. Synthesis

- Organize major concepts and explain how they connect.
- Compare standards, facility types, and related requirements.
- Identify mainstream applications and conceptual frameworks.

### C. Applied knowledge

- Apply rules to realistic projects and field conditions.
- Work through facility use, occupancy, ownership, funding, scope, and existing conditions.
- Interpret routes, parking, doors, plumbing, and other elements in their context.
- Confirm the required result or deliverable, not merely a memorized rule.

### D. Edge cases and uncertainty

- Identify uncommon or emerging conditions and exceptions.
- Record what is uncertain, which assumptions are being challenged, and what primary source must be checked.
- Use edge cases to refine the conceptual framework, not to overstate uncertain conclusions.

### E. Subject-specific mastery

Record CASp-specific knowledge that may need multi-step analysis, inspection judgment, reporting, or other modalities that cannot be evaluated by a simple multiple-choice question alone.

## 4. CASp topic map

Maintain a recursive tree that can be expanded as study reveals additional topics and specializations. Initial top-level branches include:

1. Code framework and governing standards
2. Accessible parking and EV charging
3. Accessible routes and site circulation
4. Scoping and alteration triggers
5. Building elements and door/threshold requirements
6. Plumbing and toilet-room requirements
7. Housing and federal law comparison
8. CASp responsibility and California law

The map should represent the scope of the subject, not the volume of existing test questions. Mark priority topics with an importance flag. Organize children under the concept they explain; do not flatten every detail into a single long category list.

For each node, maintain only knowledge-map information and question links, such as:

- Stable node ID
- Name and optional description
- Parent/child relationship
- Importance flag
- Optional mastery dimensions, misconceptions, and source references as the knowledge base develops
- Linked test-bank source path and question IDs

Do not store question wording, choices, keyed answers, explanations, or copied question metadata in this database.

## 5. Test-bank source of truth and two-way links

### Question batch JSON

The files under `CASp/CASp Test/` own all question content. Preserve the existing question structure, including ID, category, subcategory, prompt, choices, answer, explanation, difficulty, authority, traps, and any provenance fields.

Add `knowledge_node_ids` to each question as an array of stable IDs from the knowledge database's `taxonomy`. Include the leaf topic and its ancestor nodes. A question may reference more than one branch when it genuinely assesses multiple topics.

### Knowledge database

Knowledge-map leaf nodes keep `test_bank_links`: relative paths to source batch JSON files plus the question IDs associated directly with that topic. Parent topic coverage is derived from descendants instead of repeating all of their question IDs. Links are references only; the database does not load or duplicate the complete question records as stored data.

This creates a simple bidirectional relationship:

- Question record → `knowledge_node_ids`
- Knowledge-map node → `test_bank_links` (`source_file` + `question_ids`)

Stable node IDs are the join key. Names can be clarified without breaking links. Each linked question carries the IDs for its topic path so either side can find the other without copying question content.

### Curation and reconciliation

- When adding, moving, or deleting a knowledge node, update the affected question `knowledge_node_ids` and node-to-question links as appropriate.
- When a question is edited, moved, duplicated into an alternate batch, or culled, reconcile its mapping and file/ID link.
- Repeated question IDs across different files are valid; a link must include both the file path and the ID.
- A node with no linked questions is retained as a visible subject-coverage opportunity.
- Do not assume that all topic nodes should have equal numbers of questions.
- Prefer a small, well-curated set of useful questions over keeping weak or nonsensical items merely to maintain counts.

See [schema for test bank json.md](./schema%20for%20test%20bank%20json.md) for the batch schema and linkage rules.

## 6. Question design and test structure

Questions must have an explicit purpose: confirm recall, synthesis, application, edge-case judgment, or broader mastery.

For the CASp exam-prep bank:

- Use multiple-choice questions for the main practice bank.
- Aim for roughly 50% scenario questions, normally with enough project context to require analysis, and 50% concise application questions.
- Build scenario facts deliberately: facility/occupancy, public or private use, owner, funding, new work or alteration, building/site conditions, applicable edition, and other controlling context.
- Train the learner to identify the governing authority and project scope before selecting technical requirements.
- Use plausible distractors based on common errors: wrong standard, facility type, code edition, trigger, or technical interpretation.
- Vary answer patterns; do not repeat the same correct-option pattern mechanically.
- Provide direct explanations that state the authority, triggering fact, technical conclusion, and relevant misconception.
- Set difficulty from 1 (basic recall) through 5 (multi-step, exam-style analysis).

### Broader Test Bank trivia

When a project is specifically for trivia or entertainment, keep trivia at about 5% by default unless the user requests a different balance. Use humor and lively answer options, including an occasional obvious “gimme.” Do not insert humor into serious CASp technical questions in a way that obscures the correct interpretation.

## 7. Knowledge-map viewer

The HTML mind-map viewer should:

- Load the knowledge-map JSON—not read or import the full bank question files.
- Show the recursive taxonomy graphically, with controls to expand and collapse child branches.
- Support adding/removing nodes, adding nested children, and flagging nodes as important.
- Display the selected node's linked source-file paths and question IDs as links to the original batch JSON.
- Make it clear that question content remains in the batch files and is not editable in the map.
- Preserve and export the topic hierarchy, importance flags, and reference links in a new versioned JSON file.
- Keep new or unlinked nodes visible for coverage planning.

The map is a planning and knowledge-refinement tool; it is not a quiz runner or question editor.

## 8. Source quality and verification

Separate verified authority from notes, recollections, and generated practice content.

- Treat selected-chat exports and study notes as secondary material.
- Verify code and legal claims against the controlling primary source and adopted edition.
- Record jurisdiction, facility context, project scope, funding triggers, and effective edition when they control an answer.
- Retain a clear distinction between authority/scoping analysis and technical application.
- Flag unresolved, conflicting, or unverified claims instead of presenting them as settled facts.

## 9. Iterative workflow

1. Define the desired learning or quiz outcome.
2. Review source materials and identify confidence/verification needs.
3. Build or refine the CASp knowledge hierarchy before drafting more questions.
4. Identify topic gaps, priority nodes, common misconceptions, and edge cases.
5. Draft or curate questions in the appropriate test-bank JSON batch only.
6. Link each question to one or more stable knowledge-node IDs.
7. Maintain reciprocal source-file/question-ID links on the knowledge-map nodes.
8. Validate JSON syntax, IDs, node references, file references, and duplicate IDs within each file.
9. Review question quality and remove or revise weak items without shrinking the knowledge map.
10. Export a new map version and periodically reconcile its links after test-bank curation.

## 10. Success criteria

- The topic map can express the subject at multiple levels of nesting.
- The knowledge database contains the knowledge structure and links, not embedded question records.
- Every linked question can be traced to its actual batch file and ID; every mapped question can identify its knowledge node.
- New topics can be recorded before questions exist.
- Culling questions does not erase the subject-matter coverage model.
- CASp practice emphasizes correct authority and scope selection before technical conclusions.
- Questions are clear, purposeful, source-grounded, and appropriately varied in difficulty.
- Knowledge-map versions can be edited and exported without modifying question content.

## Final direction

Build and refine the knowledge map as the durable model of what should be mastered. Keep actual questions in their existing batch JSON files and connect the two with stable IDs and source paths. The map should guide question coverage—not be another copy of the test bank.
