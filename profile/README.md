# Parabellum

**A Declarative Native Plugin Runtime (Composable Microkernel Runtime) written in Zig.**

Instead of shipping 500 MB monolithic executables, Parabellum separates **orchestration** from **capabilities**:

- **For users**: Install a single native runtime (`parabellum-engine`). Applications are lightweight declarative YAML manifests (`pipeline.yaml`, `configs.yaml`, `schemas.yaml`). The runtime resolves, caches, and shares the required native `.so` plugins automatically under `~/.parabellum/packages/`.
- **For developers**: Build small, reusable native `.so` plugins (tasks, datasource drivers, kernel extensions) against the C-ABI `parabellum_sdk`, without bundling an engine or maintaining per-app distribution pipelines.

---

## Core Repositories

| Repository | Description |
|---|---|
| [engine](https://github.com/parabellum-kernel/engine) | The microkernel runtime: DAG scheduler, reactive router, C-ABI plugin loader, and daemon. |
| [sdk](https://github.com/parabellum-kernel/sdk) | Unified `parabellum_sdk` for building Task, Driver, and Extension plugins, plus in-process testing (`TaskHarness`, `TestEngine`). |
| [public-docs](https://github.com/parabellum-kernel/public-docs) | Public architecture and SDK specifications. |

---

## Ecosystem Components

### Drivers ([driver](https://github.com/parabellum-kernel/driver))
Pluggable storage and datasource backends (`libdriver_<name>.so`):
- `file_store` — Local filesystem blob/file storage
- `memory` — In-memory ephemeral store (ideal for pipelines & tests)
- `sqlite` — SQLite relational datasource
- `ssh_store` — Remote storage over SSH/SFTP
- `yaml` — YAML-backed structured datasource

### Kernel Extensions ([extension](https://github.com/parabellum-kernel/extension))
Inbound transports, observability, and lifecycle hooks (`libext_<name>.so`):
- **Inbound Transports**: `inbound_cli`, `inbound_http`, `inbound_grpc`, `inbound_fs`
- **Observability & Diagnostics**: `opentelemetry`, `trace_jsonl`, `pcapng`, `dag_viz`

### Libraries ([lib](https://github.com/parabellum-kernel/lib))
Zero-dependency reusable Zig libraries shared across plugins and apps:
- **Media & Container Parsers**: `isobmff` (MP4), `ebml` (MKV/WebM), `riff` (AVI/WAV), `media_ir`, `tika` (magic/content detection)
- **Archives & Metadata**: `zip_archive`, `comicinfo`
- **Data & Storage**: `sql`, `sqlite`, `yaml`, `vfs`

---

## Reference Applications

Real-world applications built as declarative YAML pipelines + task plugins on top of Parabellum:

| Application | Description |
|---|---|
| [vidsec](https://github.com/parabellum-kernel/vidsec) | Video container security & forensics analyzer (MP4/MKV/AVI triage, structural dissection, anomaly detection, and stream remediation) |
| [mangadex](https://github.com/parabellum-kernel/mangadex) | Source-agnostic manga ingestion, image analysis, and CBZ archiving with embedded metadata (`ComicInfo.xml`) |
| [vault](https://github.com/parabellum-kernel/vault) | Virtual filesystem (VFS) catalog with chunked data management and bidirectional file streaming (inbound & outbound) over pluggable storage backends |
