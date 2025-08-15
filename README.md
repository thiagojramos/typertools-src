# Typer Tools (CEP para Photoshop)

Extensão CEP (HTML/JS) para Photoshop com UI em React, build em Webpack e scripts ExtendScript para automações de texto.

## Como construir

- Desenvolvimento (com source maps):
  - `npm run build_dev`
- Observando alterações:
  - `npm run build_watch`
- Produção (minificado):
  - `npm run build`

A saída é gerada em `./app` (index.html, index.js, host.jsx, CSS, assets).

## Instalação (CEP)

1. Ative o modo de desenvolvedor CEP.
   - Windows (PowerShell como Admin):
     - Defina a chave de registro `HKCU\Software\Adobe\CSXS.9` (ou versão instalada) `PlayerDebugMode = 1`.
   - macOS:
     - `defaults write com.adobe.CSXS.9 PlayerDebugMode 1` (ajuste o sufixo da versão conforme o CEP do seu Photoshop).
2. Copie a pasta do projeto (ou apenas a pasta `app` com `CSXS/manifest.xml` e `icons`) para a pasta de extensões CEP do sistema:
   - Windows: `%AppData%/Adobe/CEP/extensions/typertools`
   - macOS: `~/Library/Application Support/Adobe/CEP/extensions/typertools`
3. Reinicie o Photoshop e abra em Janela > Extensões > Typer Tools.

Observação: este repositório inclui utilitários para empacotar/assinar (`pack.zxp.cmd`, `ZXPSignCmd.exe`).

## Localização (i18n)

Os textos estão em `locale/` e são carregados via `CSInterface.initResourceBundle()`.

## Pontos de melhoria sugeridos

- Migração para UXP (futuro): CEP está obsoleto. Considere portar gradualmente para UXP, mantendo CEP como fallback até a transição.
- Armazenamento: em `app_src/utils.js`, atualmente grava em `SystemPath.EXTENSION`. Prefira `SystemPath.USER_DATA` e um arquivo `storage.json` por usuário, evitando permissões somente leitura.
- Dependências/Build:
  - Trocar `node-sass` por `sass` (Dart Sass) e atualizar `sass-loader` se necessário.
  - Substituir `babel-eslint` por `@babel/eslint-parser`.
  - Considerar módulos de assets nativos do Webpack 5 no lugar de `file-loader`/`url-loader`.
  - Remover `babel-polyfill` (deprecated) e usar `core-js` + `regenerator-runtime` se ainda necessário.
- Lint/Format:
  - Adicionar configuração do ESLint e Prettier com hooks de pre-commit (lint-staged/husky) para padronizar código.
- CI/CD:
  - Workflow no GitHub Actions para `npm ci && npm run build` e artefatos do pacote (ZIP/ZXP). Opcional: assinar ZXP em CI.
- UX/Perf:
  - Fonts: o carregamento assíncrono de `getUserFonts()` pode retornar lista vazia no primeiro render. Propagar via estado/contexto e exibir feedback de "Carregando fontes".
  - Melhorias de acessibilidade (foco, labels, contraste) e atalhos.
- Scripts ExtendScript:
  - Centralizar conversões/unidades e documentar IDs (charIDToTypeID/stringIDToTypeID) para manutenção.
  - Pequenas verificações extras de erro antes de `jamText.setLayerText`.
- Sincronização de versão:
  - Alinhar a versão do `manifest.xml` com `package.json` (script de bump automatizado).

## Licença

MIT (veja `LICENSE.md`).
