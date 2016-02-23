# Cardiac Surgery Risk Calculator

The Society for Thoracic Surgery maintains an online [Adult Cardiac Surgery Risk Calculator](http://riskcalc.sts.org/stswebriskcalc) which can be used to estimate the risk of mortality and other complications in cardiac surgery patients. While the only required variables are procedure type, age, and sex, supplying additional information about the patient and procedure increases the accuracy of the score.

Although the calculator is designed for manual input of data fields on the webpage, the underlying API can be used to automate the calculation of the STS scores. This document describes this API in more detail.

## API Details

URL: `http://riskcalc.sts.org/stswebriskcalc/v1/calculate/stsall`  
Method: `POST`  
Content type: `application/json`

Example curl command:

```sh
curl -H "Content-Type: application/json" -X POST -d '{"age":70,"gender":"Male","opcab":"Yes, planned","heightcm":100,"weightkg":50}' http://riskcalc.sts.org/stswebriskcalc/v1/calculate/stsall
```

and the response:

```json
{
  "predmort": 0.00394,
  "predmm": 0.04148,
  "preddeep": 0.00115,
  "pred14d": 0.01323,
  "predstro": 0.00287,
  "predvent": 0.02133,
  "predrenf": 0.00638,
  "predreop": 0.0269,
  "pred6d": 0.7656
}
```

which corresponds to the likelihoods of:

- Mortality
- Morbidity or Mortality
- DSW Infection
- Long Length of Stay
- Permanent Stroke
- Prolonged Ventilation
- Renal Failure
- Reoperation
- Short Length of Stay

## Calculator Inputs

| Procedure Type | `.json` |
|---|---|
| CAB Only | `{ opcab: "Yes, planned" }` |
| AV Replacement | `{ opvalve: "Yes", vsav: "Yes, planned", vsavpr: "Replacement" }` |
| MV Replacement Only | `{ opvalve: "Yes", vsmv: "Yes, planned", vsmvpr: "Replacement" }` |
| MV Repair | `{ opvalve: "Yes", vsmv: "Yes, planned", vsmvpr: "Repair" }` |
| AV Replacement + CAB | `{ opcab: "Yes, planned", opvalve: "Yes", vsav: "Yes, planned", vsavpr: "Replacement" }`|
| MV Replacement + CAB | `{ opcab: "Yes, planned", opvalve: "Yes", vsmv: "Yes, planned", vsmvpr:"Replacement" }` |
| MV Repair + CAB | `{ opcab:"Yes, planned", opvalve:"Yes", vsmv: "Yes, planned", vsmvpr: "Repair" }` |

| Patient Characteristics | `.json` |
|---|---|
| Age &isin; [1, 110] | `{ age: 50 }` |
| Gender: Male | `{ gender: "Male" }` |
| Gender: Female | `{ gender: "Female" }` |
| Height (cm) &isin; [20, 251] | `{ heightcm: 100 }` |
| Weight (kg) &isin; [10, 250] | `{ weightkg: 100 }` |

| Hemo Data-EF | `.json` |
|---|---|
| Hemo Data-EF Done: No | `{ hdefd: "No" }`|
| Hemo Data-EF Done: Yes | `{ hdefd: "Yes" }`|
| Hemo Data-EF Value | `{ hdef: 10 }`|

| Heart Failure within 2 weeks | `.json` |
|---|---|
| Yes | `{ chf: "Yes" }` |
| No | `{ chf: "No" }` |
| Unknown | `{ chf: "Unknown" }` |

| Race | `.json` |
|---|---|
| Race Documented: Yes | `{ racedocumented: "Yes" }` |
| Race Documented: No | `{ racedocumented: "No" }` |
| Race Documented: Patient declined to disclose | `{ racedocumented: "Patient declined to disclose" }` |
| Race - Black / African American: Yes | `{ raceblack: "Yes" }` |
| Race - Black / African American: No | `{ raceblack: "No" }` |
| Hispanic or Latino or Spanish Ethnicity: Yes | `{ ethnicity: "Yes" }` |
| Hispanic or Latino or Spanish Ethnicity: No| `{ ethnicity: "No" }` |
| Hispanic or Latino or Spanish Ethnicity: Not Documented | `{ ethnicity: "Not Documented" }` |
| Race - Asian: Yes | `{ raceasian: "Yes" }` |
| Race - Asian: No | `{ raceasian: "No" }` |

| RF | `.json` |
|---|---|
| RF-Renal Fail-Dialysis: Yes | `{ dialysis: "Yes" }` |
| RF-Renal Fail-Dialysis: No | `{ dialysis: "No" }` |
| RF-Renal Fail-Dialysis: Unknown | `{ dialysis: "Unknown" }` |
| RF-Last Creat Level &isin; [0.1, 30] | `{ creatlst: 20 }` |

| Cardiac Presentation/Symptoms - At Time Of This Admission | `.json` |
|---|---|
| Stable Angina | `{ cardsymptimeofadm: "Stable Angina" }` |
| Unstable Angina | `{ cardsymptimeofadm: "Unstable Angina" }` |
| Angina equivalent | `{ cardsymptimeofadm: "Angina equivalent" }` |
| Non-ST Elevation MI (Non-STEMI) | `{ cardsymptimeofadm: "Non-ST Elevation MI (Non-STEMI)" }` |
| ST Elevation MI (STEMI) | `{ cardsymptimeofadm: "ST Elevation MI (STEMI)" }` |
| Other | `{ cardsymptimeofadm: "Other" }`|
| No Symptoms | `{ cardsymptimeofadm: "No Symptoms" }` |

| Cardiac Symptoms - At Time Of Surgery | `.json` |
|---|---|
| Stable Angina | `{ cardsymptimeofsurg: "Stable Angina" }` |
| Unstable Angina | `{ cardsymptimeofsurg: "Unstable Angina" }` |
| Angina equivalent | `{ cardsymptimeofsurg: "Angina equivalent" }` |
| Non-ST Elevation MI (Non-STEMI) | `{ cardsymptimeofsurg: "Non-ST Elevation MI (Non-STEMI)" }` |
| ST Elevation MI (STEMI) | `{ cardsymptimeofsurg: "ST Elevation MI (STEMI)" }` |
| Other | `{ cardsymptimeofsurg: "Other" }`|
| No Symptoms | `{ cardsymptimeofsurg: "No Symptoms" }` |

| MI | `.json` |
|---|---|
| Prior MI: Yes | `{ prevmi: "Yes" }` |
| Prior MI: No | `{ prevmi: "No" }` |
| Prior MI: Unknown | `{ prevmi: "Unknown" }` |
| MI-When <=6 hrs | `{ miwhen: " <=6 Hrs" }` |
| MI-When >6 but <24 hrs | `{ miwhen: " >6 Hrs but <24 Hrs" }` |
| MI-When 1 to 7 days | `{ miwhen: "1 to 7 Days" }` |
| MI-When 8 to 21 days | `{ miwhen: "8 to 21 Days" }` |
| MI-When >21 days | `{ miwhen: " >21 Days" }` |

| Cardiac Arrhythmia | `.json` |
|---|---|
| Cardiac Arrhythmia: Yes | `{ arrhythmia: "Yes" }` |
| Cardiac Arrhythmia: No | `{ arrhythmia: "No" }` |
| Cardiac Arrhythmia: Unknown | `{ arrhythmia: "Unknown" }` |
| Cardiac Arrhythmia - Atrial Fibrillation: Paroxysmal | `{ arrhythafib: "Paroxysmal" }` |
| Cardiac Arrhythmia - Atrial Fibrillation: Continuous / persistent | `{ arrhythafib: "Continuous / persistent" }` |
| Cardiac Arrhythmia - Atrial Fibrillation: None | `{ arrhythafib: "None" }` |

| RF | `.json` |
|---|---|
| Chronic Lung Disease: Mild | `{ chrlungd: "Mild" }` |
| Chronic Lung Disease: Moderate | `{ chrlungd: "Moderate" }` |
| Chronic Lung Disease: Severe | `{ chrlungd: "Severe" }` |
| Chronic Lung Disease: Lung disease documented, severity unknown | `{ chrlungd: "Lung disease documented, severity unknown" }` |
| Chronic Lung Disease: No | `{ chrlungd: "No" }` |
| Chronic Lung Disease: Unknown | `{ chrlungd: "Unknown" }` |
| Cerebrovascular Dis: Yes | `{ cvd: "Yes" }` |
| Cerebrovascular Dis: No | `{ cvd: "No"  }` |
| Cerebrovascular Dis: Unknown | `{ cvd: "Unknown" }` |
| Prior CVA: Yes | `{ cva: "Yes" }` |
| Prior CVA: No | `{ cva: "No" }` |
| Prior CVA: Unknown | `{ cva: "Unknown" }` |
| Peripheral Arterial Disease: Yes | `{ pvd: "Yes" }` |
| Peripheral Arterial Disease: No | `{ pvd: "No" }` |
| Peripheral Arterial Disease: Unknown | `{ pvd: "Unknown" }` |
| Diabetes: Yes | `{ diabetes: "Yes" }` |
| Diabetes: No | `{ diabetes: "No" }` |
| Diabetes: Unknown | `{ diabetes: "Unknown" }` |
| Diabetes-Control: Diet only | `{ diabctrl: "Diet only" }` |
| Diabetes-Control: Oral | `{ diabctrl: "Oral" }` |
| Diabetes-Control: Insulin | `{ diabctrl: "Insulin" }` |
| Diabetes-Control: Other | `{ diabctrl: "Other" }` |
| Diabetes-Control: Other subcutaneous medication | `{ diabctrl: "Other subcutaneous medication" }` |
| Diabetes-Control: None | `{ diabctrl: "None" }` |
| Diabetes-Control: Unknown | `{ diabctrl: "Unknown" }` |
| Hypertension: Yes | `{ hypertn: "Yes" }` |
| Hypertension: No | `{ hypertn: "No" }` |
| Hypertension: Unknown | `{ hypertn: "Unknown" }` |
| Immunocompromise: Yes | `{ immsupp: "Yes" }`|
| Immunocompromise: No | `{ immsupp: "No" }`|
| Immunocompromise: Unknown | `{ immsupp: "Unknown" }`|
| Endocarditis: Yes | `{ infendo: "Yes" }`|
| Endocarditis: No | `{ infendo: "No" }`|

| Coronary | `.json` |
|---|---|
| Coronary Anatomy/Disease Known: Yes | `{ coranatdisknown: "Yes" }` |
| Coronary Anatomy/Disease Known: No | `{ coranatdisknown: "No" }`|
| Num Dis Vessels: One | `{ numdisv: "One" }` |
| Num Dis Vessels: Two | `{ numdisv: "Two" }` |
| Num Dis Vessels: Three | `{ numdisv: "Three" }` |
| Num Dis Vessels: None | `{ numdisv: "None" }` |

| Stenonis | `.json` |
|---|---|
| Percent Native Artery Stenosis Known: Yes | `{ pctstenknown: "Yes" }` |
| Percent Native Artery Stenosis Known: No | `{ pctstenknown: "No" }` |
| Percent Stenonis - Left Main &isin; [0, 100] | `{ pctstenlmain: 50 }` |

| Status | `.json` |
|---|---|
| Elective | `{ status: "Elective" }` |
| Urgent | `{ status: "Urgent" }` |
| Emergent | `{ status: "Emergent" }` |
| Emergent Salvage | `{ status: "Emergent Salvage" }` |

| Resuscitation | `.json` |
|---|---|
| Yes - Within 1 hour of the start of the procedure | `{ resusc: "Yes - Within 1 hour of the start of the procedure" }`|
| No | `{ resusc: "No" }` |
| Yes - More than 1 hour but less than 24 hours of the start of the procedure | `{ resusc: "Yes - More than 1 hour but less than 24 hours of the start of the procedure" }` |

| Cardiogenic Shock | `.json` |
|---|---|
| Yes - At the time of the procedure | `{ carshock: "Yes - At the time of the procedure" }`|
| No | `{ carshock: "No" }`|
| Yes, not at the time of the procedure but within prior 24 hours | `{ carshock: "Yes, not at the time of the procedure but within prior 24 hours" }` |

| Classification-NYHA| `.json` |
|---|---|
| Class I | `{ classnyh: "Class I" }` |
| Class II | `{ classnyh: "Class II" }` |
| Class III | `{ classnyh: "Class III" }` |
| Class IV | `{ classnyh: "Class IV" }` |

| IABP | `.json` |
|---|---|
| IABP: Yes | `{ iabp: "Yes" }` |
| IABP: No | `{ iabp: "No" }` |
| IABP-When Inserted: Preop | `{ iabpwhen: "Preop" }` |
| IABP-When Inserted: Intraop | `{ iabpwhen: "Intraop" }` |
| IABP-When Inserted: Postop | `{ iabpwhen: "Postop" }` |

| Meds-Inotropes | `.json` |
|---|---|
| Yes | `{ medinotr: "Yes" }` |
| No | `{ medinotr: "No" }` |

| Prev Cardiac Intervent | `.json` |
|---|---|
| Prev Cardiac Intervent: Yes | `{ prcvint: "Yes" }` |
| Prev Cardiac Intervent: No | `{ prcvint: "No" }` |
| Previous PCI: Yes | `{ pocpci: "Yes" }` |
| Previous PCI: No | `{ pocpci: "No" }` |
| Previous PCI-Interval <= 6 hrs | `{ pocpciin: " <= 6 Hours" }` |
| Previous PCI-Interval > 6 hrs | `{ pocpciin: " > 6 Hours" }` |

| VD | `.json` |
|---|---|
| VD-Mitral: Yes | `{ vdmit: "Yes" }` |
| VD-Mitral: No | `{ vdmit: "No" }` |
| VD-Stenosis-Mitral: Yes | `{ vdstenm: "Yes" }` |
| VD-Stenosis-Mitral: No | `{ vdstenm: "No" }` |
| VD-Aortic: Yes | `{ vdaort: "Yes" }` |
| VD-Aortic: No | `{ vdaort: "No" }` |
| VD-Stenosis-Aortic: Yes | `{ vdstena: "Yes" }` |
| VD-Stenosis-Aortic: No | `{ vdstena: "No" }` |
| VD-Insuff-Mitral: Trivial/Trace | `{ vdinsufm: "Trivial/Trace" }` |
| VD-Insuff-Mitral: Mild | `{ vdinsufm: "Mild" }` |
| VD-Insuff-Mitral: Moderate | `{ vdinsufm: "Moderate" }` |
| VD-Insuff-Mitral: Severe | `{ vdinsufm: "Severe" }` |
| VD-Insuff-Mitral: None | `{ vdinsufm: "None" }` |
| VD-Insuff-Mitral: Not documented | `{ vdinsufm: "Not documented"}` |
| VD-Insuff-Tricuspid: Trivial/Trace | `{ vdinsuft: "Trivial/Trace" }` |
| VD-Insuff-Tricuspid: Mild | `{ vdinsuft: "Mild" }` |
| VD-Insuff-Tricuspid: Moderate | `{ vdinsuft: "Moderate" }` |
| VD-Insuff-Tricuspid: Severe | `{ vdinsuft: "Severe" }` |
| VD-Insuff-Tricuspid: None | `{ vdinsuft: "None" }` |
| VD-Insuff-Tricuspid: Not documented | `{ vdinsuft: "Not documented" }` |
| VD-Insuff-Aortic: Trivial/Trace | `{ vdinsufa: "Trivial/Trace" }` |
| VD-Insuff-Aortic: Mild | `{ vdinsufa: "Mild" }` |
| VD-Insuff-Aortic: Moderate | `{ vdinsufa: "Moderate" }` |
| VD-Insuff-Aortic: Severe | `{ vdinsufa: "Severe" }` |
| VD-Insuff-Aortic: None | `{ vdinsufa: "None" }` |
| VD-Insuff-Aortic: Not documented | `{ vdinsufa: "Not documented" }` |


| Incidence | `.json` |
|---|---|
| First cardiovascular surgery | `{ incidenc: "First cardiovascular surgery" }` |
| First re-op cardiovascular surgery | `{ incidenc: "First re-op cardiovascular surgery" }` |
| Second re-op cardiovascular surgery | `{ incidenc: "Second re-op cardiovascular surgery" }` |
| Third re-op cardiovascular surgery | `{ incidenc: "Third re-op cardiovascular surgery" }` |
| Fourth or more re-op cardiovascular surgery | `{ incidenc: "Fourth or more re-op cardiovascular surgery" }` |

| Prev CAB | `.json` |
|---|---|
| Yes | `{ prcab: "Yes" }` |
| No | `{ prcab: "No" }` |

| Prev Valve | `.json` |
|---|---|
| Yes | `{ prvalve: "Yes" }` |
| No | `{ prvalve: "No" }` |
