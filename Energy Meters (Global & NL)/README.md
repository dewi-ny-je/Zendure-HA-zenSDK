# Home energy meter packages (Global & NL)

Ready-to-use Home Assistant **package files** for the P1 and CT meters listed on the
[P1/CT meters (API's) wiki page](https://github.com/Gielz1986/Zendure-HA-zenSDK/wiki/Global-and-NL-%E2%80%90-P1-CT-meters-(API's)).

Instead of pasting a `rest:` block into your `configuration.yaml`, you drop one file
into the same `packages` folder you already use for `zendure_gielz1986_global.yaml`
(EN) or `zendure_gielz1986_nl.yaml` (NL). The procedure is then identical for every
meter, and `configuration.yaml` stays untouched.

## Available meters

| Meter | Package file | Produces | Needs |
|-|-|-|-|
| Homewizard P1 - local API v1 | [`p1_homewizard_api_v1.yaml`](./packages/p1_homewizard_api_v1.yaml) | `sensor.p1_homewizard_api_v1` | Local API enabled in the Homewizard Energy app |
| Homewizard P1 - local API v2 | [`p1_homewizard_api_v2.yaml`](./packages/p1_homewizard_api_v2.yaml) | `sensor.p1_homewizard_api_v2` | [Authorization token](https://api-documentation.homewizard.com/docs/v2/authorization) |
| Zendure P1 meter | [`p1_zendure_api.yaml`](./packages/p1_zendure_api.yaml) | `sensor.p1_zendure_api` | - |
| Ecotracker P1 meter | [`p1_ecotracker_api.yaml`](./packages/p1_ecotracker_api.yaml) | `sensor.p1_ecotracker_api` | Local HTTP-server enabled in the Ecotracker app |
| Zendure CT meter | [`ct_zendure_api.yaml`](./packages/ct_zendure_api.yaml) | `sensor.ct_zendure_api` | - |
| Shelly Pro 3EM CT meter | [`ct_shelly_pro_3em_api.yaml`](./packages/ct_shelly_pro_3em_api.yaml) | `sensor.ct_shelly_pro_3em_api` | - |

## How to use

1. Copy **only** the file for the meter you own into your `packages` folder.
2. Open it and replace the `<IP-...>` placeholder with the IP-address of your meter
   (and `<TOKEN>` for Homewizard API v2).
3. Restart Home Assistant.
4. Fill the sensor from the table above in on the settings dashboard:
   - Global (EN): **Home Energy Meter Sensor** - `input_text.home_energy_setting_meter_sensor`
   - Dutch (NL): **Afwijkende P1 sensor** - `input_text.afwijkende_p1_sensor`

   That sensor then takes priority over the built-in Homewizard reading.
5. Give the meter a fixed IP-address (DHCP reservation in your router), so the
   address in the package file cannot change.

## Good to know

- **Install one file only.** The RESTful integration polls the address once per
  second whatever it renders to, and every failed poll is logged. An address that
  does not answer - a placeholder you forgot to replace, a meter you do not own -
  fills `home-assistant.log` with `Error fetching data: ... failed with ...`.
- **Several packages may define `rest:`.** Home Assistant merges the `rest:` lists of
  all package files, so a meter package and the main Zendure package coexist fine.
- **Homewizard API v1 owners have a second option.** The optional package
  `zendure_gielz1986_homewizard_global.yaml` / `..._nl.yaml` reads the same meter
  from the IP-address field on the settings dashboard, so no file editing and no
  sensor name to fill in. Use either one, not both.
- Every sensor reports **+watt = import from the grid, -watt = export to the grid**,
  and is guarded so a briefly unreachable meter reports `unavailable` instead of
  logging a template error on each poll.
