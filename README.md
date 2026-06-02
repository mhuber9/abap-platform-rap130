[Home - Workshops about the ABAP RESTful Application Programming Model (RAP)](https://github.com/SAP-samples/abap-platform-rap-workshops/blob/main/README.md)

[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/abap-platform-rap130)](https://api.reuse.software/info/github.com/SAP-samples/abap-platform-rap130)

# RAP130 - Build SAP Fiori Apps with ABAP Cloud and SAP Joule for Developers in Visual Studio Code

## Description

This repository contains the material for the hands-on session **RAP130 - Build SAP Fiori Apps with ABAP Cloud and SAP Joule for Developers in Visual Studio Code** 💎

You will build a transactional SAP Fiori elements app for travel management using the **ABAP RESTful Application Programming Model (RAP)** — entirely from **Visual Studio Code**, powered by the **ADT MCP Server** to generate and enhance your application.

**Table of Content**
- [Requirements](#requirements)
- [Overview](#overview)
- [Exercises](#exercises)
- [Known Issues](#known-issues)
- [How to obtain support](#how-to-obtain-support)
- [Further Information](#further-information)

## Requirements

> To complete the practical exercises, you need:
> - **Visual Studio Code** — Download from [https://code.visualstudio.com/](https://code.visualstudio.com/)
> - **ADT for Visual Studio Code** — ADT for Visual Studio Code includes the built-in ADT MCP Server. Here is the [link](https://marketplace.visualstudio.com/items?itemName=SAPSE.adt-vscode) to download the extension. 
> - **IMPORTANT!** ℹ️ 
  A **coding agent** extension in Visual Studio Code that supports Visual Studio Code's virtual workspace filesystem — required to call MCP tools and read/edit ABAP files. 
  **GitHub Copilot** is the primary tested agent and the one used and recommended for this tutorial.
> - Access to an **SAP BTP ABAP Environment** or **SAP S/4HANA Cloud Public Edition** or **SAP S/4HANA Cloud Private Edition** system that has the [ABAP Flight Reference Scenario](https://github.com/SAP-samples/abap-platform-refscen-flight) imported and SAP Joule for developers, ABAP AI capabilities enabled.
>
> (*) SAP BTP ABAP environment, SAP S/4HANA Cloud Public Edition, SAP S/4HANA Cloud Private 2025 Edition are currently supported.
>
>> #### ⚠ Exception regarding SAP-led events, such as "ABAP Developer Day" and "SAP CodeJam"
>> → A dedicated ABAP system for the hands-on workshop participants will be provided.
>> → Access to the system details for the workshop will be provided by the SAP instructors during the session.


## Overview

> In this hands-on session, you will learn how to use the **ADT MCP Server** to build a transactional SAP Fiori elements app entirely from Visual Studio Code — using AI-powered generation, code enhancement, and unit test support — powered by ABAP Cloud and the ABAP RESTful Application Programming Model (RAP).

<details>
  <summary>🔵 Click to expand!</summary>

  This hands-on workshop covers the full developer journey from Visual Studio Code environment setup to a fully functional, AI-enhanced transactional app:

  - Set up Visual Studio Code with the ADT for Visual Studio Code extension and connect to your ABAP Cloud system
  - Configure the built-in **ADT MCP Server** and connect it to **your coding agent**
  - Use ADT MCP tools to **generate a complete RAP business object** (Travel + Booking) and its OData UI service — all via natural language prompts
  - Explore and adjust the generated artifacts (CDS views, behavior definitions, metadata extensions) directly in Visual Studio Code
  - Publish the service binding and preview the **SAP Fiori elements app** in the browser
  - Add backend validations
  - Run ABAP unit tests from Visual Studio Code
  - Debug the SAP Fiori App from Visual Studio Code


</details>

## Exercises

Follow these steps to build a SAP Fiori App using the ADT MCP Server and a coding agent (GitHub Copilot or compatible) in Visual Studio Code.

| Exercises | -- |
| ------------- | -- |
| [Getting Started](exercises/ex0/README.md)| -- | 
| [Exercise 1: Enable the ADT MCP Server](exercises/ex01/README.md) | -- | 
| [Exercise 2: Generate the SAP Fiori App via MCP Tools](exercises/ex02/README.md)| -- |
| [Exercise 3: Publish and Preview the Travel App](exercises/ex03/README.md)| -- |
| [Exercise 4: Add a Validation](exercises/ex04/README.md)| -- |
| [Exercise 5: Generate ABAP Unit Tests](exercises/ex05/README.md) | -- | 

#### Optional Exercises

| Exercises | -- |
| ------------- | -- | 
| [Exercise 6: Debug ABAP Code in Visual Studio Code](exercises/ex06/README.md) | -- | 
| [Exercise 7: Create a Custom Agent](exercises/ex07/README.md) | -- | 


## Known Issues
<!-- You may simply state "No known issues. -->

## How to obtain support
[Create an issue](https://github.com/SAP-samples/abap-platform-rap130/issues) in this repository if you find a bug or have questions about the content.
 
For additional support, [ask a question in SAP Community](https://answers.sap.com/questions/ask.html).

## Further Information

You can find more information about ABAP AI, ABAP Cloud, RAP, and ADT for Visual Studio Code here:

- [AI in ABAP Cloud](https://help.sap.com/docs/abap-ai/generative-ai-in-abap-cloud/generative-ai-in-abap-cloud?locale=en-US)
- [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US)
- [ABAP Cloud Roadmap Information - GenAI](https://help.sap.com/docs/abap-cross-product/roadmap-info/genai?locale=en-US)
- [ABAP Development Tools for Visual Studio Code - Official Documentation](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/abap-development-tools-for-visual-studio-code?locale=en-US)
- [ABAP Development Tools for Visual Studio Code: Everything You Need to Know](https://community.sap.com/t5/technology-blog-posts-by-sap/abap-development-tools-for-vs-code-everything-you-need-to-know/ba-p/14258129)
- [Behind the Design: How We Transformed the ABAP Development Tools Architecture to Support More IDEs](https://community.sap.com/t5/technology-blog-posts-by-sap/behind-the-design-how-we-transformed-the-abap-development-tools/ba-p/14258121)
- [Getting Started with ABAP RESTful Application Programming Model (RAP)](https://pages.community.sap.com/topics/abap/rap)


## Contributing
If you wish to contribute code, offer fixes or improvements, please send a pull request. Due to legal reasons, contributors will be asked to accept a DCO when they create the first pull request to this project. This happens in an automated fashion during the submission process. SAP uses [the standard DCO text of the Linux Foundation](https://developercertificate.org/).

## License

Copyright (c) 2026 SAP SE or an SAP affiliate company and abap-platform-rap130 contributors. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file; you may not use any file of this project except in compliance with the License.

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the LICENSE for the specific language governing permissions and limitations under the License.
