# Site na Nuvem (AWS)

## 1° Etapa ( Resolver o problema )

<p> Inicialmente é necessário analisar qual o tipo de problema a empresa precisa resolver<br>
criei uma situação fictícia para poder resolver de forma rápida e prática através da AWS</p><br>
<h2>Problema: </h2><br>
    <code> Uma empresa chamada VFX utilizava um software de desenvolvimento próprio<br>
         que estava apresentando lentidão e quedas constantes e isso ocasionava um<br>
         grande impacto nos resultados do negócio. <code>
    
<details>
<summary><h2> Problemas Técnicos</h2></summary>
- Estava junto com outras VM´s
- Quedas no sistema
- Apresentava problemas de hardware
- Precisava trocar o servidor já que tinha 5 anos
- Usuários reclamando de lentidão
- Backup ineficiente e perda de dados
- Custos altos de manutenção e contratação
</details>

<details>
<summary><h2> Impactos no negócio</h2></summary>
- Equipe sem poder trabalhar
- Clientes insatisfeitos por motivo da demora
- Os usuários culpam o sistema
- Perda de clientes
- Atrasos na logística
- Perdas financeiras
- Produtividade da equipe
</details>

<details>
<summary><h2> Impacto para infra</h2></summary>
- Muitos recursos para gerenciar
- Falta de equipamentos de qualidade
- Incidentes fora do horário (dor de cabeça)
- Viver apagando incêndio e sem tempo para capacitação
- Stress constante e quase sem vida social
- Só recebem críticas e não se sentem valorizados
- Desmotivado com a profissão
</details>

<details>
<summary><h2> Impacto para Devs</h2></summary>
- Necessidade de gerenciar infra
- Deficuldade no deploy
- Muitos projetos inviabilizados
- Muito suporte
- Dificuldade de manter a alta disponibilidade
- Problemas para criar ambientes de homologação
</details>
    
    
## Detalhes da instância AWS (Amazon Web Services)
Inicialmente foi necessário criar uma instância EC2 do tipo Ubuntu server Versão 20.04LTS<br>
abaixo terá a tabela completa, lembre de adicionar um ip públixo a instância<br>
    
Tipo da Instância | Tipo de Chave | Config. de Rede | Armazenamento
---|---|---|---
T2 Micro | .pem | 0.0.0.0/0 | 8GiB (Gp2)
Family | .ppk | Seu_IP |  
    
### Portas da instância (entradas e saídas)

TCP | UDP | NFS | HTTP | SSH | HTTPS
---|---|---|---|---|---
8080 | 2049 | 2049 | 80 | 22 | 443
111 | 111 | 
443 | 
80 |
    
    
<a href="https://cdn-icons-png.flaticon.com/512/5610/5610989.png" target="_blank"><img height="20" width="20" src="https://cdn-icons-png.flaticon.com/512/5610/5610989.png" target="_blank"></a>  Observação: A configuração de rede está com 0.0.0.0/0 para facilitar o acesso mas<br>
não é uma boa prática utilizar essa cong. de rede pois pode deixar a instância vulnerável<br>
estou utilizando porquê é um ambiente apenas para testes, nada de confidêncial está em risco<br>
a instância foi criada apenas para fins práticos.
</div><br>
    
# Passo a passo para a execução do projeto
    
Após a configuração da instãncia EC2 na opção de <strong>Dados do Usuário</strong><br>
Será adicionado o seguinte código bash para adiantar algumas coisas<br>
    
    
    #!/bin/bash
    sudo apt-get update -y
    sudo apt-get install apache2 php7.4 libapache2-mod-php7.4 php7.4-common php7.4-curl php7.4-intl php7.4-mbstring       php7.4-json php7.4-xmlrpc php7.4-soap php7.4-mysql php7.4-gd php7.4-xml php7.4-cli php7.4-zip wget mysql-client       unzip git binutils ruby -y
    sudo systemctl start apache2
    sudo systemctl enable apache2
    sudo systemctl restart apache2
    wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
    chmod +x ./install
    sudo ./install auto
    sudo service codedeploy-agent start
    sudo chmod 777 /etc/init.d/codedeploy-agent
    sudo wget https://s3.sa-east-1.amazonaws.com/pages.cloudtreinamentos.com/aws/bootcamp/Bootcamp9.zip 
    sudo unzip -o Bootcamp9.zip -d /var/www/html/
    sudo rm /var/www/html/index.html
    sudo chmod -R 777 /var/www/html
    
Após isso pode executar a instância, assim que as verificações estarem completas entre no<br>
navegador e coloque o ip público da instãncia em execução para ver o site que foi adicionao<br>
na máquina no ar.
    
    
# 2° Etapa ( Disponiblidade )

<h2> A partir daqui será configurado o banco de dados, de maneira geral é bem simples<br>
após a 1° etapa está pronta vamos configurar e deixar no ar o banco de dados criado<br>
através do RDS na AWS.</h2>
    
    
## Detalhes do Banco de Dados RDS
    
.|.|.|.
---|---|---|---
Tipo de Banco de Dados | Nome do BD | Tipo de criação | Modelo de BD
Relacional | banco | Padrão | MySQL Community
    
    
<h3> Ao selecionar a opção <strong>Criar Banco de dados</strong> você irá selecionar<br>
    as seguintes opções: </h3>

- Criação Padrão
- MySQL ( Edição MySQL Community )
- Versão 8.0.32
- Modelos ( Nível Gratuito )
- identificados ( ou deixa o padrão ou coloque o nome que preferir )
- Nome do usuário (admin ou de sua escolha)
- Senha principal ( Recomendo utilizar uma senha numérica apenas pois a aplicação está com bug )
- Tipo de Armazenamneto ( SSD )
- Escolher qual a subnet a ser usada ( OBS: é necessário ter subnets em 2 AZ´s )
- Acesso público ( Não )
- Grupo de VPC ( Pode ser Default )
- Autenticação de banco de dados ( Autenticação de senha )
- Criar Banco de Dados

OBS: Após criar o Banco de Dados espera de 3 a 5 minutos, demora um pouco.
    
    
# 3° Etapa ( Criar um S3 )
    
<h2> A configuração do Bucket S3 será a Seguinte: </h2>

- Escolher o serviço S3
- Escolher um nome para o bucket
- Escolher a região do bucket ( pode ser o norte da virgínia mesmo )
- Propriedades do Objeto (Acls Habilitadas)
- Configurações de bloqueio ( Desmarca a seleção de bloqueio para ficar público )
- Confirma que reconhece as configurações atuais
- Ativa o versionamento
- Criptografia ( padrão )
- Configurações avançadas - Bloqueio de objeto ( Desativar )
- Criar bucket

## É necessário algumas configurações de segurança dentro do bucket:
 
- Propriedades ( Habilitar Hospedagem de site estático )
- Documento de índice ( index.html )
- Documento de erro ( index.html )
- Permissões
- política de acesso ( editar )
- gerador de políticas
    - Tipo de política ( S3 bucket policy )
    - Efeito ( Allow ou permitir )
    - principal ( * )
    - Actions ( GetObject )
    - e adicione o link do ARN do seu bucket criado + /*
      Exemplo: linkDoArn/*
    - Gerar política
    - Copia a política gerada e cola lá onde estava a opção de editar política do bucket S3
    - Salvar Alterações
    
## Agora será necessário criar um usuário no IAM para ter acesso ao bucket S3:
    
- Entre no IAM da AWS
- Entre na opção de <strong>Usuários</strong>
- Adicionar Usuários
- Escolha o nome do Usuário
- Anexar política diretamente
- Escolha a política <strong>AmazonS3FullAccess</strong>
- Próximo
- Pode criar o usuário

## Agora é necessário criar um Access Key e uma Secret key para esse usuário

- Clica no usuário criado
- Clica em credenciais de segurança
- Escolha a opção "Aplicação em execução em um serviço computacional da AWS"
- Marca a opção que compreende a recomendação acima
- Crie um nome para a chave (por exemplo "ec2s3" vai ser utilizado aqui")
- Criar chave de acesso

### Agora lá no site que ficou no ar pode preencher com as informações que se pede,<br>
vou preencher de acordo com que fiz no meu:
    
.|.|.|.
---|---|---|---
Bucket | Região | AccessKey | SecretKey
first-bucket-s3-test | us-east-1 | Sua AccessKey | Sua SecretKey

# Após o sucesso, pode enviar uma logo através do botão

# Agora vamos criar uma AMI

- Entra na EC2
- Entra na opção Instâncias
- Seleciona a instância a ser copiada
- Ações
- Imagens e Modelos
- Criar Imagem

# Após isso preencha o que se pede

- Escolha o nome da imagem ( aqui vai ser "app_funcionando-web" )
- Deixa a opção não reinicializar desmarcada
- Criar imagem
- A partir dai será criada um modelo para criar novas instâncias a partir dessa imagem

# Verficcar todos os serviços
    
 - Agora é necessários se todos os serviçoes estão funcionando corretamente
 - Verificar bugs e erros que podem estar acontecendo
