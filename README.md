# O joelho sob carga: aprendizado de máquina e lesão do ligamento cruzado anterior no futebol

Repositório de códigos, dados derivados e resultados da dissertação de mestrado de João Gabriel da Silva Figueiredo, apresentada ao Programa de Pós-Graduação em Modelagem e Simulação de Biossistemas (PPGMSB) da Universidade do Estado da Bahia (UNEB), Campus II, Alagoinhas.

Orientador: Prof. Dr. Erivelton Nonato de Santana. Coorientador: Prof. Dr. José Roberto de Araújo Fontoura.

DOI (versão 1.1.0): [10.5281/zenodo.24605888](https://doi.org/10.5281/zenodo.24605888). DOI de todas as versões: [10.5281/zenodo.24358609](https://doi.org/10.5281/zenodo.24358609). A versão 1.0.1 ([10.5281/zenodo.24358610](https://doi.org/10.5281/zenodo.24358610)) corresponde às etapas 1 a 7 e sustenta a dissertação e o artigo do ACDSA 2027.

## Objetivo

Estimar, por simulação neuromusculoesquelética no OpenSim, a sobrecarga do joelho durante a mudança de direção de 90° (v-cut) e avaliar se variáveis biomecânicas extraídas das simulações discriminam, por Máquinas de Vetores de Suporte (SVM) com kernel linear, ciclos de elevada sobrecarga em valgo em participantes não utilizados no treinamento.

## Dados de entrada (não redistribuídos)

Os dados experimentais e os modelos musculoesqueléticos escalonados pertencem à base pública de Nitschke et al.:

- NITSCHKE, M. et al. *Optical motion capturing of change of direction motions reconstructed with inverse kinematics and dynamics and optimal control simulation*. Zenodo, 2022. DOI: [10.5281/zenodo.6949012](https://doi.org/10.5281/zenodo.6949012).
- NITSCHKE, M. et al. Change the direction: 3D optimal control simulation by directly tracking marker and ground reaction force data. *PeerJ*, v. 11, e14852, 2023. DOI: [10.7717/peerj.14852](https://doi.org/10.7717/peerj.14852).

Foram analisados os Participantes 02 a 11, com três ensaios de v-cut cada (30 ciclos).

## Sequência de execução

Os códigos estão em notebooks Jupyter, salvos com as respectivas saídas, o que permite conferir os resultados sem executá-los.

| Ordem | Arquivo | Função |
|---|---|---|
| 1 | `scripts/fase3_reprocessamento_opensim.ipynb` | Processamento dos ensaios no OpenSim 4.5: filtragem Butterworth a 15 Hz, Dinâmica Inversa, Otimização Estática e Análise de Reação Articular do joelho direito, com comparação da Dinâmica Inversa aos momentos de referência dos autores da base |
| 2 | `scripts/Comparar controle P02 0122.ipynb` | Controle do ensaio P02/0123: compara o P02/0122 processado com a janela normal e com início 50 ms antes |
| 3 | `scripts/Fase4 extracao desfecho frontal.ipynb` | Extração do desfecho (pico do momento em valgo na aceitação de peso, em Nm/kg) e dos sete preditores |
| 4 | `scripts/Fase5 validacao svm desfecho frontal.ipynb` | SVM linear (C = 1, pesos de classe balanceados): Nível 1, GroupKFold aninhado (K = 3) com limiar por G-Mean no laço interno; Nível 2, validação LOSO com bootstrap por conglomerado (2.000 réplicas); vetor de pesos |
| 5 | `scripts/Fase6 robustez svm.ipynb` | Análises de sensibilidade: limiar P75 recalculado no treino, teste de permutação no nível do participante (1.000), exclusão do Participante 09, VIF e comparação entre validação por ciclo e por participante (50 repetições) |
| 6 | `scripts/Fase7 robustez contato.ipynb` | Extração do desfecho com exclusão dos primeiros 10 ms após o contato inicial |
| 7 | `scripts/GERAÇÃO DE FIGURAS.ipynb` e `scripts/gerar_figuras_cap3.py` | Geração das Figuras 3 e 4 da dissertação (validação da Dinâmica Inversa e desfecho por participante), em versões de notebook e de script |
| 8 | `scripts/Fase8 sbmsb2026 reacoes articulares.ipynb` | Extração das reações articulares do joelho direito a partir das saídas da etapa 1, sem executar o OpenSim: picos de compressão e das forças anteroposterior e médio-lateral, momentos em valgo e em varo na aceitação de peso, ângulo de flexão do joelho, resíduos da pelve na Dinâmica Inversa e curvas médias normalizadas pelo apoio. Gera a Tabela 2 e a Figura 1 do artigo apresentado ao VI SBMSB 2026 (trabalho SBMSB2026-007) e confere o desfecho com a etapa 3 (mediana do valgo de 0,114 Nm/kg e do varo de 0,815 Nm/kg) |

## Organização das pastas

```
scripts/           notebooks e script de figuras, na ordem de execução
data/              conjuntos de dados por ciclo e validação da Dinâmica Inversa
results/           saídas das análises, predições e pesos
results-figures/   matrizes de confusão e curvas ROC da etapa 4 e curvas de reação articular da etapa 8
```

## Arquivos de dados e resultados

| Arquivo | Conteúdo |
|---|---|
| `data/dataset_desfecho_frontal.xlsx` | Desfecho, rótulo e sete preditores por ciclo (30 ciclos), gerado na etapa 3 |
| `data/dataset_desfecho_frontal_ignora10ms.xlsx` | Idem, com o desfecho extraído sem os primeiros 10 ms após o contato (etapa 6) |
| `data/validacao_ID_30.csv` | Concordância entre a Dinâmica Inversa deste estudo e a dos autores da base nos 30 ensaios (RMSE e r de Pearson para quatro momentos articulares) |
| `results/fase3_validacao_ID.csv` e `results/fase3_relatorio.txt` | Registro da execução da etapa 1 para o Participante 02 (ensaios 0122 e 0123) |
| `results/fase5_resultados.txt` | Saída completa da etapa 4 |
| `results/fase5_predicoes_oof.csv` | Predições fora da amostra por ciclo: participante, massa, pico do momento em valgo, desfecho normalizado, classe, escore e predição do Nível 1 e escore e predição do LOSO |
| `results/fase5_pesos.csv` | Vetor de pesos do SVM ajustado com todos os ciclos e média e desvio padrão nas partições do Nível 1 e do LOSO |
| `results-figures/fase5_nivel1.png` e `results-figures/fase5_nivel2.png` | Matrizes de confusão e curvas ROC dos Níveis 1 e 2 |
| `results/fase6_robustez.txt` | Saída completa da etapa 5 |
| `results/fase7_robustez_contato.txt` | Saída completa da etapa 6 |
| `results/fase8_tabela1_por_ciclo.csv` | Reações articulares do joelho por ciclo (30 ciclos): durações do apoio e da aceitação de peso, flexão do joelho, picos e instantes da compressão e das forças anteroposterior e médio-lateral (em peso corporal e em N/kg), picos dos momentos em valgo e em varo (Nm/kg), gerado na etapa 8 |
| `results/fase8_tabela1_resumo.csv` | Resumo de cada variável: mediana e intervalo interquartil dos ciclos, mínimo e máximo, média e desvio padrão entre as médias dos participantes e ICC(1) |
| `results/fase8_residuos_pelve.csv` | Resíduos da pelve na Dinâmica Inversa durante o apoio, por ciclo (forças em % do peso corporal; momentos em Nm/kg) |
| `results-figures/fase8_figura1_curvas.png` | Curvas médias de compressão, forças anteroposterior e médio-lateral, momento frontal e flexão do joelho durante o apoio (média ± desvio padrão entre participantes) |

Observações:
- Os arquivos `results/fase5_predicoes_oof.csv`, `results/fase5_pesos.csv` e `results/fase3_validacao_ID.csv` usam ponto e vírgula como separador e vírgula como separador decimal. O arquivo `data/validacao_ID_30.csv` usa vírgula como separador e ponto como separador decimal.
- O arquivo de predições identifica o participante, mas não o número do ensaio.
- Na etapa 8, o nome "tabela1" dos arquivos corresponde à Tabela 2 do artigo SBMSB2026-007, e a figura corresponde à Figura 1 do mesmo artigo. Os arquivos da etapa 8 usam ponto e vírgula como separador e vírgula como separador decimal. Na etapa 8, a força anteroposterior positiva é a força anterior aplicada sobre a tíbia, e a médio-lateral positiva é a força lateral; o momento frontal positivo indica resistência a uma solicitação em valgo.
- As Figuras 7 e 8 da dissertação foram geradas a partir dos valores das saídas das etapas 4 e 5 e não estão incluídas neste repositório.

## Como reproduzir

1. Faça o download da base de Nitschke et al. no Zenodo e descompacte-a em uma pasta do seu computador.
2. Crie uma pasta para as saídas do processamento.
3. **Ajuste os caminhos.** Os notebooks foram executados no computador do autor, e os caminhos de pastas estão escritos no início do código de cada um (por exemplo, `C:\JG\opensim\Dados_Zenodo` e `C:\JG\opensim\Resultados_FASE3`). Antes de executar, substitua esses caminhos pelos do seu computador, conforme a tabela abaixo.
4. Execute as etapas na ordem da tabela de execução. As etapas 3 a 8 dispensam o OpenSim depois que as saídas da etapa 1 existem.

| Arquivo | Variável | O que informar |
|---|---|---|
| `scripts/fase3_reprocessamento_opensim.ipynb` | `PASTA_ZENODO` | Pasta da base de Nitschke et al. |
| | `PASTA_SAIDA` | Pasta onde serão gravadas as saídas do OpenSim |
| | `OPENSIM_CMD` | Caminho do `opensim-cmd.exe`; se ficar vazio, o notebook procura o programa automaticamente |
| `scripts/Comparar controle P02 0122.ipynb` | `PASTA` | Pasta das saídas da etapa 1 |
| `scripts/Fase4 extracao desfecho frontal.ipynb` | `PASTA_FASE3` | Pasta das saídas da etapa 1 |
| | `PASTA_ZENODO` | Pasta da base de Nitschke et al. |
| `scripts/Fase5 validacao svm desfecho frontal.ipynb` e `scripts/Fase6 robustez svm.ipynb` | `PASTA_ZENODO` | Pasta da base de Nitschke et al. |
| | `PASTA_SAIDA` | Pasta onde serão gravados os resultados (no computador do autor, a mesma pasta das saídas da etapa 1) |
| | `CAMINHO_EXCEL` | Caminho completo do `dataset_desfecho_frontal.xlsx` |
| `scripts/Fase7 robustez contato.ipynb` | `PASTA_FASE3` e `PASTA_ZENODO` | Pasta das saídas da etapa 1 e pasta da base |
| `scripts/GERAÇÃO DE FIGURAS.ipynb` | `PASTA_FASE3`, `PASTA_SAIDA` e `PASTA_ZENODO` | Pasta das saídas da etapa 1, pasta para as figuras e pasta da base |
| `scripts/gerar_figuras_cap3.py` | `PASTA_FASE3` e `PASTA_ZENODO` | Pasta das saídas da etapa 1 e pasta da base |
| `scripts/Fase8 sbmsb2026 reacoes articulares.ipynb` | `RESULTADOS` | Pasta das saídas da etapa 1 |
| | `BASE` | Pasta da base de Nitschke et al. |
| | `SAIDA` | Pasta onde serão gravadas as tabelas e a figura (os nomes dos arquivos gerados não têm o prefixo `fase8_`, acrescentado neste repositório) |

No Windows, mantenha o prefixo `r` antes das aspas, como em `r'D:\Meus_Dados\Nitschke'`. Em Linux ou macOS, use barras normais, como em `'/home/usuario/Nitschke'`.

Requisitos: Python 3 com numpy, pandas, scipy, scikit-learn e matplotlib (ver `requirements.txt`), além do openpyxl para a leitura dos arquivos `.xlsx`, e OpenSim 4.5 (ferramentas de linha de comando) para a etapa 1.

## Licenças

Código: MIT (arquivo `LICENSE`). Dados derivados e resultados: CC BY 4.0. Ao utilizar este material, cite também a base original de Nitschke et al.

## Como citar

FIGUEIREDO, J. G. S. **O joelho sob carga**: aprendizado de máquina e lesão do ligamento cruzado anterior no futebol (códigos e dados). Versão 1.1.0. [S. l.]: Zenodo, 2026. DOI: https://doi.org/10.5281/zenodo.24605888.

Consulte também o arquivo `CITATION.cff`.
