# ROADMAP

Business timeline and resource planning for Shopping List Aggregator.

## Timeline Overview

| Phase | Duration | Dates | Team | Deliverable |
|-------|----------|-------|------|-------------|
| Phase 0: Scaffolding | 2 weeks | Week 1-2 | 2 devs | Deployable skeleton |
| Phase 1: MVP Development | 6 weeks | Week 3-8 | 2 devs | Functional MVP |
| Phase 1.5: Beta Testing | 4 weeks | Week 9-12 | 2 devs + 10 users | Validated product |
| Phase 2: Automation | 4 weeks | Week 13-16 | 2 devs | Polished product |
| **Total** | **16 weeks** | **4 months** | **2 devs** | **Public launch** |

---

## Phase 0: Scaffolding (Week 1-2)

### Goals
- Set up development environment
- Deploy database schema
- Scaffold all routes (stubs)
- Implement authentication

### Deliverables
| Item | Owner | Deadline |
|------|-------|----------|
| Database migrations | Backend dev | End of Week 1 |
| Auth system (Lucia) | Backend dev | End of Week 1 |
| Frontend routing (Next.js) | Frontend dev | End of Week 1 |
| API scaffolding (Express) | Backend dev | End of Week 2 |
| CI/CD pipeline | DevOps | End of Week 2 |
| Deployed to staging | Team | End of Week 2 |

### Exit Criteria
- All routes exist (can be empty)
- User can signup/login
- Database schema deployed
- App deployed to staging

---

## Phase 1: MVP Development (Week 3-8)

### Week 3-4: Item Capture + Memory
- Item CRUD with tags
- Item pool display
- Tag filtering UI
- Price memory display

### Week 5-6: Views + Budget
- Generate views by tags
- Budget warning system
- View filters (store, urgency)
- Priority sorting

### Week 7-8: Trips + Receipts
- Trip generation by store
- Shop mode (offline)
- Receipt upload to S3
- Manual receipt matching
- Price memory updates

### Deliverables
| Feature | Owner | Deadline |
|---------|-------|----------|
| Item capture + tags | Frontend + Backend | End of Week 4 |
| Item pool display | Frontend | End of Week 4 |
| View generation | Frontend + Backend | End of Week 6 |
| Budget warnings | Frontend | End of Week 6 |
| Trip generation | Frontend + Backend | End of Week 8 |
| Shop mode (offline) | Frontend | End of Week 8 |
| Receipt upload | Frontend + Backend | End of Week 8 |
| Receipt matching | Frontend | End of Week 8 |

### Exit Criteria
- 10 internal users complete full loop (capture → trip → receipt → price memory)
- No critical bugs
- Offline mode works
- Price memory updates correctly

---

## Phase 1.5: Beta Testing (Week 9-12)

### Goals
- Invite 100 beta users
- Monitor usage and retention
- Fix bugs and iterate on UX
- Validate product-market fit

### Beta User Cohorts
| Cohort | Size | Week | Focus |
|--------|------|------|-------|
| Friends/Family | 10 users | Week 9 | General feedback |
| Power Users | 20 users | Week 10 | Heavy usage patterns |
| Budget-Conscious | 30 users | Week 11 | Budget features |
| Multi-Store Shoppers | 40 users | Week 12 | Cross-store eligibility |

### Metrics to Track
- **Adoption**: % completing first trip
- **Retention**: % returning after 7 days
- **Engagement**: Trips per user per week
- **Loop Closure**: % uploading receipts
- **NPS**: Net Promoter Score

### Exit Criteria
- 100 beta users onboarded
- Retention > 50% at 7 days
- NPS > 40
- No blockers reported

---

## Phase 2: Automation (Week 13-16)

### Goals
- Add AID (Assisted Item Definition)
- Add OCR receipt scanning
- Add voice input
- Improve auto-categorization

### Deliverables
| Feature | Owner | Deadline |
|---------|-------|----------|
| AID integration (Google Shopping API) | Backend dev | End of Week 14 |
| OCR (Google Vision API) | Backend dev | End of Week 15 |
| Voice input (Web Speech API) | Frontend dev | End of Week 16 |
| Auto-categorization | Backend dev | End of Week 16 |

### Exit Criteria
- AID suggests products with 80%+ accuracy
- OCR extracts items with 70%+ accuracy
- Voice input works on Chrome/Safari
- Auto-categorization works with 80%+ accuracy

---

## Phase 3: Groups (Week 17-20) - Optional

### Goals
- Add group creation
- Add group permissions
- Add shared item pools
- Add real-time sync

### Deliverables
- Group CRUD
- Group member management
- Shared item pool
- Real-time updates (WebSocket or Pusher)

### Exit Criteria
- 20 groups created
- Groups complete 10+ shared trips
- No permission bugs

---

## Resource Planning

### Team Composition
- **Frontend Developer**: React + Next.js + TypeScript
- **Backend Developer**: Node.js + Express + PostgreSQL
- **DevOps** (part-time): CI/CD + monitoring
- **Designer** (contractor): Wireframes + UI polish
- **Product Manager** (you): Strategy + user research

### Budget Estimate (Phase 1)

| Category | Cost | Notes |
|----------|------|-------|
| Developer salaries | $40,000 | 2 devs x 2 months x $10k/month |
| Infrastructure (Vercel + Railway) | $200 | Staging + production |
| AWS S3 (receipt storage) | $50 | 10GB storage |
| Monitoring (Sentry + PostHog) | $100 | Free tiers |
| **Total Phase 1** | **$40,350** | |

### Budget Estimate (Phase 2)

| Category | Cost | Notes |
|----------|------|-------|
| Developer salaries | $20,000 | 2 devs x 1 month x $10k/month |
| External APIs (Google Shopping + Vision) | $500 | 10k queries |
| **Total Phase 2** | **$20,500** | |

### Total Budget: $60,850 for Phases 1-2

---

## Risk Management

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Users don't close receipt loop | High | High | Gamify: badges, streaks |
| Offline sync conflicts | Medium | Medium | Last-write-wins + manual merge UI |
| Price memory inaccurate | Low | Medium | Allow manual override |
| AID API costs too high | Medium | High | Cache results, limit queries |
| Groups too complex | High | Low | Defer to Phase 3 if not validated |

---

## Launch Strategy

### Private Beta (Week 9)
- 10 hand-picked users
- Manual onboarding calls
- Weekly feedback sessions

### Limited Beta (Week 10-12)
- 100 users from waitlist
- Self-service onboarding
- In-app feedback widget

### Public Launch (Week 13+)
- Open registration
- Product Hunt launch
- Social media campaign
- Tech blog post

---

## Success Definition

### Phase 1 Success
- 100 beta users
- 50%+ retention at 7 days
- 30%+ close receipt loop
- NPS > 40

### Phase 2 Success
- 1000 active users
- 60%+ retention at 30 days
- 50%+ close receipt loop
- NPS > 50
- Revenue > $1000/month (premium tier)

### Phase 3 Success (Groups)
- 100+ groups created
- 40%+ of users in groups
- Group retention > 70% at 30 days

---

**Next Steps**: See [EXECUTION_PLAN.md](../EXECUTION_PLAN.md) for technical phasing and [MVP_SPEC.md](./MVP_SPEC.md) for MVP rules.
