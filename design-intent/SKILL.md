---
name: design-intent
description: Review or redesign a page by its intent, not its looks. Breaks the page into its components, asks of each one why it is there, who carries the complexity and where the user goes next, then checks how the components fit together, and applies the fixes. Uses Mobbin, when it is connected, to see how real products solved the same job. Based on the video "Why everyone can tell you're a vibe coder". Use when the user says "design intent", "intent review", "does this look vibe coded", "why does this feel generic", "apply intent to this page", or runs it on a page, screen, component or flow.
---

# Design intent

> "If you don't have an opinion, AI will give you one."

This skill is a north star for UI work. It does not judge how a page _looks_. It asks why each part is there, who carries the complexity, and whether the parts lead the user where they want to go next.

Source: the video _Why everyone can tell you're a vibe coder_, since renamed _The Weird Death of Design Taste_ (https://youtu.be/NHOCPS1G1xw). Viewer comments that sharpen it are marked **Viewers added**.

**Frames.** When the video says "this" or "here", the frame it points at is in [`frames/`](frames/), placed under the quote and named by timestamp (`MM-SS-what-it-shows.jpg`). Each one has a caption saying what is on screen. When the caption isn't enough to understand an example, open the image with your file-reading tool.

---

## The philosophy

### Why vibe coded software all looks the same

> "One of these cards was vibe coded and you probably spotted it instantly. Now, try explaining why. Sure, there's the obvious stuff, but take all that away and you can still tell."

![Two profile cards for "Jeff Richardson" side by side. Left: avatar with a red verified badge, all metadata in one block, a blue "Following" button. Right: banner header, a "CREATOR" tag, a black "Following" button. One of them was vibe coded.](frames/00-00-cards-one-vibe-coded.jpg)

> "Under the hood, design is just code, and AI writes shockingly good code. So, why can't it do the same with design? We're creating more websites and software than ever before. So, why does it all look the same?"

![Lovable's stats: 100,000 new projects built on Lovable every day.](frames/00-19-lovable-stats.jpg)
![Vibe coded site: "Calculate Your Crypto Gains", dark, with an orange bar chart.](frames/00-20-vibe-coded-site-crypto.jpg)
![Vibe coded site: "Start it. Build it. Launch it.", dark, with a purple gradient headline.](frames/00-21-vibe-coded-site-encubator.jpg)
![Vibe coded site: "Grow Your Etsy Shop with Every Tool You Need", dark hero, then four tool cards with "Pro" badges.](frames/00-21-vibe-coded-site-listinglift.jpg)
![Vibe coded site: "Plan Your Meals Like a Grandmaster", dark, with gold text.](frames/00-21-vibe-coded-site-meals.jpg)
![Vibe coded site: "Turn TypeScript Into AI-Ready Context", dark, with a purple accent and a node diagram.](frames/00-22-vibe-coded-site-logicstamp.jpg)

> "The answer has less to do with code and more to do with this, the infamous Magic Mouse from Apple."

![Apple Magic Mouse and keyboard on a wooden desk.](frames/00-26-magic-mouse.jpg)

> "There are three tells to any vibe coded software and only the first one is actually about how the design looks."

The three tells are **purpose**, **complexity** and **connection**. Underneath all three is **opinion**.

### Tell 1: Purpose. AI copies what something looks like, not why it's there

**The arrow that points down while the menu opens up**

> "Why does this arrow point down if when I click it, the popover appears above? Arrows tend to point towards where something's about to appear, down, to the right, to the left, but rarely up. This comes from drop downs where the menu really did open downwards. But when the account button moved to the bottom of the screen, the menu only had one place to go, up. And the arrow stayed, because by then it had stopped meaning down and had started telling people there's a menu here."

![Sidebar bottom-left: green "J" avatar, "James Smith" and a chevron pointing DOWN, plus an info icon on the right.](frames/00-34-sidebar-arrow-points-down.jpg)
![The same account button clicked: the popover (name, email, Dark mode, Settings, Language, Need help?, Log out) opens UPWARD, above the down chevron.](frames/00-58-account-popover-opens-up.jpg)
![Where the convention comes from: an "Assignee / Assign a team member" select whose down chevron opens a list below it.](frames/00-45-dropdown-arrow-down.jpg)
![Old Internet Explorer toolbar (File, Edit, View…, Back/Forward with small down-arrow dropdowns): menus that really did open downwards.](frames/00-54-old-dropdowns-opened-down.jpg)

> "But AI doesn't really understand why the arrow is here. It just copies the result. And for a sidebar like this, that works out fine."

!["A sidebar like this": a plain sidebar whose bottom account button ("Kole · Free" plus a chevron) only needs to say "there's a menu here".](frames/01-04-sidebar-account-button.jpg)

A copied convention is fine **when its meaning survived the move**. The arrow still means "there's a menu here".

**Viewers added:** don't "fix" a convention people already read correctly.

> "As a user, I'd be annoyed to see the up arrow. Everyone knows what the down arrow means." (@sdstorm)

**The email that answers nothing**

> "Visually, it looks normal enough. But why is my email at the top? I know my email. My name is already there to tell me I'm logged in. Here's that same account popover in Notion. It also has an email, but right underneath are two workspaces and then a whole other login with its own set. So here the email is answering a question of which account you're actually in. But for our popover, it's not needed at all."

![Our popover, zoomed: "James Smith" circled in red with "james@acme.com" right under it. The email repeats what the name already says.](frames/01-15-popover-email-under-name.jpg)
![Notion's account popover: email "jainkole@gmail.com" with workspaces Personal and YouTube, then a second email "kolejain@gmail.com" with "Kole's Notion" (checked), each with "+ New workspace", then "Log out all accounts". Here the email tells you which account you're in.](frames/01-23-notion-popover-two-logins.jpg)
![Notion's full popover: Kole's Notion header with Upgrade, Settings, Invite members, Add account, then both logins and their workspaces.](frames/01-32-notion-popover-full.jpg)

> "Simply imitating what we see in Notion or any other application doesn't work as well as the down arrow, because now the reason why it's there actually starts to matter."

**Viewers added:** the email isn't wrong everywhere. It's right exactly where a person can hold more than one account.

> "I have 2 emails and I appreciate when a tool shows me if I am using my work email or my personal one." (@stacy_isa)

> "That's the point, it doesn't apply to all products. Some products are B2B only. Then you're not using your personal email. Yet you will see it in the profile picker of every app because it's everywhere, so AI copies it." (@JKP836)

**The cubicle: parts survive, reasons don't**

> "When Herman Miller released the Action Office in the 1960s, it looked nothing like this. The walls were diagonal at 120°. You could stand or sit at the desk. And the idea behind it was flexibility, not conformity. But companies figured out that if you rotate those walls to 90°, you could fit a lot more people on one floor. By the 1990s, around 40 million Americans were working in them."

![Black and white photo of the original Herman Miller Action Office, with open, flexible desks and a standing-height surface.](frames/01-44-action-office.jpg)
![Action Office layout with yellow lines tracing walls joined at 120°, and people working at open stations.](frames/01-50-action-office-120-walls.jpg)
![The result: a sea of 90° cubicles seen from above, packed with workers.](frames/01-48-cubicles-90-degrees.jpg)
![Cubicle office with a counter running up toward 40 million workers.](frames/02-03-cubicle-worker-count.jpg)

> "When you take the parts of something designed for one purpose and reshape them to fit somewhere else, whether that's your product or the 32nd floor of an office, the parts survive, but the reason behind it doesn't always."

**The fix: design from expectation, not from examples**

> "So, when we look at our own account popover, we need to ask, what would someone expect when they open this? In a task tracker, we might want to set an away status, pause notifications, or switch workspaces. And with our help icon easily accessible, we don't need that in the menu, too. You could change this to an agentic coding interface or an interior design software and the options and styling all change with it. Not based on what 10 others look like, but based on what the user actually needs."

![The redesigned task tracker popover: James Smith (PRO), then "Set yourself as away", "Pause notifications ›", "Switch workspace ›", then Settings, Dark mode, Log out. "Need help?" is gone because the help icon sits next to the button.](frames/02-33-popover-task-tracker.jpg)
![The same popover for a different kind of product (e.g. interior design software): Render credits 42/100, Storage, Plan & billing, Settings, Download the app, Log out. Several variants are stacked to show the options change with the product.](frames/02-40-popover-other-product.jpg)

> "Because AI is really good at copying what something looks like, but not the reason why it's there."

### Tell 2: Complexity. Someone has to carry it

> "So now we have components that are purposeful and they look like they belong, but that all falls apart once you start actually using them."

**The date picker**

> "Which of these interactions looks simpler to use? This one where I can select the date from three dropdowns, or the second where I have a whole calendar and can type in the date. The first might seem pretty simple, but what if I'm trying to select this upcoming Friday? Now, I need to figure out what day it is today and then count those days ahead to figure out that Friday lands on the 12th. In the second, you get a whole calendar that shows you exactly what you need."

![Option 1: three separate dropdowns, Day (DD), Month (MM), Year (YYYY). The DD list is open showing 1, 2, 3… with no weekdays, so "this Friday" means counting.](frames/03-00-date-three-dropdowns.jpg)
![Option 2: a table cell with a "Select a date…" input and a calendar for Oct 2026. Today (the 7th) is circled in red and the cursor is on Friday the 12th, readable at a glance.](frames/03-17-date-calendar.jpg)

> "And this is how date pickers started, since they were easier to build, up until about the 2010s, and then evolved into a calendar for better usability. And notice just how these evolved by placing more complexity on the creator in order to make the user's experience easier."

![How it started: an old social network signup form with "Birthday: Month ▾ Day ▾ Year ▾" dropdowns.](frames/03-19-old-date-dropdowns-signup.jpg)
![Where it went: a modern CRM table (Attio-style) with a calendar popover on a date column, including "Today", "Tomorrow" and "Next week" shortcuts.](frames/03-25-date-calendar-in-table.jpg)

**Viewers added:** the calendar isn't always the answer. Which input carries the complexity depends on the date.

> "The thing about datepickers is that it's context dependent. If the user is picking a time in the future and there's an unknown variable to the user, such as the day of the week or date, then yes, a datepicker is great. If it's a date that's already known to the user and other variables are either known or don't matter, e.g. a birthday or a date in the past, then a datepicker is actually counterproductive, and allowing the user to type in the date is much better UX." (@guineapiig8089)

> "It's not hard anymore to allow both: a typeable text field with a calendar button on the right for the popup." (@Salzui)

> "At work, I regularly input dates. We do it like that. We have a box I can type in and a datepicker. I almost exclusively use the box unless I'm incrementing a timeframe, because we got a convenient +3 month button." (@RunaKovacs)

So: a calendar for picking a future date by weekday ("this Friday"), and typing for a date the user already knows (birthday, issue date, anything in the past). Where both happen, offer a typeable field with a calendar button, plus shortcuts for the common jumps (Today, Next week, +3 months).

**Tesler's law**

> "This is what's called Tesler's law, named after Larry Tesler, the guy who invented cut, copy, and paste. He argued that complexity can't be removed, only relocated. So either the system carries it or the user will have to."

**Onboarding: explaining vs building**

> "I've just entered a new app and I'm on the homepage. Now what should I be doing, creating a project first or adding my team members? Clearly, we need an onboarding. And on a vibe coded app, this is almost always what you get. A modal with a few slides to click through. It's all telling and no showing."

![The new app's homepage: "Hi James!" and an empty "Ask anything" box. Nothing says whether to create a project or invite the team first.](frames/04-06-new-app-homepage.jpg)
![Shown at "Clearly, we need an onboarding": a real app's first-run modal, "Start with a team?", offering three ready-made helpers (Hilo, Rex, Dot) with "Not now" and "Add the team".](frames/03-52-onboarding-real-app-modal.jpg)
![Shown at the same moment: Quo's onboarding asks "Why do you need Quo?" (Signing up to another service / Travelling or living abroad / A general purpose second number / Other).](frames/03-53-onboarding-asks-why.jpg)
![The vibe coded version: a centered modal "Work seamlessly with your team" with an illustration, "1 of 3", Skip and Continue. Slides that explain.](frames/03-55-onboarding-slides-modal.jpg)

> "Picture two groups. One gets this onboarding and the other gets nothing. And they both try the same tasks. Who does better? The Nielsen Norman Group found that the group that sat through it succeeded 91% of the time versus 94% for those who skipped it. The onboarding didn't help at all because the modal doesn't remove complexity. It just explains your app."

> "So, what if instead we start by asking a few questions, like the features they need and the departments in their company? Then we walk them through a flow to set up their very first project and let them know that this is all editable later so that they're never locked in. Because a bad onboarding explains your product and a good one builds it into what the user actually needs."

![The building version: a "Workspace setup, 2 of 3" step asking for theme (Light / Dark previews) and "Day of the week starts on" (Sunday / Monday), then Continue. It configures the product instead of describing it.](frames/04-41-onboarding-setup-flow.jpg)

> "The complexity here is prioritizing the most important features and dropping everything else, which is what makes this so difficult, and it's exactly why vibe coded apps rarely have a good one."

> "Notion and Canva both start their onboardings with asking you what you'll use the product for and then tailor the workspace to what you need."

### Tell 3: Connection. A product isn't just a bunch of pages

> "But a product isn't just a bunch of flows. Take this task page. It doesn't look vibe coded. I can change the status, the dates, the project, even see the activity. I'm about to assign someone to it, but first I want to see who's already working on it. So, I have to remember the project's name. Go to projects, find it, open it, and then there's my answer. That was four clicks, and I've completely left the task that I was previously on."

![The task page: "Organize design team offsite", a description, "+ Add sub-tasks", Activity (Eva Knoll updated 2 attributes, Alex Black created this task), and a Details panel: Type EVENTS, Status IN PROGRESS, Owner "Add owner", Budget $4,500, Due June 14th, Project "Yearly Events". "Yearly Events" is plain text, not a link.](frames/05-38-task-page.jpg)
![Details panel close-up with the cursor about to set Owner. Who's on the project isn't visible from here.](frames/05-41-task-page-details.jpg)
![The four-click detour: the sidebar (Home, Tasks, Projects, Members, Sprints, Agents; Teams: Sales, Engineering) with the cursor on Projects, leaving the task.](frames/07-10-sidebar-projects-four-clicks.jpg)
![Where the answer was: the project page's details (Status IN PROGRESS, Lead Eva Knoll, Assignees "Eva, James, Albert, Joseph", Budget, Dates Mar 18 – Sept 15, Team Human resources).](frames/07-13-task-details-with-links.jpg)

**Wikipedia**

> "Now, try picturing the Wikipedia navigational menu. Can you? It has one. A main page, contents, current events. Like me, you've probably used Wikipedia for years. Never once visited it. So, how have you been getting around? Almost every noun in an article is a link to more information. You never go back to a menu because the page you're on already links everywhere you'd want to go next."

**Why vibe coded apps are disconnected**

> "On a vibe coded app, every prompt usually becomes a page. Tasks, then projects, members, sprints, agents. Each prompt was only instructed to build a feature. So every page connects to the sidebar but never to each other."

**Links, then previews**

> "Following what Wikipedia does, the project becomes a link and the owner becomes a link. And now we can jump from details page to details page and completely skip the sidebar. But that still kind of removes you completely from what you were working on."

> "When you just need a quick answer, hover on a link and get a preview without ever leaving the page."

> "But check out what that card actually had to know. Not everything about the project, just what I'd want to know from a task. Who's assigned and the status and team."

![Hovering "Yearly Events" on the task page opens a small dark card with only Status (IN PROGRESS, 31 tasks), Team (Human resources) and Assignees (avatars: Eva, James, Albert, Joseph). The answer, without leaving the task.](frames/06-58-hover-preview-card.jpg)

### Underneath it all: opinion

> "AI chose what content was important in the account popover. And that was exactly what was off about this card at the start of the video. It's exactly why sites like this instantly reek of being vibe coded. Then again, it shows simplicity for us, the creator, over the user with date selection. And it shows how your whole product fits together around the things you asked for, not the work that the user is trying to do."

![Recap: the vibe coded profile card from the opening. AI chose its content (banner, CREATOR tag, every stat) with no reason behind it.](frames/07-21-recap-card.jpg)

**Viewers added:** the same averaging shows up in copy and layout.

> "AI doesn't truly grasp UX or how to connect the dots. It writes pretty captions that say nothing and builds disconnected modules instead of a real user flow." (@ho8753)

> "It's almost never the font. More often it's even padding and the three feature cards that could belong to anyone. … And leave the beige palette in the hotel lobby where it belongs." (@bachatabrainbreaks)

> "When you vibe code, your product has no opinion. And unless instructed otherwise, it produces the average of whatever it's been trained on, which is basically the exact opposite of the Magic Mouse."

> "The story goes that Apple wanted this clean, cordless and effortless feeling mouse, which isn't really possible when you have a cord. That was considered along with the fact that you don't charge mice all that often. And so, the designers at Apple chose form over function. Now, right or wrong, the point is that they took all these details and formed an opinion on how their product should work. An opinion that they were willing to be disliked for."

![The Magic Mouse on a desk: clean and cordless. Form over function, on purpose.](frames/07-43-magic-mouse.jpg)

### The four questions

> "So, before you prompt, answer the questions that this entire video has been asking."

1. **Why would someone open this?**
2. **How are they actually using it?**
3. **What would they want to know next?**
4. **What are you willing to leave out?**

---

## How to run it on a page

The target can be a file, a route in the codebase, a running page, a screenshot or a Figma frame. Read the real thing: open the code, and render or screenshot it when you can. Don't review from a file name.

### Step 0: Set the product's opinion

Before looking at any component, write down:

- **Product and user**: what the product is, who uses this page, and their job right now. Get it from the code, copy, routes and data model. If you can't tell, say what you assumed. Ask the user only when a wrong guess would change the verdicts.
- **Page job**: one sentence, in the form "The user comes here to ___, then goes on to ___."
- **Critical user journeys**: the 2–4 real paths through this page, step by step (for example "first visit → set up the first project", "daily: open task → assign someone → back to the list"). Judge every component by the journeys it sits on, not as a standalone page.

  > "Modern agentic UX development typically works with visual references and CUJs rather than prompting out individual pages, because you need to be able to regenerate the entire artifact from the spec." (@KirinDave, a viewer)
- **The opinion**: what this product cares about more than the average app, and what it is willing to leave out. If none exists, say so. That is finding #1.

Every verdict below is measured against this, never against "what other apps do".

### Step 1: Break the page into components

List the page as a tree, from regions down to components down to the content inside them:

```
Page: Task detail
├─ Sidebar
│  ├─ Workspace switcher
│  ├─ Nav list (Tasks, Projects, Members, Sprints, Agents)
│  └─ Account button → Account popover
│        ├─ Name + email
│        ├─ Dark mode toggle
│        └─ Settings / Language / Need help? / Log out
├─ Task header (title, description)
├─ Details panel (Type, Status, Owner, Budget, Due, Project)
└─ Activity feed
```

For each node, note its **content** (what it shows), **affordances** (what you can do with it) and **states** (empty, loading, error, open/closed). Popovers, menus, modals and hover states count as components too. Open them.

### Step 2: Look at each component through the three tells

Ask these of every component, and of every piece of content inside it:

**Purpose** (the arrow, the email, the cubicle)

- Why is this here? What question does it answer for _this_ user on _this_ page?
- Was it copied from somewhere (another app, a template, a component library default, the AI's average)? If so, did its reason come with it?
  - The reason survived, as with the down arrow: keep it. Don't "fix" a convention people already read correctly.
  - The reason didn't survive, as with the email: cut it or change it. Check the real condition first: an email belongs in the account menu exactly when a person can hold more than one account here (work and personal, several logins). In a B2B-only product it doesn't.
- Does the copy say something? Read every headline, label, empty state and helper text. If it could sit in any other app unchanged ("Work seamlessly with your team"), it's a caption that says nothing. Rewrite it to name this product's actual thing, or cut it.
- What would someone _expect_ when they open this? Do the options come from that expectation, or from what ten other apps have?
- Is it already available somewhere else on the screen, like the help icon next to the "Need help?" item? Then one of them goes.

**Complexity** (the date picker, Tesler's law, onboarding)

- Who carries the complexity, the system or the user? Point to the exact step where the user does the work: counting days, remembering a name, re-typing something the system already knows.
- Was this built the way that's easiest to _build_ rather than easiest to _use_? Three dropdowns vs a calendar. A slides modal vs a setup flow.
- Does the input fit what the user already knows? A date the user knows (birthday, a past date) → typing. A date they pick relative to today ("this Friday") → a calendar. Both → a typeable field with a calendar button and shortcuts for common jumps. The same test works for any input: is the user recalling a value or choosing one?
- Does it _explain_ or _build_? Tooltips, tours and empty-state essays explain. Smart defaults, pre-filled setup and "you can change this later" build.
- What does it prioritize, and what did it leave out? A component that gives everything equal weight has no opinion.

**Connection** (the task page, Wikipedia, hover previews)

- Which nouns on this component are other objects in the product (people, projects, teams, files)? Is each one a link?
- What would the user want to know next from here? Can they get it without leaving: inline, on hover, in a peek?
- If there's a preview, does it show only what matters _from this context_? From a task, a project preview needs assignees, status and team, not everything about the project.
- Count the clicks to the most likely next question. Four clicks and losing your place is a finding.

### Step 3: Look across components

Problems that only show up when the parts are seen together:

- **Map how pages connect.** Draw which pages link to which. If everything only connects back through the sidebar ("every prompt became a page"), that's the main finding.
- **Duplicates and contradictions**: the same action in two places, or two components that disagree on what matters.
- **Shared reasons**: components that should change together when the product changes. Turn the task tracker into an agentic coding tool or interior design software, and the account popover's options _should_ change with it. If nothing would change, the component was designed for no one.
- **Journeys**: walk each critical user journey from Step 0 through the components, step by step. Where does the user carry complexity the system could take? Where does a journey dead-end or bounce back to the sidebar?
- **One opinion**: do all the components serve the same idea of what the product is for? Name the one that is "the average".
- **Generic visual tells**: the averages that make a page read as anyone's. Look for equal padding and equal weight on everything (nothing is more important than anything else), a row of three interchangeable feature cards, and a default palette nobody chose. Treat these as symptoms of a missing opinion, and fix the opinion (what matters most here), not just the pixels.

### Step 4: Check against real products with Mobbin (when it's connected)

Look for Mobbin tools first. In Claude Code they are named like `mcp__claude_ai_Mobbin__search_screens`, `search_flows` and `search_sections`. If they are deferred, load them with ToolSearch (`+Mobbin search`). If they aren't there, skip this step and say Mobbin wasn't available.

When it is available, use it for the components and flows that got findings:

- `search_screens` for a single screen or component context ("task tracker account menu", "project management task detail page").
- `search_flows` for journeys ("project management app onboarding", "invite teammate").
- `search_sections` for marketing and website sections ("pricing section", "hero").
- Use the platform the page targets (`web` or `ios`). Keep `task_intent` and `output_destination` the same across calls: `code` in a repo, `doc` for a written review.

**Use Mobbin for reasons, not looks.** The whole point of this skill is that copying the result is the tell. For each reference, say _why_ that product made its choice ("Notion shows the email because there are two logins underneath") and whether that reason applies here. Never write "Linear does X, so do X".

Look at the returned images, not only the metadata. Cite every screen as a link to its `mobbin_url`. If a result includes `ai_usage_notice`, show its text word for word as its own block.

### Step 5: Apply

If the user asked you to apply or fix, or the target is code in this repo and the change is clear, make the changes:

- Cut content whose reason didn't survive. Deleting is the most common intent fix.
- Move complexity onto the system: better inputs, defaults, pre-filling, inline creation.
- Turn nouns into links, and add previews that show only what's needed from this context.
- Replace explaining onboarding with building onboarding when the page is an entry point.
- Match the codebase's existing components and patterns. Intent fixes don't mean a restyle.

Anything that needs a product decision (what to leave out, who the user is), ask instead of guessing.

---

## Output format

```
## Design intent: <page>

**Page job:** The user comes here to ___, then goes on to ___.
**Product opinion:** <what this product cares about / will leave out, or "none: it's the average">
**Critical journeys:** 1. <step → step → step>  2. ...

### Components
<tree from step 1>

### Findings
| # | Component | Tell | What's wrong (the reason that didn't survive / who carries it / where it dead-ends) | Fix |
|---|-----------|------|------|-----|

### Across the page
- Connections: <map summary, sidebar-only links, click counts>
- Duplicates / contradictions: ...
- Opinion: ...

### Real-world references (Mobbin)
- [App, screen](mobbin_url): why they did it, and whether that reason applies here

### The four questions
1. Why would someone open this? ...
2. How are they actually using it? ...
3. What would they want to know next? ...
4. What are you willing to leave out? ...

### Applied
<changes made, or "not applied: needs decision on ___">
```

Order findings by impact on the user's main job, not by where they sit on the page. Leave a component out of the findings when it has a clear reason and needs nothing. Not everything copied is wrong: "for a sidebar like this, that works out fine."

## Rules

- Judge against the user's job and the product's opinion, never against "what others look like".
- Every finding names the specific content, the specific reason it fails, and a specific fix. "Feels generic" is not a finding.
- Don't restyle. Colors, spacing and fonts are out of scope unless they block a reason (for example, an arrow that points the wrong way and so says the wrong thing), or they're one of the generic visual tells in Step 3, where the fix is giving the page a hierarchy, not a new theme.
- Don't invent user research. When intent can't be known from the product, say so and ask.
