# REFA API

Documentation for the REFA API: how to authenticate, how to query the endpoints, and the full description of the request payloads.

## Overview

The REFA API calculates feasible transmission line upgrade projects (Rebuild, Reconductoring, Voltage Upgrade, HVDC) and their lifecycle costs, given the characteristics of a line project and a set of candidate conductors.

The main endpoint, `/calculate-feasible-projects`, is queried with the `payload_calculate-feasible-projects.json` file, whose structure and fields are described in [Payload Structure](payload.md).

The API accepts payloads in **either metric or imperial units**. See [Unit Systems](unit-systems.md) for details.


