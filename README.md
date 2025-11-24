# Projeto-Final-SOLID: Controle de Estacionamento Inteligente

Este projeto é um sistema simples de controle de estacionamento, feito utilizando **PHP 8+**, **Composer** e **SQLite**.

## Nome - RA
Ana Karla de Souza Moretão - 1986881
Allan França Padovan - 1986140
Lucas Gimenez - 1996567

## Tecnologias e Pré-requisitos

* **PHP:** Versão 8.2 ou superior.
* **Composer:** Com autoload PSR-4.
* **Aplicação:**  PSR-12.

* **XAMPP / Servidor Web Local:** Para servir a aplicação via `http://localhost/controle-estacionamento/public/`.

## Estrutura do Projeto (PSR-4 e Camadas)

A organização segue o padrão de camadas e o autoloading `App\` via PSR-4:
```
CONTROLE-ESTACIONAMENTO/
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
```

  ## Instalação e Execução

1.  **Clone ou Crie o Projeto:**
    Coloque a pasta `nomedapasta` na raiz do seu servidor web (Ex: pasta `htdocs` no XAMPP).

2.  **Instale as Dependências (Composer):**
    Abra o terminal na pasta raiz do projeto (`controle-estacionamento/`) e execute:
    ```
    composer install
    ```

3.  **Acesse a Aplicação:**
    A aplicação deve ser acessível via a pasta `public/`:
    ```
    http://localhost/controle-estacionamento/public/
    ```
