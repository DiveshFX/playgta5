# playgta5.com investigation snapshot

Source: https://playgta5.com/ — acquired 2026-10-06.
Build: `8b0b5899ed`. Data manifest version: `1791169379-5814`.

Completed: 12,482 mirrored files, 21,158,604,965 bytes (21.16 GB / 19.71 GiB),
including 6,594 shader files expanded from the original packs. All 5,814 data
manifest entries are present; no failed transfers, missing files or local size
errors. The live data manifest was unchanged at the end of acquisition.
All received files were SHA-256 hashed during transfer; selected runtime files
were rehashed afterward. WASM signature, local isolation headers, byte ranges,
and plain/gzip batch I/O passed. Gameplay has not been tested in a browser.

The server returned two HTML files 2 bytes longer than its manifest:
`common/non_final/www/parser/psomismatch.html` and `schemas.html`. The actual
served bytes were preserved and the differences recorded in `verification.json`.

Additional public discovery found and saved `robots.txt` and `favicon.ico`.
The scan covered conventional discovery endpoints, source-map/build candidates,
known directories and their ancestors, and reachable textual references.
See `snapshot/DISCOVERY_REPORT.md` and `discovery-report.json` for every outcome.
Many uncached paths timed out or returned Cloudflare origin errors. Those results
do not prove absence. Unknown, unlinked names cannot be exhaustively enumerated
without a directory index or a server-provided inventory.

The website serves one engine and a shared data tree. Its title screen offers
GTA V Map (sandbox mode 2) and GTA VI Map (sandbox mode 3). The page explicitly
identifies the latter as the `env_test` level. This documents the site's label;
it does not establish that the assets are an official released GTA VI map.

## Files

- `mirror/playgta5.com/`: original page, runtime, WASM, title artwork, fonts,
  boot sets, shader index/packs/pipeline seeds, and every file in the data manifest.
- `data-manifest.json`: original server inventory: 5,814 files, 20,944,285,552 bytes.
- `snapshot/download-plan.json`: source URLs, expected lengths and transfer settings.
- `snapshot/manifest-sha256.json`: saved inventory, source URLs, actual lengths,
  SHA-256, available HTTP metadata and shader extraction provenance.
- `snapshot/download.log`, `status.json`, `failures.json`: transfer evidence.
- `snapshot/verification.json`: final focused validation results, when completed.
- `mirror_site.py`: resumable downloader. Initial defaults are three transfers
  and 24 MiB/s; the user requested maximum speed, so this snapshot uses 32
  concurrent transfers with no bandwidth cap.
- `serve_local.py`, `Start-Local.ps1`: local HTTP server with byte ranges,
  cross-origin isolation headers and `/data/batch` support.

The source data groups, classified by path: `gta5` 1,687 files / 8,843,043,940 bytes;
`env_test` 276 files / 5,490,639,577 bytes; shared 3,851 files / 6,610,602,035 bytes.
Both modes need shared resources; these are intentionally retained together.

Shader WGSL and constant tables are expanded byte-for-byte from the original
packs using index offsets. The packs are also retained. Derived files are marked
`derived_from` in the SHA-256 inventory; redundant loose shader downloads are avoided.

## Run locally

For the portable Windows x64 ZIP, extract the entire archive and double-click
`Launch-Local.cmd`. It starts the bundled Python server and opens your default
browser. No Python installation or internet access is required for local assets.
Keep the server window open while using the site; Ctrl+C stops it.
The bundled isolated Python 3.12.14 runtime contains only the standard library
and runtime DLLs, with its license at `runtime/LICENSE.txt`.

Alternatively, from PowerShell in this folder on the original PC:

```powershell
.\Start-Local.ps1
```

Open http://localhost:8000/ in a browser supporting WebGPU and the build's
WebAssembly features. At the title screen press Space for sandbox selection,
then 5 for GTA V Map or 6 for the site's GTA VI Map. Enter selects Story Mode.
Use Ctrl+C to stop the server. Do not open `index.html` through `file://`.

Direct links: GTA V sandbox http://localhost:8000/?mode=sandbox and the site's
GTA VI sandbox http://localhost:8000/?mode=sandbox&map=env_test .
If port 8000 is occupied, run `runtime\python.exe serve_local.py --port 8001 --open`.

The server binds only to loopback. It supplies the headers required for shared
memory and implements the byte ranges and POST batches used by the engine.
Precomputed `/data/batchc/` cache blobs are redundant server-generated caches;
the client falls back to the local batch endpoint. No server-side PHP source,
private files, save games or unpublished build history can be captured from
the public client. Remote logging is off by default in the original page.

## Resume / verify

```powershell
& 'C:\Users\sebas\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' .\mirror_site.py
& 'C:\Users\sebas\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' .\verify_snapshot.py
```

Scripts require standard-library Python 3.11 or newer. The bundled Python path
in the commands above is specific to the original PC; the portable ZIP instead
provides `runtime\python.exe` and the double-click launcher.
Completed files and `.part` transfers are retained for resumption. Final checks
cover inventory sizes, runtime hash samples, WASM signature, HTTP isolation,
range reads and both batch formats. Actual gameplay requires separate browser
validation. A public client snapshot is not the site's original development repository.

To resume with the current uncapped settings, append `--workers 32 --rate-mib 0`
to the downloader command. Content lengths from HTTP take precedence over the
source manifest when it is stale; mismatches are recorded explicitly.
