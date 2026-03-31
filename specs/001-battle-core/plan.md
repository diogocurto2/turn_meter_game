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
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                 |
|------------------------------|--------------------------------------|----------------------|------------------------------------------------------|
| CharacterData                | (propriedades)                       | ScriptableObject     | Dados base/configuração do personagem                |
| Character                    | (construtor, propriedades, métodos)  | MonoBehaviour        | Instância viva do personagem em cena                 |
| DeckService                  | GerarDeck(distintoPara: string)      | Classe pura (POCO)   | Gera um deck distinto para jogador ou CPU            |
| Deck                         | (propriedades, métodos de acesso)    | Classe pura (POCO)   | Estrutura de dados para armazenar personagens        |
| CLIController                | ObterDeck()                          | Classe pura (POCO)   | Chama DeckService, exibe lista no CLI                |
| CLIController                | MostrarDetalhesPersonagem(ID)        | Classe pura (POCO)   | Exibe detalhes de um personagem do deck no CLI       |
| TeamSelectionService         | SelecionarTime(jogadorOuCPU, deck)   | Classe pura (POCO)   | Permite seleção de time a partir de um deck          |
| TeamSelectionService         | ListarTimesCPU()                     | Classe pura (POCO)   | Retorna possíveis times gerados para a CPU           |
| Team                         | (propriedades, métodos de acesso)    | Classe pura (POCO)   | Estrutura de dados para armazenar o time             |
| CLIController                | ListarTimesCPU()                     | Classe pura (POCO)   | Exibe no CLI os times possíveis da CPU               |

**Nota:**
- `CharacterData` (ScriptableObject): asset de dados editável no editor, nunca instanciado em cena.
- `Character` (MonoBehaviour): instanciado em cena, representa o personagem ativo na batalha e referencia um CharacterData.


### US2 – Seleção de Times: CPU vs Jogador
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                 |
|------------------------------|--------------------------------------|----------------------|------------------------------------------------------|
| CLIController                | SelecionarTimeUsuario()              | Classe pura (POCO)   | Orquestra seleção de time pelo usuário via CLI       |
| CLIController                | SelecionarTimeCPU()                  | Classe pura (POCO)   | Seleção automática de time para CPU                  |
| BattleManager                | IniciarBatalha(timeUsuario, timeCPU) | MonoBehaviour        | Inicia a batalha com os times definidos              |




### US3 – Motor de Controle da Batalha
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| BattleManager                | IniciarBatalha(time1, time2)         | MonoBehaviour        | Inicia e controla o ciclo da batalha                |
| BattleManager                | AvancarTurno()                       | MonoBehaviour        | Avança o turno, atualiza estados                    |
| BattleManager                | ProcessarAcoes()                     | MonoBehaviour        | Processa ações dos personagens                      |
| BattleManager                | VerificarFimDeBatalha()              | MonoBehaviour        | Checa condições de término                          |
| BattleManager                | IntegrarTurnMeter()                  | MonoBehaviour        | Integra o TurnMeter ao fluxo da batalha             |
| BattleManager                | IntegrarBattleLog()                  | MonoBehaviour        | Gera e registra eventos no BattleLog                |
| BattleLog                    | RegistrarEvento(evento: BattleEvent)  | Classe pura (POCO)   | Registra eventos e ações ocorridas                  |
| BattleLog                    | ListarEventos() : List<BattleEvent>  | Classe pura (POCO)   | Retorna todos os eventos registrados                |
| BattleLog                    | Limpar()                             | Classe pura (POCO)   | Limpa o log                                         |
| TurnMeter                    | AtualizarTurnMeter()                 | Classe pura (POCO)   | Atualiza valores do turn meter                      |
| TurnMeter                    | ProximoParaAgir() : Character        | Classe pura (POCO)   | Determina próximo personagem a agir                 |
| CLIController                | ExibirEstadoBatalha()                | Classe pura (POCO)   | Mostra estado atual da batalha no CLI               |



### US4 – Timer de Tempo Real da Batalha
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| BattleTimer                  | IniciarTimer(duracao)                | MonoBehaviour        | Inicia o timer da batalha                           |
| BattleTimer                  | VerificarTimeout()                   | MonoBehaviour        | Checa se o tempo expirou                            |
| BattleManager                | EncerrarPorTimeout()                 | MonoBehaviour        | Encerra batalha por tempo                           |
| CLIController                | ExibirTempoRestante()                | Classe pura (POCO)   | Mostra tempo restante no CLI                        |


### US5 – Turn Meter System
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| TurnMeter                    | AtualizarTurnMeter()                  | Classe pura (POCO)   | Atualiza valores do turn meter                      |
| TurnMeter                    | ProximoParaAgir()                     | Classe pura (POCO)   | Determina próximo personagem a agir                 |
| BattleManager                | IntegrarTurnMeter()                   | MonoBehaviour        | Integra turn meter ao fluxo da batalha              |
| CLIController                | ExibirOrdemTurnos()                   | Classe pura (POCO)   | Mostra ordem de ação dos personagens no CLI         |


### US6 – Escolha de Ação
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| ActionSelector               | ListarAcoesDisponiveis(personagem)    | Classe pura (POCO)   | Lista ações possíveis para o personagem             |
| ActionSelector               | SelecionarAcao(personagem, acao)      | Classe pura (POCO)   | Permite seleção de ação pelo usuário/CPU            |
| BattleManager                | ExecutarAcao(personagem, acao)        | MonoBehaviour        | Executa ação escolhida                              |
| CLIController                | ExibirAcoesDisponiveis()              | Classe pura (POCO)   | Mostra opções de ação no CLI                        |
| CLIController                | SelecionarAcao()                      | Classe pura (POCO)   | Seleciona uma acao do usuario, do CPU a acao é selecionada automaticamente |


### US7 – Seleção de Alvo
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| TargetSelector               | ListarAlvosValidos(personagem)        | Classe pura (POCO)   | Lista alvos possíveis para ação                     |
| TargetSelector               | SelecionarAlvo(personagem, alvos)     | Classe pura (POCO)   | Permite seleção de alvo pelo usuário/CPU            |
| BattleManager                | ValidarAlvo(alvo)                     | MonoBehaviour        | Valida se alvo é permitido                          |
| CLIController                | ExibirAlvosDisponiveis()              | Classe pura (POCO)   | Mostra opções de alvo no CLI                        |


### US8 – Affinity and Archetype Effects
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| CharacterRules               | CalcularAfinidade(atacante, alvo)     | Classe pura (POCO)   | Calcula bônus/penalidade de afinidade               |
| CharacterRules               | CalcularArquetipo(personagem)         | Classe pura (POCO)   | Aplica efeitos de arquétipo                         |
| BattleManager                | IntegrarAfinidadeArquetipo()          | MonoBehaviour        | Integra regras ao fluxo de dano/ação                |
| CLIController                | ExibirEfeitosAfinidade()              | Classe pura (POCO)   | Mostra efeitos de afinidade/arquetipo no CLI        |


### US9 – Execução do Ataque
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| AttackService                | ExecutarAtaque(atacante, alvo)        | Classe pura (POCO)   | Realiza cálculo e aplicação de dano                 |
| BattleManager                | ProcessarAtaque()                     | MonoBehaviour        | Orquestra execução do ataque                        |
| CLIController                | ExibirResultadoAtaque()               | Classe pura (POCO)   | Mostra resultado do ataque no CLI                   |


### US10 – Morte de Personagem
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| DefeatService                | VerificarMorte(personagem)            | Classe pura (POCO)   | Checa se personagem foi derrotado                   |
| BattleManager                | RemoverPersonagem(personagem)         | MonoBehaviour        | Remove personagem da batalha                        |
| CLIController                | ExibirMensagemMorte()                 | Classe pura (POCO)   | Mostra mensagem de derrota no CLI                   |


### US11 – Passar a Vez Automaticamente
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| AutoPassService              | VerificarAcoesPossiveis(personagem)   | Classe pura (POCO)   | Checa se personagem pode agir                       |
| AutoPassService              | PassarVez(personagem)                 | Classe pura (POCO)   | Passa a vez automaticamente                         |
| BattleManager                | IntegrarAutoPass()                    | MonoBehaviour        | Integra lógica de auto-pass ao fluxo                |
| CLIController                | ExibirMensagemPassarVez()             | Classe pura (POCO)   | Mostra mensagem de turno pulado no CLI              |


### US12 – Log de Ações
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| BattleLog                    | RegistrarEvento(evento)               | Classe pura (POCO)   | Registra evento de ação, morte, fim de batalha      |
| BattleLog                    | ListarEventos()                       | Classe pura (POCO)   | Lista eventos registrados                           |
| CLIController                | ExibirLogBatalha()                    | Classe pura (POCO)   | Mostra log de ações no CLI                          |


### US13 – Análise de Fim de Batalha
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| EndOfBattleService           | VerificarFimDeBatalha()               | Classe pura (POCO)   | Checa condições de vitória, derrota ou empate       |
| BattleManager                | EncerrarBatalha(resultado)            | MonoBehaviour        | Finaliza batalha e define resultado                 |
| CLIController                | ExibirResultadoFinal()                | Classe pura (POCO)   | Mostra resultado final no CLI                       |


### US14 – Resetar Partida ao Final
| Classe/Asset                  | Função/Método                        | Tipo Unity           | Descrição/responsabilidade principal                |
|------------------------------|--------------------------------------|----------------------|-----------------------------------------------------|
| MatchResetService            | ResetarPartida()                      | Classe pura (POCO)   | Reinicia o fluxo de seleção e batalha               |
| BattleManager                | IntegrarReset()                       | MonoBehaviour        | Integra lógica de reset ao ciclo principal          |
| CLIController                | ExibirOpcaoReset()                    | Classe pura (POCO)   | Mostra opção de resetar partida no CLI              |



#### Contratos Públicos e Eventos

**BattleManager**
- Propriedades: EstadoAtual (BattleState), Time1, Time2, TurnoAtual, Log (BattleLog)
- Eventos: OnBattleStarted, OnTurnAdvanced, OnActionProcessed, OnBattleEnded

**BattleLog**
- Estrutura de evento (BattleEvent):
	- Timestamp
	- TipoEvento (ex: "Ataque", "Morte", "TurnoAvancado", "FimDeBatalha")
	- Descrição
	- Dados adicionais (ex: IDs dos personagens envolvidos)

**Exemplo de evento:**
```json
{
	"Timestamp": "2026-03-31T20:00:00Z",
	"TipoEvento": "Ataque",
	"Descricao": "Alice atacou Bob causando 120 de dano.",
	"Dados": { "AtacanteId": 1, "AlvoId": 2, "Dano": 120 }
}
```

**TurnMeter**
- Propriedades: Lista de personagens e seus valores de turn meter
- Métodos: AtualizarTurnMeter(), ProximoParaAgir()

**Critérios de Sucesso (mensuráveis):**
- 100% dos eventos relevantes da batalha são registrados no BattleLog
- O método ProximoParaAgir() sempre retorna o personagem correto conforme o estado do TurnMeter
- O ciclo de batalha executa do início ao fim sem intervenção manual