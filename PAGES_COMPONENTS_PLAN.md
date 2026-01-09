================================================================================
THE ONE LIST SYSTEM
Capture  Decide  Shop  Close Loop  Scale Up
================================================================================

# Pages and Components Plan (MVP with Phased Execution)

This plan enumerates key pages, minimal components, and shared state/data needs.

---

## Pages and Minimal Components

### Marketing

- `/` Home
  - Components: HeroValueProp, CTAButtons, SocialProofStrip, FooterLinks
- `/how-it-works`
  - Components: StepCard, CTAInline
- `/features`
  - Components: FeatureGrid, FeatureCard
- `/pricing`
  - Components: PricingCard, FAQStub
- `/privacy`, `/terms`
  - Components: LegalText

### Auth + Onboarding

- `/login`
  - Components: AuthForm, OAuthPlaceholder
- `/signup`
  - Components: AuthForm
- `/onboarding`
  - Components: GroupCreatePanel, ListCreatePanel, BudgetPrompt, InviteLinkPanel

### App Home

- `/app/home`
  - Components: ActiveListCard, QuickStatsRow, ActivityFeed, ShortcutBar

### Groups

- `/app/groups`
  - Components: GroupList, CreateGroupButton
- `/app/groups/:groupId`
  - Components: GroupHeader, MemberList, PermissionsPanelStub, ActiveListSelector, ArchiveList

### List Hub and Tabs

- `/app/lists/:listId`
  - Components: ListHeader, BudgetRibbon, TabNav, ListSummary

#### Items (Capture)

- `/app/lists/:listId/items`
  - Components: QuickEntryInput, ItemList, ItemRow, ItemDetailSheet
  - Utilities: BulkEditBar, FilterBar

#### Store View (Shop)

- `/app/lists/:listId/stores`
  - Components: StoreChipBar, StoreItemList, UnassignedItemPanel
  - Utilities: AssignToStoreAction

#### Budget (Decide)

- `/app/lists/:listId/budget`
  - Components: BudgetEditor, TotalVsBudget, OverBudgetBanner, TradeoffActions
  - Utilities: PriceMemoryPanel

#### Shop Mode (Shop)

- `/app/lists/:listId/shop`
  - Components: ShopModeHeader, Checklist, PurchaseToggle, OfflineIndicator

#### Receipts (Close the Loop)

- `/app/lists/:listId/receipts`
  - Components: ReceiptUpload, ManualMatchTable, ActualPriceInput, BudgetActualSummary

#### History (Close the Loop)

- `/app/lists/:listId/history`
  - Components: ActivityTimeline, PriceChangeLog

#### Voice (Capture)

- `/app/lists/:listId/voice`
  - Components: VoiceStubPanel, PasteInputFallback

#### Recipes (Decide)

- `/app/lists/:listId/recipes`
  - Components: RecipeList, RecipeCard, AddRecipeForm

#### Recurring (Scale Up)

- `/app/lists/:listId/recurring`
  - Components: RecurringRuleList, AddRecurringRule

#### Delivery (Shop)

- `/app/lists/:listId/delivery`
  - Components: DeliveryStubPanel, ExportListAction

#### Pooling (Scale Up)

- `/app/lists/:listId/pooling`
  - Components: PoolSetupPanel, BuyerAssignmentTable, SplitSummary

#### Upgrades

- `/app/lists/:listId/upgrades`
  - Components: FeatureFlagGrid, PhaseBadge

### Settings and Help

- `/app/settings`
  - Components: ProfileForm, SessionList, MemberManagement
- `/app/help`
  - Components: FeedbackForm, SupportLinks

### System States

- `/system`
  - Components: EmptyState, ErrorState, OfflineBanner, SyncConflictNotice

---

## Shared State and Data Model Needs

### Shared State (Client)

- Active list context (listId, groupId)
- User session and membership
- Feature flags and phase gating
- Filters (store, priority, tag, added-by)
- Offline state and sync status

### Data Models (Server)

- Users (id, email, profile)
- Groups (id, name, members, permissions)
- Lists (id, groupId, name, status: active/archived, budget)
- Items (id, listId, name, qty, priority, storeId, priceEstimate, status, addedBy)
- Stores (id, listId, name, custom)
- Receipts (id, listId, images, lineItems, actualTotal)
- PriceMemory (itemName, lastPaid, lastUpdated)
- ActivityLog (actorId, action, entityId, timestamp)
- FeatureFlags (feature, phase)

---

## Phase Behavior Summary

- Phase 0: route exists with stub UI, no blocking flows
- Phase 1: manual workflows and data capture
- Phase 2: automation and integrations
