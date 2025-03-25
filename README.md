### Prompt para Criar um Jogo 3D de Labirinto com Soldado Controlável Utilizando Three.js

**Título:** Desenvolva um jogo 3D interativo de labirinto com um soldado controlável, utilizando a biblioteca Three.js (versão r128), seguindo rigorosamente as especificações detalhadas abaixo. O jogo deve incluir movimentação, pulos, colisões e animações, com uma interface de usuário para depuração e instruções.

**Descrição Detalhada:**

---

#### 1. Configuração da Cena e Ambiente

- **Cena:**
  - Configure uma cena 3D com um fundo na cor azul claro, representada pelo valor hexadecimal `#87CEEB`.

- **Iluminação:**
  - Adicione uma luz ambiente do tipo hemisférica com as seguintes propriedades:
    - Cor clara: branco puro (`#ffffff`).
    - Cor escura: cinza escuro (`#444444`).
    - Intensidade ajustada para 60% do máximo.
  - Inclua uma luz direcional com:
    - Cor: branco puro (`#ffffff`).
    - Intensidade ajustada para 80% do máximo.
    - Posicionada nas coordenadas `(20, 30, 20)` no espaço 3D.
    - Ative a projeção de sombras com as configurações:
      - Distância mínima da câmera de sombra: 0.1 unidades.
      - Distância máxima da câmera de sombra: 100 unidades.
      - Limites da câmera de sombra: -30 a 30 unidades nos eixos esquerdo/direito e superior/inferior.
      - Resolução do mapa de sombras: 1024x1024 pixels.

- **Chão:**
  - Crie um plano retangular com 64 unidades de largura por 64 unidades de profundidade, divididas em 16 células de 4 unidades cada.
  - Desenhe uma textura procedural para o chão em um canvas de 256x256 pixels:
    - Preencha o fundo com a cor marrom (`#8B4513`).
    - Adicione linhas de grade na cor marrom escuro (`#654321`), com espessura de 2 pixels, espaçadas a cada 16 pixels, formando uma grade 16x16.
    - Aplique a textura ao plano, configurando-a para repetir 16 vezes em ambas as direções (largura e profundidade).
  - Rotacione o plano 90 graus no sentido anti-horário em torno do eixo X para deixá-lo na horizontal.
  - Permita que o plano receba sombras projetadas por outros objetos.

- **Paredes:**
  - Construa as paredes usando blocos retangulares com dimensões de 4 unidades de largura, 3 unidades de altura e 4 unidades de profundidade.
  - Crie uma textura procedural para as paredes em um canvas de 128x128 pixels:
    - Preencha o fundo com a cor cinza médio (`#808080`).
    - Desenhe 50 retângulos aleatórios com larguras e alturas variando entre 5 e 15 pixels, em tons de cinza com valores RGB entre 100 e 200.
    - Aplique a textura aos blocos, configurando-a para se ajustar exatamente às dimensões de cada bloco (repetição 1x1).
  - Configure as paredes para projetar e receber sombras.

---

#### 2. Geração do Labirinto

- **Tamanho do Labirinto:**
  - O labirinto deve ser composto por uma grade de 16x16 células, onde cada célula mede 4 unidades de largura por 4 unidades de profundidade.

- **Regras de Geração:**
  - Coloque paredes em todas as células das bordas externas:
    - Nas linhas e colunas onde as coordenadas são 0 ou 15 (ou seja, `x = 0`, `x = 15`, `z = 0`, `z = 15`).
  - Mantenha uma área central aberta, sem paredes:
    - Nas células onde as coordenadas `x` e `z` estão entre 6 e 9, inclusive (formando um quadrado 4x4 no centro).
  - Para todas as outras células (exceto as bordas e a área central):
    - Adicione uma parede em cada célula com uma probabilidade de 15%.
  
- **Posicionamento das Paredes:**
  - Centralize o labirinto em torno da origem `(0, 0, 0)` no espaço 3D:
    - Ajuste as posições das paredes subtraindo metade do tamanho total do labirinto (32 unidades) menos metade do tamanho de uma célula (2 unidades).
  - Posicione cada parede com sua base a 1.5 unidades acima do chão, correspondendo à metade de sua altura.

---

#### 3. Personagem (Soldado)

- **Modelo 3D:**
  - Carregue um modelo 3D de um soldado no formato GLB a partir da URL pública `https://threejs.org/examples/models/gltf/Soldier.glb`.

- **Configuração do Modelo:**
  - Mantenha a escala do modelo em proporção natural (1.0 em todos os eixos).
  - Posicione o soldado inicialmente nas coordenadas `(0, 0, 0)`.
  - Habilite a projeção de sombras para todas as partes visíveis do modelo.

- **Animações:**
  - Gerencie as animações do soldado utilizando um sistema que sincronize os movimentos com o tempo.
  - Associe as animações disponíveis do modelo às seguintes ações:
    - **Ocioso (Idle):** use a primeira animação do modelo (índice 0).
    - **Correndo (Run):** use a quarta animação do modelo (índice 3).
    - **Pulando (Jump):** use a segunda animação do modelo (índice 1).
  - Inicie o jogo com o soldado na animação ociosa.
  - Faça transições suaves entre as animações, com uma duração de 0.2 segundos para cada mudança.

---

#### 4. Controles e Movimentação

- **Teclas de Controle:**
  - Movimentação:
    - Frente: tecla `W` ou seta para cima.
    - Trás: tecla `S` ou seta para baixo.
    - Esquerda: tecla `A` ou seta para esquerda.
    - Direita: tecla `D` ou seta para direita.
  - Pulo: tecla `Espaço`, permitindo um segundo pulo no ar.

- **Movimentação:**
  - Defina a velocidade de deslocamento como 5 unidades por segundo.
  - Ajuste a direção do movimento para ser relativa à orientação da câmera, considerando apenas o plano horizontal (eixos X e Z).
  - Gire o soldado para alinhar-se com a direção do movimento:
    - Calcule o ângulo de rotação com base na direção do movimento e adicione 180 graus para corrigir a orientação.

- **Pulo:**
  - Configure o primeiro pulo para alcançar 8 unidades de altura.
  - Configure o segundo pulo (pulo duplo) para alcançar 12 unidades de altura.
  - Adicione um intervalo mínimo de 300 milissegundos entre pulos consecutivos.
  - Limite o número de pulos a 2 por sequência, restaurando essa contagem ao tocar o chão.

---

#### 5. Física e Colisões

- **Gravidade:**
  - Aplique uma força descendente de 15 unidades por segundo ao quadrado quando o soldado estiver no ar.

- **Colisões:**
  - Represente o soldado com uma esfera invisível de raio 0.8 unidades para verificar colisões.
  - Detecte colisões com as paredes (cada uma representada por um retângulo 3D) e com o chão (nível `y = 0`).
  - Trate colisões de forma distinta:
    - **Laterais:** bloqueie o movimento horizontal ao atingir uma parede.
    - **Verticais:** ao cair, ajuste a posição vertical do soldado para ficar ligeiramente acima do chão ou do topo de uma parede (somando o raio da esfera à altura do ponto de contato).
  - Permita que o soldado suba em paredes se o pulo atingir ou exceder a altura de 3 unidades.

- **Estado do Soldado:**
  - Monitore se o soldado está no chão ou sobre uma parede com uma variável de estado.
  - Restaure a contagem de pulos para 2 sempre que o soldado estiver em contato com uma superfície.

---

#### 6. Câmera e Controles de Visualização

- **Câmera:**
  - Use uma câmera em perspectiva com:
    - Campo de visão de 75 graus.
    - Proporção ajustada automaticamente ao tamanho da janela.
    - Distância mínima de visibilidade: 0.1 unidades.
    - Distância máxima de visibilidade: 1000 unidades.
  - Posicione a câmera inicialmente em `(0, 20, 30)`.

- **Controles da Câmera:**
  - Implemente um sistema de controle orbital que permita rotação e zoom com o mouse.
  - Configure a câmera para seguir o soldado, ajustando o ponto de foco para a posição exata do soldado, incluindo sua altura vertical.

- **Responsividade:**
  - Atualize a proporção da câmera e o tamanho da área de renderização sempre que a janela do navegador mudar de tamanho.

---

#### 7. Interface de Usuário

- **Painel de Debug:**
  - Coloque um painel no canto superior esquerdo da tela.
  - Estilize-o com fundo preto semitransparente (opacidade 70%), texto branco, bordas arredondadas de 5 pixels, margem interna de 10 pixels e fonte Arial de 14 pixels.
  - Mostre as seguintes informações atualizadas em tempo real:
    - **Posição:** coordenadas `x`, `y`, `z` do soldado, com 2 casas decimais.
    - **Rotação:** ângulo de rotação do soldado em graus, com 2 casas decimais.
    - **Animação:** nome da animação atual (Idle, Run ou Jump).
    - **Pulos:** número de pulos disponíveis sobre o total, no formato "disponíveis/2".

- **Instruções:**
  - Coloque um texto no canto inferior esquerdo da tela.
  - Use o mesmo estilo visual do painel de debug.
  - Exiba a mensagem: "WASD ou Setas: Mover | Espaço: Pular | Espaço no ar: Pular novamente".

---

#### 8. Renderização e Loop de Animação

- **Renderizador:**
  - Configure um renderizador gráfico com suavização de bordas ativada.
  - Habilite o suporte a mapas de sombra para iluminação realista.

- **Loop de Animação:**
  - Crie um ciclo contínuo de atualização usando uma função que execute a cada frame.
  - Atualize as animações do soldado com base no tempo decorrido entre frames, calculado em segundos.
  - Em cada frame:
    - Aplique gravidade se o soldado estiver no ar.
    - Verifique colisões verticais e ajuste a posição vertical conforme necessário.
    - Processe os comandos do jogador para movimentação horizontal, verificando colisões laterais.
    - Ajuste a rotação do soldado para acompanhar a direção do movimento.
    - Altere a animação com base no estado atual (parado, correndo ou pulando).
    - Atualize o foco da câmera para acompanhar a posição do soldado.
    - Redesenhe a cena completa.

---

#### 9. Detalhes Técnicos Adicionais

- **Bibliotecas Necessárias:**
  - Use a biblioteca Three.js versão r128.
  - Inclua um carregador para modelos GLB.
  - Adicione controles orbitais para a câmera.

- **Estrutura Visual:**
  - Crie uma página HTML básica com um canvas que ocupe toda a tela, removendo margens e barras de rolagem.
  - Posicione os elementos de interface (painel de debug e instruções) sobre o canvas, sem interferir na renderização 3D.

- **Eventos:**
  - Registre comandos de teclado (pressionar e soltar teclas) para controlar o soldado.
  - Adicione um monitoramento para redimensionamento da janela, ajustando a câmera e a área de renderização dinamicamente.

---

#### 10. Requisitos Finais

- **Precisão nas Colisões:**
  - Garanta que o soldado não atravesse paredes ao se mover lateralmente.
  - Permita que ele suba em paredes se o pulo for alto o suficiente.
  - Corrija a posição vertical ao aterrissar, alinhando-o corretamente com o chão ou o topo das paredes.

- **Animações:**
  - Ative a animação de corrida ao mover-se no chão.
  - Ative a animação de pulo ao saltar.
  - Retorne à animação ociosa quando parado em uma superfície.

- **Interface:**
  - Atualize o painel de debug a cada frame com informações precisas.
  - Mantenha as instruções visíveis durante todo o jogo.

---

**Resultado Esperado:**  
Um jogo 3D interativo onde o jogador controla um soldado em um labirinto de 16x16 células, com gráficos detalhados, física realista, movimentação fluida, pulos duplos, colisões precisas e uma interface funcional para depuração e orientação.

**Observação:**  
Organize o desenvolvimento de forma clara e modular, seguindo as melhores práticas para jogos 3D, e respeite todas as especificações descritas para garantir um resultado fiel ao esperado.

--- 

Forneça-me o HTML