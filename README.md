# Tealium Collect for sGTM

Tealium Collect for sGTM is a Google Tag Manager server-side tag template that sends event data from a GTM Server container to Tealium Collect.

This template is intended for teams that want to route server-side event traffic through GTM Server and forward normalized payloads into Tealium Customer Data Hub.

## What this template does

* Sends server-side GTM event data to the Tealium Collect `/event` endpoint
* Supports a custom HTTPS collection endpoint override
* Resolves the event name from common fields such as `tealium_event`, `event_name`, `event`, and `name`
* Optionally ignores GTM internal events
* Supports custom attribute additions and overrides
* Optionally flattens nested payloads into dot-notation keys
* Supports include and exclude filtering for outbound payload fields
* Supports a custom event data object override
* Provides configurable request timeout and logging behavior

## Requirements

* A Google Tag Manager Server container
* A Tealium account and profile
* Optional Tealium data source key
* A server-side event source feeding requests into the GTM Server container

## Installation

This template is intended to be installed from the Google Tag Manager Community Template Gallery.

1. Open your GTM Server container.
2. Create a new tag.
3. In the tag type picker, open the Community Template Gallery.
4. Search for `Tealium Collect for sGTM`.
5. Add the template to your container.
6. Select the template as your tag type.
7. Configure the tag fields for your Tealium environment.
8. Add an appropriate trigger and save the tag.

## Alternative installation for development

If you are testing or contributing to this repository directly, you can also import the `template.tpl` file manually through the Templates area in a GTM Server container.

## Template fields

### Required

* Tealium Account
* Tealium Profile

### Optional

* Tealium Data Source Key
* Fallback Event Name
* Ignore GTM Internal Events
* Additional Ignored Events
* Use Custom Event Data Object
* Custom Event Data Object
* Data Collection Endpoint
* Request Timeout (ms)
* Custom Data Attributes or Attribute Override
* Flatten Nested Objects
* Flatten Depth
* Include Keys
* Exclude Keys
* Logging Behavior

## Field behavior

### Event name resolution

The template resolves the outbound event name in this order:

1. `tealium_event`
2. `event_name`
3. `event`
4. `name`
5. configured fallback event name
6. `server_event`

### Custom event data object

If enabled, the template uses the configured variable or expression result instead of the default server event data.

### Flattening

When enabled, nested objects and arrays are flattened into dot-notation keys before the payload is sent.

Example:

```json
{
  "page": {
    "name": "home"
  },
  "client_hints": {
    "architecture": "arm"
  }
}
```

becomes:

```json
{
  "page.name": "home",
  "client_hints.architecture": "arm"
}
```

### Include and exclude filtering

The template supports both allowlist and blocklist style filtering.

* Include Keys keeps only matching keys after flattening
* Exclude Keys is applied twice:
  * before flattening for top-level object exclusion such as `client_hints`
  * after flattening for exact flattened-key exclusion such as `client_hints.architecture`

## Recommended setup

A typical configuration includes:

* Tealium Account set to your account name
* Tealium Profile set to your target profile
* Tealium Data Source Key set when you want events attributed to a specific data source
* Ignore GTM Internal Events enabled
* Flatten Nested Objects enabled
* Logging Behavior set to Preview and Debug only during validation

## Example use cases

### Send all eligible server events to Tealium Collect

Use the template with no include filters and only minimal exclusions.

### Exclude an entire object

Add `client_hints` to Exclude Keys to remove the full top-level object before flattening.

### Exclude a single flattened field

Add `page.name` to Exclude Keys to remove only that flattened leaf key.

### Override payload fields

Use Custom Data Attributes or Attribute Override to add fields such as:

* `tealium_event`
* `env`
* `source_system`

## Testing

Recommended validation flow:

1. Import the template into a test GTM Server container.
2. Create a test tag from the template.
3. Configure Tealium Account and Tealium Profile.
4. Enable Preview in GTM Server.
5. Send test events into the server container.
6. Verify the outbound request and payload in Preview logs.
7. Confirm the request is accepted by the Tealium endpoint.

## Logging

Logging behavior can be configured as:

* Preview and Debug only
* Always log
* Never log

For production use, Preview and Debug only is recommended unless active troubleshooting is required.

## Repository contents

* `template.tpl` — GTM Server tag template import file
* `README.md` — setup and usage documentation
* `LICENSE` — repository license
* `metadata.yaml` — Community Template Gallery metadata

## Community Template Gallery

If this template is published to the Google Tag Manager Community Template Gallery, update `metadata.yaml` with the correct repository URLs and release commit SHA values before submission.

## Support

This repository provides the template as-is for use with Tealium Collect and GTM Server. Validate all behavior in a non-production environment before rolling out to production traffic.

## License

See `LICENSE` in this repository.