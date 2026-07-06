# End-to-End Automation Testing Guide

This project uses [Playwright](https://playwright.dev/) to run automated tests against the SpeakFeed dev environment.

## Why start with login?

Sign-up and forgot-password flows use reCAPTCHA, which blocks automation. Therefore the first automated coverage focuses on the **login flow**, which is the daily entry point for every Speaker and Organizer.

## What is covered now

1. Speaker login with valid credentials and dashboard redirect
2. Invalid credential error handling
3. Empty-field validation on the login form
4. Organizer login and organizer-dashboard redirect
5. Authenticated dashboard smoke tests using saved sessions

## Prerequisites

1. Node.js 20+
2. Dependencies installed:
   ```bash
   npm ci
   ```
3. Playwright browsers installed:
   ```bash
   npx playwright install
   ```
   On Linux/WSL you may also need:
   ```bash
   npx playwright install --with-deps
   ```

## Configure test credentials

Because reCAPTCHA blocks automated sign-up, test accounts must exist before running tests. Ask your backend team to create them, or create them manually once through the UI.

Copy the example file and fill in real credentials:

```bash
cp .env.test.local.example .env.test.local
```

Edit `.env.test.local`:

```env
SPEAKER_TEST_EMAIL=speaker-test@example.com
SPEAKER_TEST_PASSWORD=YourStrongPassword

ORGANIZER_TEST_EMAIL=organizer-test@example.com
ORGANIZER_TEST_PASSWORD=YourStrongPassword
```

`.env.test.local` is gitignored and must **never** be committed.

## Run tests

```bash
# Run all E2E tests against dev
npm run test:e2e

# Run tests in UI mode (great for debugging)
npm run test:e2e:ui

# View the HTML report
npm run test:e2e:report
```

## Project structure

```
playwright.config.ts            # Playwright configuration
tests/
  auth/
    login.setup.ts              # Creates saved login sessions
    login.spec.ts               # Login happy path + validation tests
  dashboard/
    speaker-dashboard.spec.ts   # Speaker dashboard smoke tests
  organizer/
    organizer-dashboard.spec.ts # Organizer dashboard smoke tests
playwright/.auth/               # Saved session files (gitignored)
playwright-report/              # HTML reports (gitignored)
```

## How session reuse works

`tests/auth/login.setup.ts` logs in once as Speaker and once as Organizer, then saves the browser state to:

- `playwright/.auth/speaker.json`
- `playwright/.auth/organizer.json`

Other tests load these saved states so they do not need to log in again. This keeps the suite fast and stable.

## CI/CD

The GitHub Actions workflow `.github/workflows/e2e-smoke.yml` runs the login smoke tests on every pull request to `main` or `hotfix/*` branches.

Required repository secrets:

- `SPEAKER_TEST_EMAIL`
- `SPEAKER_TEST_PASSWORD`
- `ORGANIZER_TEST_EMAIL`
- `ORGANIZER_TEST_PASSWORD`

## Next flows to automate

Once the login suite is stable, add tests in this order:

1. Talk / event creation
2. QR code feedback submission
3. Lead conversion from feedback
4. AI follow-up to attendees
5. Subscription purchase (Stripe test mode)
6. Pause / resume / cancel subscription
7. Read-only mode enforcement across pages
8. Speaker ↔ Organizer role switching

## Tips for manual testers writing automation

- Prefer stable selectors like `data-testid`, `id`, or semantic roles over CSS classes.
- Ask developers to add `data-testid` attributes to new UI elements that you need to interact with.
- Keep test accounts in a known state. Reset test data after destructive tests.
- Never commit credentials.
- Fix flaky tests immediately; flaky tests erode trust in the whole suite.
