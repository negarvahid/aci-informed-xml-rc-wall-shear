# RC Wall Shear Strength Database

Experimental database of reinforced concrete (RC) structural walls that failed
in shear or shear-flexure under lateral loading, released with the paper:

> **Study-Agnostic and ACI-Informed Explainable Machine Learning for Shear
> Strength Prediction of Reinforced Concrete Walls**
> Negar Vahid, Gulsen Taskin, Zeynep Tuna Deger

The database was compiled by the corresponding author and co-workers from
published experimental programs. Each specimen row names its source publication.

## Contents

| File | Description |
|------|-------------|
| `walls_dataset.xlsx` | 331 wall specimens from 72 source publications, one worksheet (`Maintable (M1) (CORRECTED)`). |

The file contains the 182 shear and 149 shear-flexure walls of the authors'
full database. Walls that failed in flexure, did not fail, or have an
unreported failure mode are not included.

Rows 1 to 4 hold a merged, multi-level header; specimen data occupy rows 5 to
335. Columns T, X and AC are empty spacer columns. Columns W, AE and AF are
spreadsheet formulas derived from other columns (see below).

## Data dictionary

| Col | Field | Unit | Description |
|-----|-------|------|-------------|
| A | Paper No | | Source publication (authors, year). Merged across all specimens from that publication, so only the first row of each block is filled when read programmatically. |
| B | Specimen | | Specimen identifier as given in the source. |
| C | Thickness, $t_w$ | mm | Wall web thickness. |
| D | Length, $l_w$ | mm | Wall length. |
| E | Height, $h_w$ | mm | Wall height. |
| F | Shear span ratio, $M/(V l_w)$ | | Ratio of base moment to shear times wall length. |
| G | Aspect ratio, $h_w/l_w$ | | |
| H | Walls cross section | code | 1 = rectangular, 2 = barbell, 3 = flanged. |
| I | Failure type | code | 1 = shear (182 walls), 2 = shear-flexure, also called mixed (149 walls). |
| J | Curvature type | code | 1 = single curvature, 2 = double curvature. |
| K | Axial load ratio, $P/(A_g f_c')$ | | |
| L | $f_c'$ | MPa | Concrete compressive strength. |
| M | $\rho_{bl}$ | % | Longitudinal reinforcement ratio of the boundary element. |
| N | $f_{ybl}$ | MPa | Yield strength of boundary longitudinal reinforcement. |
| O | $\rho_{sh}$ | % | Transverse reinforcement ratio of the boundary element. |
| P | $\rho_t$ | % | Web transverse (horizontal) reinforcement ratio. |
| Q | $f_{yt}$ | MPa | Yield strength of web transverse reinforcement. |
| R | $\rho_l$ | % | Web longitudinal (vertical) reinforcement ratio. |
| S | $f_{yl}$ | MPa | Yield strength of web longitudinal reinforcement. |
| U | Loading protocol | code | Recorded for 79 specimens, in mixed formats, and not used as a model input in the paper. Two-digit codes: first digit 1 = force control, 2 = displacement control, 3 = force then displacement control, 4 = displacement then force control; second digit 0 = constant amplitude, 1 = one cycle per increment, 2 = two cycles per increment. Other entries are alphanumeric codes (for example `F1I-Y-D3I`) or references to a separate loading-protocol worksheet that is not part of this release. |
| V | $A_b/A_g$ | | Boundary element area over gross section area. |
| W | $l_w/t_w$ | | Formula: `D / C`. |
| Y | $\rho_l f_{yl}/f_c'$ | | Web longitudinal mechanical reinforcement index, with $\rho$ in percent. |
| Z | $\rho_t f_{yt}/f_c'$ | | Web transverse mechanical reinforcement index, with $\rho$ in percent. |
| AA | $\rho_{sh} f_{ysh}/f_c'$ | | Boundary transverse mechanical reinforcement index, with $\rho$ in percent. $f_{ysh}$ is not given as a separate column. |
| AB | $\rho_{bl} f_{ybl}/f_c'$ | | Boundary longitudinal mechanical reinforcement index, with $\rho$ in percent. |
| AD | Maximum shear force, $V_{max}$ | kN | Peak lateral force. |
| AE | $V/\sqrt{f_c'}$ | psi units | Normalized shear strength used as the model target: $V_{max}/(t_w l_w)$ converted to psi, divided by $\sqrt{f_c'}$ in psi. |
| AF | Shear stress | MPa | Average shear stress, $V_{max}/(t_w l_w)$. |

### Missing and special values

- A `-` in column N (one wall) marks a value that was not reported or does
  not apply.
- A value of 0 in the boundary reinforcement columns (M, N, O, AA, AB) means
  that no such reinforcement was recorded for the wall.
- Cell comments in the workbook record provenance notes: corrections applied to
  values from the source, test observations, and reasons a specimen may be
  unsuitable for some studies. A few comments are in Turkish.

## Reading the data in Python

```python
import pandas as pd

df = pd.read_excel("walls_dataset.xlsx", header=None, skiprows=4, usecols="A:AF")
df[0] = df[0].ffill()          # fill source publication down its merged block
df = df[df[1].notna()]         # keep specimen rows
```

Formula columns are read as their stored calculated values.

## How to cite

If you use this database, please cite the paper above (full reference and DOI
will be added on publication) and the original experimental studies listed in
column A.

## License

The database is released under the
[Creative Commons Attribution 4.0 International License](LICENSE) (CC BY 4.0).
You may share and adapt it for any purpose, provided you give appropriate
credit.

## Contact

Zeynep Tuna Deger (corresponding author), Earthquake Engineering and Disaster
Management Institute, Istanbul Technical University.
