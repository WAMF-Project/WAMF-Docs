# Species

WAMF provides a species page for wildlife identified in recorded detections.

The species page combines information about the species with detection
statistics, activity and the individual detections recorded by WAMF.

<figure>
  <img
    src="../../assets/images/species-details.png"
    alt="WAMF v0.4.0 Species Details"
    class="wamf-screenshot"
  >
  <figcaption>Species information and detection history in WAMF v0.4.0</figcaption>
</figure>

## Species Information

The top of the species page identifies the species and displays available
reference information.

This can include:

- common name
- scientific name
- a detection snapshot
- species description
- reference image
- link to external reference information

Species metadata supplements WAMF's own detection data and helps provide
context when reviewing an identified species.

## Species Metadata

WAMF can retrieve additional information about identified species, including
a description and reference image.

Metadata requires Internet access when it is retrieved, but WAMF can continue
processing and recording detections if additional species information is
unavailable.

Species metadata can be reviewed and refreshed from
**Admin → Species**.

## Detection Statistics

The species page provides statistics for the selected date, including:

- number of detections
- first detection
- last detection
- peak activity hour

These provide a quick summary of when the species was active during the
selected day.

## Activity by Hour

The **Activity By Hour** chart shows how detections of the species were
distributed throughout the selected day.

This makes it easier to see periods when the species was most active.

## Detection History

Individual detections for the species are displayed below the activity
summary.

Each detection card can include:

- snapshot
- common species name
- classification confidence
- detection time
- camera

This provides a visual history of the detections WAMF recorded for that
species.

## Common and Scientific Names

WAMF records both common and scientific species names where available.

For example:

| Common name | Scientific name |
|---|---|
| Eurasian Blue Tit | *Cyanistes caeruleus* |
| European Robin | *Erithacus rubecula* |
| Great Tit | *Parus major* |

Scientific names provide a consistent identifier where common names may vary
between countries or regions.

## Missing Species Information

A species may appear without additional metadata when the information has not
yet been retrieved or a metadata lookup was unsuccessful.

The WAMF detection data remains available even when additional species
metadata is missing.

Missing metadata can be reviewed and refreshed from
**Admin → Species**.

## Classification Accuracy

Species pages are based on WAMF classification results.

A higher confidence value indicates a stronger classification result, but
does not guarantee that the identification is correct.

When an identification appears unusual, use the original detection snapshot
and the species reference information to help compare the result.