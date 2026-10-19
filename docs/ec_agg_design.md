# Eye Care Case-based Programme — Aggregate Design Guide { #ec-agg-design }

## 1. Introduction

The Eye Care Case-based Programme provides a standardised digital toolkit for collecting, analysing and reporting routine eye care data required to monitor the World Health Organization (WHO) Eye Care Indicator Menu and the integrated people-centred eye care (IPEC) framework.

The toolkit is implemented using the DHIS2 aggregate data model and provides a harmonised framework for routine eye care reporting from eye care service delivery points — primary eye care posts, secondary clinics, tertiary hospitals and outreach — to subnational and national programme managers. It enables countries to collect a core set of eye care statistics while remaining flexible enough to accommodate national eye health strategies and reporting arrangements.

The toolkit combines:

1. Case utilisation and condition reporting.
2. Cataract surgery pathway reporting.
3. Refractive error service reporting.
4. Referral and uptake reporting.
5. Assistive products need and provision reporting.

Together, these datasets provide the information required to monitor eye care service coverage, cataract surgical outcomes, refractive error correction, referral performance and assistive product availability.

**This is a reference package.** The datasets, data elements, indicators and dashboards it contains are offered as guidance for an implementation, not as a prescriptive standard. Countries are expected to take what fits their national eye health reporting framework, adapt the parts that do not, and extend the package where their policies require more. Nothing in the toolkit assumes that every element will be used, and no part of it is mandatory.

## 2. Background

Eye care systems require reliable, timely and comparable data to support planning, service delivery, cataract surgical quality assurance and monitoring of national and global eye health commitments.

Many countries collect eye care statistics through fragmented facility registers, programme-specific surveys and cataract audit forms, and these datasets are often difficult to analyse together. The Eye Care Toolkit addresses this challenge by providing a unified metadata package that aligns routine eye care reporting with internationally recognised indicators.

The toolkit is primarily aligned with:

* World Health Assembly resolution WHA73.4 on integrated people-centred eye care.
* The WHO Eye Care Indicator Menu (ECIM).
* The World report on vision targets for 2030, including effective cataract surgical coverage (eCSC) and effective refractive error coverage (eREC).
* National eye health strategic plans and routine health information system reporting requirements.

Rather than replacing an existing eye health information system, the toolkit provides a minimum harmonised reporting framework that countries can localise and extend according to national eye health policies.

Two country implementations inform this reference package: a Kenya rollout that drove the first full build of the package (including the Kenya-specific extensions to the cataract surgery pathway and the referral and assistive products modules) and a Sri Lanka rollout in planning at the time of writing, intended to test the package in a second health system context.

## 3. Purpose

The purpose of the Eye Care Toolkit is to provide DHIS2 implementers and Ministries of Health with a reusable metadata package that supports routine collection and analysis of WHO ECIM eye care indicators.

The toolkit aims to:

1. Standardise eye care data collection across levels of the health system.
2. Collect the minimum routine eye care data required for ECIM reporting.
3. Harmonise routine eye care reporting with cataract audit and refractive error service reporting.
4. Improve eye care service planning and resource allocation.
5. Strengthen monitoring of equity in eye care access, including the referral pathway from community outreach.
6. Support reporting to WHO against the 2030 eCSC and eREC targets, and to regional eye health programmes.

## 4. Toolkit Scope

The toolkit supports data collection across three administrative levels.

| Administrative level | Purpose                                                                                  |
| -------------------- | ---------------------------------------------------------------------------------------- |
| **Facility**         | Routine eye care reporting from service delivery points (primary / secondary / tertiary).|
| **Subnational**      | District or county aggregation, validation of facility submissions and local analysis.   |
| **National**         | Programme management, ECIM reporting and national indicator analysis.                    |

Outreach and camp-based services report through the facility that organises them, using the organisation unit under which the camp is registered.

Core design principles

* Harmonised metadata — common eye care data elements aligned with WHO ECIM.
* Scalable architecture — supports national, subnational and facility reporting structures.
* Modular datasets — five dataset modules per workflow, so a facility reports only on the services it provides.
* Configurable implementation — countries may add or remove metadata according to national policies.
* Analytics first — metadata is designed so indicators and dashboards work immediately after installation.
* ECIM alignment — data elements are selected because they contribute to WHO ECIM monitoring.
* Consistency across levels — the same concept is named, structured and disaggregated the same way at every level of care, so that reporting can be compared across the system rather than only within one level.

## 5. System Design Overview

**Reporting Levels**

| Level           | Primary Reporting Unit                                                                      |
| --------------- | ------------------------------------------------------------------------------------------- |
| **Facility**    | Service delivery points submit the modules they are commissioned to report on.              |
| **Subnational** | District or county offices validate facility submissions and analyse subnational indicators.|
| **National**    | Ministry of Health reports national ECIM indicators and monitors eCSC / eREC progress.      |

**DHIS2 Configuration Components**

| Metadata Component                     | Purpose                                                               |
| -------------------------------------- | --------------------------------------------------------------------- |
| **Datasets**                           | Monthly reporting forms, one per workflow module.                     |
| **Data Elements**                      | Individual eye care variables.                                        |
| **Category Combinations**              | Sex, age band, condition, intervention, surgeon type and other disaggregations. |
| **Sections**                           | The common structure applied to each reporting form.                  |
| **Data Entry Forms**                   | Section forms used for web and Android data entry.                    |
| **Indicators**                         | ECIM calculations and derived eye care measures.                      |
| **Data Element and Indicator Groups** | Grouping for analysis and for navigating the package.                 |
| **Dashboards and Visualisations**     | Visual monitoring across ECIM targets.                                |
| **User Groups**                        | Access to metadata, data and dashboards.                              |

## 6. Toolkit Structure

The toolkit contains five datasets, each one aligned to a distinct eye care workflow.

| Dataset                   | Frequency | Reporting Level | Scope                                                              |
| ------------------------- | --------- | --------------- | ------------------------------------------------------------------ |
| **EC Cataract Surgery**   | Monthly   | Facility        | Cataract surgical pathway: listing, surgery, outcomes, complications.|
| **EC Utilisation**        | Monthly   | Facility        | New cases, primary condition, vision status at presentation.         |
| **EC Referral**           | Monthly   | Facility        | Referral in by source and service, outreach referral uptake.         |
| **EC Refractive Error**   | Monthly   | Facility        | Uncorrected refractive error identification, intervention and outcome.|
| **EC Assistive Products** | Monthly   | Facility        | AP need identified and AP provision by product type.                 |

**Why five datasets rather than one.** A single large dataset would force every facility to report on every workflow, including those it does not deliver. A primary eye care post that only screens and refers does not perform cataract surgery; a tertiary cataract centre may not run outreach referral. Splitting the package into five workflow-aligned modules lets an implementation commission each facility onto the datasets that match the services it actually provides. Shared denominator data elements (new cases by sex and age, for example) appear in more than one dataset where needed, so cross-module indicators continue to work.

### 6.1 A common structure across datasets

Every dataset follows the same section ordering, so a reporting officer moving between modules encounters the same layout, and an analyst comparing two modules is comparing like with like.

| # | Section                                 | Collects                                                                                               |
| - | --------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 1 | Service context                         | Service setting (OPD / IPD / community / outreach), reporting period.                                   |
| 2 | Case denominators                       | New cases by sex and age (shared across modules that need the denominator).                            |
| 3 | Workflow-specific inputs                | The variables unique to the module: cataract pathway, RE assessment, referral in, AP need and provision.|
| 4 | Workflow-specific outcomes              | The outcome variables for the module: surgical outcome, RE outcome, referral uptake, AP met-need.      |
| 5 | Disaggregation-driven breakdown rows    | Variables that carry a disaggregation (surgeon type, drop-out reason, intervention type, RE type).      |

The exception is **EC Utilisation**, which has no workflow-specific inputs section — the module exists to characterise the case-mix at presentation and feeds the denominator for most of the other indicators.

### 6.2 Shared denominators and the new-case concept

A single conceptual data element — new eye care cases in the reporting period, disaggregated by sex and age band — is used as the denominator for utilisation indicators (ECIM EC09) and as the base for referral uptake and assistive product need indicators. It is defined once and referenced from each dataset that needs it, rather than being redefined per module. This keeps the arithmetic consistent across the package: a change to the age banding, for example, propagates to every indicator that uses the denominator.

### 6.3 Population estimates

Several indicators — effective cataract surgical coverage, service utilisation rates, assistive product coverage — need a population denominator by age band and sex.

The toolkit does **not** include a population dataset. Most target systems already maintain population estimates, whether from a census projection, a national statistics office feed or an existing DHIS2 dataset, and replacing that arrangement would do more harm than good. The toolkit therefore supplies the population data element itself, disaggregated by age band and sex, and expects the implementation to connect it to whatever population reporting already exists or replace it with the existing data element(s).

### 6.4 The Eye Care Facility List

The toolkit includes an organisation unit group named **EC — Eye Care Facility List**, which ships empty. It is intended to contain every eye care service delivery point in the country at the organisation unit level where the datasets are captured, and it serves as the denominator for indicators that express service coverage as a proportion of eye care facilities (for example, the proportion of facilities offering refractive error services). Because its membership is entirely specific to a country's organisation unit hierarchy, it cannot be supplied with the package and must be populated during implementation.

## 7. WHO ECIM Indicator Framework

The toolkit computes indicators across the WHO Eye Care Indicator Menu, grouped by workflow and separately in a WHO ECIM group that gathers the internationally reported measures in one place.

| Code      | Title                                                                | What the toolkit computes                                                                                    |
| --------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **EC01**  | Pre-operative visual acuity for cataract surgery                     | Distribution of pre-op BCVA bands; disaggregated by sex and high-risk comorbidity.                           |
| **EC02**  | Cataract surgery pre-operative drop-out rate                         | Listed for cataract surgery vs operated; disaggregated by drop-out reason.                                   |
| **EC03**  | Time to cataract surgery                                             | Days between listing and surgery, aggregated per band.                                                       |
| **EC04**  | Cataract surgical complications                                      | Intra-operative and post-operative complication rates; disaggregated by surgeon type and comorbidity.         |
| **EC05**  | Cataract surgery post-operative drop-out rate                        | Listed for post-op review vs reviewed; disaggregated by reason.                                              |
| **EC06**  | Post-operative visual acuity                                         | UCVA band distribution and ≥ 2-line improvement rate; disaggregated by cause of suboptimal outcome.          |
| **EC07**  | Effective refractive correction rate after cataract surgery          | Post-op UCVA improved to good after RE correction, where UCVA was suboptimal due to refractive error.        |
| **EC09**  | Eye care services utilisation                                        | New cases by sex, age band, primary condition, vision status, service setting.                               |
| **EC10**  | Eye care interventions distribution                                  | Interventions delivered, by WHO category.                                                                    |
| **EC11**  | Referral in for eye care                                             | Referrals received by source and by service type.                                                            |
| **EC12**  | Assistive products need and provision in eye care                    | Need identified vs provided, by AP type.                                                                      |
| **EC13**  | Effective refractive intervention rate                               | RE intervention delivered with good outcome, disaggregated by RE type.                                        |
| **EC14**  | Refractive intervention rate                                         | Uncorrected RE identified vs intervention delivered, disaggregated by RE type.                                |
| **EC15**  | Referral uptake for eye care                                         | Patients referred from community outreach who presented for eye care.                                         |

**ECIM EC08 — Diabetic retinopathy screening coverage — is deliberately out of scope for the aggregate reference package.** Diabetic retinopathy screening is addressed in the companion tracker reference design, where it is modelled as an optional case-management stage; facilities that run DR screening programmes capture and analyse DR coverage through the tracker rather than through the aggregate reporting modules. An implementation that needs DR screening data in aggregate form adds a sixth module locally; the reference package does not ship one.

Alongside the WHO ECIM indicators, each workflow has its own indicator group covering measures a programme manager needs for day-to-day management rather than for international reporting — surgical volume by facility and surgeon type, cataract audit outcomes against local targets, RE service throughput, referral in volume by source, AP stock-out proxies and so on.

### 7.1 Extending the indicator set

The toolkit collects more than it currently computes, deliberately. Several extensions can be added by an implementation without any change to data collection, because the underlying fields are already in the forms:

* **Equity analysis.** Every workflow measure is disaggregated by sex, and most by age band. Parity indices between population groups can be derived from the data already collected.
* **Facility performance benchmarking.** Surgical volume, pre-op VA mix and complication rates are collected per facility; percentile and rank indicators can be configured locally without additional data entry.
* **Trend analysis across the cataract audit cycle.** The audit variables (pre-op BCVA, surgeon type, complications, post-op UCVA, cause of suboptimal outcome) are already in the dataset. Period-over-period indicators can be defined on top.
* **Insurance and payment analysis.** Several country implementations extend the service context section with an insurance or payment data element. In Kenya this carries the National Hospital Insurance Fund (NHIF) option, and derived indicators report service coverage by payment type.

These were left as configuration points rather than shipped indicators because the exact definition — which age bands, which comparison groups, which reference population — varies between countries and between reporting frameworks. The data is in place; the calculation is a local decision.

## 8. Disaggregation

Disaggregation is where most of the design effort sits, and it follows a small number of repeating patterns rather than a separate scheme per dataset.

| Family                                   | Applied to                                                                                               |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Sex**                                  | Almost everything, so that every headline measure can be reported by sex.                                 |
| **Age band**                             | New cases, cataract surgery outcomes, AP need and provision, RE outcomes. Cataract indicators run on sex only in the reference package following the current ECIM revision. |
| **Primary condition** (WHO 7)            | Utilisation reporting (EC09): cataract, refractive error, diabetic retinopathy, glaucoma, corneal, other posterior segment, other anterior segment.                       |
| **Intervention category**                | Interventions distribution (EC10): seven WHO intervention categories.                                     |
| **Surgeon type**                         | Cataract complications and outcomes, where a country collects surgeon type (ophthalmologist, clinical officer, cataract surgeon, resident). |
| **Pre-op BCVA band**                     | Pre-op visual acuity reporting (EC01).                                                                   |
| **Post-op UCVA band**                    | Post-op visual acuity and effective correction (EC06, EC07).                                             |
| **Drop-out reason**                      | Pre-operative drop-out (EC02 — up to nine reasons in national extensions) and post-operative drop-out (EC05 — eight reasons). |
| **Referral source** × **service type**   | Referral in reporting (EC11): seven sources by three service types.                                      |
| **Refractive error type**                | RE indicators (EC13, EC14): distance, near, combined.                                                     |
| **Assistive product type**               | AP need and provision indicators (EC12).                                                                 |
| **Service setting**                      | OPD, IPD, community, outreach — carried on every workflow module and mirrored across stages in the tracker companion design. |

Country implementations are expected to extend the disaggregation lists where their policies require more — Kenya adds a nine-reason pre-operative drop-out list and a four-level surgeon type list, both of which the reference package carries as the default option sets following that implementation experience.

## 9. Data entry forms

Each of the five reporting datasets uses the standard DHIS2 **section form** generated from its dataset sections. Section forms were chosen over custom HTML forms because the eye care package is more likely to be captured on the DHIS2 Android app at point of care than on a web browser at month-end, and section forms work in both.

The sections described in section 6.1 are maintained as the single source of truth for form layout. An implementation that adds or removes data elements updates the section definition, and the change propagates automatically to web and Android data entry.

Where a country already uses a custom paper census form or a tablet-based capture tool that does not map to the standard section layout, implementers are free to build a custom data entry form on top of the dataset; the reference package does not ship one.

## 10. Analytics and Dashboard

*[Chapter to be developed. The reference package will ship a set of dashboards aligned to the WHO ECIM target families; this section will describe their structure, intended audience and content once the dashboards have been built.]*

![](resources/images/eye_care_dashboard_overview.png)
*Placeholder — Eye Care dashboard overview.*

### 10.1 WHO ECIM dashboard

*[Placeholder — to be filled once the dashboard is built.]*

![](resources/images/eye_care_ecim_dashboard.png)
*Placeholder — WHO ECIM indicator dashboard.*

### 10.2 Cataract surgical audit dashboard

*[Placeholder — to be filled once the dashboard is built. Expected contents: pre-op BCVA mix, surgical volume, complication rates, post-op UCVA distribution, cause of suboptimal outcome, surgeon-level benchmarking.]*

![](resources/images/eye_care_cataract_audit_dashboard.png)
*Placeholder — Cataract surgical audit dashboard.*

### 10.3 Refractive error service dashboard

*[Placeholder — to be filled once the dashboard is built.]*

![](resources/images/eye_care_refractive_error_dashboard.png)
*Placeholder — Refractive error service dashboard.*

### 10.4 Referral and uptake dashboard

*[Placeholder — to be filled once the dashboard is built.]*

![](resources/images/eye_care_referral_dashboard.png)
*Placeholder — Referral in and outreach uptake dashboard.*

### 10.5 Assistive products dashboard

*[Placeholder — to be filled once the dashboard is built.]*

![](resources/images/eye_care_ap_dashboard.png)
*Placeholder — Assistive products need and provision dashboard.*

## 11. Visualisations

*[Chapter to be developed. The reference package will ship a catalogue of standard visualisations used across the dashboards; this section will list each visualisation, the indicator it carries, its default disaggregation and the dashboard(s) it appears on, once the visualisations have been built.]*

![](resources/images/eye_care_visualisation_catalogue.png)
*Placeholder — Visualisation catalogue overview.*

## 12. Languages

The toolkit metadata is currently available in English. Translation to additional languages is planned alongside country rollouts: French (for francophone African implementations), Spanish and Russian (for a Central Asian implementation) and the national languages of implementing countries, including Swahili for the Kenya rollout and Sinhala and Tamil for the Sri Lanka rollout.

## 13. Intended Users

| User                                    | Primary Responsibilities                                                       |
| --------------------------------------- | ------------------------------------------------------------------------------ |
| **Facility reporting officers**         | Capture the monthly dataset(s) commissioned for the facility.                   |
| **Facility clinical leads**             | Review facility-level dashboards and act on cataract audit outcomes.            |
| **District / county eye care coordinators** | Validate facility submissions and analyse subnational indicators.           |
| **National eye health programme managers** | Monitor ECIM indicators, dashboards and progress against eCSC / eREC targets. |
| **Planning departments**                | Resource allocation across facilities and workflow modules.                     |
| **Development partners**                | Programme monitoring and reporting against international frameworks.            |

User groups

The toolkit ships three user groups. They are supplied empty; assigning users to them is part of implementation.

| User Group              | Dashboard and visualisations | Metadata        | Data              |
| ----------------------- | ---------------------------- | --------------- | ----------------- |
| **EC Admin**            | View and edit                | View and edit   | No access         |
| **EC Access**           | View                         | View            | View              |
| **EC Data Capture**     | View                         | View            | Capture and view  |

The separation is deliberate: the group that may change the configuration is not the group that may change the data. Note that the dashboard is not publicly visible — a user who belongs to none of these groups will not see it at all, so adding users to a group is one of the first steps after installation.

## 14. Adapting the toolkit

The toolkit is a starting point. Implementations are expected to change it, and the design anticipates the most common changes:

* **Removing what does not apply.** A facility that does not perform cataract surgery is commissioned onto the Utilisation, Referral, Refractive Error and Assistive Products modules only. The Cataract Surgery dataset is left aside at that facility, and indicators that depend on it will return no values for it, which is the intended behaviour.
* **Adding national measures.** New data elements, sections and indicators can be added alongside the toolkit's own. Following the same naming and disaggregation patterns keeps the package coherent and makes future upgrades easier to apply.
* **Adjusting disaggregation lists.** The reasons for pre-operative drop-out, the surgeon type list, the referral source list and the insurance / payment list are all configuration points. Kenya's nine-reason drop-out list and four-type surgeon list are shipped as defaults because they informed the reference package; a country with a different workforce or referral structure will replace them.
* **Age bands.** The age band structure is a category combination that can be edited without changing the data elements it is applied to.
* **Localising language.** Names and translations can be replaced with national terminology without affecting how indicators are calculated.
* **Pairing with the case-based tracker.** For facilities that need case-level eye care data — cataract audit at surgical level, patient-level AP provision history — the companion tracker reference design can be deployed alongside the aggregate package; the tracker and the aggregate datasets share the same indicator definitions where they overlap.

What the toolkit does not include, and does not attempt to provide: an organisation unit hierarchy, population estimates, validation rules, data quality thresholds, or any national target values. These belong to the implementation, not to a reference package.
