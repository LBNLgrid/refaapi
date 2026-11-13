# Notes

1. **Units Consistency**: Ensure all units are consistent with the specifications above. Mixing units (e.g., miles and kilometers) will cause errors.

2. **Temperature Ranges**: Conductor temperatures should follow: `temp_dc_c ≤ temp_low_c ≤ temp_high_c ≤ max_temperature_c` (or the `_f` equivalents in imperial payloads).

3. **Economic Parameters**: The `wacc` and `inflation` values should be in decimal form (e.g., 7% = 0.07).

4. **Binary Flags**: Fields like `consider_losses`, `consider_corona`, etc., use 0 for false and 1 for true.
