# Payload Structure

This section explains the structure and fields of the `payload_calculate-feasible-projects.json` file, the request body of the `/calculate-feasible-projects` endpoint.

The payload JSON contains eight main sections:

1. [`project_options`](#project_options) - Configuration for different upgrade options (rebuild, reconductoring, e.g.)
2. [`conductors`](#conductors) - List of available conductor types and specifications
3. [`existing`](#existing) - Current transmission line configuration (if known to the user)
4. [`data`](#data) - Project-level parameters
5. [`line_design`](#line_design) - Physical line design specifications
6. [`environment`](#environment) - Installation environment and location parameters
7. [`loading`](#loading) - Mechanical loading conditions
8. [`economics`](#economics) - Economic analysis parameters

---

## `project_options`

Defines different upgrade scenarios to evaluate. Each option represents a different approach to upgrading the transmission line.

### Available Options:
- **Rebuild** - Complete line rebuild
- **Reconductoring** - Replace conductors only
- **Voltage Upgrade** - Increase voltage level
- **HVDC** - Convert to High Voltage Direct Current

### Fields for Each Option:

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `option` | string | - | Name of the upgrade option |
| `structure_remaining_life` | integer | years | Remaining useful life of structures (0 = replace) |
| `conductor_remaining_life` | integer | years | Remaining useful life of conductors (0 = replace) |
| `voltage_kv` | integer | kV | Operating voltage level |
| `structure_upgrade` | integer | 0/1 | Whether structures need upgrading (0=no, 1=yes) |
| `nbr_bundles` | integer | - | Number of conductor bundles per phase |
| `nbr_conds_per_bundle` | integer | - | Number of conductors per bundle |
| `cost_of_structures_dol_per_unit` | integer | $/unit | Cost per structure unit |

### Structures Sub-object:

**For AC Options (Rebuild, Reconductoring, Voltage Upgrade):**

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `distance_a_b_m` | integer | meters | Horizontal distance between phase A and B conductors |
| `distance_b_c_m` | integer | meters | Horizontal distance between phase B and C conductors |
| `distance_a_c_m` | integer | meters | Horizontal distance between phase A and C conductors |
| `line_angle_deg` | integer | degrees | Line angle relative to wind direction |
| `nbr_structures` | integer | - | Total number of structures |
| `str_tgt` | integer | - | Number of tangent structures |
| `str_ra` | integer | - | Number of running angle structures |
| `str_nade` | integer | - | Number of non-angle dead-end structures |
| `str_ade` | integer | - | Number of angle dead-end structures |
| `mat_str` | string | - | Structure material type (e.g., "Steel_Pole") |
| `str_tgt_height_m` | integer | meters | Height of tangent structures |
| `str_ra_height_m` | integer | meters | Height of running angle structures |
| `str_nade_height_m` | integer | meters | Height of non-angle dead-end structures |
| `str_ade_height_m` | integer | meters | Height of angle dead-end structures |
| `str_tgt_cost` | integer | $/unit | Cost of tangent structures |
| `str_ra_cost` | integer | $/unit | Cost of running angle structures |
| `str_nade_cost` | integer | $/unit | Cost of non-angle dead-end structures |
| `str_ade_cost` | integer | $/unit | Cost of angle dead-end structures |

**For HVDC Option:**

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `distance_pos_neg_poles_m` | integer | meters | Distance between positive and negative poles |
| `str_height_m` | integer | meters | Structure height |
| `line_angle_deg` | integer | degrees | Line angle relative to wind direction |
| `nbr_structures` | integer | - | Total number of structures |
| `str_tgt` | integer | - | Number of tangent structures |
| `str_ra` | integer | - | Number of running angle structures |
| `str_nade` | integer | - | Number of non-angle dead-end structures |
| `str_ade` | integer | - | Number of angle dead-end structures |
| `mat_str` | string | - | Structure material type |
| `str_tgt_height_m` | integer | meters | Height of tangent structures |
| `str_ra_height_m` | integer | meters | Height of running angle structures |
| `str_nade_height_m` | integer | meters | Height of non-angle dead-end structures |
| `str_ade_height_m` | integer | meters | Height of angle dead-end structures |
| `str_tgt_cost` | integer | $/unit | Cost of tangent structures |
| `str_ra_cost` | integer | $/unit | Cost of running angle structures |
| `str_nade_cost` | integer | $/unit | Cost of non-angle dead-end structures |
| `str_ade_cost` | integer | $/unit | Cost of angle dead-end structures |

---

## `conductors`

Array of conductor specifications. Each conductor object contains detailed electrical, mechanical, and thermal properties.

### Conductor Types Available:
- **ACSR** - Aluminum Conductor Steel Reinforced
- **ACSS** - Aluminum Conductor Steel Supported
- **ACCC** - Aluminum Conductor Composite Core
- **AECC** - Aluminum Ecapsulated Carbon Core
- **ACCR** - Aluminum Conductor Composite Reinforced
- **ACCS** - Aluminum Conductor Composite Supported

### Conductor Fields:

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `type` | string | - | Conductor type (ACSR, ACSS, ACCC, etc.) |
| `code` | string | - | Conductor code/name (e.g., "795.0_DRAKE") |
| `cost_dol_per_km` | number | $/km | Material cost per kilometer |
| `installation_cost_dol_per_km` | number | $/km | Installation cost per kilometer |
| `accessories_cost_dol_per_km` | number | $/km | Accessories cost per kilometer |
| `area_mm2` | number | mm² | Cross-sectional area |
| `diameter_mm` | number | mm | Conductor diameter |
| `weight_n_per_m` | number | N/m | Weight per unit length |
| `conductor_rts_kn` | number | kN | Rated tensile strength |
| `temp_dc_c` | number | °C | DC resistance reference temperature |
| `temp_low_c` | number | °C | AC Low operating temperature |
| `temp_high_c` | number | °C | AC High operating temperature |
| `max_temperature_c` | number | °C | Maximum allowable temperature |
| `res_dc_ohm_per_m` | number | Ω/m | DC resistance at reference temperature |
| `res_low_ohm_per_m` | number | Ω/m | Resistance at AC low temperature |
| `res_high_ohm_per_m` | number | Ω/m | Resistance at AC high temperature |
| `elastic_modulus_gpa` | number | GPa | Elastic modulus (Young's modulus) |
| `coeff_thermal_expan_per_c` | number | 1/°C | Coefficient of thermal expansion |
| `emissivity` | number | - | Surface emissivity (0-1) |
| `solar_absorptivity` | number | - | Solar absorptivity (0-1) |
| `rugosity_coefficient` | number | - | Surface roughness coefficient |

---

## `existing`

Describes the current transmission line configuration before any upgrades.

### Fields:

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `nbr_circuits` | integer | - | Number of circuits |
| `nbr_conds_per_bundle` | integer | - | Number of conductors per bundle |
| `type` | string | - | Current conductor type |
| `code` | string | - | Current conductor code |

**Plus all conductor specification fields** (same as in [`conductors`](#conductor-fields) section) and **structure specification fields** (same as in [`Structures Sub-object`](#structures-sub-object)).

---

## `data`

Project-level configuration and analysis parameters.

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `prj_name` | string | - | Project name/identifier |
| `power_mw` | number | MW | Required/New power capacity |
| `voltage_kv` | number | kV | Operating voltage |
| `load_factor` | number | - | Load factor (0-1, typical 0.63) |
| `consider_losses` | integer | 0/1 | Include electrical losses in analysis (0=no, 1=yes) |
| `consider_corona` | integer | 0/1 | Include corona losses in analysis (0=no, 1=yes) |
| `consider_str_type` | integer | 0/1 | Consider structure type in analysis (0=no, 1=yes) |
| `customize_inv_options` | integer | 0/1 | Allow customized investment options (0=no, 1=yes) |
| `include_infeasible` | integer | 0/1 | Include infeasible conductors in results when their performance in terms of ampacity, sag, or corona is desired even if they don't satisfy the requirements (0=no, 1=yes) |

---

## `line_design`

Physical design specifications of the transmission line.

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `nbr_circuits` | integer | - | Number of circuits |
| `nbr_conds_per_bundle` | integer | - | Number of conductors per bundle |
| `length_km` | number | km | Total line length |
| `nbr_structures` | integer | - | Total number of structures |
| `max_span_m` | number | meters | Maximum span length |
| `avg_span_m` | number | meters | Average span length |
| `max_sag_m` | number | meters | Maximum conductor sag |

---

## `environment`

Environmental and meteorological conditions for the line location.

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `latitude` | number | degrees | Geographic latitude |
| `atmosphere` | string | - | Atmospheric condition (e.g., "Clear") |
| `elevation_m` | number | meters | Elevation above sea level |
| `date` | string | YYYY-MM-DD | Reference date |
| `day` | integer | - | Day of month |
| `month` | integer | - | Month (1-12) |
| `year` | integer | - | Year |
| `hour` | integer | - | Hour of day (0-23) |
| `wind_speed_m_per_s` | number | m/s | Wind speed |
| `wind_angle` | number | degrees | Wind direction angle |
| `cw_angle_direction_rel_to_north` | number | degrees | Conductor-wind angle relative to north |
| `ambient_temperature_c` | number | °C | Ambient air temperature |
| `temp_coeff_resistivity` | number | 1/°C | Temperature coefficient of resistivity |
| `weather_correction_factor` | number | - | Weather correction factor |

---

## `loading`

Mechanical loading conditions and design criteria.

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `initial_tension_percentage` | number | - | Initial tension as percentage of RTS (0-1) |
| `initial_temperature_c` | number | °C | Temperature at initial tension |
| `Loading` | string | - | Loading standard (e.g., "NESC_250B_Heavy") |
| `ice_density_kg_per_m3` | number | kg/m³ | Ice density |
| `ice_thickness_m` | number | meters | Ice thickness on the conductor |
| `pressure_pa` | number | Pa | Wind pressure |
| `wind_speed_m_per_s` | number | m/s | Design wind speed |
| `wind_ice_temperature_c` | number | °C | Temperature during ice/wind loading |
| `additive_loading_n_per_m` | number | N/m | Additional loading per unit length |
| `ld_select` | string | - | Selected loading case |

### Common Loading Standards:
- `NESC_250B_Heavy` - National Electric Safety Code (NESC), Heavy loading
- `NESC_250B_Medium` - NESC Medium loading
- `NESC_250B_Light` - NESC Light loading

---

## `economics`

Economic analysis parameters for lifecycle cost calculations.

| Field | Type | Unit | Description |
|-------|------|------|-------------|
| `conductors_lifetime` | integer | years | Expected conductor lifetime |
| `structures_lifetime` | integer | years | Expected structure lifetime |
| `conductor_remaining_life` | integer | years | Remaining life of existing conductors |
| `structure_remaining_life` | integer | years | Remaining life of existing structures |
| `cost_of_structures_dol_per_unit` | number | $/unit | Cost per structure unit |
| `wacc` | number | - | Weighted Average Cost of Capital (decimal, e.g., 0.07 = 7%) |
| `inflation` | number | - | Annual inflation rate (decimal, e.g., 0.02 = 2%) |
| `cost_of_losses_dol_per_mwh` | number | $/MWh | Cost of electrical losses |
| `cost_of_congestion_dol_per_mwh` | number | $/MWh | Cost of transmission congestion |
| `cost_structures_modif_v_upgd_dol` | number | $ | Cost of structure modifications for voltage upgrade |
| `cost_structures_modif_hvdc_dol` | number | $ | Cost of structure modifications for HVDC conversion |
| `cost_substations_upgrade_dol` | number | $ | Substation upgrade costs |
| `cost_converters_dol` | number | $ | HVDC converter station costs |


