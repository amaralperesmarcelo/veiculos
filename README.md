# Sistema Multi-Tenant para Múltiplas Concessionárias

Este repositório é uma demonstração (clone portfólio) do nosso ecossistema de software desenvolvido para gerenciar e exibir estoques de múltiplas concessionárias de veículos.

Neste projeto, desenvolvemos uma arquitetura **multi-tenant**, onde um **único backend e um único banco de dados** servem dezenas de sites frontend independentes. Cada concessionária tem sua própria identidade visual, domínio e leads, mas toda a infraestrutura por trás é unificada, garantindo facilidade de manutenção e escalabilidade.

*(Nota: Os dados, logos, endereços e telefones das lojas foram substituídos por dados fictícios para preservar a privacidade dos clientes reais e demonstrar o funcionamento do sistema).*

## 🏗 Arquitetura do Sistema

O sistema é dividido em duas partes principais:

### 1. Backend Centralizado (FastAPI)
- **Única Fonte da Verdade:** Um backend construído em Python com **FastAPI** gerencia todas as requisições de todos os sites.
- **Isolamento de Dados (Multi-Tenant):** O backend identifica a qual concessionária a requisição pertence (através de um `tenant_id` ou domínio) e filtra os dados (carros, leads, configurações) para retornar apenas o que pertence àquela loja específica.
- **Alta Performance:** O uso do FastAPI garante que o sistema seja assíncrono e extremamente rápido, capaz de suportar múltiplos sites simultaneamente.
- **Banco de Dados Único:** Utilizamos um banco de dados relacional unificado, onde tabelas como `veiculos` e `leads` possuem uma coluna `tenant_id` para separação lógica dos dados.

### 2. Frontend Descentralizado (Next.js & React)
- **Vários Frontends, Um Padrão:** Cada concessionária ganha um site em **Next.js** (React) com Server-Side Rendering (SSR) e Static Site Generation (SSG) para SEO perfeito.
- **Identidade Visual Única:** Apesar de consumirem a mesma API, cada frontend tem seu próprio `tailwind.config`, cores (ex: Dark/Gold para lojas premium, cores vibrantes para lojas populares) e componentes customizados.
- **Geração de Leads Direta:** O sistema envia os leads diretamente para o WhatsApp do vendedor da loja e também salva no painel de administração daquela concessionária.

## 🚀 Tecnologias Utilizadas

### Backend
- **Python 3**
- **FastAPI**: Framework web moderno e de alta performance.
- **SQLAlchemy**: ORM para manipulação do banco de dados.
- **PostgreSQL / SQLite**: Banco de dados relacional.
- **Pydantic**: Para validação e serialização de dados (Schemas).

### Frontend
- **Next.js (App Router)**: Framework React para renderização híbrida e SEO.
- **TypeScript**: Para tipagem estática e segurança do código.
- **Tailwind CSS**: Estilização rápida e criação de temas customizados por loja.
- **React Icons & Lucide**: Bibliotecas de ícones.
- **Integração WhatsApp**: Redirecionamento dinâmico de leads para o aplicativo.

## 💡 Como a Mágica Acontece (Multi-Tenancy)

Quando um usuário acessa `lojaA.com.br`, o frontend da Loja A faz uma requisição para a API central (ex: `/api/cars?tenant_id=loja-a`). O backend consulta o banco de dados filtrando apenas os carros onde `tenant_id == 'loja-a'` e os devolve. O mesmo acontece para a `lojaB.com.br`. 

Isso nos permitiu:
1. Atualizar regras de negócio em um único lugar (Backend).
2. Adicionar novas concessionárias em minutos apenas subindo um novo frontend e cadastrando um novo `tenant_id`.
3. Manter custos de infraestrutura baixíssimos comparado a rodar uma API para cada cliente.

## 📸 Demonstrações (Links)

Aqui estão exemplos de frontends conectados ao nosso ecossistema (com dados falsos para demonstração):

- **Concessionária 1:** [Link do deploy] - *Exemplo de tema Dark/Gold Premium.*
- **Concessionária 2:** [Link do deploy] - *Exemplo de tema com cores fortes e modernas.*

---
*Este repositório foi criado exclusivamente para fins de portfólio. Códigos sensíveis, chaves de API e dados reais de clientes não estão presentes aqui.*
