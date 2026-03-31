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
├── TurnMeterGame.Battle/        # DLL de domínio para regras e operações de batalha - implementa as User Story 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 e 13
├── TurnMeterGame.GameManager/   # DLL de domínio para regras e operações de gerenciamento da partida - implementa as user story 2 e 14
├── TurnMeterGame.CLI/           # Console Application (apresentação, comandos, interface CLI)
│   └── (não contém lógica de domínio; apenas recebe comandos do usuário, chama serviços de domínio e apresenta resultados)
```

#### CLI (TurnMeterGame.CLI)

O projeto CLI **não implementa nenhuma lógica de domínio**. Ele apenas:
- Recebe comandos do usuário (ex: iniciar batalha, listar personagens do deck, mostrar detalhes de personagem)
- Chama os serviços apropriados do domínio (ex: BattleManager, DeckService)
- Exibe o resultado no formato de interface de linha de comando (CLI)
- Não toma decisões de lógica de jogo: toda decisão (ex: escolha de ação do CPU, regras de batalha, sorteio, validação) é feita nos serviços de domínio.

**Validação e feedback de erro:**
- Todas as entradas do usuário (seleção de personagem, time, ação, alvo) devem ser validadas pelo domínio.
- O CLI deve apresentar mensagens de erro claras e amigáveis sempre que a entrada for inválida, orientando o usuário a tentar novamente.

## Implementation Notes

- Cada domínio principal (Deck, Personagem, GameManager) será implementado em uma DLL separada, seguindo DDD.
- As DLLs expõem apenas regras e operações de domínio, sem dependência de UI ou apresentação.
- O projeto CLI referencia as DLLs e orquestra a interação via terminal.
- Testes unitários para cada domínio, cobrindo regras e fluxos principais.
- O plano será expandido para incluir todas as User Stories conforme forem detalhadas.

## Planned Classes and Functions by User Story

### US1 – Listar Personagens Disponíveis
| Classe                        | Função/Método                        | Descrição/responsabilidade principal                 |
|-------------------------------|--------------------------------------|------------------------------------------------------|
| Character                     | (construtor, propriedades)           | Representa um personagem individual                  |
| DeckService                   | GerarDeck(distintoPara: string)      | Gera um deck distinto para jogador ou CPU            |
| Deck                          | (propriedades, métodos de acesso)    | Estrutura de dados para armazenar personagens        |
| CLIController                 | ObterDeck()                          | Chama DeckService, exibe lista no CLI                |
| CLIController                 | MostrarDetalhesPersonagem(ID)        | Exibe detalhes de um personagem do deck no CLI       |
| TeamSelectionService          | SelecionarTime(jogadorOuCPU, deck)   | Permite seleção de time a partir de um deck          |
| TeamSelectionService          | ListarTimesCPU()                     | Retorna possíveis times gerados para a CPU           |
| Team                          | (propriedades, métodos de acesso)    | Estrutura de dados para armazenar o time             |
| CLIController                 | ListarTimesCPU()                     | Exibe no CLI os times possíveis da CPU               |

### US2 – Seleção de Times: CPU vs Jogador
| Classe                        | Função/Método                        | Descrição/responsabilidade principal                 |
|-------------------------------|--------------------------------------|------------------------------------------------------|
| CLIController                 | SelecionarTimeUsuario()              | Orquestra seleção de time pelo usuário via CLI       |
| CLIController                 | SelecionarTimeCPU()                  | Seleção automática de time para CPU                  |
| BattleManager                 | IniciarBatalha(timeUsuario, timeCPU) | Inicia a batalha com os times definidos              |


### US3 – Motor de Controle da Batalha
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| BattleManager    | IniciarBatalha(time1, time2)         | Inicia e controla o ciclo da batalha                |
| BattleManager    | AvancarTurno()                       | Avança o turno, atualiza estados                    |
| BattleManager    | ProcessarAcoes()                     | Processa ações dos personagens                      |
| BattleManager    | VerificarFimDeBatalha()              | Checa condições de término                          |
| BattleLog        | RegistrarEvento(evento)               | Registra eventos e ações ocorridas                  |
| CLIController    | ExibirEstadoBatalha()                | Mostra estado atual da batalha no CLI               |

### US4 – Timer de Tempo Real da Batalha
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| BattleTimer      | IniciarTimer(duracao)                 | Inicia o timer da batalha                           |
| BattleTimer      | VerificarTimeout()                    | Checa se o tempo expirou                            |
| BattleManager    | EncerrarPorTimeout()                  | Encerra batalha por tempo                           |
| CLIController    | ExibirTempoRestante()                 | Mostra tempo restante no CLI                        |

### US5 – Turn Meter System
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| TurnMeter        | AtualizarTurnMeter()                  | Atualiza valores do turn meter                      |
| TurnMeter        | ProximoParaAgir()                     | Determina próximo personagem a agir                 |
| BattleManager    | IntegrarTurnMeter()                   | Integra turn meter ao fluxo da batalha              |
| CLIController    | ExibirOrdemTurnos()                   | Mostra ordem de ação dos personagens no CLI         |

### US6 – Escolha de Ação
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| ActionSelector   | ListarAcoesDisponiveis(personagem)    | Lista ações possíveis para o personagem             |
| ActionSelector   | SelecionarAcao(personagem, acao)      | Permite seleção de ação pelo usuário/CPU            |
| BattleManager    | ExecutarAcao(personagem, acao)        | Executa ação escolhida                              |
| CLIController    | ExibirAcoesDisponiveis()              | Mostra opções de ação no CLI                        |
| CLIController    | SelecionarAcao()                      | Seleciona uma acao do usuario, do CPU a acao é selecionada automaticamente                      |

### US7 – Seleção de Alvo
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| TargetSelector   | ListarAlvosValidos(personagem)        | Lista alvos possíveis para ação                     |
| TargetSelector   | SelecionarAlvo(personagem, alvos)     | Permite seleção de alvo pelo usuário/CPU            |
| BattleManager    | ValidarAlvo(alvo)                     | Valida se alvo é permitido                          |
| CLIController    | ExibirAlvosDisponiveis()              | Mostra opções de alvo no CLI                        |

### US8 – Affinity and Archetype Effects
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| CharacterRules   | CalcularAfinidade(atacante, alvo)     | Calcula bônus/penalidade de afinidade               |
| CharacterRules   | CalcularArquetipo(personagem)         | Aplica efeitos de arquétipo                         |
| BattleManager    | IntegrarAfinidadeArquetipo()          | Integra regras ao fluxo de dano/ação                |
| CLIController    | ExibirEfeitosAfinidade()              | Mostra efeitos de afinidade/arquetipo no CLI        |

### US9 – Execução do Ataque
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| AttackService    | ExecutarAtaque(atacante, alvo)        | Realiza cálculo e aplicação de dano                 |
| BattleManager    | ProcessarAtaque()                     | Orquestra execução do ataque                        |
| CLIController    | ExibirResultadoAtaque()               | Mostra resultado do ataque no CLI                   |

### US10 – Morte de Personagem
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| DefeatService    | VerificarMorte(personagem)            | Checa se personagem foi derrotado                   |
| BattleManager    | RemoverPersonagem(personagem)         | Remove personagem da batalha                        |
| CLIController    | ExibirMensagemMorte()                 | Mostra mensagem de derrota no CLI                   |

### US11 – Passar a Vez Automaticamente
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| AutoPassService  | VerificarAcoesPossiveis(personagem)   | Checa se personagem pode agir                       |
| AutoPassService  | PassarVez(personagem)                 | Passa a vez automaticamente                         |
| BattleManager    | IntegrarAutoPass()                    | Integra lógica de auto-pass ao fluxo                |
| CLIController    | ExibirMensagemPassarVez()             | Mostra mensagem de turno pulado no CLI              |

### US12 – Log de Ações
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| BattleLog        | RegistrarEvento(evento)               | Registra evento de ação, morte, fim de batalha      |
| BattleLog        | ListarEventos()                       | Lista eventos registrados                           |
| CLIController    | ExibirLogBatalha()                    | Mostra log de ações no CLI                          |

### US13 – Análise de Fim de Batalha
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| EndOfBattleService| VerificarFimDeBatalha()              | Checa condições de vitória, derrota ou empate       |
| BattleManager    | EncerrarBatalha(resultado)            | Finaliza batalha e define resultado                 |
| CLIController    | ExibirResultadoFinal()                | Mostra resultado final no CLI                       |

### US14 – Resetar Partida ao Final
| Classe           | Função/Método                        | Descrição/responsabilidade principal                |
|------------------|--------------------------------------|-----------------------------------------------------|
| MatchResetService| ResetarPartida()                      | Reinicia o fluxo de seleção e batalha               |
| BattleManager    | IntegrarReset()                       | Integra lógica de reset ao ciclo principal          |
| CLIController    | ExibirOpcaoReset()                    | Mostra opção de resetar partida no CLI              |