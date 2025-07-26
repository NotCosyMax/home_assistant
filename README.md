# HomeAssistant

A collection of my Home-Assistant configuration files (template, utility meter, blueprints, automations etc.)

## User Configurations

The "user configuration" folder contains a set of template sensors and utility meters meant to provide 
a easy but detailed breakdown on cost and profit from energy usage (imported, exported, solar, batteries).

The template sensors is divided (using name prefix) into 
"Template Sensor" (TS:) and "Template Sensor Proxy" (TSP:) groups. "Template Sensors" entities has a state that
can be directly interesting for a user. "Template Sensor Proxy" entities are meant to be used only by other entities. 
The template sensors can be trigger based, or not, depending on what makes the most sense (to me).

The Utility Meters are prefixed with "UM:" to make them easier to filter on.

### Template Sensors

Located in ```user_configuration/templates```. 

#### Grid Energy Metrics

Location ```user_configuration/templates/grid_energy_metrics.yaml```. 

Currently the following template sensors is provided

- TS: Cumulative Grid Energy Export
  - A trigger-based sensor providing the cumulative energy exported to the grid. 
    This sensor is most likely going to be the duplication of the trigger entity. The
    reason for the potential duplication is to provide a "known enitity" for other
    templates, and reduce the potential maintaince effort of changing a sensor.
- TS: Cumulative Grid Energy Import
  - A trigger-based sensor providing the cumulative energy imported from the grid. 
    For the same reason as the "export" sibling, this sensor is likely going to be
    a duplication of the trigger entity.
- TSP: Import Cost Delta
  - A trigger-based **proxy** sensor. Meant to used by the "grid_metrics" utility meter configurations
    (if used) to track import cost (h, d, w, m, y, total).
- TSP: Export Profit Delta
  - A trigger-based **proxy** sensor. Meant to used by the "grid_metrics" utility meter configurations
    (if used) to track export profit (h, d, w, m, y, total).

#### Battery Energy Metrics

Location ```user_configuration/templates/battery_energy_metrics.yaml```. 

Currently the following template sensors is provided

- TSP: Battery Cost Delta
  - A trigger-based **proxy** sensor. Meant to used by the "battery_metrics" utility meter configurations
    (if used) to track battery charge cost (h, d, w, m, y, total).
- TSP: Battery Profit Delta
  - A trigger-based **proxy** sensor. Meant to used by the "battery_metrics" utility meter configurations
    (if used) to track total battery discharge profit (h, d, w, m, y, total).
- TSP: Battery Self-Consumed Profit Delta
  - A trigger-based **proxy** sensor. Meant to used by the "battery_metrics" utility meter configurations
    (if used) to track battery charge cost (h, d, w, m, y, total). The delta
    value are teh "self-consumption" part of the "Total Discharge Profit".
- TSP: Battery Exported Profit Delta
  - A trigger-based **proxy** sensor. Meant to used by the "battery_metrics" utility meter configurations
      (if used) to track battery charge cost (h, d, w, m, y, total). The delta
      value are teh "Grid Exported" part of the "Total Discharge Profit".

### Integration Specific Templates

Located in ```user_configuration/templates/integration_specific```. These templates are 
aimed towards "fixing" and/or improving the usability of some sensors provided by
existing integrations. Each file is aimed towards a single integration, and in case
the integration is from HACS, the files is prefixed with "hacs_".

#### Nordpool

Location ```user_configuration/templates/integration_specific/hacs_nordpool.yaml```. 

Much of the "functionality" added by the sensors could maybe be done as part of the 
integration already today. But as this entitiy is re-used all over the place, it made
sense creating a seperate sensor for it (less updates if the source sensor changes.)

Currently the following template sensors is provided

- TS: Current Energy Import Price
  - A version of the current import price as given by the Nordpool integration.
    It has two the additional cost factors, VAT and "grid cost".
- TS: Current Energy Export Profit
  - A version of the current export profit as given by the Nordpool integration.
    It has one the additional profit factors.

#### FerroAmp

Location ```user_configuration/templates/integration_specific/hacs_ferroamp.yaml```. 

Currently the following template sensors is provided

- TS: Grid Import Power
  - The FerroAmp integartion provides grid power as one sensor with +- sign to
    indicate direction (import/export). This is a split version for Import only.
- TS: Grid Export Power
  - The Export version of the FerroAmp "Grid Power" split. It removes the sign
    notiotion (export is not given as a negative number).
- TS: Total Solar Energy
  - Simple "assure strictly increasing" version of the origional. This as a 
    reboot of the FerroAmp system can cause the value to "go back to 0" for a short 
    while before restoring, which messes up the Energy Dashboard if used there.
- TS: Total Battery Energy Consumed
  - Simple "assure strictly increasing" version of the origional for the same reason
    as above.
- TS: Total Battery Energy Produced
  - Simple "assure strictly increasing" version of the origional for the same reason
    as above.