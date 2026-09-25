<p align="center"><img src="Banner.png" alt="NoobForge" width="720"></p>

# NoobForge 1.0.8

Preparação de assets do 3ds Max para Unreal Engine: colisões simples, pivots por peça e exportação FBX em lote.

Desenvolvido por **NoobDev** — [GitHub](https://github.com/FernandoSilvaC).

## Instalação

1. Baixe `NoobForge_Installer_1.0.8.mzp` no [repositório público de releases](https://github.com/FernandoSilvaC/NoobForge-Release/releases).
2. Arraste o arquivo para uma viewport do 3ds Max.
3. Para adicionar à toolbar, procure a categoria **NoobViz**, ação **NoobForge_Open**.

O instalador permite limpar versões anteriores. A instalação é por usuário e a ferramenta abre sem reiniciar o Max. Não extraia nem renomeie o MZP.

Validação automatizada realizada no **3ds Max 2026**. Outras versões precisam de teste específico; este README não declara compatibilidade já comprovada com todas elas.

## Ferramentas

### Asset / Pivot

- Nomenclatura `SM_`, com atualização das colisões associadas.
- Pivot na base, centro ou centro X/Y de cada peça com Z=0 mundial.
- Reset XForm + Collapse, Auto Smooth, unificação de normais e limpeza de geometria.
- Helpers para sockets.

Renomeação, pivots, Reset XForm e shading alteram os objetos selecionados. Salve a cena antes de operações que colapsam a stack.

### Colisões

- Caixas orientadas `UBX_`, esferas `USP_`, cápsulas `UCP_` e Convex Hull `UCX_`.
- Modo Auto e presets para mobiliário, arquitetura, orgânicos e objetos físicos.
- Preview editável antes da aceitação.
- Gerar um preview não descarta previews de outros assets.
- Aceitar/descartar atua nos previews selecionados ou nos previews do asset selecionado.
- A substituição valida os previews e prepara as novas colisões antes de remover as anteriores.
- Novas colisões registram o identificador do objeto; colisões antigas usam correspondência estrita de nomes.

A decomposição e as métricas são heurísticas. Revise espaços de circulação, assentos e vazios importantes. Cobertura é amostrada sobre a malha de trabalho; volume acumulado não desconta sobreposições entre colisores. Após merge de cenas distintas, confira os vínculos e regenere as colisões quando necessário.

### Exportação FBX

- Um FBX por asset, por seleção ou por todos os assets `SM_` da cena.
- Inclusão de colisões e sockets, validação prévia, subpastas e relatório CSV.
- Exportação por cópias temporárias, preservando a cena original.
- Bloqueio de nomes de destino duplicados antes de gravar o lote.
- Opção **Pivot como origem** para respeitar o pivot escolhido.
- Opção **Cada peça: centro X/Y e Z=0** para preservar a altura de telhados e outros objetos elevados.
- CSV com pivots mundiais e unidades para auxiliar na recomposição da montagem.

Arquivos separados não recompõem automaticamente a montagem na Unreal. A importação e o posicionamento na engine ainda devem ser conferidos.

### Materiais

A preparação/conversão de materiais foi removida. Use sua ferramenta de materiais antes da exportação. A coleta opcional apenas copia imagens encontradas; não faz bake nem conversão de shaders. Texturas diferentes com o mesmo nome ainda exigem revisão.

## Atualizações

O cliente consulta exclusivamente:

`https://api.github.com/repos/FernandoSilvaC/NoobForge-Release/releases/latest`

- Verificação automática com intervalo de 24 horas após consulta bem-sucedida.
- Botão **Atualizar** para consulta manual.
- Consulta assíncrona com prazo de 20 segundos; download com prazo de 180 segundos.
- Seleção do instalador pela lista de assets, sem depender da ordem dos campos JSON.
- Validação de origem, versão, SHA-256 e estrutura do MZP antes de executar.
- Falhas de download não executam o instalador.
- Confirmação de conclusão pelo instalador, sem tratar erro interno como sucesso.

O update exige um release público publicado, não draft/prerelease, contendo exatamente um `NoobForge_Installer_VERSAO.mzp` com digest SHA-256 fornecido pelo GitHub. Recomenda-se tag `v1.0.8` para esta versão. Anexar o arquivo a um commit ou deixar o release em rascunho não o disponibiliza pelo endpoint `latest`.

A nova rotina só passa a valer depois que a 1.0.8 estiver instalada. Usuários com um atualizador antigo defeituoso podem precisar instalar este MZP manualmente uma vez.

## Novidades da 1.0.8

- Interface por abas, botões secundários nativos e destaque discreto nas ações principais.
- Remoção do módulo de materiais/bake.
- Exportação de peças elevadas com origem no piso e preservação de altura.
- Proteções contra associação por prefixos semelhantes, previews globais e destinos duplicados.
- Encaixe envolvente de esferas/cápsulas, métricas de hull e unidades corrigidas.
- Download de updates com prazo limite, leitura estruturada do JSON e confirmação da instalação.
- Testes de regressão de segurança e do pacote protegido.

## Projeto e build

O [repositório privado](https://github.com/FernandoSilvaC/NoobForge) mantém fontes, testes e ferramentas de build. O repositório público recebe somente a distribuição protegida.

- `NoobForge.ms`: carregador.
- `NoobForge_Install.ms`, `NoobForge.mcr`, `mzp.run`: instalação e registro.
- `modules/`: análise/geração de colisões, ferramentas de asset, exportação, validação, updater e UI.
- `tests/`: testes executados por `3dsmaxbatch.exe`.
- `tools/EncryptModules.ms`: geração dos sete módulos `.mse`.
- `dist/`: MZP gerado localmente; não versionado no Git.
- `docs/`: revisão técnica e instruções de publicação.

O carregador prefere `.ms` durante desenvolvimento e usa `.mse` no pacote protegido. Não envie os módulos `.ms` ao repositório público.

Consulte [publicação da 1.0.8](docs/RELEASE_1.0.8.md) e [revisão técnica](docs/REVISAO_TECNICA.md).

## Testes e limites

Suítes: `SafetyRegressionTest`, `SmartCollisionTest`, `BatchExportTest`, `AssemblyExportTest`, `UITest`, `UpdaterTest`, `VersionConsistencyTest` e `ProtectedReleaseTest`.

Os testes incluem nomes semelhantes, falhas simuladas, persistência após salvar/reabrir, pivots elevados, exportação e carregamento protegido. Logs ficam em `tests/artifacts/`, ignorado pelo Git.

Testes no Max não substituem homologação na Unreal. Antes de produção sem supervisão, valide assets reais na versão da engine utilizada, incluindo escala, pivots, sockets e colisão física.
