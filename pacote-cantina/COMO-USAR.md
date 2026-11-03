# Como usar o pacote

Ordem de aplicação (cada passo gera um commit):

| Passo | Conteúdo | Arquivo |
|---|---|---|
| 0 | Base incompleta (pom, wrapper, app, schema.sql, README curto) | `00-base/` |
| 1 | Produtos (com validação duplicada em `cadastrar()`/`atualizar()`) | `01-produtos.patch` |
| 2 | Clientes e algoritmos | `02-clientes-algoritmos.patch` |
| 3 | Pedidos sem cancelamento | `03-pedidos.patch` |
| 4 | Cancelamento, extração de `validarDadosProduto()`, diagrama e README final | `04-cancelamento-refatoracao.patch` |

## Commit inicial

```bash
cp -r pacote-cantina/00-base/. <seu-repositorio>/
cd <seu-repositorio>
git add -A
git commit -m "Estrutura inicial do projeto"
```

## Patches

Os patches não têm autor (gerados com `git diff --binary`), então o commit fica com o autor de quem aplica.
Rode a partir da raiz do repositório:

```bash
git apply --check ../pacote-cantina/01-produtos.patch
git apply ../pacote-cantina/01-produtos.patch
git add -A && git commit -m "CRUD de produtos"

git apply ../pacote-cantina/02-clientes-algoritmos.patch
git add -A && git commit -m "Clientes e algoritmos de busca/ordenação"

git apply ../pacote-cantina/03-pedidos.patch
git add -A && git commit -m "Pedidos"

git apply ../pacote-cantina/04-cancelamento-refatoracao.patch
git add -A && git commit -m "Cancelamento de pedidos e refatoração do ProdutoService"
```

## Validação após cada passo

```bash
./mvnw compile
./mvnw test -Dtest='!CantinaApplicationTests'
```

`CantinaApplicationTests` (contextLoads) exige banco PostgreSQL configurado.

Observação: o projeto original não tem `.mvn/wrapper/maven-wrapper.properties`, portanto `./mvnw` falha
("cannot open ./.mvn/wrapper/maven-wrapper.properties"). Use `mvn` instalado ou gere o wrapper com
`mvn wrapper:wrapper` antes de usar `./mvnw`. Também não há `.gitignore`: não commite a pasta `target/`.
