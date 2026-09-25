# M5.5 – File-Based Workflow Persistence

## Purpose

M5.5 provides the first concrete persistence implementation for the workflow module while keeping the domain and SPI layers independent of storage technology.

## Implementation

`FileWorkflowPersistence` implements `WorkflowPersistence` and stores each workflow as one UTF-8 text file below the configured root directory.

- File extension: `.workflow`
- One workflow per file
- Filename is derived from `WorkflowId`
- Metadata string values are Base64 encoded using UTF-8
- Enum values are persisted by name
- Metadata attributes are sorted by key for deterministic output
- Missing records return `Optional.empty()`
- Deleting a missing record is a no-op
- I/O and malformed content are reported as `WorkflowPersistenceException`

## Serialized structure

1. Base64 encoded workflow UUID
2. workflow type
3. workflow status
4. Base64 encoded metadata name
5. Base64 encoded metadata description
6. Base64 encoded attribute key/value pairs, sorted by key

The format deliberately uses one logical value per line and Base64 for free-form strings, avoiding delimiter collisions.

## Boundaries

The implementation depends on the `WorkflowPersistence` SPI and workflow domain only. No database, framework, HTTP, or application-layer dependency is introduced.

## Verification

The test suite covers round-trip persistence, directory creation, missing records, deletion, metadata/status preservation, null arguments, UTF-8/Base64 handling, and malformed files.
