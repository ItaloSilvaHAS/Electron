

<h2>⚡ Passo a Passo para Rodar o Projeto no Electron:</h2>

1. **Acessar o site do Node.js:**
   - Visite: [https://nodejs.org/pt](https://nodejs.org/pt)
   - Baixe a versão **LTS** (mais estável).
   - Para verificar se deu certo, rode os seguintes comandos no terminal:
     ```bash
     node -v
     npm -v
     ```

2. **Criar a pasta do projeto:**
   - Abra o terminal e digite os seguintes comandos:
     ```bash
     mkdir nome-do-projeto
     cd nome-do-projeto
     ```

3. **Inicializar o projeto Node.js:**
   - Execute o comando:
     ```bash
     npm init -y
     ```

4. **Instalar o Electron:**
   - No terminal, instale o Electron como dependência de desenvolvimento:
     ```bash
     npm install electron --save-dev
     ```

5. **Criar os arquivos principais:**
   - Crie os arquivos `main.js` e `index.html` dentro da pasta do projeto. O arquivo `main.js` será o responsável por inicializar a janela do Electron, enquanto o `index.html` será o conteúdo da página carregada.

6. **Adicionar o script para rodar o Electron:**
   - Abra o `package.json` e, na seção `scripts`, adicione o seguinte:
     ```json
     "scripts": {
       "start": "electron ."
     }
     ```

7. **Rodar o projeto:**
   - No terminal, execute:
     ```bash
     npm start
     ```
   - Isso vai abrir uma janela com o HTML carregado, mostrando seu primeiro app feito com Electron!

---

<h2>📥 Exemplo de Código:</h2>

### **HTML com script incluso:**
```
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Meu Navegador</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
      * {
        box-sizing: border-box;
      }

      body {
        margin: 0;
        font-family: 'Inter', sans-serif;
        background-color: #0f172a;
        color: white;
        height: 100vh;
        display: flex;
        flex-direction: column;
      }

      #drag-region {
        -webkit-app-region: drag;
      }
    </style>
  </head>
  <body>
    <!-- Barra de título arrastável -->
    <div id="drag-region" class="h-8 bg-slate-800 w-full"></div>

    <!-- Interface principal -->
    <div class="flex h-full">
      <!-- Barra lateral -->
      <aside class="w-20 bg-slate-900 flex flex-col items-center py-4 space-y-4">
        <button
          onclick="navegarPara('https://web.whatsapp.com')"
          class="hover:bg-slate-700 p-2 rounded"
        >
          <img src="https://cdn-icons-png.flaticon.com/512/733/733585.png" class="w-8 h-8" />
        </button>
        <button
          onclick="navegarPara('https://chat.openai.com')"
          class="hover:bg-slate-700 p-2 rounded"
        >
          <img src="https://cdn-icons-png.flaticon.com/512/1045/1045946.png" class="w-8 h-8" />
        </button>
      </aside>

      <!-- Conteúdo principal -->
      <main class="flex-1 flex flex-col">
        <!-- Barra de pesquisa e ações -->
        <div class="bg-slate-800 p-3 flex gap-2">
          <input
            id="barra"
            type="text"
            placeholder="Digite uma URL ou termo de pesquisa"
            class="flex-1 px-4 py-2 rounded bg-white text-black focus:outline-none"
          />
          <button
            onclick="pesquisar()"
            class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded"
          >Buscar</button>
          <button
            onclick="toggleModoClaro()"
            class="bg-gray-600 hover:bg-gray-700 text-white px-4 py-2 rounded"
          >Modo Claro</button>
        </div>

        <!-- Navegação por abas (futuro) -->
        <div id="abas" class="bg-slate-700 px-3 py-1 text-sm flex gap-3"></div>
      </main>
    </div>

    <script>
      const { ipcRenderer } = require('electron');

      function pesquisar() {
        const barra = document.getElementById('barra');
        let valor = barra.value.trim();

        if (!valor) return;

        if (!/^https?:\/\//.test(valor)) {
          valor = 'https://www.google.com/search?q=' + encodeURIComponent(valor);
        }

        ipcRenderer.send('navegar-para', valor);
      }

      function navegarPara(url) {
        ipcRenderer.send('navegar-para', url);
      }

      function toggleModoClaro() {
        ipcRenderer.send('modo-claro');
      }
    </script>
  </body>
</html>

```

<br>
<br>

<h2>🔥 main.js:</h2>

### **Main.js:**
```
const { app, BrowserWindow, BrowserView, ipcMain } = require("electron");
const path = require("path");

let mainWindow;
let view;
let modoClaroAtivo = false;

function createWindow() {
  mainWindow = new BrowserWindow({
    width: 1200,
    height: 800,
    webPreferences: {
      preload: path.join(__dirname, "preload.js"),
      nodeIntegration: true,
      contextIsolation: false,
    },
  });

  mainWindow.loadFile("index.html");

  view = new BrowserView({
    webPreferences: {
      nodeIntegration: false,
      contextIsolation: true,
    },
  });

  mainWindow.setBrowserView(view);
  view.setBounds({ x: 0, y: 100, width: 1200, height: 700 });
  view.setAutoResize({ width: true, height: true });
}

ipcMain.on("navegar-para", (event, url) => {
  if (!/^https?:\/\//.test(url)) url = "https://" + url;
  view.webContents.loadURL(url);
});

ipcMain.on("modo-claro", () => {
  if (!view) return;
  modoClaroAtivo = !modoClaroAtivo;

  const cssClaro = `
    html, body {
      background: white !important;
      color: black !important;
    }
    * {
      background-color: white !important;
      color: black !important;
      border-color: #ccc !important;
    }
  `;

  if (modoClaroAtivo) {
    view.webContents.insertCSS(cssClaro);
  } else {
    view.webContents.reload(); // volta ao modo original
  }
});

app.whenReady().then(createWindow);

