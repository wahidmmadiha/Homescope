# Homescope
comprehensive real estate platform with a full dashboard featuring separate experiences for home buyers and real estate agents. The platform helps buyers search for homes, save favorite properties and communicate directly with listing agents, while giving agents tools to manage listings, respond to inquiries, and track their property performance.

Over one weekend, I learned how to use Floot and Perfai Security over one weekend and built a real estate app.

The entire application started with one detailed prompt inside Floot.
The impressive part is that I was able to build a feature-rich, role-based platform that resembles a production-ready real estate marketplace in just a weekend. That was exactly why I wanted to build it.

HomeScope is a role-based real estate platform designed around two types of users:
Home Buyers
Real Estate Agents
Home buyers can use the platform to:
Search homes using advanced filters.
Explore listings on an interactive map.
Save favorite properties into custom collections.
Compare neighborhoods using demographic and market data.
Calculate mortgage affordability.
Schedule home tours.
Contact listing agents through built-in messaging.
Receive personalized property recommendations.
Track recently viewed homes and saved searches from a personalized dashboard.
Real estate agents receive a different dashboard and can:
Create new property listings.
Upload photos and manage listing details.
Edit pricing and property information.
Mark listings as sold or pending.
Respond to buyer inquiries.
Approve or reschedule home tours.
Monitor listing performance, including views, saves, and inquiries.
Manage conversations with potential buyers.
I wanted to explore what a modern real estate platform could look like when AI handled much of the engineering work.
I also wanted to test what building a software company looks like when the first version of the product can be generated in days rather than months.

My initial question was:
"How far can a non-traditional development workflow take a real product idea?"

Real estate felt like a useful category for the experiment because even a relatively focused platform introduces meaningful complexity.
There are multiple user roles, each requiring different permissions, workflows, and private data. Buyers should only access their own saved homes, tours, and conversations, while agents should only manage their own listings and customer inquiries. Proper role separation is essential for both usability and security.
Each individual feature is understandable. The challenge comes from connecting them into one coherent, trustworthy application. External APIs can fail, map requests can time out, location permissions can be denied, and listing data constantly changes.
I was less interested in whether Floot could generate attractive dashboards. I wanted to see whether it could take a broad product specification and turn it into a functional system with authentication, workflows, integrations, and meaningful role separation that could realistically serve as the foundation of a real startup.

My Tech Stack
The application was built with:
Floot
TypeScript
React / Next.js
Tailwind CSS
NextAuth
PostgreSQL with Prisma
Google Maps Platform
RentCast API
PerfAI Security
Floot handled most of the initial application structure, component creation, and integration work.
NextAuth provided secure authentication while PostgreSQL and Prisma managed persistent application data. Google Maps powered interactive property maps, and RentCast supplied listing and market information.

Finally, I used PerfAI Security to validate the application before launch. As AI-generated code becomes more common, security can no longer be treated as an afterthought. Automated security testing helped identify potential vulnerabilities early, ensuring the platform was not only functional and feature-rich but also secure enough for real users

How? One Prompt
I began with one long prompt describing the product, users, pages, design direction, workflows, and technical expectations.
The goal was to provide enough context that Floot could generate a connected application rather than building isolated screens one at a time.

First Iteration
The initial generation created most of the application's overall structure. Floot established a consistent design system throughout the platform.
Typography, spacing, buttons, cards, tables, forms, dashboards, and navigation all felt like parts of the same application rather than independently generated pages.
The role separation immediately made the platform feel much closer to a launch-ready real estate product than a generic dashboard template with different labels.

What Still Required Iteration
A detailed initial prompt created a strong foundation, but it did not produce a finished company in one attempt.
I continued working with Floot to refine workflows, connect generated components, improve user experience, resolve edge cases, and make the application behave more like the product I originally envisioned.
Some areas required considerably more attention.
Role-Based Experiences
Showing different dashboards was only the beginning.
Navigation, permissions, listings, saved properties, messages, scheduled tours, and database records all needed to reflect the current user's role.
A home buyer should never see agent management tools.
A real estate agent should never have access to another agent's listings or a buyer's private dashboard data.
The visual separation was straightforward. Making that separation meaningful throughout the application required much more iteration.
Third-Party Integrations
The property search APIs and Google Maps integrations also required proper loading, empty, and error states.
A production-quality application cannot assume every API request succeeds. Users need graceful fallbacks whenever external services become unavailable.

Results
By the end of the weekend, HomeScope had become a functional full-stack prototype.
Users could create an account, choose either the buyer or agent role, and immediately access their own dashboard.
Data persisted across sessions, and the application could be published directly from Floot to a live URL. From a product demonstration perspective, it looked complete.
The screens connected naturally. The primary workflows functioned correctly. The application featured responsive layouts, realistic property data, interactive maps, personalized dashboards, and enough polish to confidently demonstrate to others.
That is usually the point where a vibe-coded application feels ready to ship.

"Published" and "Ready to Release" Are Different States
Publishing the application was simple.
Floot generated a live deployment within minutes.
However, deployment only proves the application can run on the internet.
It does not prove that authorization rules cannot be bypassed.
This distinction matters for every application, but especially for a real estate platform.
HomeScope contains user accounts, saved properties, private messages, scheduled tours, agent dashboards, and listing management workflows. Even with demo data, these features create the same trust boundaries a production application must protect.
Before considering the application release-ready, I ran the deployed version through PerfAI Security.

The Release-Readiness Check
Before considering HomeScope release-ready, I ran the live deployment through PerfAI Security.
PerfAI Security tested the deployed application rather than reviewing only the source code or generated interface.
I submitted the live URL, and the platform mapped the application's accessible functionality, routes, requests, and user workflows before testing for security weaknesses.
This wasn't intended to become another development phase. It was simply part of the release checklist, alongside verifying mobile responsiveness, checking integrations, and confirming that core user flows worked correctly.
The scan identified four security issues, including two high-severity findings and two medium-severity findings, with an estimated $3,100 in potential bug bounty exposure avoided.

Sensitive Data in Error & Metadata Responses (High Severity)
The most severe finding involved sensitive information appearing in error and metadata responses.
Applications frequently expose more information than intended when errors occur. In this case, internal details were being returned through responses that users should never need to see.
Information leakage can provide attackers with valuable insight into application structure, underlying services, or implementation details that make future attacks easier.
The issue received a CVSS score of 7.5 and represented the largest potential security impact identified during testing.

Missing Rate Limiting Protections (High Severity)
The second high-severity finding involved missing rate-limit protections on application requests.
Without appropriate request limits, attackers can repeatedly access endpoints, automate abusive behavior, or attempt credential-based attacks at a much larger scale.
While the application functioned normally during testing, this issue highlighted an important distinction between a working application and a resilient one.

Enumerable Resource IDs (Medium Severity)
PerfAI also detected resource identifiers that could potentially be predicted or enumerated.
When resource IDs follow predictable patterns, attackers may be able to discover records they were never intended to access by systematically guessing identifiers.
Even when authorization controls exist, predictable identifiers increase the attack surface and create unnecessary risk.

Broken Pagination Limits (Medium Severity)
The final issue involved pagination controls that did not properly enforce limits.
Pagination seems like a small feature, but improperly enforced limits can allow excessive data retrieval, increase infrastructure costs, and create opportunities for abuse.
Production systems should always validate pagination parameters rather than trusting client requests.
None of the issues were obvious from looking at the finished application.
The design looked polished. The buyer and agent dashboards were properly separated. Authentication worked. Property search, messaging, and tour scheduling behaved as expected.
From a normal user's perspective, everything appeared ready for launch.
That was precisely why the security scan proved valuable.
It highlighted issues that would never have been discovered through visual inspection alone and demonstrated the difference between a convincing prototype and a product approaching production readiness.
This version ties directly to the actual PerfAI report and sounds much more realistic than generic examples like broken access control that weren't actually found.
