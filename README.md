<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/banner-dark.svg">
  <img src="assets/banner-light.svg" alt="Vitor Tosti" width="100%">
</picture>

<p align="center">
  <a href="https://www.linkedin.com/in/vitortosti/"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-vitortosti-0A66C2?style=flat-square&logo=linkedin&logoColor=white"></a>
  <img alt="UFABC" src="https://img.shields.io/badge/Ci%C3%AAncia%20da%20Computa%C3%A7%C3%A3o-UFABC-1D2129?style=flat-square">
  <img alt="São Bernardo do Campo" src="https://img.shields.io/badge/S%C3%A3o%20Bernardo%20do%20Campo-SP-1D2129?style=flat-square">
</p>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/stats-dark.svg">
  <img src="assets/stats-light.svg" alt="15 usuários em 4 perfis no InfoCAIJ · 8.207 movimentações conciliadas com o ERP · instalação de SO de 90 para 5 minutos · 130+ arquivos de teste" width="100%">
</picture>

## Projetos

<table>
  <tr>
    <td width="50%">
      <a href="https://github.com/VitorTosti/infocaij-case-study">
        <picture>
          <source media="(prefers-color-scheme: dark)" srcset="assets/card-infocaij-dark.svg">
          <img src="assets/card-infocaij-light.svg" alt="InfoCAIJ: venda, bancada, estoque, nota fiscal e garantia em uma fila rastreável, integrada ao ERP. Cerca de 300 máquinas e pedidos por mês." width="100%">
        </picture>
      </a>
    </td>
    <td width="50%">
      <a href="https://github.com/VitorTosti/infomachine">
        <picture>
          <source media="(prefers-color-scheme: dark)" srcset="assets/card-infomachine-dark.svg">
          <img src="assets/card-infomachine-light.svg" alt="InfoMachine: triagem de notebooks com leitura de hardware, 5 testes, histórico por serial e etiqueta. Cerca de 100 máquinas por semana." width="100%">
        </picture>
      </a>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <a href="https://github.com/VitorTosti/CoreUFABC">
        <picture>
          <source media="(prefers-color-scheme: dark)" srcset="assets/card-coreufabc-dark.svg">
          <img src="assets/card-coreufabc-light.svg" alt="CoreUFABC: histórico do SIGAA vira métricas, progresso por eixo e simulação de grade. 66 testes automatizados." width="100%">
        </picture>
      </a>
    </td>
    <td width="50%">
      <picture>
        <source media="(prefers-color-scheme: dark)" srcset="assets/card-fichas-dark.svg">
        <img src="assets/card-fichas-light.svg" alt="Em construção: extração estruturada de fichas técnicas com LLM, com taxa de acerto medida." width="100%">
      </picture>
    </td>
  </tr>
</table>

## Por dentro do InfoMachine

<table>
  <tr>
    <td width="33%"><img src="assets/painel.png" alt="Painel do atendimento com leitura, rastreador, testes e etapas"></td>
    <td width="33%"><img src="assets/testes.png" alt="Central com cinco testes de hardware"></td>
    <td width="33%"><img src="assets/etiqueta.png" alt="Prévia da etiqueta térmica de 80 × 50 mm"></td>
  </tr>
  <tr>
    <td align="center"><sub>Andamento do atendimento</sub></td>
    <td align="center"><sub>Testes de hardware</sub></td>
    <td align="center"><sub>Etiqueta antes da impressão</sub></td>
  </tr>
</table>

## Como os sistemas se conectam

```mermaid
flowchart LR
    T(["Técnico na bancada"]) --> IM["InfoMachine<br/>app no pendrive"]
    V(["Vendedores · estoque · gestão"]) --> IC["InfoCAIJ<br/>plataforma web"]
    IM -- "serial e rastreador" --> ERP[("ERP<br/>AlterTag/VHSYS")]
    IC <-- "OS · pedidos · NF · estoque" --> ERP
    IM -- "ficha técnica" --> IC
    IM -- "impressão" --> NAS["Serviço no NAS"]
    NAS --> ET[/"Etiqueta com QR Code"/]
    IC --> VPS[("VPS<br/>banco + backup")]
```

## Decisões técnicas

<details>
<summary><b>InfoCAIJ</b> · da mensagem no WhatsApp para uma fila com 8 etapas</summary>
<br>

- **Quantidade vira unidade na entrada.** "4 notebooks" viram quatro itens, cada um com serial, OS, técnico e histórico próprios.
- **Estado técnico separado do comercial.** Uma máquina pode estar entregue na bancada e devolvida no comercial sem regras contraditórias, e o histórico sobrevive à revenda.
- **Respostas antigas do ERP são descartadas.** A consulta da OS dispara 400 ms após a digitação, e uma resposta atrasada não sobrescreve a atual.
- **Conciliação sem palpite.** 8.207 movimentações de estoque comparadas por serial; casos ambíguos vão para revisão manual.
- **Migração com caminho de volta.** Do computador local para uma VPS, com hash do banco, ambiente de teste e plano de retorno.

[Estudo de caso completo →](https://github.com/VitorTosti/infocaij-case-study)
</details>

<details>
<summary><b>InfoMachine</b> · leitura automática como sugestão, não como verdade</summary>
<br>

- **Todo campo lido pode ser corrigido.** BIOS e Windows erram tipo de disco e memória; o técnico confirma antes de imprimir.
- **A decisão ambígua fica com quem está com a máquina.** Mais de um rastreador para a mesma serial vira uma escolha, não um palpite.
- **Atualização verificável.** Pacotes no NAS com SHA-256 e backup antes da troca, para que todos os pendrives rodem a mesma versão.
- **Diagnóstico por camada.** A falha de impressão é separada entre NAS, publicação do QR e impressora.

[Repositório →](https://github.com/VitorTosti/infomachine)
</details>

<details>
<summary><b>CoreUFABC</b> · regras acadêmicas com fonte e teste</summary>
<br>

- **Parser tolerante ao PDF.** Junta linhas quebradas do histórico do SIGAA e normaliza para um schema JSON.
- **Métricas com fonte normativa.** CR, CA e CPk seguem o Guia da Graduação; a métrica própria é marcada como tal.
- **Privacidade no relato de erro.** O relato anônimo mantém só a estrutura da linha, sem nome, RA ou conceitos.

[Repositório →](https://github.com/VitorTosti/CoreUFABC)
</details>

<details>
<summary><b>Como trabalho</b></summary>
<br>

1. Levanto o problema com quem vai usar: regras, exceções e o que conta como pronto.
2. Implemento com assistentes de código, como o Codex, revisando cada mudança.
3. Escrevo testes para o comportamento que importa antes de publicar.
4. Valido no uso real e itero a partir do que acontece na operação.
</details>

## Em construção

| Projeto | Objetivo |
|:--|:--|
| **Extração estruturada de fichas técnicas** | Descrições livres de hardware em campos padronizados, com validação de esquema e taxa de acerto medida |
| **Laboratório de avaliação de prompts** | Prompts e modelos comparados na mesma tarefa, com dataset fixo e análise de erros |
| **Assistente com respostas fundamentadas** | Respostas a partir de documentos técnicos, com fonte citada e recusa sem evidência |

## Ferramentas

<p>
  <img alt="TypeScript, JavaScript, Python, React, Node.js, Express, Tailwind CSS, PostgreSQL, SQLite, Supabase, PowerShell, Linux, Git, GitHub Actions, Cloudflare" src="https://skillicons.dev/icons?i=ts,js,py,react,nodejs,express,tailwind,postgres,sqlite,supabase,powershell,linux,git,githubactions,cloudflare&perline=16">
</p>
