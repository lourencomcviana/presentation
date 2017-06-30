#HSLIDE
# Schema de Log

#HSLIDE
## Apresentação
- O que é?
- Log Simples
- Prioridade
- Contexto e relações de contextuais
- Referências de log
- Consultas
- Casos de uso
- Estrutura e modelagem

#HSLIDE
### Log Simples
- Semelhante a grande maioria das soluções
- Simples de entender e utilizar

#VSLIDE
``` SQL
BEGIN
  INTERFACE.PKG_LOG.P_LOG('OLA MUNDO');
END;
/
SELECT DATA_LOG,SEQ, DESCRICAO_UPDATE FROM LOG_AUDITORIA.V_LOG_RECENTE;
```
#VSLIDE
DATA_LOG | SEQ | DESCRICAO_UPDATE
--| ------ | ------
30/06/17 08:37:40,991218 | 64  |  OLA MUNDO

#VSLIDE

Linha | DATA_LOG | SEQ | PRIORIDADE | COD_UPDATE | DATA_UPDATE | DESCRICAO_UPDATE | LOG | PATH_CONTEXTO | ID_RELACAO | ID_CONTEXTO
--| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | --- 



#HSLIDE
### Prioridade
- Define a criticidade do log
- Menor o número, maior a criticidade
- Possibilita a configuração de que tipo de log deve ou não ser salvo em cada base
- Dependendo da criticidade, um log pode ou não ser registrado na base
   - Cada base tem uma prioridade pré definida
   - Se o logo estiver com uma prioridade a baixo do ambiente, o mesmo não é registrado

#VSLIDE
### Exemplo:
Prioridade | Descricao
--| -----
1 | CORP
2 | HOMCORP
3 | DEVCORP

#VSLIDE
``` SQL
BEGIN
  INTERFACE.PKG_LOG.P_LOG('OLA MUNDO',2);
END;
/
SELECT DATA_LOG,SEQ,PRIORIDADE, DESCRICAO_UPDATE
FROM LOG_AUDITORIA.V_LOG_RECENTE;
```
Executado em desenvolvimento*

#VSLIDE
DATA_LOG | SEQ |PRIORIDADE| DESCRICAO_UPDATE
--| ------ | - | ------
30/06/17 08:37:40,991218 | 64 | 2 |  OLA MUNDO




#HSLIDE
### Contexto e relações de contextuais
- Permite a categoriação dos logs
- Funciona como "Namespace" do log
- Pode indicar onde exatamente o log estava quando o mesmo iniciou
- Muito útil para consultas e monitoramento

#VSLIDE
### Exemplo:
Prioridade | Descricao
--| -----
1 | CORP
2 | HOMCORP
3 | DEVCORP

#VSLIDE
``` SQL
BEGIN
  INTERFACE.PKG_LOG.P_LOG('OLA MUNDO',2,'TESTE.SIMPLES');
END;
/
SELECT DATA_LOG,SEQ,PRIORIDADE, DESCRICAO_UPDATE,
  ID_RELACAO,PATH_CONTEXTO
FROM LOG_AUDITORIA.V_LOG_RECENTE;
```
Executado em desenvolvimento*

#VSLIDE
DATA_LOG | SEQ | PRIORIDADE | DESCRICAO_UPDATE
-- | ------ | - | ------------
30/06/17 08:37:40,991218 | 64 | 2 |  OLA MUNDO

#VSLIDE
ID_RELACAO | PATH_CONTEXTO
 ---- | ------
298272 | TESTE.SIMPLES





#HSLIDE
## Documentação
[helper](http://x-oc-config.sefa.pa.gov.br/gitlab/sefa/sefa-ui/wikis/helper)