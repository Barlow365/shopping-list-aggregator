# EXECUTION PLAN

**🔍 ZOOM: PRODUCT_MAP → Technical Implementation**

This document maps PRODUCT_MAP nodes to technical implementation phases.

## Phase Definitions

- **PHASE 0** = Scaffolding, routes, stubs, feature flags (no real functionality)
- **PHASE 1** = Manual workflow implementation (MVP proof, no AI/automation)
- **PHASE 2** = Automation, intelligence, advanced features (post-MVP)

## Implementation Matrix

| PRODUCT_MAP Node | Phase 0 (Stub) | Phase 1 (Manual MVP) | Phase 2 (Automation) | Priority |
|------------------|----------------|----------------------|----------------------|----------|
| **CAPTURE** | | | | |
| Intake > Quick Add | Route + input field | Quick add with tags | Voice input | P0 |
| Intake > AID | Modal stub | Manual entry only | Vision API + suggestions | P1 |
| Tags > Store | Dropdown stub | Manual select from list | Smart store suggestions | P0 |
| Tags > Urgency | Checkbox stub | Manual high/med/low | Auto-detect from keywords | P1 |
| Tags > Budget | Field stub | Manual $ entry | Auto budget categories | P1 |
| Tags > Person | Field stub | Manual name entry | Household member picker | P2 |
| Tags > Category | Dropdown stub | Manual category select | Auto-categorize | P1 |
| **MEMORY** | | | | |
| Item Pool > List | Route + empty state | Display items, CRUD | Smart grouping | P0 |
| Item Pool > Edit | Edit form stub | Edit item + tags inline | Bulk edit | P0 |
| Item Pool > Delete | Delete button stub | Soft delete with undo | Archive instead | P0 |
| Price Memory > Display | Static $0 | Show last known price | Show price trend | P1 |
| **VIEW** | | | | |
| View > Generate | Button stub | Filter by tags manually | Smart view recommendations | P1 |
| View > Budget Warning | Static text | Manual threshold check | Auto threshold + alerts | P0 |
| View > Priority Sort | Random order | Manual drag-to-reorder | Auto-sort by urgency | P1 |
| **TRIP** | | | | |
| Trip > Select Store | Dropdown stub | Manual store picker | Eligible items filter | P0 |
| Trip > Generate | Button stub | Manual item selection | Auto-suggest items by store | P0 |
| Trip > Shop Mode | Route stub | Offline checklist | Location-aware aisle order | P0 |
| Trip > Confirm/Skip | Buttons stub | Tap confirm/skip | Swipe gestures | P0 |
| Trip > Offline | Feature flag | Service worker cache | Full offline CRUD | P1 |
| **CLOSE LOOP** | | | | |
| Receipt > Upload | Upload stub | Manual file upload | Camera + live capture | P1 |
| Receipt > Match | List stub | Manual item matching | OCR + auto-match | P2 |
| Receipt > Update Prices | Button stub | Manual price entry | Auto-extract from receipt | P1 |
| **GROUP (Phase 2)** | | | | |
| Group > Create | - | - | Group creation flow | P2 |
| Group > Share Items | - | - | Share to group | P2 |
| Group > Permissions | - | - | Role-based access | P2 |

## Technical Dependencies

### Foundation (Must Build First - Phase 0)
1. **Auth system** (login, signup, session management)
2. **Database migrations** (items, tags, trips, receipts tables)
3. **API scaffolding** (REST endpoints stubbed)
4. **Frontend routing** (all routes exist, show stubs)
5. **State management setup** (Redux/Zustand configured)

### Core Features (Phase 1 - MVP)
**Items → Tags**: Item CRUD must exist before tagging
**Tags → Views**: Tags must exist before views can filter
**Views → Trips**: Views must work before trip generation
**Trips → Receipts**: Trips must exist before receipt matching

Can parallelize:
- Item capture + Tag UI (different routes)
- View generation + Trip start (no dependency)
- Shop mode + Receipt upload (different flows)

### Advanced Features (Phase 2 - Post-MVP)
**AID**: Requires external API (Google Shopping / Amazon Product Advertising API)
**OCR**: Requires OCR service (Google Vision API / AWS Textract)
**Groups**: Requires permission system, real-time sync

## Phase Exit Criteria

### Phase 0 Exit (Scaffolding Complete)
- [ ] All routes exist and render (can be empty states)
- [ ] Database schema deployed
- [ ] API endpoints return 200 (can be empty responses)
- [ ] Auth flow works (login → dashboard)
- [ ] Feature flags configured
- [ ] CI/CD pipeline running
- **Deliverable**: Deployable app that does nothing

### Phase 1 Exit (MVP Complete)
- [ ] User can add items with tags manually
- [ ] User can view "Things you need" (item pool)
- [ ] User can generate views by tags
- [ ] User can see budget warnings
- [ ] User can start trip by selecting store
- [ ] User can complete trip in shop mode (offline)
- [ ] User can upload receipt and match items manually
- [ ] Price memory updates after receipt match
- [ ] 10 beta users complete full loop (capture → trip → receipt)
- **Deliverable**: Functional MVP proving the loop works

### Phase 2 Exit (Automation Complete)
- [ ] AID suggests products from external API
- [ ] OCR extracts items from receipts automatically
- [ ] Auto-categorization works with 80%+ accuracy
- [ ] Offline mode supports full CRUD
- [ ] Groups allow multi-user collaboration
- [ ] Analytics show users return 2+ times per week
- **Deliverable**: Polished product ready for launch

## API Dependency Map

### Phase 1 (No External APIs)
All functionality is manual, no third-party services required.

### Phase 2 (External APIs Required)
| Feature | API | Cost | Rate Limit |
|---------|-----|------|------------|
| AID Product Search | Google Shopping API | $5 per 1000 queries | 10 req/sec |
| Receipt OCR | Google Vision API | $1.50 per 1000 images | No limit |
| Voice Input | Web Speech API | Free (browser native) | N/A |

## Rollout Strategy

### Phase 1: Private Beta (Week 1-8)
- 10 hand-picked users (friends/family)
- Manual onboarding + support
- Weekly feedback sessions
- Iterate on UX pain points

### Phase 2: Limited Beta (Week 9-12)
- 100 users (waitlist)
- Self-service onboarding
- In-app feedback widget
- Monitor retention metrics

### Phase 3: Public Launch (Week 13+)
- Open registration
- Marketing push
- Premium tier consideration

## Risk Mitigation

| Risk | Impact | Mitigation |
|------|--------|------------|
| Users don't close receipt loop | High | Gamify: "Complete trip" badge |
| Offline sync conflicts | Medium | Last-write-wins + manual merge UI |
| Price memory inaccurate | Low | Allow manual price override |
| Groups too complex | High | Defer to Phase 2, validate need first |
| AID API costs too high | Medium | Cache results, limit queries per user |

## Open Questions (Requires Product Decision)

| Question | Owner | Blocking Phase | Decision Deadline |
|----------|-------|----------------|-------------------|
| Is budget per-list, per-trip, or per-month? | Product | Phase 1 | Week 1 |
| Do groups share item pool or just views? | Product | Phase 2 | Week 8 |
| Should we support multiple stores per trip? | Product | Phase 1 | Week 2 |
| What's the free tier limit? (items, trips, storage) | Business | Phase 2 | Week 10 |

## Phase Timing (Estimated)

| Phase | Duration | Effort | Team Size |
|-------|----------|--------|-----------|
| Phase 0 | 2 weeks | 80 hours | 2 devs |
| Phase 1 | 6 weeks | 240 hours | 2 devs |
| Phase 2 | 4 weeks | 160 hours | 2 devs |
| **Total** | **12 weeks** | **480 hours** | **2 devs** |

---

**Next Steps**: Expand ARCHITECTURE.md with detailed schema, API contracts, and tech stack specifications.
