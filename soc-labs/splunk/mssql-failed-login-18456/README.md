# SOC Lab – MSSQL Failed Login Analysis (EventID 18456)

## 📌 Objetivo
Simular a atuação de um **Analista de SOC Nível 1** por meio da ingestão e análise de eventos de falha de autenticação MSSQL no Splunk, com foco em triagem de alertas, análise de logs e tomada de decisão baseada em evidências.

---

## 🧰 Ferramentas Utilizadas
- Splunk Enterprise (Free)
- Logs de segurança em formato XML
- SPL (Search Processing Language)

---

## 📂 Fonte dos Dados
- Logs contendo eventos de falha de autenticação do Microsoft SQL Server  
- EventID analisado: **18456** (Login failed for user)

---

## 🔎 Metodologia
1. Ingestão de logs XML no Splunk
2. Validação da leitura e indexação dos eventos
3. Extração manual do EventID utilizando expressão regular (`rex`)
4. Filtragem específica do EventID 18456
5. Análise temporal dos eventos utilizando `timechart`
6. Remoção de intervalos sem ocorrência para redução de ruído
7. Avaliação do padrão observado e tomada de decisão SOC

---

## 📊 Query Principal
```spl
index=main
| rex "<I32 N=\"Id\">(?<EventID>\d+)</I32>"
| search EventID=18456
| timechart count
| where count > 0

## 🧠 Análise

Foram identificadas múltiplas falhas de login MSSQL (EventID 18456) concentradas em um curto intervalo de tempo, com ocorrências registradas em milissegundos.

A frequência e a proximidade temporal dos eventos são incompatíveis com interação humana comum, tornando improvável a hipótese de um usuário digitando repetidamente a senha de forma manual.
O padrão observado é mais consistente com comportamento automatizado, como aplicação mal configurada, serviço tentando se autenticar com credenciais inválidas ou tentativa automatizada de acesso.

## 🚨 Decisão SOC

O alerta foi escalado para investigação, considerando o padrão anômalo identificado.
Não foi assumido ataque sem evidência conclusiva, seguindo boas práticas de SOC e evitando falsos positivos precipitados.

## 📘 Aprendizados

Ingestão e análise de logs XML no Splunk

Extração manual de campos em cenários de parsing não padronizado

Análise temporal para identificação de padrões anômalos

Avaliação crítica de hipóteses (erro humano vs comportamento automatizado)

Tomada de decisão baseada em evidência no contexto de SOC Nível 1