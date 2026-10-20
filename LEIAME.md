# Colônia — protótipo

Jogo de sobrevivência de colônia no estilo RimWorld, em **um único arquivo** (`index.html`).
HTML + JavaScript + Canvas puros: sem instalação, sem internet, sem etapa de build.
A arte é toda **desenhada por código** em estilo vetorial (curvas suaves e contornos escuros, como no RimWorld). Não é preciso nenhum arquivo de imagem.

## Como abrir

1. Copie a pasta `colonia` para qualquer lugar do computador.
2. Dê **dois cliques em `index.html`**. Abre no navegador padrão (Chrome, Edge ou Firefox).
3. Cada abertura gera um mapa novo. Para repetir um mapa, use `index.html?seed=12345`.
   Parâmetros de teste: `&hora=21` (hora inicial) e `&zoom=2.4` (zoom de 0.35 a 3).

## Como se joga

Os colonos vivem sozinhos. Você só dá ordens:

- **Cortar árvores:** tecla **C** (ou *Arquitetar → Cortar árvores*). Clique ou arraste sobre as árvores. Aparece um machado em cada uma.
- **Minerar rocha:** tecla **M**. Clique ou arraste sobre os maciços de rocha. Aparece uma picareta.
- **Cancelar ordens:** tecla **X**. Para sair do modo de ordens: **Esc** ou botão direito.
- **Mandar andar:** selecione um colono e use o clique direito.

Os colonos cortam, mineram e **carregam** a madeira e a pedra para a **zona de estoque** (o quadrado tracejado azul). O contador no canto superior esquerdo mostra o que está guardado: madeira, pedra e frutas.

### Inteligência dos colonos (automática)

Em ordem de prioridade:

1. **Comer** quando a fome passa de 35%: frutas do chão/estoque ou direto do arbusto, o que estiver mais perto. Eles também comem antes de dormir.
2. **Dormir** à noite (22h–5h) ou quando o sono acaba, no saco de dormir de cada um.
3. **Triste** (humor < 25%): recusa trabalho e fica vagando e suspirando, até o humor passar de 40%.
4. **Trabalho leve:** colher frutas quando a comida está acabando. Fora isso, alternar entre as ordens (cortar/minerar) e carregar itens ao estoque.
5. **Passear** quando não há nada a fazer.

**Humor:** começa em 50 e soma os pensamentos: com fome, cansado, conversou com alguém, discutiu, dormiu ao relento, comeu... Os pensamentos aparecem no painel *Inspecionar*.

**Conversas:** quando dois colonos estão perto, às vezes puxam papo (balões de fala), mesmo trabalhando. Conversa boa: humor + e opinião +. Discussão (às vezes): humor − e opinião −. A opinião de um sobre o outro é um número simples, mostrado no painel.

As frutas dos arbustos voltam a crescer em 3 dias.

## Tela

- **Topo, no centro:** retratos dos colonos com barra de humor. Clique para selecionar e dê duplo clique para centralizar.
- **Topo à esquerda:** contador de recursos do estoque e, abaixo, as mensagens, que somem sozinhas. O histórico completo fica na aba *Registro*.
- **Embaixo à esquerda:** painel *Inspecionar*, com o colono (atividade, humor, fome, sono, pensamentos, opiniões) ou o tile/item/árvore clicado. Logo acima fica o terreno sob o mouse.
- **Embaixo à direita:** hora, dia, estação, temperatura e velocidade (❚❚ ▶ ▶▶ ▶▶▶).
- **Barra inferior:** *Arquitetar* (ordens: cortar, minerar, cancelar), *Trabalho* (o que cada colono está fazendo), *Agenda* (horário de sono), *Registro* (histórico) e *Ajuda*.

## Controles

| Ação | Tecla / mouse |
|---|---|
| Mover a câmera | WASD ou setas |
| Zoom | Roda do mouse (aproxima no ponto do cursor) |
| Arrastar a câmera | Segurar o botão direito ou do meio e arrastar |
| Selecionar colono / inspecionar | Clique esquerdo |
| Próximo colono | Tab (e centraliza) |
| Sair do modo de ordens / desmarcar / fechar aba | Esc |
| Mandar andar | Clique direito num tile (com um colono selecionado) |
| Ordens: cortar / minerar / cancelar | C / M / X, depois clicar ou arrastar no mapa |
| Caçar animais / Caçada em grupo (v8) | J / U, depois clicar no animal |
| Pausar / continuar | Espaço |
| Velocidade | 1, 2, 3 |
| Qualidade gráfica (Baixo → Médio → Alto) | G, ou botão **⚙ Opções** na barra de baixo |
| Novo mapa | N |
| Ajuda | H |

O caminho (A*) desvia de água funda, árvores e rochas. A água rasa é atravessável, mas é lenta.
1 segundo real equivale a 10 minutos de jogo em 1x, então um dia dura 2,4 minutos.

## Qualidade gráfica (Baixo / Médio / Alto)

Mude pelo botão **⚙ Opções (G)** ou apertando **G**. A troca é na hora: o terreno é redesenhado e aparece um aviso com o nível. A escolha fica salva no navegador (`localStorage`), e para testes dá para forçar com `index.html?q=baixo|medio|alto`.

- **Baixo**: para PC fraco. Terreno com 16 px por tile (cache de cerca de 1600×1600), menos traços de grama, sem flores, sombras fracas e nenhuma nas rochas. Copas com uma forma só, sem detalhe de folhas. Água sem ondas, sem partículas. O colono vira de direção na hora e tem menos detalhe (sem pezinhos).
- **Médio**: o visual padrão (32 px por tile).
- **Alto**: terreno com 40 px por tile, mais grama, flores e pedrinhas. Copas com camadas extras, brilho e cachos de folhas, e 2 ou 3 tufos que balançam cada um no seu ritmo. As sombras das árvores são suaves (em degradê) e mudam de direção e comprimento conforme a hora do sol. Há folhas caindo de carvalhos e álamos, vaga-lumes à noite e cintilação na água. Os colonos ganham pé que rola, cabelo e roupa com balanço próprio (mola) e piscam.

Atenção: no Alto o cache do terreno ocupa cerca de 64 MB de memória de vídeo, e no Médio cerca de 40 MB.

## Animação de caminhada

O passo é calculado pela **distância andada**, não pelo tempo: andando devagar na água, o ciclo fica mais lento. O corpo sobe e desce a cada passo, os pés alternam embaixo do corpo, as mãos balançam ao contrário das pernas e o corpo se inclina para a frente. O colono vira suavemente entre as direções, acelera ao sair e freia ao chegar. Carregando algo, segura a carga com as duas mãos e o passo fica mais curto.
A simulação roda em passo fixo de 30 ticks/s (`CONFIG.SIM_HZ`), e o desenho interpola a posição entre dois ticks. Assim o movimento fica liso a 60 fps em 1x, 2x e 3x. Os ajustes ficam em `CONFIG.ACELERACAO`, `CONFIG.FREIO` e `CICLO_PASSO` (tiles por ciclo de 2 passos).

## O que tem no mapa

Grama, terra, terra rica, areia, água rasa e funda (com margem suave) e maciços de rocha (granito).
Árvores: carvalho, pinheiro, álamo e arbusto de frutas, todas balançando suavemente. Também há tocos, plantinhas, flores e pedrinhas.
Itens no chão: madeira, pedra e frutas silvestres (com quantidade). Os colonos começam com 40 de madeira e 40 de frutas no estoque.

## Como usar spritesheets do Piskel (opcional)

Para trocar o desenho por código por arte própria feita no [Piskel](https://www.piskelapp.com):

### Personagem (colono)

1. No Piskel, crie o sprite em **32x32** (ou 16x16, 64x64...).
2. **Quadro 0 = parado.** Os quadros seguintes são a caminhada.
3. A folha deve ter **4 linhas = direções** e **N colunas = quadros**, nesta ordem:
   - linha 0: olhando para **baixo**
   - linha 1: **esquerda**
   - linha 2: **direita**
   - linha 3: **cima**

   Exemplo: 3 quadros de 32x32 → imagem de 96x128 px.
   No Piskel, faça os 12 quadros em sequência e use **Export → PNG → Spritesheet** com *columns* = número de quadros por direção.
4. **Teste rápido:** selecione um colono e **arraste o PNG para a janela do jogo**. O tamanho do quadro é calculado como altura ÷ 4.
5. **Definitivo:** salve em `sprites/` (ao lado do `index.html`) e edite o objeto `SPRITES` no começo do script:

   ```js
   colono: { arquivo: 'sprites/colono.png', larguraQuadro: 32, alturaQuadro: 32,
             quadrosCaminhada: 0,       // quantos quadros de caminhada usar (0 = todas as colunas menos a 0)
             distanciaPorQuadro: 0.3,   // tiles andados por quadro: o sprite anima pela distância
             porColono: { 'Ana': 'sprites/ana.png' } },   // opcional: uma folha por colono
   ```

   O quadro é desenhado com 1 tile de largura e os pés no chão. A arte em pixel aparece nítida, sem borrar.

### Árvores e terreno

- **Árvore:** PNG de 1 linha. Cada quadro tem 2 tiles de largura por 2,5 de altura (ex.: 32x40), com a base do tronco no centro inferior. A ordem é espécie × variação (carvalho 0-2, pinheiro 0-2, álamo 0-2, arbusto 0-2). O balanço continua sendo feito pelo código. Configure em `SPRITES.arvore`.
- **Terreno:** PNG de 1 linha com um quadro por tipo: grama, terra, terra rica, areia, água rasa, água funda, rocha. Configure em `SPRITES.terreno`. Atenção: com terreno por sprite a grade de tiles volta a aparecer.

Se um arquivo não existir ou falhar, o jogo volta ao desenho por código.

## Organização do código (`index.html`)

- `CONFIG`: tamanho do mapa, zoom, velocidades, colonos (nome, pele, cabelo, roupa, estilo de cabelo, barba) e ajustes da IA (queda das necessidades, tempos de trabalho, limite de tristeza, frequência das conversas).
- `SPRITES`: gancho para os PNGs.
- `UTIL/RUÍDO`: aleatório com semente e ruído de valor.
- `MAPA`: campos de ruído, tipos de terreno e árvores.
- `ITENS`: pilhas no chão, zona de estoque, contador.
- `PATHFINDING`: A* com custo por terreno.
- `COLONOS`: IA por prioridades, tarefas (cortar, minerar, colher, carregar, comer, dormir), pensamentos/humor, conversas.
- `ARTE`: terreno pré-renderizado a 32 px/tile com transições suaves (repintado só no pedaço que muda quando se corta ou minera), rochas, copas, itens e colonos vetoriais.
- `RENDER`: câmera, ordem de desenho, luz do dia/noite, colchetes de seleção, rótulos, balões de fala, barras de progresso, "z z z".
- `INPUT`, `UI` e `LOOP`.

## Roadmap

- **Feito:** mapa procedural, câmera, arte vetorial estilo RimWorld, colonos com IA (comer, dormir, triste), conversas com balões e opinião, cortar/minerar/colher, carregar para o estoque, contador de recursos, ciclo dia/noite, abas.
- **Próximo — construção:** paredes, portas, chão e camas com madeira/pedra (plantas-baixa que os colonos constroem). Mais zonas de estoque desenhadas pelo jogador.
- **Comida de verdade:** plantação na terra rica, cozinha e refeições, mesa (pensamento "comeu sem mesa").
- **Eventos e animais:** animais selvagens, caça, clima, visitantes.
- **Prioridades de trabalho** por colono (aba *Trabalho*) e habilidades.
- **Salvar/carregar** (localStorage), sons, polimento.

---

## Novidades da v5

(A v4 ficou guardada em `v4_index.html`.)

### Animações de trabalho
- **Corte:** a árvore balança a cada golpe, tomba ~90° com aceleração e um pequeno quique, solta folhas (Médio/Alto), a sombra acompanha e por fim vira toras + toco.
- **Mineração:** lascas, tremor/clarão a cada golpe, rachaduras que crescem com o progresso e desmoronamento ao terminar.
- **Ferramentas:** *machado* e *picareta*. Ana começa com machado, Bento com picareta, e há um de cada no chão perto do centro (colonos pegam quando vão cortar/minerar). Com ferramenta: golpe por cima com a ferramenta na mão. Sem ferramenta: soca/puxa a 0,4× da velocidade. O painel de inspeção mostra as ferramentas.
- Animações também para colher (alcançar/puxar), construir (martelar), comer (mão à boca), carregar e dormir.
- **Bancada de trabalho** (15 madeira): em Arquitetar → Fabricar, peça Machado (4 madeira + 2 pedra) ou Picareta (4 madeira + 3 pedra). O colono fica de frente para a bancada e serra, entalha e martela. A peça aparece na bancada e toma forma em etapas (tábuas → cabo → cabeça encaixada → amarração). Há serragem/faíscas (Médio/Alto), pulsos de "toque" em cada golpe, barra de progresso e brilho ao terminar.

### Aba Trabalho (prioridades)
Uma grade com colonos × Corte, Mineração, Coleta, Construção, Fabricação e Transporte.
- Clique para ciclar 1 → 2 → 3 → 4 → desligado (botão direito volta).
- O colono faz primeiro o menor número e, no empate, o trabalho mais perto.
- Fica salvo no estado do jogo.

### Construção (Arquitetar)
- Parede de madeira (5 madeira), Parede de pedra (5 pedra), Cama (10 madeira), Bancada (15 madeira).
- As plantas aparecem como fantasma. Para paredes, arraste: uma linha, ou o contorno de uma sala, que deixa automaticamente um **vão de porta** no meio do lado de baixo.
- Colonos levam o material até a planta e constroem com martelo e barra de progresso.
- Paredes bloqueiam o caminho, se conectam sozinhas e mostram tábuas/tijolos.
- Cama construída ganha dono: o colono dorme nela ("Dormiu numa cama" +4) em vez de "Dormiu ao relento" (−3).
- **X** cancela plantas e designações, ou manda desconstruir uma estrutura pronta (devolve 60% do material).

### Indicadores
Ícones sobre a cabeça quando fome, sono ou humor estão baixos; tecla **I** liga/desliga. A barra de progresso sobre qualquer trabalho aparece sempre.

### Eventos (a cada 1–2 dias de jogo)
Cada evento chega com uma carta no canto direito (clique para fechar) e uma entrada no registro.
- **Chuva:** gotas e respingos, tela mais escura, humor menor para quem está ao ar livre, frutas rebrotam 2× mais rápido.
- **Visitante:** chega pela borda, cumprimenta, conversa e vai embora depois de algumas horas, deixando os colonos um pouco mais felizes.
- Para testar, use `index.html?evento=chuva` ou `?evento=visitante`.

### Limites conhecidos
- Não há portas nem telhados: a chuva afeta todos ao ar livre, inclusive "dentro" de salas.
- Os materiais da fabricação são retirados direto das pilhas, sem transporte até a bancada.
- Não há salvar/carregar em arquivo: "salvo no estado" vale para a partida atual.
- Ferramentas no chão só são pegas por quem vai cortar/minerar.

---

## Novidades da v6 (celular + melhorias)

(A v5 ficou guardada em `v5_index.html`.)

### Celular / tablet (toque)
O modo toque liga sozinho em aparelhos de toque. No PC nada muda. Para forçar: `?toque=1`; para desligar: `?toque=0`.
- **1 dedo arrastando:** move a câmera. **Pinça:** zoom.
- **Tocar:** seleciona um colono ou inspeciona o tile.
- **Segurar ~0,4 s:** manda o colono selecionado até ali (como o botão direito). Num modo de ordem, segurar sai do modo.
- **Cortar / Minerar / Construir / Cancelar / Plantio:** toque ou arraste no mapa para aplicar. Com 2 dedos ainda dá para mover e dar zoom.
- **Barra de botões embaixo** (rola para o lado): Pausa, Velocidade, Cortar, Minerar, Construir, Cancelar, Abas (gaveta com Arquitetar / Trabalho / Agenda / Registro / Opções / Ajuda), Qualidade, próximo Colono, Salvar.
- **Layout:** os painéis se ajustam em retrato e paisagem. A grade de prioridades rola para o lado, e as cartas cabem na tela.
- **Primeira vez no celular:** começa no Baixo (dá para trocar).
- **Desempenho:** a resolução do canvas é limitada no toque (1,5× no Baixo, 2× no Médio/Alto).

**Abrir no Android:** é um arquivo único, sem módulos, sem internet e sem nada externo. Opções:
1. Copie o `index.html` para o celular e abra pelo app Arquivos → "Abrir com" → Chrome.
2. Ou sirva pela rede local: no PC, rode `python -m http.server 8000` na pasta e abra `http://IP-do-PC:8000` no celular.

Obs.: dependendo da versão do Android/Chrome, abrir arquivo local pode não guardar o localStorage. Nesse caso use Exportar/Importar ou a opção 2.

### Comida
- Mais arbustos de frutas, e as frutas rebrotam em 1,5 dia (era 3).
- **Zona de plantio** (Arquitetar ou tecla P; arraste num campo de grama/terra): os colonos semeiam milho, que cresce em ~1,6 dia (2× com chuva, mais rápido em terra fértil). Depois colhem 7 de milho por tile e semeiam de novo.
- Milho é comida, igual às frutas.
- Nova coluna **Cultivo** na aba Trabalho.

### Portas, salas e telhado
- **Porta** (6 madeira): dá passagem, abre e fecha com animação quando alguém passa.
- Arrastar um retângulo de parede agora põe uma **porta** no meio do lado de baixo (em vez de um vão).
- **Telhado automático:** uma sala totalmente fechada por paredes/portas (até 150 tiles) ganha telhado. Quem está sob o telhado não fica "Molhado pela chuva".
- Tecla **T** mostra/esconde o telhado (azul).

### Salvar / carregar
- **Salvamento automático:** a cada 2 minutos e ao fechar ou trocar de aba/app. Ao abrir o jogo de novo, a partida continua (`?seed=` ou `?novo` começa outra).
- **Manual:** K ou Ctrl+S salva, L carrega. Tem também botões em Opções.
- O save guarda o mapa, as construções, as plantas, os itens, os colonos (necessidades, prioridades, ferramentas, camas, memórias), o clima e os pedidos.
- **Levar para outro aparelho** (Opções, ou o botão Salvar no celular):
  - *Exportar arquivo .json* (baixa `colonia_diaN.json`) e *Importar arquivo* (seletor de arquivo; funciona no Android).
  - *Copiar código* (texto compactado, começa com `C6:`) e *Importar código* (cole na caixa).
  - A versão é conferida: saves/códigos de outra versão são recusados com aviso.

### Outros
- Colonos sem ferramenta pegam as que estiverem largadas por perto antes de trabalhar ou quando estão à toa.
- A árvore caindo ficou mais visível: rastro de movimento, sombra e copa mais clara enquanto cai.
- A simulação usa um aleatório próprio (salvo junto): a mesma semente dá a mesma partida em qualquer qualidade gráfica.

### Limites conhecidos (v6)
- Testado em emulação de celular do Chrome (sem aparelho Android real).
- Portas não trancam. O telhado é só lógico (aparece com T), sem desenho de telhado por cima. A chuva continua sendo desenhada dentro de casa.
- Visitantes e animações em andamento não entram no save; madeira de árvore caindo vira tora na hora.
- Em `file://` alguns navegadores bloqueiam "Copiar código": o código aparece na caixa para copiar à mão.

## v7 — arte nova, animações, desempenho e iPhone

- **Arte:** atlas de sprites desenhado em SVG (Inkscape) e embutido no HTML em duas resoluções (Baixo usa o pequeno; Médio/Alto o grande). Inclui 6 espécies de árvore (entre elas Bétula e Bordo), arbustos, fases do plantio, pedras, paredes de madeira/pedra, cama, bancada, ferramentas, itens e ícones. Em **Opções** dá para voltar à arte **procedural** antiga.
- **Animações:** árvore range antes de cair e levanta poeira no impacto; golpes com rastro e "amassada" no impacto; Zs ao dormir.
- **Saves:** versão 7 (lê saves da v6). Código começa com `C7:` (compactado) ou `J7:` (navegadores sem compressão).

### iPhone / iPad (Safari)
O iOS não abre arquivos HTML locais: publique a pasta no **GitHub Pages** (ou outro site https) e abra o link no Safari.
- **Adicionar à Tela de Início:** botão Compartilhar → "Adicionar à Tela de Início". Abre em tela cheia com ícone próprio.
- O zoom da página por pinça/toque duplo e o "elástico" da rolagem ficam bloqueados; o zoom de pinça é só o do jogo. A barra de baixo e os painéis respeitam o notch e a barra de início (safe-area).
- **Exportar** no iPhone abre o menu Compartilhar (Salvar em Arquivos, AirDrop…). **Importar** usa o seletor de arquivos ou o código colado.
- No celular o cache do chão fica limitado a 3200×3200 px, abaixo do limite de memória de canvas do iOS.

## v8 — saúde, medicina, animais selvagens, armas e caçada em grupo

### Saúde (aba **Saúde** no painel do colono)
- Painel com abas **Geral / Saúde**. A aba Saúde mostra Sangue, Dor, Movimento e Trabalho e um **diagrama do corpo** com 4 camadas: **Esqueleto, Músculos, Pele, Roupa**.
- Toque/clique numa parte (cabeça, pescoço, tronco, braços, mãos, pernas, pés) para ver só os ferimentos dela; "(todas)" volta à lista completa.
- Ferimentos: **contusão, corte, mordida, fratura**; sangramento **leve / médio / forte** (perda de sangue, pingos e manchas no chão que somem aos poucos); **infecção** (surge em feridas abertas mal tratadas, piora com o tempo, o corpo reage devagar e o tratamento cura mais rápido); **desmaio**.
- Efeitos: dor, mancar (perna/pé), braço pendurado, trabalho mais lento, caretas de dor, e o colono **cai** quando perde muito sangue ou a dor fica alta demais. Caído, fica deitado até ser resgatado ou melhorar.

### Medicina
- Prioridade **Médico** na aba Trabalho (Iara começa com 1). Médicos tratam automaticamente e tratar vem **antes** do trabalho comum; médicos acordam/levantam para socorrer quem caiu.
- **Bandagens** na bancada: de pele (1 pele → 3) ou de fibra (4 frutas → 1). Com bandagem o tratamento é bem melhor. A colônia pede bandagens sozinha quando há menos de 4.
- Cama ocupada por paciente vira **cama médica** (cobertor azul com cruz); repousar na cama cura mais rápido (×1,7; no chão ×1,3).
- 20 animações de tratamento, escolhidas pelo tipo de ferida e parte do corpo: tratar perna / pé / braço / mão / cabeça / tronco, bandagem na perna / braço / cabeça / tronco, tala na perna / braço, pressionar sangramento, limpar ferida, tratar infecção, examinar (pulso), ajudar a sentar, tratar na cama, e as do paciente: desmaiar e levantar.

### Resgate — 5 jeitos de carregar
Escolhido automaticamente: **no ombro (bombeiro)** para desacordados, **no colo** para paciente leve ou ferido no tronco, **arrastando** se a perna está quebrada ou quem carrega está ferido, **apoio no ombro** para quem manca, **de cavalinho** para pé machucado. Todos com transição suave de pegar e de pôr na cama.

### Animais selvagens
- **Veado**: pasta em manada, foge quando percebe alguém e cansa depois de algumas fugas.
- **Lobo**: matilha de 2–3, caça à noite e vai embora depois de derrubar alguém.
- **Urso**: raro e territorial, ataca quem chega perto da toca.
- Andar, correr, atacar e morrer têm animação. Os ataques ferem partes específicas do corpo.
- Colonos atacados lutam (punhos, ferramenta ou arma) ou fogem, e quem está perto ajuda.
- Animal abatido vira **carcaça**, carneada automaticamente na bancada em **carne** (comida) e **pele**.

### Armas (bancada)
- **Espada**: corpo a corpo, mais dano que o machado.
- **Arco**: à distância. Precisa de **flechas** (madeira → 12). Tem animação de puxar, mirar e soltar, e a flecha voa em arco.
- **Escudo**: bloqueia parte dos golpes, com animação e faíscas.
- Encaixes: arma + escudo. O arco usa as duas mãos, então quem pega arco larga o escudo. Os colonos equipam sozinhos o que estiver no estoque.

### Caçar
- **Caçar (J)**, ou o botão 🎯 no celular: toque ou arraste sobre animais para marcá-los. Colonos com **Caça** ligada vão atrás.
- **Caçada em grupo (U)**: toque num animal ou bando e escolha de 2 a 4 caçadores. O painel já mostra a estratégia:
  - **Emboscada** (se houver arqueiro): arqueiros escondidos atiram primeiro, e o corpo a corpo espera a primeira flechada para investir.
  - **Cerco** (3 ou mais): os caçadores se espalham em arco e fecham juntos.
  - **Flanco**: um batedor se aproxima enquanto o outro dá a volta e corta a fuga.
- A IA planeja as posições **a favor do vento** e atrás de árvores, rochas e arbustos (cobertura). Os caçadores andam **agachados** perto da presa. O animal percebe por visão (menos de noite, com cobertura ou com o caçador agachado), por cheiro (vento contra) e por barulho.
- Todos esperam em posição antes do ataque. Se a presa percebe antes, ela foge (veado) ou investe (lobo/urso).
- No mapa aparecem marcadores das posições planejadas, linhas, papéis ("arqueiro · coberto"), o nome da estratégia e a seta do vento. Tudo vai para o registro.

### Salvar
Save versão **8** (códigos `C8:`/`J8:`). Lê saves e códigos das versões 6 e 7. Saves antigos ganham a fauna inicial e saúde zerada.

### Limites conhecidos (v8)
- **Ninguém morre**: sangue tem piso e infecção sem tratamento acaba sarando devagar. Um colono pode ficar muito tempo caído se não houver médico.
- Algumas animações compartilham código: as 4 bandagens usam a mesma rotina de "voltas" (`orbitaBandagem`) com alvo/raio diferentes, as 2 talas usam `talas`, e desmaiar/levantar/deitado são poses do paciente (`poseDeitada`), não do médico.
- Cozinhar carne não existe (é comida crua como as frutas). Não há armadura nem munição recuperável além das flechas que ficam no chão por um tempo.
- O tempo de luta é em segundos de simulação, então nas velocidades 2×/3× um combate dura "horas" no relógio do jogo.

## v9 — Modo Dev (ferramentas de desenvolvedor)

- **Entrada:** só por **Opções → Desenvolvedor → 🛠 Modo Dev** (no celular/iPhone: botão ⚙/Opções da barra de toque). Sem atalho de teclado.
- **Senha fixa:** o jogo não guarda a senha, só o hash SHA-256 dela. O que você digita passa pelo SHA-256 (UTF-8) e é comparado com esse hash. Usa `crypto.subtle` quando o navegador tem e um SHA-256 em JS puro quando não tem (`file://` ou página não segura). Senha errada: a caixa treme e aparece "Senha errada.". Não dá para criar nem trocar a senha.
- Depois de desbloqueado, o modo continua ligado nesta e nas próximas sessões (flag `colonia.devAtivo` no localStorage) até você usar **⏻ Sair do modo dev**. O selo **MODO DEV** fica no canto inferior esquerdo; tocar nele abre o painel.
- **Painel** (escuro, no estilo debug actions do RimWorld): busca, categorias, funciona no toque, botões de minimizar e fechar. Ações com 📍 pedem um clique ou toque no mapa, e arrastar coloca várias de uma vez. Esc ou botão direito cancela.
  - Animais: veado, lobo, urso, matilha de lobos (3), manada de veados (3).
  - Árvores: carvalho, pinheiro, álamo, bétula, bordo × 3 variantes + aleatória. Plantas: arbusto com ou sem frutas, amadurecer plantações, rebrotar frutas. Terreno: rocha minerável.
  - Itens (com quantidade): madeira, pedra, fruta, milho, carne, pele, flechas, bandagem, machado, picareta, espada, arco, escudo.
  - Colonos: colono extra (visual e prioridades aleatórios, nome opcional; entra no save), curar um ou todos, encher ou zerar necessidades, equipar espada+escudo ou arco+flechas.
  - Ferimentos num colono clicado (tipo, parte do corpo e gravidade): contusão, corte, sangramento leve/médio/forte, fratura, infecção, desmaio.
  - Construções instantâneas: parede de madeira ou pedra (linha ou sala), porta, cama, bancada, concluir todas as plantas.
  - Eventos: chuva liga/desliga, visitante, ataque de lobos agora, matilha na borda, urso, ataque de urso agora, manada de veados.
  - Tempo: +1 h, +6 h, +1 dia (simula de verdade, passo a passo), ir para uma hora.
  - Ferramentas: apagar coisa (animal, visitante, item, colono (nunca o último), construção sem devolver material, árvore, toco, rocha, plantação, zona), ver o mapa inteiro.
  - Modo deus (liga/desliga): construção instantânea, fabricação instantânea, sem fome, sem sono, revelar (rótulos de todos os animais do mapa; o jogo não tem névoa de guerra).
- Toda ação dev aparece no registro como "🛠 DEV: …".
- Saves: a versão continua C8 e saves da v8 carregam normalmente.
