# Criação de tema customizado para o WebSphere Portal

Segundo a IBM, a melhor maneira de criar um tema customizado, é copiar o tema padrão do próprio WebSphere Portal, para que na criação do tema customizado, não seja esquecido de incluir as configurações necessárias.

> The best way to start creating your own custom theme is by copying the portal WebSphere® Portal theme. This ensures that your theme has all the required elements for the theme to function.

Todos os passos demonstrados abaixo tiveram como base em um vídeo hospedado no Youtube que pode ser acessado através deste [link](https://www.youtube.com/watch?v=bMmeynv8ke0&list=PLogxjp96lSq8H4XLiVJnZMzN8e3m1sqO7).

1. [Cópia do tema padrão do WebSphere Portal](#1-cópia-do-tema-padrão-do-websphere-portal)
1. [Cópia da skin padrão do WebSphere Portal](#2-cópia-da-skin-padrão-do-websphere-portal)

## 1. Cópia do tema padrão do WebSphere Portal

Para se fazer a cópia do tema padrão do WebSphere Portal precisaremos acessar o diretório onde se encontra o tema dentro de uma instância do WebSphere Portal. A leitura desse diretório se dá através do protocolo WebDAV (Web Distributed Authoring and Versioning) que é um protocolo de transferência de arquivos, semelhantemente ao protocolo FTP, com a vantagem de conter informações sobre a versão e o autor dos arquivos e mudanças.

Para acessar o diretório remoto e transfência dos arquivos do tema padrão do WebSphere Portal é preciso ter um cliente WebDAV.  Sugiro utilizar o WinSCP que é o programa mais conhecido, também muito utilizado para transferência de arquivos via protocolo FTP.

Abrir o WinSCP e configurar uma nova conexão definindo os seguintes parâmetros:

Campo | Descrição
---|---
Protocolo de arquivo | WebDAV
Criptografia | Sem criptografia (Dependendo de como está disponibilizado a instância)
Host | O endereço IP ou DNS da instância do WebSphere Portal
Porta | 10039
Usuário | O usuário que acessa a administração do WebSphere Portal
Senha | A senha do usuário que acessa a administração do WebSphere Portal
Diretório remoto | /wps/mycontenthandler/dav/themelist
Diretório local | Diretório definido na máquina local onde os arquivos serão copiadas

Uma vez conectado será exibido todo o conteúdo do **Diretório remoto**, feito isso basta copiar o diretório `ibm.portal.85Theme` para a máquina local (no meu caso estou utilizando o WebSphere Portal 8.5, por isso do "85Theme", no seu caso pode ser outro) e renomear para o nome desejado para o seu tema. 

Feito tudo isso ainda é preciso executar algumas tarefas:

1. Definir quais linguagens será dado suporte em /metadata
1. Nos arquivos de localização definidos em `title` alterar o nome do tema
1. No arquivo metadata.properties em `com.ibm.portal.layout.template.href` alterar o nome do tema de acordo com o nome do diretório raíz do tema
1. Excluir a pasta skins

## 2. Cópia da skin padrão do WebSphere Portal

Da mesma maneira feita em [1. Cópia do tema padrão do WebSphere Portal](#1-cópia-do-tema-padrão-do-websphere-portal), iremos fazer para o skin.

Criar uma nova conexão no WinSCP e definir os seguintes parâmetros: 

Campo | Descrição
---|---
Protocolo de arquivo | WebDAV
Criptografia | Sem criptografia (Dependendo de como está disponibilizado a instância)
Host | O endereço IP ou DNS da instância do WebSphere Portal
Porta | 10039
Usuário | O usuário que acessa a administração do WebSphere Portal
Senha | A senha do usuário que acessa a administração do WebSphere Portal
Diretório remoto | /wps/mycontenthandler/dav/skinlist
Diretório local | Diretório definido na máquina local onde os arquivos serão copiadas

Uma vez conectado será exibido todo o conteúdo do Diretório remoto, feito isso basta copiar o diretório `ibm.portal.85Hidden` para a máquina local (no meu caso estou utilizando o WebSphere Portal 8.5, por isso do "85Hidden", no seu caso pode ser outro) e renomear para o nome desejado para o seu Skin.

Feito tudo isso ainda é preciso executar algumas tarefas:

1. Definir quais linguagens será dado suporte em /metadata
1. Nos arquivos de localização definidos em `title` alterar o nome da skin

## 3. Cópia dos recursos dinâmicos do WebSphere Portal para ser utilizado no seu tema.

Mesmo que você planeje somente fazer alterações nas partes estáticas do seu recente copiado tema, é recomendável que você faça cópia também dos recursos dinâmicos do tema padrão do WebSphere Portal para garantir que futuras CFs (Cumulative Fix) não afetem o seu tema.

Para isso realize os seguintes passos:

Com ajuda de uma IDE Java de sua preferência, criar um módulo web (.war) contendo a seguinte estrutura:

```
.
+-- web
|   +-- WEB-INF
|   |   +-- web.xml
+-- pom.xml
```

1. Copiar o diretório `themes` do seguinte caminho de uma instalação do WebSphere Portal: *<WebSphere Portal Installation Root>/theme/wp.theme.themes/default85/installedApps/DefaultTheme85.ear/DefaultTheme85.war/themes* para o diretório `web` do módulo web criado
1. Copiar o diretório `skins` do seguinte caminho de uma instalação do WebSphere Portal: *<WebSphere Portal Installation Root>/theme/wp.theme.themes/default85/installedApps/DefaultTheme85.ear/DefaultTheme85.war/skins* para o diretório `web` do módulo web criado
1. Copiar o diretório: `tld` e os arquivos: `decorations.xml` e `plugin.xml` do seguinte caminho de uma instalação do WebSphere Portal *<WebSphere Portal Installation Root>/theme/wp.theme.themes/default85/installedApps/DefaultTheme85.ear/DefaultTheme85.war/WEB-INF* para o diretório `web/WEB-INF` do módulo criado


O resultado esperado é a seguinte estrutura abaixo:

```
.
+-- web
|   +-- themes
|   +-- skins
|   +-- WEB-INF
|   |   +-- tld
|   |   +-- web.xml
|   |   +-- decorations.xml
|   |   +-- plugin.xml
+-- pom.xml
```

## 4. Alterar as referências de recurso dinâmicas para seu tema

É importante fazer isso porque se você não atualizar as referências o seu tema pode não funcionar corretamente e até acarretar no mal funcionamento do tema padrão do WebSphere Portal.

No módulo web criado (.war), realizar os seguintes passos:

1. Abrir o arquivo `plugin.xml`
1. Alterar o atributo `id` no elemento `<extension>` para um nome específico que será referenciado no seu tema
1. Alterar o atributo `id` no elemento `<module>` para um nome específico que será referenciado no seu tema
1. Alterar o atributo `ref-id` dos elementos `sub-contribution` para um nome específico que será referenciado no seu tema

No diretório do tema criado no passo [1. Cópia do tema padrão do WebSphere Portal](#cópia-do-tema-padrão-do-websphere-portal), realizar os seguintes passos:


