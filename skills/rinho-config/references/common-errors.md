# Common errors

Things that look right and fail. Most of them fail **silently**: the device answers an error and
the rule is simply not loaded.

| Mistake | What happens | Fix |
|---|---|---|
| Rule slot `0A`-`9F` (e.g. `RL6A`, `RL7B`) | Slot does not exist: device rejects it, configurator rejects it | Use `00`-`99` or `A0`-`FF` |
| Framed command inside a comment: `// send >SSSXP0111<` | The Code-mode importer loads it as a real command | Write it without `>` `<` in comments |
| Two lines for the same slot (`STD04...` twice) | Only the last one stays | Give each feature its own free slot |
| Writing rules at RL00-RL31 in **Basic** mode | Those slots belong to the guided modules and get overwritten | Custom rules at RL50+ in Basic mode |
| `SRLnnD` to "pause" a rule | **Erases** the rule | Gate it with a flag in `CND` instead. `SRLnnD` is an action: if you really mean to delete, the user sends it from the monitor, not in the block |
| Action commands inside the block (`SCXDLE`, `SRLnnD`, `SZONES`, `SZMODE`, `SZFRAC`, `SRESET`, a bare `SSSXP`/`SSSU`/`G..`) | The importer doesn't keep them; the validator says `ACTION_NOT_CONFIG` | Send them one by one from the configurator's monitor or the platform. Inside a rule they go in `ACC={...}` |
| Lower-case command (`>std00e...<`) | Treated as stray text | Commands are upper case |
| Three signals, one operator: `CND=IN00!IN01!IN02!&` | Invalid expression | One operator per pair: `IN00!IN01!&IN02!&` |
| One-shot timer without stop | Timer reloads and fires again and again | `{STDnnC}` in the rule that handles `TDnn+` |
| Timer as `STDnnE...` when the rule arms it | `E` makes it periodic from boot and saved | Define with `D`, arm from rules with `R`, stop with `C` |
| Wrong digit width (`SVL00E921` instead of `SVL00E00921`) | Field shifts, wrong value or rejected | Follow the exact widths of the doc page; use `explain` to check |
| CAN or 1-Wire commands on a **Zero** | Zero has neither: the configurator sends nothing of it | Ask the model first; use an alternative (e.g. BLE fuel probe) |
| Report `GU0...` hardcoded | UC00 may be in use (e.g. by Wialon) | Use the UC slot you defined |
| Outputs toggled with `SSSXP` as a config line | Not persistent: the importer skips it | An output that must stay on under a condition is a pair of rules: `TRG=<cond>+;ACC={SSSXP0011}` and `TRG=<cond>-;ACC={SSSXP0000}` |
| Two recipes sharing an output or flag | One recipe's trigger fires on the other's action (buzzer on → engine-cut countdown) | Separate flags/timers per feature; gate with `CND=` |
| "Clean the old rules" with `SRL**D` or `SFACTORY` | `SRL**D` erases all 196 rules; `SFACTORY` wipes everything (ID, APN, server) | Read and export the device first; delete only the slots you replace, with `SRLnnD` sent from the monitor |
| Assuming the import validates the device side | Import OK ≠ behavior OK | Test on the vehicle, especially outputs |

## When the user reports an import error

- **"Parser retornó null"** / `NOT_UNDERSTOOD`: the line has a format the configurator doesn't
  accept (slot, width, unknown field). Fix the format against the doc page.
- **"Sin handler"** / `NO_HANDLER`: the configurator does not model that command. The rest imports,
  but that line is lost if they later export from the UI. Decide with the user if it stays.
- **CAN / OneWire not available**: the selected model doesn't have that hardware.
- **`ACTION_NOT_CONFIG` / `SECURITY_COMMAND`**: the line is not configuration (see the table) or is a
  command the skill never produces. Take it out of the block and explain.
- The validator's `reason` texts are in Spanish; `FIRMWARE_TOO_OLD` and the other warning messages
  are in English. Translate both for the user.
