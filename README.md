

<h2>🛠️ Tecnologias que estou utilizando:</h2>
<p>
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Electron-47848F?style=for-the-badge&logo=electron&logoColor=white" alt="Electron">
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript">
</p>

---

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


