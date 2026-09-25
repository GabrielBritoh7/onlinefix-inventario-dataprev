# 📦 Inventário TI — Sistema de Cadastro e Controle Patrimonial

Aplicação web progressiva (**PWA**) para triagem e inventário físico de computadores. Várias pessoas podem cadastrar ao mesmo tempo, cada uma no seu celular, e **tudo vai para uma única planilha compartilhada da equipe** (Google Sheets). Sem internet, o app continua funcionando e envia os registros quando a conexão volta.

> **Custo zero:** a "nuvem" é uma planilha do Google + um pequeno script (Google Apps Script) publicado de graça. O site continua hospedado no Netlify ou GitHub Pages

---

## 🎯 Visão Geral

O **Inventário TI** foi projetado para resolver a lentidão e os erros comuns no levantamento manual de parques de equipamentos (como estações de trabalho e desktops Daten, Dell, Lenovo, etc.). 

A ferramenta funciona diretamente no navegador do smartphone ou computador, com leitura de códigos de barras pela câmera, validação de duplicidades em toda a equipe, fila offline no aparelho e sincronização com a planilha compartilhada.

---

## ✨ Principais Funcionalidades

- 📷 **Leitura de Código de Barras pela Câmera:** Utiliza a API nativa `BarcodeDetector` para leitura rápida de etiquetas de patrimônio e número de série, com opção alternativa de digitação manual.
- 🔢 **Tratamento Automático de Patrimônio:** Normalização inteligente que extrai apenas os 6 dígitos úteis do patrimônio quando o leitor retorna etiquetas de 8 dígitos (descartando o prefixo padrão `39`).
- 🚫 **Bloqueio de Duplicidades:** Alerta imediato e impedimento de cadastro caso um patrimônio ou número de série já tenha sido registrado anteriormente.
- 📊 **Painel com Indicadores:** Contadores automáticos no topo da tela exibindo:
  - Total registrado (na planilha da equipe, quando configurada)
  - Quantidade em estado funcional (OK)
  - Quantidade com defeito
- 🔍 **Busca Instantânea:** Filtro em tempo real por patrimônio, número de série, marca ou modelo.
- ✏️ **Edição e Exclusão:** Gerenciamento completo de registros existentes direto da lista de equipamentos.
- 📥 **Exportação para Planilha (.CSV):** Geração de arquivo CSV formatado com codificação UTF-8 com BOM e delimitador `;`, abrindo perfeitamente no Microsoft Excel e Google Planilhas.
- 📱 **Instalação como Aplicativo (PWA):** Pode ser instalado na tela inicial do celular ou do desktop e funciona **mesmo sem conexão com a internet** (offline).

---

## 📂 Estrutura de Arquivos

```text
DATENBOMBA-inventario-ti/
├── index.html            # Estrutura visual, formulários, modais e telas
├── config.js             # URL da planilha da equipe e chave (preencher após publicar o Apps Script)
├── app.js                # Lógica da aplicação, scanner, filtros, validações, fila offline e sincronização
├── apps-script/
│   └── Codigo.gs         # Backend gratuito: cole no Apps Script da planilha do Google
├── styles.css            # Folha de estilos moderna, tema escuro na barra e layout responsivo
├── sw.js                 # Service Worker responsável pelo cache e funcionamento offline
├── manifest.webmanifest  # Configuração para instalação como PWA (nome, ícone, tema)
├── icon.svg              # Ícone vetorial da aplicação
├── icon-192.png / icon-512.png / icon-maskable-512.png  # Ícones para instalação no Android
└── README.md             # Esta documentação completa do projeto
```

---

## 💻 Tecnologias Utilizadas

- **HTML5 Semântico:** Estrutura acessível com suporte a diálogos modais e tags de formulário nativas.
- **CSS3 Moderno:** Flexbox, CSS Grid, variáveis customizadas (*design tokens*) e media queries para celulares e telas maiores.
- **JavaScript Vanilla (ES6+):** Código leve, sem frameworks pesados, garantindo carregamento instantâneo.
- **Web APIs:**
  - `localStorage`: Persistência local no navegador do cliente.
  - `BarcodeDetector API` & `getUserMedia`: Acesso à câmera do dispositivo e decodificação de códigos de barras (Code 128, Code 39, EAN, QR Code, etc.).
  - `Service Worker API` & `Cache API`: Cache de ativos estáticos para suporte offline.

---

## 📱 Como Usar no Celular (Passo a Passo)

1. **Acesso:** Abra o link HTTPS gerado (pelo GitHub Pages) no Google Chrome ou Safari do celular.
2. **Instalação:** 
   - No Chrome: toque nos três pontinhos superiores e selecione **Adicionar à tela inicial** ou toque no botão **Instalar** no topo da página.
   - O aplicativo criará um ícone igual a um app nativo no celular.
3. **Cadastro:**
   - Preencha o Patrimônio ou toque em **Escanear** para acionar a câmera e ler a etiqueta.
   - Preencha o Número de Série (ou escaneie).
   - Selecione a Marca (**Daten**, Dell, Lenovo, HP, etc.) e digite o Modelo.
   - Indique o **Estado** (Funcionando / Com defeito) e a **Situação** (Não formatado / Em formatação / Formatado).
   - Marque os componentes encontrados e adicione laudos/observações se necessário.
   - Toque em **Salvar computador**.
4. **Listagem e Busca:**
   - Alterne para a aba **Equipamentos** para consultar o que já foi registrado ou buscar por qualquer termo.

---

## 📊 Exportação e Manipulação de Dados

1. Acesse a aba **Equipamentos**.
2. Clique no botão **Exportar CSV** (funciona offline; para .xlsx, baixe direto da planilha do Google).
3. O navegador fará o download imediato de um arquivo nomeado no padrão:
   `estoque_computadores_AAAA-MM-DD.csv`
4. **Campos do CSV:**
   `ID; Patrimônio; Número de série; Marca; Modelo; Estado; Situação; Componentes; Observações / laudo; Última atualização`
5. **Compatibilidade:** O arquivo já inclui o caractere de controle `\uFEFF` (BOM UTF-8), garantindo que acentos e caracteres especiais abram corretamente no Excel sem desconfigurar o texto.

---

## 🔒 Segurança e Privacidade

- **Onde ficam os dados:** na planilha do Google da conta que criou o script, e uma cópia de trabalho no navegador de cada aparelho (para funcionar offline).
- **Quem vê a planilha:** só quem for convidado nela pelo botão Compartilhar do Google Sheets.
- **Chave da equipe:** impede gravações de quem não tem o link completo. Como o site é público, ela não é uma senha forte. Registre apenas patrimônio, série e laudo técnico; não coloque dados pessoais.
- **Política da empresa:** confirme com a área de segurança da informação se o uso de uma conta Google para este inventário é permitido. Se for exigido Microsoft 365, veja a seção de limitações.
- **Sem nuvem:** deixando `config.js` e as Configurações em branco, o app volta a funcionar 100% local, como antes.

---

## ⚠️ Limitações conhecidas

- **Excel no SharePoint/OneDrive:** um site estático não consegue gravar direto numa planilha do SharePoint sem um registro de aplicativo no Azure AD (depende do administrador do tenant) ou um fluxo do Power Automate com gatilho HTTP (licença Premium). Por isso a opção gratuita usa Google Sheets; a planilha pode ser baixada em .xlsx a qualquer momento.
- **Leitura de código de barras:** usa a API `BarcodeDetector`, disponível no Chrome/Edge para Android. No iPhone (Safari) a leitura automática não existe; a digitação manual continua funcionando.
- **Edição simultânea do mesmo registro:** vale a última alteração salva.
