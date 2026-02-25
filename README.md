# DC Reading Order with GitLab CI/CD

[![GitLab Pipeline Status](https://gitlab.com/r8vnhill/dc-reading-order/badges/main/pipeline.svg)](https://gitlab.com/r8vnhill/dc-reading-order/-/pipelines)
[![GitHub Mirror](https://img.shields.io/badge/mirror-GitHub-181717?logo=github)](https://github.com/r8vnhill/dc-reading-order)
[![GitLab](https://img.shields.io/badge/source-GitLab-FC6D26?logo=gitlab)](https://gitlab.com/r8vnhill/dc-reading-order)

This project uses GitLab CI/CD as a visual tracker for reading DC comic arcs.

Each arc is represented by a pipeline job, and reading-order arrows are modeled with `needs`.

> **ℹ️ Note:** The CI/CD pipeline functionality is only available on GitLab. The GitHub repository is a mirror for visibility and collaboration.

----

## Table of Contents

- [DC Reading Order with GitLab CI/CD](#dc-reading-order-with-gitlab-cicd)
  - [Table of Contents](#table-of-contents)
  - [Personal context](#personal-context)
  - [Core idea](#core-idea)
  - [Quick Start](#quick-start)
  - [Mermaid graph](#mermaid-graph)
    - [Crisis on Infinite Earths](#crisis-on-infinite-earths)
    - [Infinite Crisis](#infinite-crisis)
  - [Final Crisis](#final-crisis)
  - [Local GitLab Runner setup (`local` tag)](#local-gitlab-runner-setup-local-tag)
  - [How to mark arcs as read](#how-to-mark-arcs-as-read)
  - [Quick example](#quick-example)
  - [Recommended workflow](#recommended-workflow)
  - [Reading variables](#reading-variables)
  - [Additional Information](#additional-information)
    - [What's included in this reading order?](#whats-included-in-this-reading-order)
      - [Batman](#batman)
      - [Superman](#superman)
      - [Green Lantern](#green-lantern)
      - [Swamp Thing \& Animal Man](#swamp-thing--animal-man)
      - [Justice League \& Aquaman](#justice-league--aquaman)
      - [Major Events (convergence points)](#major-events-convergence-points)
    - [Key convergence points](#key-convergence-points)

----

## Personal context

This is my personal DC Comics mainline recommendation, so it is intentionally opinionated.
You will notice a lot of Batman and Green Lantern (yes, that is very much on purpose).

My suggestion is to start at one of the major crossovers depending on how many years you want to spend reading:

- Longer commitment: start after `Crisis on Infinite Earths` (or from the beginning if you want the full ride)
- Medium commitment: start after `Infinite Crisis`
- Shorter commitment: start after `Flashpoint`

----

## Core idea

- By default, **all jobs fail**.
- A job only passes if its `READ_*` variable is set to `1`.
- Jobs also depend on previous arcs, so you cannot progress without completing prerequisites.
- All jobs are tagged with `local`, so they run on a runner with that tag.

>[!TIP] Why a local runner?
> Using a local GitLab Runner means you won't waste your CI/CD quota on silly reading trackers, plus it's a great exercise to learn how runners work c:

----

## Quick Start

1. **Fork or clone the repository** (on GitLab for full pipeline functionality)
2. **Set up a local GitLab Runner** with the `local` tag (see [setup instructions](#local-gitlab-runner-setup-local-tag))
3. **Choose your starting point**:
   - New to DC? Start from the beginning (Crisis on Multiple Earths)
   - Want the modern era? Start after Flashpoint
   - Want something in between? Start after Crisis on Infinite Earths or Infinite Crisis
4. **Run your first pipeline** on GitLab without any variables to see all jobs fail
5. **As you read each arc**, rerun the pipeline with the corresponding `READ_*` variable set to `1`
6. **Watch your progress** through the visual pipeline graph as jobs turn green!

**Example**: After reading your first arc, go to **Build > Pipelines > Run pipeline** and add:
```
READ_CRISIS_ON_MULTIPLE_EARTHS=1
```

>[!NOTE] About Crisis on Multiple Earths Collections
> If you're reading the *Crisis on Multiple Earths* thematic collections (Crossing Over, Crisis Crossed, Countdown to Crisis), note that *Justice League of America: The League That Defeated Itself* is relatively contemporary to *Countdown to Crisis*. However, *Crisis on Multiple Earths* is **not required** as a prerequisite for JLA.

----

## Mermaid graph

>[!TIP]
> For better visualization of the flowcharts below, copy the mermaid code and paste it into [mermaid.live](https://mermaid.live) for an interactive view with zoom and pan capabilities.

### Crisis on Infinite Earths

```mermaid
flowchart TD
  justice_league_of_america_league_that_defeated_itself["Justice League of America: The League That Defeated Itself"] --> crisis_on_infinite_earths["Crisis on Infinite Earths"]
  crisis_on_multiple_earths["Crisis on Multiple Earths"] --> crisis_on_infinite_earths["Crisis on Infinite Earths"]
  roots_of_the_swamp_thing["Roots of the Swamp Thing"] --> crisis_on_infinite_earths
```

### Infinite Crisis

```mermaid
flowchart TD
  crisis_on_infinite_earths["Crisis on Infinite Earths"]
  crisis_on_infinite_earths --> batman_year_one["Batman: Year One"]
  crisis_on_infinite_earths --> the_death_of_superman["The Death of Superman"]
  crisis_on_infinite_earths --> green_lantern_emerald_dawn["Green Lantern: Emerald Dawn"]
  crisis_on_infinite_earths --> saga_of_the_swamp_thing
  crisis_on_infinite_earths --> grant_morrisons_animal_man["Grant Morrison's Animal Man"]

  roots_of_the_swamp_thing["Roots of the Swamp Thing"] --> saga_of_the_swamp_thing["Saga of the Swamp Thing"]

  justice_league_of_america_league_that_defeated_itself["Justice League of America: The League That Defeated Itself"] --> identity_crisis["Identity Crisis"]

  subgraph sg_infinite_crisis["Infinite Crisis"]
    the_death_of_superman
    green_lantern_emerald_dawn
    grant_morrisons_animal_man

    batman_year_one --> batman_haunted_knight["Batman: Haunted Knight"]
    batman_haunted_knight --> batman_the_long_halloween["Batman: The Long Halloween"]
    batman_the_long_halloween --> batman_dark_victory["Batman: Dark Victory"]
    batman_dark_victory --> batman_the_killing_joke["Batman: The Killing Joke"]
    batman_the_killing_joke --> arkham_asylum_serious_house["Arkham Asylum: A Serious House on Serious Earth"]
    batman_the_killing_joke --> batman_gothic["Batman: Gothic"]
    arkham_asylum_serious_house --> batman_a_death_in_the_family["Batman: A Death in the Family"]
    batman_gothic --> batman_a_death_in_the_family
    batman_a_death_in_the_family --> batman_a_lonely_place_of_dying["Batman: A Lonely Place of Dying"]
    batman_a_lonely_place_of_dying --> batman_hush["Batman: Hush"]

    the_death_of_superman --> rise_of_the_supermen["Rise of the Supermen"]
    rise_of_the_supermen --> the_return_of_superman["The Return of Superman"]

    the_return_of_superman --> green_lantern_emerald_twilight["Green Lantern: Emerald Twilight"]
    green_lantern_emerald_dawn --> green_lantern_emerald_twilight
    green_lantern_emerald_twilight --> green_lantern_a_new_dawn["Green Lantern: A New Dawn"]
    green_lantern_a_new_dawn --> zero_hour["Zero Hour"]
    zero_hour --> the_final_night["The Final Night"]
    the_final_night --> day_of_judgment["Day of Judgment"]

    saga_of_the_swamp_thing --> swamp_thing_love_and_death["Swamp Thing: Love and Death"]
    swamp_thing_love_and_death --> swamp_thing_the_curse["Swamp Thing: The Curse"]
    swamp_thing_the_curse --> swamp_thing_a_murder_of_crows["Swamp Thing: A Murder of Crows"]
    swamp_thing_a_murder_of_crows --> swamp_thing_earth_to_earth["Swamp Thing: Earth to Earth"]
    swamp_thing_earth_to_earth --> swamp_thing_reunion["Swamp Thing: Reunion"]
    swamp_thing_reunion --> swamp_thing_by_brian_k_vaughan["Swamp Thing by Brian K. Vaughan"]

    batman_hush --> identity_crisis

    day_of_judgment --> identity_crisis

    identity_crisis --> jla_crisis_of_conscience["JLA: Crisis of Conscience"]
    jla_crisis_of_conscience --> villains_united["Villains United"]
    jla_crisis_of_conscience --> omac_project["OMAC Project"]
    jla_crisis_of_conscience --> day_of_vengeance["Day of Vengeance"]
    jla_crisis_of_conscience --> rann_thanagar_war["Rann-Thanagar War"]
    identity_crisis --> green_lantern_rebirth["Green Lantern: Rebirth"]
    day_of_judgment --> green_lantern_rebirth

    batman_under_the_hood["Batman: Under the Hood"] --> infinite_crisis["Infinite Crisis"]

    identity_crisis --> batman_under_the_hood
    batman_hush --> batman_under_the_hood

    villains_united --> infinite_crisis["Infinite Crisis"]
    omac_project --> infinite_crisis
    day_of_vengeance --> infinite_crisis
    rann_thanagar_war --> infinite_crisis
    green_lantern_rebirth --> infinite_crisis
    grant_morrisons_animal_man --> infinite_crisis
    swamp_thing_by_brian_k_vaughan --> infinite_crisis
  end
```

## Final Crisis

>[!NOTE]
> Recommended reading order tweak: read **Green Lantern: Secret Origin** immediately after **Infinite Crisis**.
> Even though *Secret Origin* was published later, it became Hal Jordan's new canonical origin after *Infinite Crisis*.

```mermaid
flowchart TD
  infinite_crisis["Infinite Crisis"] --> one_year_later["One Year Later"]
  infinite_crisis --> one_year_later["One Year Later"]
  infinite_crisis --> fifty_two["52"]
  infinite_crisis --> green_lantern_secret_origin["Green Lantern: Secret Origin"]
  green_lantern_rebirth["Green Lantern: Rebirth"] --> green_lantern_secret_origin
  infinite_crisis --> batman_face_the_face["Batman: Face the Face"]
  batman_under_the_hood["Batman: Under the Hood"] --> batman_face_the_face

  subgraph sg_final_crisis["Final Crisis"]
    batman_face_the_face --> batman_and_son["Batman and Son"]
    batman_and_son --> batman_resurrection_of_ras_al_ghul["Batman: The Resurrection of Ra's al Ghul"]
    batman_resurrection_of_ras_al_ghul --> batman_the_black_glove["Batman: The Black Glove"]
    batman_the_black_glove --> batman_rip["Batman R.I.P."]

    one_year_later --> countdown["Countdown"]
    fifty_two --> countdown

    one_year_later --> seven_soldiers_of_victory["Seven Soldiers of Victory"]
    fifty_two --> seven_soldiers_of_victory

    fifty_two --> death_of_the_new_gods["Death of the New Gods"]

    fifty_two --> sinestro_corps_war["Sinestro Corps War"]
    green_lantern_secret_origin --> green_lantern_no_fear["Green Lantern: No Fear"]
    green_lantern_no_fear --> sinestro_corps_war
    one_year_later --> sinestro_corps_war

    countdown --> final_crisis["Final Crisis"]
    seven_soldiers_of_victory --> final_crisis
    death_of_the_new_gods --> final_crisis
    sinestro_corps_war --> final_crisis
    batman_rip --> final_crisis
  end
```

```mermaid
flowchart TD
  subgraph pre_flashpoint["Pre Flashpoint"]
    final_crisis --> flash_rebirth["The Flash: Rebirth"]
    final_crisis --> batman_battle_for_the_cowl["Batman: Battle for the Cowl"]
    batman_rip --> batman_battle_for_the_cowl
    batman_battle_for_the_cowl --> batman_and_robin["Batman and Robin"]
    batman_and_robin --> batman_return_of_bruce_wayne["Return of Bruce Wayne"]
    final_crisis --> war_of_light["War of Light"]
    sinestro_corps_war --> war_of_light
    war_of_light --> blackest_night["Blackest Night"]
    flash_rebirth --> blackest_night
    blackest_night --> brightest_day["Brightest Day"]
    blackest_night --> batman_return_of_bruce_wayne
  end
  
  subgraph new_52["New 52"]
    batman_return_of_bruce_wayne --> flash_flashpoint["Flash: Flashpoint"]
    brightest_day --> flash_flashpoint
    flash_rebirth --> flash_flashpoint
    grant_morrisons_animal_man --> animal_man_the_hunt["Animal Man: The Hunt"]
    flash_flashpoint --> animal_man_the_hunt["Animal Man: The Hunt"]
    swamp_thing_by_brian_k_vaughan --> swamp_thing_raise_them_bones["Swamp Thing: Raise Them Bones"]
    flash_flashpoint --> swamp_thing_raise_them_bones
    animal_man_the_hunt --> animal_man_animal_vs_man["Animal Man: Animal vs. Man"]
    swamp_thing_raise_them_bones --> animal_man_animal_vs_man
    swamp_thing_raise_them_bones --> swamp_thing_family_tree["Swamp Thing: Family Tree"]
    swamp_thing_family_tree --> swamp_thing_rotworld_green_kingdom["Swamp Thing: Rotworld - The Green Kingdom"]
    animal_man_animal_vs_man --> swamp_thing_rotworld_green_kingdom
    swamp_thing_family_tree --> animal_man_rotworld_red_kingdom["Animal Man: Rotworld - The Red Kingdom"]
    animal_man_animal_vs_man --> animal_man_rotworld_red_kingdom

    flash_flashpoint --> aquaman_the_trench["Aquaman: The Trench"]
    aquaman_the_trench --> aquaman_the_others["Aquaman: The Others"]
    aquaman_the_others --> throne_of_atlantis["Throne of Atlantis"]
    throne_of_atlantis --> aquaman_death_of_a_king["Aquaman: Death of a King"]

    flash_flashpoint --> justice_league_origin["Justice League: Origin"]
    justice_league_origin --> justice_league_villains_journey["Justice League: The Villain's Journey"]
    justice_league_villains_journey --> throne_of_atlantis
    throne_of_atlantis --> justice_league_of_america_worlds_most_dangerous["Justice League of America: World's Most Dangerous"]
    throne_of_atlantis --> trinity_war["Trinity War"]
    aquaman_death_of_a_king --> trinity_war
    justice_league_of_america_worlds_most_dangerous --> trinity_war
    trinity_war --> forever_evil["Forever Evil"]
    forever_evil --> justice_league_injustice_league["Justice League: Injustice League"]
    justice_league_injustice_league --> justice_league_darkseid_war["Justice League: The Darkseid War"]

    flash_flashpoint --> batman_court_of_owls["Batman: The Court of Owls"]
    batman_court_of_owls --> batman_night_of_the_owls["Batman: Night of the Owls"]
    batman_court_of_owls --> batman_and_robin_born_to_kill["Batman and Robin: Born to Kill"]
    batman_court_of_owls --> batman_faces_of_death["Batman: Faces of Death"]
    batman_night_of_the_owls --> batman_death_of_the_family["Batman: Death of the Family"]
    batman_and_robin_born_to_kill --> batman_death_of_the_family
    batman_faces_of_death --> batman_death_of_the_family
    batman_death_of_the_family --> batman_requiem["Batman: Requiem"]
    flash_flashpoint --> batman_zero_year["Batman: Zero Year"]
    batman_requiem --> batman_endgame["Batman: Endgame"]
    batman_zero_year --> batman_endgame
    batman_endgame --> batman_superheavy["Batman: Superheavy"]

    flash_flashpoint --> green_lantern_sinestro["Green Lantern: Sinestro"]
    green_lantern_sinestro --> green_lantern_revenge_of_the_black_hand["Green Lantern: Revenge of the Black Hand"]
    green_lantern_revenge_of_the_black_hand --> green_lantern_rise_of_the_third_army["Green Lantern: Rise of the Third Army"]
    batman_requiem --> trinity_war
    green_lantern_rise_of_the_third_army --> trinity_war
  end
```

----

## Local GitLab Runner setup (`local` tag)

1. Install GitLab Runner on your machine.
2. In GitLab, go to **Settings > CI/CD > Runners** and copy a runner registration token.
3. Register the runner: `gitlab-runner register`
4. Use these values during registration:
   - GitLab instance URL: your GitLab URL (for example `https://gitlab.com`)
   - Token: the registration token from your project/group
   - Description: any name (for example `local-runner`)
   - Tags: `local`
   - Run untagged jobs: `false`
   - Lock to current project: `true` (recommended)
   - Executor: `shell` (or your preferred local executor)
5. Start the runner service: `gitlab-runner run`

If your runner is already installed as a service, ensure it is running and has the `local` tag.

----

## How to mark arcs as read

Prerequisite: a GitLab Runner with tag `local` must be online, otherwise jobs stay pending.

1. Go to **GitLab > Build > Pipelines > Run pipeline**.
2. In Variables, add `READ_...=1` for each arc you already read.
3. Run the pipeline.
4. Only jobs enabled by both variables and dependencies will pass.

----

## Quick example

If you already read the first three arcs:

- `READ_CRISIS_ON_MULTIPLE_EARTHS=1`
- `READ_JUSTICE_LEAGUE_OF_AMERICA_LEAGUE_THAT_DEFEATED_ITSELF=1`
- `READ_CRISIS_ON_INFINITE_EARTHS=1`

When you run the pipeline:

- `crisis_on_multiple_earths` passes.
- `crisis_on_infinite_earths` passes.
- Next jobs fail (or stay blocked) until you add their variables.

----

## Recommended workflow

1. Start with a pipeline run without variables to see the full pending/failing state.
2. Every time you finish an arc, add its `READ_*` variable in the next run.
3. Repeat until you complete the full path.

----

## Reading variables

These are the variables used by the pipeline. Set each one to `=1` to approve an arc:

- `READ_CRISIS_ON_MULTIPLE_EARTHS`
- `READ_ROOTS_OF_THE_SWAMP_THING`
- `READ_JUSTICE_LEAGUE_OF_AMERICA_LEAGUE_THAT_DEFEATED_ITSELF`
- `READ_CRISIS_ON_INFINITE_EARTHS`
- `READ_SAGA_OF_THE_SWAMP_THING`
- `READ_SWAMP_THING_LOVE_AND_DEATH`
- `READ_SWAMP_THING_THE_CURSE`
- `READ_SWAMP_THING_A_MURDER_OF_CROWS`
- `READ_SWAMP_THING_EARTH_TO_EARTH`
- `READ_SWAMP_THING_REUNION`
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
- `READ_ARKHAM_ASYLUM_SERIOUS_HOUSE`
- `READ_BATMAN_GOTHIC`
- `READ_BATMAN_A_LONELY_PLACE_OF_DYING`
- `READ_BATMAN_HUSH`
- `READ_ZERO_HOUR`
- `READ_THE_FINAL_NIGHT`
- `READ_DAY_OF_JUDGMENT`
- `READ_IDENTITY_CRISIS`
- `READ_JLA_CRISIS_OF_CONSCIENCE`
- `READ_VILLAINS_UNITED`
- `READ_OMAC_PROJECT`
- `READ_DAY_OF_VENGEANCE`
- `READ_RANN_THANAGAR_WAR`
- `READ_GREEN_LANTERN_REBIRTH`
- `READ_BATMAN_UNDER_THE_HOOD`
- `READ_INFINITE_CRISIS`
- `READ_ONE_YEAR_LATER`
- `READ_52`
- `READ_GREEN_LANTERN_SECRET_ORIGIN`
- `READ_GREEN_LANTERN_NO_FEAR`
- `READ_BATMAN_FACE_THE_FACE`
- `READ_BATMAN_AND_SON`
- `READ_BATMAN_RESURRECTION_OF_RAS_AL_GHUL`
- `READ_BATMAN_THE_BLACK_GLOVE`
- `READ_BATMAN_RIP`
- `READ_COUNTDOWN`
- `READ_SEVEN_SOLDIERS_OF_VICTORY`
- `READ_DEATH_OF_THE_NEW_GODS`
- `READ_SINESTRO_CORPS_WAR`
- `READ_FINAL_CRISIS`
- `READ_FLASH_REBIRTH`
- `READ_BATMAN_AND_ROBIN`
- `READ_BATMAN_BATTLE_FOR_THE_COWL`
- `READ_BATMAN_RETURN_OF_BRUCE_WAYNE`
- `READ_WAR_OF_LIGHT`
- `READ_BLACKEST_NIGHT`
- `READ_BRIGHTEST_DAY`
- `READ_FLASH_FLASHPOINT`
- `READ_GRANT_MORRISONS_ANIMAL_MAN`
- `READ_ANIMAL_MAN_THE_HUNT`
- `READ_SWAMP_THING_BY_BRIAN_K_VAUGHAN`
- `READ_SWAMP_THING_RAISE_THEM_BONES`
- `READ_ANIMAL_MAN_ANIMAL_VS_MAN`
- `READ_SWAMP_THING_FAMILY_TREE`
- `READ_SWAMP_THING_ROTWORLD_GREEN_KINGDOM`
- `READ_ANIMAL_MAN_ROTWORLD_RED_KINGDOM`
- `READ_AQUAMAN_THE_TRENCH`
- `READ_AQUAMAN_THE_OTHERS`
- `READ_JUSTICE_LEAGUE_ORIGIN`
- `READ_JUSTICE_LEAGUE_VILLAINS_JOURNEY`
- `READ_THRONE_OF_ATLANTIS`
- `READ_AQUAMAN_DEATH_OF_A_KING`
- `READ_JUSTICE_LEAGUE_OF_AMERICA_WORLDS_MOST_DANGEROUS`
- `READ_TRINITY_WAR`
- `READ_FOREVER_EVIL`
- `READ_JUSTICE_LEAGUE_INJUSTICE_LEAGUE`
- `READ_JUSTICE_LEAGUE_DARKSEID_WAR`
- `READ_BATMAN_COURT_OF_OWLS`
- `READ_BATMAN_NIGHT_OF_THE_OWLS`
- `READ_BATMAN_DEATH_OF_THE_FAMILY`
- `READ_BATMAN_ZERO_YEAR`
- `READ_BATMAN_AND_ROBIN_BORN_TO_KILL`
- `READ_BATMAN_REQUIEM`
- `READ_BATMAN_FACES_OF_DEATH`
- `READ_BATMAN_ENDGAME`
- `READ_BATMAN_SUPERHEAVY`
- `READ_GREEN_LANTERN_SINESTRO`
- `READ_GREEN_LANTERN_REVENGE_OF_THE_BLACK_HAND`
- `READ_GREEN_LANTERN_RISE_OF_THE_THIRD_ARMY`

----

## Additional Information

### What's included in this reading order?

This pipeline extends beyond the core Crisis events with carefully selected story arcs organized by character/team:

#### Batman

- Early Post-Crisis: Year One, Haunted Knight, The Long Halloween, Dark Victory
- Classic: The Killing Joke, Arkham Asylum, Gothic, A Death in the Family, A Lonely Place of Dying
- Modern: Hush, Under the Hood, Face the Face, Batman and Son, Resurrection of Ra's al Ghul
- Morrison's run: The Black Glove, R.I.P., Battle for the Cowl, Batman and Robin, Return of Bruce Wayne
- New 52: Court of Owls, Night of the Owls, Death of the Family, Zero Year, Requiem, Endgame, Superheavy, Faces of Death

#### Superman
  
- Death and Return: The Death of Superman, Rise of the Supermen, The Return of Superman

#### Green Lantern

- Emerald Twilight: Emerald Dawn, The Return of Superman, Emerald Twilight, A New Dawn
- Geoff Johns' run: Rebirth, Secret Origin, No Fear, Sinestro Corps War, War of Light, Blackest Night, Brightest Day
- New 52: Sinestro, Revenge of the Black Hand, Rise of the Third Army

#### Swamp Thing & Animal Man

- Alan Moore era: Roots, Saga of the Swamp Thing, Love and Death, The Curse, A Murder of Crows, Earth to Earth, Reunion
- Morrison's Animal Man
- Modern: Swamp Thing by Brian K. Vaughan
- New 52: Raise Them Bones, The Hunt, Animal vs. Man, Family Tree, both Rotworld crossovers

#### Justice League & Aquaman

- New 52 Justice League: Origin, The Villain's Journey, World's Most Dangerous, Injustice League, Darkseid War
- Aquaman run: The Trench, The Others, Throne of Atlantis, Death of a King

#### Major Events (convergence points)

- Crisis on Multiple Earths → Crisis on Infinite Earths
- Zero Hour → The Final Night → Day of Judgment → Identity Crisis
- Countdown to Infinite Crisis — JLA: Crisis of Conscience, Villains United, OMAC Project, Day of Vengeance, Rann-Thanagar War
- Infinite Crisis → 52 & One Year Later → Seven Soldiers of Victory
- Death of the New Gods → Final Crisis
- The Flash: Rebirth → Flashpoint (reboot)
- Trinity War → Forever Evil

### Key convergence points

These are the major crossover events where multiple storylines merge:

1. **Crisis on Multiple Earths** - The beginning
2. **Crisis on Infinite Earths** - DC's first universe reboot
3. **Identity Crisis** - Dark turning point for DC heroes
4. **Infinite Crisis** - Aftermath of Identity Crisis
5. **Final Crisis** - Grant Morrison's New Gods epic
6. **Flash: Flashpoint** - New 52 reboot point
