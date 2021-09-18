# Instalação de Wordpress com Docker e Ansible no Ubuntu

Será utilizado uma stack com Docker para subir os serviços necessários
## Preparação

Utilizar o comando para clonar o repositório

```bash
git clone https://www.git.com/alexeiev/wordpress-traefik.git
```

Dentro da raiz do repositório, execute o comando abaixo para a criação do arquivo **.env** onde serão guardadas as variáveis de ambiente para o nosso arquivo docker-compose.yaml utilizar.

Execute o comando no terminal para criar o arquivo
```bash
cat << EOF >> roles/deploy-app/files/.env
WORDPRESS_DB_HOST=db
WORDPRESS_DB_USER=wpadmin
WORDPRESS_DB_PASSWORD=wordpress
WORDPRESS_DB_NAME=wordpress

MYSQL_ROOT_PASSWORD=WordPressAdm1n
MYSQL_DATABASE=wordpress
MYSQL_USER=wpadmin
MYSQL_PASSWORD=wordpress
HOST=www
DOMINIO=DOMINIO.com.br
EMAIL=
EOF
```
> **OBS.: Modificar as variáveis de acordo com o seu projeto**


## Iremos falar sobre a estrutura do Playbook do Ansible.

Na raiz do projeto nós temos um arquivo de inventário chamado **hosts**
> **Adicionar o IP do servidor logo abaixo do grupo de hosts [node1]**

E teremos dentro do diretório roles, com duas roles para serem executadas.
Vamos descrever basicamente a utilização das duas.

* **Install-Docker**
  Dentro desta role teremos dois diretórios para entrar e editar arquivos
  * **vars**
    O arquivo de variável para esta role tem duas variáveis importantes, caso seja necessário modificar:
    1 - docker_url
    2 - compose_url

  * **tasks**
    Dentro do arquivo install-docker.yaml teremos a definição da tarefa:
    - Instalando o docker
    - instalando o docker-compose
    - Adicionar o usuário ubuntu no grupo compose
    - Restartar o serviço do docker
    - Habilitar o docker no systemD

* **Deploy-app**
  Dentro desta role teremos três diretórios para entrar e editar arquivos
  * **vars**
    O arquivo de variável para esta role tem apenas uma variável
    1 - work_dir

  * **tasks**
    Dentro do arquivo deploy-app.yaml teremos a definição da tarefa:
    - Criando diretório para deploy
    - Copiando files para deploy
    - Subindo o serviço com o docker-compose

  * **files**
    Dentro do diretório files nós teremos três arquivos:
    - .env
    - acme.json
    - docker-compose.yaml

    
Vamos executar nosso playbook.

```bash
ansible-playbook -i hosts main.yaml
```



## License
[MIT](https://choosealicense.com/licenses/mit/)


### Features

- [x] Utilizar docker-compose para rodar aplicação
- [x] Preparar ambiente com Ansible
- [ ] Criar servidor com Terraform

