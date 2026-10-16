# 🌱 Hypothesis Garden

An interactive, multi-page 3D website that turns five research papers on human-in-the-loop and agentic AI into something you can explore. A research agent's dataset becomes terrain, and every hypothesis it generates grows as a plant. Hypotheses that survive multiple-testing correction glow. The rest wilt, stay amber, or stay grey.

Everything runs in the browser from a single HTML file. There is no backend, no build step and no API key.

<!-- Add a screenshot or GIF of the Garden page here:
![Hypothesis Garden](docs/garden.gif) -->

**Live demo:** _add your published link here_

---

## Why this exists

Most agent demos show the agent working. Few show whether the human supervising it is still paying attention, or how many hypotheses the agent tested before reporting the one that looked good.

This project combines ideas from five papers to make those two questions visible:

1. **How many hypotheses were tried?** (forking paths and multiple-testing correction)
2. **Is the human reviewer still vigilant?** (canary tasks, attention decay, pre-commitment)

---

## Pages

| Page | What it shows | Papers |
|---|---|---|
| **Garden** | 3D terrain with one plant per testable column pair. Orbit, zoom, hover, click for r, n, raw p, Bonferroni p and BH q. Includes a guided tour of the significant plants. | P3, P2, P4 |
| **Profiler** | Missingness per column, a pairwise correlation heatmap, and a slider for the minimum pairwise sample size that gates which hypotheses are allowed. | P3 |
| **Planner Lab** | Monte-Carlo comparison of ReAct, Tree-of-Thoughts and ReAcTree over 2,000 simulated tasks, with sliders for step reliability and verifier accuracy. | P2 |
| **Review Board** | Five-dimension rubric radar, a revision loop, and a 5-vote entropy gate that decides whether to escalate to a human. | P3, P4 |
| **Human Loop** | Active vs random labeling curves, plus an oversight session with canary tasks, attention decay and a pre-commitment toggle. | P1, P5 |
| **Audit** | Bonferroni and Benjamini–Hochberg correction across all 21 hypotheses, with rainfall pairs as known-null canaries. | P3, P5 |
| **Papers** | Table mapping each page to its source paper and arXiv ID. | all |

Each page shows chips at the top naming the papers and sections that inspired it.

---

## Source papers

| ID | Paper | arXiv |
|---|---|---|
| P1 | Human-in-the-Loop Machine Learning: survey | 2108.00941 |
| P2 | Agentic AI taxonomy | 2601.12560 |
| P3 | HLER: multi-agent economic research | 2603.07444 |
| P4 | Human-on-the-loop legal discovery | 2606.19812 |
| P5 | AI agents push humans out of the loop | 2608.23642 |

> Titles above are shortened. Check each arXiv ID against the original paper before citing.

**How each idea was used**

- **P1:** uncertainty sampling vs random sampling for choosing which items a human labels.
- **P2:** planner topologies (ReAct, Tree-of-Thoughts, ReAcTree) and the agent loop that grows candidate branches.
- **P3:** dataset-aware hypothesis generation, a reviewer rubric with a revision loop, and the multiple-testing gap.
- **P4:** self-consistency entropy as an uncertainty gate for escalating to a human, and wilting for low-confidence results.
- **P5:** canary tasks, behavioral monitoring and pre-commitment as defences against reviewer disengagement.

---

## How the numbers work

All figures are computed live in your browser from seeded generators, so they are identical on every reload.

- **Dataset:** 1,500 synthetic rows × 7 columns (education, income, health, crime, unemployment, commute, rainfall), seed `11`, with per-column missingness. Rainfall is pure noise by construction.
- **Hypotheses:** all 21 column pairs. Correlations use pairwise-complete rows. P-values come from a Fisher z-transform.
- **Corrections:** Bonferroni and Benjamini–Hochberg, applied across the full family of 21 tests.
- **Gate:** a hypothesis is only "planted" if its pairwise n meets the minimum-n slider on the Profiler. The Garden regrows when you change it.
- **Shared controls:** the α slider (Audit) and the minimum-n slider (Profiler) affect every page.

### What is quoted vs simulated

| Item | Status |
|---|---|
| Feasibility rising from 41% to 87% with dataset-aware generation | **Quoted from P3**, the only paper-sourced number |
| Dataset, correlations, p-values, corrections | Computed on synthetic data |
| Active vs random labeling curves | Simulated: logistic regression, 12 seed labels, +3 per round, 8 averaged trials |
| Planner Lab results | Simulated: 350 tokens per step, ToT samples 3 candidates, ReAcTree uses 3-step subtrees with one verified retry |
| Review Board scores | Heuristic functions of r, n and missingness. A revision adds +0.12 to the weakest dimension. |
| Attention decay and the ×1.5 pre-commitment effect | Modeling assumptions, **not** measurements from P5 |

**This is a learning and visualisation project. It is not a reproduction of the papers' experiments and not a live LLM benchmark.**

---

## Run it locally

No install is needed.

```bash
git clone https://github.com/<your-username>/hypothesis-garden.git
cd hypothesis-garden

# Option 1: open the file directly
open hypothesis_garden.html        # macOS
# xdg-open hypothesis_garden.html  # Linux

# Option 2: serve it
python3 -m http.server 8000
# then visit http://localhost:8000/hypothesis_garden.html
```

An internet connection is needed on first load for two external resources:

- three.js r128 from cdnjs
- the Fraunces font from Google Fonts (falls back to Georgia)

---

## Controls

| Action | Result |
|---|---|
| Drag | Orbit the garden |
| Scroll / pinch | Zoom |
| Hover a plant | Show hypothesis name and r |
| Click a plant | Fly to it and show full statistics |
| Start tour | Visit each significant plant in turn |
| ♪ Sound | Toggle ambient hum and selection chimes (off by default) |

The garden drifts slowly when idle. Reduced-motion system settings disable growth animation, drift and orbit.

---

## Tech

- Vanilla HTML, CSS and JavaScript in one file, with a hash router (`#/garden`, `#/profile`, `#/planner`, `#/review`, `#/human`, `#/audit`, `#/papers`)
- [three.js](https://threejs.org/) r128 for the Garden scene
- Canvas 2D for charts and the rubric radar
- Web Audio API for optional sound
- Seeded PRNG (mulberry32-style) for reproducible results

---

## Limitations

- Synthetic data only. The "significant" hypotheses are significant because the generator built those relationships in.
- Planner, labeling and attention results depend on the stated simulation assumptions and say nothing about real model performance.
- Review Board scores are not produced by an actual reviewer model.
- Only the Garden page uses 3D. The other pages use tables and 2D charts.
- The canvas size is set when a page loads and does not resize live.

---

## Roadmap

- [ ] 3D labeling view for the Human Loop page
- [ ] 3D Planner Lab
- [ ] Upload your own CSV and profile it in the browser
- [ ] More hypothesis types beyond pairwise correlation
- [ ] Optional live reviewer agent behind a user-supplied API key
- [ ] Real user sessions on the canary task, replacing the simulated decay model

---

## Contributing

Issues and pull requests are welcome. If you change a number or assumption, update the "What is quoted vs simulated" table so readers can see what is real and what is modeled.

---

## Author

Built by **Soham** · [Scadiva](https://github.com/<your-username>)

## License

_Choose a license (MIT is a common default for demos) and add a `LICENSE` file._
