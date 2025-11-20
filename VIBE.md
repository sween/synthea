1. Quick disease profile for a first-pass SCD module

Very simplified, simulation-oriented view (not medical advice, just modeling assumptions):

Onset & genetics

SCD is inherited and present from birth.

In the US, SCD occurs in about 1 in 365 Black births and ~1 in 16,300 Hispanic births. 
CDC
+1

Trait is much more common (≈1 in 13 Black babies), but we can treat trait as “no SCD” in a first-pass model. 
CDC

Key clinical features we’ll simulate

Chronic condition: “Sickle cell disease” (ICD/SNOMED condition).

Recurrent vaso-occlusive pain crises → ED visits / hospitalizations.

Increased risk of early death, but we’ll keep the mortality logic simple at first.

Treatments we’ll roughly represent

“Hydroxyurea” (single generic RxNorm code, not getting into dosing).

ED/hospital pain management (opioid/NSAID as generic meds).

Modeling stance

We’ll intentionally oversample SCD so your test population actually contains enough SCD patients (you can later tune the probabilities to match CDC stats).

2. Synthea module design

We’ll implement this as a single GMF module with these core state blocks:

Initial → decide whether this patient has SCD at birth.

No_SCD → immediately Terminal (module does nothing for non-SCD patients).

SCD_Diagnosis → ConditionOnset for SCD.

Chronic_SCD → loop that:

waits some time (Delay)

sometimes triggers a Pain Crisis Encounter

Pain_Crisis_Encounter → ED encounter + simple pain meds.

Optional Hydroxyurea_Start state after first crisis.

Optional SCD_Death state with slightly elevated mortality.

Terminal → end of module progression (not necessarily death).
