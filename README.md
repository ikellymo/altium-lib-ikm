# Altium libraries

Ian's Altium Designer component library: one symbol library and one footprint library.

| File | Contents |
|---|---|
| `Components.SchLib` | 76 symbols, each with a `Category` parameter |
| `Components.PcbLib` | 82 footprints, with embedded 3D models where available |

Each symbol links to its footprint by name with no library path, so Altium finds it in any
installed library. On the Windows VM this repo is cloned to
`C:\Users\ikellymo\projects\altium-lib-ikm`, and both files are installed in Altium from that
clone. Don't install or run git on a copy on the Parallels share (`X:`).

## Categories

Filter or group by the `Category` parameter in the Components panel.

| Category | Parts |
|---|---|
| Power | TLF35584QKVS2XUMA2, TPS65381AQDAPRQ1, LT8650SEV#PBF, TPS61088RHLR, TPS259531DSGR, LTC4380HMS-1#PBF, LM74700QDBVTQ1, NCP1117ST33T3, TPS3700QDDCRQ1, LM4040AIM3-10.0, LM4040D41IDBZT, BTS7008-1EPA, BTS70082EPAXUMA1 |
| Processors_Memory | RP2354B, W25Q128JVPIQ_TR |
| Interface | MCP2518FDT-E_SL, MCP251863T-E_SS, MCP251863T-E_9PX, MCP251863T-H_SSVAO, ISOW1044BDFMR, W5500, TIC12400DCPR, MC33CD1030AE, MAX9927AEE+, VOMA617A-3X001T |
| Sensors | BMI088, MMC5983MA |
| Modules | UM982, 453-00084R, RMC20452T (Raspberry Pi Radio Module 2) |
| Discretes | BUK9M43-100EX, BUK9Y19-100E, DMG1012T, DMG1013T-7, SMBJ36CA, MMBZ27VCLT1G, BZX384B5V1-E3-18, PMEG6010ELR, BAT54S, PRTR5V0U2X |
| Analog_Logic | NE555DR |
| Passives | XFL4020-472MEC, XFL5030-102MEB, AOTA-B201610S3R3-101-T, ABM8-272-T3, 749010012A, 0452002.MRL, EEH-ZA1H101P |
| Connectors | CONN_dualrow_{N}pos_jump (N = 4 to 50), CONN_dualrow_10pos_alt_skip7, FRDM-A-S32K358_SHIELD, DT13-4P-G003, 632723300011 |

Use one of these values for new parts; add a new category only when none fits.

The footprint library also holds the generic chip footprints the JLCPCB symbols ask for:
CAP-0402, CAP-0603, CAP-0805, CAP-1206, IND-0402, RES-0402, RES-0603, RES-1206.

## Adding parts

`X:\AltiumLL\SamacSys.SchLib` / `.PcbLib` is an inbox, not a library to keep parts in.

1. Download the part with the SamacSys Library Loader (Tools > Symbols | Footprints |
   3D Models). It lands in the inbox.
2. Copy the symbol (SCH Library panel) into `Components.SchLib` and the footprint
   (PCB Library panel, which keeps the 3D model) into `Components.PcbLib`.
3. In the copied symbol: clear the footprint link's library path, add the `Category`
   parameter, and change an `IC?` designator to `U?`.
4. If the part is already placed, update it from `Components.SchLib`.
5. Delete the part from the inbox and save both libraries.
6. Commit and push.

Parts built by hand from a datasheet go straight into `Components.*` (steps 3, 6).

## Not in this repo

- JLCPCB passive symbols (464 MB, third party): `X:\altium_jlcpcb_libraries_2025-04-18\`,
  from https://github.com/gsuberland/altium_jlcpcb_libraries/
- The SamacSys inbox (`X:\AltiumLL\`), and Altium's automatic backups (`History/`).

Many parts came from SamacSys / Component Search Engine, whose models have their own terms
of use. Keep this repo private.

## Conventions

- Standard packages: IPC Compliant Footprint Wizard, pads overridden to the manufacturer's
  recommended land pattern, STEP model embedded.
- Footprint layers: Top Overlay = silk and pin-1 mark, Mechanical 13 = assembly outline,
  Mechanical 15 = courtyard.
- Footprint names: `<PACKAGE>_<PART>` or `<VENDOR>_<PACKAGE>` for part-specific land
  patterns; SamacSys parts keep their IPC names (e.g. `SOIC127P600X175-8N`).
- Designators: `U?` ICs and modules, `Q?` FETs, `D?` diodes, `L?` inductors, `Y?` crystals,
  `J?` connectors.

## Workflow

1. Edit and save the library in Altium.
2. Commit and push from the local clone, then pull on any other machine before editing there.
3. Library files are binary: git cannot merge them, and every commit stores the whole file.
   Avoid editing on two machines at once. If the repo grows too large, split into one file
   per component.

## Open items

- LFPAK56_SOT669 has no 3D body. Add the Nexperia STEP (`BUK9Y19-100E,115.stp`) with
  Place > 3D Body and set its Z offset so the part sits on the board.
- Footprints built from datasheets still to check against the manufacturer drawings:
  LFPAK56_SOT669 drain pads, TI_RHL0020A thermal-pad fingers, TI_DSG0008A paste layout.
- IFX_PG-TSDSO-14 and PG-TSDSO-14_BTS7008 are the same package; consider keeping one.
