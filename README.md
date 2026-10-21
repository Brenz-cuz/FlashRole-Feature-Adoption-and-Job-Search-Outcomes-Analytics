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

