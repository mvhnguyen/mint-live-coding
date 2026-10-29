# MiNT Sprint 2.0 — Bilingual Protocol Scorer

A browser-based, examiner-facing scoring tool for administering a bilingual English–Mandarin picture-naming protocol. It provides a fixed 80-item scoring grid for each language, examiner scripts, a two-pass workflow, cumulative timing, automatic language-order counterbalancing, and downloadable session data.

**This is an examiner scoring interface, not a participant-facing picture presentation task.** The grid contains written item labels and accepted naming alternatives rather than picture stimuli. The examiner must administer the actual picture-naming materials separately.

## Features

- English and Mandarin Chinese scoring blocks (80 items per language, presented in an 8 × 10 item sequence).
- Participant ID and examiner/RA name entered before testing.
- Automatic block ordering based on the numeric participant ID:
  - **Even ID:** English → Mandarin.
  - **Odd ID:** Mandarin → English.
- On-screen examiner scripts for first-pass naming and second-pass review in the relevant language.
- Interactive item marking for first-pass errors and second-pass corrections.
- Stopwatch that continues across both passes of a language block, with separate first- and second-pass durations recorded.
- Live block-level score display and a downloadable CSV containing both languages.
- Responsive interface designed for modern desktop and tablet browsers, including touch interaction.

## Running the tool

The application is contained in one HTML file. Save the supplied code as `index.html` in the root of your repository. Open the file in a browser or publish it with GitHub Pages.

For GitHub Pages:

1. Upload `index.html` to your GitHub repository.
2. Open **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**, choose your publishing branch (`main` or `master`) and `/ (root)`, then save.
4. Open the Pages URL displayed in the repository settings.

The page uses Tailwind CSS from a public CDN, so an internet connection is needed for the intended styling, even if the HTML file is opened locally.

## Administration workflow

1. **Set up the session.** Enter the participant ID and examiner name. The tool displays the assigned language order; select **Start Testing Session**.
2. **First pass.** Read the displayed instructions, start the stopwatch, and mark any incorrect or unanswered items by tapping their cards. All items initially appear as correct, so **every missed item must be marked**. Finish the first pass using the corresponding button.
3. **Second pass.** Review items marked incorrect in the first pass. Select **GOT** when an item is retrieved or **DK** when it remains incorrect/unanswered. The stopwatch continues until **Finish Block Review** is selected.
4. **Next language.** After the first block, select **Proceed to Next Block** and repeat the procedure in the automatically assigned second language.
5. **Export.** After both blocks are complete, select **Export Master CSV** and verify that the file was saved before closing or resetting the page.

The displayed protocol mentions a three-minute first-pass limit, but **the current code does not automatically stop the timer at three minutes or enforce that limit**. The examiner must implement any required time limit manually.

## Scoring and timing

For each language block, the interface calculates:

| Field | Meaning |
| --- | --- |
| First-pass correct | Number of items still marked correct at the end of the first pass |
| Second-pass got | Number of first-pass misses subsequently marked `GOT` |
| Total correct | First-pass correct plus second-pass got |
| First-pass time | Seconds from stopwatch start to **Finish 1st Pass** |
| Second-pass time | Additional seconds from **Finish 1st Pass** to **Finish Block Review** |
| Total block time | Sum of the two pass durations |
| Efficiency (items/sec) | **80 divided by first-pass duration in seconds**, as currently implemented |

**Important:** The efficiency field is currently based on all 80 presented items, **not the number of correctly named items**. It should not be interpreted as correct items per second without changing the calculation. The code records durations in whole seconds.

## Data export

Once both language blocks are completed, the app downloads a file named:

`MiNT_Master_<ParticipantID>.csv`

The export contains two sections:

1. **Block-level summary:** participant ID, examiner, counterbalancing rule, block order, language, pass durations, total time, scores, and efficiency.
2. **Item-level detail:** block order, language, item ID (1–80), row, column, Chinese and English item labels, first-pass correctness, and second-pass result (`GOT`, `DK`, or `N/A`). There are 160 item-level records across both languages.

**Data-format note:** The current export places summary rows, a text divider, and item-level rows with different column headers into the *same* `.csv` file. It can be opened as text or in a spreadsheet, but it is **not a single rectangular CSV table**. For automated analysis, split the two sections or adapt the export to produce separate summary and item-level files.

## Data handling and practical limitations

- The application runs in the browser; the current code does **not** upload results to GitHub or any server.
- Session responses are held in browser memory until exported. Reloading, closing, or resetting the page may result in data loss.
- Verify each download on the device, particularly when administering from an iPad.
- The examiner-facing interface displays scores during administration. It is not intended to be shown to participants.
- The participant ID is checked as numeric for counterbalancing, but the code does not strictly require a whole-number ID. Use nonnegative integer IDs for consistent even/odd assignment.
- The tool is a digital scoring aid; it does not independently establish the validity of an adapted test protocol or replace the original administration and scoring guidance.

## Files and customization

The current implementation is self-contained within `index.html`. The item labels are defined in the JavaScript `rawData` array, and the protocol scripts, scoring behavior, and CSV export are implemented in the same file. Changes to item labels or accepted responses should be reviewed against the study's approved stimulus and scoring materials.

## Attribution

MiNT Sprint 2.0 is a custom web-based protocol scoring interface. The supplied source file does not identify its author, repository, or the reference for the underlying naming test, so no authorship or test citation is asserted here. Add the appropriate project maintainer, licensing terms, and citation to the original Multilingual Naming Test (MiNT) materials before publishing if required by your study or reuse permissions.

## References

Gollan, T. H., Garcia, D. L., Stasenko, A., Murillo, M., Kim, C., Galasko, D., & Salmon, D. P. (2024). The MINT Sprint 2.0: A picture naming test for detection of naming impairments in Alzheimer's disease and in preclinical AD. *Alzheimer's & Dementia, 20*(1), 112–123. https://doi.org/10.1002/alz.13381

Sheng, L., Lu, Y., & Gollan, T. H. (2014). Assessing language dominance in Mandarin–English bilinguals: Convergence and divergence between subjective and objective measures. *Bilingualism: Language and Cognition, 17*(2), 364–383. https://doi.org/10.1017/S1366728913000424

### Test Materials

Original MINT Sprint 2.0 materials: https://osf.io/7r9mq/

**Note:** This repository contains a modified, web-based scoring interface for bilingual English–Mandarin administration of MINT Sprint 2.0. The original test and materials were developed by Gollan and colleagues. The Mandarin–English MINT research by Sheng et al. (2014) is cited as relevant background; it does not describe this specific web implementation.
