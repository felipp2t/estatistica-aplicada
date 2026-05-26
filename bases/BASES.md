# 📊 Documentação das Bases de Dados — Estatística Aplicada

Para manter o repositório leve e garantir o cumprimento das políticas de limite de tamanho de arquivos do GitHub, **os dados brutos em formato JSON não são sincronizados no controle de versão** (estão configurados no arquivo `.gitignore` sob a regra `bases/*.json`).

Se você acabou de clonar este repositório, siga as orientações abaixo para baixar as bases corretas diretamente do portal oficial de transparência e salvá-las com a nomenclatura exata esperada pelo pipeline de processamento (ETL).

---

## 🔗 Link para Acesso às Bases
Todos os arquivos devem ser extraídos do Portal de Dados Abertos da Prefeitura Municipal:
👉 **[Portal de Transparência - Dados Abertos (Betha Cloud)](https://transparencia.betha.cloud/#/n4W91vnHptoBkiHKAxioOA==/dados-abertos?esconderCabecalho=S&esconderMenu=S&esconderRodape=S)**

---

## 📥 Relação de Arquivos e Filtros
No portal, localize os serviços listados na tabela abaixo, aplique os filtros correspondentes, selecione a opção de exportação em **JSON** e salve-os dentro da pasta `bases/` com o respectivo **Nome do Arquivo**.

| Serviço / Base de Dados | Exercício (Ano) / Filtros | Nome exato do arquivo (Salvar em `/bases`) | Formato |
| :--- | :--- | :--- | :---: |
| **Adiantamentos** | `2025` | `Adiantamentos-2025.json` | `JSON` |
| **Adiantamentos** | `2026` | `Adiantamentos-2026.json` | `JSON` |
| **Agentes Públicos** | Situação do Vínculo: `Trabalhando` | `Agentes Públicos-Trabalhando.json` | `JSON` |
| **Despesas com Pessoal** | `2025` | `Despesas com Pessoal-2025.json` | `JSON` |
| **Despesas com Pessoal** | `2026` | `Despesas com Pessoal-2026.json` | `JSON` |
| **Quadro de Cargos** | `2025` | `Quadro de Cargos-2025.json` | `JSON` |
| **Quadro de Cargos** | `2026` | `Quadro de Cargos-2026.json` | `JSON` |

> [!IMPORTANT]  
> **Atenção à nomenclatura dos arquivos:**  
> O pipeline automático de ETL localizado em `src/etl.py` realiza uma busca por correspondência de prefixo (`startswith`). Portanto, garanta que os arquivos possuam **exatamente** os nomes descritos na tabela acima, respeitando maiúsculas, minúsculas, acentuação e hífens.

---

## 🛠️ Passo a Passo para Download

1. Acesse o [Portal de Dados Abertos](https://transparencia.betha.cloud/#/n4W91vnHptoBkiHKAxioOA==/dados-abertos?esconderCabecalho=S&esconderMenu=S&esconderRodape=S).
2. Utilize a barra de pesquisa ou navegue na lista para encontrar o serviço desejado (ex: *Adiantamentos*).
3. Clique em **Filtros** para refinar a busca:
   - Para **Exercício/Ano**, selecione o respectivo ano (`2025` ou `2026`).
   - Para **Agentes Públicos**, aplique o filtro de situação do vínculo para trazer apenas os servidores com status `Trabalhando`.
4. Clique em **Gerar/Exportar** e escolha o formato **JSON**.
5. Salve o arquivo na pasta `/bases` do seu repositório local seguindo os nomes especificados na tabela.

---

## 🚀 Próximos Passos (Processamento)
Uma vez que todos os 7 arquivos JSON estejam na pasta `bases/`, você poderá rodar o pipeline completo para gerar a base unificada e prosseguir com as análises estatísticas e regressões lineares:

```bash
# Para instalar as dependências e executar o pipeline principal
uv run python main.py
```

Isso executará a padronização e consolidação dos dados brutos em um único arquivo de mais de 20 mil registros chamado `base_unificada.csv` no diretório raiz do projeto.
