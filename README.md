# Hope-Robot

## Visão Geral do Projeto

Este repositório documenta o processo de desenvolvimento de um **Robô 2WD** (Two-Wheel Drive) autônomo, concebido especificamente para auxiliar idosos em tarefas cotidianas, como o transporte de pequenos itens essenciais (ex: medicamentos) dentro de suas residências. O projeto integra conceitos de robótica móvel, programação embarcada e simulação de sistemas robóticos. Desenvolvido como parte do projeto final da disciplina de Robótica, este trabalho abrange todas as etapas, desde o planejamento e montagem física até a implementação de algoritmos de navegação e a validação do sistema em ambiente simulado.

## Funcionalidades Principais

* **Transporte de Pequenos Itens:** Capacidade primária de carregar e mover objetos leves, visando a autonomia de idosos.
* **Montagem e Prototipagem:** Desenvolvimento e integração do chassi 2WD com todos os componentes eletrônicos e mecânicos essenciais.
* **Simulação em CoppeliaSim:** Modelagem tridimensional detalhada do robô e do ambiente de teste. A simulação permite o desenvolvimento e a validação de algoritmos de controle e navegação antes da implementação no hardware físico.
* **Navegação Inteligente:** Implementação de um algoritmo de otimização de rotas (como PSO - Particle Swarm Optimization ou similar) para permitir que o robô encontre os caminhos mais eficientes para seus destinos.
* **Controle Interativo via UI:** Interface de usuário (UI) desenvolvida diretamente no CoppeliaSim, proporcionando controle manual intuitivo sobre a velocidade linear e angular do robô, além de comandos diretos de movimentação.
* **Testes e Validação Rigorosos:** Realização de testes comparativos entre o comportamento do robô no ambiente real e na simulação. Validação das funcionalidades (busca e transporte de objetos) em cenários controlados, como labirintos simples.
* **Pinça/Gripper:** Incorporação de uma pinça (`MicoHand`) para potencial manipulação e transporte de objetos, com seu projeto e calibração sendo parte integrante do desenvolvimento.

## Tecnologias Utilizadas

### Hardware

* **Chassi:** Plataforma robótica 2WD.
* **Locomoção:** Motores de corrente contínua e rodas, incluindo um rodízio (caster_wheel) para estabilidade e manobrabilidade.
* **Controle:** Microcontrolador (Arduino) como cérebro do robô.
* **Sensores:**
    * **Proximity Sensor (Sonar):** Essencial para detecção de obstáculos e medição de distâncias, crucial para a navegação autônoma.
    * **Vision Sensor:** Para percepção visual do ambiente e possíveis tarefas de reconhecimento.
    * **TCRF5000:** Sensor adicional que pode ser utilizado para seguir linhas ou detecção de proximidade mais específica.
* **Atuadores Adicionais:** Servos para controle de rodas e do gripper, e um buzzer para sinalização sonora.
* **Alimentação:** Powerbank para fornecer energia ao sistema.
* **Comunicação:** Módulo Bluetooth para comunicação sem fio.
* **Manipulador:** Pinça (`MicoHand`) com junta controlável (`gripper_joint`).

### Software e Ambiente de Simulação

* **CoppeliaSim:** Ambiente de simulação robusto utilizado para modelagem, programação e teste do robô virtualmente.
    * **Arquivos de Cena (.ttt):** Contêm o modelo 3D do robô (`DYOR_robot.ttt`) com todos os seus componentes (rodas, sensores, gripper, etc.) e as cenas de teste, incluindo ambientes como labirintos e objetos de interação (como a cesta rosa).
* **Linguagem de Scripting Lua:** Utilizada extensivamente dentro do CoppeliaSim para programar a lógica de controle, simulação de sensores, atuação dos motores e a interface de usuário.
* **Algoritmo de Otimização:** Implementação de um algoritmo como o Particle Swarm Optimization (PSO) para otimização de rotas.

## Detalhes do Controle e Simulação (CoppeliaSim Script)

O script Lua integrado ao CoppeliaSim é o coração da lógica de controle do robô, gerenciando a movimentação, a leitura de sensores e a interação com uma interface de usuário personalizada.

### Parâmetros e Componentes Iniciais (`sysCall_init`):
Na inicialização da simulação, o script define parâmetros físicos do robô e obtém referências para os objetos da cena, garantindo que o software interaja corretamente com o modelo virtual:
* `sonar`: Handle para o sensor de proximidade (`/proximitySensor`).
* `gripper_joint`: Handle para a junta principal do gripper (`../gripper_joint`).
* `left_wheel`, `right_wheel`: Handles para as juntas das rodas (`:/l_wheel_joint`, `:/r_wheel_joint`).
* `wheel_radius`: Raio das rodas (0.03 metros).
* `max_speed`: Velocidade linear máxima do robô (0.2 m/s).
* `max_turn`: Velocidade angular máxima do robô (0.3 rad/s).
* `b`: Distância entre o centro do robô e o eixo das rodas (0.0565 metros), crucial para cálculos de cinemática diferencial.
* `max_dist`: Distância máxima que o sensor de proximidade consegue detectar (1 metro).
* O gripper é inicializado com velocidade zero e uma força de 1000 N, indicando uma configuração para mantê-lo em uma posição fixa inicial.

### Funções de Movimentação (`move`, `moveForward`, etc.):
O script oferece um conjunto de funções para controlar o robô, convertendo comandos de alto nível em velocidades específicas para as juntas das rodas:
* `move(v, w)`: Função central que ajusta as velocidades angulares das rodas esquerda e direita com base na velocidade linear desejada (`v`) e na velocidade angular (`w`) do robô. A cinemática diferencial é aplicada aqui:
    * `sim.setJointTargetVelocity(left_wheel, (v - b * w) / wheel_radius)`
    * `sim.setJointTargetVelocity(right_wheel, (v + b * w) / wheel_radius)`
* `moveForward()`: Move o robô para frente com uma velocidade linear de 75% da `max_speed`.
* `moveBackwards()`: Move o robô para trás com 75% da `max_speed`.
* `turnLeft()`: Faz o robô girar para a esquerda (roda esquerda para trás, roda direita para frente).
* `turnRight()`: Faz o robô girar para a direita (roda esquerda para frente, roda direita para trás).
* `stop()`: Para o movimento do robô, zerando as velocidades das rodas.

### Leitura de Sensores (`getDistance`, `sysCall_sensing`):
A percepção do ambiente é feita através do sensor de proximidade:
* `getDistance(sensor, max_dist)`: Função que lê o sensor de proximidade especificado. Retorna a distância detectada. Se o sensor não detectar nada, retorna a `max_dist` pré-definida.
* `sysCall_sensing()`: Esta função é chamada a cada passo de simulação e é responsável por atualizar a variável global `dist` com a leitura mais recente do sensor `sonar`.

### Interface de Usuário (UI) no CoppeliaSim:
Uma UI interativa é criada na inicialização (`sysCall_init`) para facilitar o teste e a depuração manual do robô:
* **Sliders:** Permitem o ajuste em tempo real da "Linear Speed" e "Angular Speed", variando de -100 a 100, que são mapeadas para as funções `onSpeedChange` e `onTurnChange`, respectivamente.
    * `onSpeedChange(uiHandle, id, newValue)`: Atualiza a velocidade linear do robô.
    * `onTurnChange(uiHandle, id, newValue)`: Atualiza a velocidade angular do robô.
* **Botões de Comando:**
    * "Forward", "Backwards", "Left", "Right", "Stop": Cada botão aciona a respectiva função de movimento do robô.

### Lógica de Controle (Exemplo Comentado no Script):
O script fornecido inclui um bloco de código `sysCall_actuation` que está comentado. Este bloco ilustra uma lógica de controle reativo simples, onde o robô moveria para frente se a distância do sensor fosse maior que 0.6 metros, e viraria à direita caso contrário, evitando obstáculos. Este serve como um ponto de partida para algoritmos de navegação mais complexos.

## Cronograma Detalhado do Desenvolvimento

O projeto foi planejado para ser executado em 7 semanas, com marcos e dependências claras:

* **Semana 1: Planejamento e Início da Prototipagem**
    * Pedido do chassi (entrega em 3 dias).
    * Recebimento e catalogação dos componentes (motores, sensores, microcontrolador, bateria, etc.).
    * Início da prototipagem do sistema elétrico e testes de componentes individuais.
    * Simulações iniciais no CoppeliaSim, incluindo a modelagem básica do robô e do ambiente de testes.
    * **Dependências:** A prototipagem elétrica só avança após a chegada dos componentes. As simulações podem ser feitas em paralelo.

* **Semana 2: Prototipagem Elétrica e Projeto da Pinça** 
    * Finalização da prototipagem elétrica, com todas as conexões testadas e validadas.
    * Avanço nas simulações do CoppeliaSim, com foco na movimentação básica do robô.
    * Desenvolvimento do projeto da pinça (desenho para impressão 3D).
    * **Dependências:** Escolha do modelo da pinça para impressão e liberação para iniciar a impressão.

* **Semana 3: Montagem Física e Simulação Avançada**
    * Montagem física completa do robô, incluindo a fixação de motores, sensores e controladores.
    * Realização de testes básicos de movimentação (frente, ré, curvas).
    * Finalização da simulação no CoppeliaSim, integrando a lógica de navegação.
    * Impressão 3D da pinça (se aplicável).
    * **Dependências:** A montagem física depende diretamente da chegada do chassi e de todos os componentes.

* **Semana 4: Integração Lógica e Início do Algoritmo de Rota** 
    * Integração total do sistema elétrico com a lógica de controle, programando o microcontrolador para a movimentação completa do robô.
    * Realização de testes comparativos entre a movimentação do robô físico e a simulação, buscando ajustes finos.
    * Início da implementação do algoritmo de otimização de rota (PSO ou similar).
    * **Dependências:** O algoritmo de rota depende da movimentação básica do robô estar plenamente funcional.

* **Semana 5: Otimização de Rota e Validação Funcional** 
    * Testes do algoritmo de otimização de rota em cenários de labirintos simples.
    * Ajustes na pinça, caso haja necessidade de melhorias de funcionalidade.
    * Validação de todas as funcionalidades do robô, como a busca e o transporte de um objeto simulado.
    * **Dependências:** Os testes dependem do algoritmo de otimização estar completamente implementado e funcional.

* **Semana 6: Testes Finais e Documentação** 
    * Realização de testes finais abrangentes, combinando a navegação em labirintos com a otimização de rota.
    * Ajustes finais no código e no hardware, focando na eficiência da bateria e dos sensores.
    * Início da preparação da documentação final do projeto (relatório técnico, vídeo de demonstração, slides de apresentação).
    * **Dependências:** Esta semana é crucial para correções de falhas detectadas nos testes.

* **Semana 7: Preparação para a Apresentação Final** 
    * Foco total na preparação da apresentação final, incluindo a revisão dos slides e o planejamento da demonstração ao vivo do robô.
    * Últimos ajustes estéticos (opcional).
    * **Dependências:** Sem dependências críticas, mas atrasos acumulados impactam diretamente a demonstração e a validação do projeto.

## Como Rodar/Simular

### Pré-requisitos:

* **CoppeliaSim:** Certifique-se de ter o CoppeliaSim instalado em sua máquina. A versão utilizada no projeto deve ser compatível com os arquivos de cena (`.ttt`) fornecidos.

### Para Simular no CoppeliaSim:

1.  **Clone o Repositório:** Faça um clone deste repositório para o seu computador.
    ```bash
    git clone [https://github.com/SeuUsuario/Hope-Robot.git](https://github.com/SeuUsuario/Hope-Robot.git)
    cd Hope-Robot/coppeliasim
    ```
2.  **Abra o Arquivo de Cena:** No CoppeliaSim, vá em `File -> Open scene...` e navegue até a pasta `coppeliasim/` do repositório clonado. Abra o arquivo `DYOR_robot.ttt`.
3.  **Explore a Simulação:** Uma vez a cena carregada, você verá o modelo do robô, os ambientes de teste e a interface de usuário interativa.
4.  **Inicie a Simulação:** Clique no botão de "Play" (geralmente um triângulo verde) na barra de ferramentas do CoppeliaSim para iniciar a simulação.
5.  **Controle o Robô:** Utilize os sliders e botões da interface de usuário flutuante (intitulada "DYOR") para controlar o movimento do robô em tempo real dentro da simulação.
6.  **Observe o Comportamento:** Acompanhe a interação do robô com o ambiente, observe a leitura dos sensores e o movimento do gripper.

## Equipe

* Aline Soares 
* Caio Victor 
* Joel Medeiros 
* Marcos Prudêncio 
* Sarah Alves 
