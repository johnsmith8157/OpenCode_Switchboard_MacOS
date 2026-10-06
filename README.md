# OpenCode Switchboard

A small macOS app for finding OpenCode configuration locations, creating named configuration profiles, and launching OpenCode with the selected profile.

## What it does

- Shows the standard global OpenCode config locations and the custom agents detected for the selected working folder.
- Scans the selected working folder and its parent folders for project-level OpenCode configuration.
- Creates named agent setups by copying the checked agent files and disabling unchecked custom agents in that setup.
- Opens a new OpenCode terminal session using the selected profile.
- Shows the detected setup as **Current** and leaves it unchanged. It does not create a profile on first launch.

Profiles are overlays: OpenCode still loads its normal global and project settings, then loads the selected `OPENCODE_CONFIG_DIR` on top. A Switchboard agent setup copies the selected Markdown agent files and uses V2 `disabled` settings to hide other discovered custom agents for that launch. Provider and model settings still come from your normal configuration. This app does not silently copy credentials or edit your existing config.

## Requirements

- macOS 13 or later
- OpenCode installed and available in a login shell's `PATH`

No third-party libraries are required. The app uses macOS AppKit and Swift.

## Run from source

```sh
./run-from-source.command
```

## Build a shareable app

```sh
./build-macos-app.sh
```

This creates `dist/OpenCode Switchboard.app`. Zip the app to share it. Recipients may need to right-click and choose **Open** the first time because the app is not signed or notarized. For a public release, build/sign/notarize it with an Apple Developer ID.

To build the app and create the upload-ready zip in one step:

```sh
./package-macos-zip.sh
```

Pushing a version tag such as `v1.0.0` to a GitHub repository with Actions enabled automatically builds the macOS app and attaches the zip to a GitHub Release. The workflow is not code-signed or notarized; that requires Apple Developer credentials and a configured signing secret.

## Use

1. Choose a working folder. The app searches it and its ancestors for project configuration.
2. Select **Current** to use the OpenCode settings already in place, or select a saved setup to use its agent selection.
3. Choose **Launch OpenCode**. The app opens Terminal and starts a new session in the selected folder.
4. Choose **New agent setup** to create a named variation under `~/.config/opencode-profiles/`. Select the agents to include. The app copies those files, so edits to the variation don’t change the originals.

The app checks common locations (`~/.config/opencode`, `~/.opencode`, the current environment's OpenCode config paths, the selected project's ancestors, and the Switchboard profile folder). It does not crawl the entire home directory. Use **Choose folder** to inspect another project.

## Agent setups

OpenCode already includes the built-in **Build** primary agent, which is its normal default. Switchboard does not need to create a separate default agent or profile. **Current** represents the setup Switchboard discovers; named setups are optional copies that you create. An older `default` profile created by an earlier Switchboard version remains available like any other saved profile.

Other setups contain an `opencode.json` and copies of the checked agents under `agents/`. The app disables unchecked detected custom agents for that launch. You can edit copied files to change a setup without changing the originals.

## Data and safety

Switchboard stores profiles in `~/.config/opencode-profiles/`. It writes there only when you create a named setup. Launching a saved setup sets `OPENCODE_CONFIG_DIR` for the new OpenCode process; launching **Current** uses the normal OpenCode configuration. Neither action changes your original global OpenCode files. Secrets are not copied into profiles.

OpenCode's `/init` command creates or updates an `AGENTS.md` file in the selected project with project-specific instructions. It is separate from Switchboard profiles and custom agents.

## License

MIT. See [LICENSE](LICENSE).
