# InfraRed-Codec-DB

Exported IR codec database from the **Xiaomi Mi Remote** IR signal database
(the proprietary DB used by the Mi Home / Mi Remote app), decrypted and
rendered with [`mi_remote_database`](https://github.com/ysard/mi_remote_database)
(see the `Noxcis/mi-remote-database` fork for the AC translation engine).

The repo uses the same directory layout as the
[Flipper-IRDB](https://github.com/IR-RESEARCH/Flipper-IRDB) collection:

```
<Device_Category>/<Brand>/<model>.ir
```

Every file is a standard Flipper Zero `.ir` file (raw timings) and is
consumable by Flipper Zero directly and by Android readers that understand
the Flipper IRDB layout, e.g. `nope-remote`'s
`FlipperDevicesIRDBRepository`.

## Layout

```
ACs/                        Air conditioners
Audio_and_Video_Receivers/  AV receivers
Cable_Boxes/                Cable / satellite receivers
Cameras/                    Digital cameras
DVD_Players/                DVD players
Fans/                       Fans
Projectors/                 Projectors
Set_Top_Boxes/              Set-top boxes
Streaming_Devices/          Android / media boxes
TVs/                        Televisions
```

The device categories mirror the Flipper-IRDB naming. Each category is split
into brand folders, and each brand folder holds one `.ir` file per remote
model (named after the Mi Remote model id).

## Source

| Category            | Mi Remote `deviceid` |
| ------------------- | -------------------- |
| `TVs`               | 1                    |
| `Set_Top_Boxes`     | 2                    |
| `ACs`               | 3                    |
| `DVD_Players`       | 4                    |
| `Fans`              | 6                    |
| `Audio_and_Video_Receivers` | 8             |
| `Projectors`        | 10                   |
| `Cable_Boxes`       | 11                   |
| `Streaming_Devices` | 12                   |
| `Cameras`           | 13                   |

## Regenerating

The exports are built from a Mi Remote database dump
([`dump_db`](https://github.com/ysard/mi_remote_database#database-dump-format)
format) using `scripts/build_flipper_irdb.py` from the
`mi-remote-database` repository:

```bash
# translate AC brand files so the AC "others" entries are rendered
make translate_ac

# regenerate every category
python3 scripts/build_flipper_irdb.py --db-path ./database_dump --output ./ircdb_out

# restrict to a few categories / buttons
python3 scripts/build_flipper_irdb.py --db-path ./database_dump --output ./ircdb_out -c ACs TVs -k power power_r
```

## Licensing

The code is derived from the Mi Remote database. The source tooling is
distributed under [AGPL-3.0](https://www.gnu.org/licenses/agpl-3.0.html);
this repo's `.ir` exports are made available under the same license.
