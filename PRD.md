# Pension Calculator — PRD

## Problem Statement

People saving into a UK defined contribution pension don't know what their savings could be worth when they retire. Working it out by hand means combining years of monthly Contributions, Tax relief, investment growth, Charges and Inflation, which few people can do. A cash figure decades from now is also hard to judge, because it says nothing about what that money would buy. People want a quick, honest answer from a handful of details they already know, without handing personal information to anyone.

## Solution

A single-page calculator. The user enters four details: their Current age, their Retirement age, the value of their Pension pot today, and the Contribution they pay each month. They press Calculate and get one figure: their Projected pot, in Today's money. The figure appears next to the Assumptions behind it (Growth rate, Charges and Inflation), with a plain explanation of Today's money and a clear label that it is an Estimate, not financial advice or a guarantee. Basic-rate Tax relief is added to the Contribution automatically. Mistakes are explained next to the field, as soon as the user moves on. Nothing the user types leaves their device.

## Requirements

**Entering details**

1. As a user, I want to enter my Current age, so that the calculator knows how many years my Pension pot has to grow.
2. As a user, I want to enter the Retirement age I plan to retire at, so that I see what my Pension pot could be worth at that age.
3. As a user with several pensions, I want to enter the combined value of all my defined contribution pensions as one Pension pot, so that I get a single total for my savings.
4. As a user, I want to enter how much I pay into my pension each month, so that my future Contributions are included in the Estimate.
5. As a user, I want to enter my Contribution as the amount I actually pay, before Tax relief, so that I don't have to work out the top-up myself.
6. As a user with no pension savings yet, I want to enter £0 as my Pension pot, so that I can see what my Contributions alone could build.
7. As a user who isn't paying in, I want to enter £0 as my Contribution, so that I can see what my existing Pension pot alone could grow to.
8. As a user, I want to type amounts with or without a £ sign and commas, so that I can enter numbers the way I normally write them.

**Checking what I enter**

9. As a user, I want to be told next to the field, as soon as I move on, when something I've entered isn't valid, so that I can fix it before calculating.
10. As a user, I want every error message to say what to enter instead, so that I don't have to guess the allowed range.
11. As a user, I want to be stopped from entering a Retirement age below the Minimum pension age of 57, with a short explanation, so that I don't plan around taking money out earlier than the rules allow.
12. As a user, I want to be stopped from entering a Retirement age that isn't later than my Current age, so that the Estimate always covers a future period.
13. As a user, I want to be stopped from entering a Retirement age above 75, with a short explanation, so that the Estimate stays within the ages at which Tax relief is available.
14. As a user, I want to be stopped from entering a Contribution above £4,000 a month, with a short explanation of the Annual allowance, so that the Estimate never includes Tax relief I couldn't get.
15. As a user, I want to be stopped from entering a Current age outside 18 to 74, so that the calculator works only with realistic ages.
16. As a user, I want to be stopped from entering a negative amount or a Pension pot above £10,000,000, so that a typing mistake can't produce a meaningless result.
17. As a user, I want the Calculate button to show me what still needs fixing when I press it with missing or invalid details, so that I'm never left wondering why nothing happened.

**Calculating and the result**

18. As a user, I want to press one Calculate button to see my result, so that I control when the Estimate appears.
19. As a user, I want basic-rate Tax relief added to my Contribution automatically, so that the Estimate reflects the full amount going into my Pension pot.
20. As a user, I want my Contribution assumed to keep its value in Today's money until my Retirement age, so that the Estimate reflects me keeping up my saving as prices rise.
21. As a user, I want to see my Projected pot as a single figure in pounds, rounded to the nearest pound, so that the result is easy to read.
22. As a user, I want the Projected pot shown in Today's money, so that I can compare it with what things cost now.
23. As a user, I want a one-sentence explanation of Today's money next to the result, so that I don't mistake it for the cash figure I'll have.
24. As a user, I want to see the Growth rate, Charges and Inflation behind my result, next to it, so that I know what the Estimate is based on.
25. As a user, I want the result labelled as an Estimate that is not financial advice or a guarantee, so that I don't treat it as a promise.
26. As a user, I want the old result to disappear as soon as I change any detail, so that I never read a result that doesn't match what I've entered.
27. As a user, I want to change any detail and calculate again, so that I can compare different Retirement ages or Contributions.

**Understanding the terms**

28. As a user new to pensions, I want terms such as Pension pot, Contribution and Tax relief explained in one sentence where they first appear, so that I can use the calculator without pension knowledge.
29. As a user, I want plain, friendly UK English throughout, so that the calculator is easy to follow.

**Privacy**

30. As a user, I want my details to stay on my device, never sent, stored or tracked, so that I can use the calculator without sharing personal information.
31. As a user, I want my details gone when I reload or close the page, so that nobody using the device after me can see them.
32. As a user, I want to use the calculator without accepting cookies, signing in or giving an email address, so that I get my answer straight away.

**Accessibility and devices**

33. As a keyboard user, I want to complete every field and calculate without a mouse, so that I can use the calculator fully.
34. As a screen reader user, I want each error message and the result announced when it appears, so that I know what has happened without seeing the screen.
35. As a user on a phone, I want the calculator to fit my screen without sideways scrolling, so that I can use it anywhere.
36. As a user with low vision, I want the page to meet WCAG 2.2 AA for contrast, text size and zoom, so that I can read and use it.

**Running the product**

37. As the product owner, I want every Assumption and limit kept as one named value, so that a change to the Annual allowance or the Growth rate is a single, visible edit.
38. As the product owner, I want the calculation checked against hand-worked examples, so that I can trust the figure the calculator shows.

## Implementation Decisions

- **A single static page with no backend.** Built with TypeScript and Vite, with zero runtime dependencies (`Technical-Context.MD`). All calculation happens in the browser, because user inputs never leave it (principle 2).
- **Nothing is stored or tracked.** No cookies, local storage, analytics or network requests carrying user inputs, and nothing in the page address. Because there are no cookies, there is no cookie banner.
- **The Assumptions are one fixed set.** Growth rate 5% a year, Charges 0.75% a year and Inflation 2.5% a year. They are moderate middle-of-the-road values, held as named values (principle 3), shown with every result, and not editable by the user.
- **Tax relief adds 25% to each Contribution.** This is basic-rate relief: an £80 Contribution becomes £100 in the Pension pot.
- **Limits.** The Contribution is at most £4,000 a month, which with Tax relief is the £60,000 Annual allowance. The Minimum pension age is 57.
- **Input ranges.** Current age 18 to 74. Retirement age 57 to 75 and later than the Current age, because Tax relief is not available on contributions from age 75. Pension pot £0 to £10,000,000, a cap that only catches typing mistakes. Contribution £0 to £4,000 a month. Ages are whole years and amounts are whole pounds, because pennies don't change an estimate meaningfully.
- **Input parsing.** A leading £ sign, commas and spaces are ignored in amount fields. Anything else that isn't a whole number is an error.
- **Calculation method.** The projection runs month by month in Today's money, for 12 × (Retirement age − Current age) months, starting from the Pension pot entered. Each month, the Pension pot first grows at the monthly real net growth rate, then that month's Contribution plus Tax relief is added. The yearly real net growth rate is (1 + Growth rate) × (1 − Charges) ÷ (1 + Inflation) − 1, and the monthly rate is (1 + yearly rate)^(1/12) − 1. Working in Today's money keeps the Contribution constant, as the glossary defines it, and needs no separate conversion at the end.
- **Precision.** Calculations use full precision. Only the displayed Projected pot is rounded, to the nearest pound.
- **When the result appears.** The Projected pot appears only when the user presses Calculate and every input is valid. Changing any input clears it until the user presses Calculate again.
- **Copy and formatting.** Plain UK English. Money is shown as pounds via the browser's built-in `en-GB` currency formatting, and percentages to two decimal places at most. Each pension term gets a one-sentence explanation where it first appears.
- **Accessibility.** WCAG 2.2 AA. Inline errors and the result are announced through live regions. Everything works by keyboard, and the layout reflows to a 320-pixel-wide screen without sideways scrolling.
- **Environments.** The product runs locally only, through the Vite dev server and production preview. Choosing a host is a separate, later decision, recorded as an ADR.

## Testing Decisions

- **A good test checks behaviour you can see from outside.** For the calculation that means inputs in, Projected pot out. For input checks it means a value in, a valid input or a message out. For the page it means what the user sees: the figure, the Assumptions, the messages. Tests never inspect internal state.
- **Projection: unit tests (Vitest, Tier 1).** Hand-worked reference cases that anyone can check with a calculator. For example, with every Assumption set to zero, a Current age of 30, a Retirement age of 67, a Pension pot of £10,000 and a Contribution of £100, the 444 months add £125 each, giving £65,500. Further cases cover a one-year projection, a £0 Pension pot, a £0 Contribution, the £4,000 maximum Contribution, and the default Assumptions against an independently worked figure.
- **Input checks: unit tests (Vitest, Tier 1).** Every rule, tested on both sides of each boundary: Current age 17, 18, 74 and 75; Retirement age 56, 57, 75 and 76, and equal to the Current age; Contribution £4,000 and £4,001; negative amounts; amounts typed with £ signs and commas; empty fields and non-numbers.
- **Calculator page: end-to-end tests (Playwright, Chromium).** These are the seam tests: they run form → calculation → result with nothing mocked. They cover the result, the Assumptions, the Estimate label, the error messages, the old result clearing when an input changes, and keyboard-only use. One test confirms that no network request is made after the page has loaded, so user inputs never leave the browser. The tests run against the dev server on every commit (Tier 1) and against a fresh production build on every PR (Tier 2).
- **Assumptions and limits** hold values only and get no tests of their own. The Projection and Input checks tests exercise them.
- **Prior art:** none yet. These are the first tests in the repo, and they follow the testing standard in `Technical-Context.MD`.

## Out of Scope

- Tax-free cash, retirement income, annuities and drawdown. The result is the Projected pot only.
- The State Pension, and final salary (defined benefit) pensions.
- Employer contributions and one-off lump-sum payments.
- Higher- and additional-rate Tax relief, the tapered Annual allowance, carry-forward of unused allowance, and the relief limit linked to earnings.
- Protected pension ages, and early access to a pension before 57.
- User-editable Assumptions, and low, medium and high growth scenarios.
- Charts and year-by-year breakdowns.
- Saving, sharing, printing or emailing results; accounts and sign-in.
- Analytics, error tracking and any other telemetry.
- Hosting and deployment.
- Languages other than English, currencies other than pounds, and users outside the UK.

## Further Notes

- The Annual allowance (£60,000) and the Minimum pension age (57 from 6 April 2028) reflect UK rules for the 2026/27 tax year. They need checking whenever those rules change.
- Deliberate simplifications: 57 is used as the Minimum pension age for everyone; basic-rate Tax relief is applied to every Contribution up to the cap without checking earnings; and the £4,000 cap applies to the Contribution in Today's money.
- The page is tested in Chromium only (`Technical-Context.MD`).
