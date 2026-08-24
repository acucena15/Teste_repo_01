# Relatório de Arquitetura e Modelagem — Rota Escolar

Este documento apresenta a estrutura técnica, a matriz de permissões (RBAC), as regras de negócio e os diagramas de modelagem do sistema **Rota Escolar**.

---

## 1. Matriz de Permissões (RBAC)

| Perfil | Cadastrar Alunos/Motoristas | Criar Rotas e Pontos | Visualizar Rota e Alunos | Registrar Embarque |
| :--- | :---: | :---: | :---: | :---: |
| **Escola** | ✅ | ✅ | ✅ | ❌ |
| **Motorista** | ❌ | ❌ | ✅ | ✅ |

---

## 2. Regras de Negócio e Segurança

* **Atribuição de Rota:** Cada motorista é associado a uma única rota por vez.
* **Vínculo do Aluno:** O aluno deve estar vinculado obrigatoriamente a uma Rota e a um Ponto de Embarque específico.
* **Restrição de Embarque:** Apenas o perfil Motorista possui permissão para registrar e salvar a presença/embarque dos alunos.
* **Simplicidade do Escopo:** O sistema foca estritamente na gestão do transporte escolar, sem integração de GPS ou sistemas externos.

---

## 3. Fluxograma do Processo de Embarque (Motorista)

```mermaid
graph TD
    A[Inicio: Login do Motorista] --> B[Acessar Menu Principal]
    B --> C[Consultar Minha Rota]
    C --> D[Visualizar Pontos de Embarque]
    D --> E[Selecionar Ponto Atual]
    E --> F[Visualizar Lista de Alunos]
    F --> G{Aluno Embarcou?}
    G -- Sim --> H[Marcar como Embarcou]
    G -- Nao --> I[Marcar como Nao Embarcou]
    H --> J[Salvar Registro de Embarque]
    I --> J

erDiagram
    MOTORISTA ||--o| ROTA : conduz
    ROTA ||--|{ PONTO_EMBARQUE : possui
    ROTA ||--|{ ALUNO : atende
    PONTO_EMBARQUE ||--|{ ALUNO : embarca
    ALUNO ||--o{ REGISTRO_EMBARQUE : possui

    MOTORISTA {
        int id_motorista PK
        string nome
        string telefone
    }

    ROTA {
        int id_rota PK
        string nome_rota
        int id_motorista FK
    }

    PONTO_EMBARQUE {
        int id_ponto PK
        string nome_ponto
        int ordem
        int id_rota FK
    }

    ALUNO {
        int id_aluno PK
        string nome
        date data_nascimento
        int id_rota FK
        int id_ponto FK
    }

    REGISTRO_EMBARQUE {
        int id_registro PK
        int id_aluno FK
        date data_registro
        string status_embarque
    }
