flowchart LR
  subgraph Price_and_Energy
    "stromligning_current_price_vat"
    "grid_export_power_average_2m_safe"
    "ev_night_reference_price_real"
    "ev_virtual_day_price_real"
    "ev_price_delta_real"
    "ev_remaining_hours_needed"
    "ev_deadline_risk"
    "ev_session_complete"
  end

  subgraph EV_Control
    "ev_charge_mode"
    "ev_ha_control_enabled"
    "ev_session_active"
    "ev_cable_connected_effective"
    "eh45khwp_power"
  end

  subgraph EV_Decisions
    "ev_charge_decision"
    "ev_charge_power_mode"
  end

  subgraph EV_Execution
    "ev_start_charging_stable"
    "ev_stop_charging_stable"
    "script.ev_charge_start"
    "script.ev_charge_stop"
    "switch.eh45khwp_charger_enabled"
  end

  subgraph IntelliCharge_Status
    "ev_intellicharge_smart_status_2"
    "intellicharge_desired_mode"
    "intellicharge_smart_sync_status"
  end

  subgraph IntelliCharge_Override
    "ev_should_force_intellicharge_allow"
    "intellicharge_overridden_by_ev"
    "intellicharge_pre_ev_mode"
    "intellicharge_override_started_at"
    "intellicharge_acquire_ev_override"
    "script.intellicharge_apply_allow"
    "script.intellicharge_apply_smart"
    "rest_command.intellicharge_set_allow_always"
    "rest_command.intellicharge_set_smart"
  end

  "stromligning_current_price_vat" --> "ev_night_reference_price_real"
  "ev_remaining_hours_needed" --> "ev_night_reference_price_real"

  "stromligning_current_price_vat" --> "ev_virtual_day_price_real"
  "grid_export_power_average_2m_safe" --> "ev_virtual_day_price_real"

  "ev_night_reference_price_real" --> "ev_price_delta_real"
  "ev_virtual_day_price_real" --> "ev_price_delta_real"

  "ev_remaining_hours_needed" --> "ev_deadline_risk"

  "ev_charge_mode" --> "ev_charge_decision"
  "ev_ha_control_enabled" --> "ev_charge_decision"
  "ev_session_active" --> "ev_charge_decision"
  "ev_cable_connected_effective" --> "ev_charge_decision"
  "ev_price_delta_real" --> "ev_charge_decision"
  "ev_deadline_risk" --> "ev_charge_decision"
  "ev_session_complete" --> "ev_charge_decision"
  "eh45khwp_power" --> "ev_charge_decision"

  "ev_ha_control_enabled" --> "ev_charge_power_mode"
  "ev_session_active" --> "ev_charge_power_mode"
  "ev_cable_connected_effective" --> "ev_charge_power_mode"
  "ev_charge_mode" --> "ev_charge_power_mode"
  "ev_price_delta_real" --> "ev_charge_power_mode"
  "ev_deadline_risk" --> "ev_charge_power_mode"
  "ev_session_complete" --> "ev_charge_power_mode"

  "ev_charge_decision" --> "ev_start_charging_stable"
  "ev_charge_decision" --> "ev_stop_charging_stable"

  "ev_start_charging_stable" --> "script.ev_charge_start"
  "ev_stop_charging_stable" --> "script.ev_charge_stop"

  "script.ev_charge_start" --> "switch.eh45khwp_charger_enabled"
  "script.ev_charge_stop" --> "switch.eh45khwp_charger_enabled"

  "ev_intellicharge_smart_status_2" --> "intellicharge_smart_sync_status"
  "intellicharge_desired_mode" --> "intellicharge_smart_sync_status"

  "ev_should_force_intellicharge_allow" --> "intellicharge_acquire_ev_override"
  "intellicharge_overridden_by_ev" --> "intellicharge_acquire_ev_override"
  "ev_intellicharge_smart_status_2" --> "intellicharge_acquire_ev_override"

  "intellicharge_acquire_ev_override" --> "intellicharge_pre_ev_mode"
  "intellicharge_acquire_ev_override" --> "intellicharge_override_started_at"
  "intellicharge_acquire_ev_override" --> "intellicharge_overridden_by_ev"
  "intellicharge_acquire_ev_override" --> "script.intellicharge_apply_allow"

  "script.intellicharge_apply_allow" --> "rest_command.intellicharge_set_allow_always"
  "script.intellicharge_apply_allow" --> "intellicharge_desired_mode"

  "script.intellicharge_apply_smart" --> "rest_command.intellicharge_set_smart"
  "script.intellicharge_apply_smart" --> "intellicharge_desired_mode"

  "ev_charge_decision" -. påvirker behov for override .-> "ev_should_force_intellicharge_allow"