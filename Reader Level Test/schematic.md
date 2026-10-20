# Reader Level Test Schematic

## Purpose

Create encouraging reading-comprehension practice that helps a child enjoy reading and build confidence. The learner reports doing Fountas & Pinnell Guided Reading Level L at school. The project has seven original banks with 15 questions apiece: four earlier long-form sets, plus three newer short-read sets.

## Scope and level note

Fountas & Pinnell Level L is a point on the F&P Text Level Gradient, a continuum for selecting texts and observing reading behaviors—not a stand-alone age or grade label. In the publisher's Benchmark Assessment System, Level L appears in System 1 (Grades K–2, A–N) and System 2 (Grades 3–8, L–Z), so the systems intentionally overlap at L. One school-district chart places L across Grades 2 and 3. Treat grade correlations as approximate; use the learner's teacher and school assessment to interpret an individual placement.

The F&P Benchmark Assessment System (BAS) is not a 15-question multiple-choice quiz: students read a leveled book orally while the teacher records reading behaviors, followed by a text-based comprehension conversation. The new short-read banks are at-home comprehension practice inspired by broad Level L goals. They do not measure oral-reading accuracy, fluency, decoding, self-correction, or full retelling, and are not BAS materials or placement assessments.

All passages and questions are original. Do not reproduce passages, benchmark prompts, test questions, or answer keys from published books or assessment resources.

## Current bank formats

- Banks 01–04 each contain one original longer passage and 15 multiple-choice questions. These remain available as the original long-read sets.
- Banks 05–07 each contain three brief original passages, with five questions about each passage (15 questions per bank).
- The short-read sets rotate among fiction and informational text and ask about details, sequence, cause/effect, vocabulary, inference, and main idea.
- Each question includes a brief, supportive explanation.
- Bank content lives in JSON and runs in the shared Test Bank app. Short passages are defined once per bank and linked to questions by `passage_id`.
- The setup screen starts with the latest three short-read banks; “Show all test banks” reveals topic groups, and each “+” expands that topic's banks.
- All seven banks are practice drafts, not validated placements.

## Project files

- `reader_level_l_01.json` — The Map in the Library Book.
- `reader_level_l_02.json` — The Bridge of Paper Cranes.
- `reader_level_l_03.json` — A Night for the Owls.
- `reader_level_l_04.json` — The Sound of the Rain.
- `reader_level_l_05.json` — three short reads: The Red Scarf, Busy Bees, and The Chalk Arrow.
- `reader_level_l_06.json` — three short reads: The Kite String, From Tadpole to Frog, and The Warm Garden Stone.
- `reader_level_l_07.json` — three short reads: The Empty Bird Feeder, A Seed's Journey, and The Extra Seat.
- `schematic.md` — scope, content rules, and follow-up roadmap.
- `../index.html` — shared browser quiz experience; loads the bank from its included list when served locally, or by direct JSON selection when opened from disk.

## Content and quality rules

- For quick practice with young readers, use several brief passages per bank rather than one long passage followed by a large question set.
- Keep language accessible while including modestly varied sentence patterns, dialogue, less-familiar words supported by context, connected events, and opportunities to infer.
- Balance questions within the text (details and sequence), beyond the text (inference and connection), and about the text (main idea or purpose).
- Use plain question wording and plausible distractors; avoid trick questions.
- Explain why the answer fits the text and encourage looking back for evidence.
- Check that each question points to the intended short passage, every answer is supported by that text, answer keys are present among choices, IDs are unique, and each bank contains exactly 15 questions.
- The app's multiple-choice score is for practice only. It is not an F&P level, running record, fluency score, or placement decision.

## Research note

- [Fountas & Pinnell Leveled Books](https://www.fandpleveledbooks.com/aboutleveledtexts.aspx): describes the F&P Text Level Gradient as levels A to Z+, its use in matching books to readers, and guided reading's role in building strategic reading actions.
- [F&P Benchmark Assessment System components](https://www.fountasandpinnell.com/BAS/bascomponents): System 1 is Grades K–2, Levels A–N; System 2 is Grades 3–8, Levels L–Z.
- [Pearson Canada BAS 3rd Edition](https://www.pearsoncanadaschool.com/literacy/products/fpl/bas3.html): describes the post-reading Comprehension Conversation as a back-and-forth discussion, with the student doing most of the talking.
- [Morrisville Borough School District F&P chart](https://www.mv.org/classroom-webpages/2nd-grade/the-2nd-grade-team/fountas-and-pinnell-reading-level-chart): an example local chart places L across Grades 2 and 3; district charts can vary.
- These public summaries are general context, not a substitute for the full Literacy Continuum or the learner's teacher and benchmark assessment.

## Roadmap

1. Try the short-read banks with the learner and note passage length, reading comfort, enjoyment, and confusing items.
2. Encourage oral retelling and evidence-based talk; do not treat quiz selections as a substitute for reading aloud.
3. Revise weak items and check text challenge with the teacher or an approved school sample.
4. Decide whether to revise or add further banks based on learner and teacher feedback.
