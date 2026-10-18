# ROCKNIX — Y3506_V05_20251215, lote 2551

Este pacote contém um **overlay de painel para ROCKNIX**, construído a partir do
DTB de fábrica exato da placa/painel `Y3506_V05_20251215 2551`.

> **Não é para substituir o DTB completo do ROCKNIX.**
> O ROCKNIX (kernel mainline) carrega o display como um *device-tree overlay*.
> Portanto o arquivo correto para este caso é `mipi-panel.dtbo`, não
> `rk3326-r36s-linux.dtb`.

## Instalação

1. Grave a imagem oficial indicada para R36S:
   `ROCKNIX-RK3326.aarch64-20261001-a.img.gz`.
2. Sem ligar o aparelho, abra no computador a partição FAT chamada `ROCKNIX`.
3. Crie a pasta `overlays` na raiz, se ela não existir.
4. Copie o arquivo **`mipi-panel.dtbo` deste pacote** para:

   ```text
   ROCKNIX/overlays/mipi-panel.dtbo
   ```

   Substitua qualquer `mipi-panel.dtbo` anterior. Não renomeie este arquivo e
   não deixe extensão dupla como `.dtbo.dtbo`.
5. Ejete o cartão corretamente, insira-o no TF1/OS e ligue. No primeiro boot,
   aguarde até cinco minutos para a instalação concluir.

## O que está no overlay

- alvo: `/dsi@ff450000/panel@0` (o caminho usado pelo ROCKNIX RK3326);
- sequência de inicialização: extraída do DTB original deste painel;
- modo padrão: 640×480, 77,67 Hz (timings originais);
- formato: RGB888, 4 lanes, flags DSI `0xe03`;
- atrasos de preparação/reset/inicialização/habilitação: `100/50/20/200 ms`.

`mipi-panel-y3506_v05_20251215-2551.dts` é a versão legível para auditoria.

## Limite importante

O `unprepare-delay-ms = 250` do `fix4` do dArkOS é uma propriedade do driver
BSP 4.4. O driver `generic-dsi` do ROCKNIX não importa essa propriedade para
um `.dtbo`; assim, este overlay resolve a **inicialização do painel no boot**,
mas não transforma o DTB do dArkOS num DTB de ROCKNIX nem inclui overclock.

## Proveniência e integridade

Entrada usada:
`Y3506_V05_20251215 2551/rk3326-r36s-linux.dtb` do repositório público
`southoz/dArkOSRE-R36`.

SHA-256 da entrada: `1afafbaff0690562385ba9383f3ec2181433191d73252c81842f1fd829e0d23f`

Os hashes do pacote estão em `SHA256SUMS`.
