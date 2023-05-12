# Born2beRoot_42 Project
__________
Tabela de conteudos ...
_________

# Introdução
Neste projeto, você criará sua primeira máquina virtual usando o VirtualBox (ou UTM, caso não possa usar o VirtualBox), seguindo instruções específicas. Ao final, você será capaz de configurar seu próprio sistema operacional, aplicando regras rigorosas.

### O que é uma Máquina Virtual?
Uma máquina virtual é um **software capaz de instalar um Sistema Operacional (SO) dentro de si, fazendo com que o SO acredite que está hospedado em um computador real**. Com máquinas virtuais, podemos criar dispositivos virtuais que se comportam da mesma forma que dispositivos físicos, usando sua própria CPU, memória, interface de rede e armazenamento. Isso é possível porque **a máquina virtual é hospedada em um dispositivo físico**, que fornece os recursos de hardware para a VM. O programa de software que cria máquinas virtuais é o hypervisor. O hypervisor é responsável por isolar os recursos da VM do hardware do sistema e implementar as funcionalidades necessárias para que a VM possa usar esses recursos.

Os dispositivos que fornecem os recursos de hardware são chamados de máquinas host ou hosts. As diferentes máquinas virtuais que podem ser atribuídas a um host são chamadas de máquinas guest ou guests. O hypervisor utiliza uma parte da CPU, armazenamento, etc., da máquina host e os distribui entre as diferentes VMs.

Podem existir várias máquinas virtuais no mesmo host, e cada uma delas estará isolada do restante do sistema. Graças a isso, podemos executar diferentes sistemas operacionais em nossa máquina. Para cada máquina virtual, podemos executar uma distribuição diferente de sistema operacional. Cada um desses sistemas operacionais se comportará como se estivesse hospedado em um dispositivo físico, ou seja, teremos a mesma experiência ao usar um SO em uma máquina física e em uma máquina virtual.

### Como funcionam as Máquinas Virtuais?
A virtualização permite compartilhar um sistema com vários ambientes virtuais. O hypervisor gerencia o sistema de hardware e separa os recursos físicos dos ambientes virtuais. **Os recursos são gerenciados de acordo com as necessidades, do host para os guests**. Quando um usuário de uma VM realiza uma tarefa que requer recursos adicionais do ambiente físico, o hypervisor gerencia a solicitação para que o SO guest possa acessar os recursos do ambiente físico.

Conhecendo o funcionamento das máquinas virtuais, é interessante observar as vantagens de utilizá-las:

* Diferentes máquinas guest hospedadas em nosso computador podem executar diferentes sistemas operacionais, permitindo múltiplos SOs na mesma máquina.
* Elas fornecem um ambiente seguro para testar programas instáveis sem afetar o sistema.
* Melhor aproveitamento dos recursos compartilhados.
* Redução de custos ao diminuir a arquitetura física.
* Facilidade de implementação, pois oferecem mecanismos para clonar uma máquina virtual para outro dispositivo físico.

### O que é LVM?
**O LVM (Gerenciador de Volumes Lógicos) é uma camada de abstração entre um dispositivo de armazenamento e um sistema de arquivos**. Obtemos muitas vantagens ao usar o LVM, mas a principal é que temos muito mais flexibilidade no gerenciamento de partições. Suponha que criamos quatro partições em nosso disco de armazenamento. Se por algum motivo precisarmos expandir o armazenamento das três primeiras partições, não poderemos, pois não há espaço disponível ao lado delas. Caso queiramos estender a última partição, teremos sempre o limite imposto pelo disco. Em outras palavras, não poderemos manipular partições de forma amigável. Graças ao LVM, todos esses problemas são resolvidos.

Usando o LVM, **podemos expandir o armazenamento de qualquer partição** (agora conhecida como volume lógico) sempre que quisermos, sem nos preocuparmos com o espaço contíguo disponível em cada volume lógico. Podemos fazer isso com armazenamento disponível localizado em diferentes discos físicos (o que não podemos fazer com partições tradicionais). Também podemos mover diferentes volumes lógicos entre dispositivos físicos. Claro, serviços e processos funcionarão da mesma forma que sempre funcionaram. Mas para entender tudo isso, precisamos saber:

* **Volume Físico (PV)**: dispositivo de armazenamento físico. Pode ser um disco rígido, um cartão SD, um disquete, etc. Este dispositivo nos fornece armazenamento disponível para uso.
* **Grupo de Volume (VG)**: para usar o espaço fornecido por um PV, ele deve ser alocado em um grupo de volume. É como um disco de armazenamento virtual que será usado pelos volumes lógicos. Os VGs podem crescer ao longo do tempo adicionando novos PVs.
* **Volume Lógico (LV)**: esses dispositivos serão os que usaremos para criar sistemas de arquivos, swaps, máquinas virtuais, etc. Se o VG é o disco de armazenamento, os LVs são as partições feitas neste disco.

### O que é AppArmor?
O AppArmor fornece segurança de **Controle de Acesso Obrigatório (MAC). Na verdade, o AppArmor permite que o administrador do sistema restrinja as ações que os processos podem executar**. Por exemplo, se um aplicativo instalado pode tirar fotos acessando o aplicativo da câmera, mas o administrador nega esse privilégio, o aplicativo não poderá acessar o aplicativo da câmera. Se ocorrer uma vulnerabilidade (algumas das tarefas restritas são realizadas), o AppArmor bloqueia o aplicativo para que o dano não se espalhe para o restante do sistema.

No AppArmor, **os processos são restritos por perfis**. Os perfis podem funcionar no modo de reclamação (complain-mode) e no modo de aplicação (enforce-mode). No modo de aplicação, o AppArmor proíbe os aplicativos de executarem tarefas restritas. No modo de reclamação, o AppArmor permite que os aplicativos realizem essas tarefas, mas cria uma entrada no registro para exibir a reclamação.

### Qual é a diferença entre Apt e Aptitude?
Nas distribuições de sistemas operacionais baseadas em Debian, **o gerenciador de pacotes padrão que podemos usar é o dpkg**. Esta ferramenta nos permite instalar, remover e gerenciar programas em nosso sistema operacional. No entanto, na maioria dos casos, esses programas vêm com uma lista de dependências que devem ser instaladas para que o programa principal funcione corretamente. Uma opção é instalar essas dependências manualmente. No entanto, o **APT (Advanced Package Tool)**, que é uma ferramenta que usa o dpkg, **pode ser usado para instalar todas as dependências necessárias ao instalar um programa**. Assim, agora podemos instalar um programa útil com um único comando.

O APT pode trabalhar com diferentes back-ends e front-ends para utilizar seus serviços. Um deles é o **apt-get**, que nos permite **instalar e remover pacotes**. Junto com o apt-get, há também muitas ferramentas como o apt-cache para gerenciar programas. Neste caso, o apt-get e o apt-cache são usados pelo apt. Graças ao apt, podemos instalar programas .deb facilmente e sem nos preocupar com as dependências. Mas, caso queiramos usar uma interface gráfica, teremos que usar o aptitude. **O Aptitude também controla melhor as dependências**, permitindo ao usuário escolher entre diferentes dependências ao instalar um programa.

### Como usar o SSH?
O SSH ou **Secure Shell** é um protocolo de **administração remota que permite aos usuários controlar e modificar seus servidores pela Internet** graças a um mecanismo de autenticação. Ele fornece um mecanismo para autenticar um usuário remotamente, transferir dados do cliente para o host e retornar uma resposta à solicitação feita pelo cliente.
O SSH foi criado como uma alternativa ao Telnet, que não criptografa as informações enviadas. O SSH usa técnicas de criptografia para garantir que todas as comunicações entre cliente e host e host e cliente sejam feitas de forma criptografada. Uma das vantagens do SSH é que um usuário usando Linux ou MacOS pode usar o SSH em seu servidor para se comunicar com ele remotamente por meio do terminal do computador. Uma vez autenticado, esse usuário poderá usar o terminal para trabalhar no servidor.

Existem três técnicas diferentes que o SSH usa para criptografia:
* **Criptografia simétrica**: um método que usa a mesma chave secreta para criptografia e descriptografia de uma mensagem, tanto para o cliente quanto para o host. Qualquer pessoa que saiba a senha pode acessar a mensagem transmitida.
* **Criptografia assimétrica**: usa duas chaves separadas para criptografia e descriptografia. Estas são conhecidas como chave pública e chave privada. Juntas, elas formam o par de chaves público-privado.
* **Hashing**: outra forma de criptografia usada pelo SSH. As funções de hash são feitas de forma que não precisam ser descriptografadas. Se um cliente possui a entrada correta, ele pode criar um hash criptográfico e o SSH verificará se ambos os hashes são iguais.

### Como implementar o UFW com SSH
O **UFW (Uncomplicated Firewall)** é um aplicativo de software responsável por garantir que o administrador do sistema possa gerenciar o iptables de maneira simples. Como é muito difícil trabalhar com iptables, o UFW nos fornece uma interface para modificar o firewall de nosso dispositivo **(netfilter)** sem comprometer a segurança. Depois de instalado o UFW, podemos escolher quais portas queremos permitir conexões e quais portas queremos fechar. Isso também será muito útil com o SSH, melhorando muito toda a segurança relacionada às comunicações entre dispositivos.

O que é cron e o que é wall?
Uma vez que saibamos um pouco mais sobre como construir um servidor dentro de uma Máquina Virtual (lembre-se de que você também deve procurar em outras páginas além deste README), veremos dois comandos que serão muito úteis no caso de sermos administradores de sistemas. Esses comandos são:

* Cron: gerenciador de tarefas do Linux que nos permite executar comandos em um determinado momento. Podemos automatizar algumas tarefas apenas dizendo ao cron qual comando queremos executar em um horário específico. Por exemplo, se quisermos reiniciar nosso servidor todos os dias às 4:00 da manhã, em vez de termos que acordar nesse horário, o cron fará isso por nós.
* Wall: comando usado pelo usuário root para enviar uma mensagem a todos os usuários atualmente conectados ao servidor. Se o administrador do sistema quiser alertar sobre uma grande mudança no servidor que possa fazer com que os usuários sejam desconectados, o usuário root poderia alertá-los com o wall.

_______
# Instalação
_____

## *Sudo*
### Passo 1: Instalar o sudo
Mude para o root e seu ambiente usando o comando su -.
```
su -
```
Mude para o root e seu ambiente usando o comando su -.
```
apt install sudo
```
Verifique se o sudo foi instalado corretamente usando dpkg -l | grep sudo. (Opcional)
```
dpkg -l | grep sudo
```

### Passo 2: Adicionar usuário ao grupo sudo
Adicione o usuário ao grupo sudo usando adduser <username> sudo.
```
adduser <username> sudo
```
ou
```
usermod -aG sudo <username>
```
Verifique se o usuário foi adicionado com sucesso ao grupo sudo usando getent group sudo.
```
getent group sudo
```
Reinicie para que as alterações entrem em vigor, faça login e verifique os poderes do sudo usando sudo -v.
```reboot```
```sudo -v```
### Passo 3: Executando comandos com privilégios de root
