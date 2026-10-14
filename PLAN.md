# Plan: a blockbuster universe, Stevie Clawdberg (a Spielberg/Amblin parody)

## Context

Clawd Clash has three parody universes (Star Clawds, Marvelous Clawds and Detective Clawds), and a fourth, Doctor Clawd, is planned on the `doctor_who` branch. This plan adds another: one ensemble drawn from nine films Spielberg directed or produced. As with Doctor Clawd, every character, place, move and transition has to come from the films. Nothing is added just to grow the count.

**The films:**
- **Directed:** Jaws, Close Encounters of the Third Kind, the Indiana Jones films, E.T., Hook and Jurassic Park.
- **Produced or story credit:** Gremlins, The Goonies and Back to the Future.

Jurassic World and Dial of Destiny are included only where they add someone the cast clearly needs (Owen, Claire, Blue, Helena, Voller).

- **Name: Stevie Clawdberg, as a working title.** The other universes pun on franchises; this one puns on a living person's name. That is still parody, but it's a step further than the others.
  - Alternatives, if the name changes: **Amblin' Clawds**, **Clawdblin** or **Blockbuster Clawds**.
  - The code key below doesn't depend on the title, the same way Star Clawds still uses the key `starWars`. Changing the name later means changing `title`, the README and this repo's name, and nothing else.
- **Inside the code:** `case clawdberg = 4` (base 4000) with key `"clawdberg"`, in the folder `Sources/App/Universes/StevieClawdberg/`, with file prefix `Clawdberg…`. It takes rawValue 4 even if Doctor Clawd (3) hasn't merged yet, so neither universe ever renumbers.
- **Naming style** follows the other universes: light puns on the on-screen names (Darth Vapor, Thorr, Superb-Man). Actors' names are never used.
- **Size:** about **115 characters** (60 heroes including HEM, 55 villains including 3 agent-only) and **23 places**. That puts it between Star Clawds (82) and the comic universes (194/208), and well under the 100-villain cap.

## How a universe plugs in

This follows the Doctor Clawd plan, which explored the code. The list is the same, with the names changed.

**New code: `Sources/App/Universes/StevieClawdberg/`**
- `ClawdbergRoster.swift`:
  - `enum ClawdbergVillain` and `enum ClawdbergHero`, append-only, with `hem` as the last hero.
  - The persona dictionaries, with `.merging([.hem: hemPersona(.clawdberg)])`.
  - `Location.clawdbergHome(of:)`, the `clawdbergNemeses`, the ride closure and `let clawdbergTheme = Theme(...)`.
- Character art goes in `Art/ClawdbergHeroes1…N.swift` and `Art/ClawdbergVillains1…N.swift`, about 8–10 personas per file and roughly one film per file where that fits.
- Places go in `Places/ClawdbergScenes1…N.swift` as `let clawdbergPlacesN: [Location: Place]`. Keys must be unique across all universes, for example `amityBeach`, `hillValley1955` and `isla`.
- `ClawdbergIcon.swift` holds the menu bar icon, 28×26 in colour and mono: a boy on a bike across a full moon, in silhouette.
- `ClawdbergWipes.swift` holds the place-to-place transitions (see below).
- `IndyShootsFirst.swift` holds the Cairo swordsman set piece (see below).

**Wiring into existing code.** `build.sh` and the tools already pick up `Universes/**`.
- **`Sources/Shared/Universe.swift`:** the new case, plus `key`, `title`, `villainCount`, `heroCount` and `agentOnly`. `claudeSide` stays the default, so Claude's sessions are heroes.
- **`Sources/Shared/Universe.swift`, rivals:** `rivals` pairs Indy with the swordsman so they face off first, like Han and Greedo.
- **Gap in rawValues:** if this ships before Doctor Clawd, rawValue 3 is unused. `Universe.of` already falls back for unknown ids. Check that nothing else assumes the rawValues run 0, 1, 2, … without gaps, for example the Settings picker and Random's cycle.
- **`Theme.swift`:** a `Theme.of` case, plus `Ride.delorean`, `Ride.moonBike` and `Ride.mothership`.
- **`Locations.swift:69`:** merge `clawdbergPlaces`.
- **`Universes/ComicTransition.swift`:** a `.clawdberg(ClawdbergTrip)` case in `duration`, `draw` and `pick`.
- **`Universes/Hem.swift`:** HEM's costume in every switch: a fedora and leather jacket, with a whip as his weapon.
- **`Rides.swift`:** `drawDelorean`, `drawMoonBike` and `drawMothership`.
- **`Sources/Saver/SaverView.swift:82, 203`:** add `"clawdberg"` to the Options choices and to the switch. This list is easy to miss: the build won't complain if it's left out.
- **`Tools/Preview/main.swift:70`:** add the cast lookup and the `clawdbergHeroes1…` batches.
- **`Tools/Transitions/main.swift`:** add the clawdberg branch.
- **`Pose.swift`, `Drills.swift`, `Habits.swift`, `Overlay.swift`:** these have universe-specific `if`s (see the Doctor Clawd plan for line numbers). Check each one and decide whether the new universe should join it.

## Cast

Each entry gives: parody name — weapon / motion — signature move. "—" means no signature: a move goes in only where there is a clearly recognisable one.

The engine's weapons map onto the films like this:
- `.saber` is melee: whips, swords, the hook, claws and teeth.
- `.blaster` is ranged: revolvers and rifles are `.bullet`, spit and thrown things are `.thrown`, Gizmo's bow is `.arrow`.
- `.lightning` with `Magic(jagged: false)` is a steady glow, used for E.T.'s finger and the Ark.
- `bulk` is for big creatures (T. Wrecks, Slawth, the mechanic), and `armoured` makes shots ricochet off.

### Heroes (60)

**Gremlins (6)**

| Name | Weapon | Signature |
|---|---|---|
| Billy Peltclawd | saber (the sword off the wall) | — |
| Kate Beringer | — | camera flash; the gremlins near her cower |
| Gizmoe | blaster `.arrow`, small | Rambo headband, then a bow shot |
| Rand Peltclawd | — | an invention that sprays and fizzles |
| Mr. Wingo | — | — |
| Murray Futterclaw | — | rants at a foreign car |

**E.T. (5)**

| Name | Weapon | Signature |
|---|---|---|
| E.Clawd | lightning steady (glowing finger) | "Phone… home", pointing at the sky; the heart glows |
| Elliclaw | — | drops a trail of candy |
| Gurtie | — | screams; E.Clawd screams back |
| Big Brother Mikael | — | — |
| Keyz | — | the key ring jangles |

**Jaws (3)**

| Name | Weapon | Signature |
|---|---|---|
| Chief Broody | blaster `.bullet` (rifle) | "You're gonna need a bigger boat" (callout) |
| Hoopah | — | a shark-cage scuba look |
| Quent | blaster `.thrown` (harpoon) | crushes a can; nails on a chalkboard |

**The Goonies (8)**

| Name | Weapon | Signature |
|---|---|---|
| Mikey Waltz | — | inhaler puff, "Goonies never say die!" |
| Brand Waltz | — | chest expander workout |
| Chunk | — | the Truffle Shuffle |
| Mouth | — | — |
| Data | blaster `.thrown` (gadgets) | Pinchers of Peril |
| Andy Carmichaw | — | plays the bone organ |
| Stef Steinclawd | — | — |
| Slawth | saber (punch), `bulk` | "Hey, you guys!" and the Superman shirt rip |

**Indiana Jones (9)**

| Name | Weapon | Signature |
|---|---|---|
| Indiana Bones | saber (whip) | shoots the swordsman (set piece); hat grab under a closing door |
| Marion Ravenwould | blaster `.thrown` (frying pan) | wins the drinking contest |
| Short Rounds | — | "No time for love, Dr. Jones!" |
| Willie Scoot | — | screams at a bug |
| Sallaw | — | sings |
| Henry Bones Sr. | — | umbrella flap scares the seagulls |
| Marcus Brodie | — | gets lost |
| Mutt | saber | combs his hair |
| Helena Shore | — | — |

**Jurassic Park (11)**

| Name | Weapon | Signature |
|---|---|---|
| Alan Grand | — | hat on, slowly takes off sunglasses |
| Ellie Saddler | — | — |
| Ian Malclawm | — | reclines with his shirt open, "Life, uh, finds a way" |
| John Hammend | — | amber cane, "Spared no expense" |
| Muldune | blaster `.bullet` (rifle) | — |
| Tim Murphey | — | — |
| Lex Murphey | — | "It's a Unix system!" |
| Ray Arnie | — | "Hold on to your butts" (callout) |
| Owen Gravy | — | raptor-taming hand |
| Claire Daring | — | a flare held high, running in heels |
| Blue | saber (claws) | — |

**Close Encounters (4)**

| Name | Weapon | Signature |
|---|---|---|
| Roy Neery | — | sculpts a mashed-potato mountain |
| Jillian Geyser | — | — |
| Little Barry | — | toys whir around him |
| Professor Lacomb | — | the five-note hand signs |

**Hook (6)**

| Name | Weapon / motion | Signature |
|---|---|---|
| Peter Panning | saber, `.fly` | crows, "Bangarang!" |
| Tinkerbelle | `.fly`, tiny | fairy-dust sprinkle |
| Roofio | saber | "Ru-fi-ooo!" |
| Thud Butt | — | cannonball roll |
| Pockets | — | — |
| Granny Windy | — | — |

**Back to the Future (7)**

| Name | Weapon | Signature |
|---|---|---|
| Marty McFry | — | the guitar solo; hoverboard |
| Doc Brawn | — | "Great Scott!" |
| Jennifer Parka | — | — |
| George McFry | saber (punch) | the one punch |
| Lorraine Baynes | — | — |
| Einstein | — (dog) | — |
| Clara Crayton | — | — |

Plus **HEM**, last as always, in a fedora with a whip.

### Villains (55)

The Nazi villains (Tote, Vogle, Donnervan, Elsa, Vollar and the mechanic) are drawn in plain grey and black uniforms without insignia, as Marvelous Clawds handles Hydra.

**Gremlins (11)**

| Name | Weapon / motion | Signature |
|---|---|---|
| Strype | blaster `.bullet` | white mohawk; cackles |
| Mohawk | saber (claws) | — |
| The Brainy Gremlin | — | glasses; sings and conducts |
| The Spider Gremlin | saber, `bulk` | — |
| The Bat Gremlin | `.fly` | — |
| The Electric Gremlin | lightning (jagged), `.hover` | — |
| Greta | — | blows kisses |
| Daffy | — | giggles, spins |
| Lenny | — | — |
| George | — | cigar |
| Mrs. Deegle | — | the stair-lift launch |

**E.T. (1)**

| Name | Weapon | Signature |
|---|---|---|
| The Spacesuit Man | — | flashlight sweep |

**Jaws (3)**

| Name | Weapon / motion | Signature |
|---|---|---|
| Bruise | saber (bite), `.hover`: a fin gliding over a patch of water | circles, then breaches |
| Bruise II | saber (bite), `.hover` | the burnt face |
| Mayor Vaun | — | anchor-print blazer, "The beaches are open!" |

**The Goonies (5)**

| Name | Weapon | Signature |
|---|---|---|
| Mama Fratoni | blaster `.bullet` | — |
| Jake Fratoni | — | sings opera |
| Francis Fratoni | — | — |
| Troy Perky | — | — |
| Elgin Perky | — | waves foreclosure papers |

**Indiana Jones (12)**

| Name | Weapon | Signature |
|---|---|---|
| René Bellock | — | holds up the golden idol |
| Major Tote | — | coat hanger; melts when the Ark is opened |
| The Cairo Swordsman | saber | sword twirl (set piece) |
| The Flying-Wing Mechanic | saber (punch), `bulk` | — |
| Mola Ramm | lightning (fire) | "Kali Ma…" heart glow |
| Lao Chay | blaster `.bullet` | the antidote vial |
| Colonel Vogle | — | — |
| Walter Donnervan | — | chooses poorly and turns to dust |
| Elsa Snyder | blaster `.bullet` | — |
| Irina Spalco | saber (rapier) | — |
| Colonel Dovchenko | blaster `.bullet` | — |
| Jürgen Vollar | blaster `.bullet` | — |

**Jurassic Park (9)**

| Name | Weapon / motion | Signature |
|---|---|---|
| T. Wrecks | saber (bite), large `bulk`, `canSit: false` | roar; a glass of water ripples |
| The Big One (raptor) | saber (claws) | taps its claw on the floor |
| Clever Girl (raptor) | saber (claws) | — |
| Spitter | blaster `.thrown` (green spit) | frill flare |
| Dennis Nerdry | — | "Uh uh uh! You didn't say the magic word!" (callout) |
| Lewis Dodgeson | — | shaving-cream can |
| Spinosaurus | saber, `bulk` | — |
| Indominous Rex | saber, `bulk`, `canSit: false` | — |
| Pteranodon | `.fly` | — |

**Close Encounters (1)**

| Name | Weapon | Signature |
|---|---|---|
| The Quarantine Major | blaster `.bullet` | gas mask |

**Hook (4)**

| Name | Weapon | Signature |
|---|---|---|
| Captain Clawk | saber (the hook) | "Bad form!" |
| Mr. Smeeh | — | — |
| Bill Jukes | saber | — |
| The Tick-Tock Croc | — | ticks; Clawk flinches |

**Back to the Future (6)**

| Name | Weapon | Signature |
|---|---|---|
| Biff Tannin | saber (punch) | "Hello? McFly?" knock on the head |
| Old Biff | — | cane and the sports almanac |
| Griff Tannin | saber (bat) | — |
| Mad Dog Tannin | blaster `.bullet` | — |
| Mr. Stricklaw | — | "Slacker!" (callout) |
| Match | — | — |

**Agent-only small fry (3).** Like the MSE-6, these only ever appear as agents: the Compy, a Well of Souls snake and a Gremlin hatchling.

### Nemeses

Each pair faces off on the same stage. A villain can have more than one hero.
- Indiana Bones ↔ René Bellock
- Marion ↔ Major Tote
- Willie, Short Rounds ↔ Mola Ramm
- Henry Sr. ↔ Walter Donnervan
- Sallaw ↔ Colonel Vogle
- Mutt ↔ Irina Spalco
- Helena ↔ Jürgen Vollar
- Marty ↔ Biff
- George ↔ Biff
- Doc ↔ Mad Dog Tannin
- Clara ↔ Mad Dog Tannin
- Jennifer ↔ Griff
- Billy ↔ Strype
- Gizmoe ↔ Mohawk
- Kate ↔ Brainy Gremlin
- Futterclaw ↔ Mrs. Deegle
- Chief Broody ↔ Bruise
- Quent ↔ Bruise
- Hoopah ↔ Mayor Vaun
- Mikey ↔ Mama Fratoni
- Slawth ↔ Mama Fratoni
- Brand ↔ Troy Perky
- Chunk ↔ Francis Fratoni
- Alan Grand ↔ T. Wrecks
- Muldune ↔ Clever Girl
- Owen ↔ Indominous Rex
- Ray Arnie ↔ Dennis Nerdry
- Hammend ↔ Dodgeson
- Peter Panning ↔ Captain Clawk
- Roofio ↔ Captain Clawk
- Tinkerbelle ↔ Mr. Smeeh
- E.Clawd ↔ the Spacesuit Man
- Elliclaw ↔ the Spacesuit Man
- Roy Neery ↔ the Quarantine Major

## Places (23)

Each is a `Place` with a still picture and a moving layer. Each also needs tall-canvas extras for the screen saver.

1. **Hill Valley, 1955:** the courthouse clock tower in a lightning storm. This is the default place.
2. **Hill Valley, 2015:** hoverboards and the Jaws 19 sign.
3. **Hill Valley, 1885:** the Wild West, with a steam train.
4. **Amity Island:** the Fourth of July beach, with a fin out past the swimmers.
5. **The Orca:** open sea, with yellow barrels bobbing.
6. **Kingston Falls:** Main Street in Christmas snow.
7. **Mr. Wingo's shop:** lanterns and cages.
8. **The Clamp Center:** a lab and a TV studio.
9. **Elliott's street:** suburban night, with the forest beyond.
10. **The moon:** a night sky with the bike crossing the full moon.
11. **The Goondocks:** the Astoria houses and the lighthouse restaurant.
12. **The Inferno:** the cove and One-Eyed Willy's ship.
13. **The Well of Souls:** snakes and the statue of Anubis.
14. **The Temple of Doom:** the lava pit and mine-cart tracks.
15. **The Grail temple:** the Petra canyon.
16. **The idol temple:** the rolling boulder.
17. **The Jurassic Park gates:** torches and the jungle.
18. **The visitor centre:** the rotunda and the falling banner.
19. **The T. rex paddock:** rain and a downed fence.
20. **Devils Tower:** the mothership's lights.
21. **Muncie, Indiana:** a country road with lights in the sky.
22. **Neverland:** the Lost Boys' treehouse.
23. **The pirate town:** the dock and the crocodile clock tower.

Homes, for example: the Tannins and McFrys in Hill Valley, the gremlins in Kingston Falls, the dinosaurs on Isla Nublar, the Fratonis at the Goondocks and the pirates in the pirate town.

## Mechanics that make it its own universe

1. **Indy shoots first (`IndyShootsFirst.swift`).** This is the universe's set piece, like Han and Greedo. When Indiana Bones and the Cairo Swordsman are both on stage, the swordsman now and then does a long, showy sword twirl. Indy shrugs, shoots once, and the swordsman topples, lies there a few seconds and gets up grumbling. It is built on the `ShootsFirst` clock and the overlay's `greedoShot` timing.
2. **Rides:**
   - `Ride.delorean`: it swoops in with fire trails and lands; it carries Marty, Doc, Jennifer and Einstein.
   - `Ride.moonBike`: E.Clawd and the kids fly in by bike.
   - `Ride.mothership`: a slow, glowing descent at Devils Tower and Muncie; it carries Roy, Lacomb and Barry.
3. **Transitions (`ClawdbergTrip`, 6).** Like the DC trips, each is weighted by who is on screen:
   - **88 mph:** the DeLorean streaks across, the screen flashes and fire trails split the old scene from the new.
   - **The map route:** a red line crosses an old map from the old place to the new one, the way Indy travels. Picked when Indy's people are on screen.
   - **The fin:** a dorsal fin cuts across the screen and the new scene is behind it. Picked when Bruise is about.
   - **The moon iris:** the bike crosses the moon and the moon opens onto the new scene.
   - **Flashbulb:** a white-out flash, the way the gremlins hate bright light. Picked when gremlins are on screen.
   - **The gates:** the Jurassic Park gates swing open onto the new scene.
4. **Bespoke habits:**
   - **The water glass:** when T. Wrecks is on stage, a glass of water on the ground ripples in time with his footsteps.
   - **After midnight:** between midnight and 5am local time, a new villain agent is more likely to be a gremlin or a hatchling.
   - **Fading photo:** when Marty McFry leaves, he fades out a stripe at a time, like the photo.
   - **Tick-tock:** Captain Clawk flinches whenever the Croc ticks nearby.
   - **Hello, McFly:** when Biff and Marty or George face off, Biff knocks on his head first.
5. **Practice drills (bespoke):**
   - Indy cracks his whip at a target.
   - Quent sharpens a harpoon.
   - Data tests a gadget.
   - Roofio fences with the air.
   - Marty runs through a guitar solo.

## Phases

Each phase builds, is checked with Preview sheets, and is shown to you before the next one.

1. **Skeleton.** The universe case, theme, Indiana Bones and René Bellock, Hill Valley 1955, the menu icon, HEM's costume, the saver option and Preview support. It runs end to end in the app, the window and the screen saver.
2. **Core ensemble.** About 45 characters: the leads and lead villain of each film, plus the obvious pairs (Indy, Marion, Bellock, Tote, the swordsman; Marty, Doc, Biff; Billy, Gizmoe, Strype; Elliclaw, E.Clawd; Broody, Quent, Bruise; the Goonies and the Fratonis; Grand, Malclawm, T. Wrecks, the raptors; Peter, Tinkerbelle, Clawk, Smeeh; Roy, Lacomb). Also 10 places and the nemeses for these.
3. **Rest of the cast and places.** Up to 115 characters and 23 places, with the homes complete.
4. **Mechanics.** Indy shoots first, the three rides, the 6 transitions, the bespoke habits, then the drills.
5. **Docs and release.**
   - Add a column to the README universe table and the roster and location sheets (`Tools/RenderDocs`).
   - The app icon gets another panel (`Tools/MakeIcon`), plus `Tools/MakeSocial` and `llms.txt`.
   - A `CHANGELOG.md` entry in each phase.
   - A trailer in `Tools/RenderPromo`.
   - **Needs your OK first, since it's public:** this repo goes public as the showcase page, and a cask is added in `release.sh`.

Ids are append-only, so the enum order is the order characters are added, and phases never renumber.

## Order with Doctor Clawd

This universe has its own branch, `clawdberg`, off `main`. Whichever universe merges second rebases onto the first; the two only touch the same switches. Because the rawValues are fixed (3 and 4), saved state stays valid whichever ships first.

## Verification

- **Build and install:** `./build.sh`. Every exhaustive switch makes a missing case a compile error. The cast-size assert at `Overlay.swift:175` catches a wrong `heroCount` or `villainCount`.
- **Characters:** `Tools/Preview/build.sh`, then:
  - `build/preview cast out.png "Indiana Bones"`.
  - `build/preview fight out.png "Indiana Bones" "René Bellock"`.
  - `build/preview place out.png "Stevie Clawdberg"`.
- **Transitions:** `Tools/Transitions` renders the frames.
- **In the app:** Settings → Universe → Stevie Clawdberg. Check that a new Claude session is cast as a hero, that subagents can be villains or agent-only small fry, that the screen saver's Options include it, that Random cycles through every universe, and that switching recasts everyone from every other universe.
