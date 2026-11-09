# Bangla Social Media Text Normalization and Spelling Correction Dataset (500 samples)

## Dataset description

This release is a **500-row synthetic benchmark** for Bangla text normalization and spelling correction. It contains paired noisy/corrected Bengali-script messages and the requested metadata columns. **It contains 0 authentic social-media records and 500 synthetic records.** Synthetic items must not be described as collected YouTube, Facebook, or other user comments.

The text is designed to resemble short Bangladeshi Bangla online comments and chat-style posts. It covers narrowly scoped orthographic and surface-form cleanup: spelling, expressive repeated characters, punctuation, whitespace, Unicode normalization, and combinations of these. The dataset does not attempt grammar correction, paraphrasing, summarization, or Romanized Bangla transliteration.

## Intended use and important caveat

Use this file for pipeline prototyping, annotation-guideline development, sanity checks, and controlled experiments on these deliberately generated error types. It is **not a substitute for a held-out human-written social-media test set**. Synthetic generation patterns may be easier for a model to learn than naturally occurring user errors; model scores on this file should not be presented as real-world performance.

## Data sources and source review

No existing records were copied into this release. Public resources were checked for suitability:

1. **BaTEClaCor** — Oshin et al. (2023), *BaTEClaCor: A Novel Dataset for Bangla Text Error Classification and Correction*, Proceedings of the First Workshop on Bangla Language Processing, pp. 124–135. DOI: <https://doi.org/10.18653/v1/2023.banglalp-1.14>. The paper describes 10,000 authentic Bangla comments collected from YouTube and a correction task; the project repository marks the dataset CC BY-NC 4.0. Repository: <https://github.com/SyedT1/BaTEClaCor>. This is highly relevant in domain, but the archive could not be downloaded/inspected in the present file-generation environment. Therefore, **zero BaTEClaCor records are included**, and no content has been attributed to it.
2. **BanglaSEC** — Bijoy, Hossain, Islam & Shatabda (2025), *A transformer-based spelling error correction framework for Bangla and resource scarce Indic languages*, *Computer Speech & Language*, 89, 101703. DOI: <https://doi.org/10.1016/j.csl.2024.101703>. Dataset card: <https://huggingface.co/datasets/mehedihasanbijoy/BanglaSEC>. The public card describes 1,186,079 correct/error **word pairs**, with fields including `Word`, `Error`, and `ErrorType`, and lists MIT as its dataset license. This is useful for word-level spelling work but does not provide the sentence-level conversational examples required here. Its card also describes its error forms as synthetically generated, not harvested from real user writing. **No BanglaSEC rows are included.**
3. **Kingfisher / BanglaTextNorm** — Raju, Ahmad & Rahman (2026), *Kingfisher: A hybrid LLM-augmented Bangla text normalization for enhanced text-to-speech*, *Natural Language Processing Journal*, 14, 100201. DOI: <https://doi.org/10.1016/j.nlp.2026.100201>. Repository: <https://github.com/Rajan-sust/BanglaTextNorm>. The repository describes 2,766 annotated tokens across semiotic classes such as symbols, cardinal/ordinal numbers, dates, times, money, abbreviations, and electronic addresses, and marks the project CC BY-NC 4.0. That is principally TTS-oriented verbalization/normalization, not noisy social-media spelling pairs. **No Kingfisher rows are included.**
4. **BanglaGEC** was also identified as a large grammatical error-correction resource, but grammatical correction is outside this project’s stated scope and it was not used: <https://huggingface.co/datasets/nahid-hub/BanglaGEC>.

### Why there are no authentic samples

BaTEClaCor is the closest match, but its repository archive was not retrievable for record-level inspection here. It would be poor research practice to take one table example from the paper, infer unseen records, or label generated items as authentic. This release therefore chooses transparent synthetic coverage over unverifiable authenticity. If you later obtain BaTEClaCor through its official repository and verify its terms and fields, you can curate a separate authentic subset with attribution and document any re-annotation.

## License and permissions

- **This generated dataset:** The text pairs in this release were generated specifically for this task. No third-party dataset records are redistributed. No separate open-source license is asserted in this release; obtain the project owner’s permission or add a license before public redistribution.
- **BaTEClaCor:** The repository/paper state CC BY-NC 4.0. Non-commercial conditions apply to reuse of the licensed material; attribution is required. Check the original license and repository before reuse, and do not assume this license applies to third-party material underlying social-media comments.
- **BanglaSEC:** The Hugging Face dataset card metadata lists MIT. Review the current card/repository and associated paper before redistributing any downloaded copy.
- **Kingfisher:** The repository states CC BY-NC 4.0. Its terms are not applied to this generated file because none of its records are included.

This README is a research-use summary, not legal advice. The license of a source dataset does not automatically grant rights over platform content, personal data, or materials governed by third-party terms.

## File schema

`bangla_social_media_500.csv` uses exactly these columns:

| Column | Meaning |
|---|---|
| `ID` | Sequential identifier (`001`–`500`) |
| `Original_Text` | Noisy synthetic input, preserving intentional spacing and invisible Unicode characters |
| `Corrected_Text` | Proposed normalized reference text |
| `Error_Type` | One of `Spelling`, `Repeated_Character`, `Punctuation`, `Whitespace`, `Unicode`, `Mixed` |
| `Source` | `Synthetic` for every row |
| `Data_Type` | `Synthetic` for every row |
| `Split` | `Train`, `Validation`, or `Test` |
| `Notes` | Short description of the targeted change |

The CSV is encoded as **UTF-8 with BOM (`utf-8-sig`)** for better Excel compatibility. Original text fields may contain repeated spaces, leading/trailing spaces, U+200B ZERO WIDTH SPACE, or canonically decomposed Unicode sequences; do not call `.strip()` or normalize the input field before measuring those error types.

## Error category distribution

| Error category | Count |
|---|---:|
| Spelling | 150 |
| Repeated_Character | 100 |
| Punctuation | 50 |
| Whitespace | 50 |
| Unicode | 75 |
| Mixed | 75 |
| **Total** | **500** |

The requested combined punctuation/whitespace target is 100 rows, split evenly into 50 `Punctuation` and 50 `Whitespace` rows so the `Error_Type` field identifies the specific operation.

## Data type distribution

| Data type | Count |
|---|---:|
| Authentic | 0 |
| Synthetic | 500 |

`Source=Synthetic` and `Data_Type=Synthetic` are set for every row. No authentic posts are claimed.

## Train / validation / test split

| Split | Count | Share |
|---|---:|---:|
| Train | 400 | 80% |
| Validation | 50 | 10% |
| Test | 50 | 10% |

The split assignment was stratified by error category. Related spelling variants generated from the same target-word family, and repeated-character variants generated from the same target-word family, were kept within a single split; no such family crosses splits. Punctuation, whitespace, Unicode, and mixed examples use separate base sentences. The spelling subset now uses individually written context-specific sentences, while repeated-character examples still use some recurring short conversational patterns. Exact duplicate checks passed. A normalized character-similarity heuristic (difflib SequenceMatcher ratio ≥ 0.82 on corrected sentences) also found zero cross-split pairs. This heuristic cannot detect every semantic near-duplicate, so a human review is still recommended before treating the split as a rigorous generalization benchmark.

## Annotation guidelines

1. Correct only the target surface-form error; preserve sentence meaning and voice.
2. Keep Bangla conversational wording and colloquial phrasing when valid; do not standardize dialect merely because it differs from formal prose.
3. Do not rewrite grammar or paraphrase content.
4. Collapse over-extended character sequences to a conventional form only where the intended word is clear (for example, emphasis through repeated letters).
5. Normalize excessive terminal punctuation to a single context-appropriate mark.
6. Normalize whitespace only where the intended word boundaries are unambiguous. Nineteen whitespace rows are flagged for manual review in `Notes`: 10 contain an extra space before terminal punctuation, and 9 contain a merged word boundary that automatic whitespace collapsing cannot reconstruct.
7. Unicode examples use canonical NFD-to-NFC normalization or remove U+200B at a word boundary. This is a code-point normalization task; the visible text may look unchanged in many fonts/editors.
8. Mixed records contain at least two separate noise mechanisms in the same input.
9. Preserve names and proper nouns in the synthetic sentences; no personally identifying user comments are included.

## Quality-control results

The build-time checks confirmed:

- 500 data rows and exactly 8 CSV columns.
- Unique sequential IDs, 500 unique `Original_Text` values, and 500 unique `Corrected_Text` values.
- No empty required fields.
- 500/500 rows marked `Synthetic` in both provenance fields.
- Exact error category counts: 150 / 100 / 50 / 50 / 75 / 75.
- Exact split counts: 400 / 50 / 50.
- No generated source-family group crosses train/validation/test.
- No cross-split corrected-text pair reached a normalized character-similarity ratio of 0.82 or higher (heuristic check).
- Nineteen whitespace rows are marked for manual review (10 extra spaces before terminal punctuation; 9 merged word boundaries).
- Unicode rows normalize to their corrected text using NFC.
- Punctuation rows contain repeated/malformed terminal punctuation.
- Original and corrected text differ in every row.

These are programmatic checks, not a completed human linguistic validation. Automatic checks cannot determine whether every spelling variant is the most plausible typo in actual user-generated Bangla, nor prove that every context has only one possible interpretation.

## Known limitations and manual review tasks

- **All 500 rows are synthetic.** Authentic YouTube/Facebook comments are absent; the dataset does not measure naturally occurring error frequencies.
- The exact category balance is designed, not prevalence-based.
- Repeated-letter examples model exaggerated typing, but the boundary between expressive style and error can be context-dependent.
- Punctuation normalization is partly style-sensitive; preserve author intent in future human annotation.
- Canonical Unicode variants may display identically. Evaluation must compare code points or normalized forms deliberately.
- Nineteen whitespace rows are flagged for manual review: 10 have a space before terminal punctuation, and 9 have a merged word boundary that requires lexical review.
- All 500 corrected references should receive an independent Bangla-speaker review before publication or use as a gold test set. Recommended review: two annotators independently verify meaning preservation and category; resolve disagreements with an adjudicator; record inter-annotator agreement.
- Add an authentic, permission-cleared, source-attributed test set before making claims about real-world social-media performance.

## Citation suggestions

- Oshin, N. T., Hoque, S., Fahim, M., Ali, A. A., Amin, M. A., & Rahman, A. K. M. (2023). BaTEClaCor: A Novel Dataset for Bangla Text Error Classification and Correction. *Proceedings of the First Workshop on Bangla Language Processing*, 124–135. https://doi.org/10.18653/v1/2023.banglalp-1.14
- Bijoy, M. H., Hossain, N., Islam, S., & Shatabda, S. (2025). A transformer-based spelling error correction framework for Bangla and resource scarce Indic languages. *Computer Speech & Language, 89*, 101703. https://doi.org/10.1016/j.csl.2024.101703
- Raju, R. S., Ahmad, A., & Rahman, M. S. (2026). Kingfisher: A hybrid LLM-augmented Bangla text normalization for enhanced text-to-speech. *Natural Language Processing Journal, 14*, 100201. https://doi.org/10.1016/j.nlp.2026.100201
