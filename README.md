# Relatório - Robótica

## Projeto de Robô Auxiliar para Idosos Controlado via Bluetooth

**Relatório Final da Disciplina de Elementos de Robótica da Universidade de Pernambuco (Graduação em Engenharia da Computação).**

### Componentes:
- Aline Soares  
- Caio Victor  
- Lucas Muniz  
- Marcos Prudêncio  
- Joel Medeiros  
- Sarah Alves  

---

## 1. Introdução: Propósito Geral do Projeto e da Disciplina

Este relatório documenta o processo de desenvolvimento de um protótipo de robô assistivo, realizado no âmbito da disciplina de Elementos de Robótica. O objetivo central foi aplicar os conceitos teóricos de modelagem e controle de robôs em um projeto prático, desde a simulação inicial no ambiente do CoppeliaSim até a construção de um protótipo físico.

O projeto partiu do conceito de um robô autônomo para entrega de medicamentos a idosos. No decorrer do desenvolvimento, o escopo foi estrategicamente adaptado para um sistema teleoperado via Bluetooth, visando maximizar o aprendizado prático e garantir a entrega minimamente viável. Este documento detalha a metodologia, as decisões que guiaram essa transição, os resultados e desafios obtidos.

---

## 2. Problema a Ser Resolvido 

### 2.1 Desafios Enfrentados por Pessoas Idosas no Acesso a Medicamentos

O envelhecimento populacional traz a necessidade de desenvolver soluções que garantam a autonomia e a segurança da população idosa, especialmente daqueles que vivem sozinhos. Um dos desafios mais significativos nesta fase da vida é a mobilidade reduzida. O projeto busca mitigar esse problema específico, oferecendo uma ferramenta para o transporte de pequenos objetos dentro de casa.

### 2.2 Como o Robô Pode Ajudar

- **Controle pelo Usuário:** Interface simples via smartphone com comandos Bluetooth.  
- **Transporte de Objetos:** Equipado com pinça para carregar medicamentos.  
- **Segurança Adicional:** Sensores ultrassônicos para prevenir colisões.

---

## 3. Funcionalidades Esperadas

### 3.1 Movimentação
Movimentação via Bluetooth com controle do usuário, substituindo a ideia inicial de navegação autônoma.

### 3.2 Detecção de Obstáculos
Sensor ultrassônico frontal para prevenir colisões durante a movimentação.

### 3.3 Coleta de Remédio com Pinça
Pinça frontal para manipular caixas de medicamentos.  
**Limitação:** Baixa aderência da garra dificultou a fixação firme dos objetos.

### 3.4 Controle via Bluetooth
Módulo Bluetooth conectado ao Arduino, com controle via smartphone Android.

---

## 4. Materiais Utilizados

### 4.1 Componentes de Hardware

#### 4.1.1 Componentes Eletrônicos
- Arduino Uno  
- Driver Ponte H  
- Sensor Ultrassônico  
- Micro Servo Motor  
- Módulo Bluetooth  
- Protoboard  
- Jumpers  
- Clip de Bateria  

#### 4.1.2 Componentes Mecânicos
- Chassi 2WD com motores  
- Pinça Robótica (3D)  
- Rodas e roda maluca  
- Suporte para sensor  
- Fita dupla face, parafusos, porcas  

### 4.2 Fontes de Energia
- Porta USB  
- Pilhas AA  
- Bateria 9V  

### 4.3 Ferramentas de Software
- CoppeliaSim  
- Arduino IDE  

---

## 5. Cronograma Planejado

| Semana | Atividades |
|--------|------------|
| 1 | Aquisição dos componentes e início da simulação |
| 2 | Finalização da prototipagem elétrica e projeto da pinça |
| 3 | Montagem do robô e testes básicos |
| 4 | Integração software-hardware e início do algoritmo de rota |
| 5 | Testes no labirinto e ajustes na pinça |
| 6 | Testes finais e documentação |
| 7 | Preparação para apresentação final |

---

## 6. Metodologia de Desenvolvimento

### 6.1 Pesquisa e Definição do Robô
Meta: robô 2WD auxiliar para idosos.

### 6.2 Levantamento e Estudo dos Materiais
Aquisição dos principais componentes logo na 1ª semana.

### 6.3 Desenvolvimento Modular no CoppeliaSim
#### 6.3.1 Simulação da Movimentação
Movimento básico modelado no ambiente virtual.

#### 6.3.2 Simulação da Pinça
Modelo da pinça desenvolvido e integrado ao robô virtual.

### 6.4 Integração das Funcionalidades no CoppeliaSim
Simulação completa do robô no ambiente de testes.

### 6.5 Montagem do Robô Físico
Fixação de sensores, motores, rodas e microcontrolador.

### 6.6 Integração com o Sistema Elétrico
Programação do Arduino para controle dos componentes.

### 6.7 Testes Práticos

#### 6.8.1 Bluetooth
Instabilidades detectadas nos testes finais.

#### 6.8.2 Pinça
Testes de manipulação com sucesso parcial.

#### 6.8.3 Testes Combinados
Testes finais com movimentação e pinça simultaneamente.

---

## 7. Resultados

### 7.1 Simulação no CoppeliaSim

#### 7.1.1 Funcionamento Alcançado
Movimento básico, pinça funcional, sensor de obstáculos.  
Navegação autônoma sem PSO foi parcialmente simulada com sucesso.

#### 7.1.2 Imagens e Vídeos

https://github.com/user-attachments/assets/0dff323c-bb53-4ab9-853d-4c5e0ddd8f56

#### 7.1.3 Arquivos Relevantes

Movimento robô

```lua
function onSpeedChange(uiHandle, id, newValue)
    speed=newValue*max_speed/100
    move(speed,turn)
end
function onTurnChange(uiHandle, id, newValue)
    turn=newValue*max_turn/100
    move(speed,turn)
end

function move(v,w)
    sim.setJointTargetVelocity(left_wheel,(v-b*w)/wheel_radius)
    sim.setJointTargetVelocity(right_wheel,(v+b*w)/wheel_radius)
end
function moveForward()
    sim.setJointTargetVelocity(left_wheel,-0.75*max_speed/wheel_radius)
    sim.setJointTargetVelocity(right_wheel,-0.75*max_speed/wheel_radius)
end
function moveBackwards()
    sim.setJointTargetVelocity(left_wheel,0.75*max_speed/wheel_radius)
    sim.setJointTargetVelocity(right_wheel,0.75*max_speed/wheel_radius)
end
function turnLeft()
    sim.setJointTargetVelocity(left_wheel,-0.5*max_speed/wheel_radius)
    sim.setJointTargetVelocity(right_wheel,0.5*max_speed/wheel_radius)
end
function turnRight()
    sim.setJointTargetVelocity(left_wheel,0.5*max_speed/wheel_radius)
    sim.setJointTargetVelocity(right_wheel,-0.5*max_speed/wheel_radius)
end
function stop()
    sim.setJointTargetVelocity(left_wheel,0)
    sim.setJointTargetVelocity(right_wheel,0)
end
function getDistance(sensor,max_dist)
    local detected, distance
    detected,distance = sim.readProximitySensor(sensor)
    if(detected<1) then
        distance = max_dist
    end
    return distance
end
function sysCall_init()
    -- do some initialization here
    sonar = sim.getObject('/proximitySensor')
    gripper_joint = sim.getObject('../gripper_joint')  
    left_wheel = sim.getObject(':/l_wheel_joint')
    right_wheel = sim.getObject(':/r_wheel_joint')
    wheel_radius=0.03
    max_speed=0.2
    max_turn=0.3
    speed=0
    turn=0
    b=0.0565
    max_dist=1
    gripper_open=false
    
    sim.setJointTargetVelocity(gripper_joint, 0)
    sim.setJointForce(gripper_joint, 1000)  -- for?a alta para travar
    
    ui=simUI.create('<ui enabled="true" modal="false" title="DYOR" closeable="true" layout="vbox" placement="relative" position="20,20">' ..
    '<label enabled="true" text="Linear Speed"></label>' ..
    '<hslider enabled="true" minimum="-100" maximum="100" on-change="onSpeedChange"></hslider>' ..
    '<label enabled="true" text="Angular Speed"></label>' ..
    '<hslider enabled="true" minimum="-100" maximum="100" on-change="onTurnChange"></hslider>' ..
    '<button enabled="true" text="Forward" on-click="moveForward"></button>' ..
    '<button enabled="true" text="Backwards" on-click="moveBackwards"></button>' ..
    '<button enabled="true" text="Left" on-click="turnLeft"></button>' ..
    '<button enabled="true" text="Right" on-click="turnRight"></button>' ..
    '<button enabled="true" text="Stop" on-click="stop"></button>' ..
    '</ui>')
    
    moveForward()
end
dist = 1

--function sysCall_actuation()
    --sim.setJointTargetVelocity(left_wheel,0)
    --sim.setJointTargetVelocity(right_wheel,0)
    --if dist < 0.6 then
        --turnRight()
    --else
        --moveForward()
    --end
--end
function sysCall_sensing()
    dist = getDistance(sonar,max_dist)
end
function sysCall_cleanup()
    -- do some clean-up here
end
```

* Movimento da garra
```lua
function sysCall_init()
    -- 1. PEGAR OS OBJETOS
    -- Obtemos os "handles" (identificadores) dos objetos que vamos usar.
    -- O './' significa que a busca ? relativa ao objeto que cont?m este script (MicoHand).
    -- Baseado na sua hierarquia, o sensor est? dentro de um objeto 'sonar'.
    sensorHandle = sim.getObject('/robot/proximitySensor')
    motor1Handle = sim.getObject('/robot/fingers12_motor1')
    motor2Handle = sim.getObject('/robot/fingers12_motor2')

    -- 2. DEFINIR VELOCIDADES
    -- Voc? pode ajustar estes valores para a garra fechar mais r?pido ou mais devagar.
    velocidadeFechar = 0.008
    velocidadeAbrir = 0.08

    -- Mensagem de confirma??o no console do CoppeliaSim
    print('Script da garra MicoHand iniciado. Aguardando objeto...')
end

-- Esta fun??o ? chamada em loop durante a simula??o
function sysCall_actuation()
    -- 3. LER O SENSOR
    -- A fun??o retorna 1 se detectou algo, ou 0 se n?o detectou.
    resultado = sim.readProximitySensor(sensorHandle)

    -- 4. CONTROLAR A GARRA
    -- Verificamos o resultado da leitura do sensor
    if (resultado <= 0) then
        -- Objeto detectado: FECHAR a garra.
        -- Para fechar, um motor geralmente gira em um sentido (+) e o outro no sentido oposto (-).
        sim.setJointTargetVelocity(motor1Handle, velocidadeFechar)
        sim.setJointTargetVelocity(motor2Handle, -velocidadeFechar) -- ATEN??O: Sinal negativo!
    else
        -- Nenhum objeto detectado: ABRIR a garra.
        -- Fazemos o movimento inverso ao de fechar.
        sim.setJointTargetVelocity(motor1Handle, -velocidadeAbrir)
        sim.setJointTargetVelocity(motor2Handle, velocidadeAbrir)
    end
end

-- Esta fun??o ? chamada uma vez no final da simula??o
function sysCall_cleanup()
    -- Boa pr?tica: parar os motores ao final da simula??o.
    sim.setJointTargetVelocity(motor1Handle, 0)
    sim.setJointTargetVelocity(motor2Handle, 0)
    print('Script da garra finalizado.')
end
```

### 7.2 Robô Físico

#### 7.2.1 Funcionamento Alcançado
- Sucesso na montagem  
- Controle via Bluetooth com instabilidades  
- Pinça funcional  
- Desafios na integração total das funcionalidades

#### 7.2.2 Imagens e Vídeos

* Ultrassonico com pinça
  
https://github.com/user-attachments/assets/0a450e2d-a8d3-495d-bf6f-72bf3a589409

* Desenvolvimento no IIT
  
https://github.com/user-attachments/assets/86142566-a8a1-4abd-aca1-4136099c0e2a

#### 7.2.3 Arquivos Relevantes

* Garra
```cpp
#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(9);  // attaches the servo on pin 9 to the servo oject
}

void loop() {
  for (pos = 0; pos <= 180; pos += 1) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15 ms for the servo to reach the position
  }
  for (pos = 180; pos >= 0; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15 ms for the servo to reach the position
  }
}
```

---

## 8. Desafios e Dificuldades

### 8.1 Componentes com Problemas
- Pontes H defeituosas  
- Baterias descarregadas ou com defeito  

### 8.2 Estrutura Física
- Baixa tolerância nas peças 3D  
- Fiação difícil de organizar  

### 8.3 Integração e Conectividade
- Falha no módulo Bluetooth  
- Sensor ultrassônico parou de funcionar  
- Sincronização de tarefas no Arduino foi desafiadora  

### 8.4 Limitações Técnicas vs. Expectativas
O escopo precisou ser simplificado de um robô autônomo para teleoperado.

---

## 9. Etapas Ainda em Aberto

### 9.1 Otimização de Rotas
Implementação de algoritmos como PSO permanece em aberto.

### 9.2 Testes Finais Completos
- Substituição de componentes com defeito  
- Validação de sistema completo em ambiente real  

---

## 10. Conclusões

O projeto proporcionou uma vivência completa no desenvolvimento de um robô real. Apesar das dificuldades técnicas e da simplificação do escopo original, o aprendizado em eletrônica, programação e gestão de projetos foi significativo. A experiência destacou a importância do planejamento e da resiliência em projetos de robótica.
