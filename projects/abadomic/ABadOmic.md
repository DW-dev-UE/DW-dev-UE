# ABadOmic — Day / Night Escape (UE5)

[English](ABadOmic.md) · [한국어](ABadOmic_ko.md) · [日本語](ABadOmic_ja.md)

---

## In one line

**2025 freelance** Unreal Engine **5.6** game built mainly in **C++**.

Survive about **two weeks** in a dormitory facility (**숙소**):  
**day** is quests and trust, **night** is tools, stealth, and guards.  
Choices and inventory push you into **Escape / Failure / Hypocrite** endings.

Client-side source and full design ownership stay with the client.  
This page documents systems that exist in the codebase — not team size or hours.

---

## Why this project

> [!IMPORTANT]
> Client-side source and full design ownership stay with the client — this page only covers what's visible from the codebase.

The design needs a loop that feels different by time of day,  
but still shares one progress bar: **trust**, **items**, and **flags**.

Day is social pressure and chores inside the dorm.  
Night is scarcity — tools appear, guards patrol, hide spots open, the clock runs.

Endings are not a single boss fight.  
They are the **result of days of small decisions** (quest interactions, kills, items, streaks).

---

## Player loop

```text
Lobby / Prologue
        │
        ▼
Day  — quests · trust · dorm activity
        │
        ▼
Night — tools · hide · guard vision / chase
        │
        ▼
Next day (or ending check)
        │
        ▼
Ending level  (narrative pages · illustration · BGM)
```

Rough constraints visible in code:

| Piece | Behavior |
|:------|:---------|
| **Horizon** | Day cycle runs toward a final window (~**day 14**) |
| **Trust** | Reliability, default **5**, clamped up to **10** |
| **Night** | Countdown (default **180s**); skip window then hard lock |
| **Guards** | Night spawn **count scales inversely with trust** |

---

## Systems architecture

```text
UABOGameInstance
  · Item / Quest / Ending DataTables
  · settings manager · run stats (days, kills, partners)
  · ending narrative page helpers
        │
        ├── UQuestSystem (GameInstanceSubsystem)
        │     day/trust filters · weighted pick · flags · interactions
        │
        ├── AABOGameState
        │     day/night · night timer · hide list · ending open
        │
        ├── AABOPlayerState
        │     trust · HUD refs · kill / sacrifice counters
        │
        └── AABOGamemode
              guard spawn day/night · chase by day · patrol pick

World managers (singletons / level actors)
  EndingManager · ToolSystemManager · BGMManager · AmbienceManager
  QuestObjectiveManager · SleepTypingManager · SequenceManager

Character
  AABOCharacter  — spring arm cam · inventory · interact / attack / hide
  AGuardCharacter — vision cone (procedural mesh) · patrol · fire · aggro
```

### Day / night

`AABOGameState` owns the phase:

- **Day** — quest objectives active, day HUD, day BGM, typing dorm NPCs  
- **Night** — tools light up, hide actors enable, night guards + patrols, night BGM, sleep NPCs  
- Transition also clears night tools, respawns random boxes, and re-rolls daily quests  

Night kill of security lowers trust and can force a **partner sacrifice** path;  
too many sacrifices hits a failure ending.

### Trust and guards

Trust feeds both quest eligibility and night difficulty.

Night guard count (from game mode):

| Trust | Guards |
|:-----:|:------:|
| ≤ 1 | 5 |
| ≤ 3 | 4 |
| ≤ 5 | 3 |
| ≤ 7 | 2 |
| else | 1 |

Patrol paths are sampled randomly from registered `AGuardPatrolPath` actors.

### Quest system

`UQuestSystem` is a **GameInstanceSubsystem**, table-driven (`FQuestDefinition`):

- Types: **Simple / Functional / Conditional**  
- Filters: day range, specific days, trust min/max, consecutive trust, required flags/items  
- Rewards: trust delta, item drops, granted flags, next-day weight candidates  
- Interactions: extra choices (items, trust, ending links)  

World side: `AQuestObjective` hierarchy + `AQuestObjectiveManager`  
activates the day’s placed objectives.  
Dedicated UMG for chores and branches (clean, trash, bedding, window, delivery,  
computer, invest, fraud, raticide, whack, guard request, …).

### Inventory & tools

Two layers on the player:

| Layer | Role |
|:------|:-----|
| **Tools** (2 slots) | Stick / Sharpness / Heavy · levels **I–IV** (higher often needs a lower tool first) |
| **Items** | **Ending** keys or **Special** effects |

Special effects present in types:  
Tracker, Bulletproof Vest, Stopwatch, Sneakers, Beer, Guard Uniform, Fur Gloves, Firecracker.

`AToolSystemManager` spawns/despawns night tools and random boxes.  
Inventory can also answer ending requirement checks (e.g. E3 / E4–E5 / E7 / E10 / E11 style gates).

### Guard AI

- Behavior Tree + blackboard (`Recognition`, `TargetActor`, patrol “last seen”)  
- **Vision cone** built with **ProceduralMesh** (radius / angle / ray count)  
- Patrol, chase, fire effect, aggro volume, sleep state  
- Day: often idle brain; night: full logic + random spawn set  

Player can attack / kill (day vs night montages), use hide spots, or bait with aggro.

### Ending system

`AEndingManager` + ending DataTables:

| Category | Role (data) |
|:---------|:------------|
| **Escape** | Successful / exit-style outcomes |
| **Failure** | Trust collapse, day limit, sacrifice, etc. |
| **Hypocrite** | High-trust streak + fraud-related flags |

Snapshots: day, trust, companion sacrifice, defense stack, items, flags.  
Immediate triggers: shot, day/night guard kills, charge phone, wall climb, short circuit,  
window escape, bribe, poison mix, rat-poison steal, and more.

`UEndingWidget` plays **paged narrative** (illustration + rich text + SFX/BGM),  
then main menu / replay.  
Game instance can split long `||`-separated narrative into pages.

### Audio · HUD · meta

| System | Notes |
|:-------|:------|
| **BGMManager** | Day / night / chase / good·bad ending · crossfade · night time warning SFX |
| **AmbienceManager** | Day / night / danger layers · tension |
| **HudWidget** | Day HUD + Night HUD + inventory · minimap · hide prompt · phone charge |
| **Settings** | Video + audio via `UGameSettingsManager` + save slot |
| **Prologue / Lobby** | Intro sequence widgets and lobby level flow |
| **SleepTypingManager** | Dorm NPCs: typing by day, sleeping by night |

Primary play map content centers on the **숙소 (dormitory)** level.  
Content also includes lobby, ending, top-down / template maps used for production.

---

## Code excerpts

Full client source stays private.  
Here are **heavier slices** that show multi-system coupling  
(from `Source/ABadOmic/…`) — not just clamp-and-if helpers.

### 1. Guard vision cone (line-trace mesh rebuild)

Each tick / construction, the cone is **ray-cast against walls**,  
then rebuilt as a **ProceduralMesh** fan (pawns ignored — walls only).

```cpp
// Character/GuardCharacter.cpp
void AGuardCharacter::DrawVisionCone()
{
	TArray<FVector> Vertices;
	TArray<int32> Triangles;
	// … Normals / UV / colors …

	Vertices.Add(FVector::ZeroVector); // apex

	float CurrentAngle = -ViewAngle / 2.0f;
	float AngleStep = ViewAngle / (float)RayCount;

	for (int32 i = 0; i <= RayCount; i++)
	{
		FRotator Rotation(0.0f, CurrentAngle, 0.0f);
		FVector Direction = Rotation.RotateVector(FVector::ForwardVector);

		FVector WorldStart = GetActorLocation();
		FVector WorldEnd = WorldStart
			+ (GetActorRotation().RotateVector(Direction) * ViewRadius);

		FHitResult HitResult;
		FCollisionQueryParams QueryParams;
		QueryParams.AddIgnoredActor(this);

		// TraceChannel defaults to WorldStatic — walls clip the cone
		bool bHit = GetWorld()->LineTraceSingleByChannel(
			HitResult, WorldStart, WorldEnd, TraceChannel, QueryParams);

		FVector HitPoint = bHit ? HitResult.Location : WorldEnd;
		FVector LocalPoint =
			GetActorTransform().InverseTransformPosition(HitPoint);
		Vertices.Add(LocalPoint);

		if (i > 0)
		{
			Triangles.Add(0);
			Triangles.Add(i + 1);
			Triangles.Add(i);
		}
		CurrentAngle += AngleStep;
	}

	ProceduralMesh->CreateMeshSection(
		0, Vertices, Triangles, Normals, UV0, VertexColors, Tangents, false);
}

void AGuardCharacter::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
	DrawVisionCone(); // live wall occlusion while rotating / moving
	// Capsule probe for melee / bribe interact affordance …
}
```

### 2. Daily quest selection (filters + weighted pick + dual slots)

`StartNewDay` walks the whole DataTable, runs **day / trust / flags / history** gates,  
forces an optional first slot, then fills a second slot without duplicates.

```cpp
// Framework/QuestSystem.cpp

static const FQuestRowView* PickQuestByWeight(
	const TArray<FQuestRowView>& Candidates, const UQuestSystem* QuestSystem)
{
	TArray<float> EffectiveWeights;
	float TotalWeight = 0.0f;

	for (const FQuestRowView& View : Candidates)
	{
		const float Weight = FMath::Max(
			0.0f,
			QuestSystem->GetEffectiveWeight(View.RowName, *View.Definition));
		EffectiveWeights.Add(Weight);
		TotalWeight += Weight;
	}

	if (TotalWeight <= 0.0f)
		return &Candidates[FMath::RandHelper(Candidates.Num())];

	float Remaining = FMath::FRandRange(0.0f, TotalWeight);
	for (int32 Index = 0; Index < Candidates.Num(); ++Index)
	{
		if (EffectiveWeights[Index] <= 0.0f) continue;
		Remaining -= EffectiveWeights[Index];
		if (Remaining <= 0.0f)
			return &Candidates[Index];
	}
	return &Candidates.Last();
}

void UQuestSystem::StartNewDay(
	int32 Day, int32 Trust, const TArray<FName>& CurrentFlags)
{
	CurrentDay = Day;
	CurrentTrust = Trust;
	// RuntimeFlags rebuild from CurrentFlags …
	CurrentDayQuests.Empty();

	TArray<FQuestRowView> ForcedFirstSlotCandidates;
	TArray<FQuestRowView> GeneralCandidates;

	for (const auto& Pair : QuestTable->GetRowMap())
	{
		const FQuestDefinition* Def =
			reinterpret_cast<const FQuestDefinition*>(Pair.Value);

		if (!DoesQuestMatchDayAndTrust(*Def, CurrentDay, CurrentTrust)) continue;
		if (!DoesQuestMatchConsecutiveTrust(*Def)) continue; // trust history window
		if (!DoesQuestMatchSpecificDays(*Def, CurrentDay)) continue;
		if (!DoesQuestMatchFlags(*Def)) continue;
		if (!Def->bCanRepeat && OneTimeCompletedQuests.Contains(Pair.Key)) continue;

		(Def->bForceFirstSlot ? ForcedFirstSlotCandidates : GeneralCandidates)
			.Add(FQuestRowView(Pair.Key, Def));
	}

	// Slot 0: forced pool first, else weighted general
	// Slot 1: re-filter table excluding FirstQuestId / ForceFirstSlot
	// …
}
```

`DoesQuestMatchConsecutiveTrust` also scans a rolling **trust history**  
so high-trust streaks can unlock (or lock) content.

### 3. Ending graph — deferred short-circuit + multi-condition routes

Not a single switch: choices **arm state**, consume items/flags,  
and resolve on **later day/night transitions**.

```cpp
// Actor/EndingManager.cpp

void AEndingManager::OnComputerRoomShortCircuitAttemptSelected()
{
	if (bEndingTriggered) return;

	// Need wire + battery + clip in the snapshot inventory
	if (!(HasItem(Item_Wire, 1)
		&& HasItem(Item_Battery, 1)
		&& HasItem(Item_Clip, 1)))
		return;

	bPendingShortCircuit = true;              // resolve later
	PendingShortCircuitDay = CurrentDay;
	bPendingShortCircuit_FireExtRemoved = HasFlag(Flag_FireExtRemoved);
}

void AEndingManager::TryResolveShortCircuitAtNightStart()
{
	if (!bPendingShortCircuit) return;
	if (PendingShortCircuitDay != CurrentDay) return;
	if (!bPendingShortCircuit_FireExtRemoved) return;

	// Towel decides Escape vs Failure on the *next* night edge
	if (HasItem(Item_Towel, 1))
		TryTriggerEndingInternal(TEXT("E4"));
	else
		TryTriggerEndingInternal(TEXT("F8"));
}

void AEndingManager::OnBribeEscapeRequestSelected()
{
	if (bEndingTriggered) return;

	const int32 MoneyCount = GetItemCount(Item_MoneyBundle);
	if (CurrentTrust == 10 && MoneyCount >= 3)
		TryTriggerEndingInternal(TEXT("E10"));
	else
		TryTriggerEndingInternal(TEXT("F10"));
}

void AEndingManager::TryCheckNightEndHypocrite()
{
	if (HighTrustStreakDays < HighTrustDaysRequired) return;
	if (!HasFlag(Flag_FraudQuestExperienced)) return;

	// Both family + friend fraud picks → H2, else H1
	const bool bFamily = HasFlag(Flag_FraudPickedFamily);
	const bool bFriend = HasFlag(Flag_FraudPickedFriend);

	TryTriggerEndingInternal((bFamily && bFriend) ? TEXT("H2") : TEXT("H1"));
}

bool AEndingManager::OnFoodDeliveryPoisonMixSelected(
	float SuccessChance01, int32 RandomSeed)
{
	if (!HasItem(Item_RatPoison, 1))
		return false;

	FRandomStream Stream(RandomSeed);
	if (Stream.FRand() <= FMath::Clamp(SuccessChance01, 0.f, 1.f))
	{
		TryTriggerEndingInternal(TEXT("E8"));
		return true;
	}
	return false; // failed mix — live continues, no ending
}
```

### 4. Guard kill → montage branch → mass-kill ending gate

Night vs day death assets, AI brain stop, collision off,  
and a **threshold kill counter** that can fire `E6_5` mid-combat.

```cpp
// Character/GuardCharacter.cpp
void AGuardCharacter::PlayerAttackGuard(AABOCharacter* Player, bool bCrossbow)
{
	RifleEnding();

	if (AGuardAIController* AICon = Cast<AGuardAIController>(
			UAIBlueprintHelperLibrary::GetAIController(this)))
	{
		AICon->StopSurveillance();
	}
	GetMesh()->SetCollisionEnabled(ECollisionEnabled::NoCollision);

	UAnimMontage* DeathAnima = deathMontage.LoadSynchronous();
	float Atimer = 13.1f;
	if (bNight || bCrossbow)
	{
		DeathAnima = nightDeathMontage.LoadSynchronous();
		Atimer = 4.22f; // shorter night kill
	}

	if (AABOPlayerState* MyPS = Cast<AABOPlayerState>(
			UGameplayStatics::GetPlayerState(GetWorld(), 0)))
	{
		MyPS->DATA.nightkilledSecurity++;
		if (MyPS->DATA.nightkilledSecurity >= 10)
		{
			if (AABOGameState* GState =
					Cast<AABOGameState>(GetWorld()->GetGameState()))
			{
				if (GState->bTimeIsDay)
					GState->StartEndingSceneState("E6_5");
			}
		}
	}
	GetMesh()->PlayAnimation(DeathAnima, false);
}
```

---

## Gameplay features (from code)

- **Day quests** that change by day number and trust  
- **Trust** as shared currency for content and night pressure  
- **Night tool hunt** with tiered tools and random boxes  
- **Hide** spots during night patrol  
- **Guard vision / chase / fire** and optional player counter-kill  
- **Partner sacrifice / execution** sequence after night kills  
- **Multi-route endings** (Escape · Failure · Hypocrite) with item/flag gates  
- **Phone charge** and other choice UIs that can link to endings  
- **Settings save**, prologue, lobby, ending presentation  

---

## Stack

| | |
|:--|:--|
| Engine | **Unreal Engine 5.6** |
| Language | **C++** (runtime module `ABadOmic`) + Blueprint assets |
| AI | AIModule · Behavior Tree / Blackboard · StateTree plugins enabled |
| UI | **UMG** / Slate |
| Mesh / FX | ProceduralMeshComponent (vision cone), Niagara/particle content |
| Input | Enhanced Input |
| Data | DataTables (quest, item, tool, ending) |
| Persist | Settings `SaveGame` (not a full campaign save in the reviewed settings path) |

---

## What I built

Freelance engagement — exact internal role split is **not** claimed here.  
What the **C++ codebase implements** (the systems I can stand behind from analysis):

| Area | Scope in source |
|:-----|:----------------|
| **Framework** | GameInstance · GameMode · GameState · PlayerState · settings manager |
| **Day / night** | Phase switch, night timer, skip rules, day advance, dorm ambient swap |
| **Quest** | Subsystem, definitions, daily roll, interactions, objective manager |
| **Inventory / tools** | Dual tool slots, items, special effects, night tool manager |
| **Guard** | Spawn by trust, patrol pick, vision cone, chase hooks, hide integration |
| **Ending** | Manager checks, categories, ending level handoff, paged ending UI |
| **Audio** | BGM + ambience managers, SFX hooks for time / guard / quest / items |
| **HUD / meta** | Day·Night HUD, inventory UI, lobby / prologue / charge / quest widgets |

Blueprints under `Content/Blueprint` wire classes into the dorm level and UI layouts.

---

## Screenshots

### 1. Lobby — ESCAPE : CAMB

<p align="center">
  <img src="assets/screenshots/shot-01-lobby.png" width="900" alt="Title lobby screen" />
</p>

**Caption**

Title **ESCAPE : CAMB** with tagline *NO WAY OUT*.  
New Game · Continue · Options · Quit.  
(`ULobbyWidget` flow)

### 2. Day — dorm, Day 1

<p align="center">
  <img src="assets/screenshots/shot-02-day.png" width="900" alt="Dorm day gameplay" />
</p>

**Caption**

Bunk-room interior (숙소).  
HUD: trust **5/10**, **DAY 1 / 14**, daily quests (mold / floor cleanup).  
Bottom-right **P PASS** can skip the day phase.  
(`DayHud` + day quest loop)

### 3. Failure ending — F4 Point-Blank

<p align="center">
  <img src="assets/screenshots/shot-03-ending.png" width="900" alt="Failure ending result screen" />
</p>

**Caption**

**FAILURE ENDING** `F4` · *Point-Blank* (shot by a guard).  
Stats: days survived / guards killed / partners sacrificed + illustration.  
Main menu · Replay.  
(`UEndingWidget` + Escape/Failure/Hypocrite branches)

---

## Disclosure

> [!NOTE]
> This is a client freelance project — the write-up below covers only what's observable in the project tree.

This was a **client freelance** project.

- Public write-up is limited to **architecture and systems** observable from the project tree  
- Full design docs, unreleased assets, and client confidential material are **not** posted  
- No headcount, hours, or “% ownership” numbers are listed  

If a public build or trailer becomes available later, this page can link it.

---

## Summary

| | |
|:--|:--|
| Type | UE5.6 C++ day/night escape in a dormitory setting |
| Core | Trust · daily quests · night tools/guards · multi-ending |
| Year | **2025** |
| Role note | Freelance · systems described from code |

