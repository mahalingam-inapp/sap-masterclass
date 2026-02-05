# SAP Masterclass — Course Plan

**Version:** 1.2 (Draft for review)  
**Target audience:** Beginners and intermediate users  
**Scope:** All major SAP modules and cloud products, one chapter per module  

---

## Course overview

- **Title:** SAP Masterclass — From Navigation to Modules & Cloud Products  
- **Description:** A structured course that introduces SAP ERP, S/4HANA, and major SAP cloud products (SuccessFactors, Ariba, Concur, CX, IBP, SAC, BTP, Signavio, and more). Each chapter covers one module or product with its purpose, features, key interactions, important tables (where applicable), and practical context. Includes quizzes to reinforce learning.
- **Prerequisites:** None for early chapters; later technical chapters assume basic familiarity from earlier modules.
- **Format:** Welcome page + 31 modules (Foundation concepts + 30 chapter modules). Each chapter includes purpose, features, data plane/control plane and deployment/integration (where applicable), interactions, tables/product concepts, and a short quiz.

---

## Learning outcomes (course-level)

By the end of the course, learners will:

- Navigate SAP systems (on-premise and cloud) confidently and understand common UI patterns.
- Explain the purpose and scope of each major SAP module and key SAP cloud products (e.g. SuccessFactors, Ariba, Concur, CX).
- Describe how key modules and products interact (e.g. FI with MM/SD/CO; S/4HANA with SuccessFactors, Ariba).
- Recognize important transaction codes and table areas (ERP) and core concepts/features (cloud products).
- Understand SAP **data plane** vs **control plane** across ERP and cloud products, and how **deployment** and **integration** options apply.
- Have a foundation to pursue role-specific or advanced training in ERP or cloud solutions.

---

## Data plane, control plane, deployment & integration

This section defines concepts that apply across all SAP modules and products. Each chapter may reference these where relevant.

### Data plane

The **data plane** is where business data is stored, processed, and transacted. It holds the operational and analytical data that applications read and write.

- **In SAP ERP / S/4HANA (on‑premise or hosted):**
  - **Storage:** SAP HANA (or legacy DB) holds transparent tables, cluster tables, and application data. Key areas: FI (BKPF, BSEG, ACDOCA), CO (COEP, COBK), MM (MARA, EKKO, MSEG), SD (VBAK, VBAP, LIKP, VBRK), PP (AFPO, RESB), etc.
  - **Processing:** Application servers execute ABAP (and optionally Java) and run business logic; the database executes SQL and stored procedures. Document flows, validations, and postings all run in the data plane.
  - **Per module:** Each functional area (FI, CO, MM, SD, PP, QM, PM, PS, HCM, WM/EWM, etc.) has its own set of tables and programs; the data plane is logically partitioned by module but physically in one (or more) database(s).

- **In SAP cloud products (SuccessFactors, Ariba, Concur, CX, IBP, SAC, etc.):**
  - **Storage:** Data lives in SAP‑operated or partner data centers. There are no customer-accessible database tables; data is represented as **entities** (e.g. User, Employee, Purchase Order, Expense Report) and accessed via **APIs** (OData, REST) and UIs.
  - **Processing:** Business logic runs in the cloud application layer; replication or integration may push/pull data to or from S/4HANA, Datasphere, or other systems.

- **In SAP Datasphere / BW:** The data plane is the **semantic layer** and stored datasets (views, persisted data). It consolidates data from ERP and cloud sources for analytics (SAC, BI tools).

- **In SAP BTP:** BTP itself is primarily a **control plane** (see below). Extensions and integrations may persist data in BTP (e.g. Cloud Foundry apps, ABAP environment) or only orchestrate access to data in S/4HANA or cloud products.

**Takeaway:** For any module or product, ask: *Where is the business data stored, and where is it processed?* That is the data plane for that solution.

### Control plane

The **control plane** is where system behavior is **configured, orchestrated, and governed**—not where day‑to‑day business transactions are stored. It manages deployment, identity, integration routing, and operations.

- **In SAP ERP / S/4HANA:**
  - **Basis / system administration:** Client, instance, application server profile; transport (STMS), patches, upgrades. Defines *which* systems exist and how they are maintained.
  - **Configuration (customizing):** SPRO/IMG and related tables (e.g. T001, T024) define company codes, plants, document types, account determination—i.e. *how* the data plane behaves.
  - **Security:** Users, roles, authorizations (SU01, PFCG) control *who* can access which data and transactions.
  - **Integration:** PI/PO or CPI (when used) routes messages; RFC destinations (SM59); OData/Gateway configuration. These control *how* data flows between systems.

- **In SAP BTP:**
  - **Global account and subaccounts:** Entitlements, quotas, regions. Defines what services (Integration Suite, Extension Suite, Datasphere, etc.) are available and where they run.
  - **Identity (IAS):** Central identity and authentication for SAP and non‑SAP apps; single sign‑on and federation.
  - **Integration Suite:** CPI flows, API Management policies, Event Mesh routing—control *how* systems and events are connected.
  - **Multi‑region / high availability (e.g. Multi‑Region Manager):** Orchestration of replication, failover, and load balancing across regions (control plane for resilience).

- **In SAP cloud products:**
  - **Admin UIs and provisioning:** SuccessFactors Admin Center, Ariba administration, Concur Admin, etc. Control tenant settings, user provisioning, and integrations.
  - **Identity and SSO:** Often federated with IAS or corporate IdP—control *who* can access the product.

**Takeaway:** For any module or product, ask: *Where is configuration, identity, and integration routing managed?* That is the control plane.

### Deployment (across modules and products)

- **On‑premise:** SAP NetWeaver (S/4HANA, ECC, BW) runs in the customer data center. Full control over infrastructure, DB, and Basis; upgrades and patches are customer‑driven. All core ERP modules (FI, CO, MM, SD, PP, QM, PM, PS, HCM, WM/EWM) and technical components (ABAP, Basis, BW) deploy together as one or more systems.

- **SAP S/4HANA Cloud (public / private):** ERP is operated by SAP or a partner. Deployment is subscription‑based; release and maintenance are managed by SAP. Same modules, but deployment and many configuration options are constrained by the cloud model. Extensions run on BTP (side‑by‑side) to keep core clean.

- **Cloud products (SuccessFactors, Ariba, Concur, CX, IBP, SAC, Datasphere, Signavio):** **SaaS only.** No customer deployment of the application; customers get a tenant and configure via admin UIs and APIs. Deployment concepts are: **tenant**, **region** (where the tenant is hosted), and **connectivity** (how it integrates with S/4HANA and BTP).

- **Hybrid:** Typical pattern is **S/4HANA (cloud or on‑premise) + BTP + one or more cloud products.** BTP is the control plane for integration and extension; data plane spans S/4HANA (transactional data) and cloud products (their own data stores).

- **By module/product:** Each chapter will state deployment options (on‑prem only, cloud only, or both) and how it fits into hybrid landscapes.

### Integration (across modules and products)

- **Within ERP (same system):** Document flow, automatic postings, and shared master data (e.g. FI–MM–SD–CO). No middleware required; integration is via function calls and shared tables (data plane) and customizing (control plane).

- **ERP to ERP or ERP to cloud:**
  - **SAP CPI (BTP):** Prebuilt and custom iFlows for S/4HANA ↔ SuccessFactors, Ariba, Concur, CX, etc. Control plane: BTP; data flows over secure channels.
  - **OData / REST / BAPIs:** S/4HANA and many cloud products expose or consume APIs. Integration applications (on BTP or elsewhere) call these APIs—control plane defines endpoints and security.
  - **IDocs / RFC:** Legacy and still used for ERP–ERP or ERP–non‑SAP. Routing and mapping in PI/PO or CPI (control plane).
  - **Events (SAP Event Mesh):** Event‑driven integration; publishers (e.g. S/4HANA, SuccessFactors) and subscribers (BTP apps, other systems). Control plane: BTP; event topics and subscriptions.

- **Cloud product to cloud product:** Often via BTP (CPI, API Management, Event Mesh). Example: SuccessFactors ↔ S/4HANA for employee and cost center; Ariba ↔ S/4HANA for P2P; Concur ↔ S/4HANA for expense posting.

- **Analytics:** Datasphere and SAC connect to S/4HANA, BW, and cloud products via connectors and APIs. Data is replicated or accessed live; control plane: Datasphere spaces, SAC connections, and BTP entitlements.

Each chapter will call out **key integration points** (which systems, which protocols) and **deployment context** (on‑prem, cloud, hybrid) so learners see how data and control planes and deployment/integration apply to that module or product.

---

## Section: ABAP

ABAP is SAP’s primary language for application and report development. It runs in the **data plane** (reading/writing business data) and is governed by the **control plane** (transports, authorizations, and deployment).

### Role of ABAP across the landscape

- **In SAP ERP / S/4HANA:** ABAP implements custom reports, enhancements, forms, and workflows. It accesses all module tables (FI, CO, MM, SD, PP, etc.), calls BAPIs and function modules, and is invoked from transactions, Fiori apps, or other systems (RFC, OData). Development is in the same system (or in a linked development system); **deployment** is via **transport requests** (export/import between dev → quality → production). **Integration:** Other systems integrate *with* ABAP via RFC, BAPI, OData services, or IDoc processing.

- **In SAP BTP — ABAP environment (Steampunk):** ABAP runs in the cloud as a BTP service. Code is **ABAP Cloud** (restricted APIs, no direct access to customer S/4HANA tables). It extends S/4HANA via released APIs and OData; **deployment** is via **git and BTP build/deploy**, not transports. **Integration:** BTP ABAP consumes S/4HANA and cloud APIs; it can expose OData and events. Control plane: BTP subaccount, entitlements, and identity (IAS).

- **Clean Core:** SAP recommends moving new logic to BTP (ABAP or other runtimes) and keeping S/4HANA core as standard. So “ABAP” today spans **on‑premise/embedded** (classic, for maintenance and allowed extensions) and **BTP ABAP** (new development, cloud‑native).

### Data plane and control plane (ABAP)

- **Data plane:** ABAP programs read/write database tables (DDIC structures), call other modules’ logic, and participate in document flows. In BTP ABAP, data plane access is only through released S/4HANA APIs and BTP persistence (e.g. draft tables), not direct ERP table access.
- **Control plane:** Transport management (SE09, STMS), role/authorization (SU53, PFCG), and (in BTP) subaccount and lifecycle management control where and how ABAP code is deployed and who can run it.

### Deployment and integration (ABAP)

- **Deployment:** On‑premise / S/4HANA Cloud: transport (dev → test → prod). BTP: ABAP environment; deploy from repository (e.g. abapGit) via BTP pipeline or manual.
- **Integration:** ABAP exposes RFC/BAPI/OData; it consumes RFC, HTTP, OData, and events. Fiori apps call ABAP backends via OData. CPI and external systems call BAPIs or OData. See **Module 24: ABAP Basics** for hands‑on development, DDIC, and debugging; **Module 29: Integration** for middleware and APIs.

---

## Module list (chapters)

### Foundation & Core ERP (1–12)

| # | Chapter title | Module code |
|---|----------------|-------------|
| 1 | **Foundation concepts** (data plane, control plane, deployment, integration) | — |
| 2 | SAP Overview & Navigation | — |
| 3 | Financial Accounting | FI |
| 4 | Controlling | CO |
| 5 | Materials Management | MM |
| 6 | Sales and Distribution | SD |
| 7 | Production Planning | PP |
| 8 | Quality Management | QM |
| 9 | Plant Maintenance | PM |
| 10 | Project System | PS |
| 11 | Human Capital Management | HCM |
| 12 | Warehouse Management / EWM | WM / EWM |

### SAP Cloud & Other Products (13–23)

| # | Chapter title | Product |
|---|----------------|---------|
| 13 | SAP SuccessFactors | SuccessFactors |
| 14 | SAP Fieldglass | Fieldglass |
| 15 | SAP Ariba | Ariba |
| 16 | SAP Concur | Concur |
| 17 | SAP Customer Experience (CX) | CX |
| 18 | SAP Transportation Management | TM |
| 19 | SAP Integrated Business Planning | IBP |
| 20 | SAP Analytics Cloud | SAC |
| 21 | SAP Datasphere | Datasphere |
| 22 | SAP Business Technology Platform | BTP |
| 23 | SAP Signavio | Signavio |

### Technical & Modern ERP (24–31)

| # | Chapter title | Module code |
|---|----------------|-------------|
| 24 | ABAP Basics | ABAP |
| 25 | Business Warehouse / Business Intelligence | BW / BI |
| 26 | SAP Basis | Basis |
| 27 | Security & Authorization | Security |
| 28 | Fiori & User Experience | Fiori |
| 29 | Integration (PI/CPI) | PI / CPI |
| 30 | SAP S/4HANA Overview | S/4HANA |
| 31 | Cross-Module Integration & Best Practices | — |

**Total:** 1 welcome + 31 modules = **32 content pages** (welcome + 31 chapters).  
Quizzes are at the end of each of the 31 modules.

---

## Chapter outlines

Each chapter will include (as applicable):

- **Purpose** — Why the module/product exists and what business problems it solves.
- **Features** — Main sub-components and capabilities.
- **Data plane & control plane** — Where this module’s data lives and is processed; where configuration, security, and integration are governed (see Module 1: Foundation concepts). Each chapter states this briefly for the module/product.
- **Deployment** — On‑premise, cloud, SaaS, or hybrid; how this solution is deployed and how it fits in a landscape.
- **Integration** — Key integration points (which systems, which protocols: CPI, OData, RFC, events, etc.).
- **Interactions** — How it connects to other modules or products (document flows, interfaces, APIs).
- **Tables / Data model** — Key database tables (ERP) or main entities/objects (cloud products).
- **Other** — Important T-codes (ERP), admin/config areas, key concepts, and tips for beginners/intermediate users.
- **Quiz** — 3–5 questions reinforcing the chapter.

Below is the content outline per chapter.

---

### Module 1: Foundation concepts

- **Purpose of chapter:** Establish the shared concepts used across all SAP modules and products: **data plane**, **control plane**, **deployment**, and **integration**. Learners will refer back to this chapter as they move through each module.
- **Content:**
  - **Purpose:** Why these concepts matter—every SAP solution has a data plane (where data lives and is processed) and a control plane (where it is configured and governed); deployment and integration determine how solutions are run and connected.
  - **Features:** Single reference section covering all four concepts; no product-specific depth here (that is in each module).
  - **Data plane:** Definition and examples for ERP/S/4HANA (tables, application server, HANA); cloud products (entities, APIs, no direct DB); Datasphere/BW (semantic layer, datasets); BTP’s role (mainly control plane; extensions may hold data). Takeaway: *Where is business data stored and processed?*
  - **Control plane:** Definition and examples for ERP (Basis, customizing, security, integration config); BTP (global account, subaccounts, IAS, Integration Suite, multi-region); cloud products (admin UIs, provisioning, SSO). Takeaway: *Where is configuration, identity, and integration routing managed?*
  - **Deployment:** On‑premise (NetWeaver, full control); S/4HANA Cloud (public/private, subscription); SaaS (SuccessFactors, Ariba, Concur, CX, IBP, SAC, Datasphere, Signavio—tenant, region, connectivity); hybrid (S/4 + BTP + cloud products). Each later chapter states deployment option(s) for that module.
  - **Integration:** Within ERP (document flow, shared tables); ERP ↔ cloud (CPI, OData/BAPIs, IDocs/RFC, Event Mesh); cloud-to-cloud and analytics (Datasphere, SAC). Each later chapter calls out key integration points.
  - **Interactions:** This chapter does not describe module-to-module flows; it provides the vocabulary. Module 2 (Overview) and subsequent modules apply these concepts.
  - **Tables / Data model:** Not applicable (conceptual chapter).
  - **Other:** How to use this chapter—before or after Module 2 (Overview); reference “Foundation concepts” when a later chapter mentions data plane, control plane, deployment, or integration.
- **Quiz:** 3–5 questions on definitions of data plane vs control plane, deployment options (on‑prem vs cloud vs SaaS), and integration patterns (within ERP vs CPI vs APIs).

---

### Module 2: SAP Overview & Navigation

- **Purpose of chapter:** Introduce SAP as a platform and teach basic navigation so learners can use any module. Builds on Module 1: Foundation concepts for data plane, control plane, deployment, and integration.
- **Content:**
  - **Purpose:** What is SAP ERP / S/4HANA; evolution and deployment options (on-premise, cloud, hybrid)—see Module 1 for deployment concepts.
  - **Features:** Logon clients, SAP GUI vs Fiori; menu structure; favorites; work centers.
  - **Data plane & control plane:** Brief recap: in ERP, data plane = DB + application server; control plane = Basis, customizing, security. Point to Module 1 for full definitions.
  - **Deployment:** ERP on‑prem vs S/4HANA Cloud vs hybrid; reference Module 1.
  - **Integration:** High-level: document flow and APIs; detail in Module 29 (Integration).
  - **Interactions:** High-level flow of business processes across modules (order to cash, procure to pay).
  - **Tables:** Not module-specific; introduce concept of transparent tables and where to look (e.g. Data Dictionary).
  - **Other:** Transaction codes (e.g. SE16, SM30), client concept, roles and authorization basics, help resources.
- **Quiz:** 3–5 questions on navigation, clients, “what is SAP,” and simple data-plane vs control-plane distinction.

---

### Module 3: Financial Accounting (FI)

- **Purpose of chapter:** Explain how SAP records and reports external financial transactions.
- **Content:**
  - **Purpose:** Legal and group reporting; reconciliation with other modules.
  - **Features:** General Ledger (FI-GL), Accounts Payable (FI-AP), Accounts Receivable (FI-AR), Asset Accounting (FI-AA), Bank Accounting, Tax, Closing.
  - **Data plane & control plane:** Data plane: FI tables (BKPF, BSEG, subledgers); processing in application server. Control plane: company code and G/L config (SPRO), security (FI authorizations). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Receives postings from MM, SD, CO; automatic account determination; interfaces for bank, tax, consolidation.
  - **Interactions:** Document flow from MM (invoices), SD (customer invoices), CO (cost postings); automatic account determination.
  - **Tables:** BKPF, BSEG; BSIS/BSAS, BSID/BSAD; SKA1, SKAT; LFA1, LFB1; KNA1, KNB1; ANKA, ANLZ; T001 (company code).
  - **Other:** Company code, chart of accounts, fiscal year, posting keys, document types; key T-codes (FB01, F-02, F-32, F-44, FB03, S_ALR_87012345).
- **Quiz:** Purpose of FI, subledgers, key tables, and document flow.

---

### Module 4: Controlling (CO)

- **Purpose of chapter:** Explain how SAP supports internal cost and management accounting.
- **Content:**
  - **Purpose:** Cost planning, allocation, and analysis; profitability; internal reporting.
  - **Features:** Cost Element Accounting (CO-OM-CEL), Cost Center Accounting (CO-OM-CCA), Internal Orders, Profit Center Accounting (EC-PCA), Product Costing (CO-PC), Profitability Analysis (CO-PA).
  - **Data plane & control plane:** Data plane: CO tables (COEP, COBK, cost center, profit center); processing for allocation and settlement. Control plane: controlling area, cost center hierarchy, customizing (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Receives costs from FI, MM, SD, PP, PS; settlement to FI/assets; interfaces for planning and reporting.
  - **Interactions:** Receives costs from FI, MM, SD, PP, PS; settlement and assessment; integration with FI for reconciliation.
  - **Tables:** COEP, COBK; CSKS, CSKU; AUFK, AUKO; CE1xxx, CE2xxx; AUSP; tables for profitability (e.g. COEP-PA).
  - **Other:** Controlling area, cost center, profit center, cost element; key T-codes (KB11N, KO8G, S_ALR_87013611).
- **Quiz:** CO vs FI, cost center vs profit center, main tables.

---

### Module 5: Materials Management (MM)

- **Purpose of chapter:** Cover procurement and inventory management in SAP.
- **Content:**
  - **Purpose:** Purchasing, inventory management, invoice verification; optimizing material flow.
  - **Features:** Purchasing (MM-PUR), Inventory Management (MM-IM), Invoice Verification (MM-IV), Material Master, Vendor Master; batch management; valuation.
  - **Data plane & control plane:** Data plane: MM tables (MARA, EKKO, MKPF, MSEG); processing for PO, GR, invoice. Control plane: plant, purchasing org, account determination (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Postings to FI/CO; link to SD and PP; Ariba/cloud procurement via CPI (see Module 15, 29).
  - **Interactions:** Purchase order → goods receipt → invoice (three-way match); postings to FI/CO; link to SD (sales orders) and PP (requirements).
  - **Tables:** MARA, MARC, MARD; EKKO, EKPO; MKPF, MSEG; LFA1, LFB1; T001W (plant), T024 (purchasing groups); A502 (account determination).
  - **Other:** Plant, storage location, purchasing organization, document types (NB, RE); key T-codes (ME21N, MIGO, MIRO, MM03).
- **Quiz:** Procure-to-pay steps, key tables, organizational levels.

---

### Module 6: Sales and Distribution (SD)

- **Purpose of chapter:** Cover order-to-cash and sales processes.
- **Content:**
  - **Purpose:** Sales orders, delivery, billing, pricing; customer and revenue management.
  - **Features:** Pre-sales (quotations, contracts); Sales (orders, scheduling); Shipping (delivery, picking); Billing (invoices, revenue); pricing, output; credit management.
  - **Data plane & control plane:** Data plane: SD tables (VBAK, VBAP, LIKP, LIPS, VBRK); processing for order, delivery, billing. Control plane: sales org, distribution channel, pricing and output config (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Postings to FI (AR, revenue); link to MM (stock), PP (availability); CRM/CX via APIs (Module 17, 29).
  - **Interactions:** Sales order → delivery → billing; postings to FI (AR, revenue); link to MM (stock), PP (availability).
  - **Tables:** VBAK, VBAP; LIKP, LIPS; VBRK, VBRP; KNA1, KNVV, KNB1; MARA/MVKE; KONV (pricing); T001 (company code).
  - **Other:** Sales organization, distribution channel, division; document flow; key T-codes (VA01, VL01N, VF01, VA03).
- **Quiz:** SD document flow, organizational structure, main tables.

---

### Module 7: Production Planning (PP)

- **Purpose of chapter:** Introduce manufacturing planning and execution in SAP.
- **Content:**
  - **Purpose:** Demand management, MRP, capacity planning, production execution and reporting.
  - **Features:** Demand Management (MPS/MRP); BOM and work center; MRP (MD04, etc.); Production orders (PP-PI-PRO); capacity planning; repetitive manufacturing; PP-DS (advanced).
  - **Data plane & control plane:** Data plane: PP tables (AFPO, AFKO, RESB, BOM, work center); MRP and order processing. Control plane: plant, MRP areas, work center and routing config (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Requirements from SD; procurement from MM; cost and settlement to CO; IBP for planning (Module 19).
  - **Interactions:** Requirements from SD; procurement from MM; cost and settlement to CO; capacity and materials.
  - **Tables:** AFPO, AUFK, AFKO; STPO, MAST; CRHD, CRTX; RESB (dependent requirements); MARC (MRP views).
  - **Other:** Plant, MRP controller, work center; key T-codes (MD04, CO01, CO02, COR1).
- **Quiz:** PP flow, BOM and work center, key T-codes and tables.

---

### Module 8: Quality Management (QM)

- **Purpose of chapter:** Explain quality processes in procurement and production.
- **Content:**
  - **Purpose:** Incoming inspection, in-process inspection, quality certificates; compliance and quality records.
  - **Features:** Quality planning (inspection types, characteristics); quality in procurement (inspection lots); quality in production; quality certificates; quality notifications (quality info system).
  - **Data plane & control plane:** Data plane: QM tables (QALS, QAVE, inspection plans); inspection processing. Control plane: inspection type and plan config (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Triggered by MM (GR) and PP (production); usage decision and stock; link to PM (defects).
  - **Interactions:** Inspection lots from MM (GR) and PP (production); usage decisions and stock status; integration with PM for defects.
  - **Tables:** QMAT (inspection type–material); QALS (inspection lot); QAVE (results); PLKO, PLPO (inspection plan); AUSP (characteristic values).
  - **Other:** Quality management in procurement/production; usage decision; key T-codes (QE51N, QA03, QE02).
- **Quiz:** QM in procurement vs production, inspection lot, key tables.

---

### Module 9: Plant Maintenance (PM)

- **Purpose of chapter:** Cover maintenance of equipment and technical objects.
- **Content:**
  - **Purpose:** Preventive and corrective maintenance; work orders; spare parts and costs.
  - **Features:** Functional location, equipment; maintenance plans; work orders (notification, order); spare parts (MM); capacity and cost (CO); mobile and EAM.
  - **Data plane & control plane:** Data plane: PM tables (EQUI, IFLO, AUFK, QMEL); order and notification processing. Control plane: maintenance plan and order type config (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Material and services from MM; cost to CO; link to PP and QM (defects).
  - **Interactions:** Notifications and orders; material and external services from MM; cost to CO; link to PP and QM.
  - **Tables:** IFLO (functional location), EQUI (equipment); AUFK, AFKO (orders); AFPO; QMEL (notification); T001K (valuation area).
  - **Other:** Technical object hierarchy; order type; key T-codes (IW31, IW32, IP30).
- **Quiz:** PM objects, order types, integration with MM/CO.

---

### Module 10: Project System (PS)

- **Purpose of chapter:** Explain project and investment management.
- **Content:**
  - **Purpose:** Project structuring (WBS, activities); scheduling; budgeting; procurement and cost settlement.
  - **Features:** Work breakdown structure (WBS); network activities; milestones; budgeting; material and external procurement; settlement to FI/CO/AM.
  - **Data plane & control plane:** Data plane: PS tables (PROJ, PRPS, networks, COEP); project and settlement processing. Control plane: project profile, settlement config (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA Cloud; core ERP.
  - **Integration:** Costs from MM and external; settlement to FI/CO/assets; link to IM.
  - **Interactions:** Costs from MM, external services, internal allocations; settlement to cost center, order, asset; link to Investment Management (IM).
  - **Tables:** PROJ (project definition); PRPS (WBS); AFKO, AFPO (networks); COEP (actuals); settlement rule tables.
  - **Other:** Project definition, WBS element, network; key T-codes (CJ20N, CJ03, CN41).
- **Quiz:** WBS vs network, settlement, key tables.

---

### Module 11: Human Capital Management (HCM)

- **Purpose of chapter:** Introduce HR and payroll in SAP ERP (on-premise core). Cloud HCM is covered in Module 13 (SuccessFactors) and Module 14 (Fieldglass).
- **Content:**
  - **Purpose:** Personnel administration, payroll, time management, organizational management.
  - **Features:** Personnel Administration (PA); Organizational Management (OM); Time Management (PT); Payroll; Recruitment; Training and events.
  - **Data plane & control plane:** Data plane: HCM infotypes and cluster tables (PA*, payroll); personnel and payroll processing. Control plane: personnel area, org structure, payroll config (SPRO). See Module 1.
  - **Deployment:** On‑premise (ECC/S/4HANA); cloud HCM in Module 13–14.
  - **Integration:** Postings to FI; optional integration with SuccessFactors (Employee Central), Concur (expenses); Module 29.
  - **Interactions:** Postings to FI (payroll, travel); integration with time and leave; interfaces to external systems; optional integration with SuccessFactors (Employee Central).
  - **Tables:** PA0001, PA0002 (infotypes); T001P (personnel area); T527X (org structure); cluster tables (e.g. PC); payroll result tables.
  - **Other:** Personnel area, personnel subarea, employee group; infotypes; key T-codes (PA30, PP01, PA20).
- **Quiz:** Infotypes, OM vs PA, payroll flow.

---

### Module 12: Warehouse Management / Extended WM (WM / EWM)

- **Purpose of chapter:** Cover warehouse operations and storage management.
- **Content:**
  - **Purpose:** Storage types, bins; putaway and picking; stock transfers; integration with MM/SD.
  - **Features:** WM: storage types, transfer orders, physical inventory; EWM: advanced warehouse, labor management, yard management, wave processing; embedded EWM in S/4HANA.
  - **Data plane & control plane:** Data plane: WM/EWM tables (LQUA, LAGP, LIKP/LIPS, /SCWM/*); transfer order and stock processing. Control plane: warehouse number, storage type, EWM config (SPRO). See Module 1.
  - **Deployment:** On‑premise and S/4HANA (embedded EWM); standalone EWM possible.
  - **Integration:** Triggered by MM and SD; integration with TM (Module 18) where used.
  - **Interactions:** Triggered by MM (goods movement) and SD (delivery); stock and quantity updates; integration with transportation (TM) where used.
  - **Tables:** LAGP (storage bin), LQUA (quant); LIKP, LIPS (delivery); MSEG (material document); EWM-specific tables in S/4HANA.
  - **Other:** Warehouse number, storage type, transfer order; key T-codes (LT01, LT0A, /SCWM/… for EWM).
- **Quiz:** WM vs EWM, transfer order, key concepts and tables.

---

### Module 13: SAP SuccessFactors

- **Purpose of chapter:** Introduce SAP’s cloud HCM suite for talent and people management.
- **Content:**
  - **Purpose:** End-to-end HR in the cloud: recruiting, onboarding, core HR (Employee Central), goals, performance, compensation, learning, succession; replace or extend on-premise HCM.
  - **Features:** Recruiting (RMK); Onboarding (RCM); Employee Central (EC) — org structure, employee profile, position management; Goals (PM-GM); Performance & Development (PM); Compensation (EC Comp); Learning (LMS); Succession (Succession Management); Workforce Analytics.
  - **Data plane & control plane:** Data plane: tenant entities (User, Employee, Position, Goal, etc.); no direct DB—APIs and UIs. Control plane: Admin Center, provisioning, RBP, integration center; IAS for SSO. See Module 1.
  - **Deployment:** SaaS only; tenant, region; connectivity to S/4HANA and BTP.
  - **Integration:** S/4HANA (employee, cost center); payroll (EC Payroll, third-party); Concur; IAS; CPI/replication (Module 29).
  - **Interactions:** Integration with S/4HANA (employee data, cost center); payroll integration (EC Payroll, third-party); Concur (expenses); identity and SSO (SAP IAS); data replication (SAP PI/CPI, middleware).
  - **Tables / Data model:** Cloud — no direct table access; key entities: User, Employee, Position, Job, Goal, Performance form, Learning assignment; Employee Central Foundation (ECF) objects; integration APIs and OData.
  - **Other:** Admin Center; provisioning; role-based permissions; RBP (rule-based permissions); integration center; SAP SuccessFactors Learning Hub / SAP Learning site.
- **Quiz:** SuccessFactors vs on-premise HCM, main modules (EC, RMK, PM, LMS), integration with S/4HANA.

---

### Module 14: SAP Fieldglass

- **Purpose of chapter:** Introduce SAP’s solution for external workforce and contingent labor.
- **Content:**
  - **Purpose:** Manage contingent workers, freelancers, and statement-of-work (SOW) projects; procurement and compliance for external labor.
  - **Features:** Vendor Management System (VMS); worker sourcing and onboarding; time and expense; invoicing and payments; analytics and compliance; integration with procurement and HR.
  - **Data plane & control plane:** Data plane: tenant entities (Program, Request, Worker, Timesheet, Invoice); APIs. Control plane: Fieldglass Admin, provisioning; IAS where used. See Module 1.
  - **Deployment:** SaaS only; tenant; connectivity to Ariba, S/4HANA, SuccessFactors.
  - **Integration:** SuccessFactors, Ariba, S/4HANA (cost/finance), Concur; CPI (Module 29).
  - **Interactions:** Often used with SuccessFactors (worker data); Ariba (procurement, invoicing); S/4HANA (cost, finance); Concur (expenses where applicable).
  - **Tables / Data model:** Cloud — key entities: Program, Request, Worker, Timesheet, Invoice; API and integration endpoints.
  - **Other:** Program types; MSP (Managed Service Provider) model; SOW vs contingent; SAP Fieldglass Admin; reporting.
- **Quiz:** Purpose of Fieldglass, contingent vs SOW, integration with Ariba/SuccessFactors.

---

### Module 15: SAP Ariba

- **Purpose of chapter:** Cover SAP’s cloud solution for sourcing and procurement.
- **Content:**
  - **Purpose:** Strategic sourcing, supplier qualification, contract management, procure-to-pay (P2P) in the cloud; buyer–supplier collaboration.
  - **Features:** Ariba Sourcing (RFx, auctions); Ariba Contract Management; Ariba Buying (catalog, requisition, PO); Ariba Supplier Lifecycle & Performance; Ariba Network (invoicing, collaboration); Ariba Payables (invoice automation).
  - **Data plane & control plane:** Data plane: tenant data (catalog, PO, invoice); S/4HANA side: replicated PO/invoice. Control plane: Ariba admin, CIG/CPI configuration. See Module 1.
  - **Deployment:** SaaS only; Ariba Network; connectivity to S/4HANA via CPI.
  - **Integration:** S/4HANA (PO, GR, invoice, FI); MM; CPI/CIG (Module 29).
  - **Interactions:** Integration with S/4HANA (PO, GR, invoice, master data); FI (payment, reconciliation); MM (material, vendor); CIG (Cloud Integration Gateway) / CPI.
  - **Tables / Data model:** Cloud — no ERP tables; S/4HANA side: purchase order and invoice replication; Ariba APIs and integration documents (cXML, OData).
  - **Other:** Ariba Network; punchout; guided buying; SAP Business Network; key integration scenarios (P2P, SLP).
- **Quiz:** Ariba Sourcing vs Buying, Ariba Network, integration with S/4HANA.

---

### Module 16: SAP Concur

- **Purpose of chapter:** Introduce SAP’s travel and expense management solution.
- **Content:**
  - **Purpose:** Book travel, submit expenses, enforce policy, and reimburse employees; visibility and control over T&E spend.
  - **Features:** Concur Travel (booking, itineraries); Concur Expense (expense reports, approvals, reimbursement); Concur Request (travel request); receipt capture; policy and audit; reporting and analytics.
  - **Data plane & control plane:** Data plane: tenant entities (User, Report, Expense, Allocation); S/4HANA cost objects. Control plane: Concur Admin, approval workflow, Connector/CPI. See Module 1.
  - **Deployment:** SaaS only; tenant; connectivity to S/4HANA, SuccessFactors.
  - **Integration:** S/4HANA/FI (cost, G/L); SuccessFactors; payroll, cards; Ariba where used; CPI (Module 29).
  - **Interactions:** Integration with S/4HANA / FI (cost allocation, general ledger); SuccessFactors (employee, approver); corporate cards and payroll; Ariba (P-card, invoicing where used).
  - **Tables / Data model:** Cloud — key entities: User, Report, Expense, Allocation; S/4HANA: cost object, G/L account; Concur APIs and connectors.
  - **Other:** Approval workflow; expense types; integration options (Concur Connector, SAP CPI); Concur Admin.
- **Quiz:** Concur Travel vs Expense, integration with FI and SuccessFactors.

---

### Module 17: SAP Customer Experience (CX)

- **Purpose of chapter:** Cover SAP’s CRM and customer-facing cloud solutions.
- **Content:**
  - **Purpose:** Unified customer data; sales, service, marketing, and commerce in one portfolio (C/4HANA, SAP CX).
  - **Features:** SAP Sales Cloud (CRM sales, pipeline, CPQ); SAP Service Cloud (service tickets, knowledge base, field service); SAP Marketing Cloud (campaigns, customer journey); SAP Commerce Cloud (B2B/B2C e-commerce); SAP Customer Data Platform (CDP) / Emarsys (engagement); SAP Revenue Cloud (billing, subscription).
  - **Data plane & control plane:** Data plane: tenant entities (Account, Contact, Opportunity, Ticket, Order); S/4HANA where replicated. Control plane: product admin, SAP Build, Integration Suite; IAS. See Module 1.
  - **Deployment:** SaaS only; tenants per product; connectivity via BTP.
  - **Integration:** S/4HANA (orders, pricing, product); FI/AR; BTP, CPI, IAS (Module 22, 29).
  - **Interactions:** Integration with S/4HANA (orders, deliveries, pricing, product master); FI/AR (billing, revenue); SAP BTP and CPI for integration; identity (IAS).
  - **Tables / Data model:** Cloud applications — entities: Account, Contact, Opportunity, Lead, Ticket, Order; S/4HANA SD/CRM tables where replicated; OData and APIs.
  - **Other:** SAP Build for unified UX; SAP Integration Suite for CX; key products (Sales Cloud, Service Cloud, Commerce Cloud).
- **Quiz:** CX product family, Sales vs Service vs Commerce Cloud, integration with S/4HANA.

---

### Module 18: SAP Transportation Management (TM)

- **Purpose of chapter:** Cover planning and execution of freight and transportation.
- **Content:**
  - **Purpose:** Plan and execute shipments; carrier selection, freight order management, and settlement; optimize cost and service.
  - **Features:** Freight order management; planning (optimization, load building); tendering and carrier selection; execution (tracking, events); freight settlement and billing; integration with EWM and shipping (SD).
  - **Data plane & control plane:** Data plane: TM tables (/SCMTMS/*); freight order, shipment processing. Control plane: TM customizing (SPRO); S/4HANA embedded or standalone. See Module 1.
  - **Deployment:** On‑premise and S/4HANA (embedded or standalone).
  - **Integration:** SD (deliveries), EWM (outbound); FI (freight cost).
  - **Interactions:** Triggered by SD (deliveries) and EWM (outbound); carrier and freight data; postings to FI (freight cost); S/4HANA embedded TM or standalone.
  - **Tables:** /SCMTMS/* (TM tables); freight order, freight unit, shipment; document flow tables; S/4HANA embedded: same namespace.
  - **Other:** Freight order vs shipment; TM vs EWM (warehouse vs transport); key Fiori apps and T-codes (/SCMTMS/…).
- **Quiz:** TM purpose, freight order, integration with SD and EWM.

---

### Module 19: SAP Integrated Business Planning (IBP)

- **Purpose of chapter:** Introduce demand and supply planning in the cloud.
- **Content:**
  - **Purpose:** Demand planning, supply planning, inventory optimization, S&OP; align demand and supply across the network.
  - **Features:** IBP for demand; IBP for supply; IBP for inventory; IBP for response and supply; S&OP and integrated business planning; analytics and dashboards.
  - **Data plane & control plane:** Data plane: cloud planning models (key figures, dimensions); S/4HANA demand/supply data. Control plane: IBP admin, integration jobs, planning areas. See Module 1.
  - **Deployment:** SaaS only; connectivity to S/4HANA (and BW) via CPI.
  - **Integration:** S/4HANA (demand, supply, inventory, master); ERP MRP; CPI; SAC (Module 20, 29).
  - **Interactions:** Data from S/4HANA (demand, supply, inventory, master data); ERP MRP and production; real-time integration (CPI, HCI) or batch.
  - **Tables / Data model:** Cloud planning models (key figures, dimensions); S/4HANA: demand, stock, capacity; integration jobs and planning areas.
  - **Other:** Planning levels and key figures; IBP Excel add-in; SAP Analytics Cloud integration; IBP vs APO (legacy).
- **Quiz:** IBP modules (demand, supply, inventory), integration with S/4HANA.

---

### Module 20: SAP Analytics Cloud (SAC)

- **Purpose of chapter:** Introduce SAP’s cloud analytics and planning platform.
- **Content:**
  - **Purpose:** Business intelligence, planning, and predictive analytics in one cloud solution; dashboards, stories, and predictive scenarios.
  - **Features:** Stories (visualizations, tables, filters); digital boardroom; planning (budgets, forecasts, allocations); predictive analytics (smart predict, search to insight); live and import data connections; SAP Datasphere connectivity.
  - **Data plane & control plane:** Data plane: SAC models and datasets; live/import from S/4HANA, BW, Datasphere. Control plane: SAC tenant, roles, connections; BTP entitlements. See Module 1.
  - **Deployment:** SaaS only; connectivity to S/4HANA, BW, Datasphere, IBP.
  - **Integration:** S/4HANA, BW/4HANA, Datasphere (Module 21); IBP; files and third-party (Module 29).
  - **Interactions:** Connects to S/4HANA (live or export); BW/4HANA; SAP Datasphere; flat files and third-party sources; IBP and other SAP apps.
  - **Tables / Data model:** No direct tables; models (dimensions, measures); live connections to ERP/BW; data export and import datasets.
  - **Other:** SAC tenant; roles and security; story vs digital boardroom; SAP Datasphere as data layer (next chapter).
- **Quiz:** SAC stories vs planning, data sources, predictive features.

---

### Module 21: SAP Datasphere

- **Purpose of chapter:** Introduce SAP’s data warehouse and data fabric in the cloud.
- **Content:**
  - **Purpose:** Unify and govern data from SAP and non-SAP sources; semantic layer and business-ready datasets for analytics and AI.
  - **Features:** Data integration (connections, replication, transformation); data modeling (spaces, entities, views, perspectives); business builder; consumption by SAC, S/4HANA, and external tools; data marketplace.
  - **Data plane & control plane:** Data plane: spaces, remote/local tables, views, perspectives; semantic layer. Control plane: BTP subaccount, spaces, authorizations. See Module 1.
  - **Deployment:** SAP BTP service (cloud); connectivity to S/4HANA, cloud products, external.
  - **Integration:** S/4HANA, SuccessFactors, Ariba, etc.; feeds SAC, BW, apps (Module 20, 22, 29).
  - **Interactions:** Ingests from S/4HANA, SuccessFactors, Ariba, and others; feeds SAC, BW, and applications; SAP BTP service.
  - **Tables / Data model:** Cloud objects — spaces, remote tables, local tables, views, perspectives; no traditional ERP table names; semantic layer and business context.
  - **Other:** Datasphere vs BW/4HANA; spaces and authorizations; SAP Analytics Cloud connection.
- **Quiz:** Datasphere purpose, spaces and views, link to SAC.

---

### Module 22: SAP Business Technology Platform (BTP)

- **Purpose of chapter:** Introduce SAP’s unified platform for extension, integration, and development. Central **control plane** for integration and extension; see Module 1.
- **Content:**
  - **Purpose:** Build, integrate, and extend SAP and non-SAP applications; run services for analytics, AI, automation, and development.
  - **Features:** SAP Integration Suite (CPI, API Management, Event Mesh); SAP Extension Suite (ABAP Cloud, Steampunk, SAP Build); SAP Datasphere (data); SAP BTP services (AI, ML, workflow, Fiori); identity (IAS) and security.
  - **Data plane & control plane:** BTP is primarily **control plane** (subaccounts, IAS, Integration Suite, entitlements); extensions and apps on BTP may hold their own data (e.g. ABAP environment, Cloud Foundry). See Module 1.
  - **Deployment:** Cloud only; global account, subaccounts, regions; hybrid with S/4HANA and cloud products.
  - **Integration:** CPI, API Management, Event Mesh connect S/4HANA and all cloud products (Module 29).
  - **Interactions:** Connects all SAP and third-party apps; CPI for Ariba, SuccessFactors, Concur, S/4HANA; BTP subaccount and entitlements.
  - **Tables / Data model:** N/A (platform); reference to connected systems and their tables/APIs.
  - **Other:** Global account, subaccount, entitlements; Cloud Foundry vs ABAP environment; SAP Build; key services (Integration Suite, Extension Suite).
- **Quiz:** BTP role, Integration Suite vs Extension Suite, subaccount.

---

### Module 23: SAP Signavio

- **Purpose of chapter:** Introduce process mining and process management.
- **Content:**
  - **Purpose:** Discover, analyze, and improve business processes using process mining and collaborative process management.
  - **Features:** Signavio Process Intelligence (process mining from event logs); Signavio Process Manager (modeling, collaboration); Signavio Process Governance (governance, control); process insights and KPIs.
  - **Data plane & control plane:** Data plane: event logs (from ERP/CRM); process models and KPIs in Signavio. Control plane: connectors, SAP Build/BTP integration. See Module 1.
  - **Deployment:** Cloud (Signavio tenant); connectivity to S/4HANA, SuccessFactors, etc.
  - **Integration:** S/4HANA, SuccessFactors (event logs); SAP Build Process Automation, BTP (Module 22, 29).
  - **Interactions:** Connectors to S/4HANA, SuccessFactors, and other systems for event log extraction; integration with SAP Build Process Automation and BTP.
  - **Tables / Data model:** Event log tables (from ERP/CRM); process models and KPIs in Signavio; no direct ERP table access in Signavio UI.
  - **Other:** Process mining vs process modeling; SAP Signavio + SAP Build; use cases (order-to-cash, procure-to-pay).
- **Quiz:** Process mining vs process modeling, Signavio products, integration with S/4HANA.

---

### Module 24: ABAP Basics

- **Purpose of chapter:** Introduce custom development and reporting in SAP and how ABAP fits into the data plane, control plane, deployment, and integration (see **Section: ABAP** in this course plan and **Module 1: Foundation concepts**).
- **Content:**
  - **Purpose:** Read and write data; reports and enhancements; extend and integrate with SAP and non-SAP systems using ABAP. Understand ABAP in the data plane (table access, business logic) and control plane (transports, roles).
  - **Features:** Data Dictionary (tables, views, lock objects); ABAP editor and ABAP Development Tools (ADT); reports (classic and ALV); modularization (subroutines, function modules, classes); debugging; enhancement options (user exits, BAdIs, key words); OData and BAPI exposure; ABAP Cloud and BTP ABAP environment (Steampunk) overview.
  - **Data plane & control plane:** Where ABAP runs (application server, HANA); how it accesses module tables (FI, CO, MM, SD, etc.); transport and client as control plane; BTP ABAP as side-by-side extension (no direct ERP table access—released APIs only).
  - **Deployment:** Transport lifecycle (dev → test → prod); transport requests and tasks (SE09, STMS); BTP ABAP: git-based, deploy to BTP subaccount; difference between on-prem/embedded and cloud ABAP.
  - **Integration:** ABAP as provider (RFC, BAPI, OData) and consumer (RFC, HTTP, OData); how Fiori, CPI, and external systems call ABAP; events and outbound interfaces; error handling and idempotency.
  - **Interactions:** Reads/writes module tables; called from transactions or Fiori; invoked by CPI and external systems via RFC/OData; transport and release.
  - **Tables:** DD02L, DD03L (DDIC); TRDIR, TADIR (development objects); E070, E071 (transports); application tables used in examples.
  - **Other:** SE80, SE38, SE24, SE11; transport requests; naming conventions; ABAP Cloud restrictions and released APIs; where to learn more (BTP ABAP, clean core).
- **Quiz:** DDIC, report types, data/control plane for ABAP, transport vs BTP deploy, integration (RFC vs OData). See Module 1 for foundation concepts.

---

### Module 25: Business Warehouse / Business Intelligence (BW / BI)

- **Purpose of chapter:** Introduce reporting and analytics architecture.
- **Content:**
  - **Purpose:** Extract, transform, load (ETL); data modeling; reporting and analysis.
  - **Features:** InfoObjects, DSO, InfoCubes (classic); CompositeProvider, Open ODS (modern); extraction from ERP; BEx and Analysis for Office; SAP Analytics Cloud (SAC) overview.
  - **Data plane & control plane:** Data plane: BW objects (DSO, cubes, CompositeProvider); ERP source tables; processing and reporting. Control plane: RSA1, extraction/load config, authorizations. See Module 1.
  - **Deployment:** On‑premise (BW/4HANA); cloud analytics via SAC, Datasphere (Module 20, 21).
  - **Integration:** Data from FI, CO, MM, SD, etc.; feeds SAC, Datasphere; Module 29.
  - **Interactions:** Data from FI, CO, MM, SD, etc.; real-time and batch extraction; reporting on BW and on S/4HANA (embedded analytics).
  - **Tables:** BW-specific (e.g. fact tables, master data); ERP source tables; RS* (BW admin).
  - **Other:** InfoProvider, key figures and dimensions; key T-codes (RSA1, RSDS, RSPC).
- **Quiz:** ETL, InfoProvider types, BW vs operational reporting.

---

### Module 26: SAP Basis

- **Purpose of chapter:** Explain system administration and technical foundation. Core of the ERP **control plane** (see Module 1).
- **Content:**
  - **Purpose:** Install, configure, and operate SAP systems; transport and client management.
  - **Features:** System architecture (application server, database); client copy and transport (STMS); background jobs; spool; monitoring (CCMS); patches and upgrades; high availability and performance basics.
  - **Data plane & control plane:** Basis is **control plane** (client, instance, transport, jobs, monitoring); data plane is the DB and application layers that Basis supports. See Module 1.
  - **Deployment:** On‑premise and hosted S/4HANA; S/4HANA Cloud is SAP-operated (Basis by SAP).
  - **Integration:** STMS for transport across systems; SM59 for RFC; link to security (Module 27) and Integration (Module 29).
  - **Interactions:** All modules run on Basis; transports carry customizing and development.
  - **Tables:** System tables (e.g. T000); transport tables (E070, E071); job tables; monitoring tables.
  - **Other:** SID, instance, client; key T-codes (SM21, SM37, STMS, SE09, SM50).
- **Quiz:** Client, transport, key Basis T-codes.

---

### Module 27: Security & Authorization

- **Purpose of chapter:** Cover how access control works in SAP. Part of the **control plane** (see Module 1).
- **Content:**
  - **Purpose:** User management; roles and authorizations; compliance and audit.
  - **Features:** User master (SU01); role design (PFCG); authorization objects and values; composite roles; SOD (segregation of duties); audit logging and GRC overview.
  - **Data plane & control plane:** Control plane: users, roles, authorizations (USR02, AGR_*, AUTH); governs who accesses data plane. See Module 1; cloud: IAS, product admin.
  - **Deployment:** On‑premise (SU01, PFCG); cloud products and BTP use IAS and product-specific admin.
  - **Integration:** Every transaction/report checks authorizations; HCM (position-based roles); IAS for SSO (Module 22).
  - **Interactions:** Every transaction and report checks authorizations; integration with HCM (position-based roles).
  - **Tables:** USR02, USR21; AGR_* (roles); AUTH (authorization values); audit log tables.
  - **Other:** Authorization object, role, user; key T-codes (SU01, PFCG, SU53).
- **Quiz:** Roles vs users, authorization objects, SU53.

---

### Module 28: Fiori & User Experience

- **Purpose of chapter:** Introduce the modern SAP UI and design principles.
- **Content:**
  - **Purpose:** Consistent, role-based UX across devices; Fiori apps and SAP Build.
  - **Features:** Fiori design guidelines; Fiori launchpad; standard and custom Fiori apps; SAP Build (low-code); SAP GUI and Web GUI; S/4HANA embedded analytics UI.
  - **Data plane & control plane:** UI layer consumes data plane (OData/BAPI); control plane: launchpad catalog, roles, Gateway config (S/4HANA or BTP). See Module 1.
  - **Deployment:** S/4HANA (embedded Fiori); BTP (Launchpad service); cloud products have own UIs.
  - **Integration:** Fiori apps call S/4HANA (OData, BAPIs); BTP Launchpad federates content; IAS for SSO (Module 22, 29).
  - **Interactions:** Fiori apps call backend (OData, BAPIs); gateway and S/4HANA; single sign-on and identity.
  - **Tables:** Backend tables exposed via OData; Fiori catalog and groups (configuration).
  - **Other:** Launchpad, catalog, OData service; key T-codes (/N/UI5/UI5_REPOSITORY_LOAD, /N/IWFND/MAINT_SERVICE).
- **Quiz:** Fiori vs SAP GUI, launchpad, OData.

---

### Module 29: Integration (PI/CPI)

- **Purpose of chapter:** Explain how SAP connects to other systems. Central **control plane** for integration (see Module 1).
- **Content:**
  - **Purpose:** A2A and B2B integration; middleware and API management.
  - **Features:** SAP PI/PO (Process Integration/Orchestration); SAP CPI (Cloud Integration); IDocs; BAPIs and RFC; OData and REST; API Management; event-driven (e.g. SAP Event Mesh).
  - **Data plane & control plane:** Integration is **control plane** (routing, mapping, APIs); data flows through it between data planes of S/4HANA and cloud products. See Module 1.
  - **Deployment:** PI/PO on‑premise; CPI on BTP (Module 22); hybrid common.
  - **Integration:** Connects S/4HANA with SuccessFactors, Ariba, Concur, CX, IBP, etc.; IDoc, BAPI, OData, events (referenced throughout Modules 13–23).
  - **Interactions:** Inbound/outbound from FI, MM, SD, etc.; mapping and routing; error handling and monitoring.
  - **Tables:** EDIDC (IDoc control); application tables for interface data; CPI is cloud (configuration, not tables).
  - **Other:** IDoc type, message type; key T-codes (WE02, WE19, SM59).
- **Quiz:** IDoc vs BAPI, PI vs CPI, key concepts.

---

### Module 30: SAP S/4HANA Overview

- **Purpose of chapter:** Position S/4HANA as the modern ERP and summarize differences from ECC.
- **Content:**
  - **Purpose:** Single source of truth; simplified data model; real-time analytics; cloud readiness.
  - **Features:** Simplified data model (e.g. ACDOCA, material ledger); embedded analytics; Fiori-first; migration paths (Brownfield, Greenfield); S/4HANA Cloud vs on-premise.
  - **Data plane & control plane:** Data plane: HANA, ACDOCA, simplified and module tables; control plane: Basis, customizing, BTP for extension (Module 1, 22).
  - **Deployment:** On‑premise, S/4HANA Cloud (public/private); hybrid with BTP and cloud products (Module 1).
  - **Integration:** All modules in one suite; CPI, APIs, Event Mesh for cloud and external (Module 29).
  - **Interactions:** All modules in one suite; universal journal; central finance and consolidation options.
  - **Tables:** ACDOCA (universal journal); S/4HANA-specific and simplified tables; compatibility views.
  - **Other:** Deployment options; release strategy; key differences from ECC.
- **Quiz:** S/4HANA vs ECC, universal journal, deployment options.

---

### Module 31: Cross-Module Integration & Best Practices

- **Purpose of chapter:** Tie modules together and give practical guidance. Recap **data plane, control plane, deployment, and integration** (Module 1) across the landscape.
- **Content:**
  - **Purpose:** End-to-end process view; design and support best practices.
  - **Features:** End-to-end flows (order-to-cash, procure-to-pay, make-to-stock); document flow and tracing; configuration vs customization; support and change management; learning path and certifications.
  - **Data plane & control plane:** Recap: data plane per module (ERP tables, cloud entities); control plane (Basis, BTP, product admin). See Module 1.
  - **Deployment:** Recap on‑prem, cloud, SaaS, hybrid; how modules and products fit (Module 1, 30).
  - **Integration:** Recap FI–CO–MM–SD–PP and cloud product links; CPI, APIs, events (Module 29).
  - **Interactions:** Recap of FI–CO–MM–SD–PP and other key links; cloud product integration patterns.
  - **Tables:** Reference to key integration tables (e.g. VBFA, MKPF–BKPF).
  - **Other:** Best practices; documentation; where to go next (certifications, SAP Learning Hub).
- **Quiz:** Document flow, configuration vs customization, data/control plane and deployment/integration, next steps.

---

## Quizzes

- **Placement:** One quiz at the end of each of the 31 modules (after the main content).
- **Format:** Per boilerplate — `.quiz-section` with multiple `.quiz-question` blocks. Each question has:
  - Question text
  - Options (A, B, C, D as needed)
  - “Show Answer” button
  - Revealed answer with correct option and short explanation
- **Scope:** 3–5 questions per module, covering purpose, features, data plane/control plane and deployment/integration (where applicable), interactions, tables/concepts, and T-codes (where applicable) as introduced in that chapter.

---

## Technical implementation (for build phase)

- **Total modules (chapters):** 31  
- **Sidebar:** 1 welcome + 31 tabs, grouped into sections: **Foundation & Core ERP** (1–12), **SAP Cloud & Products** (13–23), **Technical & Modern ERP** (24–31).  
- **Files:**  
  - `course/index.html`  
  - `course/modules/welcome.html`  
  - `course/modules/module-1.html` … `course/modules/module-31.html`  
- **`totalModules` in JS:** 31  
- **Iframe pattern:** `modules/module-${i}.html` for i = 1..31.
- **Module 1: Foundation concepts** is a dedicated chapter containing the full “Data plane, control plane, deployment & integration” content from this plan. **Start Learning** CTA on the welcome page should show **Module 1 (Foundation concepts)** first so learners get the shared vocabulary before SAP Overview (Module 2) and all other modules.
- **Each chapter (2–31):** Include a short **Data plane & control plane**, **Deployment**, and **Integration** callout (1–2 sentences) consistent with the structure in this plan; reference Module 1 where helpful.
- **Module 22 (BTP)** and **Module 29 (Integration):** Reinforce control plane and integration patterns.
- **Module 31 (Cross-Module):** Recap deployment and integration across the landscape.
- **Section: ABAP:** Full text from “Section: ABAP” in this plan should appear in **Module 24** (ABAP Basics)—e.g. as a dedicated subsection “ABAP in the landscape” before or after the hands-on ABAP basics. Ensure Module 24 covers data plane, control plane, deployment, and integration for ABAP as in the outline above.

---

## Next steps (after plan is finalized)

1. **You confirm:** Scope (all 31 chapters), order, and any add/remove/rename.  
2. **Build:** Create folder structure, `index.html`, `welcome.html`, and all 31 module HTML files from this plan (module-1 = Foundation concepts through module-31 = Cross-Module).  
3. **Content:** Populate each module with full text, tables/concepts, and quiz questions based on the outlines above.  
4. **Review:** You review content and suggest edits.  
5. **Test:** Run through welcome, all modules, and quizzes; fix layout and links.  
6. **Optional:** Theming (e.g. SAP-style colors), extra interactivity, or PDF export.

---

*End of course plan. Please review and confirm or adjust before we proceed to implementation.*
