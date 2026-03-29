# Implementation Plan: Battle Core CLI

**Branch**: `[001-battle-core]` | **Date**: 2026-03-29 | **Spec**: [specs/001-battle-core/spec.md]
**Input**: Feature specification from `/specs/001-battle-core/spec.md`

## Summary

Implementar a lógica de sorteio de decks e seleção de times (CPU e jogador) para o sistema de batalha, utilizando C# em um projeto Console Application (CLI) separado da camada Unity. O objetivo é garantir regras de seleção, decks distintos e times sem repetições, sem interface gráfica.

## Technical Context

**Language/Version**: C# 10 (.NET 6 ou superior)
**Primary Dependencies**: System, System.Collections.Generic
**Storage**: N/A (dados em memória)
**Testing**: xUnit ou NUnit
**Target Platform**: Console (CLI), multiplataforma
**Project Type**: Biblioteca de domínio + Console Application (CLI)
**Performance Goals**: Execução instantânea para operações de sorteio e seleção
**Constraints**: Separação clara entre domínio e apresentação (CLI)
**Scale/Scope**: Lógica de decks, times e seleção para 10-20 personagens

## Constitution Check

- Separação de domínio e apresentação
- Lógica de regras testável sem dependência de UI
- Decks e times distintos, sem repetições

## Project Structure

### Documentation (this feature)

```text
specs/001-battle-core/
├── plan.md              # Este arquivo
├── research.md          # (futuro)
├── data-model.md        # (futuro)
├── quickstart.md        # (futuro)
├── contracts/           # (futuro)
└── tasks.md             # (futuro)
```

### Source Code (repository root)

```text
src/
├── TurnMeterGame.Deck/          # DLL de domínio para regras e operações de Deck - implementa as User Story 1
├── TurnMeterGame.Characters/    # DLL de domínio para regras e operações de Personagens - implementa as User Story 1 
├── TurnMeterGame.Battle/    # DLL de domínio para regras e operações de Personagens - implementa as User Story 3, 4, 5, 6, 7, 8, 9 10, 11, 12 e 13
├── TurnMeterGame.GameManager/   # DLL de domínio para regras e operações de gerenciamento da partida - implementa as user story 2 e 14
└── TurnMeterGame.CLI/           # Console Application (entrada, comandos, apresentação)
```

## Implementation Notes

- Cada domínio principal (Deck, Personagem, GameManager) será implementado em uma DLL separada, seguindo DDD.
- As DLLs expõem apenas regras e operações de domínio, sem dependência de UI ou apresentação.
- O projeto CLI referencia as DLLs e orquestra a interação via terminal.
- Testes unitários para cada domínio, cobrindo regras e fluxos principais.
- O plano será expandido para incluir todas as User Stories conforme forem detalhadas.

## Next Steps

- Detalhar data-model.md e contratos
- Implementar lógica de domínio e CLI
- Validar regras com testes automatizados
