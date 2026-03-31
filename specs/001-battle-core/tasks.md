# Tasks: Battle Core

**Feature**: Battle Core CLI

---

## Phase 1: Setup (Project Initialization)

- [ ] T001 Create solution and base folder structure in /src
- [ ] T002 Create TurnMeterGame.Deck project (Class Library) in /src/TurnMeterGame.Deck
- [ ] T003 Create TurnMeterGame.Characters project (Class Library) in /src/TurnMeterGame.Characters
- [ ] T004 Create TurnMeterGame.Battle project (Class Library) in /src/TurnMeterGame.Battle
- [ ] T005 Create TurnMeterGame.GameManager project (Class Library) in /src/TurnMeterGame.GameManager
- [ ] T006 Create TurnMeterGame.CLI project (Console Application) in /src/TurnMeterGame.CLI

## Phase 2: Foundational (Blocking Prerequisites)

- [ ] T007 Add references between projects as per plan (CLI references all DLLs)
- [ ] T008 Configure xUnit or NUnit test projects for each domain DLL in /tests
- [ ] T009 [P] Add initial test scaffolding for Deck, Characters, Battle, GameManager

## Phase 3: User Story 1 - Listar Personagens Disponíveis (P0)

- [ ] T010 [P] [US1] Create base classes for Personagem in /src/TurnMeterGame.Characters/Personagem.cs
- [ ] T010.1 [P] [US1] Create an class for each personagem in Lista Inicial de Personagens
- [ ] T011 [P] [US1] Create Deck class in /src/TurnMeterGame.Deck/Deck.cs
- [ ] T012 [P] [US1] Implement logic for generating distinct decks in /src/TurnMeterGame.Deck/DeckService.cs
- [ ] T013 [P] [US1] Implement validation for no repeated characters in decks in /src/TurnMeterGame.Deck/DeckService.cs
- [ ] T014 [P] [US1] Unit tests for Deck logic in /tests/TurnMeterGame.Deck.Tests/DeckServiceTests.cs
- [ ] T015 [P] [US1] Unit tests for Personagem logic in /tests/TurnMeterGame.Characters.Tests/PersonagemTests.cs

-[ ] TP3.1 [P] [US1] Crie as funções no CLI para iniciar a batalha. A batalha iniciará
-[ ] TP3.2 [P] [US1] no CLI o usuario podera ver a lista de personagens do seu deck e selecionar um para ver os detalhes

## Phase 4: User Story 2 - Seleção de Times: CPU vs Jogador (P1)

- [ ] T016 [P] [US2] Implement team selection logic in /src/TurnMeterGame.CLI/TeamSelectionService.cs 
- [ ] T017 [P] [US2] Validate no repeated characters in teams in /src/TurnMeterGame.CLI/TeamSelectionService.cs
- [ ] T018 [P] [US2] Unit tests for team selection in /tests/TurnMeterGame.GameManager.Tests/TeamSelectionServiceTests.cs
- [ ] TP4.1 [P] [US2] Implement team automatic selection logic in /src/TurnMeterGame.CLI/TeamSelectionService.cs 

## Phase 5: User Story 3 - Motor de Controle da Batalha (P0)

- [ ] T019 [P] [US3] Create BattleEngine class in /src/TurnMeterGame.Battle/BattleEngine.cs
- [ ] T020 [P] [US3] Implement turn order and battle state logic in /src/TurnMeterGame.Battle/BattleEngine.cs
- [ ] T021 [P] [US3] Unit tests for BattleEngine in /tests/TurnMeterGame.Battle.Tests/BattleEngineTests.cs

## Phase 6: User Story 4 - Timer de Tempo Real da Batalha (P1)

- [ ] T022 [P] [US4] Implement battle timer logic in /src/TurnMeterGame.Battle/BattleTimer.cs
- [ ] T023 [P] [US4] Unit tests for battle timer in /tests/TurnMeterGame.Battle.Tests/BattleTimerTests.cs

## Phase 7: User Story 5 - Turn Meter System (P2)

- [ ] T024 [P] [US5] Implement Turn Meter logic in /src/TurnMeterGame.Battle/TurnMeter.cs
- [ ] T025 [P] [US5] Unit tests for Turn Meter in /tests/TurnMeterGame.Battle.Tests/TurnMeterTests.cs

## Phase 8: User Story 6 - Escolha de Ação (P1)

- [ ] T026 [P] [US6] Implement action selection logic in /src/TurnMeterGame.Battle/ActionSelector.cs
- [ ] T027 [P] [US6] Unit tests for action selection in /tests/TurnMeterGame.Battle.Tests/ActionSelectorTests.cs

## Phase 9: User Story 7 - Seleção de Alvo (P1)

- [ ] T028 [P] [US7] Implement target selection logic in /src/TurnMeterGame.Battle/TargetSelector.cs
- [ ] T029 [P] [US7] Unit tests for target selection in /tests/TurnMeterGame.Battle.Tests/TargetSelectorTests.cs

## Phase 10: User Story 8 - Affinity and Archetype Effects (P3)

- [ ] T030 [P] [US8] Implement affinity and archetype rules in /src/TurnMeterGame.Characters/CharacterRules.cs
- [ ] T031 [P] [US8] Unit tests for affinity/archetype in /tests/TurnMeterGame.Characters.Tests/CharacterRulesTests.cs

## Phase 11: User Story 9 - Execução do Ataque (P1)

- [ ] T032 [P] [US9] Implement attack logic in /src/TurnMeterGame.Battle/AttackService.cs
- [ ] T033 [P] [US9] Unit tests for attack logic in /tests/TurnMeterGame.Battle.Tests/AttackServiceTests.cs

## Phase 12: User Story 10 - Morte de Personagem (P2)

- [ ] T034 [P] [US10] Implement defeat/removal logic in /src/TurnMeterGame.Battle/DefeatService.cs
- [ ] T035 [P] [US10] Unit tests for defeat/removal in /tests/TurnMeterGame.Battle.Tests/DefeatServiceTests.cs

## Phase 13: User Story 11 - Passar a Vez Automaticamente (P2)

- [ ] T036 [P] [US11] Implement auto-pass logic in /src/TurnMeterGame.Battle/AutoPassService.cs
- [ ] T037 [P] [US11] Unit tests for auto-pass in /tests/TurnMeterGame.Battle.Tests/AutoPassServiceTests.cs

## Phase 14: User Story 12 - Log de Ações (P2)

- [ ] T038 [P] [US12] Implement battle log in /src/TurnMeterGame.Battle/BattleLog.cs
- [ ] T039 [P] [US12] Unit tests for battle log in /tests/TurnMeterGame.Battle.Tests/BattleLogTests.cs

## Phase 15: User Story 13 - Análise de Fim de Batalha (P2)

- [ ] T040 [P] [US13] Implement end-of-battle analysis in /src/TurnMeterGame.Battle/EndOfBattleService.cs
- [ ] T041 [P] [US13] Unit tests for end-of-battle in /tests/TurnMeterGame.Battle.Tests/EndOfBattleServiceTests.cs

## Phase 16: User Story 14 - Resetar Partida ao Final (P2)

- [ ] T042 [P] [US14] Implement match reset logic in /src/TurnMeterGame.GameManager/MatchResetService.cs
- [ ] T043 [P] [US14] Unit tests for match reset in /tests/TurnMeterGame.GameManager.Tests/MatchResetServiceTests.cs

## Final Phase: Polish & Cross-Cutting Concerns

- [ ] T044 Add error handling and validation across all services
- [ ] T045 Add CLI commands for all main flows in /src/TurnMeterGame.CLI/Program.cs
- [ ] T046 Add README usage instructions in /README.md

---

## Dependencies

- US1 → US2 → US3 → US4/US5/US6/US7/US8/US9/US10/US11/US12/US13/US14
- US3 (Battle Engine) is central for all battle logic

## Parallel Execution Examples

- All unit tests ([P]) can be implemented in parallel with their respective logic
- Base class creation for each domain can be parallelized
- CLI and domain logic can be developed in parallel after initial setup

## Independent Test Criteria

- Each user story phase includes unit tests for its logic
- All business rules are validated by automated tests
- Decks, teams, and battle logic are independently testable

## MVP Scope

- Phases 1–5 (Setup, Foundational, US1, US2, US3)

## Format Validation

- All tasks follow strict checklist format (checkbox, ID, [P] if parallel, [USx] if user story, file path)
