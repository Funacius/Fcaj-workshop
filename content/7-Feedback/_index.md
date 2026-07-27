---
title: "Sharing and Feedback"
weight: 7
chapter: false
pre: "<b>7.</b>"
---

# Sharing and Feedback

This section summarizes my personal reflections after participating in the First
Cloud AI Journey internship program and completing the EduCloud Lite project.

## Overall Evaluation

**1. Working Environment**

The internship environment encouraged self-learning and practical project
development. I was able to work independently, research AWS services, test
deployment options, and gradually turn a local application into a public website.
Although I did not communicate with the team very frequently, the program
structure, sample report template, and learning direction gave me a clear path to
follow.

**2. Support from Mentor / Program Direction**

The available guidance helped me understand what needed to be submitted and how
the project should be documented. The most useful support was the structure of
the internship report and workshop template, because it helped me organize the
project into worklog, proposal, blogs, events, workshop, self-assessment, and
feedback sections. This made the final submission clearer and more professional.

**3. Relevance of Work to Academic Major**

The EduCloud Lite project is closely related to my Computer Science major. It
required backend API design, frontend development, database modeling,
authentication, deployment, debugging, and software documentation. These tasks
allowed me to connect university knowledge with real deployment constraints such
as CORS, IAM permissions, environment variables, cloud storage, and production
health checks.

**4. Learning & Skill Development Opportunities**

During the internship, I learned how different AWS services work together in a
full-stack system. I practiced deploying FastAPI with Elastic Beanstalk, hosting
React with Amplify, managing authentication with Cognito, storing secrets in
Parameter Store, delivering private files with S3 and CloudFront, and connecting
the backend to Supabase PostgreSQL. I also improved my ability to troubleshoot
production-only errors instead of only testing locally.

**5. Program Culture & Learning Spirit**

The First Cloud AI Journey program encouraged learning by building. Instead of
only studying AWS theory, I had to produce a working website, document the
architecture, explain design decisions, and prepare evidence for deployment. This
made the learning process more realistic and helped me understand the difference
between a local prototype and a deployable cloud application.

**6. Internship Policies / Benefits**

The biggest benefit of the internship was the opportunity to work on a project
that combined software development and cloud deployment. The flexible learning
process allowed me to spend more time on difficult parts such as authentication,
CloudFront/S3 access, Elastic Beanstalk configuration, and Hugo documentation.

---

## Additional Questions

**What did I find most satisfying during the internship?**

The most satisfying moment was when EduCloud Lite became accessible through a
public Amplify URL and the login, course, instructor, profile, upload, assessment,
and certificate flows worked together. It showed that the project was no longer
only a local demo, but a complete cloud-hosted application.

**What should be improved for future interns?**

It would be helpful to provide a short deployment checklist earlier in the
program, especially for IAM roles, Parameter Store, CORS, CloudFront behaviors,
S3 bucket policies, Cognito configuration, and cleanup. These are the areas where
students can easily lose time if they are new to AWS.

**Would I recommend this internship to a friend?**

Yes. I would recommend it to students who want to understand how software
projects are deployed and operated on cloud infrastructure. The program is
especially useful for students who already know basic web development and want to
learn how to make their application publicly available in a secure and structured
way.

---

## Suggestions & Expectations

- Provide a minimal reference architecture at the beginning of the internship so
  students can understand the target deployment model earlier.
- Encourage students to capture screenshots and write worklog notes every week
  instead of reconstructing evidence near the end.
- Add a short guide on estimating AWS cost and cleaning up resources after the
  submission.
- Include common troubleshooting examples for Cognito login, CORS, CloudFront
  origin behavior, S3 OAC access, and Elastic Beanstalk deployment logs.
- Continue allowing students to choose their own project idea, because building
  a personally meaningful application makes the learning process much more
  engaging.

