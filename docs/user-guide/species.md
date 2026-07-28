# Species

The Species section groups observations by their identified species.

It provides an overview of the wildlife recorded by WAMF and makes it easier to explore the history of an individual species.

![Species page](../images/species-overview-placeholder.png)

*Species page screenshot to be added.*

## Species List

The species list displays the species identified in recorded observations.

Each species entry may include:

- common name
- scientific name
- thumbnail image
- number of observations
- date of the most recent observation

![Species list](../images/species-list-placeholder.png)

*Species list screenshot to be added.*

Select a species to open its species details page.

## Species Details

The species details page combines species information with observations recorded by WAMF.

It may contain:

- common name
- scientific name
- species image
- description
- observation count
- first recorded observation
- most recent observation
- recent snapshots and clips

![Species details](../images/species-details-placeholder.png)

*Species details screenshot to be added.*

## Species Metadata

WAMF can retrieve additional information about identified species.

Metadata may include:

- a short description
- a representative image
- scientific classification
- links to external reference material

Metadata availability depends on the configured provider and Internet access.

WAMF can continue recording observations when metadata is unavailable.

## Observation History

Each species page includes observations associated with that species.

This makes it possible to review:

- how frequently the species appears
- which cameras recorded it
- when it was most recently detected
- changes in activity over time

![Species observation history](../images/species-observations-placeholder.png)

*Species observation history screenshot to be added.*

## Common and Scientific Names

WAMF stores both common and scientific species names where available.

For example:

| Common name | Scientific name |
|---|---|
| Blue Tit | *Cyanistes caeruleus* |
| European Robin | *Erithacus rubecula* |
| Great Tit | *Parus major* |

Scientific names provide a consistent identifier where common names differ between countries or regions.

## Missing Species Information

A species may appear without a description or thumbnail when:

- Internet access is unavailable
- the metadata provider cannot find a match
- the species name is incomplete
- the metadata request failed
- metadata has not yet been refreshed

The observation itself remains available even when the additional species metadata is missing.

## Classification Accuracy

Species pages are created from classifier results.

An incorrect classification may therefore place an observation under the wrong species.

Review the associated snapshot or video when:

- the confidence value is low
- the species is unusual for the location
- two species have similar markings
- the subject is partly hidden or blurred