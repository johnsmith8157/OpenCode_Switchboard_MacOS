# OpenCode Switchboard

OpenCode Switchboard is a small macOS utility for viewing the OpenCode setup detected on your Mac, creating named agent setups, and launching OpenCode in a chosen working folder.

This repository currently distributes the **prebuilt macOS app**. It does not include the app's source code or instructions for building it.

## Download and open

1. Download `OpenCode-Switchboard-macOS.zip` from the project's GitHub Release.
2. Unzip it and move **OpenCode Switchboard.app** to Applications or another location.
3. Open the app. Because the app is not signed or notarized, macOS may require you to Control-click it and choose **Open** the first time.

The app requires macOS 13 or later and OpenCode installed and available in a login shell's `PATH`.

## Use

1. Choose a working folder. Switchboard searches it and its parent folders for project-level OpenCode configuration.
2. Select **Current** to launch with the OpenCode setup already in place, or select a saved setup.
3. Choose **Launch OpenCode**. Switchboard opens Terminal and starts a new OpenCode session in the selected folder.
4. Choose **New agent setup** to create a named copy of selected custom agents under `~/.config/opencode-profiles/`. You can edit those copies without changing the originals.

Switchboard checks common OpenCode configuration locations and selected project ancestors; it does not crawl the entire home directory. Use **Choose folder** to inspect another project.

## Profiles and OpenCode defaults

**Current** represents the setup Switchboard detects from your existing OpenCode configuration. Switchboard does not create a profile on first launch or change your existing configuration.

OpenCode already provides the built-in **Build** primary agent as its normal default. The `/init` command is separate: it creates or updates an `AGENTS.md` file with project-specific instructions.

Named setups contain copies of the selected agent Markdown files and an `opencode.json` that disables detected custom agents you did not select. Launching a saved setup sets `OPENCODE_CONFIG_DIR` for that OpenCode session. Launching **Current** uses the normal OpenCode configuration. Switchboard does not copy credentials into profiles.

If you see a saved profile named **default**, it may have been created by an earlier version of Switchboard; it is treated like any other saved profile.

## License

MIT. See [LICENSE](LICENSE).
