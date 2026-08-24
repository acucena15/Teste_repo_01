# Relatório de Arquitetura e Modelagem - CTBJ Conecta

Este documento detalha a rastreabilidade do projeto, as regras de negócio, a estrutura de permissões (RBAC) e os diagramas de modelagem do sistema CTBJ Conecta.

---

## 1. Rastreabilidade e Gestão Ágil

* **Quadro Kanban no Trello:** https://trello.com/invite/b/Y9bM6m6r/ATTI6b2d572005ceec848f55124a1647f62bF0EF9FF9/pi-ctbj-conecta-
* **Documentação Interativa no Notion:** https://absorbed-currency-75a.notion.site/PI2-CTBJ-Conecta-Reserva-de-Salas-e-Recursos-3c20b479d9ef8010af85feef5c614b82?source=copy_link

---

## 2. Matriz de Permissões (RBAC)

| Papel | Reservar Equipamento | Reservar Espaço | Aprovar Reserva | Cadastrar Recursos |
| :--- | :---: | :---: | :---: | :---: |
| **Aluno** | ✅ | ❌ | ❌ | ❌ |
| **Professor** | ✅ | ✅ | ❌ | ❌ |
| **Coordenador** | ✅ | ✅ | ✅ | ❌ |
| **Diretor / Administrador** | ✅ | ✅ | ✅ | ✅ |

---

## 3. Regras de Negócio e Segurança

* **Solicitação de Reservas:** Alunos só podem solicitar equipamentos. Professores e Coordenadores podem solicitar espaços físicos (salas/laboratórios).
* **Aprovação Obrigatória:** Nenhuma reserva é confirmada automaticamente; todas passam por validação do Coordenador ou Diretor responsável.
* **Política de Conflitos:** O sistema não permite agendamentos duplos para o mesmo recurso no mesmo horário.
* **Restrição do Diretor:** Apenas o Diretor/Administrador possui acesso total para cadastrar e alterar a estrutura do sistema.

---

## 4. Fluxograma do Processo de Reserva

```mermaid
graph TD
    A[Inicio: Usuario solicita reserva] --> B{Possui permissao?}
    B -- Nao --> C[Exibir mensagem de erro]
    B -- Sim --> D{Recurso disponivel?}
    D -- Nao --> E[Notificar indisponibilidade]
    D -- Sim --> F[Encaminhar para aprovacao]
    F --> G{Aprovado pelo Coordenador?}
    G -- Nao --> H[Reserva Recusada]
    G -- Sim --> I[Reserva Confirmada]
erDiagram
    USUARIO ||--o{ RESERVA : faz
    RECURSO ||--o{ RESERVA : contem

    USUARIO {
        int id_usuario PK
        string nome
        string email
        string papel
    }

    RECURSO {
        int id_recurso PK
        string nome
        string tipo
        string status
    }

    RESERVA {
        int id_reserva PK
        int id_usuario FK
        int id_recurso FK
        datetime data_inicio
        datetime data_fim
        string status
    }
