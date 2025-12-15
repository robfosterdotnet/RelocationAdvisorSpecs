# Stakeholder Meeting Minutes

**Meeting:** Project Kickoff - Relocation Advisor
**Date:** [Your Start Date]
**Attendees:**
- Marcus Thompson (Product Owner)
- Sarah Chen (Tech Lead)
- David Park (Senior Developer)
- Andrew [Your Last Name] (Junior Developer) ← That's you!
- Jennifer Walsh (UX Designer - Contractor)

---

## Meeting Notes

### Marcus (Product Owner) - Business Context

> "Thanks everyone for joining. I'm excited to kick off the Relocation Advisor project.
>
> **The Problem:** Our client, a real estate consultancy, has customers who are looking to relocate but feel overwhelmed by all the factors involved - cost of living, job markets, climate, schools. They want a data-driven tool that helps people make informed decisions.
>
> **The Opportunity:** There's nothing on the market that combines all this data in one place with personalized weighting. People are either looking at 10 different websites or just going with gut feelings.
>
> **The Goal:** Build an MVP that demonstrates the value. If successful, this could become a SaaS product.
>
> **Timeline:** We have about 10 weeks for the initial build. We're a small team, so we need to be focused and pragmatic."

---

### Sarah (Tech Lead) - Technical Approach

> "Thanks Marcus. From a technical standpoint, we're going to keep this simple but professional.
>
> **Stack decisions:**
> - PostgreSQL for the database - it's reliable and handles geographic data well
> - Python with FastAPI for the backend - fast to develop, great performance
> - React with TypeScript for the frontend - industry standard, good for Andrew's resume
>
> **Architecture:**
> We'll follow a standard three-tier architecture. Nothing fancy. The complexity here is in the data, not the infrastructure.
>
> **Data is the hard part:**
> We're pulling from Census, BLS, and NOAA. Different formats, different update schedules, different geographic identifiers. Getting this data clean and normalized is probably 40% of the project.
>
> **Andrew:** I've documented our patterns and conventions. Please follow them - it'll make code reviews faster and keep the codebase consistent. Don't hesitate to ask questions, but do try to research first. I'd rather you spend 20 minutes Googling than an hour stuck on something, but I also don't want you to ask questions that are easily findable."

---

### Jennifer (UX Designer) - User Experience Notes

> "I'll be providing wireframes at the start of the frontend sprints. For now, know that our primary user is someone who:
>
> - Is considering relocating (job change, retirement, lifestyle)
> - Has specific priorities (some care about weather, others about affordability)
> - Wants to compare options side-by-side
> - May not be very technical
>
> **Key UX principles:**
> - Progressive disclosure (don't overwhelm with data)
> - Clear calls to action
> - Mobile-responsive (many users will be on phones)
> - Fast load times (nobody waits for slow data)"

---

### David (Senior Developer) - Technical Notes

> "I'll be working on some infrastructure pieces and code reviewing Andrew's work. A few notes:
>
> **Git hygiene:**
> - Small, focused commits
> - Meaningful commit messages
> - One feature per pull request
> - Always pull before you push
>
> **Code quality:**
> - Follow the patterns in the codebase
> - Write self-documenting code (good variable names)
> - Add comments for the 'why', not the 'what'
> - If you're copy-pasting, you're probably doing it wrong
>
> **Andrew:** I know you're new, so I'll be more available in the first few sprints. But try to build good habits now - it's harder to unlearn bad ones later."

---

## Action Items

| Owner | Action | Due |
|-------|--------|-----|
| Andrew | Set up development environment | End of Sprint 0 |
| Andrew | Initialize GitHub repository | End of Sprint 0 |
| Sarah | Share technical documentation | Done |
| Jennifer | Begin wireframe exploration | Sprint 5 |
| David | Set up code review process | Sprint 1 |

---

## Key Takeaways for Andrew

1. **Data is the complexity** - The app itself isn't that complicated; getting the data right is
2. **Follow the patterns** - Don't invent new ways to do things
3. **Ask questions wisely** - Research first, then ask with context
4. **Small commits** - Don't batch up a week of work into one commit
5. **MVP mindset** - Perfect is the enemy of done

---

## Questions to Consider

Before moving on, think about these questions:

1. Who is the end user of this application?
2. What makes this project different from just looking at multiple websites?
3. Why did Sarah choose PostgreSQL over something simpler like SQLite?
4. What did David mean by "comments for the 'why', not the 'what'"?

---

*Next: Read `02-requirements.md`*
