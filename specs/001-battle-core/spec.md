
# Feature Specification: Battle Core

**Feature Branch**: `[001-battle-core]`  
**Created**: [2024-06-07]  
**Status**: Draft  
**Input**: User description: "Core battle system for turn-based combat game, with initial character roster, turn meter, affinities, archetypes, and attribute restrictions."

## User Scenarios & Testing *(mandatory)*
### User Story 0 - Listar Personagens Disponíveis (Priority: P0)
Ao iniciar o jogo ou o modo PvP, o sistema deve exibir a lista de personagens disponíveis (deck) para seleção. Tanto a CPU quanto o jogador devem receber decks distintos, definidos antes da escolha dos times. Nenhum dos decks pode conter todos os personagens do jogo; cada partida pode ter decks diferentes.

**Por que essa prioridade**: Garante variedade, estratégia e evita repetição de partidas, além de ser pré-requisito para a seleção de times.

**Teste Independente**: Ao carregar o modo PvP, o sistema exibe decks distintos para CPU e jogador, cada um com um subconjunto dos personagens totais, e nenhum personagem aparece em ambos os decks.

**Cenários de Aceitação**:
1. **Dado** que o modo PvP foi iniciado, **Quando** o sistema carrega os decks, **Então** CPU e jogador recebem listas distintas de personagens disponíveis.
2. **Dado** que os decks foram definidos, **Quando** a seleção de times começa, **Então** só é possível escolher personagens do respectivo deck, sem repetições entre CPU e jogador.

### User Story 1 - Seleção de Times: CPU vs Jogador (Priority: P1)
Antes do início da batalha, a CPU escolhe aleatoriamente seu time (sem personagens repetidos). Em seguida, o jogador escolhe seu time, sem poder selecionar personagens já escolhidos pela CPU. Nenhum dos times pode ter personagens repetidos.

**Por que essa prioridade**: Garante o fluxo básico de início de partida, previne conflitos de seleção e prepara o cenário para a batalha central.

### User Story 4 - Escolha de Ação (Priority: P1)
Quando um personagem atinge 100% no Turn Meter, o sistema apresenta as opções de ação disponíveis (atualmente apenas "Atacar"). O jogador (ou CPU) deve escolher a ação para o personagem ativo.

**Por que essa prioridade**: É o ponto central de interação do jogador e define o fluxo do combate.

**Teste Independente**: Ao atingir 100% no Turn Meter, o personagem ativo só pode escolher "Atacar".

**Cenários de Aceitação**:
1. **Dado** que um personagem está pronto para agir, **Quando** sua vez começa, **Então** o sistema exibe as opções de ação disponíveis (apenas "Atacar").
2. **Dado** que o jogador/CPU escolheu a ação, **Quando** a ação é confirmada, **Então** o sistema executa a ação escolhida.

### User Story 8 - Seleção de Alvo (Priority: P1)
Quando um personagem executa a ação "Atacar", o jogador (ou CPU) deve escolher um alvo válido entre os inimigos vivos. Se houver apenas um alvo, a escolha é automática.

**Por que essa prioridade**: Garante controle estratégico e clareza na execução das ações.

**Teste Independente**: Ao atacar, o sistema permite selecionar qualquer inimigo vivo como alvo.

**Cenários de Aceitação**:
1. **Dado** que há múltiplos inimigos vivos, **Quando** o personagem ataca, **Então** o jogador/CPU pode escolher o alvo.
2. **Dado** que só resta um inimigo, **Quando** o personagem ataca, **Então** o alvo é selecionado automaticamente.

### User Story 5 - Execução do Ataque (Priority: P1)
Ao escolher "Atacar", o personagem ativo realiza um ataque contra um alvo válido do time adversário, aplicando as regras de afinidade, atributos e removendo vida do alvo.

**Por que essa prioridade**: Permite validar a mecânica central de dano e interação entre personagens.

**Teste Independente**: O ataque é executado corretamente, aplicando dano e regras de afinidade.

**Cenários de Aceitação**:
1. **Dado** que o personagem ativo escolheu atacar, **Quando** a ação é executada, **Então** o alvo perde vida conforme o cálculo de dano.
2. **Dado** que o ataque ocorre, **Quando** há vantagem ou desvantagem de afinidade, **Então** o bônus/penalidade é aplicado corretamente.

### User Story 6 - Morte de Personagem (Priority: P2)
Quando um personagem tem sua vida reduzida a zero ou menos, ele é considerado derrotado e removido da batalha (não pode mais agir nem ser alvo de ações).

**Por que essa prioridade**: Garante o fluxo correto do combate e previne bugs de personagens "mortos" agindo.

**Teste Independente**: Personagens derrotados não aparecem mais na ordem de ação nem podem ser alvos.

**Cenários de Aceitação**:
1. **Dado** que um personagem recebe dano letal, **Quando** sua vida chega a zero, **Então** ele é removido da batalha.
2. **Dado** que um personagem está derrotado, **Quando** seria sua vez de agir, **Então** ele é ignorado.

### User Story 10 - Passar a Vez Automaticamente (Priority: P2)
Se um personagem não puder executar nenhuma ação válida em seu turno (por exemplo, estiver atordoado ou não houver alvos), o sistema deve passar a vez automaticamente. O mecanismo deve estar presente desde já, mesmo que inicialmente só "passar a vez" seja possível.

**Por que essa prioridade**: Garante robustez para futuras mecânicas e previne travamentos.

**Teste Independente**: Se não houver ação possível, o turno é pulado automaticamente.

**Cenários de Aceitação**:
1. **Dado** que um personagem não pode agir, **Quando** seu turno começa, **Então** o sistema passa a vez automaticamente.
2. **Dado** que novas ações sejam implementadas no futuro, **Quando** não houver nenhuma disponível, **Então** o comportamento padrão permanece passar a vez.

### User Story 9 - Log de Ações (Priority: P2)
Após cada ação (ataque, morte, fim de batalha), o sistema deve registrar e exibir um log/resumo da ação para o jogador, mostrando quem agiu, quem foi o alvo, quanto de dano foi causado e se alguém foi derrotado.

**Por que essa prioridade**: Facilita o entendimento do que aconteceu na batalha e permite depuração.

**Teste Independente**: Após cada ação, o log é atualizado e visível ao jogador.

**Cenários de Aceitação**:
1. **Dado** que uma ação foi executada, **Quando** o turno termina, **Então** o log exibe um resumo claro da ação.
2. **Dado** que um personagem é derrotado, **Quando** isso ocorre, **Então** o log registra o evento.

### User Story 7 - Análise de Fim de Batalha (Priority: P2)
Após cada ação, o sistema verifica se todos os personagens de um time foram derrotados. Se sim, a batalha termina e o resultado é apresentado (vitória, derrota ou empate).

**Por que essa prioridade**: Define o encerramento do ciclo de jogo e permite feedback imediato ao jogador.

**Teste Independente**: O sistema detecta corretamente o fim da batalha e apresenta o resultado.

**Cenários de Aceitação**:
1. **Dado** que uma ação foi executada, **Quando** todos os personagens de um time estão derrotados, **Então** a batalha termina imediatamente.
2. **Dado** que ambos os times são derrotados na mesma ação, **Quando** a verificação ocorre, **Então** o sistema declara empate.

### User Story 11 - Resetar Partida ao Final (Priority: P2)
Após o término da batalha, o sistema deve oferecer ao jogador a opção de resetar/reiniciar o PvP. Ao escolher resetar, o usuário retorna ao início do processo: recebe um novo deck e pode escolher seu time novamente (conforme US1).

**Por que essa prioridade**: Facilita testes, repetição de partidas e melhora a experiência do usuário.

**Teste Independente**: Ao terminar uma batalha, o jogador pode optar por reiniciar e todo o fluxo de seleção de deck/time é reiniciado.

**Cenários de Aceitação**:
1. **Dado** que a batalha terminou, **Quando** o sistema exibe o resultado, **Então** o jogador pode escolher "resetar partida".
2. **Dado** que o jogador escolheu resetar, **Quando** o processo reinicia, **Então** um novo deck é gerado e o fluxo de seleção de time começa novamente.

**Teste Independente**: O jogador inicia uma partida, vê o time da CPU já formado, e só pode escolher personagens restantes, sem repetições em nenhum time.

**Cenários de Aceitação**:
1. **Dado** que a CPU escolheu seu time aleatoriamente, **Quando** o jogador for selecionar seu time, **Então** apenas personagens não escolhidos pela CPU estarão disponíveis.
2. **Dado** que ambos os times estão completos e sem repetições, **Quando** a batalha começa, **Então** cada personagem pertence a apenas um time.

---

### User Story 2 - Turn Meter System (Priority: P2)
The game uses a turn meter (0-100%) to determine when each character acts, based on their speed attribute.

**Why this priority**: The turn meter is central to the pacing and strategy of combat.

**Independent Test**: The turn order and frequency of actions reflect each character's speed, and the system is transparent to players.

**Acceptance Scenarios**:
1. **Given** characters with different speeds, **When** the battle progresses, **Then** faster characters act more frequently.
2. **Given** a character is defeated, **When** their turn would occur, **Then** they are skipped.

---

### User Story 3 - Affinity and Archetype Effects (Priority: P3)
Each character has an affinity (Físico, Magia, Tecnologia) and archetype (Tank, DPS, Suporte/Healer) that affect combat interactions.

**Why this priority**: Affinities and archetypes add depth and strategic variety to battles.

**Independent Test**: Affinity advantages/disadvantages and archetype roles are correctly applied in combat calculations.

**Acceptance Scenarios**:
1. **Given** a character attacks another with a weaker affinity, **When** the attack is resolved, **Then** the correct bonus or penalty is applied.
2. **Given** a Suporte/Healer uses a healing ability, **When** the action resolves, **Then** the correct ally receives healing.

---

### Edge Cases

- What happens if both teams lose their last character in the same round? (Draw scenario)
- How does the system handle a character with maximum speed versus minimum speed? (Turn frequency extremes)
- What if a player tries to select an action for a defeated character? (Action should be blocked)
- How are ties in turn meter resolved? (Consistent tiebreaker rule)

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The system MUST allow two players to select teams and play a full local (hotseat) battle.
- **FR-002**: The system MUST implement a turn meter (0-100%) for each character, advancing based on speed.
- **FR-003**: The system MUST enforce attribute caps: Vida ≤ 1400, Ataque ≤ 270, Defesa ≤ 350, Velocidade ≤ 200.
- **FR-004**: The system MUST apply affinity (Físico, Magia, Tecnologia) advantages as follows:
	- Magia tem vantagem contra Físico (Magia causa mais dano em Físico)
	- Físico tem vantagem contra Tecnologia (Físico causa mais dano em Tecnologia)
	- Tecnologia (Energia) tem vantagem contra Magia (Tecnologia causa mais dano em Magia)
	- Magia sofre penalidade extra contra Tecnologia/Energia (Magia causa menos dano em Tecnologia)
	- Nenhum personagem pode ter vantagem contra si mesmo; empates não geram bônus
	- Essas regras devem ser aplicadas em todos os cálculos de dano e efeitos de afinidade
- **FR-005**: The system MUST differentiate archetypes (Tank, DPS, Suporte/Healer) and their roles in combat.
- **FR-006**: The system MUST allow for defeat and removal of characters from the turn order.
- **FR-007**: The system MUST provide clear feedback for invalid actions (e.g., trying to act with a defeated character).
- **FR-008**: The system MUST support a draw outcome if both teams are defeated simultaneously.

### Key Entities

- **Personagem**: Nome, Vida, Ataque, Defesa, Velocidade, Afinidade, Arquétipo, Alinhamento (Luz/Sombras)
- **Turn Meter**: Valor percentual (0-100%) que determina quando o personagem pode agir
- **Time**: Grupo de personagens controlados por um jogador

#### Lista Inicial de Personagens

| Nome              | Afinidade   | Arquétipo   | Alinhamento | Vida | Ataque | Defesa | Velocidade |
|-------------------|-------------|-------------|-------------|------|--------|--------|------------|
| Rei Arthur        | Físico      | Tank        | Luz         | 1200 | 200    | 300    | 100        |
| Saci              | Magia       | DPS         | Luz         | 800  | 260    | 120    | 180        |
| Joana d’Arc       | Físico      | DPS         | Luz         | 950  | 240    | 180    | 140        |
| Sherlock Holmes   | Tecnologia  | Suporte     | Luz         | 900  | 160    | 160    | 160        |
| Hércules          | Físico      | Tank        | Luz         | 1300 | 220    | 320    | 90         |
| Nikola Tesla      | Tecnologia  | Suporte     | Luz         | 850  | 170    | 150    | 170        |
| Van Helsing       | Físico      | DPS         | Luz         | 900  | 250    | 150    | 150        |
| Amelia Earhart    | Tecnologia  | DPS/Suporte | Luz         | 850  | 210    | 140    | 180        |
| Uther Pendragon   | Magia       | Tank        | Sombras     | 1200 | 210    | 300    | 100        |
| Cuca              | Magia       | Suporte     | Sombras     | 900  | 170    | 160    | 160        |
| Minotauro         | Físico      | DPS         | Sombras     | 1000 | 260    | 180    | 130        |
| Conde Drácula     | Magia       | DPS         | Sombras     | 950  | 250    | 170    | 150        |
| Frankenstein      | Tecnologia  | Tank        | Sombras     | 1250 | 200    | 320    | 80         |
| Gilles de Rais    | Magia       | DPS/Tank    | Sombras     | 1100 | 230    | 220    | 120        |

#### Restrições de Atributos dos Personagens

- Vida máxima: 1400
- Ataque máximo: 270
- Defesa máxima: 350
- Velocidade máxima: 200

Nenhum personagem pode ultrapassar esses valores base. Ajustes para balanceamento devem respeitar esses limites.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Dois jogadores conseguem completar uma partida local sem bugs críticos.
- **SC-002**: O sistema respeita corretamente as regras de afinidade e arquétipo em 100% dos testes.
- **SC-003**: O turn meter reflete a velocidade dos personagens e não apresenta inconsistências.
- **SC-004**: Todos os personagens respeitam os limites de atributos definidos.
- **SC-005**: Feedback de erro é apresentado para todas as ações inválidas.

## Assumptions

- Usuários têm acesso ao mesmo dispositivo para jogar (hotseat/local).
- Não há suporte a multiplayer online nesta versão inicial.
- Interface pode ser simples e focada em funcionalidade, não em arte final.
- Balanceamento inicial pode ser ajustado em versões futuras.
