# Printseca

Programa que lembra de imprimir, e/ou imprime sozinho, uma página de manutenção a cada poucos dias, para a tinta da impressora não ressecar por falta de uso.

> **Projeto arquivado.** Esta versão de computador não recebe mais atualizações. O Printseca continua na web, direto no navegador e sem instalar nada: [print.jonathasmotta.com](https://print.jonathasmotta.com).

![Release](https://img.shields.io/github/v/release/jonathaxs/printseca-pc?style=for-the-badge&labelColor=f0f0f0&color=f0f0f0)
![macOS](https://img.shields.io/badge/macOS-f0f0f0?logo=apple&logoColor=black&style=for-the-badge)
![Linux](https://img.shields.io/badge/Linux-f0f0f0?logo=linux&logoColor=black&style=for-the-badge)
![Windows](https://img.shields.io/badge/Windows-f0f0f0?logo=windows&logoColor=0078D6&style=for-the-badge)

Impressoras de cartucho e de tanque de tinta que ficam muito tempo paradas podem ressecar e entupir, o que costuma sair caro. O Printseca fica quieto na bandeja do sistema e, quando chega o dia, avisa ou imprime sozinho uma página que passa todas as tintas pela impressora.

## Funcionalidades

**Manutenção**
- Intervalo em dias escolhido pelo usuário.
- Página colorida, que usa ciano, magenta, amarelo e preto, ou página só em preto.
- Escolha da impressora, ou uso da padrão do sistema.

**Dois modos**
- **Avisar:** mostra uma notificação quando a manutenção vence.
- **Automático:** imprime sozinho no dia certo.
- Botões para imprimir agora e para marcar uma impressão feita por fora, que zera a contagem.

**Uso no dia a dia**
- Fica na bandeja do sistema, sem janela aberta e, no macOS, sem ícone no Dock.
- Inicia junto com o computador e abre uma única vez, mesmo se clicado de novo.
- No máximo um aviso a cada 20 horas, para não incomodar.
- Tema claro, escuro ou automático.
- Interface em Português do Brasil e English, conforme o idioma do sistema ou escolhido nas configurações.

## Arquitetura

- **Tauri v2**, com o backend em **Rust** e a janela em **TypeScript** puro com **Vite**, sem framework de interface e sem Electron.
- O backend é o dono do estado. Cada comando devolve o estado já atualizado, e a janela só desenha o que recebe.
- O agendador roda numa thread própria e confere a cada 30 minutos se a manutenção venceu, com base em datas salvas. Não depende de o computador ficar ligado no horário exato.
- Impressão pelo próprio sistema: `lp` do CUPS no macOS e no Linux, e o SumatraPDF, empacotado junto com o app, no Windows.
- As páginas de manutenção são PDFs em CMYK, para forçar o uso de cada tinta.
- Configuração salva em um arquivo JSON, sem banco de dados.
- Traduções próprias, no Rust para a bandeja e as notificações e no TypeScript para a janela.

## Estrutura do repositório

```text
printseca/
├── src/                       Janela: lógica, traduções e estilos
├── src-tauri/
│   ├── src/                   Backend: bandeja, agendador, comandos, impressão e traduções
│   ├── resources/             PDFs de manutenção, colorido e preto
│   ├── icons/                 Ícones do app e da bandeja por sistema
│   └── tauri.conf.json        Configuração do app e dos pacotes
├── scripts/                   Gerador dos PDFs em CMYK
├── packaging/macos/           Empacotamento do macOS com guia de abertura
├── branding/                  Arquivos fonte do ícone
└── index.html
.github/workflows/release.yml  Build e publicação dos instaladores
```

## Downloads

Os instaladores da última versão estão em [Releases](https://github.com/jonathaxs/printseca-pc/releases/latest):

| Sistema | Formato |
|---|---|
| macOS | `.zip` com o app e um guia de abertura (assinatura ad-hoc, sem notarização da Apple) |
| Windows | Instalador `.exe` |
| Linux | `.deb`, `.rpm` e `.AppImage` |

Os cinco arquivos são gerados pelo GitHub Actions nas três plataformas, a partir de uma tag de versão, sem nenhum passo manual.

## Executar pelo código

Com o Node.js e o Rust instalados:

```bash
cd printseca
npm install
npm run tauri dev
```

No Windows, a impressão precisa do `SumatraPDF.exe` em `src-tauri/resources/`. No build de release, o GitHub Actions baixa e inclui esse arquivo.

## Autor

Desenvolvido por **Jonathas Motta** ([@jonathaxs](https://github.com/jonathaxs)). Mais projetos em [jonathasmotta.com](https://jonathasmotta.com).
