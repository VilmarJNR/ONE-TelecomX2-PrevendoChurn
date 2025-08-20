# Relatório de Fatores de Evasão — TelecomX2

## 1) Sumário Executivo
- Foram treinados dois modelos: **Regressão Logística** e **Random Forest** para prever a evasão (`Cancelado`).
- **Desempenho no teste (n=2110):**
  - Regressão Logística — **Acurácia 80.24%**, *classe churn* (1): precision 0.66, recall 0.52, f1 0.58.
  - Random Forest — **Acurácia 78.48%**, *classe churn* (1): precision 0.63, recall 0.48, f1 0.54.
- A Logística apresentou **melhor equilíbrio** entre desempenho e interpretabilidade (maior recall para churn e leitura direta de coeficientes).

## 2) Metodologia (resumo)
- Pré-processamento com *encoding* de variáveis categóricas e **padronização** de escalas.
- **Balanceamento** da classe minoritária com **SMOTE**, reduzindo vieses do treinamento.
- Avaliação com matriz de confusão e *classification report* (precision/recall/f1), além de comparação de **coeficientes** (Logística) e **importância de features** (Random Forest).

## 3) Principais Fatores que Aumentam o Risco de Evasão
(interpretados pelos **coeficientes positivos** da Regressão Logística)
- ▲ **Servico_Internet_Fiber optic** (coef. +0.440)
- ▲ **Fatura_Online** (coef. +0.362)
- ▲ **Forma_Pagamento_Electronic check** (coef. +0.345)
- ▲ **Idoso** (coef. +0.304)
- ▲ **Multiplas_Linhas** (coef. +0.143)
- ▲ **Streaming_TV** (coef. +0.103)
- ▲ **Streaming_Filmes** (coef. +0.026)
- ▲ **Gasto_Mensal** (coef. +0.010)

**Leituras de negócio (hipóteses):**
- **Serviço de Internet — Fiber optic** e **Fatura Online** associam-se a maior propensão de churn, possivelmente por **sensibilidade a preço/experiência digital** e expectativas elevadas.
- **Forma de Pagamento — Electronic check** indica maior risco que outras modalidades (ex.: cartão automático).
- **Perfil — Idoso** pode demandar **suporte mais assistido** e comunicação simplificada.
- **Múltiplas Linhas / Streaming** sugerem pacotes complexos que elevam custo e pontos de fricção.

## 4) Fatores que Reduzem o Risco de Evasão
(interpretados pelos **coeficientes negativos** da Regressão Logística)
- ▼ **Suporte_Tecnico** (coef. -0.498)
- ▼ **Servico_Telefonico** (coef. -0.488)
- ▼ **Tipo_Contrato_Two year** (coef. -0.454)
- ▼ **Servico_Internet_No** (coef. -0.451)
- ▼ **Seguranca_Online** (coef. -0.370)
- ▼ **Tipo_Contrato_One year** (coef. -0.353)
- ▼ **Forma_Pagamento_Mailed check** (coef. -0.240)
- ▼ **Forma_Pagamento_Credit card (automatic)** (coef. -0.191)
- ▼ **Backup_Online** (coef. -0.167)
- ▼ **Dependentes** (coef. -0.153)

**Leituras de negócio:**
- **Contrato anual/bienal** é o maior protetor (vs. mensal), indicando **retenção por compromisso** e/ou desconto.
- **Suporte Técnico, Segurança Online, Backup** reduzem churn — evidência de que **experiência de suporte** e **serviços de valor** protegem a base.
- **Pagamentos automáticos (cartão)** reduzem churn, sugerindo que **fricção de pagamento** contribui para cancelamento.

## 5) Variáveis de Maior Influência (Magnitude)
(segundo **Random Forest Feature Importance**)
- **Gasto_Total** — importância 19.5%
- **Gasto_Mensal** — importância 17.6%
- **Meses_Contrato** — importância 16.7%
- **Servico_Internet_Fiber optic** — importância 4.8%
- **Forma_Pagamento_Electronic check** — importância 4.4%
- **Sexo_Male** — importância 2.9%
- **Tipo_Contrato_Two year** — importância 2.9%
- **Fatura_Online** — importância 2.8%
- **Parceiro** — importância 2.3%
- **Suporte_Tecnico** — importância 2.3%

**Interpretação conjunta:** *Gasto Total*, *Gasto Mensal* e *Meses de Contrato* têm **grande poder explicativo** (efeitos possivelmente **não-lineares**). Em geral, **tenure maior** tende a reduzir churn, enquanto **custo mensal alto** pode elevá-lo. As categorias *Fiber optic*, *Electronic check* e *Fatura Online* aparecem entre os principais fatores, corroborando a Logística.

## 6) Estratégias de Retenção por Alavanca
**a) Preço e Contrato**
- Converter **mensal → anual/bienal** com *oferta personalizada* (desconto progressivo por risco).
- Criar **garantia de preço por 12/24 meses** para quem tem **Gasto Mensal alto**.

**b) Pagamentos**
- Incentivar **débito automático/cartão** (ex.: 5–10% off por 3 meses) e reduzir *Electronic check*.
- Alertas proativos para **falhas de pagamento** (evitar churn involuntário).

**c) Experiência e Suporte**
- **Proativo para alto risco** (p≥0.6): contato em 48h com **time dedicado**.
- **Pacote de valor**: incluir **Suporte Técnico**, **Segurança Online** e **Backup** em *trial* de 60–90 dias para quem não possui.

**d) Produto e Bundle**
- Para **Fiber optic**: revisar **posicionamento de preço** e **SLA**; oferecer **upgrade de roteador** ou **otimização de Wi‑Fi** sem custo inicial.
- **Bundles** com *Streaming* e **Múltiplas Linhas**: simplificar planos, reduzir *add-ons* pouco usados.

**e) Comunicação segmentada**
- **Idosos**: canal preferencial, fatura simplificada (mesmo se online), tutorial guiado e atendimento humano prioritário.
- **Paperless (Fatura Online)**: revisão de UX de cobrança, **notificações claras** de vencimento e benefícios tangíveis do cadastro em **débito automático**.

## 7) Operacionalização do Modelo
- **Threshold dinâmico** por objetivo: aumentar **recall** (ex.: p≥0.45) quando a meta é **reduzir churn** a qualquer custo; aumentar **precision** (ex.: p≥0.65) quando o foco é **ROI de retenção**.
- **Regra de priorização** (exemplo): `score × CLV × probabilidade de resposta ao incentivo` para ordenar lista de abordagem.
- **Teste A/B** de ofertas por segmento (Fiber / High ARPU / Sem Suporte Técnico / Electronic check).

## 8) Métricas de Sucesso
- **Churn Rate** (mensal, trimestral), **Retenção incremental (uplift)**, **Custo por Cliente Retido**, **Payback** por oferta.
- **NPS / CSAT** pós-atendimento para clientes abordados.

## 9) Próximos Passos
- Incluir modelos **gradiente boosting** (XGBoost/LightGBM) e **calibração** de probabilidades (Platt/Isotonic).
- Adotar **SHAP values** para explicabilidade local (por cliente) e regras de ação automaticamente sugeridas.
- Construir **dashboard** (ex.: Power BI/Streamlit) para monitorar *score*, *churn* e *ROI de retenção*.

---

### Anexo — Tabelas de Drivers
**Top coeficientes positivos (aumentam churn):**
- ▲ **Servico_Internet_Fiber optic** (coef. +0.440)
- ▲ **Fatura_Online** (coef. +0.362)
- ▲ **Forma_Pagamento_Electronic check** (coef. +0.345)
- ▲ **Idoso** (coef. +0.304)
- ▲ **Multiplas_Linhas** (coef. +0.143)
- ▲ **Streaming_TV** (coef. +0.103)
- ▲ **Streaming_Filmes** (coef. +0.026)
- ▲ **Gasto_Mensal** (coef. +0.010)
- ▲ **Gasto_Total** (coef. +0.000)

**Top coeficientes negativos (reduzem churn):**
- ▼ **Suporte_Tecnico** (coef. -0.498)
- ▼ **Servico_Telefonico** (coef. -0.488)
- ▼ **Tipo_Contrato_Two year** (coef. -0.454)
- ▼ **Servico_Internet_No** (coef. -0.451)
- ▼ **Seguranca_Online** (coef. -0.370)
- ▼ **Tipo_Contrato_One year** (coef. -0.353)
- ▼ **Forma_Pagamento_Mailed check** (coef. -0.240)
- ▼ **Forma_Pagamento_Credit card (automatic)** (coef. -0.191)
- ▼ **Backup_Online** (coef. -0.167)
- ▼ **Dependentes** (coef. -0.153)

**Top importâncias (Random Forest):**
- **Gasto_Total** — importância 19.5%
- **Gasto_Mensal** — importância 17.6%
- **Meses_Contrato** — importância 16.7%
- **Servico_Internet_Fiber optic** — importância 4.8%
- **Forma_Pagamento_Electronic check** — importância 4.4%
- **Sexo_Male** — importância 2.9%
- **Tipo_Contrato_Two year** — importância 2.9%
- **Fatura_Online** — importância 2.8%
- **Parceiro** — importância 2.3%
- **Suporte_Tecnico** — importância 2.3%
- **Seguranca_Online** — importância 2.3%
- **Tipo_Contrato_One year** — importância 2.3%
- **Backup_Online** — importância 2.2%
- **Idoso** — importância 2.2%
- **Multiplas_Linhas** — importância 2.0%
