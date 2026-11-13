# Unit Systems

The API works internally with **metric units**, but clients may send requests in either unit system:

- **Metric payloads** use metric field suffixes (e.g., `distance_a_b_m`, `length_km`, `temp_high_c`) — this is the format of `payload_calculate-feasible-projects.json`.
- **Imperial payloads** use imperial field suffixes (e.g., `distance_a_b_ft`, `length_mile`, `temp_high_f`).

## How the API handles each unit system

| Request | What the API does | Response |
|---------|-------------------|----------|
| Metric payload | Uses the values as-is | Metric field names and values, plus `"unit_system": "metric"` |
| Imperial payload | **Converts all values to metric on entry**, then runs the analysis | Imperial field names and values (converted back from the metric results), plus `"unit_system": "imperial"` |

Unit-system detection is automatic: the API inspects the field names (and the optional top-level `"unit_system"` field) of the payload, so no extra parameter is required.

Fields without a unit suffix (percentages, costs in $, kV, MW, degrees, dimensionless ratios, etc.) are identical in both unit systems and are left unchanged.

Technical-performance fields returned by the engine are also converted and renamed, e.g.:

- `sag_m` → `sag_ft`, `sag_at_loading_m` → `sag_at_loading_ft`
- `max_sag_m` → `max_sag_ft`
- `temp_at_ampacity_c` → `temp_at_ampacity_f`
- `total_force_n` → `total_force_lbf`, `tension_n` → `tension_lbf`

Dimensionless or non-length results (`ampacity_a`, `peak_current_a`, `resistance_at_ampacity_ohm_per_m`, `losses_at_peak_mwh_per_m`, `congestion_mw`, costs in $, etc.) keep the same field names and values in both modes.


