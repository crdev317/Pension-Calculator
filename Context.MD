# Pension Calculator

A simple calculator that estimates what a UK saver's defined contribution pension could be worth when they retire, shown in today's money.

## Language

### Inputs

**Current age**:
The user's age today, in whole years.

**Retirement age**:
The age, entered by the user, at which they plan to start taking money from their **Pension pot**.
_Avoid_: State Pension age, pension age

**Pension pot**:
The combined value of all the user's defined contribution pensions, treated as one pot invested until **Retirement age**.
_Avoid_: pension fund, savings, "pension" on its own

**Contribution**:
The amount the user pays into their **Pension pot** each month, in **Today's money** and before **Tax relief** is added.
_Avoid_: payment, premium, deposit

**Tax relief**:
The basic-rate top-up added to each **Contribution**: £20 for every £80 the user pays.
_Avoid_: tax rebate, government bonus, top-up

### Limits

**Annual allowance**:
The yearly limit, currently £60,000, on what can go into a **Pension pot** with **Tax relief**, counting the **Tax relief** itself.
_Avoid_: AA, pension allowance, contribution limit

**Minimum pension age**:
The earliest age at which the user can take money from their **Pension pot**: 57, the UK minimum from 6 April 2028.
_Avoid_: pension access age, NMPA, early retirement age

### Assumptions

**Assumption**:
A fixed value, set by the calculator rather than the user, that the **Projected pot** depends on: the **Growth rate**, **Charges** or **Inflation**.
_Avoid_: default, setting, parameter

**Growth rate**:
The assumed yearly rate at which the **Pension pot**'s investments grow, before **Charges** and **Inflation**.
_Avoid_: return, interest, investment return

**Charges**:
The yearly cost of running the pension, as a percentage of the **Pension pot**'s value.
_Avoid_: fees, AMC, management charge

**Inflation**:
The assumed yearly rise in prices, used to express the **Projected pot** in **Today's money**.
_Avoid_: CPI, cost of living

**Net growth rate**:
The yearly rate at which the **Pension pot** grows in **Today's money**: the **Growth rate**, less **Charges**, adjusted for **Inflation**.
_Avoid_: real return, real growth rate

### Results

**Projected pot**:
The **Estimate** of the **Pension pot**'s value at **Retirement age**, in **Today's money**.
_Avoid_: forecast, final pot, pension value, retirement fund

**Estimate**:
What every result is: an illustration based on the user's inputs and the **Assumptions**, never financial advice or a guarantee.
_Avoid_: quote, prediction, guarantee, advice

**Today's money**:
A value adjusted for **Inflation** to show what it would buy at today's prices.
_Avoid_: real terms, present value

## Relationships

- A **Contribution** has **Tax relief** added before it goes into the **Pension pot**.
- A **Contribution** stays the same in **Today's money** until **Retirement age**, so in cash terms it rises with **Inflation**.
- A **Contribution** is at most £4,000 a month, because with **Tax relief** that reaches the **Annual allowance** of £60,000 a year.
- **Retirement age** is later than **Current age** and no earlier than the **Minimum pension age**.
- The **Pension pot** grows at the **Growth rate**, less **Charges**, from **Current age** to **Retirement age**.
- The **Projected pot** is the **Pension pot**'s value at **Retirement age**, expressed in **Today's money** using **Inflation**.
- Every **Projected pot** is an **Estimate**, shown with the **Assumptions** that produced it.

## Example dialogue

> **Dev:** "If someone pays £80 a month, is their **Contribution** £80 or £100?"
> **Domain expert:** "The **Contribution** is the £80 they pay. **Tax relief** adds £20, so £100 goes into the **Pension pot**."
> **Dev:** "And the figure we show at the end, is that the cash they'll have when they retire?"
> **Domain expert:** "No. The **Projected pot** is in **Today's money**, so it's lower than the cash figure. And it's an **Estimate**, not a promise."

## Flagged ambiguities

- "Retirement age" was listed as an assumption in `Technical-Context.MD` — resolved: the user enters it, so it is an input, not an **Assumption**.
- "Pension" on its own could mean a defined contribution pot, a final salary (defined benefit) pension or the State Pension — resolved: the calculator covers only a defined contribution **Pension pot**.
- "Tax relief" could include higher- or additional-rate relief — resolved: only basic-rate relief, because higher-rate relief is claimed through a tax return and never reaches the **Pension pot**.
- The result could include tax-free cash or a retirement income — resolved: the calculator shows the **Projected pot** only.
- A **Contribution** could mean the same cash amount every month — resolved: it stays the same in **Today's money**, rising with **Inflation** in cash terms.
- A user with several pensions could enter one or all of them — resolved: the **Pension pot** is the total of all their defined contribution pensions.
- Some people can take money from a pension before 57 (a protected pension age, or reaching 55 before 6 April 2028) — resolved: the calculator uses 57 as the **Minimum pension age** for everyone.
