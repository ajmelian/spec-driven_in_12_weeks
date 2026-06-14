# Frontend Guide: [Module or Project]

## Metadata

| Field | Value |
|---|---|
| Applies to | Project / Module |
| Version | 0.1 |
| Status | Draft / Reviewed / Approved |

## 1. Frontend Strategy

This project uses server-side rendered HTML5 views and Bootstrap 5. JavaScript frameworks are out of scope unless explicitly approved.

## 2. Layouts

| Layout | Purpose | Used by |
|---|---|---|
| main.php | Authenticated pages | |
| auth.php | Login and account recovery | |

## 3. Components

| Component | Purpose | File |
|---|---|---|
| Navbar | | app/Views/partials/navbar.php |
| Flash messages | | app/Views/partials/flash_messages.php |

## 4. Forms

Every form must:

- Use POST for state-changing operations.
- Include CSRF protection.
- Use server-side validation.
- Display validation errors safely.
- Use proper HTML5 input types.
- Use Bootstrap 5 classes consistently.

## 5. Accessibility

- Use semantic HTML5.
- Use labels associated with inputs.
- Preserve keyboard navigation.
- Use sufficient visual contrast.
- Do not rely only on color to communicate errors.

## 6. JavaScript Policy

Allowed:

- Small vanilla JS progressive enhancements.
- Non-critical UI improvements.

Not allowed by default:

- React, Vue, Angular, Svelte.
- Vite, Webpack or frontend build pipelines.
- CDN dependencies without approval.

## 7. Security Considerations

- Escape all dynamic output.
- Never render untrusted HTML unless sanitized.
- Do not expose sensitive information in hidden inputs.
- Avoid external assets unless approved.

## 8. Page Inventory

| Page | Route | Layout | Notes |
|---|---|---|---|
| | | | |
