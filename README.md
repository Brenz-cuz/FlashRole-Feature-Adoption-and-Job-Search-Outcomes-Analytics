# FlashRole-Feature-Adoption-and-Job-Search-Outcomes-Analytics
Power BI portfolio project: Feature Adoption and Application Outcomes

FlashRole gives job seekers several AI-assisted tools, but which tools do people use, return to and progress with towards interviews and offers?

In this project, I analysed recorded feature engagement and application outcomes, then built a two-page Power BI report to help leadership review onboarding and product priorities. The analysis covers **100,000 registered users, 70,224 applicants and 351,493 applications**.

The main finding: feature adoption varies substantially, but higher scores, linked cover letters, recorded interview preparation and more resume revisions are not consistently associated with better recorded outcomes.

## Dashboard previews

### Feature Adoption

![FlashRole Feature Adoption dashboard showing headline KPIs, feature adoption and repeat use, recorded user journey, and resume revisions](Flashrole%20Feature%20Adoption%20Dashboard.png)

### Application Outcomes

![FlashRole Application Outcomes dashboard comparing interview and offer rates by cover-letter group, job-match bands, ATS bands and interview preparation](Flashrole%20Application%20Outcome%20Dashboard.png)

## Business problem

FlashRole is an AI-assisted job-search platform offering resume building, ATS scoring, job matching, cover-letter generation, mock interviews and scam screening across Free, Premium and Pro plans.

Leadership needed a shared view of feature engagement and progression towards offers. The operational data contained activity and application records, but did not provide a consolidated view of:

- Which features users adopt and use repeatedly.
- How long users take to start using each feature.
- How many users reach recorded signup-to-offer milestones.
- Whether feature use, assessment scores and resume revisions are associated with better application outcomes.

## My role and scope

I worked on the feature adoption and its relationship with interviews and offers. My work covered data preparation, analytical definitions, DAX calculations, dashboard development, interpretation and recommendations.

### Business questions

1. Which features have the highest and lowest adoption and repeat-use rates?
2. How long after signup do adopters first use each feature?
3. How many users reach each recorded journey milestone?
4. How do application outcomes differ with and without linked cover letters?
5. How do interview and offer rates vary across ATS and job-match score bands?
6. How do outcomes differ according to interview preparation recorded by application date?
7. Are more recorded resume revisions associated with higher ATS scores and better outcomes?

The report supports comparisons across Free, Premium and Pro through a Plan Tier slicer.

## Dataset description

The project used a supplied nine-table SQLite operational dataset. Date coverage differs by table; recorded applications span **3 July 2025 to 6 August 2026**.

| Source table | Rows loaded | Main purpose |
|---|---:|---|
| `users` | 100,000 | User profiles, plans and signup dates |
| `resumes` | 165,284 | Resume versions, upload dates and current status |
| `applications` | 351,493 | Applications, user/resume/job links, current outcomes and cover-letter flag |
| `assessments` | 566,777 | ATS, job-match and scam-risk assessments |
| `cover_letters` | 158,536 | Generated letters and application links |
| `interview_sessions` | 45,037 | Mock interview preparation records |
| `job_postings` | 50,000 | Job attributes and company links |
| `companies` | 3,000 | Company attributes |
| `scam_flags` | 2,255 | Posting risk flags|

A derived `Fact_activity` table brings together user-linked activity records for engagement analysis. `Dim_Date` provides calendar fields within the model.

**Tools:** Power BI Desktop, Power Query and DAX, using the supplied SQLite operational data.

## Data cleaning and modelling

The preparation includes:

- Converting signup, upload, application, assessment, cover-letter and preparation dates to Date.
- Assigning numeric types to versions, salary fields, preparation duration, question counts and flags.
- Removing user names and email addresses, and the company-name field, from loaded reporting queries.
- Combining resume uploads, applications, cover letters, interview preparation and user-linked ATS/job-match records into `Fact_activity`.
- Creating activity month and elapsed days/months since signup, excluding records without a user or activity date from the combined activity table.

The model links users to their applications, resumes, cover letters, preparation sessions and activity records. Resumes link to assessments, and job postings link to applications and companies. A disconnected journey-stage table supplies milestone labels for the visual.

## DAX measures and analytical definitions

The calculations distinguish **users** from **applications** so that each rate has a clear denominator.

| Metric | Definition |
|---|---|
| Adoption rate | Distinct users with a recorded feature use / registered users in the selected context |
| Repeat-use rate | Adopters with at least two records of the same activity / adopters of that activity |
| Median days to first use | Median of each adopter's first recorded use minus signup date |
| Applicants with Interview or Offer | Distinct applicants with at least one current Interview/Offer application / distinct applicants |
| Applicants with Offer | Distinct applicants with at least one current Offer application / distinct applicants |
| Offer share of Interview/Offer users | Users with a current Offer / users with a current Interview/Offer application |
| Application interview-or-offer rate | Applications currently Interview/Offer / applications in the comparison group |
| Application offer rate | Applications currently Offer / applications in the comparison group |
| Average ATS score | Application-weighted mean of nonblank ATS scores available by application date |

Example definitions from the supplied DAX library:

```DAX
Total Users =
DISTINCTCOUNT(users[user_id])
```

```DAX
Adoption_Rate =
DIVIDE([Feature_Users], [Total Users])
```

```DAX
Applications Interview Rates =
DIVIDE([Interview Reached Applications], [Total Applications])
```
Calculated columns support the comparisons:

- **ATS score:** matches the application's resume to its ATS assessment, dated on or before application.
- **Job-match score:** matches both resume and job, selecting the latest qualifying assessment dated on or before application.
- **Score bands:** Below 40, 40 to below 60, 60 to below 80 and 80 to 100, with unavailable scores kept separate.
- **Preparation group:** identifies whether the user has a mock interview session dated on or before each application.
- **Recorded revisions:** highest user resume version available by application date, minus one.

[View the DAX measure library](Flashrole%20schema.png).

## Headline KPIs

- **100,000** registered users.
- **70,224** distinct applicants.
- **351,493** applications.
- **33.13%** of applicants have a current Interview or Offer application.
- **9.59%** of applicants have a current Offer application.
- **28.94%** of Interview/Offer users have a current Offer.
- **66.69 / 100** average ATS score available by application.

The user-based percentages above are different from the application-based rates in the comparison charts.

## Key findings

### Feature adoption and repeat use

| Recorded activity | Adoption | Repeat use among adopters | Median days to first use |
|---|---:|---:|---:|
| Resume upload | 100.00% | 50.28% | 73 |
| ATS check | 100.00% | 50.28% | 75 |
| Application | 70.22% | 98.17% | 132.5 |
| Job match | 70.22% | 98.17% | 134 |
| Cover letter | 63.91% | 73.70% | 150 |
| Interview preparation | 24.98% | 55.37% | 53 |

Resume upload and ATS checks have universal recorded coverage, while interview preparation reaches approximately one in four users. Cover-letter use starts latest. Job matching has strong repeat activity among its adopters.

### Recorded user journey

All registered users have resume records; approximately 80,000 have a currently finalised resume. There are 70,224 applicants and 6,733 users with a recorded offer. This gives leadership a view of milestone reach without treating every difference as a verified sequential drop-off.

### Cover letters

Applications with linked letters have an interview-or-offer rate of **7.95%**, versus **8.05%** without linked letters. Offer rates are **2.00%** and **2.02%**, respectively. The recorded rates are similar.

### ATS scores

Interview-or-offer rates remain approximately **8%** across score bands. The 60-to-below-80 band has a rate of **8.05%**, compared with **7.98%** in the 80-to-100 band. Higher ATS scores do not show a consistent outcome advantage in this comparison.

### Job-match scores

The 80-to-100 band has the highest interview-or-offer rate at **8.36%** and offer rate at **2.15%**. However, rates do not rise consistently across all score bands.

### Interview preparation

Applications with preparation recorded by application date have an interview-or-offer rate of **8.08%**, compared with **7.98%** without it. Their offer rates are **1.98%** and **2.02%**. This comparison shows no consistent advantage across both outcomes.

### Resume revisions

| Recorded revisions available by application | Interview-or-offer rate | Offer rate |
|---|---:|---:|
| 0 | 8.03% | 2.03% |
| 1 | 7.95% | 1.99% |
| 2 | 8.03% | 1.97% |

Average ATS scores are also similar across the groups. More recorded revisions are not consistently associated with better scores or outcomes.

## Strategic recommendations

The following are proposed actions for FlashRole, not implemented changes or realised business results.

| Priority | Recommendation | Suggested owner | Metric to monitor |
|---|---|---|---|
| Feature discovery | Test timely prompts for interview preparation and cover-letter generation | Product / UX | Adoption, repeat use and first-use delay |
| Application progression | Review application-to-interview progression by plan and role to identify support needs | Product Analytics | Applicant Interview/Offer and Offer shares |
| Resume quality | Encourage specific content improvements rather than additional versions alone | Resume Product | ATS scores and application outcome rates |
| Score guidance | Pair ATS and match scores with guidance on skills, experience and job requirements | ATS / Matching Product | Outcomes and sample counts by score band |
| Application support | Present cover letters as supporting material and tailor interview practice to role requirements | Career Tools Product | Feature engagement and application outcomes |

## Analytical assumptions

- Findings show associations between recorded feature use and outcomes, rather than causal effects.
- Interview rates include applications currently marked `Interview` or `Offer`. Journey visuals represent recorded milestones rather than complete application histories.
- Scores and interview preparation are matched to records dated on or before application; same-day ordering is unavailable.
- Cover-letter linkage does not confirm submission. Revision groups represent versions available to the user, which may differ from the version submitted.
- Outcome comparisons and average ATS scores are calculated per application. Adoption is measured among registered users; repeat use and first-use timing are measured among feature adopters.

Further definitions and data considerations are documented in the methodology report.


## Repository contents

| Path | Contents |
|---|---|
| `README.md` | Project overview, methodology, findings and recommendations |
| `Flashrole%20Feature%20Adoption%20Dashboard.png` | Feature Adoption dashboard preview |
| `Flashrole%20Application%20Outcome%20Dashboard.png` | Application Outcomes dashboard preview |
| `FlashRole_Methodology_and_Final_Report%20(1).pdf` | Dataset, preparation, definitions and findings |
| `dax-measures/FlashRole_DAX_Measure_Library.xlsx` | Documented DAX measures |

### Explore the project

1. Review the dashboard previews above.
2. Open the [methodology and final report](FlashRole_Methodology_and_Final_Report%20(1).pdf) for preparation and definitions.
4. Review the [DAX library](dax-measures/FlashRole_DAX_Measure_Library.xlsx) for measure logic.

This package contains static previews and supporting documentation. The Power BI file and original database are not included; previews do not provide interactive filtering.

## Conclusion

This project brings feature engagement and job-search outcomes into a shared analytical view. Its main message is that broader feature use, higher assessment scores and more revisions do not automatically correspond to better recorded outcomes. The recommendations focus on useful guidance, timely feature discovery and relevant application support, with metrics to evaluate subsequent changes.

