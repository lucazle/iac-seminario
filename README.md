# IaC Seminário: Terraform + Docker

Demonstração prática de **Infraestrutura como Código** para a disciplina de Administração Avançada em Sistemas Operacionais (IFPE Igarassu).

O arquivo `main.tf` descreve um servidor web (um container nginx). O Terraform lê essa descrição e cria o servidor automaticamente, sempre com o mesmo resultado.

## O que tem aqui

- `main.tf`: a infraestrutura descrita em código (imagem e container do nginx)
- `.terraform.lock.hcl`: trava a versão do plugin do Docker, para o resultado ser reprodutível
- `.devcontainer/devcontainer.json`: descreve o ambiente do Codespace (Ubuntu 24.04, Docker e Terraform já instalados)
- `.gitignore`: impede que o estado local do Terraform (`tfstate`) seja versionado

## Como rodar

### No GitHub Codespaces (recomendado)

1. Clique em **Code → Codespaces → Create codespace on main**
2. Espere o ambiente montar
3. No terminal, siga o fluxo da demo abaixo

### Localmente

Pré-requisitos: [Docker](https://docs.docker.com/get-docker/) e [Terraform](https://developer.hashicorp.com/terraform/install) instalados.

## Fluxo da demo

```bash
terraform init      # baixa o plugin do Docker
terraform plan      # mostra o que será criado, sem criar nada
terraform apply     # cria o container (digite yes)
curl localhost:8081 # deve responder "Welcome to nginx!"
```

Para mostrar que o Terraform ajusta só o que mudou: troque a porta `external` no `main.tf`, rode `terraform plan` e `terraform apply` de novo.

Para desfazer tudo:

```bash
terraform destroy   # remove o container (digite yes)
```

Rodar `terraform apply` outra vez recria o servidor idêntico.

## Observações

- O `terraform.tfstate` não é versionado: ele guarda o estado da infraestrutura e pode conter dados sensíveis.
- Se alguém remover o container por fora do Terraform (`docker rm -f servidor-web`), o `terraform plan` detecta a diferença (*drift*) e o `apply` corrige.
