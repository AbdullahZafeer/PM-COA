# PM-COA

SAP S/4HANA Cloud PMCOA (Certificate of Analysis) App
A comprehensive RAP-based Fiori Elements application designed to generate, format, and print Certificates of Analysis (CoA) for packaging materials. This solution handles complex QA inspection data, applies dynamic rounding logic, and routes output to specific Adobe Form layouts based on runtime parameters.

🏗️ Architecture Overview
The application is built using the ABAP RESTful Application Programming Model (RAP) and SAP Fiori Elements (OData V4).

1. Core Data Services (CDS)
Header Views: Extracts core inspection lot data, manufacturing orders, batches, and dynamic classification values (Procedure No, Quality Standard).

Characteristic & Result Views (ZQM_PMCOA_RText): Implements a custom "Hybrid Result Logic" that intelligently switches between user-entered raw strings (InspectionResultOriginalValue) and dynamically rounded system calculations (InspectionResultMeanValue) to perfectly mirror the QA32 Fiori screen.

Abstract Entity (ZQM_PMCOA_A_COA_PARAM): Defines the structure for the Fiori action popup parameter dialog, capturing user inputs such as Remarks and CompanyCode.

2. ABAP Behavior & Printing Logic
RAP Behavior Implementation: The unmanaged action GenerateCoA handles user authorization, reads the parameter inputs, and delegates processing to the PDF rendering class.

Print Class (ZCL_QM_PMCOA_COA_PDF):

Instantiates the Form Data Provider (FDP) API.

Executes an XSLT transformation (ZQM_SORT_COA_CHAR) to correctly sequence the XML payload.

Injects escaped string inputs (like user remarks) directly into the XML tree.

Dynamically routes the layout payload to either ZQM_PMCOA_COA_FORM (Noventa Pharma) or ZQM_PMCOA_COA_FORM_S (Searle) based on the dropdown selection.

3. Adobe LiveCycle Forms
Features JavaScript-based client-side initialization scripts to parse formatted numerical strings, ensuring trailing zeros are correctly managed based on backend master data constraints.

💻 Fiori Elements Frontend (BAS Guided Development)
To trigger the RAP action and download the rendered PDF, a custom action button was added to the standard Fiori Elements List Report/Object Page using SAP Business Application Studio (BAS) Guided Development.

Controller Extension (GenerateCoA.js)
The UI5 controller extension invokes the bound RAP action, passes the popup parameters, and handles the base64/xstring response to download the PDF directly to the user's local machine.
