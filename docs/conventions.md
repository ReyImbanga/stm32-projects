# Repository Conventions

Short rules that keep the repository consistent and reviewable.

## Where things go

| Folder | Content |
|---|---|
| `projects/` | Complete builds that can be demonstrated: documented, with hardware and result described |
| `learning/NN-topic/` | Guided exercises, ordered by topic. Promote an exercise to `projects/` once it is documented and complete |
| `docs/` | Setup, conventions and templates |

## Naming

- Folders: `kebab-case`, descriptive (`adc-dma-uart-monitor`, not `UART_ADC`).
- Learning topics are prefixed with a two-digit order: `01-gpio`, `02-timers`.
- A project folder name should match the name of its CubeIDE project when possible.

## What to commit

Commit: `Core/`, `Drivers/`, `Middlewares/`, the `.ioc`, `.project`, `.cproject`, `.mxproject`, the linker script, and the README.

Do not commit: `Debug/`, `Release/`, `*.elf`, `*.map`, `*.o`, `.metadata/`, `.settings/`. The root `.gitignore` already excludes them.

## Every project has a README

Copy [`PROJECT_README_TEMPLATE.md`](PROJECT_README_TEMPLATE.md). Minimum content: overview, hardware and pins, how it works, status, how to run it. Add a photo or a schematic when one exists.

Use one of these statuses:

| Status | Meaning |
|---|---|
| Documented | Complete, with a README describing hardware and behavior |
| Exercise | A learning exercise, working or experimental |
| In progress | Partly implemented, with a roadmap |
| Skeleton | Generated project with little or no application code |

## Commits

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat(uart): add HELP command to the command parser
fix(adc): correct DMA half-buffer index
docs(readme): add wiring diagram
refactor(gpio): extract button debounce into its own function
chore: update gitignore
```

One logical change per commit. Write the subject in the imperative, 72 characters or fewer.

## Branches

- `main` stays working and presentable.
- Work on short branches named `feat/<topic>`, `fix/<topic>`, `docs/<topic>`, `chore/<topic>`, then merge by pull request.

## Code style

- Keep application code inside `USER CODE` markers.
- Prefer non-blocking timing (`HAL_GetTick`) over `HAL_Delay` in application logic.
- Name pins in CubeMX with user labels (`LED_STATUS`, `BTN_MODE`) so `main.h` stays readable.
- Comment the *why*, not the *what*.
