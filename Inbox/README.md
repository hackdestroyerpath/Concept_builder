# Inbox

[Back to README](../README.md)

Related files:
- [Service issue registry](../Issues/registry.jsonl)
- [Repository file index](../Repository/file_index.jsonl)

## Purpose

`Inbox/` stores incoming materials that can become service-level or concept-level issues.
A specific input folder is created only when there is a real input material.

## Input folder rule

A new input uses the path `Inbox/input_id/` and contains `entry.md`, `input_manifest.json`, and optional attachments.
`input_id` must be stable and readable, for example `input_YYYYMMDD_HHMMSS_slug`.

`entry.md` stores the source material or a short source description.
`input_manifest.json` links the input with registry records, attachments, and cleanup status.

## Current status

There are no active input folders.
Empty folders are not created because GitHub does not store directories without files.
