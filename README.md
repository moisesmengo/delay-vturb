# Tutorial Delay Vturb

## Passo 1: Editar o Vídeo

1. Vá até o vídeo que deseja configurar e clique em **EDITAR**.

## Passo 2: Configurar Botão de Ação

1. Na barra lateral, procure e clique na opção **BOTÃO DE AÇÃO**.
2. Clique no botão **ADICIONAR NOVO BOTÃO**.
3. Configure apenas o tempo de início e marque a opção **continuar exibindo após término do vídeo**. Não é necessário configurar texto, tamanho e link.
4. Salve as alterações.

## Passo 3: Obter Código Embed

1. Clique em **Embed** e copie o código do vídeo.

## Passo 4: Inserir Código Embed no HTML

1. Cole o conteúdo do embed no seu código HTML onde deseja que o vídeo seja exibido. Exemplo:

    ```html
    <!-- código de embed do vídeo -->
    <div 
      id="vid_658b5f6b09a2c30009fbc909" 
      style="position: relative; width: 100%; padding: 56.25% 0 0"
    >
      <img 
        id="thumb_658b5f6b09a2c30009fbc909" 
        src="https://images.converteai.net/bff5868f-b8e9-4bb4-af45-52d9b7681a6e/players/658b5f6b09a2c30009fbc909/thumbnail.jpg" 
        style=" 
          position: absolute; 
          top: 0; 
          left: 0; 
          width: 100%; 
          height: 100%; 
          object-fit: cover; 
          display: block; 
        " 
      />
      <div 
        id="backdrop_658b5f6b09a2c30009fbc909" 
        style=" 
          position: absolute; 
          top: 0; 
          width: 100%; 
          height: 100%; 
          -webkit-backdrop-filter: blur(5px); 
          backdrop-filter: blur(5px); 
        "
      ></div>
    </div>
    <script type="text/javascript" id="scr_658b5f6b09a2c30009fbc909">
      var s = document.createElement("script"); 
      s.src = 
        "https://scripts.converteai.net/bff5868f-b8e9-4bb4-af45-52d9b7681a6e/players/658b5f6b09a2c30009fbc909/player.js"; 
      s.async = true; 
      document.head.appendChild(s); 
    </script>
    ```

## Passo 5: Ocultar Conteúdo

1. Todo o conteúdo que deve ficar oculto até que o vídeo chegue no tempo determinado deve ficar dentro de uma tag `section` (ou `div`, `article`) e conter nela o atributo `data-delay`. Exemplo:

    ```html
    <section data-delay>
      <div>Conteúdo escondido</div>
      <button>Comprar</button>
    </section>
    ```

## Passo 6: Adicionar Script no Final do Body

1. Cole o script a seguir no final do `body` de sua página:

    ```html
    <script> 
      document.addEventListener("DOMContentLoaded", function () { 
        // Função para mostrar todas as seções ocultas 
        function showHiddenSections() { 
          console.log("Exibindo as seções ocultas"); 
          const hiddenSections = document.querySelectorAll("[data-delay]"); 
          hiddenSections.forEach((section) => { 
            section.style.display = "block"; 
          }); 
        }

        // Função para observar mudanças na classe e estilo de um elemento 
        function observeClassAndStyleChanges(element) { 
          const observer = new MutationObserver((mutations) => { 
            mutations.forEach((mutation) => { 
              if ( 
                mutation.attributeName === "class" || 
                mutation.attributeName === "style" 
              ) { 
                const display = window.getComputedStyle(element).display; 
                const isHidden = element.classList.contains("smartplayer-hide"); 
                console.log( 
                  `Mudança detectada: display = ${display}, isHidden = ${isHidden}` 
                ); 
                if (display !== "none" && !isHidden) { 
                  showHiddenSections(); 
                  observer.disconnect(); // Parar de observar após exibir as seções 
                } 
              } 
            }); 
          });

          observer.observe(element, { attributes: true }); 
        }

        // Função para encontrar e observar o contêiner do botão 
        function findAndObserveButtonContainer() { 
          const delayButtonContainer = document.querySelector( 
            '[id^="callaction_"][class*="smartplayer-call-action"]' 
          ); 
          if (delayButtonContainer) { 
            console.log("Contêiner do botão encontrado"); 
            observeClassAndStyleChanges(delayButtonContainer); 
            clearInterval(findButtonInterval); // Parar de verificar após encontrar o contêiner 
          } else { 
            console.log( 
              "Contêiner do botão não encontrado, tentando novamente..." 
            ); 
          } 
        }

        // Verificar a presença do contêiner do botão a cada 500ms 
        const findButtonInterval = setInterval( 
          findAndObserveButtonContainer, 
          500 
        ); 
      }); 
    </script>
    ```

## Passo 7: Adicionar Código de Estilo

1. Cole o código de estilo abaixo dentro do `head` de sua página:

    ```html
    <style> 
      .elementor-element:has(#smartplayer) { 
        width: 100%; 
      } 
      [data-delay] { 
        display: none; /* Seção oculta inicialmente */ 
      }

      .smartplayer-call-action { 
        transform: scale(0); 
        overflow: hidden; 
        padding: 0 !important; 
        margin: 0; 
        width: 0px; 
        height: 0px; 
      } 
    </style>
    ```
