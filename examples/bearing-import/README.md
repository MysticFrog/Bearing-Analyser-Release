# Bearing import files

Bearing libraries are imported in the **Library Manager** with **Import CSV...** or
**Import JSON/JSONL...**. Each file becomes a new library named after the file.

The three example files here hold the same three bearings, so they can be compared side by
side. The bearing values are illustrative, not catalogue data: take real geometry and fault
frequencies from the bearing maker.

| File | Format |
|---|---|
| [bearings.csv](bearings.csv) | Comma-separated, one bearing per row, header row first |
| [bearings.json](bearings.json) | A JSON array of bearings |
| [bearings.jsonl](bearings.jsonl) | JSON Lines: one bearing object per line |

## Fields

Every bearing needs its geometry; the analyser calculates the fault frequencies from it.
Fault frequencies given in the file are used instead of the calculated ones.

| Field | CSV header | JSON key | Unit |
|---|---|---|---|
| Name | `Bearing Number` (or `Name`) | `name` | text |
| Number of rolling elements | `Number of Balls` (or `Rolling Elements`) | `rolling_elements` | whole number |
| Ball (roller) diameter | `BD mm` (or `Ball Diameter mm`) | `ball_diameter_mm` | mm |
| Pitch diameter | `Pitch mm` (or `Pitch Diameter mm`) | `pitch_diameter_mm` | mm |
| Contact angle | `Angle` (or `Contact Angle deg`) | `contact_angle_deg` | degrees |
| Outer race fault | `BPFO` | `manual_frequencies.bpfo` | see below |
| Inner race fault | `BPFI` | `manual_frequencies.bpfi` | see below |
| Ball spin | `BSF` | `manual_frequencies.bsf` | see below |
| Cage (train) | `FTF` | `manual_frequencies.ftf` | see below |
| Speed the fault values are for | - | `manual_frequencies_reference_rpm` | RPM |

## CSV

- The first line is the header. Headers are matched ignoring case, spaces and punctuation,
  so `Pitch (mm)` and `pitch_mm` both work, and columns may be in any order.
- Values are separated by commas, with `.` as the decimal point. Fields cannot be quoted, so
  a bearing name cannot contain a comma.
- The fault columns are **orders**: multiples of shaft speed, as bearing makers usually
  publish them. They are optional, but all four must be filled in on a row for any to be
  used; leave all four empty (as for `7205 Example`) to have them calculated.
- Rows missing a name or any geometry value are skipped.

## JSON and JSON Lines

- `.json` holds an array `[ {...}, {...} ]`; `.jsonl` holds one object per line, with no
  commas between lines. Blank lines are ignored.
- `manual_frequencies` is optional: leave it out or set it to `null` to have the fault
  frequencies calculated. When given, all four of `bpfo`, `bpfi`, `bsf` and `ftf` are
  required.
- The fault values are in **Hz at `manual_frequencies_reference_rpm`**, and scale with the
  running speed:
  - Orders, as in the CSV: set `manual_frequencies_reference_rpm` to `60` (one revolution per
    second), as `6205 Example` does.
  - Hz measured or published at a known speed: give that speed, as `6310 Example` does at
    1480 RPM.
