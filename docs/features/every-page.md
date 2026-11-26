# Every page

These criteria apply to all pages of the site. The other feature files do not repeat them. You use the site on your phone on a train, often with no signal.

**As** the person who practises LeetCode on the commute,
**I want** the site to work on my phone with or without a signal,
**so that** I can practise for the full trip and keep my progress.

## Acceptance criteria

### 1. Read every page on a phone

- **Given** I open any page on my phone
- **When** I look at the page
- **Then** I can read all the text and code without zooming
- **And** the page does not scroll sideways, except inside a code block

### 2. Do everything with taps

- **Given** I am on any page
- **When** I do a task on the page
- **Then** I can do it with taps only
- **And** no page asks me to type text

### 3. Add the site to the home screen

- **Given** I open the site in the phone browser
- **When** I choose to add it to my home screen
- **Then** an icon for the site is on my home screen
- **And** the site opens full screen from that icon, with no browser address bar

### 4. Practise with no signal

- **Given** I opened the site at least one time with a signal
- **And** my phone now has no signal
- **When** I open the site from the home screen
- **Then** I see all the pages and all the cards that I had before I lost the signal
- **And** I can do a full practice session

### 5. Keep ratings made with no signal

- **Given** I rated cards while my phone had no signal
- **When** my phone gets a signal again
- **Then** those ratings are in my progress on every device where I use the site
- **And** I do not lose a rating

### 6. See the same progress on every device

- **Given** I rated cards on my phone
- **When** I open the site on my laptop
- **Then** I see the same due cards, ratings and settings as on my phone

### 7. Go between the main pages

- **Given** I am on any page
- **When** I look at the bottom of the screen
- **Then** I see a menu with `Practice`, `Pattern quiz`, `Patterns`, `Desk list` and `Settings`
- **And** the menu item of the current page is marked

## Decisions

- **Progress is the same on all devices.** You chose this on 20 Nov 2026, over progress that stays only on the phone.
- **The site works with no signal.** You chose this on 20 Nov 2026, because trains go underground. It installs to the home screen like an app.
- **No typing.** You said on 20 Nov 2026 that you do not want to type on the commute.
- **Only you use the site in this version.** You said on 26 Nov 2026 that you are the only person for now. Opening it to other people is for a later version.
- **The menu labels.** You agreed on 26 Nov 2026 to `Practice`, `Pattern quiz`, `Patterns` and `Settings`. You chose `Pattern quiz` over `Drill`.
- **The new problem controls are in `Settings`.** You agreed to this on 26 Nov 2026.
- **The desk list has its own page in the menu.** You chose this on 26 Nov 2026, over a link inside `Practice`.
