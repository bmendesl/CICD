## CICD

Repositorio base

https://github.com/jrballot/CICD

http://dontpad.com/524-4linux

https://github.com/jrballot/CICD

https://app.vagrantup.com/boxes/search

Vagrantfile do curso:
https://raw.githubusercontent.com/jrballot/CICD/master/Vagrantfile

Gitlab Comparado a outras ferramentas:
https://about.gitlab.com/devops-tools/

Jenkins Pipeline Syntax:
https://jenkins.io/doc/book/pipeline/syntax/


Projeto Exemplo Java+SpringBoot:
https://github.com/jrballot/hw-springboot.git


### GIT

```

Instalando

> yum install git -y -q

-----------------------------------------------------------------
mkdir git 

Criar repo local

> git init

-----------------------------------------------------------------
Informando/Configurando user

> git config --global user.name "Bruno Mendes"
> git config --global user.email bml@4linux.com

-- Git congif e a chave para utilizar todas as infos.
	hierarquias : System(a maquina que estou rodando) > global(ao repo) > local(associado ao user)

-----------------------------------------------------------------
Mostrar as configuracoes que realizei:

> git config --list

* Bare eh um gitlab da vida sem gerenciador, mas voce eh o gerenciador da porra toda.

-----------------------------------------------------------------
Trocar o editor default:

> git config --global core.editor vim
-----------------------------------------------------------------
> touch arq00
> git status - status do commit e dos arquivos
> git add arq00
> git status 
> git rm --cached arq00
> > git add arq00
> git commit -m "initial"
> git log
> git diff -- mostra a diferenca
> git commit -a -m "texto added"
> git blame arq00 - culpado da alteracao		
-----------------------------------------------------------------
Git GC

> Limpa arquivos nao necessarios no repo local.
https://git-scm.com/docs/git-gc
-----------------------------------------------------------------
Desfazendo as merdas:

*Checkout
> echo CICD >> arq00
> git checkout arq00 -- (limpei minha alteracao no meu arquivo que acabei de usar, ele volta o estado do commit.)

*Reset
> echo CICD >> arq00
> git comit -a -m "nome do curso adicionado"
> git log
> git log --color --oneline --decorate
> git reset HEAD~1 --- (Estou solicitando para voltar o commit anterior e removo o atual, mas nao removo do meu arquivo)

*Revert - revert as alteracoes de um commit realizado em outro commit -- preciso informar qual commit estou revertendo

> git commit -a -m "voltando curso CICD para arq00"
> git revert e742348
> git revert --continue
> git add arq00
> git revert --continue
> git log
-----------------------------------------------------------------
## Branch - ramificação da arvore principal, copia de uma branch que ja estou.

> git branch nova_branch -- criei nova branch
> git branch
> git checkout nova_branch -- mudei para minha branch
> git checkout -b  nova_nova_branch - crio e ja mudo para minha branch
> git branch -d/-D nova_nova_branch -- D maiusculo forca a remocao e d minusculo so deixa se tiver feito merge 
-----------------------------------------------------------------
## Merge - mesclar as coisas de uma branch com outra

> git merge nova_branch

Fast-forward - movimenta o HEAD referenciando para a branch nova, para economia dos recursos.
-----------------------------------------------------------------
## Rebase - deixa o commit alinhado.

> git rebase <branch>
-----------------------------------------------------------------
## cherry-pick - permite pegar um commit especifico, e cria uma nova estrutura.

> git cherry-pick <branch hash>
-----------------------------------------------------------------
## Git flow - forma de gerenciar meu repo, faco uma alteracao imediata e depois posso usar o cherry-pick para atualizar as outras branchs.

-----------------------------------------------------------------
## tag

> git tag -a 1.0 -m "Versao 1.0" --
> git tag -a 1.1 -m "versao 1.1" <hash do commit> -- tag anotada, caso nao especificar o commit ele pega o atual
> git show <tag> -- mostra a tag e o commit
-----------------------------------------------------------------
> git checkout v0.1 ---meu HEAD muda o ponteiro para o commit da tag e nao enxergo os commits mais recentes. Para sair so realizar checkout para a feature || master.
-----------------------------------------------------------------
(major).(minor).(fix)

fix - correcao 
minor - 
major - mudanca de estrutura
-----------------------------------------------------------------

## Instalando Jenkins

Tem que ser a versao abaixo e o maven que é o gerenciador de pacotes e dependencias do java.

> yum install -y java-1.8.0-openjdk maven

> curl http://pkg.jenkins-ci.org/redhat/jenkins.repo -o /etc/yum.repos.d/jenkins.repo
> rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
> yum install jenkins -y
> systemctl enable jenkins
> systemctl start jenkins

PIPELINE IN JENKINS:

=> https://jenkins.io/doc/book/pipeline/syntax/

 - Especificar um agent - minhas maquinas que irei executar ou executar local 'any'.
 - Depois meu bloco de stage, minhas execucoes;
 - Dentro do blco, sou obrigado a criar um step;
 ===================================
 pipeline {

    // agent {
    //     docker {
    //         image 'alpine'
    //     }
    // }
    
    agent any
    stages {
        stage('Compilacao do Projeto'){
            steps{
                echo "$WORKSPACE"
                sh "curl ifconfig.io"
            }
        }
        stage('Finalizando Projeto'){
            steps{
                echo "Projeto compilado com sucesso"
            }
        }
    }    
}
 ====================================
 
 ======================================
 pipeline {

    // agent {
    //     docker {
    //         image 'alpine'
    //     }
    // }
    
    agent any
    stages {
        stage('Compilacao do Projeto'){
            steps{
                echo "$WORKSPACE"
                sh "curl ifconfig.io"
            }
        }
        stage('Rodando testes em paralelo'){
            parallel{
                stage('teste no windows XP'){
                    steps{
                        echo "Windows"
                    }
                }
                stage('teste no IOS'){
                    steps{
                        echo "Apple IOS X - Maverik"
                    }
                }
                stage('teste no FreeBSD 12'){
                    steps{
                        echo "Melhor que Linux"
                    }
                }
            }
        }
        stage('Deploy em Paralello'){
            parallel{
                stage('Deploy AWS'){
                    steps{
                        echo "AWS"
                    }
                }
                stage('Deploy GCloud'){
                    steps{
                        echo "GCloud"
                    }
                }
            }
        }
        stage('Finalizando Projeto'){
            steps{
                echo "Projeto compilado com sucesso"
            }
        }
    }    
}
 ======================================
 
 ======================================
 pipeline {
    
//    agent {
//        docker {
//            image 'alpine'
//        }
//    }

    agent any
    
    environment {
        GIT_URL="git@192.168.88.10/root/git.git"
        
    }
    
    stages {
        
        stage('Compilação do Projeto'){
           
            steps {
                echo "$WORKSPACE"
                sh "curl ifconfig.io"
            }
        }
        
        
        stage('Rodando testes em paralelo'){
            parallel {
                stage('Teste no Windows XP'){
                    steps {
                        echo "Windows XP"
                    }
                }
                stage('Teste no iOS'){
                    steps {
                        echo "Apple iOS X - Maverik"
                    }
                }
                stage('Teste no FreeBSD 12'){
                    steps {
                        echo "Melhoro que Linux"
                    }
                }
            }
        }
        
         stage('Deploy em Paralelo'){
            parallel {
                stage('Deploy AWS'){
                    steps {
                        echo "AWS"
                    }
                }
                stage('Deploy GCloud'){
                    steps {
                        echo "GCloud"
                    }
                }
  
            }
        }
        stage('Finalizando Projeto'){
            steps {
                echo "Projeto compilado com sucesso"
            
                echo "$GIT_URL"
                echo PATH
            }
        }
        
    }
    
}
 ======================================
 
 
 =========================================
 pipeline {
    
//    agent {
//        docker {
//            image 'alpine'
//        }
//    }

    agent any
    
    environment {
        GIT_URL="git@192.168.88.10/root/git.git"
        
    }
    
    stages {
        
        stage('Compilação do Projeto'){
           
            steps {
                echo "$WORKSPACE"
                sh "curl ifconfig.io"
            }
        }
        
        
        stage('Rodando testes em paralelo'){
            parallel {
                stage('Teste no Windows XP'){
                    steps {
                        echo "Windows XP"
                    }
                }
                stage('Teste no iOS'){
                    steps {
                        echo "Apple iOS X - Maverik"
                    }
                }
                stage('Teste no FreeBSD 12'){
                    steps {
                        echo "Melhoro que Linux"
                    }
                }
            }
        }
        
         stage('Deploy em Paralelo'){
            parallel {
                stage('Deploy AWS'){
                    steps {
                        echo "AWS"
                    }
                }
                stage('Deploy GCloud'){
                    steps {
                        echo "GCloud"
                    }
                }
  
            }
        }
        stage('Finalizando Projeto'){
            steps {
                echo "Projeto compilado com sucesso"
            
                echo "$GIT_URL"
                echo PATH
            }
        }
        
    }
    
    post {
        always {
            echo "Sempre mostrar essa msg"
        }
        success{
            echo "Somente mostrar quando for Sucesso"
        }
        failure{
            echo "Somente mostrar quando falhar"
        }
    }
    
    
    
}
 =========================================
 
 =========================================
 pipeline {
    
    agent {
        docker {
            image 'alpine'
        }
    }

    // agent any
    
    environment {
        GIT_URL="git@192.168.88.10/root/git.git"
        
    }
    
    stages {
        
        stage('Compilação do Projeto'){
           
            steps {
                echo "$WORKSPACE"
                sh "curl ifconfig.io"
            }
        }
        
        
        stage('Rodando testes em paralelo'){
            parallel {
                stage('Teste no Windows XP'){
                    steps {
                        echo "Windows XP"
                    }
                }
                stage('Teste no iOS'){
                    steps {
                        echo "Apple iOS X - Maverik"
                    }
                }
                stage('Teste no FreeBSD 12'){
                    steps {
                        echo "Melhoro que Linux"
                    }
                }
            }
        }
        
         stage('Deploy em Paralelo'){
            parallel {
                stage('Deploy AWS'){
                    steps {
                        echo "AWS"
                    }
                }
                stage('Deploy GCloud'){
                    steps {
                        echo "GCloud"
                    }
                }
  
            }
        }
        stage('Finalizando Projeto'){
            steps {
                echo "Projeto compilado com sucesso"
            
                echo "$GIT_URL"
                echo PATH
            }
        }
        
    }
    
    post {
        always {
            echo "Sempre mostrar essa msg"
        }
        success{
            echo "Somente mostrar quando for Sucesso"
        }
        failure{
            echo "Somente mostrar quando falhar"
        }
    }
    
    
    
}
 =========================================

## Instalando Docker na maquina pipeline

> yum install -y yum-utils device-mapper-persistent-data lvm2
> yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
> yum install -y docker-ce docker-ce-cli containerd.io
> systemctl enable docker
> systemctl start docker

## Rodando SonarQube no Docker

> docker run -dti --name sonarqube --restart always -p 9000:9000 sonarqube

## SONARQUBE

Tool de QA. Realiza uma analise de boas praticas para o codigo. 

Configurar no Jenkins:

Manage jenkins > Configure System > SonarQube servers > Add SonarQube > preencher os dados > Salvar o Nome pois eh com ele que vai gerar a parada.

> adicionar um codigo no gitLab
> git clone https://github.com/jrballot/hw-springboot.git hwspringboot
> git remote set-url origin git@192.168.88.10:4linux/hwspringboot.git

#  Configurar um maven

> Manage jenkins > Global Tool Configuration > Maven > de um nome para ele > 


Jenkins File para esse projeto:
===========================================
pipeline {
    
    agent any
    
    tools {
        maven 'maven360'
    }
    
    environment {
        GIT_URL="git@192.168.88.10:4linux/hwspringboot.git"
    }
    
    stages {
        stage('Compilação do Projeto') {
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }
    }
}
===========================================

Adicionando um novo stage

===========================================
pipeline {

    agent any

    tools {
        maven 'maven360'
    }


    stages{
        stage('Compilação do Projeto'){
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('SonarQube'){
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=hwspringboot'
  
                }

            }
        }
        
    }

}
===========================================
Incrementando para ter chamada do Sonar:

No Sonar:
> Administration > Configuration > Webhooks > create > a url é o endereço da http://192.168.88.20:8080/sonarqube-webhook
===========================================
pipeline {

    agent any

    tools {
        maven 'maven360'
    }


    stages{
        stage('Compilação do Projeto'){
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('SonarQube'){
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=hwspringboot'
  
                }

            }
        }
        
        stage('Resultado do SonarQube'){
            steps{
                timeout(time:1, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }

}
===========================================


Mais uma interação para controlar - a entrada de input que aguarda uma resposta

===========================================
pipeline {

    agent any

    tools {
        maven 'maven360'
    }


    stages{
        stage('Compilação do Projeto'){
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('SonarQube'){
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=hwspringboot'
  
                }

            }
        }
        
        stage('Resultado do SonarQube'){
            input{
                message "Continuar Pipeline?"
            }
            steps{
                timeout(time:1, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }

}
===========================================

--------------------------------------------------------------------------------

## NEXUS
## Rodando SonatypeNexus OSS no Docker

> docker run -dti --name nexus --restart always -p 8081:8081 sonatype/nexus3


--------------------------------------------------------------------------------
Manage Jenkins > Manage files > Add a new Config > Global Maven settings.xml

edit 

Conexao com meu nexus:

============================================
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">
	    
   <servers>
    <server>
      <id>nexus-snapshots</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
    <server>
      <id>nexus-releases</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
  </servers>

  <mirrors>
    <mirror>
      <id>central</id>
      <name>central</name>
      <url>http://localhost:8081/repository/maven-public/</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirrors>

</settings>
============================================

No git > pom.xml  - editar com as configuracoes novas de Ips

============================================
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.boraji.tutorial.springboot</groupId>
  <artifactId>spring-boot-hello-world-example</artifactId>
  <version>0.0.1</version>
  <packaging>war</packaging>
  <properties>
    <java.version>1.8</java.version>
    <wildfly-hostname>192.168.56.101</wildfly-hostname>
    <wildfly-port>10020</wildfly-port>
  </properties>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.4.RELEASE</version>
  </parent>
  <dependencies>
		<dependency>


			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<exclusions>
				<exclusion>
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-starter-tomcat</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<scope>provided</scope>
		</dependency>
		 <dependency>
			         <groupId>com.h2database</groupId>
				         <artifactId>h2</artifactId>
					         <version>1.3.156</version>
						     </dependency>
  </dependencies>
  <build>
		<finalName>helloworld</finalName>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<plugin>
				<groupId>org.wildfly.plugins</groupId>
				<artifactId>wildfly-maven-plugin</artifactId>
				<version>1.2.1.Final</version>
				<configuration>
					<hostname>${wildfly-hostname}</hostname>
					<port>${wildfly-port}</port>
				</configuration>
			</plugin>
		</plugins>
	</build>
	<distributionManagement>
		<snapshotRepository>
			<id>nexus-snapshots</id>
			<url>http://192.168.88.20:8081/repository/nexus-snapshots/</url>
		</snapshotRepository>
		<repository>
			<id>nexus-releases</id>
			<url>http://192.168.88.20:8081/repository/nexus-releases/</url>
		</repository>
	</distributionManagement>
</project>
============================================

agra editar o jenkinsfile com um stage a mais

=============================================
pipeline {

    agent any

    tools {
        maven 'maven360'
    }


    stages{
        stage('Compilação do Projeto'){
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('SonarQube'){
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=hwspringboot'
  
                }

            }
        }
        
        stage('Resultado do SonarQube'){
            input{
                message "Continuar Pipeline?"
            }
            steps{
                timeout(time:1, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
        stage('Salvando Artefato'){
            steps{
                sh 'mvn deploy clean package'
            }
        }
    }

}
=============================================
maquina PIPELINE
> /var/lib/jenkins/.m2
> curl -O https://raw.githubusercontent.com/jrballot/524-settings/master/settings.xml
> vim settings.xml

==================================================
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">
	    
   <servers>
    <server>
      <id>nexus-snapshots</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
    <server>
      <id>nexus-releases</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
  </servers>

  <mirrors>
    <mirror>
      <id>central</id>
      <name>central</name>
      <url>http://localhost:8081/repository/maven-public/</url>
      <mirrorOf>*</mirrorOf>
    </mirror>
  </mirrors>

</settings>
==================================================






```

