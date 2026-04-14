flowchart LR
  "stromligning_current_price_vat" --> "ev_night_reference_price_real"
  "grid_export_power_average_2m_safe" --> "ev_virtual_day_price_real"
  "stromligning_current_price_vat" --> "ev_virtual_day_price_real"
  "ev_night_reference_price_real" --> "ev_price_delta_real"
  "ev_virtual_day_price_real" --> "ev_price_delta_real"

  "ev_price_delta_real" --> "ev_charge_decision"
  "ev_deadline_risk" --> "ev_charge_decision"
  "ev_session_complete" --> "ev_charge_decision"
  "ev_charge_mode" --> "ev_charge_decision"
  "ev_session_active" --> "ev_charge_decision"
  "ev_cable_connected_effective" --> "ev_charge_decision"

  "ev_charge_decision" --> "ev_start_charging_stable"
  "ev_charge_decision" --> "ev_stop_charging_stable"
  "ev_start_charging_stable" --> "script.ev_charge_start"
  "ev_stop_charging_stable" --> "script.ev_charge_stop"

  "ev_should_force_intellicharge_allow" --> "intellicharge_acquire_ev_override"
  "intellicharge_acquire_ev_override" --> "script.intellicharge_apply_allow"
  "script.intellicharge_apply_allow" --> "rest_command.intellicharge_set_allow_always"
  "script.intellicharge_apply_allow" --> "intellicharge_desired_mode"

  "ev_intellicharge_smart_status_2" --> "intellicharge_smart_sync_status"
  "intellicharge_desired_mode" --> "intellicharge_smart_sync_status"