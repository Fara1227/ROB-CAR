# ROB-CAR
O presente projeto descreve sobre um veículo robô de prevenção de obstáculos, um site e uma aplicação.
O veículo robô de prevenção de obstáculos é controlado por um sensor ultrassónico e por microcontroladores Arduíno. O sensor ultrassónico é colocado na parte frontal do veículo robô. O sensor é montado num servo motor que se move para a esquerda, direita e centro para ajudar o sensor ultrassónico a detetar obstáculos. O sensor sente o obstáculo e desvia-se do seu caminho para escolher um caminho livre de obstáculos. O sensor envia os dados para o controlador, que então decide o movimento das rodas do robô. O movimento e a direção das rodas baseiam-se na deteção do sensor ultrassónico e no uso de um motor driver. Este veículo é usado para detetar obstáculos e evitar a colisão. Além disso, um módulo de câmera é usado para assistir à transmissão em direto do robô. Esse módulo de câmera cria o seu próprio servidor local via Wi-Fi para transmitir o vídeo diretamente. Este recurso pode ser útil, especificamente, para tarefas como espionagem de missões, observação de um espaço apertado remotamente e outros trabalhos perigosos.
Para além do veículo em si, também haverá um site que permite aos utilizadores verem a informação sobre o projeto, fazer download do código, da aplicação e do relatório. 
	Para além do veículo e do site, teremos uma aplicação que permitirá controlar o veículo ou deixá-lo realizar a sua função de se desviar dos obstáculos.
O funcionamento do nosso robô que evita obstáculos é o seguinte:
1) Conectamos a bateria ao Arduíno.
2) Ele, automaticamente, começa a avançar e os LEDs vermelhos estão desligados. A câmera esp32 também se conecta ao Wi-Fi e começa a transmitir ao vivo no seu servidor local.
3) O sensor ultrassónico está posicionado no meio e procura obstáculos na frente. À medida que a distância até ao obstáculo se torna inferior a 40 centímetros (cm), o robô para e os LEDs vermelhos acendem.
4) O servo então varre da esquerda para a direita para ajudar o sensor ultrassónico a detetar qual é o melhor caminho.
5) Se a distância até ao obstáculo à direita for maior do que à esquerda, o robô vira à direita por 360 ms.
6) Se a distância até ao obstáculo à esquerda for maior do que à direita, o robô vira à esquerda por 360 ms.
7) Se ambas as distâncias forem menores que 40 cm, o robô retrocede por 180 ms.
8) Tudo isto num ciclo contínuo.

