# Repository Guidelines

## Project Structure & Module Organization

This is a browser extension with its distributable source in `extension/`. The Manifest V3 entry point is `extension/manifest.json`; it registers `background.js` as the service worker, `popup/popup.html` as the toolbar UI, and `options/options.html` as the settings UI. Keep each UI's JavaScript and CSS beside its HTML (`popup/` and `options/`). Static extension artwork belongs in `extension/images/`. `README.md` covers installation and `RELEASES.md` records release notes.

## Build, Test, and Development Commands

There is no package manager, bundler, or automated test command. Develop by loading the `extension/` directory directly:

```text
Chrome:  chrome://extensions → Developer mode → Load unpacked → extension/
Firefox: about:debugging → This Firefox → Load Temporary Add-on → extension/manifest.json
```

After changing source files, use the browser's Reload control for the extension, then reopen the popup. Use the extension service worker inspector to investigate background-script errors.

## Coding Style & Naming Conventions

Write browser-native JavaScript using `const`/`let`, semicolons, and two-space indentation, following the dominant style in `popup.js` and `options.js`. Use camelCase for functions and local variables (`fetchHadith`, `preferredTheme`); reserve UPPER_SNAKE_CASE for constants (`ALARM_NAME`). Keep Chrome extension API calls (`chrome.storage`, `chrome.alarms`, `chrome.action`) compatible with the manifest's MV3 service-worker model. Use descriptive DOM IDs that match the markup, such as `hadith-reference`. CSS uses two-space indentation, kebab-case custom properties, and `dark-mode` class overrides.

## Testing Guidelines

No automated test framework or coverage threshold is configured. Manually verify affected flows in both Chrome and Firefox where possible: popup loading, Arabic and English content direction, daily versus random fetch modes, theme selection, saved preferences, option-page navigation, and the unread badge alarm. Check the browser console for API, network, and service-worker errors. Add focused automated tests only alongside the tooling needed to run them consistently.

## Commit & Pull Request Guidelines

Recent history favors short imperative subjects, for example `Add hadith source`, `Fix badge showing even if user has interacted already`, and `Release v1.7.0`. Keep commits narrowly scoped; use release commits only for versioned releases. Pull requests should explain the user-visible change, link the relevant issue when available, and include screenshots for popup/options UI or theme changes. Note the browsers and manual scenarios tested, and update `README.md` or `RELEASES.md` when behavior, installation, or release notes change.

## Configuration & Security

Treat `manifest.json` permissions and external API URLs as security-sensitive. Request only necessary permissions, preserve the existing Firefox `browser_specific_settings`, and review any new network endpoint before adding it.
