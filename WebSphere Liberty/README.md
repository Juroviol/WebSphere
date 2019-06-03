# WebSphere Liberty
Documentação do que é e como utilizar o WebSphere Liberty


## Introdução

### O que é

O WebSphere Liberty é um servidor de aplicação Java, rápido, dinâmico e fácil de usar. Ideal para desenvolvedores mas também em produção, com tempo de inicialização rápida, mudanças sem necessidade de reiniciar e uma simples configuração via XML.

### Por que?

Desenvolver aplicações para serem instaladas no servidor de aplicação WebSphere Aplication Server (WAS), pode ser um trabalho díficil se for utilizado o próprio servidor, como servidor onde será realizados os testes durante o desenvolvimento de uma aplicação. Visto que o mesmo não foi feito para ser utilizado em tempo de desenvolvimento, mas sim em ambientes de homologação e produção. 

É claro que um container web como o tomcat ou jetty pode ser utilizado para a maioria dos casos em que não há necessidade dos recursos de um servidor de aplicação Java EE, ou um servidor de aplicação Java EE como o Wildfly da Red Hat, que possui ampla documentação disponível na internet e é muito mais simples a sua instalação e configuração do que o WAS. Porém, pode ser que você acabe trabalhando no desenvolvimento de alguma aplicação cujo servidor será o WAS. Não por sua causa, mas por ser o servidor de aplicação do cliente. Por isso o Liberty será a sua melhor opção, pois com ele há uma garantia de que a aplicação irá rodar também no WAS (WebSphere Aplication Server), o que pode não acontecer caso seja utilizado durante o desenvolvimento outro servidor de aplicação ou container.

### Como funciona?

O WebSphere Liberty, uma vez instalado não irá possuir nenhum recurso Java EE instalado, como por exemplo: JSF, Portlet, CDI, JMS, JPA, etc. Ficará na responsabilidade do utilizador fornecer ao Liberty, quais são os recursos necessários. Isso trás leveza e dinamismo, pois o servidor de aplicação não precisará inicializar os diversos recursos Java EE que existem em um servidor de aplicação normal, o qua à sua aplicação, não tem necessidade. 

A especificação dos recursos desejados se dá através da configuração de um arquivo xml denominado `server.xml`. Existe também a possibilidade da mesma configuração ser realizado via interface gráfica pela central de administração do Liberty. Vale ressaltar porém que a central de administração por inteface gráfica não está disponível por padrão devendo ser instalado.

## Instruções

### Habilitando Admin Center

Para se habilitar o Admin Center é preciso primeiro instalar a funcionalidade no Liberty. Para isso executar a instalação através de linha de comando a partir do diretório raiz da instalação:

```
{installationRoot}/bin/installUtility install adminCenter-1.0` 
```

Depois é preciso especificar no arquivo `server.xml` que queremos utiliza-lo no nosso servidor. Para isso adicionar a feature adminCenter-1.0 conforme abaixo:

```
...
<features>
  ...
  <feature>adminCenter-1.0</feature>
</features>
...
```

Com isso já é possível acessar a interface gráfica de administração através do endereço: https://localhost:9443/adminCenter/ e será apresentado a tela de autenticação. Como não configuramos nenhuma conta, não será possível acessar ainda.

Para configurar uma conta de acesso a interface gráfica de administração do Liberty, adicionar a configuração de conta inicial no arquivo `server.xml` conforme abaixo:

```
...
<quickStartSecurity userName="admin" userPassword="admin" />
...
```
Agora já podemos acessar a interface gráfica de configuração, todavia ainda não podemos fazer modificações, apenas visualiza-las. Mais uma configuração no `server.xml` deverá ser realizado para especificar ao liberty que queremos ter a possibilidade de modificarmos as configurações via interface gráfica:

```
...
<remoteFileAccess>
  <writeDir>${server.config.dir}</writeDir>
</remoteFileAccess>
...
```

Pronto! Agora podemos acessar e realizar modificações nas configurações do servidor.

### Liberty Features

No link abaixo é possível encontrar todas as funcionalidades que podem ser ativas no WebSphere Liberty.
https://www.ibm.com/support/knowledgecenter/SSAW57_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/rwlp_feat.html
