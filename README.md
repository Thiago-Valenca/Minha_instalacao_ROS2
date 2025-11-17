# Processo de Instalação do ROS 2 Jazzy para Ubundu 24.04 Noble.

Siga os comandos abaixo para instalar o ROS2 Jazzy no Ubuntu 24.04:

````
locale
````
````
sudo apt update && sudo apt install locales
````
````
sudo locale-gen en_US en_US.UTF-8
````
````
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
````
````
export LANG=en_US.UTF-8
````
````
locale
````
Agora, em outro terminal:
````
sudo apt install software-properties-common
````
````
sudo add-apt-repository universe
````
````
sudo apt update && sudo apt install curl -y
````
````
sudo curl -sSL http://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
````
````
echo "deb [arch=$(dpkg --print-architecture)
signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
````
````
sudo apt update && sudo apt install ros-dev-tools
````
Agora, em outro terminal:
````
sudo apt install ros-jazzy-desktop
````
````
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
````
````
source ~/.bashrc
````
````
sudo apt-get install gedit -y
````
````
gedit ~/.bashrc
````
Pode fechar a janela que foi aberta.

Você pode usar usar os três comandos abaixo para checar que a versão instalada do ROS é o Jazzy:
````
printenv ROS_DISTRO
````
````
env | grep ROS
````
````
echo $ROS_DISTRO
````
Continuando a instalação:
````
sudo rosdep init
````
````
rosdep update
````
Então, podemos instalar alguns softwares necessários:
````
sudo apt-get install python3 python3-pip -y
sudo apt-get install ros-${ROS_DISTRO}-ros-gz -y
sudo apt-get install python3-numpy
````
Já podemos rodar um exemplo de simulação no Gazebo através do seguinte comando em um novo terminal:
````
$ LIBGL_ALWAYS_SOFTWARE=1 QT_QPA_PLATFORM=xcb gz sim -v 4 shapes.sdf
````
Então pressione CTRL C para fechar.

Em outro terminal:
````
echo "export LIBGL_ALWAYS_SOFTWARE=1" >> ~/.bashrc
echo "export QT_QPA_PLATFORM=xcb" >> ~/.bashrc
source ~/.bashrc
````
Para abrir o Gazebo:
````
gz sim
````
Então pressione CTRL C para fechar.

Em outros dois terminais, pode-se rodar o mesmo exemplo anterior por meio do ROS ou do Gazebo, respectivamente:

Usando ROS:
````
ros2 launch ros_gz_sim gz_sim.launch.py gz_args:="shapes.sdf"
````
CTRL C para fechar.

Usando Gazebo:
````
gz sim shapes.sdf -v 4
````
CTRL C para fechar.


## Exemplo de um programa ROS2: talker-listener
É possível rodarnos um programa que imprime mensagens na tela e outro que escuta estas mensagens:
````
ros2 run demo_nodes_cpp talker
````
Em outro terminal:
````
ros2 run demo_nodes_cpp listener
````
CTRL C nos dois terminais para fechar.

## Pacote para ver vários terminais simultaneamente
````
sudo apt-get install terminator -y
terminator
````

# Entendendo DDS e a Variável ROS_DOMAIN_ID

ROS_DOMAIN_ID é uma variável que determina o canal de comunicação entre diferentes equipes de robôs. Por exemplo, se todos os robôs precisam se comunicar, eles terão o mesmo ROS_DOMAIN_ID, mas se precisarem se comunicar entre si sem interferir em outras equipes de robôs, precisarão ter ROS_DOMAIN_ID diferentes. Alguns exemplos:
O comando abaixo muda o valor do ROS_DOMAIN_ID (para 8, por exemplo).

O comando abaixo me mostra qual é o ROS_DOMAIN_ID  atual do meu terminal.
````
export ROS_DOMAIN_ID=8
````
# Como Criar uma Workspace no ROS 2 Jazzy

Uma Workspace é uma pasta que será o local principal de desenvolvimento e organização do software do robô.

Primeiro, escrevemos o seguinte comando no terminal:

````
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/
````
````
colcon build
````
Isto retornará 0 packages, porque não fizemos nenhum ainda. O comando colcon build sempre deve ser realizado no root do meu workspace, quando fazemos modificações no código fonte do robô.

Agora, se escrevermos o comando abaixo no terminal, veremos as novas pastas que foram criadas em nosso workspace.
````
dir
````
Vemos que as pastas build, install e log foram criadas.

Para ter certeza que nosso workspace pode ser acessado em qualquer momento que abrimos um novo terminal:
````
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
````
Para ver se nosso workspace foi adicionado à fonte dos terminais:
````
gedit ~/.bashrc
````
Na janela aberta, vemos que nosso workspace foi adicionado no fim do arquivo. Agora, nosso workspace pode ser encontrado sempre que abrimos um novo terminal.

# Instalando e Configurando VS Code para ROS 2 Jazzy

Podemos instalar o VS Code pesquisando "visual studio code download linux" no Google.

No terminal:
````
cd ~/Downloads
pwd
dir
sudo dpkg -i {nome que aparece depois que você digita dir, começando com "code"}
````
Agora, o VS Code foi instalado. Então, precisamos ir para nosso workspace.
````
cd ~/ros2_ws
pwd
````
Para abrir o VS Code:
````
code .
````
Podemos trocar o tema para Solarized Dark para melhor conforto visual. Na aba de extensões, devemos baixar a extensão de C/C++ (da Microsoft), a extensão do ROS (da Microsoft), a extensão do CMake (do twxs), a extensão CMake Tools (da Microsoft), a extensão XML (da Red Hat), a extensão Python (da Microsoft) e a extensão autoDocstring (do Nils Werner). Agora, vá em File, Preferences, Setting, pesquise por Indentation e na aba Editor: Tab Size, troque de 4 para 2. Feche o VS Code em File, Close Window.

No terminal
````
cd ~/ros2_ws/src
````
Assim, abrimos o VS Code dentro do folder de código fonte do nosso workspace

# Como Criar um Pacote no ROS 2 Jazzy

No ROS, pacotes são pastas que contém funcionalidades ou componentes específicos de um sistema robótico. Ou seja, o pacote contém códigos, arquvos, definições de mensagens. O spacotes ajudam a modular o desenvolvimento e manter o sistema funcionando, pemritindo reutilizar o pacote em outros projetos.

No terminal:
````
cd ~/ros2_ws/srck
ros2 pkg create --build-type ament_cmake --license Apache-2.0 ros2_fundamentals_examples
````
Este comando criou o pacote ros2_fundamentals_examples em nosso workspace."--build-type ament_cmake" especifica que o pacote deve usar o ament cmake build system, que é o sistema de criação recomendado para pacotes escritos em C++, para ter certeza que tudo está compilado, ligado e pronto para funcionar. "--license Apache-2.0" é uma licença com restrições mínimas que permite que os usuários modifiquem e distribuam o software

Para ver o pacote na workspace:
````
clear
dir
````
Para construir nosso pacote, navegamos até o root do workspace:
````
cd ..
clear
colcon build
````
Para checar se nosso pacote é reconhecido pelo ROS 2:
````
source ~/.bashrc
clear
````
Em outro terminal:
````
ros2 pkg list
````
Podemos ver que nosso pacote foi incluído
````
clear
echo "alias build='cd ~/ros2_ws && colcon build'" >> ~/.bashrc && source ~/.bashrc
````
Esta linha de comando faz com que, ao digitarmos "build" no terminal, vamos executar os caomandos cd ~/ros2_ws e colcon build. Além disso, ele adiciona o alias ao arquivo fonte do bashrc para ser reconhecido aem qualquer terminal. Agora, se dermos o comando:
````
build
````
Será o mesmo que digitarmos
````
cd ~/ros2_ws
colcon build
````
Se você quiser construir um pacote específico, e não todos os pacotes, podemos digitar:
````
colcon build --packages-select ros2_fundamentals_examples
````
Vamos instalar alguns pacotes que irão nos auxiliar.
````
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install terminator
````
Pode pesquisar por "Terminator" nos aplicativos e abrí-lo ou digitar:
````
terminator
````
Agora sim, para instalar os pacotes:
````
sudo curl -sSL http://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
````
````
echo "deb [signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu noble main" | sudo tee /etc/apt/sources.list.d/ros2.list
````
````
sudo apt update
clear
````
````
sudo apt-get install -y ros-${ROS_DISTRO}-ros-gz
````
````
sudo apt-get install -y ros-${ROS_DISTRO}-ros-gz ros-${ROS_DISTRO}-gz-ros2-control ros-${ROS_DISTRO}-gz-ros2-control-demos ros-${ROS_DISTRO}-joint-state-publisher-gui ros-${ROS_DISTRO}-moveit ros-${ROS_DISTRO}-xacro ros-${ROS_DISTRO}-ros2-control ros-${ROS_DISTRO}-ros2-controllers libserial-dev python3-pip
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
````
