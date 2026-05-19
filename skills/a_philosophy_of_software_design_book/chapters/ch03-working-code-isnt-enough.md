# Chapter 3: Working Code Isn't Enough (Strategic vs. Tactical Programming)

## Core Idea
Adopt a strategic mindset: your primary goal is a great design that also happens to work. Treating "working code" as the goal produces tactical programming, which accumulates technical debt and slows development long-term.

## Frameworks Introduced
- **Tactical programming**: focus on getting features/fixes done quickly. Each task contributes a few new complexities. Optimizes the short term.
- **Strategic programming**: invest time to improve the design as you build. Continually make small improvements (proactive + reactive). Optimizes the long term.
- **The 10–20% rule**: spend 10–20% of total development time on design investments. You'll be slightly slower at first; within 6–18 months, the strategic approach pulls ahead permanently.
- **Tactical tornado**: a prolific developer who pumps out features tactically. Other engineers must clean up the wake; the tornado looks heroic, the cleaners look slow.

## Key Concepts
- **Investment mindset**: trade short-term effort for long-term productivity (the design equivalent of compound interest).
- **Technical debt**: the borrowed time from tactical shortcuts; usually never fully repaid.
- **Proactive investment**: extra design work up front, alternatives, documentation.
- **Reactive investment**: when you discover a design mistake, fix it (don't patch around it).

## Mental Models
- Think strategically: the long-term structure of the system is more important than the next feature shipping today.
- Use the "investment curve" image: tactical wins early then loses; strategic loses early then wins. Crossover ≈ 6–18 months.
- "Working code" is the floor, not the ceiling. Code must work AND have a great design.

## Anti-patterns
- **"Move fast and break things"** — tactical culture; eventually unsustainable (Facebook later changed to "Move fast with solid infrastructure").
- **Tactical tornado worship** — rewarding speed while ignoring the wreckage erodes design quality and recruiting.
- **"We'll clean it up later"** — once spaghetti, almost impossible to fix; "later" rarely arrives.
- **Quick patches around existing problems** — creates more complexity, requiring more patches.

## Key Takeaways
1. Your goal is great design that works, not just working code.
2. Make small, continual investments rather than big up-front design.
3. Fix design problems when discovered; don't patch around them.
4. Tactical programming is contagious — once you start, hard to stop.
5. Best engineers care about design; bad code bases drive away talent.
6. Investment pays off in 6–18 months; for the rest of the system's life you go faster.

## Connects To
- **Ch 1**: The strategic mindset is how you operationalize "fight complexity continuously".
- **Ch 16**: Stay strategic when modifying existing code, not just when greenfielding.
- **Ch 19.4 (TDD)**: Test-driven development risks reinforcing the tactical mindset (focus on features, not abstractions).
