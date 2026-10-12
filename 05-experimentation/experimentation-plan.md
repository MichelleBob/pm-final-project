# A/B Experiment Brief, Riverty business-unit case

## Parameters
| Parameter | Decision |
|---|---|
| Feature under test | ree amount input alongside the slider: Typing a number lets a variable-income debtor enter their own figure instead of dragging from the 150 anchor. |
| Persona | A debtor with variable income and multiple claims who is looking for a quick answer. |
| Expected outcome | ariable-income debtors choose an instalment amount based on their own realistic income instead of the preset, and complete the slider step with confidence instead of dropping off or calling. Fewer plans are created at amounts that are too high. |
| Primary success metric | Slider-step completion rate with an actively chosen amount, i.e. the share of users who reach the payment slider and complete it with a value they changed from the preset. The drop-off rate from 26.8% should be decreased. |
| Baseline rate | 19% of new instalment agreements missing payments in 60 days |
| Guardrail metric | Share of new instalment agreements missing a payment within 60 days. Must not exceed the 19% baseline; segmented by chosen amount vs. preset default. |
| Guardrail boundary | Payment rate must not drop more than 1 pt |
| Second guardrail | · |
| Minimum Detectable Effect | +5 pts |
| Sample size per arm | 1000 |
| Traffic split | 10/10/80 |
| Test duration | 14 days |
| Significance threshold | p < 0.05 |

## Control vs. Variant
- **Control (A):** No one can tell what's affordable, and nobody explains why they're asking. 
Workflow:
1. Arrive with one question ("can I pay in pieces?") and scan dense case information for the answer.
2.Hit the slider and read the preset value as the recommended or "normal" amount, because there's no variable-income input to override it with.
3.Hit the income request without knowing why it's needed, who sees it, or whether it affects credit.
4. Resolve the uncertainty in one of four ways:
a. Call the contact centre, mainly for reassurance about what happens after a missed payment, not because information is missing.
b. Under-report income deliberately to avoid giving a "wrong" answer.
c. Accept the default and silently commit to an amount that's too high.
d. Abandon, in some cases without starting at all.
- **Variant (B):** Screen descriptions (exactly 3)
Entry point (case overview): Greeting with first name, two placeholder claim rows, one combined total (€1,240), and a single primary button, “Pay in instalments”.
Feature core (choose your amount): Title “How much can you pay per month?”, the total open balance, an empty euro field, a slider with a muted thumb at the minimum, always-visible bounds (€52 to €1,240), an inline error area, and a disabled “Set up plan” button. Should-haves: the one-line hint “Base it on a weaker month, not your best one.” and a numeric keypad with a € suffix.
Success / confirmation: “Your plan is set up”, chosen amount, number of instalments, last instalment if it differs, “Need to change something? Contact us.”, and a debug strip showing amount_source. No “back to edit” link.

Functional requirements
ID	Rule
FR1	A valid typed value moves the slider within 100 ms, and the field never changes from what was typed.
FR2	Dragging the slider fills the field and sets amount_source = slider; typing afterwards sets typed.
FR3	On first render the field is empty, the placeholder has no digit, and no amount is stored.
FR4	Min and max are always visible next to the field.
FR5	Whole euros only. Decimals and other characters are rejected with “Please enter whole euros.” and nothing is rounded silently.
FR6	A value outside the range shows an error naming the limit and keeps “Set up plan” disabled, so no invalid value reaches screen 3.
FR7	Tapping “Set up plan” re-runs the same validation on the stored state and blocks on failure.
- **Held constant (isolation check):** the existence of a slider

## Hypothesis
> I believe that ree amount input alongside the slider: Typing a number lets a variable-income debtor enter their own figure instead of dragging from the 150 anchor. for A debtor with variable income and multiple claims who is looking for a quick answer. will result in ariable-income debtors choose an instalment amount based on their own realistic income instead of the preset, and complete the slider step with confidence instead of dropping off or calling. Fewer plans are created at amounts that are too high., as measured by a +5 pts change in Slider-step completion rate with an actively chosen amount, i.e. the share of users who reach the payment slider and complete it with a value they changed from the preset. The drop-off rate from 26.8% should be decreased. within 14 days. We will protect Share of new instalment agreements missing a payment within 60 days. Must not exceed the 19% baseline; segmented by chosen amount vs. preset default. throughout the test.

## Shipping criteria
> We will **ship** if Slider-step completion rate with an actively chosen amount, i.e. the share of users who reach the payment slider and complete it with a value they changed from the preset. The drop-off rate from 26.8% should be decreased. improves by ≥ +5 pts at p < 0.05 and Share of new instalment agreements missing a payment within 60 days. Must not exceed the 19% baseline; segmented by chosen amount vs. preset default. does not reach Payment rate must not drop more than 1 pt after 14 days.
> We will **iterate** if direction is positive but lift is below the MDE.
> We will **kill** if the primary metric shows no improvement or moves negatively.
> The read date is fixed at the end of 14 days, no results reviewed before this date.
