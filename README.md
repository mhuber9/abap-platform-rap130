# Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code

## Description

This repository contains a hands-on workshop for ABAP developers learning **GitHub Copilot with ADT in Visual Studio Code**.

You will build a Travel and Booking application using **ordinary ABAP Cloud classes and database tables**. An executable class displays data and save results in the **ABAP Console**. Copilot and the **ADT MCP Server** help you create objects, enhance code, and generate unit tests.

The repository and package identifiers retain `RAP130` for continuity. The exercises use no RAP runtime or Fiori UI.

**Table of Contents**

- [Requirements](#requirements)
- [Overview](#overview)
- [Exercises](#exercises)
- [Known Issues](#known-issues)
- [How to obtain support](#how-to-obtain-support)
- [Further Information](#further-information)

## Requirements

- Familiarity with ABAP classes, basic SQL, and ABAP development; the focus is the Copilot workflow.
- [Visual Studio Code](https://code.visualstudio.com/) and [SAP ADT for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=SAPSE.adt-vscode), including the ADT MCP Server.
- GitHub Copilot with access to Chat and Agent mode, signed in and able to work with the ADT virtual workspace.
- An **SAP BTP ABAP Environment**, **SAP S/4HANA Cloud Public Edition**, or suitable **SAP S/4HANA Cloud Private Edition** development system supporting the installed ADT extension and its MCP capabilities. Confirm backend/version requirements with your instructor and the linked SAP documentation.
- The [ABAP Flight Reference Scenario](https://github.com/SAP-samples/abap-platform-refscen-flight) installed with `/DMO/TRAVEL_DATA`, `/DMO/BOOKING_DATA`, and populated `/DMO/TRAVEL`, `/DMO/BOOKING`, and `/DMO/CUSTOMER` source tables accessible to workshop code.
- Authorization to create and activate participant objects, run console classes and ABAP Unit tests, and optionally debug. The ABAP SQL test double framework must be available.
- Package `#######_RAP130_AI`, supplied by the instructor or created using the Eclipse ADT fallback in Exercise 2. Application development then takes place in VS Code.

Joule predictive code completion is an optional activity in Exercise 4 and requires its own enabled capabilities. Ask the instructor to confirm any backend entitlements required for ADT agentic tools; using Copilot does not replace those system prerequisites.

> For SAP-led events, the instructors provide system access details and participant group IDs.

## Overview

<details>
  <summary>🔵 Click to expand!</summary>

The workshop preserves the journey from environment setup to a working application with validation and tests:

- Connect VS Code to your ABAP system and enable the ADT MCP Server.
- Use Copilot to inspect reference definitions and create Travel and Booking tables and classes.
- Review generated source and activate dependent objects.
- Load a small sample into participant tables and display travels with their bookings.
- Integrate customer validation before saving and inspect accepted and rejected outcomes.
- Generate isolated unit tests and inspect failures and corrections.
- Optionally debug the executable application and create a custom agent.

The executable class `YCL_TRAVEL_APP_####` calls `YCL_TRAVEL_SERVICE_####`; the service reads and saves participant data and uses `YCL_TRAVEL_HELPER_####` for customer validation. The executable caller owns commit/rollback boundaries. Sample loading preserves existing participant data, and `/DMO/` data remains read-only.

</details>

## Exercises

Complete the mandatory exercises in order. Replace `#######` in the package name with your seven-character package identifier, and `####` in class and table names with your four-digit participant suffix. Replace the longer placeholder first. [Prompt guidelines](resources/prompt-guidelines.md) provide reusable prompts and links to the complete specifications.

| Exercise | Learning focus |
|----------|----------------|
| [Getting Started](exercises/ex0/README.md) | Environment and system connection |
| [Exercise 1: Enable the ADT MCP Server](exercises/ex01/README.md) | Tool discovery and source access |
| [Exercise 2: Generate the ABAP Travel Application](exercises/ex02/README.md) | Tables, classes, review, and activation |
| [Exercise 3: Run the Travel Application](exercises/ex03/README.md) | Sample data, console execution, and helper creation |
| [Exercise 4: Add a Validation](exercises/ex04/README.md) | Validation before persistence |
| [Exercise 5: Generate ABAP Unit Tests](exercises/ex05/README.md) | Isolated tests and correction workflow |

### Optional Exercises

| Exercise | Learning focus |
|----------|----------------|
| [Exercise 6: Debug ABAP Code in Visual Studio Code](exercises/ex06/README.md) | Breakpoints, stepping, watches, and call stack |
| [Exercise 7: Create a Custom Agent](exercises/ex07/README.md) | Reusable application context and instructions |

## Known Issues

- ADT command labels and MCP tool availability can vary by installed version. Discover actual tools in Exercise 1; use the documented manual object-creation fallback when required.
- Empty Flight Reference Scenario source data prevents demo loading. Ask the instructor to provision it before proceeding.
- The rewritten workflow has been checked for documentation consistency locally. Backend activation, console execution, ABAP Unit tests, and debugger attachment still require verification on a connected workshop system.

## How to obtain support

[Create an issue](https://github.com/mhuber9/abap-platform-rap130/issues) for problems with this adapted workshop. Include the exercise, ADT/backend versions, and actual error message.

For general ABAP questions, use [SAP Community](https://community.sap.com/).

## Further Information

- [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US)
- [ADT MCP Tools](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/mcp-tools?locale=en-US)
- [ABAP Basic Features for Visual Studio Code](https://developers.sap.com/tutorials/abap-environment-adt-coretools-vscode)
- [ADT for Visual Studio Code documentation](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/abap-development-tools-for-visual-studio-code?locale=en-US)
- [VS Code custom agents](https://code.visualstudio.com/docs/agent-customization/custom-agents)

## Contributing
If you wish to contribute code, offer fixes or improvements, please send a pull request. Due to legal reasons, contributors will be asked to accept a DCO when they create the first pull request to this project. This happens in an automated fashion during the submission process. SAP uses [the standard DCO text of the Linux Foundation](https://developercertificate.org/).

## License

Copyright (c) 2026 SAP SE or an SAP affiliate company and abap-platform-rap130 contributors. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file; you may not use any file of this project except in compliance with the License.

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the LICENSE for the specific language governing permissions and limitations under the License.
