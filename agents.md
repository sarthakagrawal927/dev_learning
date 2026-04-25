# agents.md — dev_learning

## Purpose
A curated collection of Git submodules aggregating high-quality learning resources, interview prep material, and hiring lists for software engineers — not a buildable application.

## Stack
- Not applicable — this is a pure content/submodule repo with no build system, package.json, or deployable artifact.

## Repo structure
The repo is organized into three top-level categories, each containing multiple Git submodules:

```
hiring/
  international/              — gist: list of international-friendly companies
  no-whiteboards/             — poteto/hiring-without-whiteboards (companies with no whiteboard interviews)
  company_list/               — Kaustubh-Natuskar/moreThanFAANGM (beyond FAANG companies)
  remote_list/                — remoteintech/remote-jobs (remote-friendly companies)

questionnaires/
  javascript-interview-questions/     — sudheerj (JS interview Q&A)
  js-advanced/                        — lydiahallie/javascript-questions (advanced JS)
  reactjs-interview-questions/        — sudheerj (React Q&A)
  frontend-end-interview-handbook/    — yangshun/front-end-interview-handbook
  Front-end-Developer-Interview-Questions/ — h5bp classic frontend questions
  Back-End-Developer-Interview-Questions/  — arialdomartini backend questions
  Security_Engineer_Interview_Questions/   — tadwhitaker security interviews
  test-your-sysadmin-skills/          — trimstray sysadmin exercises
  devops-exercises/                   — bregman-arie DevOps exercises
  system-design/                      — arpitbbhayani system design questions
  grokking-oop/                       — tssovi OOP design interview
  interview-company-wise-problems/    — liquidslr company-specific LC problems

resources/
  my-cs-degree/               — logancyang self-taught CS curriculum
  university/                 — ossu/computer-science open CS degree
  90DaysOfDevOps/             — MichaelCade DevOps 90-day guide
  system-design-resources/    — InterviewReady system design
  system-design/              — karanpratapsingh system design book
  system-design-101/          — ByteByteGoHq system design 101
  system-design-q/            — ashishps1 awesome system design resources
  low-level-design/           — ashishps1 awesome low-level design
  Resources-for-Beginner-Bug-Bounty-Hunters/ — nahamsec bug bounty
  awesome-ai-ml-resources/    — armankhondker AI/ML resources
  interview_help/             — Olshansk/interview general interview prep
  ml-system-design-case-studies/ — Engineer1999 ML system design cases

databases/
  DBMS-Indexology/            — yingjunwu database indexing deep-dive
```

## Key commands
```bash
# Clone with all submodules
git clone --recurse-submodules <repo-url>

# Update all submodules to latest remote HEAD
git submodule update --remote --merge

# Initialize after a plain clone
git submodule update --init --recursive
```

## Architecture notes
- There is no code to build, test, or deploy — all content is Markdown/PDFs inside submodules.
- Each submodule is an independently maintained external repo; changes must be made upstream.
- To add a new resource: `git submodule add <url> <category>/<name>` then commit `.gitmodules` and the new submodule pointer.
- The `databases/` category currently holds only one submodule (DBMS-Indexology); it appears to be the start of a DB-specific section.

## Active context

