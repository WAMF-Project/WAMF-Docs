# Recent Feed

The **Recent Feed** provides a continuously updating view of recent wildlife
detections recorded by WAMF.

It provides a quick way to review the latest activity across the cameras
monitored by WAMF.

<figure>
  <img
    src="../../assets/images/dashboard-recent-feed.png"
    alt="WAMF v0.4.0 Recent Feed"
    class="wamf-screenshot"
  >
  <figcaption>Recent detections in the WAMF v0.4.0 Recent Feed</figcaption>
</figure>

## Recent Detections

Detections are displayed as cards, with the most recent activity shown first.

Each card contains:

- a snapshot of the detection
- the identified species
- classification confidence
- detection date and time

The feed combines recent detections into a visual grid, making it easy to
review wildlife activity at a glance.

## Live Updates

A **Live** indicator is displayed alongside the Recent Detections heading.

New detections can appear in the feed as WAMF processes new wildlife events,
without requiring the page to be manually refreshed.

## Classification Confidence

The confidence value shows the confidence of WAMF's species classification
for that detection.

A higher confidence value indicates a stronger classification result, but
does not guarantee that the identification is correct.

Classification can be affected by factors such as:

- poor lighting
- motion blur
- partial views of the bird
- distance from the camera
- similar-looking species
- obstructions such as branches or feeders

The minimum classification confidence accepted by WAMF is configured using
the `classification.threshold` setting in `config.yml`.

See the [Configuration Reference](../reference/configuration.md) for more
information.

## Species History

WAMF also provides species-specific pages containing information and
detection history for individual species.

These pages include detection statistics, activity by hour and the individual
detections recorded for that species.

See [Species](species.md) for more information.