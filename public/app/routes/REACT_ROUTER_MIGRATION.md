# React Router migration notes (CS-516)

## Completed in this slice

- **`RoutesWrapper.tsx`**: Replaced `CompatRouter` with `unstable_HistoryRouter` backed by a new `LocationService.getRouterHistory()` adapter. An outer v5 `Router` remains for legacy consumers that still import raw `react-router-dom` hooks.
- **`LocationService`**: Added a `@remix-run/router` history adapter so the existing `history@4` service can drive the inner v6 router during the migration.
- **Test wrappers**: `test-utils.tsx`, `TestProvider.tsx`, and the Explore test helper now mirror the app's dual-context router stack.
- **Import cleanup**: Consolidated remaining raw `react-router` imports to `react-router-dom-v5-compat`.
- **Pilot**: `DataSourcesListView` uses `useLocation` from `react-router-dom` (v5 outer context) instead of the compat layer.

## Remaining blockers for full migration

1. **Outer v5 `Router` still required**  
   Some components still import hooks from `react-router-dom` (v5). The outer router preserves their context until they are migrated.

2. **Widespread `react-router-dom-v5-compat` imports**  
   Many files import hooks and components from `react-router-dom-v5-compat`. Replacing them wholesale requires coordinated updates and test mock paths (`jest.mock('react-router-dom-v5-compat', ...)`).

3. **`RouteDescriptor` types**  
   `public/app/core/navigation/types.ts` imports `Params` from `react-router-dom-v5-compat` and `Location` from `history`. Aligning types with pure v6 APIs should wait until the router shell no longer depends on the compat layer.

4. **`Navigate` and nested routing**  
   Top-level routes in `routes.tsx` still use compat `Navigate`, `useParams`, and `useLocation` for redirects and helpers (for example datasource to connections redirects). Migrating those touches core routing and permission-gated routes.

5. **`FormPrompt` / `history.block` flows**  
   Not yet migrated to v6 navigation blocking APIs.

## Suggested follow-ups

- Incrementally swap feature-area imports from `react-router-dom-v5-compat` to `react-router-dom` where only v6 APIs are used (`useLocation`, `useParams`, `Routes`, `Route`, `Navigate`).
- Plan a phase to remove the outer v5 `Router` and `react-router-dom-v5-compat` entirely, after auditing all `locationService` and `react-router-dom` consumers.
- Add an ESLint rule or codemod to discourage new `react-router-dom-v5-compat` imports outside the routing shell.
