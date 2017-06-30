#HSLIDE
# Schema de Log

#HSLIDE
## Apresentação
- O que é?
- Log Simples
- Prioridade
- Contexto e relações de contextuais
- Referências de log e parâmetros
- Documentação

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


#VSLIDE
``` SQL
SELECT ID_RELACAO,ID_RELACAO_PAI,ID_CONTEXTO,
  ID_PRIORIDADE_DEFAULT,NIVEL, CONTEXTO,PATH
FROM TABLE(
  LOG_AUDITORIA.PKG_LOG.F_CONTEXTO_LEAF(298272)
);
```

#VSLIDE
ID_RELACAO | ID_RELACAO_PAI | ID_CONTEXTO
---- | -- | ---
298272 | 45 | 896047
45 |  | 139

#VSLIDE
ID_PRIORIDADE_DEFAULT | NIVEL | CONTEXTO 
-- | -------- | ----- 
2 | 1 | SIMPLES  
3 | 2 | TESTE  

#VSLIDE
``` SQL
SELECT * 
FROM LOG_AUDITORIA.TAB_CONTEXTO C 
WHERE
 EXISTS(
  SELECT ID_CONTEXTO 
  FROM TABLE(
   LOG_AUDITORIA.PKG_LOG.F_CONTEXTO_LEAF(298272)
  )
  WHERE ID_CONTEXTO=C.ID
 )
```
#VSLIDE
ID |  CONTEXTO 
-- | -------- 
896047 | SIMPLES  
139 | TESTE  

#HSLIDE

Exemplo completo

#VSLIDE
``` sql
declare
 p_saida varchar2(4000);
 logx XMLTYPE;
 logerror xmltype;
begin
 Log_auditoria.Pkg_Log.P_LOG('sasa',3,'TESTE',logx);
 Log_auditoria.Pkg_Log.P_ADD('teste','asasa',logx);
 Log_auditoria.Pkg_Log.P_ADD('testenum',2,logx);
 Log_auditoria.Pkg_Log.P_ADD('testedata',sysdate,logx);
 Log_auditoria.Pkg_Log.P_LOG('inserir xml',logx);
 Log_auditoria.Pkg_Log.P_ADD('testexml',xmltype('<p>eu sou um <b>paragrafo</b> html</p>'),logx);
 dbms_output.put_line(logx.getClobVal());
 p_saida:=2/0;
 
 exception
   when others THEN
     Log_auditoria.PKG_LOG.P_LOG_ERRO('Erro em producao','TESTE.ERRO.ANONYMOUS',logx,logerror);
     dbms_output.put_line(logerror.getClobVal());
     Log_auditoria.PKG_LOG.P_ADD('OBS','ERRO DEVIDO A COMPREENSAO FALHA DA MATEMATICA',logerror);
end;
```

#HSLIDE
## Documentação
[Log no banco de dados gitLab](http://x-oc-config.sefa.pa.gov.br/gitlab/sefa/tutoriais/wikis/usando-schema-de-log-no-banco)