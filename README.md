# PhishGuard · ACM SYNAPSE

PhishGuard is an explainable URL and email checker for **ACM_PS3**. Its **Email drafter** workspace reviews outgoing text and prepares a draft to copy into your email app. **Already clicked?** helps users choose practical next steps after interacting with a suspicious message or link.

The phishing assessment and writing review use transparent rules. They are not comprehensive grammar or factual checks, and a low concern score does not establish safety.

## Run immediately

1. Extract the project.
2. Open `public/index.html` for the Threat analyzer.
3. Open `public/email.html` for the Email drafter.
4. Open `public/recovery.html` for Already clicked?, or use the navigation.

Analysis, local review, draft preparation, and the incident questionnaire run in your browser without API keys, an email-account connection, or installation. Online email proofreading is optional and sends the subject and message to LanguageTool; the recipient is not sent. Submitted links are inspected as text. The hosted site retains its existing access policy.

For a local static server:

```bash
python -m http.server 8000 --directory public
```

Open `http://localhost:8000/index.html`.

## Threat analyzer

- **Website URL** identifies the destination hostname and checks structural indicators such as embedded usernames, selected brand lookalikes, unencrypted HTTP, and shortened links.
- **Email message** checks pasted incoming content for sensitive-information requests, pressure tactics, and concerning links.
- Each finding explains its evidence and score contribution. The indicator score is not a probability of phishing.
- Built-in examples, reset, copy report, and report download support a quick demonstration.
- **How it works** opens and scrolls to the explanation of checks, scoring, and limitations.

## Email drafter

1. Write the message. Recipient and subject are optional while preparing the draft.
2. Select **Review draft** and inspect the corrections and advisory tips.
3. Apply suggested corrections or keep your wording. You can also edit the text yourself.
4. Select **Create draft** to inspect the complete, approved wording in a final preview.
5. Use **Copy message**, **Copy subject**, or **Copy full draft**.
6. Paste the text into your email app, add any recipient or attachment details, and send it there.

Editing the input invalidates the previous review and prepared draft so the preview reflects the text you approved. Clipboard controls copy only after an explicit click. If clipboard access is unavailable, select and copy the preview text manually.

The final preview is a local draft; creating it does not send a message or save anything to a mailbox. The app does not connect to an email provider.

Local writing rules cover selected common misspellings, contractions, sentence capitalisation, punctuation spacing, and repeated spaces. Select **Use online proofreading** to send the subject and message (not the recipient) to [LanguageTool](https://languagetool.org/). Do not use this option for confidential text. The public API has request and rate limits, supports up to 20,000 characters per request, and suggests replacements for at most 30 misspelled words. Advisory tips can flag a vague or missing subject, a missing greeting or closing, attachment references, and tone. Protected technical strings, numeric amounts, and paragraph breaks are preserved by local checks. Neither review mode verifies facts or intended meaning; read every proposed change before accepting it.

## Already clicked?

1. Select what happened: opening a link, entering a password, sharing an OTP or approving a sign-in, sharing payment details, sending money, downloading or installing something, granting remote access, or sharing personal information. Users can select more than one relevant action or choose that they are unsure.
2. Choose the reporting location and indicate whether a work or school device or account is involved.
3. Build an action plan with steps grouped into **Now**, **Next**, and **Keep watch**. Check off steps during the current session, or copy/download the plan.

The form asks for categories of events, never the password, OTP, card number, banking information, or suspect URL itself. Answers and progress are not stored or submitted. Changing answers requires a fresh plan.

Guidance prioritises contacting the bank through known official channels when payments or banking access are involved, protecting affected accounts from a trusted device, and containing suspicious installed software or remote access. India reporting information points to **1930** for financial cyber fraud and the official [National Cyber Crime Reporting Portal](https://cybercrime.gov.in/). Reporting options depend on the selected location. These are guidance links, not an automatic report or recovery service.

The page does not scan a device or verify account security. Completing a checklist cannot establish safety or guarantee recovery. The page includes official sources for further help.

## Development

The project retains its Vinext hosting setup. Use Node.js **22.13 or later** for the full development server:

```bash
npm install
npm run dev
```

Open the local URL printed by the server. The browser tools also work through the static-server command above.

```bash
npm test
npm run build
```

`dist/` is generated deployment output. Edit authored browser assets under `public/`.

## Source layout

| Location | Purpose |
| --- | --- |
| `public/index.html`, `public/styles.css`, `public/app.js` | Threat analyzer and navigation |
| `public/engine.js` | Local URL/email analysis and indicator scoring |
| `public/email.html`, `public/email.css`, `public/email.js` | Writing review, final draft preview, and copying |
| `public/reviewer.js` | Local review rules and optional LanguageTool integration |
| `public/recovery.html`, `public/recovery.css`, `public/recovery.js` | Incident questionnaire, action checklist, and plan copying/download |
| `public/recovery-engine.js` | Local mapping from reported actions to recovery guidance |
| `app/`, `worker/` | Existing Vinext hosting entry points |
| `tests/` | Automated behavior checks with controlled inputs |
| `DEMO_GUIDE.md` | Short walkthrough and presentation questions |

## Practical limits

- Low concern means fewer configured indicators were found; it does not establish safety.
- HTTPS encrypts a connection and does not prove that a website belongs to the claimed organisation.
- Legitimate messages can trigger rules, and phishing messages can avoid them.
- The brand reference list and English writing rules are intentionally limited.
- Pages, redirects, live reputation feeds, and attachments are not inspected.
- Pasted email text cannot authenticate a sender or verify SPF, DKIM, or DMARC.
- Writing review does not verify facts, promises, intent, or every grammar error.
- Passing behavioral tests is not a measured real-world detection accuracy.
- Recovery guidance cannot inspect a device, secure an account, recover funds, or establish that an incident is resolved.

## Deployment

The project preserves its existing Sites identity and access policy. Publish the matching source and build through the Sites workflow. Keep credentials and local runtime state out of source control.
