# AGENTS.md

## Cursor Cloud specific instructions

This is a **Space Station 14** fork (C#/.NET 9 game, Robust Toolbox engine). Client-server architecture: the server runs headless, the client requires GPU/display.

### Key setup facts
- **.NET 9 SDK** (9.0.100+) is required — see `global.json`. Installed at `/usr/share/dotnet`.
- **Git submodules** must be initialized (`git submodule update --init --recursive`) — `RobustToolbox` engine lives there.
- The **game client** (`Content.Client`) requires OpenGL/SDL2/GLFW and a display — **cannot run in headless Cloud VM**.
- The **game server** (`Content.Server`) runs headless fine: `dotnet run --project Content.Server`.

### Build & test
- Build: `dotnet build --configuration DebugOpt --no-restore /m`
- Unit tests: `dotnet test --no-build --configuration DebugOpt Content.Tests/Content.Tests.csproj -- NUnit.ConsoleOut=0`
- Integration tests: `dotnet test --no-build --configuration DebugOpt Content.IntegrationTests/Content.IntegrationTests.csproj -- NUnit.ConsoleOut=1 NUnit.MapWarningTo=Failed` (set `DOTNET_gcServer=1`)
- See `.github/workflows/build-test-debug.yml` for CI reference.

### Gotchas
- First `dotnet` command after SDK install prints a welcome banner — this is normal.
- Build produces many warnings from upstream RobustToolbox code — these are expected and not from this fork's changes.
- The server uses SQLite by default for development — no external database needed.
