# DC Reading Order with GitLab CI/CD

This project uses GitLab CI/CD as a visual tracker for reading DC comic arcs.

Each arc is represented by a pipeline job, and reading-order arrows are modeled with `needs`.

## Core idea

- By default, **all jobs fail**.
- A job only passes if its `READ_*` variable is set to `1`.
- Jobs also depend on previous arcs, so you cannot progress without completing prerequisites.

## How to mark arcs as read

1. Go to **GitLab > Build > Pipelines > Run pipeline**.
2. In Variables, add `READ_...=1` for each arc you already read.
3. Run the pipeline.
4. Only jobs enabled by both variables and dependencies will pass.

## Quick example

If you already read the first two arcs:

- `READ_CRISIS_ON_MULTIPLE_EARTHS=1`
- `READ_CRISIS_ON_INFINITE_EARTHS=1`

When you run the pipeline:

- `crisis_on_multiple_earths` passes.
- `crisis_on_infinite_earths` passes.
- Next jobs fail (or stay blocked) until you add their variables.

## Recommended workflow

1. Start with a pipeline run without variables to see the full pending/failing state.
2. Every time you finish an arc, add its `READ_*` variable in the next run.
3. Repeat until you complete the full path.

## Reading variables

These are the variables used by the pipeline. Set each one to `=1` to approve an arc:

- `READ_CRISIS_ON_MULTIPLE_EARTHS`
- `READ_CRISIS_ON_INFINITE_EARTHS`
- `READ_BATMAN_YEAR_ONE`
- `READ_THE_DEATH_OF_SUPERMAN`
- `READ_GREEN_LANTERN_EMERALD_DAWN`
- `READ_BATMAN_HAUNTED_KNIGHT`
- `READ_RISE_OF_THE_SUPERMEN`
- `READ_BATMAN_THE_LONG_HALLOWEEN`
- `READ_THE_RETURN_OF_SUPERMAN`
- `READ_BATMAN_DARK_VICTORY`
- `READ_GREEN_LANTERN_EMERALD_TWILIGHT`
- `READ_BATMAN_THE_KILLING_JOKE`
- `READ_GREEN_LANTERN_A_NEW_DAWN`
- `READ_BATMAN_A_DEATH_IN_THE_FAMILY`
- `READ_ZERO_HOUR`
- `READ_THE_FINAL_NIGHT`
- `READ_DAY_OF_JUDGMENT`
- `READ_IDENTITY_CRISIS`
- `READ_VILLAINS_UNITED`
- `READ_OMAC_PROJECT`
- `READ_DAY_OF_VENGEANCE`
- `READ_RANN_THANAGAR_WAR`
- `READ_GREEN_LANTERN_REBIRTH`
- `READ_INFINITE_CRISIS`
- `READ_ONE_YEAR_LATER`
- `READ_52`
- `READ_GREEN_LANTERN_SECRET_ORIGIN`
- `READ_COUNTDOWN`
- `READ_SEVEN_SOLDIERS_OF_VICTORY`
- `READ_DEATH_OF_THE_NEW_GODS`
- `READ_SINESTRO_CORPS_WAR`
- `READ_FINAL_CRISIS`
- `READ_WAR_OF_LIGHT`
- `READ_BLACKEST_NIGHT`

## Note

The pipeline follows the attached diagram flow, including key branching and convergence points toward:

- `Identity Crisis`
- `Infinite Crisis`
- `Final Crisis`
- `War of Light`
- `Blackest Night`

## Mermaid graph

```mermaid
flowchart TD
  crisis_on_multiple_earths["Crisis on Multiple Earths"] --> crisis_on_infinite_earths["Crisis on Infinite Earths"]

  crisis_on_infinite_earths --> batman_year_one["Batman: Year One"]
  crisis_on_infinite_earths --> the_death_of_superman["The Death of Superman"]
  crisis_on_infinite_earths --> green_lantern_emerald_dawn["Green Lantern: Emerald Dawn"]

  batman_year_one --> batman_haunted_knight["Batman: Haunted Knight"]
  batman_haunted_knight --> batman_the_long_halloween["Batman: The Long Halloween"]
  batman_the_long_halloween --> batman_dark_victory["Batman: Dark Victory"]
  batman_dark_victory --> batman_the_killing_joke["Batman: The Killing Joke"]
  batman_the_killing_joke --> batman_a_death_in_the_family["Batman: A Death in the Family"]

  the_death_of_superman --> rise_of_the_supermen["Rise of the Supermen"]
  rise_of_the_supermen --> the_return_of_superman["The Return of Superman"]

  the_return_of_superman --> green_lantern_emerald_twilight["Green Lantern: Emerald Twilight"]
  green_lantern_emerald_dawn --> green_lantern_emerald_twilight
  green_lantern_emerald_twilight --> green_lantern_a_new_dawn["Green Lantern: A New Dawn"]
  green_lantern_a_new_dawn --> zero_hour["Zero Hour"]
  zero_hour --> the_final_night["The Final Night"]
  the_final_night --> day_of_judgment["Day of Judgment"]

  batman_a_death_in_the_family --> identity_crisis["Identity Crisis"]
  day_of_judgment --> identity_crisis

  identity_crisis --> villains_united["Villains United"]
  identity_crisis --> omac_project["OMAC Project"]
  identity_crisis --> day_of_vengeance["Day of Vengeance"]
  identity_crisis --> rann_thanagar_war["Rann-Thanagar War"]
  identity_crisis --> green_lantern_rebirth["Green Lantern: Rebirth"]

  villains_united --> infinite_crisis["Infinite Crisis"]
  omac_project --> infinite_crisis
  day_of_vengeance --> infinite_crisis
  rann_thanagar_war --> infinite_crisis
  green_lantern_rebirth --> infinite_crisis

  infinite_crisis --> one_year_later["One Year Later"]
  infinite_crisis --> fifty_two["52"]
  infinite_crisis --> green_lantern_secret_origin["Green Lantern: Secret Origin"]

  one_year_later --> countdown["Countdown"]
  fifty_two --> countdown

  one_year_later --> seven_soldiers_of_victory["Seven Soldiers of Victory"]
  fifty_two --> seven_soldiers_of_victory

  fifty_two --> death_of_the_new_gods["Death of the New Gods"]
  green_lantern_secret_origin --> death_of_the_new_gods

  fifty_two --> sinestro_corps_war["Sinestro Corps War"]
  green_lantern_secret_origin --> sinestro_corps_war

  countdown --> final_crisis["Final Crisis"]
  seven_soldiers_of_victory --> final_crisis
  death_of_the_new_gods --> final_crisis
  sinestro_corps_war --> final_crisis

  final_crisis --> war_of_light["War of Light"]
  war_of_light --> blackest_night["Blackest Night"]
```
