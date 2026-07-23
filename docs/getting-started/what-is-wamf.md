# What is WAMF?

The Wildlife Activity Monitoring Framework (WAMF) is an open-source platform for monitoring, identifying, recording and analysing wildlife activity using cameras and artificial intelligence.

WAMF is designed for hobbyists, wildlife enthusiasts, researchers and conservation projects that want to collect structured wildlife observations from cameras without manually reviewing large volumes of footage.

## Overview

WAMF sits between a wildlife camera system and the user.

It receives wildlife detections from supported detection systems, classifies species, stores observations and presents the results through a web-based interface.

The platform is designed to automate the process of turning camera detections into useful wildlife records.

## How WAMF Works

A typical workflow looks like this:

1. A camera captures wildlife activity.
2. A detection system identifies an object of interest.
3. WAMF receives the detection event.
4. WAMF classifies the species using AI models.
5. The observation is stored in the database.
6. Associated media is retained according to configured policies.
7. Results become available through the dashboard and API.

This allows users to focus on wildlife observations rather than manually processing footage.

## Key Features

### Species Classification

WAMF can identify wildlife species from captured images and store both common and scientific names.

### Observation Database

All observations are stored in a searchable database, allowing long-term monitoring of wildlife activity.

### Species Metadata

Additional species information can be automatically retrieved and displayed to provide context for observations.

### Media Management

Snapshots and clips can be retained according to configurable policies, helping balance storage usage against historical record keeping.

### Web Dashboard

The built-in dashboard provides access to observations, species information, system status and administrative tools.

### REST API

WAMF exposes APIs that allow other applications and services to integrate with wildlife observations and system data.

## Design Goals

WAMF is built around several core principles:

- Open source and self-hosted
- Wildlife-focused rather than general surveillance
- Modular and extensible architecture
- Resource efficient deployment
- Support for a wide range of hardware
- Long-term observation and data collection

## Current Status

WAMF is currently in active development.

The current focus is the v0.4 platform stabilisation release, which includes deployment validation, documentation improvements and long-term testing.

Future development will focus on abstraction layers, distributed edge devices and support for additional capture and detection systems.

## Typical Use Cases

WAMF can be used for:

- Garden bird monitoring
- Wildlife feeding stations
- Nest box observation
- Camera trap projects
- Educational wildlife monitoring
- Citizen science projects
- Small-scale conservation initiatives

## Learn More

To get started with WAMF, continue to the Installation guide.
