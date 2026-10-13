
# Shark Attacks: Analysis of 100+ Years of Reported Incidents

Capstone project for the Digitaley Drive Data Analyst Bootcamp.

## Overview

This project cleans and analyses a raw dataset of reported shark attacks, including location, activity, victim details, injury, time of day and shark species. It answers six questions about trends over time, the most dangerous places, body parts injured, time of day and species.

**Main finding:** most attacks happen in the USA, Australia and South Africa, in the afternoon, and mostly injure legs and feet. White, tiger and bull sharks are the species most often named.

## Dataset

`Shark_attacks.xlsx` has one sheet, `attacks`, with 25,614 rows and 22 columns.

**Cleaning steps**
- Removed 19,519 blank rows, leaving 6,095 incidents.
- Kept 14 useful columns and dropped duplicated Case Number columns, links and order helpers.
- Limited the trend analysis to 1900 to 2017, which leaves 5,409 incidents (683 are earlier and 3 have no year).
- Standardised country names (casing, spaces, typos) and fixed stray values in the Fatal column.
- Converted free-text Time into hours and time bands.
- Used keyword matching on Injury (body parts) and Species.

**Limits:** 45% of records have no species and 49% have no usable time. The 16 repeated case numbers were kept, since they may be separate victims. 2017 is a partial year.

## Tools

Excel and Power BI. Power Query was used to clean and shape the data, a data model and DAX measures to calculate the results, and Power BI for the dashboard.

## Findings

| Question | Result |
|---|---|
| Q1 Trend | Rises from about 15 a year in the early 1900s to 100 to 140 a year since 2005. Peak: 141 in 2015. Fatal share fell from 62% (1900s) to 9% (2010s) |
| Q2 Countries | USA 2,030, Australia 1,147, South Africa 541 (69% of all incidents) |
| Q2 Hotspots | Florida (988), New South Wales (413), KwaZulu-Natal (197). Top location: New Smyrna Beach, Volusia County (164) |
| Q4 Body parts | Leg 45.2%, foot/ankle 28.7%, arm 18.3%, hand 16.5%, torso 10.5% |
| Q5 Time of day | Afternoon 48.1%, morning 32.2%, evening 16.0%, night 3.8% |
| Q6 Species | White 33.7%, tiger 14.9%, bull 11.2% of identified species |

## Recommendations

1. Focus lifeguards and signage on hotspots such as Volusia County and New South Wales.
2. Warn swimmers, since swimming attacks are fatal 37% of the time against 5% for surfing.
3. Run safety campaigns from 11:00 to 17:00, and in July and August in the northern hemisphere.
4. Invest in emergency care in high-fatality countries such as Papua New Guinea, Brazil and Mexico.
5. Improve record-keeping, especially species and time of day.

## How the analysis was done

1. **Clean in Power Query.** Load the `attacks` sheet and then:
   - Remove blank rows and the columns that are not needed (duplicated Case Number columns, links and order helpers).
   - Set Year to a whole number and filter it to 1900 to 2017.
   - Clean Country: trim spaces, change to uppercase, remove stray question marks, and replace values (England to United Kingdom, Columbia to Colombia).
   - Clean Fatal (Y/N): trim, uppercase, and replace "F" with "Y", so only Y and N remain as known outcomes.
   - Add an Hour column from Time (the part before the "h" in values such as "08h30"), plus a conditional column that maps text times such as "Afternoon" and "Night".
   - Add a Time Band column from Hour: Night (21:00 to 04:59), Morning (05:00 to 11:59), Afternoon (12:00 to 16:59) and Evening (17:00 to 20:59).
   - Add Body Part columns (Leg, Foot / ankle, Arm, Hand / fingers, Torso, Head / face, Neck) using conditional columns with `Text.Contains` on the lower-case Injury text.
   - Add a Species Group column the same way (White, Tiger, Bull and so on), with "Unidentified" when no species is named.
2. **Create DAX measures** (the main table is called `Attacks` here).

```DAX
Attacks = COUNTROWS(Attacks)
Fatal Rate =
DIVIDE(
    CALCULATE(COUNTROWS(Attacks), Attacks[Fatal] = "Y"),
    CALCULATE(COUNTROWS(Attacks), Attacks[Fatal] IN {"Y", "N"})
)
Share % = DIVIDE(COUNTROWS(Attacks), CALCULATE(COUNTROWS(Attacks), ALL(Attacks)))
```

3. **Build visuals and answer each question.**

| Question | How it was answered |
|---|---|
| Q1 | Line chart of Attacks by Year (1900 to 2017), with a decade summary |
| Q2 | Bar chart of Attacks by Country (Top 10), then Attacks by Area and Location with the Country slicer set to USA, Australia and South Africa |
| Q4 | Bar chart of the Body Part columns, counted when the value is true, shown as a share of injuries that name a body part |
| Q5 | Column chart of Attacks by Hour, and a donut chart of Attacks by Time Band |
| Q6 | Bar chart of Attacks by Species Group, with Unidentified shown separately |

Excel pivot tables can be used to cross-check the same counts. Because `Text.Contains` matches parts of words, check edge cases such as "whitetip" (not a white shark) and "side" in injury text, as the counts may differ slightly from a whole-word match.

## Project files

| File | Description |
|---|---|
| `Shark_attacks.xlsx` | Source data |
| `[your file name].pbix` | Power BI report with the data model, measures and dashboard |
| Dashboard screenshot | Picture of the dashboard (also posted on LinkedIn) |
| Presentation deck | Slides summarising the findings |


## Author

[GIFTY ALORZUKE]

# Shark-Attack-capstone
Shark Attack analysis
