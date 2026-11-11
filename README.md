# Roon for Home Assistant, with stable paths and search

A drop-in replacement for Home Assistant's built-in [Roon integration](https://www.home-assistant.io/integrations/roon/).
It is the built-in integration from Home Assistant 2026.11.1, plus:

- **Stable `path:` media IDs.** Browsing gives every item an ID made from its place in the Roon menus,
  e.g. `path:Playlists/Store badet`. Roon's own item keys change between sessions, so automations that
  store them break; path IDs keep working after restarts. A `/` inside a name is encoded, so
  `AC/DC` is safe.
- **Library search.** The entity supports Home Assistant's media search across Library (Artists, Albums,
  Tracks, Composers), Playlists and Genres, returning path IDs you can play directly.
- **Clear playback errors.** If Roon does not answer, or a path does not exist, `media_player.play_media`
  raises an error that says so instead of failing inside `roonapi`.

Because it uses the same domain (`roon`), Home Assistant loads it *instead of* the built-in integration.
Existing Roon config entries and entities carry on unchanged.

## Install

**HACS:** add this repository as a custom repository (category *Integration*), install it, restart Home Assistant.

**Manually:** copy `custom_components/roon` into your Home Assistant `config/custom_components/`, restart.

## Use in an automation

```yaml
- action: media_player.play_media
  target:
    entity_id: media_player.bathroom_airplay
  data:
    media_content_type: library
    media_content_id: path:Playlists/Store badet
```

Find the ID of anything by browsing the player in **Media** in Home Assistant, or with media search.

## Keeping up with Home Assistant

This copy does not change when Home Assistant updates its built-in integration. To catch up, take the
built-in `homeassistant/components/roon` from the new release and re-apply the additions in
`media_browser.py` (path browsing) and `media_player.py` (`play_media` path handling and the search methods).

## Licence

Apache License 2.0, like Home Assistant, which this is derived from. See [NOTICE](NOTICE).
