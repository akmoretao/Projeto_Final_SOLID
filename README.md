# Projeto-Final-SOLID: Controle de Estacionamento Inteligente

Este projeto é um sistema simples de controle de estacionamento, feito utilizando **PHP 8+**, **Composer** e **SQLite**.

## Nome - RA
Ana Karla de Souza Moretão - 1986881
Allan França Padovan - 1986140
Lucas Gimenez - 1996567

## Tecnologias e Pré-requisitos

* **PHP:** Versão 8.2 ou superior.
* **Composer:** Com autoload PSR-4.
* **XAMPP / Servidor Web Local:** Para servir a aplicação via `http://localhost/nomedapasta/public/`.

## Estrutura do Projeto (PSR-4 e Camadas)

A organização segue o padrão de camadas e o autoloading `App\` via PSR-4:
products-srp-demo/
├─ composer.json
├─ vendor/
├─ src/
│ ├─ Contracts/           # Interfaces (Contratos do Domínio)
│ │ ├─ ProductRepository.php
│ │ └─ ProductValidator.php
│ ├─ Application/         # Camada de Orquestração (Regras de Uso)
│ │ └─ ProductService.php  # Orquestra Validação e Persistência
│ ├─ Domain/              # Camada de Regras de Negócio
│ │ └─ SimpleProductValidator.php # Implementação da Validação
│ └─ Infra/               # Camada de Infraestrutura (Detalhes técnicos)
│   └─ FileProductRepository.php # Persistência em arquivo (único que toca no storage)
├─ public/                # Camada de Apresentação (I/O e HTTP)
│ ├─ index.php            # Formulário de Cadastro (POST para create.php)
│ ├─ create.php           # Processa POST, chama o Service, lida com HTTP status
│ └─ products.php         # Lista produtos (Somente leitura)
└─ storage/
  └─ products.txt       # Arquivo de persistência (JSON por linha)

  ## Instalação e Execução

1.  **Clone ou Crie o Projeto:**
    Coloque a pasta `products-srp-demo` na raiz do seu servidor web (Ex: `htdocs` no XAMPP).

2.  **Instale as Dependências (Composer):**
    Abra o terminal na pasta raiz do projeto (`products-srp-demo/`) e execute:
    ```
    composer install
    ```

3.  **Acesse a Aplicação:**
    A aplicação deve ser acessível via a pasta `public/`:
    ```
    http://localhost/products-srp-demo/public/
    ```

## Casos de Teste (Manuais)

Para garantir que o SRP e as regras de negócio foram aplicados corretamente, siga os passos abaixo:

| Caso de Teste | Entrada (index.php) | Fluxo Esperado | Resultado Esperado |
| :---: | :---: | :---: | :---: |
| **1. Cadastro Válido** | Nome: `Teclado Mecânico`, Preço: `120.50` | 1. `create.php` chama `Validator`. 2. `Validator` retorna `null`. 3. `Service` chama `Repo->save()`. 4. Redireciona para `products.php`. | **HTTP 201** e o produto aparece na listagem (`products.php`) com um ID (`id: 1`). |
| **2. Nome Curto (Inválido)** | Nome: `T`, Preço: `50` | 1. `create.php` chama `Validator`. 2. `Validator` retorna `errors` (`name < 2`). 3. Redireciona para `index.php`. | **HTTP 422** e a página `index.php` (cadastro) deve indicar a falha na validação. |
| **3. Preço Negativo (Inválido)** | Nome: `Mouse`, Preço: `-10` | 1. `create.php` chama `Validator`. 2. `Validator` retorna `errors` (`price < 0`). 3. Redireciona para `index.php`. | **HTTP 422** e a página `index.php` (cadastro) deve indicar a falha na validação. |
| **4. Listagem Vazia** | Arquivo `storage/products.txt` está vazio. | `products.php` chama `Repo->findAll()`. | Página de listagem exibe: "Nenhum produto cadastrado." |
| **5. Múltiplos Cadastros** | Cadastrar 3 itens válidos. | `Repo->getLastId()` deve garantir IDs sequenciais. | Listagem mostra 3 itens com IDs `1`, `2`, `3` (ou incrementais se já houverem dados). |

## Critérios de Aceite Confirmados

| Critério | Status | Observações |
| :---: | :---: | :---: |
| **Acesso via `public/`** | ✅ OK | A aplicação roda no diretório `http://localhost/products-srp-demo/public/`. |
| **SRP: Service** | ✅ OK | `ProductService` apenas orquestra, não contém `echo`, I/O, ou regras de validação. |
| **SRP: Repository** | ✅ OK | `FileProductRepository` é o único a ler/escrever em `storage/products.txt`. |
| **SRP: Validator** | ✅ OK | `SimpleProductValidator` apenas implementa as regras de negócio de validação. |
| **PSR-4 e Camadas** | ✅ OK | O projeto segue rigorosamente a estrutura `App\Application`, `App\Domain`, `App\Infra`. |
| **Persistência** | ✅ OK | O arquivo `products.txt` armazena um JSON por linha. |
