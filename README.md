# Rede Neutra — Dashboard GitHub Releases

Repositório novo e autocontido para o dashboard **Rede Neutra**.

## Arquitetura

- `index.html`: dashboard completo (Base, Consolidado, Gráficos, Diário, Análise de Impacto, E-mail, Histórico e Glossário).
- `release-assets/RSUL-26-09.json`: recorte RPS/Sul.
- `release-assets/RLESTE-26-09.json`: recorte Região Leste.
- `release-assets/release-manifest.json`: checksums e controles do par.
- `scripts/validate_release.py`: valida schema, competência, corte comum, datas, quantidades e duplicidades.
- `.github/workflows/pages.yml`: publica o dashboard no GitHub Pages.
- `.github/workflows/release.yml`: cria uma GitHub Release ao publicar uma tag `v*` e anexa os arquivos operacionais.

## Correção central do modelo V2

A projeção usa `dataCorte` explícita e igual nos dois ativos. Ela **não** usa mais a última data que teve ocorrência. Para a competência `26-09`, o corte é `2026-09-24`. Assim:

- RPS: 13 ocorrências / 24 × 30 = 16,25 → **16**;
- BR.: 36 ocorrências / 24 × 30 = **45**.

## Publicação do zero

```bash
# crie no GitHub um repositório público chamado rede-neutra-dashboard
git remote add origin https://github.com/otaviofelisberto0-creator/rede-neutra-dashboard.git
git push -u origin main
git push origin v1.0.0
```

O push da tag cria a Release e anexa:

1. `RSUL-26-09.json`
2. `RLESTE-26-09.json`
3. `release-manifest.json`
4. `Rede_Neutra_index_v1.0.0.html`

Depois, habilite **Settings → Pages → Source: GitHub Actions**.

## Atualização mensal

1. Gere o novo par pelo bloco **BASE** do dashboard.
2. Substitua os dois JSONs em `release-assets/`.
3. Atualize o manifesto e `github-minimum-competence` no `index.html`.
4. Execute `python scripts/validate_release.py release-assets`.
5. Faça commit e publique uma nova tag.

## Regra de ativação

O dashboard só ativa um par quando ambos os JSONs:

- estão na mesma Release publicada;
- têm a mesma competência e o mesmo `dataCorte`;
- usam `rede-neutra-base-v2`;
- têm `QT` inteira e positiva;
- não contêm linhas compactadas duplicadas;
- passam pela validação transacional completa.
