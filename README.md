# HomeScope — A Real Estate Platform Built in One Weekend with Floot + PerfAI Security

I learned **Floot** and **PerfAI Security** over one weekend and used them to build a comprehensive real estate platform — complete with a full dashboard offering separate experiences for home buyers and real estate agents.

HomeScope helps buyers search for homes, compare neighborhoods, save favorite properties, schedule home tours, receive personalized recommendations, and communicate directly with listing agents — while giving agents powerful tools to manage listings, respond to inquiries, and track property performance.

The entire application started with **one detailed prompt** inside Floot. The impressive part: a feature-rich, role-based platform resembling a production-ready real estate marketplace, built in a single weekend.


## What Is HomeScope?

HomeScope is a role-based real estate platform designed around two types of users: **Home Buyers** and **Real Estate Agents**.

### Home Buyers can:
- Search homes using advanced filters
- Explore listings on an interactive map
- Save favorite properties into custom collections
- Compare neighborhoods using demographic and market data
- Calculate mortgage affordability
- Schedule home tours
- Contact listing agents through built-in messaging
- Receive personalized property recommendations
- Track recently viewed homes and saved searches from a personalized dashboard

### Real Estate Agents can:
- Create new property listings
- Upload photos and manage listing details
- Edit pricing and property information
- Mark listings as sold or pending
- Respond to buyer inquiries
- Approve or reschedule home tours
- Monitor listing performance (views, saves, inquiries)
- Manage conversations with potential buyers

---

## Why I Built This

I wanted to explore what a modern real estate platform could look like when AI handled much of the engineering work — and what building a software company looks like when the first version of a product can be generated in days rather than months.

My initial question was:

> "How far can a non-traditional development workflow take a real product idea?"

Real estate was a useful test case because even a focused platform introduces meaningful complexity:

- Multiple user roles, each requiring different permissions, workflows, and private data
- Buyers should only access their own saved homes, tours, and conversations
- Agents should only manage their own listings and customer inquiries
- Proper role separation is essential for both usability and security

Each individual feature is understandable on its own. The real challenge is connecting them into one coherent, trustworthy application — especially when external APIs can fail, map requests can time out, location permissions can be denied, and listing data constantly changes.

I wasn't just interested in whether Floot could generate attractive dashboards. I wanted to see whether it could take a broad product specification and turn it into a functional system with authentication, workflows, integrations, and meaningful role separation — the kind of foundation a real startup could be built on.

---

## Tech Stack

- **Floot** — application scaffolding, component generation, and integration work
- **TypeScript**
- **React / Next.js**
- **Tailwind CSS**
- **NextAuth** — authentication
- **PostgreSQL + Prisma** — persistent data
- **Google Maps Platform** — interactive property maps
- **RentCast API** — listing and market data
- **PerfAI Security** — pre-launch security validation

Floot handled most of the initial application structure, component creation, and integration work. NextAuth provided secure authentication while PostgreSQL and Prisma managed persistent application data. Google Maps powered interactive property maps, and RentCast supplied listing and market information.

Finally, I used **PerfAI Security** to validate the application before launch. As AI-generated code becomes more common, security can no longer be treated as an afterthought — automated security testing helped identify potential vulnerabilities early, ensuring the platform was not only functional and feature-rich, but secure enough for real users.

---

## How It Was Built: One Prompt

I began with one long prompt describing the product, users, pages, design direction, workflows, and technical expectations. The goal was to provide enough context for Floot to generate a **connected application** rather than isolated screens built one at a time.

### First Iteration

The initial generation created most of the application's overall structure. Floot established a consistent design system throughout — typography, spacing, buttons, cards, tables, forms, dashboards, and navigation all felt like parts of the same application rather than independently generated pages.

The role separation immediately made the platform feel much closer to a launch-ready real estate product than a generic dashboard template with different labels.

### What Still Required Iteration

A detailed initial prompt created a strong foundation, but it didn't produce a finished company in one attempt. I continued working with Floot to refine workflows, connect generated components, improve UX, resolve edge cases, and make the application behave more like the product I originally envisioned.

**Role-Based Experiences**
Showing different dashboards was only the beginning. Navigation, permissions, listings, saved properties, messages, scheduled tours, and database records all needed to reflect the current user's role. A home buyer should never see agent management tools, and an agent should never access another agent's listings or a buyer's private dashboard data. The visual separation was straightforward — making that separation *meaningful* throughout the application required much more iteration.

**Third-Party Integrations**
The property search APIs and Google Maps integrations required proper loading, empty, and error states. A production-quality application can't assume every API request succeeds — users need graceful fallbacks whenever external services become unavailable.

---

## Results

By the end of the weekend, HomeScope had become a functional full-stack prototype. Users could create an account, choose either the buyer or agent role, and immediately access their own dashboard. Data persisted across sessions, and the application could be published directly from Floot to a live URL.

From a product demonstration perspective, it looked complete: screens connected naturally, primary workflows functioned correctly, and the app featured responsive layouts, realistic property data, interactive maps, personalized dashboards, and enough polish to confidently demonstrate to others.

That's usually the point where a vibe-coded application *feels* ready to ship.

---

## "Published" and "Ready to Release" Are Different States

Publishing the application was simple — Floot generated a live deployment within minutes. But deployment only proves the application can run on the internet. It does **not** prove that authorization rules can't be bypassed.

This distinction matters for every application, but especially for a real estate platform. HomeScope contains user accounts, saved properties, private messages, scheduled tours, agent dashboards, and listing management workflows. Even with demo data, these features create the same trust boundaries a production application must protect.

Before considering the application release-ready, I ran the deployed version through **PerfAI Security**.

---

## The Release-Readiness Check

PerfAI Security tested the *deployed application*, not just the source code or generated interface. I submitted the live URL, and the platform mapped the application's accessible functionality, routes, requests, and user workflows before testing for security weaknesses.

This wasn't meant to be another development phase — it was simply part of the release checklist, alongside verifying mobile responsiveness, checking integrations, and confirming that core user flows worked correctly.

The scan identified **4 security issues** — 2 high-severity and 2 medium-severity — with an estimated **$3,100** in potential bug bounty exposure avoided.

| Severity | Finding |
|----------|---------|
| High (CVSS 7.5) | Sensitive data exposed in error & metadata responses |
| High | Missing rate-limiting protections |
| Medium | Enumerable resource IDs |
| Medium | Broken pagination limits |

### Sensitive Data in Error & Metadata Responses (High — CVSS 7.5)
The most severe finding involved sensitive information appearing in error and metadata responses. Applications frequently expose more information than intended when errors occur — in this case, internal details were being returned through responses users should never need to see. This kind of information leakage can give attackers valuable insight into application structure, underlying services, or implementation details that make future attacks easier.

### Missing Rate Limiting Protections (High)
Without appropriate request limits, attackers can repeatedly access endpoints, automate abusive behavior, or attempt credential-based attacks at scale. The application functioned normally during testing, but this issue highlighted an important distinction between a *working* application and a *resilient* one.

### Enumerable Resource IDs (Medium)
PerfAI detected resource identifiers that could potentially be predicted or enumerated. When resource IDs follow predictable patterns, attackers may discover records they were never intended to access simply by guessing identifiers. Even with authorization controls in place, predictable IDs increase the attack surface unnecessarily.

### Broken Pagination Limits (Medium)
Pagination controls did not properly enforce limits. This seems like a small feature, but improperly enforced limits can allow excessive data retrieval, increase infrastructure costs, and create opportunities for abuse. Production systems should always validate pagination parameters rather than trusting client requests.

## Takeaway

None of these issues were obvious from looking at the finished application. The design looked polished. Buyer and agent dashboards were properly separated. Authentication worked. Property search, messaging, and tour scheduling all behaved as expected — from a normal user's perspective, everything appeared ready for launch.

That's exactly why the security scan proved valuable. It surfaced issues that would never have been found through visual inspection alone, and it demonstrated the real difference between **a convincing prototype** and **a product approaching production readiness**.
