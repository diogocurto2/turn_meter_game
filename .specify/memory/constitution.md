
# Arkham Prototype (Turn-Based Combat) Constitution


## Core Principles

### 1. Simplicidade acima de tudo
- Escopo mínimo viável primeiro. Nunca adicionar feature antes de ter a anterior 100% funcional e divertida.

### 2. Código limpo e arquitetado
- Seguir SOLID, Clean Architecture onde fizer sentido.
- Usar ScriptableObjects para dados (personagens, habilidades, afinidades).
- Command Pattern para ações de combate.
- State Machine clara para o fluxo de batalha.

### 3. Turn Meter é o coração do jogo
- Toda mecânica deve girar em torno do sistema de velocidade/Turn Meter.
- Manipulação de TM (aumentar/reduzir) deve ser a principal fonte de estratégia.

### 4. Data-driven sempre que possível
- Stats, habilidades, afinidades e efeitos devem vir de ScriptableObjects ou arquivos de dados (não hard-coded).


### 5. Compartilhamento e Web
- O objetivo é lançar uma versão web jogável e fácil de compartilhar.
- Multiplayer local e/ou online podem ser considerados, mas o foco é acessibilidade e compartilhamento.

### 6. Testabilidade
- Combate deve ser fácil de unit testar (BattleSimulator pode rodar batalhas sem Unity).

### 7. Performance não é preocupação inicial
- Prioridade é clareza e diversão. Otimização só se ficar lento.

### 8. Estilo Visual
- Protótipo pode ser feio (quadrados coloridos + texto).
- Foco total em mecânica antes de arte/polimento.


## Technology
- Engine: Unity 6 (ou a versão mais recente estável) ou framework web adequado
- Linguagem: C# (Unity) ou JavaScript/TypeScript (web)
- Padrões: ScriptableObjects, Command, State Machine, Observer onde necessário


## Definition of Done
- Feature compila sem warnings
- Funciona no Editor + Build Windows/Mac
- Tem pelo menos 1 teste automatizado (quando aplicável)
- Documentada na spec
- Testada com amigos e considerada "divertida"


## Governance
- Esta constituição substitui quaisquer outras práticas conflitantes no projeto.
- Alterações exigem documentação clara, aprovação do grupo e atualização deste documento.
- Toda feature, revisão ou PR deve ser avaliada quanto à aderência aos princípios acima.
- Revisões periódicas são recomendadas a cada ciclo de desenvolvimento ou sempre que houver mudança significativa de escopo.


<!--
Sync Impact Report
- Version change: 1.0.0 → 1.1.0
- Modified principles: 5 (Hotseat Local Only → Compartilhamento e Web), Technology
- Added sections: nenhuma
- Removed sections: nenhuma
- Templates requiring updates: ✅ plan-template.md, ✅ spec-template.md, ✅ tasks-template.md, ✅ README.md
- Follow-up TODOs: nenhuma pendência
-->

**Version**: 1.1.0 | **Ratified**: 2026-03-26 | **Last Amended**: 2026-03-26
