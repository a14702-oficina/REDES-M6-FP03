# Sistema de Gestão de Infraestrutura de Rede Escolar (REDES-M6-FP03)

Este projeto consiste num sistema de gestão para infraestruturas de rede escolar, desenvolvido como parte do módulo M6 da disciplina de Redes. O sistema permite gerir salas, equipamentos, técnicos e intervenções técnicas.

## 🚀 Funcionalidades

- **Gestão de Salas:** Registo e organização dos locais físicos.
- **Gestão de Equipamentos:** Inventário de routers, switches, access points, etc.
- **Gestão de Técnicos:** Registo dos profissionais responsáveis pela manutenção.
- **Registo de Intervenções:** Histórico detalhado de todas as ações técnicas realizadas.
- **Relatórios:** Visualização de equipamentos sem intervenções e listagens detalhadas com associações entre tabelas.

## 🛠️ Tecnologias Utilizadas

- **PHP:** Lógica do lado do servidor.
- **MySQL:** Base de dados relacional.
- **CSS:** Estilização da interface.
- **SQL:** Consultas complexas com JOINs e integridade referencial.

## 📂 Estrutura do Projeto

- `/ficha1`: Contém o código fonte da aplicação.
  - `/config`: Configurações de ligação à base de dados.
  - `/pages`: Páginas específicas de gestão e listagem.
  - `/relatorio`: Documentação e evidências do sistema.
- `bd.sql`: Script de criação da base de dados e tabelas.

## ⚙️ Como Configurar

1. Importe o ficheiro `bd.sql` para o seu servidor MySQL.
2. Configure as credenciais de acesso em `ficha1/config/ligacao.php`.
3. Coloque os ficheiros num servidor web com suporte para PHP (ex: XAMPP, WAMP).
4. Aceda ao `index.php` para iniciar a aplicação.

---
*Projeto desenvolvido para fins académicos.*
