# Reuters Permission Request Generator

A browser-based prototype for generating multilingual permission-request wording when Reuters staff seek to clear video, photographs, or other content supplied by sources outside Reuters.

The tool guides users through the main clearance choices, inserts optional case-specific details into an approved language template, requires a human review before export, and produces text that can be copied or downloaded as a PNG.

> **Internal prototype:** This application is designed as a workflow aid for Reuters staff. It does not itself obtain consent, record a source's acceptance, verify ownership, approve publication, or replace editorial, legal, or rights-clearance judgement.

---

## Contents

- [Purpose](#purpose)
- [Intended users](#intended-users)
- [What the prototype does](#what-the-prototype-does)
- [User workflow](#user-workflow)
- [Permission types](#permission-types)
- [Language support](#language-support)
- [Favourites](#favourites)
- [Generated output and export](#generated-output-and-export)
- [Human-review safeguard](#human-review-safeguard)
- [Running the prototype](#running-the-prototype)
- [Technical architecture](#technical-architecture)
- [Data handling and privacy](#data-handling-and-privacy)
- [Accessibility and responsive behaviour](#accessibility-and-responsive-behaviour)
- [Validation and error handling](#validation-and-error-handling)
- [Known limitations](#known-limitations)
- [Recommended production requirements](#recommended-production-requirements)
- [Testing checklist](#testing-checklist)
- [Maintenance guide](#maintenance-guide)
- [Suggested governance](#suggested-governance)

---

## Purpose

Reuters staff may need to request permission to use visual material supplied by a member of the public, eyewitness, social-media user, freelancer, organisation, or another third-party source. The exact wording can vary according to:

- the language understood by the source;
- the rights Reuters is requesting;
- whether resale and archive use are included;
- whether permission is limited by date;
- whether the material was captured by drone;
- the identity of the source or rights holder; and
- the specific content being cleared.

The Permission Request Generator brings those choices into one guided interface. Its central goals are to:

1. reduce the risk of selecting or manually editing the wrong template;
2. make multilingual permission wording easier to locate;
3. explain which rights option is being selected;
4. keep the user's workflow consistent across languages and teams;
5. require an explicit human check before export; and
6. provide a reusable output that can be sent through the appropriate communication channel.

The application is intended to help prepare a permission request. The actual clearance process remains complete only when the appropriate source or rights holder has received the wording and given an acceptable, documented response under Reuters procedures.

---

## Intended users

The prototype is intended for Reuters colleagues involved in acquiring, verifying, producing, editing, or clearing third-party visual content, including users who may not work with permission language every day.

It is designed to be usable across regions and desks. A user does not need to know the internal template identifiers or manually search through a collection of translations. The interface exposes only the permission types available for the selected language.

### Typical use case

A Reuters journalist or producer has identified a third-party video that may be suitable for editorial use. The user:

1. identifies the person who owns or is authorised to license the material;
2. selects the language in which that person should receive the request;
3. selects the required rights configuration;
4. adds the person's name and a precise description or link for the content;
5. generates and checks the wording;
6. edits it if necessary without changing the intended rights;
7. confirms human review;
8. copies or downloads the request; and
9. sends it to the rights holder and retains the resulting permission evidence through the appropriate Reuters workflow.

---

## What the prototype does

### Core features

- **Searchable language selector:** Users can search or browse the available languages.
- **Template-aware permission choices:** Only permission types with a template for the selected language can be selected.
- **Progressive workflow:** Permission and content sections remain locked until a language is chosen.
- **Four permission categories:** Access All, No Resales, Time Limited, and Drone.
- **Conditional expiry date:** An expiry-date field appears only for Time Limited requests.
- **Optional personalisation:** The source's name/title and a description of the content can be inserted into the wording.
- **Placeholders:** If optional details are omitted, `{NAME}` and `{CONTENT}` remain visible for completion or review.
- **Editable output:** Generated wording remains editable before export.
- **Right-to-left support:** Arabic, Hebrew, Dari, Pashto, and Kurdish Sorani output is displayed right-to-left.
- **Human-review gate:** Copying and PNG export remain disabled until the user confirms review.
- **Live PNG preview:** Edits to the text are reflected in a branded canvas preview after a short delay.
- **Copy and download:** Approved text can be copied to the clipboard or downloaded as a PNG.
- **Favourites:** Frequently used language and permission combinations can be saved locally in the browser.
- **Keyboard shortcut:** `Ctrl+Enter` on Windows/Linux or `Command+Enter` on macOS generates text when the required selections are complete.
- **Reset controls and notifications:** Toast messages report generation, copy, download, validation, and reset events.

### What it does not do

The prototype does not:

- contact a source;
- authenticate the user or source;
- verify that the source owns the copyright or has licensing authority;
- collect or store the source's response;
- create an audit trail or clearance record;
- upload or inspect the content being cleared;
- connect to Reuters content-management, messaging, commissioning, or rights systems;
- determine which permission type is editorially or legally appropriate;
- guarantee that every translation is current or legally approved; or
- prevent a user from making material edits after generation.

---

## User workflow

## 1. Select a language

Use the **Search or select a language** field to browse or type part of a language name. Matching choices appear in the list. Select the language the source can understand and use to grant permission.

The application starts with no language selected. It deliberately removes any previously remembered language when the page loads, reducing the risk of accidentally reusing the last selection for a different source.

After a language is selected:

- the **Permission Type** and **Content Details** sections unlock;
- the app shows which permission types exist for that language;
- unavailable permission cards are greyed out and disabled; and
- any previously selected permission type that is unavailable in the new language is cleared.

The availability indicator lists:

- Access All;
- No Resales;
- Time Limited; and
- Drone.

A green marker means the corresponding template is available. A grey marker means that combination is not currently present in the prototype.

### Important selection principle

Select the language that the person granting the rights can understand. The presence of a translation in the tool does not by itself establish that the translation has received current legal or linguistic approval.

## 2. Select a permission type

Choose one of the enabled permission cards. The app only permits a language/type combination if a corresponding entry exists in its template database.

See [Permission types](#permission-types) for a detailed explanation of each option.

## 3. Add an expiry date when required

If **Time Limited** is selected, an **Expiry Date** field appears and becomes mandatory.

The generated template states that no new uses may be made after the selected date, while use or display initiated before that date may continue according to the wording in the relevant translation.

The interface converts the selected ISO date into an English display format such as `13 October 2026`. Because the surrounding template may be in another language, date localisation should be reviewed before production deployment.

## 4. Add content details

Two optional fields are provided:

### Person Name / Title

Enter the name of the person or entity being asked to grant rights, with a title if necessary.

Example:

```text
Alex Smith, owner of the account @example
```

If left empty, the generated text retains:

```text
{NAME}
```

### Content Description

Identify the material precisely enough that both Reuters and the source can understand what is covered by the request.

A useful description may include:

- the type of content;
- what the content shows;
- the approximate date and location;
- the relevant social-media post or source URL;
- the account or uploader; and
- any distinction between multiple clips or files.

Example:

```text
the original video posted at [URL], showing flooding and rescue operations in central Bangkok on 12 October 2026
```

If left empty, the generated text retains:

```text
{CONTENT}
```

Do not send unresolved placeholders to a source unless the approved workflow expressly allows them. Their presence should be treated as a prompt for completion during human review.

## 5. Generate the request

Select **Generate Permission Text** or use the keyboard shortcut.

Generation requires:

- a selected language;
- an available permission type; and
- an expiry date when Time Limited is selected.

The selected template is copied into the output and all occurrences of these tokens are replaced:

```text
{NAME}
{CONTENT}
{DATE}
```

For omitted optional fields, the corresponding placeholder remains visible.

## 6. Review and edit

The generated wording appears in an editable text area. Metadata beneath it shows:

- the selected language;
- the permission type; and
- the generation timestamp.

Review the complete wording, not just the inserted details. Check that:

- the correct language was selected;
- the source can understand that language;
- the correct rights option was used;
- the name or entity is accurate;
- the content is unambiguously identified;
- the expiry date is correct, where relevant;
- no placeholder remains unintentionally;
- no material legal wording was accidentally changed; and
- the message is ready to be sent through the intended channel.

The output can be edited directly. Any edits are reflected in the PNG preview after approximately 600 milliseconds.

## 7. Confirm human review

Tick **Reviewed by human** after reading and verifying the full request.

Until that box is ticked:

- **Copy Text** is disabled;
- **Download PNG** is disabled; and
- the export area displays an approval lock message.

This is a workflow safeguard, not an independent validation. The application does not analyse whether the text is legally correct or whether the review was adequate.

## 8. Export

After human review, use either:

- **Copy Text** to place the current editable text on the clipboard; or
- **Download PNG** to create a branded image of the current text.

The PNG file name follows this pattern:

```text
reuters-permission_<language>_<permission-type>_<YYYY-MM-DD>.png
```

Example:

```text
reuters-permission_french_access-all_2026-10-13.png
```

## 9. Send and retain evidence

Send the request to the source through the appropriate channel. The generated request is not permission by itself. Follow Reuters procedures for:

- confirming that the responder is the rights holder or authorised representative;
- obtaining an unambiguous affirmative response;
- preserving the request and response together;
- recording restrictions and expiry dates; and
- linking the clearance evidence to the relevant content or production record.

---

## Permission types

The labels below describe the prototype's intended workflow. They should be aligned with current Reuters-approved legal and editorial guidance before production use.

### Access All

**Interface description:** Full perpetual rights.

This option uses the broadest standard template. In the English version, the source represents that they own the content or are authorised by the copyright owner, and grants Reuters and its licensees perpetual rights to publish, republish, display, edit, prepare derivative works, and distribute the content worldwide, in all languages and media. It also includes wording allowing Reuters to license and sell the content through its online archive.

Use this option only when the requested and approved rights include the archive/resale paragraph contained in the applicable template.

### No Resales

**Interface description:** No Resales / Archive.

This option generally retains broad, perpetual publication and distribution rights but omits the final archive/resale paragraph found in Access All templates.

The visible internal label is:

```text
No Resales / No Archive (Limited Rights)
```

Because translations vary and labels can simplify complex rights, users should review the complete generated wording rather than rely solely on the card title.

### Time Limited

**Interface description:** Rights expire by date.

This option requests rights subject to a selected cutoff date. The standard English wording says that no new uses may be made after that date, while the right to host and display content used or published before the date remains perpetual.

An expiry date is mandatory. Users must ensure that any selected date and continuing-use wording match the actual agreement and applicable Reuters policy.

### Drone

**Interface description:** Drone content wording.

This option is structurally different from the other rights templates. The English version focuses on the accuracy of the submitted content, lawful newsgathering, third-party rights, injury or property damage, and indemnification.

It should not automatically be treated as a substitute for all other copyright or licensing permissions. Before production use, the intended relationship between drone wording and the other licence options should be documented and approved.

---

## Language support

The language selector contains **40 options**:

1. English
2. French
3. Spanish
4. Mandarin
5. Arabic
6. Hebrew
7. Thai
8. Japanese
9. Italian
10. German
11. Indonesian
12. Malay
13. Romanian
14. Serbian
15. Turkish
16. Korean
17. Dari
18. Pashto
19. Portuguese
20. Russian
21. Ukrainian
22. Bosnian
23. Bulgarian
24. Croatian
25. Czech
26. Danish
27. Greek
28. Hungarian
29. Latvian
30. Lithuanian
31. Catalan
32. Montenegrin
33. Norwegian
34. Polish
35. Kurdish Sorani
36. Khmer / Cambodian
37. Hindi
38. Burmese / Myanmar
39. Tagalog / Filipino
40. Vietnamese

### Availability varies by language

Not every language contains all four permission types. The UI derives availability directly from the template database and disables missing combinations.

The current prototype includes a mixture of:

- languages with all four template types;
- languages with Access All and No Resales only;
- languages with some combination of Time Limited or Drone; and
- individual templates that require linguistic, formatting, or legal review.

The interface is designed to prevent selection of a missing combination, but it does not assess the quality, consistency, provenance, or approval status of the translations.

### Right-to-left output

The editable output and PNG rendering switch to right-to-left direction for:

- Arabic;
- Hebrew;
- Dari;
- Pashto; and
- Kurdish Sorani.

This improves display direction, but production testing is still required for punctuation, mixed Latin/non-Latin text, line wrapping, font coverage, shaping, and exported-image fidelity.

---

## Favourites

The favourites feature is intended to speed up repeated workflows.

### Saving a combination

After selecting a valid language and permission type, select **Yes** beside:

```text
Save this combination to Favourites for next time?
```

Once saved, the message changes to:

```text
Added to your favourites.
```

### Opening favourites

Select the **Favourites** pill at the top of the settings panel. The count represents the total number of stored favourite languages and language/permission combinations.

Saved items are grouped as:

- **Favourite Languages**; and
- **Favourite Combinations**.

Selecting a favourite language applies the language and moves the user to the permission section. Selecting a combination applies both the language and permission type and moves the user toward content details.

Each favourite can be removed using its adjacent remove control.

### Storage behaviour

Favourites are stored in browser `localStorage` under:

```text
reuters_permission_favourites_v1
```

This means favourites are:

- local to that browser and browser profile;
- not synchronised between devices;
- not stored in a Reuters account by this prototype;
- potentially removed when site data is cleared; and
- available to other sessions using the same browser profile.

No source names, content descriptions, generated requests, or permission responses are deliberately stored in `localStorage` by the current code. Only favourite language identifiers and language/permission combinations are stored.

---

## Generated output and export

### Editable permission text

The text area is the working output. Export operations use its current contents, including any manual edits made after generation.

Changing the original form fields after generation does not automatically regenerate the text. Select **Generate Permission Text** again if new form values need to be inserted into the template.

### Metadata

The application displays the selected language, internal permission label, and generation timestamp. This metadata is shown in the interface but is not added to the copied permission text.

### PNG preview

The PNG preview is generated locally in an HTML canvas and includes:

- a drawn Reuters-style mark and wordmark;
- the current permission text;
- paragraph spacing and line wrapping;
- left-to-right or right-to-left direction; and
- an automatically calculated document height.

For left-to-right output, wrapped lines may be justified. For right-to-left output, text is right-aligned.

### Copy behaviour

The application first uses the modern Clipboard API. If that fails, it selects the output text and attempts the legacy browser copy command as a fallback.

### Reset behaviour

**Clear & Reset** clears the generated output, preview, review state, and export controls. It does **not** clear the selected language, permission type, content-detail form fields, expiry date, or favourites. The current button label may therefore suggest a broader reset than the function performs.

---

## Human-review safeguard

The export gate is one of the most important workflow controls in the prototype.

Each newly generated request resets the review checkbox. Users must re-confirm review before copying or downloading. This helps prevent immediate export of unreviewed text.

However, after the box is ticked, the user can continue editing the text without automatically invalidating the approval state. A production version should normally reset the review confirmation whenever the output is changed after approval.

Recommended production behaviour:

1. generate or edit text;
2. mark the output as needing review;
3. complete review;
4. enable export;
5. if text changes, revoke review and disable export again; and
6. require a new review confirmation.

---

## Running the prototype

The application is a single, self-contained HTML file with inline CSS and JavaScript. It has no declared package manager, build step, external library, or server-side dependency.

### Basic local use

Open the HTML file in a modern browser:

```text
permission_genrator-v5.html
```

> The source filename currently contains `genrator`. Consider renaming it to `permission-generator-v5.html` for consistency.

### Recommended local-server use

Some browser features, especially clipboard access, behave more reliably in a secure context or on localhost. From the containing directory, a simple development server can be used, for example:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/permission_genrator-v5.html
```

Use only an approved Reuters hosting and development environment. Do not expose the prototype publicly or use unapproved hosting for real source information.

### Browser expectations

The code relies on modern browser capabilities including:

- optional chaining;
- `Set`;
- template literals;
- `localStorage`;
- the Clipboard API, with a fallback;
- HTML canvas;
- the `inert` property;
- Unicode regular-expression behaviour; and
- `toLocaleString`.

Test against Reuters-supported browser versions before deployment.

---

## Technical architecture

### Single-file structure

The prototype contains four main layers in one HTML document:

1. **HTML:** Header, guided input panel, output cards, review control, and export actions.
2. **CSS:** Reuters-inspired colours, responsive two-column layout, form states, tooltips, favourites, locking, cards, toast notifications, and mobile breakpoints.
3. **Template database:** A JavaScript `TEMPLATES` object keyed by language and permission type.
4. **Application logic:** Selection, availability, substitution, review state, favourites, editable preview, clipboard export, canvas rendering, and reset behaviour.

### Template model

Templates use this shape:

```javascript
const TEMPLATES = {
  LANGUAGE_KEY: {
    ACCESS_ALL: "...",
    NO_RESALES: "...",
    TIME_LIMITED: "...",
    DRONE: "..."
  }
};
```

Not every permission-type key is required for every language. Its presence determines UI availability.

### Supported placeholders

Templates may contain:

```text
{NAME}
{CONTENT}
{DATE}
```

During generation, all occurrences are replaced globally with form values. If a name or content description is omitted, its literal placeholder is retained. The date remains a placeholder only outside a required Time Limited flow.

### Permission labels

Internal permission keys are mapped to display labels:

```text
ACCESS_ALL   -> Access All (Perpetual Rights)
NO_RESALES   -> No Resales / No Archive (Limited Rights)
TIME_LIMITED -> Limited Access with Time Limit
DRONE        -> Drone Content Wording
```

### State model

The app maintains state through:

- current form-control values;
- checked radio buttons;
- `currentLang` and `currentPermType` for rendered output;
- review-checkbox state;
- DOM visibility and disabled states; and
- browser `localStorage` for favourites.

There is no back-end state, account state, server persistence, or database.

### File naming

PNG exports are generated using the selected language, permission type, and current ISO date. Only the first underscore in the permission key is replaced with a hyphen by the current implementation. For example, `TIME_LIMITED` becomes `time-limited`, while identifiers containing additional underscores would not be fully normalised.

---

## Data handling and privacy

### Current prototype behaviour

All visible processing is performed in the browser. The code contains no network request, analytics call, remote API, upload endpoint, or server-side storage operation.

The user may enter personal data in the name/title and content-description fields. In the prototype:

- that information remains in the current page's memory;
- it is inserted into the editable output;
- it may be copied to the system clipboard;
- it may be rendered into and downloaded as a PNG; and
- it is not intentionally written to `localStorage`.

### Operational cautions

Even without a back end, users should treat real source information as sensitive:

- use only approved Reuters devices, browsers, and hosting;
- avoid entering unnecessary personal information;
- do not use public or shared devices;
- take care when copying to the clipboard;
- store exported PNGs only in approved locations;
- retain the minimum evidence required by policy; and
- follow applicable Reuters security, privacy, records, and source-protection guidance.

### Production privacy review

Before deployment, document:

- the approved hosting environment;
- whether telemetry is permitted and what it records;
- retention rules for any future saved requests;
- access-control requirements;
- treatment of confidential sources;
- handling of personal data across jurisdictions;
- clipboard and download risks;
- incident response; and
- whether a formal privacy or security assessment is required.

---

## Accessibility and responsive behaviour

The prototype contains several accessibility-oriented features:

- semantic landmarks for header, main content, aside, and output section;
- visible labels for form controls;
- required and optional indicators;
- ARIA labels for language search, radio groups, controls, preview, and canvas;
- live regions for availability, favourite confirmation, preview messages, and toast notifications;
- keyboard navigation in the custom language list;
- focus styling on primary controls;
- disabled states for unavailable actions;
- right-to-left text direction for selected languages; and
- responsive stacking below 900 pixels.

### Keyboard interaction

The custom language selector supports:

- typing to filter;
- `Arrow Up` and `Arrow Down` to move through results;
- `Enter` to select;
- `Escape` to close; and
- `Tab` to leave the selector.

The app also supports `Ctrl+Enter` or `Command+Enter` to generate.

### Areas requiring accessibility testing

Before production release, test with keyboard-only operation and supported screen readers. Pay particular attention to:

- the custom combobox's ARIA implementation;
- focus movement after applying a favourite;
- tooltip access on touch screens and by keyboard;
- the use of colour in availability indicators;
- hidden and inert sections;
- toast timing and announcement;
- radio-card states;
- the review lock and its status messaging;
- high-contrast and forced-colour modes;
- browser zoom and text resizing;
- right-to-left output; and
- accessibility of information embedded in downloaded PNGs.

A PNG is not an accessible substitute for text. Where possible, preserve a text version alongside image exports.

---

## Validation and error handling

The application prevents generation when required selections are missing.

### Validation conditions

- No language: generation remains unavailable.
- No permission type: generation remains unavailable.
- Unsupported language/type combination: the radio option is disabled, and direct generation is rejected.
- Time Limited without a date: an error toast is shown and focus moves to the expiry-date field.
- Empty optional fields: generation proceeds with visible placeholders.
- Empty output during copy: the user receives an error message.

### Notifications

Toast messages communicate events such as:

- successful generation;
- missing required choices;
- missing expiry date;
- successful clipboard copy;
- fallback copy;
- PNG download; and
- output clearing.

---

## Known limitations

The following issues should be considered before the prototype is used beyond controlled testing.

### 1. Legal and editorial approval is not encoded

A template's presence in the file does not prove that it is current, approved, or suitable for a particular acquisition. There is no template version, owner, approval date, reviewer, jurisdiction, or change history.

### 2. Translation coverage is uneven

Some languages include all four permission types, while others include only one, two, or three. Users may be unable to generate the required rights configuration in a selected language.

### 3. Translation quality appears inconsistent

The template database contains visible anomalies that should be reviewed by qualified Reuters legal and language owners before real-world deployment. Examples include apparent source-specific wording in a Burmese template, an Arabic-script template under the Pashto Drone key, editorial instructions embedded in Norwegian text, punctuation/markup anomalies, inconsistent Reuters naming, and uneven structure between languages.

### 4. Labels may oversimplify rights

Short labels such as **Access All**, **No Resales**, and **Time Limited** cannot describe every legal consequence. The generated wording, approved guidance, and case context must control.

### 5. Drone is not integrated with other licence choices

Drone is implemented as an alternative radio option, not an additional clause. Its template may therefore omit ordinary copyright/licensing language available under other types. This product decision requires explicit legal confirmation.

### 6. Approval is not invalidated after editing

Once **Reviewed by human** is checked, later edits do not disable export or require another review. This weakens the review control.

### 7. Optional fields can leave unresolved placeholders

The prototype permits `{NAME}` and `{CONTENT}` to remain in an export. The review gate relies on the user to notice them.

### 8. Date formatting is not localised

All dates are rendered with English month names, regardless of the selected template language.

### 9. No date-policy validation

The app does not stop users selecting a past date, today's date, or an operationally unsuitable date. It also does not clarify time zone or end-of-day treatment.

### 10. No consent capture or audit trail

The tool generates outbound wording only. It does not record the source's response, identity, timestamp, communication channel, attached content, or subsequent edits.

### 11. No authentication or authorisation

Anyone with access to the file can run it. The `Internal Tool` badge is presentational and does not enforce Reuters-only access.

### 12. No content security policy or integrity controls

The prototype is a single inline-script page. A production deployment should introduce appropriate security headers, controlled hosting, code review, release management, dependency policy, and tamper detection.

### 13. Local favourites are device-specific

Favourites are not synchronised, backed up, centrally managed, or associated with a Reuters user account.

### 14. Clear & Reset is partial

The reset function clears output but retains inputs and selections. Either the label or behaviour should be changed to avoid user misunderstanding.

### 15. Form changes do not update generated text automatically

After generation, changing name, content, date, language, or permission fields does not automatically update the output. Users must generate again. The app does not prominently warn that the output may be stale.

### 16. Generated text can be altered freely

The editable area is useful operationally, but the app does not distinguish approved template text from user edits or flag changes to legal clauses.

### 17. PNG rendering needs multilingual verification

Canvas output depends on locally available system fonts. Complex-script shaping, glyph coverage, punctuation, line breaking, and bidirectional text may vary by browser and operating system.

### 18. Branding requires approval

The interface and PNG draw Reuters-inspired branding directly. Production use should follow current Reuters brand, trademark, and internal product-design approvals.

### 19. No automated tests are included

There is no test suite for template completeness, placeholder integrity, UI behaviour, rendering, accessibility, or regression detection.

### 20. Filename spelling

The HTML filename uses `permission_genrator-v5.html`, with `generator` misspelled. Renaming may require updating references in documentation or hosting.

---

## Recommended production requirements

Before making the tool broadly available across Reuters, consider the following minimum workstreams.

### Legal and template governance

- Assign a named owner for each permission type and each translation.
- Confirm the legal meaning and approved use case for every option.
- Decide whether Drone is a standalone template, an additional clause, or a separate workflow.
- Record template version, approval status, approver, approval date, and next-review date.
- Lock approved legal wording or provide a visible diff when it is edited.
- Define who may update and publish templates.
- Remove instructions, source-specific examples, and translation anomalies from template text.
- Establish fallback guidance when a requested combination is unavailable.

### Product controls

- Revoke human approval whenever the output changes.
- Block export when unresolved placeholders remain, unless an explicit override is justified.
- Warn when the form and generated output no longer match.
- Localise dates according to the selected language.
- Validate expiry dates against agreed policy.
- Clarify reset behaviour.
- Provide a plain-text export and, if needed, an accessible PDF or approved message format.
- Add a visible template version to the generated record, without necessarily including it in source-facing wording.

### Clearance evidence

- Define how the request and response are saved together.
- Capture the source identity, ownership basis, content URL/file identifier, sent time, response time, channel, permission type, restrictions, expiry date, and staff member.
- Preserve the exact version sent and the exact response received.
- Link clearance evidence to the media asset or editorial record.
- Ensure later users can see restrictions without relying on a PNG alone.

### Security and privacy

- Host only in an approved authenticated Reuters environment.
- Conduct security, privacy, data-protection, records, and threat-model reviews.
- Apply access controls, security headers, logging rules, and change management.
- Avoid recording source details in analytics or error logs.
- Define retention and deletion behaviour.
- Assess use with confidential sources and sensitive investigations.

### Accessibility and internationalisation

- Complete WCAG-aligned testing.
- Validate every language with appropriate native-language reviewers.
- Test right-to-left layout, Unicode, fonts, line breaking, and canvas output on supported platforms.
- Localise interface instructions if the application itself must serve non-English-speaking Reuters users.
- Avoid making image export the only durable format.

### Engineering and deployment

- Separate templates, interface code, and styles into maintainable components.
- Add schema validation for template data.
- Add automated unit, integration, accessibility, and visual-regression tests.
- Add environment/version information and release notes.
- Use source control, peer review, CI/CD, rollback, and controlled production releases.
- Monitor errors without collecting unnecessary source information.

---

## Testing checklist

### Functional workflow

- [ ] The page starts with no selected language.
- [ ] Permission and content sections are locked before language selection.
- [ ] Language search filters correctly.
- [ ] Keyboard navigation selects the highlighted language.
- [ ] Clearing the language also clears the selected permission type.
- [ ] Availability markers match the template object.
- [ ] Missing permission types are disabled.
- [ ] Selecting Time Limited reveals the expiry-date field.
- [ ] Time Limited cannot generate without a date.
- [ ] Name, content, and date placeholders are replaced correctly.
- [ ] Repeated placeholders are all replaced.
- [ ] Optional empty values leave the intended placeholders.
- [ ] Generation resets the human-review state.
- [ ] Export remains locked until review is confirmed.
- [ ] Copy uses the current edited output.
- [ ] PNG uses the current edited output.
- [ ] PNG filename matches the selected language, type, and date.
- [ ] Clear/reset behaviour matches the product wording.

### Favourites

- [ ] A valid language/type combination can be saved.
- [ ] Duplicate combinations are not created.
- [ ] Favourites persist after reload.
- [ ] Invalid or removed templates are filtered from stored favourites.
- [ ] Applying a favourite updates availability and the selected radio card.
- [ ] Removing a favourite updates the count.
- [ ] Storage failures do not break the application.

### Template integrity

- [ ] Every language selector value has a corresponding template object.
- [ ] Every template uses only supported placeholders.
- [ ] Time Limited templates contain `{DATE}`.
- [ ] Templates requiring personalisation contain the intended placeholders.
- [ ] Access All and No Resales differ only in approved ways.
- [ ] No internal instruction remains in source-facing wording.
- [ ] No template contains case-specific or accidental content.
- [ ] Every translation has a recorded reviewer and approval version.

### Multilingual rendering

- [ ] Latin-script languages wrap correctly.
- [ ] Arabic and Hebrew appear in the correct direction.
- [ ] Dari, Pashto, and Kurdish Sorani are correctly shaped and aligned.
- [ ] CJK scripts wrap without losing characters.
- [ ] Thai, Khmer, Burmese, Hindi, and Korean render with suitable fonts.
- [ ] Mixed URLs and Latin names remain readable in RTL output.
- [ ] PNG text matches the editable text exactly.

### Accessibility

- [ ] All controls are reachable by keyboard.
- [ ] Focus order is logical.
- [ ] Focus remains visible.
- [ ] Combobox state and options are announced correctly.
- [ ] Locked sections are not interactable.
- [ ] Required, disabled, selected, and approved states are announced.
- [ ] Tooltips are available without a mouse.
- [ ] Status messages are announced without excessive repetition.
- [ ] Colour is not the sole indicator of availability.
- [ ] Layout remains usable at 200% and 400% zoom.

### Security and privacy

- [ ] No source data is sent to an unapproved endpoint.
- [ ] No personal data is recorded in analytics.
- [ ] Clipboard and downloaded-file risks are documented.
- [ ] Production hosting requires Reuters authentication.
- [ ] Security headers and content policy are configured.
- [ ] Logs and retention follow approved policy.

---

## Maintenance guide

### Adding a language

1. Add the language to the selector with a stable uppercase key.
2. Add a matching top-level object in `TEMPLATES`.
3. Add only the permission types that have approved templates.
4. If the language is right-to-left, add its key to `RTL_LANGUAGES`.
5. Validate all supported placeholders.
6. Test selection, generation, editable output, copy, PNG, and favourites.
7. Record the translation owner and approval information outside the runtime object or in a future governed template schema.

Example structure:

```javascript
"EXAMPLE_LANGUAGE": {
  "ACCESS_ALL": "Approved text containing {NAME} and {CONTENT}...",
  "NO_RESALES": "Approved text containing {NAME} and {CONTENT}...",
  "TIME_LIMITED": "Approved text containing {NAME}, {CONTENT}, and {DATE}...",
  "DRONE": "Approved drone wording..."
}
```

### Updating a template

Treat permission wording as controlled content:

1. obtain the required legal/editorial and language approval;
2. record the old and new versions;
3. test placeholder preservation;
4. compare related permissions to ensure only intended clauses differ;
5. test source-facing formatting;
6. update release notes; and
7. deploy through a reviewed release process.

### Removing a template

When a template key is removed, its permission card becomes unavailable for that language. Previously stored favourites referencing the missing combination are filtered out when favourites are loaded.

### Placeholder rules

- Keep placeholder spelling and braces exact.
- Use `{DATE}` in every Time Limited template that needs the chosen date.
- Do not translate placeholder identifiers themselves.
- Avoid unapproved additional brace tokens because the generator will not replace them.
- Test templates with long names, long URLs, multiline descriptions, and non-Latin input.

---

## Suggested governance

A production permission generator should have clearly assigned responsibilities:

- **Product owner:** owns scope, user workflow, adoption, and roadmap.
- **Legal/content-rights owner:** approves rights categories and source-facing wording.
- **Language owners:** validate each translation and its ongoing accuracy.
- **Editorial representatives:** confirm operational fit across desks and regions.
- **Engineering owner:** maintains code, testing, security, and releases.
- **Security/privacy/records stakeholders:** approve data handling, access, retention, and logging.
- **Accessibility owner or reviewer:** validates inclusive use and export formats.

Recommended metadata for every governed template:

```yaml
language: French
permission_type: ACCESS_ALL
version: 1.0.0
status: approved
legal_owner: <team or role>
language_reviewer: <team or role>
approved_on: YYYY-MM-DD
review_due: YYYY-MM-DD
source_reference: <controlled document ID>
change_summary: <short description>
```

This metadata should be maintained in a controlled source of truth and made available to the application or its administrators.

---

## Prototype status summary

The current version demonstrates a strong guided workflow: searchable multilingual selection, template-aware restrictions, optional personalisation, favourites, editable output, right-to-left handling, human review, clipboard export, and PNG generation in a self-contained browser application.

Before Reuters-wide operational use, the highest-priority work is to validate and govern every template, define the exact operational meaning of each permission type, resolve translation anomalies, strengthen the review gate, establish secure authenticated hosting, and connect generated requests to a durable clearance-evidence workflow.
