# PUM Calculator

A single-file, mobile-friendly web tool that estimates the **Percentage Uniform Mark (PUM)** for Cambridge (IGCSE / A-Level) syllabi from your raw marks and the published grade thresholds.

No installation, no dependencies, no build step: just open `pum_calculator.html` in any browser.

> **Estimate only.** Official PUMs are issued by Cambridge on your Statement of Results. PUM is reported at syllabus level only, never per component.

---

## Features

- **Single-subject calculator**: enter a grade band's thresholds and your raw marks to get your PUM, grade and raw percentage.
- **Distance to the next grade**: shows how many raw marks (and uniform marks) you need to reach the next grade, with a progress bar through the current band.
- **Multi-subject overall**: add any number of subjects and get the average PUM and overall grade, with a per-subject breakdown.
- **Grade scale bar**: visual A*–G scale with your awarded grade highlighted.
- **Formula viewer**: expandable "How is PUM calculated?" panel that shows the formula with your own numbers filled in.
- **Input validation**: clear error messages for missing values, invalid thresholds, or marks outside the band.
- **Guided inputs**: fields unlock step by step (grade → total/thresholds → your marks) so nothing is entered out of order.
- **Responsive dark-gold theme**: designed for phones, works on desktop.

---

## How to Use

### Single subject

1. **Select the grade of the lower threshold**: the grade whose boundary is your lower threshold (e.g. choose `B` if your marks sit between the B and A boundaries).
2. Enter the **total marks** for the syllabus.
3. Enter the **lower threshold** and **higher threshold** (raw marks) from the grade threshold table.
4. Enter **your marks**, which must lie between the two thresholds.
5. Press **Calculate PUM** (or hit Enter in any field).

**Special case, A\*:** the higher threshold is automatically set to the total marks, since there is no grade above A\*.

### Multiple subjects

1. Press **+ Add Subject** for each subject.
2. Fill in the name (optional), grade, total, thresholds and your marks.
3. Press **Calculate Overall** to see the average PUM and overall grade.

---

## How PUM Is Calculated

PUM maps your raw mark onto a fixed 0–100 scale, where each grade owns a 10-point band:

| Grade | PUM range |
|-------|-----------|
| A\*   | 90–100    |
| A     | 80–89     |
| B     | 70–79     |
| C     | 60–69     |
| D     | 50–59     |
| E     | 40–49     |
| F     | 30–39 (IGCSE) |
| G     | 20–29 (IGCSE) |
| U     | below 20  |

For a mark between a lower threshold (`lo`) and a higher threshold (`hi`):

```
PUM = (marks − lo) / (hi − lo) × 10 + base        // all grades except A*
PUM = (marks − lo) / (hi − lo) × 11 + 90          // A* (spans 90–100)
Raw % = marks / total × 100
```

`base` is the bottom of the grade band (e.g. 70 for B). The result is clamped to 0–100.

The **overall PUM** across subjects is the simple average of each subject's PUM, then mapped back to a grade using the table above.

> PUM is **not** the same as your raw percentage; it is a rescaling so that grade boundaries fall on fixed values.

---

## Example

Grade of lower threshold: **B**, total: **300**, lower: **184**, higher: **213**, your marks: **191**

```
PUM   = (191 − 184) / (213 − 184) × 10 + 70 ≈ 72.4%
Grade = B
Raw % = 191 / 300 × 100 ≈ 63.7%
```

The tool then tells you how many more raw marks you need to reach grade A.

---

## Project Structure

```
pum_calculator.html   # Everything: markup, styles and logic in one file
README.md
```

- **HTML**: input card, results card, multi-subject card.
- **CSS**: custom properties in `:root` control the colour theme (`--accent`, `--bg`, `--card`, etc.).
- **JavaScript**: vanilla JS inside an IIFE. Core functions:
  - `calcPUM()` / `calcPUMm()`: PUM formula (single / multi-subject)
  - `gradeFromPUM()` / `gradeFromPUMm()`: PUM → grade
  - `render()`: validates input and draws the single-subject results
  - `addSubject()` / `attachSubjLocking()`: build and manage subject rows

---

## Customising

- **Colours**: edit the CSS variables at the top of the `<style>` block.
- **Grade bases**: edit the `GRADE_BASE` / `GRADE_BASE_M` objects.
- **A\* span**: change the `11` multiplier in `calcPUM()` / `calcPUMm()` if the A* band definition differs.

---

## Limitations

- Thresholds must be entered manually from Cambridge's published grade threshold tables for the relevant series.
- Marks must fall between the two thresholds you enter; the tool does not auto-detect the band.
- Overall PUM is an unweighted average of subjects.
- Results are estimates and may differ slightly from official Statement of Results figures.

---

## Browser Support

Any modern browser (Chrome, Safari, Firefox, Edge) on desktop or mobile.
