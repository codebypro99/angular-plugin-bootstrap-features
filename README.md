# Angular Dynamic Plugin Bootstrap Feature Sample

A TypeScript bootstrap excerpt that retrieves plugin registrations from a server, transforms them into an Angular Native Federation manifest, and initializes federation before importing the application bootstrap.

**Source:** [main.ts](./features/main.ts)

## Purpose

This feature shows how I coordinate runtime plugin discovery with application startup. Server-provided registrations determine the remote-entry map, while a fallback path attempts to start the main application without plugins when initialization fails.

## Manifest Transformation

- Fetches plugin registrations from the configured public API endpoint.
- Describes plugin metadata with a TypeScript type.
- Excludes disabled plugins and entries without either a production or development base URL.
- Selects a base URL according to the environment's development flag.
- Maps plugin identifiers to remoteEntry.json URLs.
- Stores plugin metadata, a route path, and the selected base URL in a browser-global registry.

**What this demonstrates:** translating an application-specific plugin contract into a federation manifest while retaining metadata for the host interface.

## Startup Coordination

- Passes the transformed manifest to initFederation.
- Reads the returned scope keys to filter the browser-global plugin registry.
- Uses a base-URL prefix comparison in development and an expected plugin path comparison in production.
- Imports the application bootstrap after federation initialization and registry filtering.

**What this demonstrates:** sequencing runtime dependency initialization before application startup.

## Fallback Path

- Logs errors from the startup promise chain.
- Clears the dynamic plugin registry.
- Attempts federation initialization with an empty manifest.
- Imports the application bootstrap after that fallback initialization.
- Logs a second error if the fallback chain fails.

**What this demonstrates:** expressing a recovery path that attempts to keep the host application available when plugin startup fails.

## Technologies

TypeScript, Fetch, Promise chaining, dynamic imports, browser-global metadata, environment configuration, and Angular Native Federation.

## Excerpt Context and Behavior Boundaries

The API implementation, environment values, response wrapper, application bootstrap, remote modules, route integration, and dependency versions belong to the surrounding application. This runtime bootstrap is documented separately from the host and plugin build configurations in the federation sample collection.

The code does not check response.ok or validate the response at runtime beyond the presence of data. Plugin eligibility accepts either base URL, so the selected environment's URL may still be empty. A missing data property returns an empty manifest before initializing the global registry; subsequent filtering can fail and enter the fallback path.

The main catch also covers errors from the application bootstrap import, rather than only federation errors. Registry filtering relies on the shape of the federation initialization result. Permission-related fields are metadata here; this file does not enforce authorization.

Original source code and imports are preserved. The description is based on source inspection. The excerpt has not been compiled or executed, and SDK-version compatibility has not been verified for this collection.

## Portfolio Message

I can structure plugin-aware application startup around runtime discovery, manifest transformation, initialization order, and an explicit fallback path.
