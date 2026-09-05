# Segurança e sanitização do repositório público

Este repositório representa uma **edição de portfólio** do Nexus.

O projeto original nasceu em um contexto operacional real. Por isso, a publicação segue a regra de que nenhuma informação necessária apenas ao ambiente interno deve ser exposta aqui.

## Nunca publicar

- arquivos `.env` reais;
- senhas, tokens, cookies ou sessões autenticadas;
- chaves privadas ou certificados;
- URLs e endpoints internos;
- endereços de banco de dados;
- dumps de banco;
- arquivos SQLite de operação;
- dados pessoais;
- planilhas operacionais reais;
- capturas de tela contendo nomes, identificadores ou informações privadas;
- nomes de clientes, campanhas, pessoas ou entidades quando não forem necessários ao case;
- regras internas que revelem detalhes proprietários do processo;
- logs e relatórios reais de execução.

## O que pode ser publicado

- documentação de arquitetura sanitizada;
- diagramas genéricos;
- descrição das tecnologias utilizadas;
- decisões de engenharia;
- padrões de validação e segurança;
- exemplos fictícios;
- testes e módulos demonstrativos criados especificamente para portfólio;
- screenshots previamente revisadas e anonimizadas.

## Variáveis de ambiente

Quando futuramente houver código demonstrativo neste repositório, qualquer configuração deve usar apenas placeholders, por exemplo:

```env
APP_ENV=development
DB_HOST=
DB_PORT=3306
DB_NAME=
DB_USER=
DB_PASSWORD=
```

Nenhum valor real deve fazer parte do histórico Git.

## Revisão antes de cada publicação

Antes de enviar novos arquivos para este repositório:

1. verificar nomes de arquivos e conteúdo textual;
2. procurar credenciais, tokens, e-mails, URLs e caminhos locais;
3. revisar screenshots e documentos;
4. substituir dados reais por exemplos fictícios;
5. confirmar que o material demonstra a solução sem revelar o ambiente operacional.

## Código operacional

O código operacional completo do Nexus não é publicado automaticamente neste repositório.

Partes do código poderão ser adaptadas futuramente para uma versão demonstrativa, desde que sejam independentes do ambiente interno e passem por sanitização antes da publicação.
