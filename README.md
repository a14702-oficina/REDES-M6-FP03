# Sistema de Gestão de Infraestrutura de Rede Escolar (REDES-M6-FP03)

## Descrição do Projeto

Este projeto consiste num sistema web desenvolvido em PHP para a gestão de infraestruturas de rede em ambiente escolar. O objetivo principal é facilitar o registo e a monitorização de salas, equipamentos de rede (routers, switches, access points), técnicos responsáveis e as intervenções realizadas nesses equipamentos. O sistema foi concebido para ser uma ferramenta eficiente na organização e rastreabilidade das operações de manutenção e configuração da rede escolar.

## Funcionalidades

O sistema oferece as seguintes funcionalidades principais:

*   **Gestão de Salas:** Registo e consulta de informações sobre as salas onde os equipamentos estão instalados, incluindo nome e localização.
*   **Gestão de Equipamentos:** Cadastro e visualização de detalhes de equipamentos de rede, como tipo, marca, modelo, endereço IP e a sala onde estão alocados. Permite filtrar equipamentos por sala.
*   **Gestão de Técnicos:** Registo e consulta de técnicos responsáveis pelas intervenções, com informações como nome, email e contacto.
*   **Registo de Intervenções:** Adição e consulta de intervenções técnicas realizadas nos equipamentos, detalhando a data, descrição, equipamento envolvido e técnico responsável. Permite filtrar intervenções por equipamento ou técnico.
*   **Equipamentos sem Intervenções:** Relatório específico que lista todos os equipamentos que ainda não tiveram qualquer intervenção registada, útil para identificar potenciais negligências ou equipamentos novos.
*   **Autenticação de Utilizadores:** Sistema de login com controlo de acesso baseado em perfis (e.g., Administrador), garantindo que apenas utilizadores autorizados possam aceder e gerir certas funcionalidades.

## Estrutura da Base de Dados

A base de dados é composta por quatro tabelas principais, desenhadas para garantir a integridade referencial e a organização lógica dos dados [1].

### Tabelas e suas Relações

| Tabela        | Descrição                                                              | Chave Primária (PK) | Chaves Estrangeiras (FK)                                     |
| :------------ | :--------------------------------------------------------------------- | :------------------ | :----------------------------------------------------------- |
| `salas`       | Armazena informações sobre os locais físicos.                          | `id_sala`           | N/A                                                          |
| `equipamentos`| Detalhes dos dispositivos de rede.                                     | `id_equipamento`    | `id_sala` (referencia `salas.id_sala`)                       |
| `tecnicos`    | Dados dos profissionais responsáveis pelas intervenções.                | `id_tecnico`        | N/A                                                          |
| `intervencoes`| Registo de ações de manutenção/configuração.                           | `id_intervencao`    | `id_equipamento` (referencia `equipamentos.id_equipamento`), `id_tecnico` (referencia `tecnicos.id_tecnico`) |
| `utilizadores`| Informações dos utilizadores do sistema (para autenticação e auditoria). | `id`                | N/A                                                          |

As **Chaves Estrangeiras (FKs)** são cruciais para manter a **integridade referencial** da base de dados, garantindo que as relações entre as tabelas sejam consistentes e prevenindo a existência de dados órfãos [1].

### Exemplos de `JOIN`s Utilizados

O projeto faz uso extensivo de operações `JOIN` para combinar dados de múltiplas tabelas e apresentar informações completas. Dois exemplos notáveis incluem [1]:

*   **`LEFT JOIN` para listar equipamentos com as suas salas:** Utilizado para mostrar todos os equipamentos, mesmo aqueles que não têm uma sala associada, garantindo que nenhum equipamento seja omitido na listagem.
    ```sql
    SELECT e.id_equipamento, e.tipo, e.marca, e.modelo, e.endereco_ip, s.nome_sala 
    FROM equipamentos e 
    LEFT JOIN salas s ON e.id_sala = s.id_sala
    ```

*   **`INNER JOIN` para listar intervenções com detalhes de equipamento e técnico:** Combina informações de intervenções, equipamentos e técnicos para fornecer uma visão detalhada de cada ação realizada.
    ```sql
    SELECT i.id_intervencao, i.data_intervencao, i.descricao, e.marca, e.modelo, t.nome 
    FROM intervencoes i 
    JOIN equipamentos e ON i.id_equipamento = e.id_equipamento 
    JOIN tecnicos t ON i.id_tecnico = t.id_tecnico
    ```

## Requisitos

Para executar este projeto, é necessário ter um ambiente de servidor web configurado com os seguintes componentes:

*   **Servidor Web:** Apache, Nginx ou similar.
*   **PHP:** Versão 7.4 ou superior (com extensões `mysqli` ou `pdo_mysql` ativadas).
*   **Base de Dados:** MySQL ou MariaDB.

## Instalação

Siga os passos abaixo para configurar e executar o projeto:

1.  **Clone ou descarregue o projeto:**
    ```bash
    git clone <URL_DO_REPOSITORIO>
    # ou descarregue e descompacte o ficheiro ZIP
    ```

2.  **Configurar a Base de Dados:**
    *   Crie uma base de dados MySQL/MariaDB (e.g., `escola_rede`).
    *   Importe o ficheiro `bd.sql` para a sua base de dados. Este ficheiro criará as tabelas necessárias (`salas`, `equipamentos`, `tecnicos`, `intervencoes`) e a tabela `utilizadores` (necessária para a autenticação), além de popular com dados de exemplo.

    ```sql
    -- Exemplo de criação da tabela utilizadores (não presente no bd.sql original, mas inferida do código)
    CREATE TABLE IF NOT EXISTS utilizadores (
        id INT AUTO_INCREMENT PRIMARY KEY,
        username VARCHAR(255) NOT NULL UNIQUE,
        password VARCHAR(255) NOT NULL, -- Atenção: o código atual usa password em texto simples
        role VARCHAR(50) NOT NULL DEFAULT 'Utilizador'
    );

    -- Exemplo de inserção de utilizador para teste
    INSERT INTO utilizadores (username, password, role) VALUES 
    ('admin', 'admin123', 'Administrador'); -- Altere a password em produção!
    ```

3.  **Configurar a Ligação à Base de Dados:**
    *   Edite o ficheiro `config/ligacao.php` com as credenciais da sua base de dados:

    ```php
    <?php
    // ... (código existente)

    define('DB_HOST', 'localhost'); // Altere para o seu host
    define('DB_USER', 'seu_utilizador_bd'); // Altere para o seu utilizador da BD
    define('DB_PASS', 'sua_password_bd'); // Altere para a sua password da BD
    define('DB_NAME', 'escola_rede'); // Altere para o nome da sua BD

    // ... (código existente)
    ?>
    ```

4.  **Coloque os ficheiros no seu servidor web:**
    *   Copie a pasta `ficha1` (ou o conteúdo dela) para o diretório raiz do seu servidor web (e.g., `htdocs` para Apache, `www` para Nginx).

## Utilização

1.  **Aceda ao sistema:** Abra o seu navegador e navegue para o URL onde o projeto foi instalado (e.g., `http://localhost/ficha1/` ou `http://localhost/`).
2.  **Login:** Utilize as credenciais de teste:
    *   **Utilizador:** `admin`
    *   **Password:** `admin123`
3.  **Navegação:** Após o login, poderá navegar pelas diferentes secções do sistema para gerir salas, equipamentos, técnicos e intervenções.

## Notas de Segurança

É crucial notar que a implementação atual da autenticação (`autenticar.php`) compara a password fornecida pelo utilizador diretamente com a password armazenada na base de dados em texto simples (`$password === $user["password"]`). Esta prática **não é segura** e **não é recomendada** para ambientes de produção.

Para uma segurança adequada, as passwords devem ser armazenadas como *hashes* (e.g., usando `password_hash()` do PHP) e verificadas com `password_verify()`. O ficheiro `gerar_hash.php` no projeto demonstra como gerar hashes de passwords, mas esta funcionalidade não está integrada no fluxo de autenticação principal.

**Recomendação:** Implementar o uso de `password_hash()` e `password_verify()` para proteger as credenciais dos utilizadores.

## Referências

[1] `explicacao.txt` - Ficheiro de explicação do projeto incluído no arquivo original.
