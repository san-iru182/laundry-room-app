# Laundry Room App

A web application designed to help students quickly check washer and dryer availability in a shared laundry room.

The goal of the project is to make laundry room usage more convenient by showing machine availability, estimated completion times, and eventually usage trends and notifications.

## Project Status

Currently in the planning and design phase.

Current work includes:

* Defining application requirements
* Designing the database schema
* Designing the system architecture
* Planning machine state transitions
* Exploring how machine status can initially be tracked manually and later automated

## Core Idea

The application will display a visual representation of a laundry room so users can quickly see the status of each washer and dryer.

Example machine states:

* `AVAILABLE`
* `IN_USE`
* `DONE`
* `OUT_OF_SERVICE`
* `UNKNOWN`

Users should eventually be able to see information such as:

* Which machines are available
* Which machines are currently running
* Estimated time remaining
* Which machines have completed their cycle
* Typical busy hours for the laundry room

## Planned Features

### Initial Version

* View washer and dryer availability
* Visual laundry room layout
* Color-coded machine status
* Start a laundry session manually
* Estimate cycle completion time
* Automatically update machine state when a cycle finishes

### Future Features

* Notifications when a user's machine finishes
* Notifications when any machine becomes available
* Usage history
* Peak-hour / "hot hours" analytics
* Multiple laundry room support
* User accounts
* Real-time machine status updates
* Automatic machine detection through sensors or machine APIs

## Proposed Architecture

The application is currently planned as a full-stack system with:

### Frontend

Responsible for:

* Displaying the laundry room layout
* Showing machine availability
* Showing remaining cycle time
* Allowing users to interact with machines
* Receiving live status updates

### Backend

Planned using Spring Boot.

Responsibilities include:

* Managing laundry rooms
* Managing machine state
* Creating and tracking laundry sessions
* Calculating completion times
* Providing REST APIs
* Sending real-time updates
* Supporting notifications and analytics

### Database

A relational database such as PostgreSQL will store application state and historical usage data.

Proposed core entities:

```text
LaundryRoom
    |
    | 1
    |
    | many
Machine
    |
    | 1
    |
    | many
LaundrySession
```

Additional entities may eventually include:

```text
User
NotificationRequest
```

## Proposed Data Model

### LaundryRoom

```text
id
name
building
floor
```

### Machine

```text
id
roomId
machineNumber
type
status
positionX
positionY
```

Machine types:

```text
WASHER
DRYER
```

### LaundrySession

```text
id
machineId
startedAt
expectedEndAt
completedAt
cycleType
source
```

A laundry session represents one use of a washer or dryer.

Keeping sessions separate from machines allows the application to retain historical usage data that can later be used to determine peak laundry hours.

## Machine State Flow

A machine will generally move through the following states:

```text
AVAILABLE
    |
    | Start cycle
    v
IN_USE
    |
    | Cycle finishes
    v
DONE
    |
    | Clothes removed
    v
AVAILABLE
```

Machines may also enter:

```text
OUT_OF_SERVICE
```

when they are unavailable due to maintenance or failure.

## Manual First, Automatic Later

The first version of the application will likely rely on users manually indicating that a machine has started.

For example:

```text
User starts Washer 3
        |
        v
Backend creates LaundrySession
        |
        v
Machine becomes IN_USE
        |
        v
Expected completion time calculated
```

The application will be designed so that this input source can eventually be replaced by automatic machine detection.

Future flow:

```text
Washer / Dryer
      |
      v
Sensor or Smart Machine API
      |
      v
Backend
      |
      v
LaundrySession
```

This allows the rest of the application to remain mostly unchanged when hardware integration is introduced.

## Usage Analytics

Historical laundry sessions can eventually be used to calculate:

* Most popular laundry hours
* Most popular laundry days
* Average machine utilization
* Typical wait times
* Recommended times to use the laundry room

For example:

```text
Monday

8 AM      ██
12 PM     █████
4 PM      ███████
6 PM      ██████████
8 PM      ████████
```

The application could eventually tell users:

> The laundry room is usually busiest between 6 PM and 9 PM.

## Development Roadmap

### Phase 1 — Planning

* Define functional requirements
* Design database schema
* Design machine state model
* Design component architecture

### Phase 2 — Frontend Prototype

* Create laundry room layout
* Display mocked machine data
* Create machine status components

### Phase 3 — Backend

* Create Spring Boot application
* Implement database entities
* Create REST APIs
* Implement machine and laundry session logic

### Phase 4 — Live Status

* Automatically update cycle timers
* Add real-time frontend updates

### Phase 5 — Notifications

* Add user/device notification subscriptions
* Notify users when cycles finish
* Notify users when machines become available

### Phase 6 — Analytics

* Track historical machine usage
* Calculate peak usage periods
* Display hot-hour recommendations

### Phase 7 — Automation

* Explore IoT sensors
* Explore smart machine APIs
* Automatically detect machine start and completion events

## Key Design Questions

Several decisions are still being explored:

* How should the application determine when a machine starts?
* How should cycle duration be determined?
* How should the application know when clothes have actually been removed?
* Should users need accounts?
* How should inaccurate manual status updates be handled?
* What hardware or APIs could eventually provide automatic machine status?

## Tech Stack

Planned technologies:

```text
Backend:   Java / Spring Boot
Database:  PostgreSQL
Frontend:  TBD
API:       REST
Real-time: WebSocket or Server-Sent Events
```

Additional technologies may be added as the project develops.

## Project Goal

The larger goal of this project is to build a system that begins as a simple software solution for checking laundry availability but is designed to eventually support real-time machine monitoring, notifications, analytics, and automatic hardware integration.
