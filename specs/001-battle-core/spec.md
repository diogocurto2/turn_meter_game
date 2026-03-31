# Feature Specification: Battle Core

**Feature Branch**: `[001-battle-core]`  
**Created**: [2024-06-07]  
**Status**: Draft  
**Input**: User description: "Core battle system for turn-based combat game, with initial character roster, turn meter, affinities, archetypes, and attribute restrictions."

## User Scenarios & Testing *(mandatory)*
### User Story 1 - Escolha do Adversário e Deck do Jogador (Priority: P0)
Ao iniciar o modo PvP, o sistema deve exibir 5 times adversários aleatórios (montados pela CPU, cada um com 4 personagens distintos). O jogador escolhe qual desses times deseja enfrentar. Após a escolha, a CPU gera um deck novo para o jogador, contendo 6 personagens aleatórios (sem repetições, de todos os personagens disponíveis no jogo).

**Por que essa prioridade**: Garante variedade estratégica, permite ao jogador escolher o desafio e limita as opções do jogador, tornando cada partida única.

**Teste Independente**: Ao iniciar o modo PvP, o jogador vê 5 times adversários distintos para escolher. Após escolher o adversário, recebe um deck de 6 personagens aleatórios para montar seu próprio time.

**Cenários de Aceitação**:
1. **Dado** que o modo PvP foi iniciado, **Quando** o sistema exibe os times adversários, **Então** o jogador pode escolher um dos 5 times para enfrentar.
2. **Dado** que o jogador escolheu o time adversário, **Quando** o sistema gera o deck, **Então** o jogador recebe uma lista de 6 personagens aleatórios (sem repetições) para montar seu time.
3. **Dado** que o deck do jogador foi definido, **Quando** a seleção de times começa, **Então** só é possível escolher personagens do respectivo deck, sem repetições.

### User Story 2 - Seleção de Times: Escolha do Adversário e Montagem do Time (Priority: P0)
Após visualizar os 5 times adversários gerados pela CPU, o jogador escolhe um deles para lutar contra. Em seguida, o jogador monta seu próprio time de 4 personagens, escolhendo a partir do deck de 6 personagens aleatórios recebido.

**Por que essa prioridade**: Garante que o jogador tenha agência na escolha do desafio e na montagem do seu time, além de preparar o cenário para a batalha central.

**Cenários de Aceitação**:
1. **Dado** que os 5 times adversários estão disponíveis, **Quando** o jogador escolhe um deles, **Então** esse time é definido como o adversário da partida.
2. **Dado** que o jogador recebeu seu deck de 6 personagens, **Quando** monta seu time, **Então** só pode escolher 4 personagens distintos desse deck para formar seu time.

### User Story 3 - Motor de Controle da Batalha (Priority: P0)
O sistema deve possuir um motor central de batalha responsável por orquestrar o fluxo do combate: avançar turnos, atualizar o turn meter, acionar a escolha de ações, processar ataques, verificar mortes, registrar logs e determinar o fim da batalha.

**Por que essa prioridade**: Garante que todas as regras e etapas do combate sejam executadas de forma ordenada, robusta e previsível, evitando inconsistências e bugs de fluxo.

**Teste Independente**: Ao iniciar uma batalha, o motor executa automaticamente todas as etapas do combate, do início ao fim, sem necessidade de intervenção manual entre as fases. para iniciar a batalha, o motor inicia com o time da cpu escolhida e com o time do usuario escolhido

**Cenários de Aceitação**:
1. **Dado** que uma batalha foi iniciada, **Quando** o motor é acionado, **Então** ele executa o ciclo completo de turnos, ações, verificações e encerramento.
2. **Dado** que ocorrem eventos como morte de personagem ou empate, **Quando** o motor processa o turno, **Então** ele atualiza corretamente o estado da batalha e aciona as regras apropriadas.

### User Story 4 - Timer de Tempo Real da Batalha (Priority: P1)
Ao iniciar a batalha, o sistema deve iniciar um timer de tempo real (padrão: 1 minuto e 20 segundos). Quando o tempo se esgota, o timer emite um aviso para o motor de batalha encerrar imediatamente a partida, declarando empate independentemente do estado atual.

Por que essa prioridade: Garante que as partidas tenham duração máxima definida, previne travamentos e incentiva decisões rápidas.

Teste Independente: Ao atingir o tempo limite, a batalha termina automaticamente e o resultado é empate.

Cenários de Aceitação:

Dado que uma batalha foi iniciada, Quando o timer chega a 0, Então o sistema encerra a batalha e declara empate.
Dado que o tempo está próximo do fim, Quando o timer expira, Então o jogador é notificado e não pode mais realizar ações.

### User Story 5 - Turn Meter System (Priority: P2)
The game uses a turn meter (0-100%) to determine when each character acts, based on their speed attribute.

**Why this priority**: The turn meter is central to the pacing and strategy of combat.

**Independent Test**: The turn order and frequency of actions reflect each character's speed, and the system is transparent to players.

**Acceptance Scenarios**:
1. **Given** characters with different speeds, **When** the battle progresses, **Then** faster characters act more frequently.
2. **Given** a character is defeated, **When** their turn would occur, **Then** they are skipped.


### User Story 6 - Escolha de Ação (Priority: P1)
Quando um personagem atinge 100% no Turn Meter, o sistema apresenta as opções de ação disponíveis (atualmente apenas "Atacar"). O jogador (ou CPU) deve escolher a ação para o personagem ativo.

**Por que essa prioridade**: É o ponto central de interação do jogador e define o fluxo do combate.

**Teste Independente**: Ao atingir 100% no Turn Meter, o personagem ativo só pode escolher "Atacar".

**Cenários de Aceitação**:
1. **Dado** que um personagem está pronto para agir, **Quando** sua vez começa, **Então** o sistema exibe as opções de ação disponíveis (apenas "Atacar").
2. **Dado** que o jogador/CPU escolheu a ação, **Quando** a ação é confirmada, **Então** o sistema executa a ação escolhida.

### User Story 7 - Seleção de Alvo (Priority: P1)
Quando um personagem executa a ação "Atacar", o jogador (ou CPU) deve escolher um alvo válido entre os inimigos vivos. Se houver apenas um alvo, a escolha é automática.

**Por que essa prioridade**: Garante controle estratégico e clareza na execução das ações.

**Teste Independente**: Ao atacar, o sistema permite selecionar qualquer inimigo vivo como alvo.

**Cenários de Aceitação**:
1. **Dado** que há múltiplos inimigos vivos, **Quando** o personagem ataca, **Então** o jogador/CPU pode escolher o alvo.
2. **Dado** que só resta um inimigo, **Quando** o personagem ataca, **Então** o alvo é selecionado automaticamente.

### User Story 8 - Affinity and Archetype Effects (Priority: P3)
Each character has an affinity (Físico, Magia, Tecnologia) and archetype (Tank, DPS, Suporte/Healer) that affect combat interactions.

**Why this priority**: Affinities and archetypes add depth and strategic variety to battles.

**Independent Test**: Affinity advantages/disadvantages and archetype roles are correctly applied in combat calculations.

**Acceptance Scenarios**:
1. **Given** a character attacks another with a weaker affinity, **When** the attack is resolved, **Then** the correct bonus or penalty is applied.
2. **Given** a Suporte/Healer uses a healing ability, **When** the action resolves, **Then** the correct ally receives healing.


### User Story 9 - Execução do Ataque (Priority: P1)
Ao escolher "Atacar", o personagem ativo realiza um ataque contra um alvo válido do time adversário, aplicando as regras de afinidade, atributos e removendo vida do alvo.

**Por que essa prioridade**: Permite validar a mecânica central de dano e interação entre personagens.

**Teste Independente**: O ataque é executado corretamente, aplicando dano e regras de afinidade.

**Cenários de Aceitação**:
1. **Dado** que o personagem ativo escolheu atacar, **Quando** a ação é executada, **Então** o alvo perde vida conforme o cálculo de dano.
2. **Dado** que o ataque ocorre, **Quando** há vantagem ou desvantagem de afinidade, **Então** o bônus/penalidade é aplicado corretamente.

### User Story 10 - Morte de Personagem (Priority: P2)
Quando um personagem tem sua vida reduzida a zero ou menos, ele é considerado derrotado e removido da batalha (não pode mais agir nem ser alvo de ações).

**Por que essa prioridade**: Garante o fluxo correto do combate e previne bugs de personagens "mortos" agindo.

**Teste Independente**: Personagens derrotados não aparecem mais na ordem de ação nem podem ser alvos.

**Cenários de Aceitação**:
1. **Dado** que um personagem recebe dano letal, **Quando** sua vida chega a zero, **Então** ele é removido da batalha.
2. **Dado** que um personagem está derrotado, **Quando** seria sua vez de agir, **Então** ele é ignorado.

### User Story 11 - Passar a Vez Automaticamente (Priority: P2)
Se um personagem não puder executar nenhuma ação válida em seu turno (por exemplo, estiver atordoado ou não houver alvos), o sistema deve passar a vez automaticamente. O mecanismo deve estar presente desde já, mesmo que inicialmente só "passar a vez" seja possível.

**Por que essa prioridade**: Garante robustez para futuras mecânicas e previne travamentos.

**Teste Independente**: Se não houver ação possível, o turno é pulado automaticamente.

**Cenários de Aceitação**:
1. **Dado** que um personagem não pode agir, **Quando** seu turno começa, **Então** o sistema passa a vez automaticamente.
2. **Dado** que novas ações sejam implementadas no futuro, **Quando** não houver nenhuma disponível, **Então** o comportamento padrão permanece passar a vez.

### User Story 12 - Log de Ações (Priority: P2)
Após cada ação (ataque, morte, fim de batalha), o sistema deve registrar e exibir um log/resumo da ação para o jogador, mostrando quem agiu, quem foi o alvo, quanto de dano foi causado e se alguém foi derrotado.

**Por que essa prioridade**: Facilita o entendimento do que aconteceu na batalha e permite depuração.

**Teste Independente**: Após cada ação, o log é atualizado e visível ao jogador.

**Cenários de Aceitação**:
1. **Dado** que uma ação foi executada, **Quando** o turno termina, **Então** o log exibe um resumo claro da ação.
2. **Dado** que um personagem é derrotado, **Quando** isso ocorre, **Então** o log registra o evento.

### User Story 13 - Análise de Fim de Batalha (Priority: P2)
Após cada ação, o sistema verifica se todos os personagens de um time foram derrotados. Se sim, a batalha termina e o resultado é apresentado (vitória, derrota ou empate).

**Por que essa prioridade**: Define o encerramento do ciclo de jogo e permite feedback imediato ao jogador.

**Teste Independente**: O sistema detecta corretamente o fim da batalha e apresenta o resultado.

**Cenários de Aceitação**:
1. **Dado** que uma ação foi executada, **Quando** todos os personagens de um time estão derrotados, **Então** a batalha termina imediatamente.
2. **Dado** que ambos os times são derrotados na mesma ação, **Quando** a verificação ocorre, **Então** o sistema declara empate.

### User Story 14 - Resetar Partida ao Final (Priority: P2)
Após o término da batalha, o sistema deve oferecer ao jogador a opção de resetar/reiniciar o PvP. Ao escolher resetar, o usuário retorna ao início do processo: recebe um novo deck e pode escolher seu time novamente (conforme US1).

**Por que essa prioridade**: Facilita testes, repetição de partidas e melhora a experiência do usuário.

**Teste Independente**: Ao terminar uma batalha, o jogador pode optar por reiniciar e todo o fluxo de seleção de deck/time é reiniciado.

**Cenários de Aceitação**:
1. **Dado** que a batalha terminou, **Quando** o sistema exibe o resultado, **Então** o jogador pode escolher "resetar partida".
2. **Dado** que o jogador escolheu resetar, **Quando** o processo reinicia, **Então** um novo deck é gerado e o fluxo de seleção de time começa novamente.

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
   - Red (Físico): vantagem de ataque contra Blue (Mágico), desvantagem contra Green (Energia)
   - Green (Energia): vantagem de ataque contra Red (Físico), desvantagem contra Blue (Mágico)
   - Blue (Mágico): vantagem de ataque contra Green (Energia), desvantagem contra Red (Físico)
   - Mesma afinidade ou relação neutra: multiplicador 1.0x
   - Vantagem de afinidade: multiplicador 1.25x no dano
   - Desvantagem de afinidade: multiplicador 0.75x no dano
   - Multiplicadores podem ser ajustados para balanceamento, mas o padrão inicial é 1.25x para vantagem e 0.75x para desvantagem.
   - Essas regras devem ser aplicadas em todos os cálculos de dano e efeitos de afinidade
- **FR-005**: The system MUST differentiate archetypes (Tank, DPS, Suporte/Healer) and their roles in combat.
- **FR-006**: The system MUST allow for defeat and removal of characters from the turn order.
- **FR-007**: The system MUST provide clear feedback for invalid actions (e.g., trying to act with a defeated character).
- **FR-008**: The system MUST support a draw outcome if both teams are defeated simultaneously.

### Key Entities

**Personagem**
  - Nome
  - Vida (atual e máxima)
  - Ataque
  - Defesa
  - Velocidade
  - Afinidade (Físico, Magia, Tecnologia)
  - Arquétipo (Tank, DPS, Suporte/Healer)
  - Alinhamento (Luz/Sombras)
  - Status (vivo/derrotado, efeitos temporários)
  - Turn Meter (valor percentual 0-100%)
  - Ações disponíveis (ex: Atacar, Passar, futuras habilidades)

**Time**
  - Jogador (CPU ou humano)
  - Lista de personagens (sem repetições)
  - Deck disponível para seleção

**Motor de Batalha**
  - Estado da batalha (em andamento, finalizada, empate)
  - Ordem de ação (fila baseada no Turn Meter)
  - Controle de turnos e avanço do Turn Meter
  - Processamento de ações e verificação de condições de fim
  - Log de ações e eventos
  - Timer de tempo real da batalha

**Ação**
  - Tipo (Atacar, Passar, Habilidade futura)
  - Personagem executor
  - Alvo(s)
  - Resultado (dano, cura, efeitos, morte)

**Log de Batalha**
  - Lista de eventos (quem agiu, ação, alvo, resultado, mortes, fim de batalha)

**Deck**
  - Subconjunto de personagens disponíveis para seleção em cada partida (distinto para CPU e jogador)

**Turn Meter**
  - Valor percentual (0-100%) que determina quando o personagem pode agir

**Timer**
  - Cronômetro de tempo real para duração máxima da batalha
  - Valor padrão: 1m20s
  - Ao expirar, aciona o fim da batalha com resultado de empate

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

## Clarificações (Sessão 2026-03-29)
- Turn Meter: avanço linear, velocidade = pontos por tick. Futuras versões podem alterar a fórmula.
- Empate de velocidade: ordem fixa, primeiro quem está atacando (player), depois quem está defendendo (CPU).
- Timer: se expirar durante uma ação, a ação é cancelada imediatamente e o resultado é o estado do momento (se não houver ganhador, é empate).
- Tamanho dos decks e times: fixo — 8 personagens no deck, 4 no time (time sempre 4, deck pode mudar no futuro).
- Composição dos decks: aleatório puro para MVP.
- Ações disponíveis: apenas “Atacar” no MVP, mas a estrutura já prevê múltiplas ações no futuro.
- Critério de empate: apenas timer (eliminação simultânea não é critério de empate).
