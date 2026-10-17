# APOL paper: audit report

Scope and evidence. Supplied: `main.tex`, `references.bib`, the compiled PDF and the audit prompt. **Not supplied:** notebooks, code, logs, per-question predictions, figure PNGs, `make_figures.py`, `emnlp2021.sty`. So every experimental number could be checked only for internal consistency and arithmetic. None could be verified against raw outputs. Figures were not regenerated, because the data and scripts are missing.

Compilation. I compiled the revised source with the current ACL style (`acl.sty`, `acl_natbib.bst`) as a stand-in, because `emnlp2021.sty` was not available. Placeholder images replaced the figures. Result: no LaTeX errors, no undefined citations or references, 33 bibliography entries rendered, one 1.2 pt overfull box. Layout under the real `emnlp2021.sty` and the real figures is **not verified**. Compile once on your side and check page by page.

## B. Issue and correction table

| # | Issue | Sev. | Evidence / location | Correction | Verified? | Remaining limitation |
|---|---|---|---|---|---|---|
| 1 | Evaluation questions are inside the classifier-development pool; KEEP labels use gold-answer containment; split is pair-level | Critical | §4.3, §8 | Quantified: 100 eval questions × 10 sentences = 1,000 of 3,000 pairs (33%). Added that gold answers entered label generation. All AHCC claims labelled in-pool | Arithmetic yes; pool membership taken from authors' own statements | Needs a clean held-out run (retrain without the 100 questions, evaluate on new HotpotQA validation questions) |
| 2 | Stage-2 set of 3,956 = 3,000 + 956, so it appears to contain val and test pairs; Stage-2 val F1 0.700 vs 0.575 is not comparable | High | §4.4, Table 3 | Stated this in §4.4, Table 3 caption and Limitations; Table 3 column renamed "Train pairs" (Stage 1 = 2,250) | Arithmetic yes; **check your code** to confirm | Whether val/test pairs were really in the Stage-2 training data is unconfirmed |
| 3 | "All tested compression methods beat the baseline" (abstract, §6.1, conclusion) contradicted by KEEP+DROP (0.165) and T5 (0.137) ablations, and by E2 | High | Abstract, §6.1, Table 4–5 | Reworded to "every method in the main comparisons"; added the exceptions to the abstract, §6.1, §6.3 and conclusion | Yes (tables) | – |
| 4 | E2 reported selectively: text said only AHCC is below baseline at 20–40% noise | High | §6.4, Table 5 | Hybrid is also below at 20% and 40%, HP at 60% (0.133 vs 0.138), AHCC at all levels. Only HP at 20% and 40% exceeds the baseline. Rewrote paragraph | Yes (table) | – |
| 5 | "Central finding / decoupling" overstated: only assembly varied (classifier fixed), gap 0.034 is about the marginal 95% half-width, no paired test | High | Abstract, §1, §6.3, §7.1 | Renamed "Assembly matters for a fixed classifier"; stated what the experiment does and does not show; significance caveat added | Yes | Provenance of 0.137 and 0.165 runs (same run and template as Comparison A?) unverifiable |
| 6 | F1/EM metric validity: EM 0.01–0.04 means 1–4 of 100 questions; baseline shifts 0.052 F1 between templates (larger than most method gaps); answer verbosity not analysed | High | Table 2 | Added to §5, §6.1 and Limitations | Arithmetic yes | Report answer length or a containment metric in future work |
| 7 | "Within sampling error" asserted for 0.217 vs 0.181, but only marginal CIs exist | Medium | §7.2 | Rewritten: difference (0.036) is smaller than marginal half-widths; significance undetermined | Yes | Paired bootstrap needs per-question predictions |
| 8 | τ=0.60 is both default and F1 peak of a sweep run on the evaluation questions | Medium | §6.6 | Added caveat: 0.181 not an unbiased estimate | Logic only | – |
| 9 | F1 vs τ is non-monotonic (0.167–0.181) but described as a "corresponding" trade-off | Medium | Abstract, §6.6, conclusion | Reworded to "varies non-monotonically" | Yes (Table 9) | – |
| 10 | "Compression makes quality insensitive to depth… retrieve more evidence without penalty" overreaches (only k=5 vs 10) | Medium | §6.5 | Narrowed to the tested range; noted top-5 uncompressed (0.209) beats AHCC (0.178, 0.181) | Yes | k>10 untested |
| 11 | Stage-2 "improves F1 at zero annotation cost" (0.010, within noise, in-pool) | Medium | §6.3 | Reworded; no improvement claim | Yes | – |
| 12 | Compression ratio called "slightly" different from prompt-length ratio; actual gap is up to ~6 points (Hybrid 0.603 vs 0.650) | Medium | §5, Table 2 | Wording corrected; gap stated as undecomposed | Arithmetic yes | Cause (template overhead vs per-sample averaging) not established |
| 13 | k=5→10 change "at most 0.005": largest rounded change is 0.004 | Low | Abstract, §1, §6.5, §9 | Changed to 0.004 or less (rounded values) in all places | Yes | – |
| 14 | "loses 19.7%": from rounded values it is 19.6% | Low | §6.5 | "about 20%" | Yes | – |
| 15 | Unfounded efficiency/latency remark on T5 ("neural inference overhead") | Low | §6.1 | Replaced by "extra neural pass, latency not measured" | – | – |
| 16 | "Phases 1–5" never defined; Phase 4 contains E1–E4 | Low | §5 | Added a Phases paragraph. **Phase 5 mapping is my inference** (efficiency analysis, §7.3) | Partly | Confirm with your notebook |
| 17 | Stage-1→2 val F1 +0.125 but pipeline F1 +0.010 not discussed | Low | §6.2 | One sentence added, flagged as within-pool | Yes | – |
| 18 | `\ahcc` small-caps macro vs plain "AHCC" in headings, captions and Appendix A | Low | §6.2 title, captions, Table 7 | All now `\ahcc{}` | Yes (grep) | – |
| 19 | Figure legends say "Hard prune (moderate/aggressive)" while text uses HP θ; "aggressive" sounds like the higher-compression setting but θ=0.3 prunes less. Labels overlap in the Pareto figure | Medium | Figs 1, 3, 4, 6 | Captions now map legend names to θ (mapping inferred from matching F1/token values in the figure text; confirm) | Partly | **Figures not regenerated** (no scripts or data). Please relabel and fix overlap |
| 20 | Token counts are 1.3 × words, not tokenizer counts | Low | §5 | Stated in §5 and table captions | Yes | – |
| 21 | Intro cited Liu et al. for "irrelevant/misleading" context (it is about position) | Medium | §1 | Split claims: Shi et al. (irrelevant context), Liu et al. (uneven use of long context) | Yes (knowledge) | – |
| 22 | Selective Context listed under sentence-level methods (it works on lexical units) | Low | §1 | Moved to token/lexical-unit group | Yes (own Table 1) | – |
| 23 | FiD described as improving "multi-hop QA"; it was evaluated on open-domain QA (NQ, TriviaQA, SQuAD-open) | Medium | §2 | Reworded | From knowledge, not re-checked online | – |
| 24 | RECOMP described as "direct QA supervision" | Low | §2 | "supervision derived from the downstream LM" (matches Table 1) | From knowledge | – |
| 25 | "Existing evaluations typically assess a single method", "to our knowledge none…" too strong | Low | §1, §2 | Softened | – | – |
| 26 | Mistral-7B-Instruct-v0.2 cited to a paper that describes v0.1 | Low | §5 | Now "a release in the Mistral 7B family" | Yes (arXiv page says v0.1) | – |
| 27 | NQ-Open cited only to the NQ paper | Low | §5 | Added Lee et al. 2019 | Yes (ACL Anthology) | – |
| 28 | "66.6M parameters" attributed to DistilBERT alone | Low | §4.2 | Now "full classifier ≈66.6M" (DistilBERT ≈66.36M from memory + my head count ≈0.20M) | Arithmetic, base count not re-verified | – |
| 29 | Added Limitations: metric validity, τ reuse, supporting-fact recall not measured, untested hypotheses | Medium | §8 | Added | – | – |
| 30 | Header comment claimed "every number is traceable to notebook output" | Low | top of `main.tex` | Replaced with an accurate statement | – | – |
| 31 | Table widths overflowed by 3–37 pt under the ACL style | Low | Tables 2, 4, 5, 7 | Tighter column spacing; hyperparameter table columns wrapped | Compile test | Recheck under `emnlp2021.sty` |

## C. Numerical consistency audit

Recomputed from the paper's own tables (Python):

| Quantity | Source | Check | Status |
|---|---|---|---|
| Token reductions 35.2–65.0% (A); AHCC S2 50.1%; HP0.5 vs top-5: 21.1% | Table 2 | recomputed | Agree with text |
| Δ F1 in Table 4 (+0.049, −0.003, −0.031, +0.003, +0.013) | Table 4 | recomputed | Correct |
| Comparison B range +0.049…+0.099; compression 30.6–70.3% | Table 2B | recomputed | Correct |
| Class counts 904+573+1,523 = 3,000; train 677+430+1,143 = 2,250; 2,250/450/300 = 75/15/10; 3,956 = 3,000+956 | §4.3–4.4 | recomputed | Consistent |
| NQ 200×5 = 1,000; HotpotQA 200×10 = 2,000 | §4.3, §5 | recomputed | Consistent |
| Macro-F1 from K/D/E-F1 = 0.6037 vs reported 0.603 | Table 3 | recomputed | Rounding only |
| CI half-widths 0.040/0.049/0.049/0.052/0.050/0.047 | Appendix B | 1.96σ/10 | Correct |
| E1 token growth +10%/+40%/+23% | §6.4 | recomputed | Correct |
| E3 deltas: +0.004, −0.002, −0.004, +0.003 | Table 6 | recomputed | Text fixed to ≤0.004 |
| E4: compression and efficiency monotonic; F1 ≥ 0.168 for τ ≥ 0.55 | Table 9 | checked | Correct |
| Efficiency vs mean-F1/mean-tokens (e.g. HP0.5 1.800 vs 1.419; Hybrid 1.877 vs 1.566; baseline 0.481 vs 0.474) | Table 2 | recomputed | Differences expected (mean of ratios), as the paper says |
| Compression (per sample) vs 1 − tokens/354.5 (e.g. Hybrid 0.603 vs 0.650) | Table 2 | recomputed | Differ by up to ~6 pts; not decomposed |

**Verified only against the paper (no raw output):** all F1, EM, token, efficiency and compression values; E1 token counts (184→203, 156→219, 129→159, 485→865); E2 compression rates and the "55–58% fewer tokens"; the per-class test F1; the 0.575, 0.603 and 0.700 scores; the 956 pseudo-labels; the 0.137 and 0.165 ablation runs.

**Possible copy error to check:** mean prompt size 123.9 appears both for Hybrid (Table 2) and for AHCC at τ=0.80 (§6.6). The compression values (0.603 vs 0.630) differ, so it may be a coincidence, but it needs checking against the notebook.

Abstract, §1, tables, figure captions, discussion and conclusion now agree on every number above.

## D. Methodology and validity assessment

What the experiments support: in this 100-question sample, heuristic and learned compression yield shorter prompts and, in the main comparisons, higher observed F1 than the uncompressed prompt. Hard pruning at θ=0.5 is the strongest and simplest method. AHCC is not better than the BM25 heuristics.

What they do not support: generalisation, robustness, statistical significance, superiority over published compressors, or any claim about k>10.

Remaining threats:
1. **Leakage.** The evaluation questions are in the classifier pool, with gold-answer-derived KEEP labels, a pair-level split, and (apparently) val/test pairs inside the Stage-2 training set. All AHCC numbers are in-pool. The heuristics have no trained part, so the AHCC-versus-heuristic gap is the most affected.
2. **No significance testing.** n=100, marginal intervals only, no paired bootstrap. The 0.034 assembly gap and the 0.036 HP-vs-AHCC gap are comparable to the interval widths.
3. **Metric.** EM is 1–4 questions out of 100. F1 depends on answer length, and the template change alone moves the baseline by 0.052. I could not check answer verbosity.
4. **Pool.** Only 10 sentences per question, so the k sweep only tests k=5 vs 10. Supporting-fact recall is unknown.
5. **τ and ablation provenance.** τ is tuned and reported on the same questions. It is unconfirmed whether the 0.137 and 0.165 runs used the same template and run as Comparison A, and whether the binary variant was retrained or derived by remapping.
6. **Untested explanations.** T5 truncation and BM25–HotpotQA alignment are labelled as hypotheses.

## E. Reference verification

Checked online (authoritative page found):
- CompAct: ACL Anthology 2024.emnlp-main.1194. **Added pages 21424–21439**; DOI was already correct.
- EXIT: Findings of ACL 2025, pages 4895–4924, authors correct.
- PISCO: Findings of ACL 2025, pages 15506–15521. **Fixed names to Hervé Déjean and Stéphane Clinchant.**
- AttnComp: Findings of EMNLP 2025, pages 8456–8472, authors correct.
- Provence: ICLR 2025, authors correct. **Fixed "Stéphane".**
- Mistral 7B (arXiv 2310.06825): **author "Jiawei Wang" was wrong; correct is Thomas Wang.**
- Lee, Chang, Toutanova 2019 (P19-1612, pages 6086–6096): **newly added** for NQ-Open.

Fixed BibTeX escaping: `K{"u}ttler`, `Rockt{"a}schel`, `R{"u}hle`, `Sch{"a}rli` rendered as `K"uttler` and so on in the PDF; they are now `{\"u}`/`{\"a}`. Protected "Natural Questions" and "Provence" in titles.

**Not independently checked online** (match my recollection of the venues and pages, but I did not look them up):
- Lewis 2020, Izacard 2021, Kwiatkowski 2019, Mallen 2023, the LLMLingua trio, Li 2023, Xu 2024, Wang 2023 (arXiv), Chevalier 2023.
- Asai 2024, Shi 2023, Liu 2024, Gao 2023, Robertson 2009, Sanh 2019, Reimers 2019, Raffel 2020.
- Yang 2018, Dettmers 2023 (no page range given), Rajpurkar 2016, Loshchilov 2019, Yarowsky 1995, Berg-Kirkpatrick 2012, Wolf 2020.

I did not check whether each cited work supports the exact sentence it is attached to, apart from the corrections in rows 21–24 above. All 32 in-text citation keys have an entry, and the bibliography now has 33 entries.

## F. Submission-readiness

- **Presentation:** good after these edits. The claims now match the evidence, and numbers, terms and references are consistent. It still needs a compile under the real `emnlp2021.sty` with your real figures, and the figure legends and overlapping labels fixed.
- **Experimental validity:** **material limitation remains.** The AHCC evaluation is in-pool, n=100, with no paired tests, one reader, no published baselines, and very low absolute EM. In its current form this is a reasonable fit for a workshop or as an exploratory report, but not for a main-track claim about learned compression.
- **Cheapest fix that changes the picture:** retrain AHCC excluding the 100 evaluation questions, evaluate on fresh HotpotQA validation questions (a few hundred), save per-question outputs, and run a paired bootstrap. That would let you state results as held-out and significance-tested.
