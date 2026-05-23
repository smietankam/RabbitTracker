---
project: "RabbitTracker"
context_type: greenfield
created: 2026-05-23
updated: 2026-05-23
checkpoint:
  current_phase: 8
  phases_completed: [1, 2, 3, 4, 5, 6, 7]

  gray_areas_resolved:
    - topic: "pain type"
      decision: "decision paralysis mid-session + format mismatch on re-entry; doom-scroll replacement for spare time"
    - topic: "primary persona"
      decision: "students and learners with divergent thinking patterns (ADHD-spectrum)"
    - topic: "core insight"
      decision: "context collapse — saved links die because they return as walls of text, not scrollable cards"
  frs_drafted: 5
  quality_check_status: accepted
---

## Vision & Problem Statement

Knowledge workers with divergent thinking patterns — students and learners who easily rabbit-hole — lose their primary task when a tangential concept surfaces mid-session. The cost is task abandonment: the original learning goal gets delayed or dropped entirely. No existing "save for later" tool prevents this because saved links suffer context collapse — they pile up unread, returning as walls of text that feel like homework rather than learning.

The insight: the anxiety driving rabbit-holing is fear of forgetting, not genuine urgency. An async feed that resurfaces captured topics as bite-sized scrollable cards removes that fear — replacing the "explore it now or lose it" compulsion with confidence that the topic will return. The product competes with doom-scrolling for spare-time attention and delivers learning instead.

## User & Persona

**Primary persona: The Divergent Learner**

A student or knowledge worker with a diagnosed or self-identified divergent thinking pattern (commonly ADHD) who is working toward a defined learning goal — completing a course, studying for an exam, finishing a research task. They are mid-session, encounter an unfamiliar concept, and feel the pull to explore it immediately.

Their pattern: opening tabs that spawn more tabs, losing the thread of the original goal. They manage their pattern actively and would use a dedicated tool to park the urge to explore now.

The moment they reach for this product: mid-study, they see an unfamiliar term or concept. The trigger: "I should understand this — but if I go now, I'll lose where I was." In their spare time, they scroll social media; this product meets them there and redirects that scrolling to the topics they parked.

## Access Control

Multi-user web app. Users authenticate via email + password or OAuth (exact providers TBD). Flat role model — every authenticated user has identical capabilities. No admin role in MVP. Feed content generated for a given user is not visible to any other user, even on overlapping topics (explicit privacy commitment from original notes).

## Success Criteria

### Primary
- User submits a keyword or topic mid-study and later scrolls through a personalized feed of bite-sized posts on that topic in an Instagram-like format, without disrupting their original task.

### Secondary
- User returns to scroll the feed on their own — no reminder needed — signalling the app earned a re-engagement habit rather than a one-time visit.

### Guardrails
- Feed completeness: at least 5 posts generated per topic for ≥ 80% of submitted keywords (the product fails below this threshold regardless of UX quality).

## Timeline acknowledgment
Acknowledged on 2026-05-23: 4–6 week MVP (using 5 weeks as representative estimate) requires sustained dedication across evenings/weekends; user accepted the sustained-effort cost explicitly.

## User Stories

### US-01: User parks a rabbit-hole topic mid-study

- **Given** a logged-in user who is mid-study and encounters an unfamiliar concept
- **When** they submit a keyword or topic to RabbitTracker
- **Then** the app accepts the submission without blocking the user, and asynchronously prepares a feed of bite-sized posts on that topic

#### Acceptance Criteria
- Submission completes in under 2 seconds from the user's perspective (the app parks the topic; generation is async)
- Submitting the same topic a second time is a no-op (no duplicate feed created)
- At least 5 posts are generated for ≥ 80% of submitted topics

### US-02: User scrolls their learning feed in spare time

- **Given** a logged-in user whose feed has at least one post ready
- **When** they open the app and start scrolling
- **Then** they see posts in Instagram-like format, with unseen posts shown first; after all posts are seen, the feed loops from the first post

#### Acceptance Criteria
- Each post is self-contained and comprehensible without reading adjacent posts
- Session state is persisted: returning to the app resumes from where the user left off

## Functional Requirements

### Authentication
- FR-001: User can register an account. Priority: must-have
  > Socrates: No counter-argument accepted — registration is required to isolate feed content per user and enable persistent state. FR stands.

- FR-002: User can log in and log out. Priority: must-have
  > Socrates: No counter-argument — login/logout is baseline auth hygiene. FR stands.

### Topic Submission
- FR-003: User can submit a keyword or topic to their personal queue. Priority: must-have
  > Socrates: Two valid counter-arguments raised. (1) Free-form input may produce unpredictable feed quality for obscure or malformed keywords — risk accepted for MVP, noted as a content generation challenge. (2) Duplicate submission handling is in-scope: submitting an existing topic is a no-op.

### Feed
- FR-004: User can scroll through their personalized feed of bite-sized posts in an Instagram-like format. Priority: must-have
  > Socrates: Counter-arguments raised: (1) a card-deck format might be more active/learning-appropriate than a passive scroll; (2) presenting advanced topics in an entertaining, self-contained way that also builds a cumulative bigger picture is a genuine design challenge. FR stands — familiar scroll format is core to the doom-scroll replacement. Open question recorded: post format and content design strategy.

- FR-005: User's feed progresses circularly — each session begins with unseen posts; if all posts have been seen, the feed loops from the first post. Priority: must-have
  > Socrates: Counter-argument raised: circular ordering doesn't support jumping to a specific post or re-reading on demand. Accepted limitation for MVP — no in-feed search or jump capability. FR stands; on-demand re-reading is a future version feature.

## Business Logic

Given a submitted keyword or topic, RabbitTracker decomposes it into a sequence of self-contained, bite-sized insights that together build understanding of the subject.

The rule consumes a single user-supplied keyword or topic as input. It produces a fixed set of posts — generated once at submission time and not updated thereafter. Each post must be comprehensible without reading adjacent posts, yet the sequence as a whole must build a progressively fuller picture of the topic. The success threshold is ≥ 5 posts for at least 80% of submitted keywords.

The user encounters the rule's output in the feed: posts appear asynchronously after submission, and the user scrolls through them in whatever spare session follows.

## Non-Functional Requirements

- A user sees acknowledgement of a topic submission within 2 seconds; post generation continues asynchronously after acknowledgement.
- Each post in the feed loads in under 1 second on a standard connection.
- The product remains usable on the latest two major versions of Chrome, Firefox, Safari, and Edge.

## Non-Goals

- **No post rating or liking** — posts are consumed passively; no rating system or like count in MVP.
- **No cross-user feed sharing** — feeds generated for one user are never exposed to another, even when topics overlap. Sharing is a post-MVP concern.
- **No content without submitted topics** — the app provides no posts if the user has not submitted any keywords. Empty queue = empty feed.
- **No quiz or active recall mechanics** — MVP is passive consumption only; no spaced repetition, no verification questions, no active learning methods.
- **No native mobile or desktop app** — web interface only; no iOS, Android, Electron, or PWA install flow in MVP.
- **No web search or external data ingestion** — the app does not crawl URLs, accept uploaded documents, or fetch external links. Content is generated from internal knowledge only.
- **No in-feed search or on-demand post navigation** — the feed is circular and sequential; users cannot jump to a specific post or search within the feed in MVP.

## Quality cross-check

Run on 2026-05-23. All elements present. Status: accepted.

- Access Control: present
- Business Logic (one-sentence rule): present
- Project artifacts: present
- Timeline-cost acknowledgement: present (5 weeks, user accepted 2026-05-23)
- Non-Goals: present (7 entries)

## Open Questions

1. **Post format and content design strategy** — how should a post be structured to be both self-contained (comprehensible alone) and part of a cumulative arc that builds a bigger picture of the topic? Does the app enforce a format template, or is format part of the generation rule? Owner: user. Block: yes for UX design and content generation implementation.

2. **Content generation mechanism** — how does the decomposition rule produce posts? The PRD intentionally does not name the implementation (e.g., LLM), but the approach must support the 80% / 5-posts success threshold. Owner: tech-stack-selector (downstream). Block: no for PRD; yes for implementation.

3. **OAuth providers** — which OAuth providers (Google, GitHub, etc.) are in scope for MVP registration? Owner: user. Block: no for PRD; yes for auth implementation.
