# Practical audit of candidate recharge wells: Faisalabad case study
100 candidate recharge-well sites in Faisalabad District were selected from a GIS index
(terrain, soil, runoff, Sentinel-1 flood data). This project checks each site for practical
problems (canals, drains, roads, buildings), moves problem wells to nearby clear locations,
and writes a final well list and map.

Scope: practical site screening only. This is not a hydrogeological or groundwater-quality
suitability assessment.

Previous work:[Faisalabad groundwater recharge suitability](https://github.com/rameelarustam47/Faisalabad-groundwater-recharge-suitability) 
## Result

![Wells without a visible problem](images/fine_wells_before_after.png)

| Step | Wells with no visible problem |
|---|---|
| Index selection | 66 of 100 |
| After OpenStreetMap audit and relocation (round 1) | 84 of 100 |
| After round 2 | 95 of 100 |

- 34 of the 100 original sites were on or next to a canal, drain, road or sports ground
  (17 inside, 11 next to, 6 unsure), judged by one reviewer on satellite imagery.
- The first filter (200 m from mapped drains and settlements) flagged none of them as a
  conflict. The drainage layer was a river network and the settlement layer was built-up
  land, so canals and roads were missing.
- The OpenStreetMap audit (roads 30 m, canals and drains 50 m) flagged 30 sites; 29 of them
  had a problem. It caught 29 of the 34. It missed a cricket ground and several drains
  that are not in OpenStreetMap.
- Moves of more than 1 km were unreliable in round 1 (all 6 still had a problem).
- 5 sites (28, 47, 59, 61, 87) are still near a drain, canal or houses and need a field
  visit. They are marked `check_in_field` in `Wells_final.csv`.

## How to run

1. Put these files in a Google Drive folder named `FSD_Raw_Export`:
   `Static_Predictors.tif`, `Recommended_Well_Locations_FINAL.csv` (columns `lon`, `lat`),
   `Faisalabad_Drainage_Network.shp`, `Faisalabad_Settlements_Vector.shp`,
   `Faisalabad_Boundary.shp`.
2. Open the notebook in Colab (badge above) and run the cells from top to bottom.
3. Outputs are written to the same folder: `Wells_audit.csv`, `Wells_field_check.csv`,
   `Wells_audit_OSM.csv`, `Wells_relocation.csv`, `Wells_relocation_round2.csv`,
   `Wells_final.csv`, and two figures.

The satellite-check notes in the notebook are specific to this case. For another area,
replace the `notes_*` and `problems_*` dictionaries with your own.

## Method

1. Audit the sites against mapped constraints (distance rules).
2. Check every site by hand on satellite imagery.
3. Download roads, canals and drains from OpenStreetMap near each site and audit again.
4. For each problem site, look within 2 to 3 km for the nearest cell that is far enough from
   every constraint, keeps its distance from other wells, and has a similar index score.
   The problem spot is also blocked as a 300 m circle, because a drain missing from the map
   continues beyond the point where it was seen.
5. Check the new locations on satellite imagery and repeat once.

## Limitations

- One reviewer, satellite imagery only. "Near" is a visual judgement.
- Round 1 used the reviewer's list of problem wells to decide which wells to move.
- OpenStreetMap does not contain every drain or sports ground, so "clear" only means no
  conflict was found in the layers used.
- The index is saturated in this flat plain: at the selected wells the terrain layers are at
  their maximum and scores differ by about 0.004, so the exact spot inside the best area is
  largely arbitrary.
- No groundwater quality, water source, land ownership or infiltration test is used.
  Not validated in the field. Not a groundwater model.

## Related tools

INOWAS (suitability, basin design, MODFLOW), MAR-DSS (Python decision support), GRAT
(California DWR, water available for recharge), UT Austin ASR recoverability tool. Buffer-based
site exclusion is common in solar and wind siting tools. This project adds a practical audit and
relocation step for MAR well sites.

## Data and licence

Roads, canals and drains: (c) OpenStreetMap contributors, ODbL.
Code: MIT licence (see `LICENSE`).
