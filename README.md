# Duskvale — website

Static site for the Duskvale mobile app, served by GitHub Pages at
<https://nodabasi.github.io/duskvale/>. The repository is `duskvale`; the working copy
sits beside the app itself, in `Duskvale-Proje/Duskvale-site/`.

Plain HTML and one stylesheet — no build step, no dependencies. Edit a file, commit,
push; Pages redeploys in a minute or two.

## Pages

Turkish is the default language and sits at the root; English lives under `/en/` for store
review and for anyone who lands here without Turkish. The app's own interface is available
in ten languages, but the site only needs these two.

| URL | File | Used in the stores as |
|---|---|---|
| `/duskvale/` | `index.html` | Marketing URL (Turkish) |
| `/duskvale/support/` | `support/index.html` | Support URL (Turkish) |
| `/duskvale/privacy/` | `privacy/index.html` | Privacy Policy URL (Turkish) |
| `/duskvale/en/` | `en/index.html` | Marketing URL (English) |
| `/duskvale/en/support/` | `en/support/index.html` | Support URL (English) |
| `/duskvale/en/privacy/` | `en/privacy/index.html` | Privacy Policy URL (English) |

**Do not rename these directories.** Once the paths are registered in App Store Connect
and Google Play Console, changing one breaks a live store link.

The absolute URLs above appear in each page's `hreflang` tags. If the repository is
published under a different name, those tags have to be updated with it — everything else
on the site uses relative links.

## Editing

- Colours and layout live in `assets/style.css`; every page shares it. The palette
  follows the app: dark ground, gold and twilight violet accents.
- `assets/logo.png` is the app icon scaled down to 320 px.
- Links are relative, so the site also opens correctly straight from the filesystem.
- When the privacy policy changes in substance, update the effective date at the top of
  both `privacy/index.html` and `en/privacy/index.html`.
- Each page declares an `hreflang` pair pointing at both language versions of itself.
  Adding a page means adding both halves, or the pair goes stale.

## Keeping the pages true

The site makes concrete claims about the app, and the app has to keep honouring them. If
any of these change in `Duskvale/`, the matching section changes here too — in both
languages:

- **The app has no server.** It persists two things in AsyncStorage: the chosen interface
  language under `duskvale.language` (`src/i18n/useLanguage.ts`) and favourite mixes under
  `duskvale.favorites` (`src/store/useFavoritesStore.ts`). The palette
  (`src/store/usePaletteStore.ts`) is in-memory only. Adding any account, sync or remote
  logging rewrites the privacy policy.
- **Ads are AdMob banners**, one above and one below the mixer, and none on the
  visualiser (`src/components/AdBanner.tsx`). Consent is gathered through Google's UMP
  form plus ATT on iOS (`src/ads/useAdsStore.ts`). A new ad format, a new placement or an
  analytics SDK all change the Advertising section.
- **The catalogue is 28 recordings in six families** — rain 8, wind 4, water 4, fire 4,
  night nature 4, resonance 4 (`src/audio/catalog.ts`). Both landing pages print those
  counts.
- **Background playback (since 1.1).** iOS has `UIBackgroundModes/audio`; Android runs a
  media-playback foreground service with a notification while sound plays
  (`src/audio/background.ts`), which adds the foreground-service and notification
  permissions listed in the privacy policy. Both support pages describe it.
- **Ten interface languages** (`src/i18n/translations.ts`), listed on both landing pages
  and both support pages.

## Local preview

```sh
python3 -m http.server 8000
# then open http://localhost:8000/
```
