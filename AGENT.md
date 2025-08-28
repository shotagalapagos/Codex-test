# Agent Memo

- Purpose: Simple survey UI where Q2 changes based on Q1.
- Mechanism: Add a small JS mapping (`q2TextMap`) and an `updateQ2ByQ1()` function that updates Q2’s title when Q1 changes.
- Minimalism: No frameworks, no external deps. Works offline.
- Accessibility: Star rating keeps native radios; `role="radiogroup"` is present. If extending, ensure labels remain clickable and required states are announced by browser.
- IDs/Selectors: `q2TitleText`, `q2Hint`, and `input[name="discover"]` are the primary hooks. Avoid renaming without updating the script.
- Reset behavior: On Q1 change, Q2 radios clear to avoid stale answers.
- Extensibility: To branch UI further (e.g., swap star rating for a dropdown), toggle visibility within `updateQ2ByQ1()` based on the selected key.
