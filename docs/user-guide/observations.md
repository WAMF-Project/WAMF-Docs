# Observations

An observation is a wildlife detection that has been received, classified and stored by WAMF.

Each observation may include a snapshot, video clip, species classification and information about when and where the detection occurred.

<!-- Screenshot: Observations page -->

*Observations page screenshot to be added.*

## Observation Information

An observation may contain:

| Field | Description |
|---|---|
| Species | The common and scientific name assigned by the classifier. |
| Confidence | The classifier's confidence in the species identification. |
| Camera | The camera that produced the detection. |
| Detected at | The date and time of the observation. |
| Snapshot | A still image associated with the detection. |
| Clip | A short video recording associated with the detection. |
| Source event | Information about the original detection event. |

The information available depends on the media and event data supplied by the configured detection system.

## Browsing Observations

The Observations page displays recorded detections in chronological order, with the newest observations shown first.

Observation cards provide a compact summary of each detection.

<!-- Screenshot: Observation cards -->

*Observation cards screenshot to be added.*

Select an observation to view its full details.

## Observation Details

The observation details page provides a closer look at an individual detection.

It may include:

- a larger snapshot
- video playback
- common species name
- scientific species name
- classification confidence
- camera name
- detection timestamp
- links to related species information

<!-- Screenshot: Observation details -->

*Observation details screenshot to be added.*

## Filtering Observations

Depending on the WAMF version, observations may be filtered by criteria such as:

- species
- camera
- date range
- classification confidence
- media availability

Filters make it easier to locate a particular detection or review activity over a specific period.

<!-- Screenshot: Observation filters -->

*Observation filtering screenshot to be added.*

## Classification Confidence

The confidence value indicates how certain the classifier was when assigning the species.

A higher confidence value generally indicates a stronger classification result, but it does not guarantee that the identification is correct.

Factors that may affect classification include:

- poor lighting
- motion blur
- partial views of the animal
- distance from the camera
- similar-looking species
- obstructions such as branches or feeders

The minimum confidence required for an observation is configured in `config.yml`.

## Observation Media

WAMF can store snapshots and video clips associated with an observation.

Media availability depends on:

- the Frigate configuration
- the original detection event
- the WAMF media configuration
- the configured retention policy

Older media may be removed automatically while the observation record remains in the database.

## Incorrect Classifications

Wildlife classification is not always perfect.

When reviewing an unusual or low-confidence observation, compare:

- the snapshot
- the video clip
- the reported species
- similar observations from the same camera

Manual correction and review features may depend on the installed WAMF version.

## Missing Observations

When an expected observation does not appear, check that:

- Frigate detected the configured object type
- MQTT events are reaching WAMF
- the camera name matches the WAMF configuration
- the classification confidence exceeded the configured threshold
- WAMF can retrieve media from the Frigate API
- sufficient storage is available