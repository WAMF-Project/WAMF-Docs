# Species Metadata

WAMF maintains cached metadata for species that have been identified in
observations.

The **Species Metadata** administration page shows the current state of this
metadata and provides tools for refreshing it.

<figure>
  <img
    src="../../assets/images/admin-species-metadata.png"
    alt="WAMF v0.4.0 Species Metadata administration page"
    class="wamf-screenshot"
  >
  <figcaption>WAMF v0.4.0 Species Metadata administration page</figcaption>
</figure>

## Metadata Status

The summary at the top of the page shows:

- **Total Species Cached** — the number of species currently held in the
  metadata cache.
- **Missing Descriptions** — species for which a description is not currently
  available.
- **Missing Thumbnails** — species for which a thumbnail is not currently
  available.
- **Last Metadata Refresh** — the date and time of the most recent metadata
  refresh.

## Species List

Each cached species is shown with its:

- Common name
- Scientific name
- Description status
- Thumbnail status
- Last updated time

The **Refresh** action updates the metadata for an individual species.

## Refreshing Metadata

Two additional refresh options are available at the top of the page:

### Refresh Missing Metadata

**Refresh Missing Metadata** refreshes metadata that is currently missing.

This is useful when one or more species are missing a description or
thumbnail without unnecessarily refreshing the complete cache.

### Refresh All Metadata

**Refresh All Metadata** refreshes the metadata for all cached species.

Use this when you want WAMF to update the complete species metadata cache.