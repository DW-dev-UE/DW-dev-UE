# ABadOmic — 昼 / 夜エスケープ（UE5）

[English](ABadOmic.md) · [한국어](ABadOmic_ko.md) · [日本語](ABadOmic_ja.md)

---

## ひとことで

**2025 フリーランス**で関わった Unreal Engine **5.6** プロジェクトです。  
中核ロジックは **C++** モジュール（`ABadOmic`）にあります。

**宿舎（숙소）** を舞台に、およそ **2週間** を生き延びる構成です。  
**昼**はクエストと信頼度、**夜**はツール・潜伏・警備 AI。  
選択とインベントリで **Escape / Failure / Hypocrite** エンディングに分岐します。

ソースと企画の所有権はクライアント側にあります。  
このページはコード上に存在するシステムを整理したもので、  
チーム役割比率や工数などの数値は **扱っていません**。

---

## プロジェクトの理由

> [!IMPORTANT]
> ソースと企画の所有権はクライアント側にあります — このページはコードから確認できる範囲のみを扱います。

1日を2つのペースに分けつつ、  
進行の軸はひとつにしたい — **信頼度 · アイテム · フラグ**。

昼: 宿舎内の日常・クエスト・社会的圧力。  
夜: ツールが現れ、警備が回り、潜伏場所とタイマーが動く時間。

エンディングはボス戦1回ではなく、  
**何日分もの小さな選択**が積もった結果として設計されています。

---

## プレイループ

```text
ロビー / プロローグ
        │
        ▼
昼  — クエスト · 信頼度 · 宿舎アクティビティ
        │
        ▼
夜  — ツール · 潜伏 · 警備の視界 / 追跡
        │
        ▼
翌日（またはエンディング判定）
        │
        ▼
エンディング レベル（ナラティブ ページ · イラスト · BGM）
```

コードから読めるおおよその制約:

| 項目 | 挙動 |
|:-----|:-----|
| **期間** | デイ サイクルが最終帯（~**14日**）まで進む |
| **信頼度** | Reliability、初期 **5**、上限 **10** |
| **夜** | カウントダウン（既定 **180秒**）、スキップ可能帯のあと固定 |
| **警備** | 夜スポーン **数が信頼度と反比例** |

---

## システム構成

```text
UABOGameInstance
  · Item / Quest / Ending DataTable
  · 設定マネージャ · ラン統計（生存日数、キル、仲間）
  · エンディング ナラティブ ページ用ヘルパ
        │
        ├── UQuestSystem（GameInstanceSubsystem）
        │     日/信頼フィルタ · 重み付け選択 · フラグ · インタラクション
        │
        ├── AABOGameState
        │     昼/夜 · 夜タイマー · 潜伏リスト · エンディング オープン
        │
        ├── AABOPlayerState
        │     信頼度 · HUD 参照 · キル / 犠牲カウンタ
        │
        └── AABOGamemode
              昼・夜の警備スポーン · 昼の追跡 · パトロール選択

ワールド マネージャ
  EndingManager · ToolSystemManager · BGMManager · AmbienceManager
  QuestObjectiveManager · SleepTypingManager · SequenceManager

キャラクター
  AABOCharacter   — スプリングアーム カメラ · インベントリ · 操作 / 攻撃 / 潜伏
  AGuardCharacter — 視界コーン（プロシージャル メッシュ）· パトロール · 射撃 · アグロ
```

### 昼 / 夜

`AABOGameState` がフェーズを持ちます。

- **昼** — クエスト オブジェクティブ、Day HUD、昼 BGM、タイピングする宿舎 NPC  
- **夜** — ツール強調、潜伏アクター有効、夜警備・パトロール、夜 BGM、睡眠 NPC  
- 遷移時に夜ツール整理、ランダム ボックス再出現、日次クエスト再抽選  

夜に警備を倒すと信頼度が大きく下がり、  
**仲間の犠牲**ルートにつながり得ます。  
犠牲が閾値を超えると失敗エンディングです。

### 信頼度と警備

信頼度はクエスト出現条件と夜の難易度の両方に効きます。

夜の警備数（GameMode 基準）:

| 信頼度 | 警備数 |
|:------:|:------:|
| ≤ 1 | 5 |
| ≤ 3 | 4 |
| ≤ 5 | 3 |
| ≤ 7 | 2 |
| それ以外 | 1 |

パトロールは登録済み `AGuardPatrolPath` からランダムに選びます。

### クエスト システム

`UQuestSystem` は **GameInstanceSubsystem** で、DataTable（`FQuestDefinition`）駆動です。

- タイプ: **Simple / Functional / Conditional**  
- フィルタ: 日範囲、特定日、信頼 min/max、連続信頼、必要フラグ/アイテム  
- 報酬: 信頼変化、アイテム ドロップ、フラグ付与、翌日の重み候補  
- インタラクション: 追加選択肢（アイテム・信頼・エンディング リンク）  

ワールド側: `AQuestObjective` 系 + `AQuestObjectiveManager` が  
その日の配置オブジェクティブを起動。  
掃除・ゴミ・寝具・窓・配達・コンピュータ・投資・詐欺（fraud）・殺鼠剤・打撃・警備依頼など  
クエスト別 UMG が付いています。

### インベントリ · ツール

プレイヤーには2層あります。

| 層 | 役割 |
|:---|:-----|
| **ツール**（2スロット） | Stick / Sharpness / Heavy · レベル **I–IV**（上位は下位ツール要求が多い） |
| **アイテム** | **エンディング** キーまたは **特殊** 効果 |

特殊効果タイプ:  
Tracker, Bulletproof Vest, Stopwatch, Sneakers, Beer, Guard Uniform, Fur Gloves, Firecracker。

`AToolSystemManager` が夜ツール・ランダム ボックスのスポーン/デスポーンを担当。  
インベントリはエンディング条件検査（E3, E4–E5, E7, E10, E11 など）も行います。

### 警備 AI

- Behavior Tree + ブラックボード（`Recognition`, `TargetActor`, 最終目撃位置）  
- **視界コーン**: **ProceduralMesh**（半径 / 角度 / レイ数）  
- パトロール、追跡、射撃エフェクト、アグロ、睡眠状態  
- 昼: ロジック停止寄り / 夜: フル ロジック + 信頼ベース スポーン  

プレイヤーは攻撃・撃破（昼/夜モンタージュ）、潜伏、アグロ誘導が使えます。

### エンディング システム

`AEndingManager` + エンディング DataTable:

| カテゴリ | 役割（データ） |
|:---------|:---------------|
| **Escape** | 脱出・成功系 |
| **Failure** | 信頼崩壊、日数限界、犠牲など |
| **Hypocrite** | 高信頼連続 + 詐欺クエスト関連フラグ |

スナップショット: 日数、信頼、仲間犠牲、防御スタック、アイテム、フラグ。  
即時トリガー: 被弾、昼/夜警備キル、フォン充電、壁登り、短絡、  
窓脱出、賄賂、毒混合、殺鼠剤窃盗など。

`UEndingWidget` は **ページ型ナラティブ**（イラスト + リッチ テキスト + SFX/BGM）のあと  
メイン メニュー / リプレイへ。  
GameInstance は `||` 区切りの長いナラティブをページ分割します。

### オーディオ · HUD · メタ

| システム | 内容 |
|:---------|:-----|
| **BGMManager** | 昼/夜/追跡/エンディング · クロスフェード · 夜時間警告 SFX |
| **AmbienceManager** | 昼/夜/危険レイヤ · 緊張度 |
| **HudWidget** | Day + Night HUD · インベントリ · ミニマップ · 潜伏プロンプト · フォン充電 |
| **Settings** | 映像・音声（`UGameSettingsManager` + セーブスロット） |
| **プロローグ / ロビー** | イントロ ウィジェット、ロビー レベル流れ |
| **SleepTypingManager** | 昼タイピング / 夜睡眠の宿舎 NPC |

メイン プレイ マップは **宿舎** 中心です。  
ロビー・エンディング・トップダウン/テンプレート マップもコンテンツに含まれます。

---

## コード抜粋

クライアント本体は非公開です。  
単純な if / クランプではなく、**複数システムが噛み合う**抜粋です  
（`Source/ABadOmic/…`）。

### 1. 警備の視界コーン（ライントレース メッシュ再生成）

毎ティック/コンストラクションでコーンを **壁にライントレース** し、  
**ProceduralMesh** の扇形として組み直します（Pawn 無視 · 壁のみ遮断）。

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
	DrawVisionCone(); // 回転·移動中も壁オクルージョンを反映
	// 近接キル / 賄賂インタラクト用カプセル …
}
```

### 2. 日次クエスト選定（フィルタ + 重み抽選 + デュアルスロット）

`StartNewDay` が DataTable 全体を走査し **日/信頼/フラグ/履歴** ゲートを通し、  
強制 1 スロットの後、重複なしで 2 スロット目を埋めます。

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

`DoesQuestMatchConsecutiveTrust` はローリング **信頼ヒストリ** を読み、  
連続高信頼コンテンツの解錠/封鎖に使います。

### 3. エンディング グラフ — 遅延短絡 + 多条件ルート

単一 switch ではありません。選択が **状態を arm** しアイテム·フラグを消費し、  
**後の昼/夜境界** でエンディングを確定します。

```cpp
// Actor/EndingManager.cpp

void AEndingManager::OnComputerRoomShortCircuitAttemptSelected()
{
	if (bEndingTriggered) return;

	// スナップショット在庫に 線+電池+クリップ が必要
	if (!(HasItem(Item_Wire, 1)
		&& HasItem(Item_Battery, 1)
		&& HasItem(Item_Clip, 1)))
		return;

	bPendingShortCircuit = true;              // 後で解決
	PendingShortCircuitDay = CurrentDay;
	bPendingShortCircuit_FireExtRemoved = HasFlag(Flag_FireExtRemoved);
}

void AEndingManager::TryResolveShortCircuitAtNightStart()
{
	if (!bPendingShortCircuit) return;
	if (PendingShortCircuitDay != CurrentDay) return;
	if (!bPendingShortCircuit_FireExtRemoved) return;

	// タオル有無で次の夜境界に Escape vs Failure
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

	// 家族+友人 fraud 両方 → H2、それ以外 H1
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
	return false; // 失敗時はエンディングなしで生存継続
}
```

### 4. 警備キル → モンタージュ分岐 → 大量キル エンディング ゲート

昼/夜の死亡モンタージュ、AI 停止、コリジョン OFF、  
戦闘中に `E6_5` を発火し得る **キル閾値カウンタ**。

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
		Atimer = 4.22f; // 夜キルは短いモンタージュ
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

## ゲームプレイ要素（コード基準）

- 日付・信頼度で変わる **昼クエスト**  
- コンテンツと夜難易度を結ぶ **信頼度**  
- 等級付きツールとランダム ボックスの **夜ツール探索**  
- 夜パトロール中の **潜伏**  
- **警備の視界 / 追跡 / 射撃** とプレイヤーのカウンター キル  
- 夜キル後の **仲間犠牲 / 処刑** シーケンス  
- アイテム・フラグ ゲート付き **マルチ エンディング**  
- エンディングに繋がる **フォン充電** などの選択 UI  
- 設定セーブ、プロローグ、ロビー、エンディング演出  

---

## スタック

| | |
|:--|:--|
| エンジン | **Unreal Engine 5.6** |
| 言語 | **C++**（ランタイム モジュール `ABadOmic`）+ Blueprint アセット |
| AI | AIModule · Behavior Tree / Blackboard · StateTree プラグイン |
| UI | **UMG** / Slate |
| メッシュ / FX | ProceduralMeshComponent（視界コーン）、Niagara/パーティクル コンテンツ |
| 入力 | Enhanced Input |
| データ | DataTable（クエスト、アイテム、ツール、エンディング） |
| 保存 | 設定用 `SaveGame`（確認した経路ではキャンペーン全体セーブではない） |

---

## 実装した範囲

フリーランス案件のため、社内の役割分割は **断定しません**。  
分析した **C++ コードベースに実装されているシステム** は次のとおりです。

| 領域 | ソース上の範囲 |
|:-----|:---------------|
| **フレームワーク** | GameInstance · GameMode · GameState · PlayerState · 設定マネージャ |
| **昼 / 夜** | フェーズ切替、夜タイマー、スキップ規則、日進行、宿舎アンビエント切替 |
| **クエスト** | サブシステム、定義、日次ロール、インタラクション、オブジェクティブ マネージャ |
| **インベントリ / ツール** | ツール 2スロット、アイテム、特殊効果、夜ツール マネージャ |
| **警備** | 信頼ベース スポーン、パトロール選択、視界コーン、追跡フック、潜伏連携 |
| **エンディング** | マネージャ判定、カテゴリ、エンディング レベル受け渡し、ページ型 UI |
| **オーディオ** | BGM・アンビエンス マネージャ、時間/警備/クエスト/アイテム SFX フック |
| **HUD / メタ** | Day·Night HUD、インベントリ UI、ロビー/プロローグ/充電/クエスト ウィジェット |

`Content/Blueprint` 下の BP が宿舎レベル・UI レイアウトにクラスを接続します。

---

## 画面

### 1. ロビー — ESCAPE : CAMB

<p align="center">
  <img src="assets/screenshots/shot-01-lobby.png" width="900" alt="タイトル ロビー画面" />
</p>

**注釈**

タイトル **ESCAPE : CAMB** / サブ *NO WAY OUT*。  
新規ゲーム · 続きから · オプション · 終了。  
（`ULobbyWidget` フロー）

### 2. 昼 — 宿舎 Day 1

<p align="center">
  <img src="assets/screenshots/shot-02-day.png" width="900" alt="宿舎の昼プレイ画面" />
</p>

**注釈**

2段ベッドの宿舎内部。  
HUD: 信頼度 **5/10**、**DAY 1 / 14**、日次クエスト（壁カビ · 床汚れ）。  
右下 **P PASS** で昼スキップ可。  
（`DayHud` + 昼クエスト ループ）

### 3. 失敗エンディング — F4 Point-Blank

<p align="center">
  <img src="assets/screenshots/shot-03-ending.png" width="900" alt="失敗エンディング結果画面" />
</p>

**注釈**

**FAILURE ENDING** `F4` · *Point-Blank*（警備に射殺）。  
生存日数 / 警備キル / 犠牲にした仲間の集計とイラスト。  
メインメニュー · リプレイ。  
（`UEndingWidget` + Escape/Failure/Hypocrite 分岐）

---

## 公開範囲

> [!NOTE]
> クライアント案件のため、以下はプロジェクトツリーから確認できる構成・システムの範囲までです。

**クライアント フリーランス** 案件です。

- 公開文はプロジェクト ツリーから確認できる **構成・システム** まで  
- 未公開アセット、全企画書、クライアント機密は載せない  
- 人数・時間・「貢献度 %」などの数値は記載しない  

今後パブリック ビルドやトレーラーが出れば、このページにリンクを足せます。

---

## まとめ

| | |
|:--|:--|
| 種別 | 宿舎舞台の UE5.6 C++ 昼/夜エスケープ |
| 核心 | 信頼度 · 日次クエスト · 夜ツール/警備 · マルチ エンディング |
| 年 | **2025** |
| 役割メモ | フリーランス · コード基準のシステム整理 |
