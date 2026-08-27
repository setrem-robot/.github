<div align="center">

<img src="https://avatars.githubusercontent.com/u/317356829?v=4" width="140" alt="SETREM Robot" />

# 🤖 SETREM Robot

**Um robô autônomo que anda e conversa pelo campus da SETREM**

*Projeto Interdisciplinar de Extensão · Engenharia de Computação*
*Sociedade Educacional Três de Maio — Três de Maio, RS*

</div>

---

## A Atlas

A **Atlas** é um robô construído do zero por alunos da SETREM. Ela tem uma cara
que olha e responde, um corpo que anda, e um celular que a dirige — e as três
coisas são software nosso, rodando num **Raspberry Pi 5**.

Nada de nuvem de terceiros para pensar: a IA de conversação roda **na nossa
própria rede**, e cai para um modelo dentro do robô quando a rede não está lá.

## Os três repositórios

A Atlas é dividida em três, e cada um roda sozinho — dá para trabalhar em
qualquer um deles sem ter os outros dois, e sem ter o robô montado.

| | Repositório | O que faz |
|---|---|---|
| 🙂 | [**atlas_ai_v2**](https://github.com/setrem-robot/atlas_ai_v2) | **A cara.** Face animada, voz, conversa com IA e a ponte Bluetooth. |
| 🦾 | [**orquestrador**](https://github.com/setrem-robot/orquestrador) | **O corpo.** Motores, GPS, Wi-Fi, telemetria e a nuvem que guarda tudo. |
| 📱 | [**aplicativo**](https://github.com/setrem-robot/aplicativo) | **O controle.** App Flutter: dirigir o robô e ver os dados. |

**Começando agora?** Abra o [`orquestrador`](https://github.com/setrem-robot/orquestrador)
— o README dele sobe a nuvem inteira na sua máquina em quinze minutos, com um
robô falso gerando dados. Não é preciso ter o robô, nem o Raspberry Pi, nem
acesso à VM da faculdade.

---

## O que já funciona

Tudo nesta lista roda e foi verificado. O que é planejamento está na lista
seguinte.

#### A cara

- Face animada em 30 quadros por segundo, desenhando direto no vídeo do kernel
  — **sem ambiente gráfico**, num Pi 5 com tela de 800×480.
- **IA dupla:** enquanto há rede, responde um modelo grande num PC (~1 s até
  começar a falar); se o PC sumir, um modelo pequeno dentro do robô assume sem
  a conversa parar, e devolve o lugar quando a rede volta.
- **Voz com reserva:** voz online mais natural por padrão, voz local em Piper
  quando não há internet. Ficar offline vira troca de timbre, não silêncio.
- Configuração pelo celular, numa página web — sem teclado, sem monitor, sem
  SSH.
- Ponte Bluetooth no próprio Pi: o celular conecta direto no robô.

#### O corpo

- **Motores** por cinemática diferencial: o comando vira velocidade de cada
  lado, passa por uma rampa de aceleração e sai como onda quadrada do PWM nos
  drivers TMC2209. Um vigia para tudo se ficar um segundo sem comando.
- **Telemetria que não se perde:** o robô publica num broker local que repassa
  para a nuvem, guardando até cem mil mensagens em disco enquanto a rede
  estiver fora — cerca de catorze horas de queda sem perder um ponto.
- **Banco de séries temporais** (TimescaleDB) e uma **API de leitura** que
  nunca escreve, rodando com um usuário do banco que só tem `SELECT`.
- Publicada na internet por um **túnel da Cloudflare**, com HTTPS válido.

#### O controle

- Dirigir por Bluetooth, com o robô parando sozinho se a conexão morrer com o
  dedo no botão.
- Quatro telas de dados: estado agora, trajeto no mapa, gráficos de histórico e
  os eventos crus.

#### E, atravessando os três

- **800 testes automatizados**, nenhum deles exigindo o robô, o
  Raspberry Pi, um broker ou o banco. O que depende de hardware fica atrás de
  uma interface; o que é lógica é testado como lógica.

---

## O que ainda falta

Lista honesta, na ordem de quanto cada item trava os outros.

- **Um broker só no Raspberry Pi.** Hoje dois programas tentam subir um
  Mosquitto na mesma porta, e o segundo falha em silêncio. O sintoma engana:
  o app conecta, o comando chega, e o robô não se mexe.
- **Os motores no hardware.** O código tem 45 testes e roda simulado num
  notebook. O que nunca aconteceu é um motor de verdade girar com ele.
- **O GPS.** O serviço existe e publica; o módulo ainda não está montado.
- **A nuvem na VM do LARCC.** Está pronta e testada, mas rodando na máquina de
  mesa de um dos integrantes. Migrar é trocar as senhas e parar de passar um
  arquivo.
- **A Atlas não sabe nada do próprio corpo.** Ela não recebe a telemetria: não
  sabe a bateria, onde está, se está andando. E não fala o que o app manda —
  o tópico existe, ninguém escuta.
- **A escuta não tem reserva de rede.** A IA e a voz já usam o PC quando ele
  existe e caem para o local quando não; a transcrição de áudio ainda roda
  sempre no próprio robô.
- **Um cooler.** Sem ventoinha, a IA local leva o Pi a quase 80 °C em trinta
  segundos, e o processador reduz a frequência.

---

## Como trabalhamos

- **Português, em todo lugar.** Código, commits, comentários e documentação.
- **Comentário explica o porquê, não o quê.** Se o código já diz o que faz, o
  comentário fica para o que não está escrito: o motivo da escolha, o que foi
  medido, o que já se tentou e não deu certo.
- **Medir antes de otimizar.** Números medidos vão para a documentação com o
  valor ao lado, para que a suspeita errada não volte daqui a três meses.
- **Documentar o que falta com a mesma clareza do que funciona.** Todo README
  aqui tem uma seção do que não está pronto.

---

<div align="center">

**Três de Maio · Rio Grande do Sul · Brasil**

</div>
