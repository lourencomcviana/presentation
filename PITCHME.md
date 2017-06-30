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


Linha | DATA_LOG | SEQ | PRIORIDADE | COD_UPDATE | DATA_UPDATE | DESCRICAO_UPDATE | LOG | PATH_CONTEXTO | ID_RELACAO | ID_CONTEXTO
--| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | --- 



#HSLIDE
Graças a característica acima. Um sistema pode adicionar índices de ajuda
de outros sistema (um sistema que gera DAE pode importar a ajuda do dae do sistema responsável por gerar DAE. Ou até direto da wiki externa do gitlab)
#HSLIDE
É possível adicionar arquivos markdown diretamente no formato string. Porém não é recomendado! Pois
o índice de ajuda criado não seria compartilhável e ficaria difícil de editar mais tarde.


#HSLIDE
## Dependências
- [ng-showdown](http://x-oc-config.sefa.pa.gov.br/gitlab/sefa/sefa-ui/wikis/showdown): interpretador de markdown [documentação oficial](https://github.com/showdownjs/ng-showdown)
- [ngDexie](http://x-oc-config.sefa.pa.gov.br/gitlab/sefa/sefa-ui/wikis/dexie): Controla os tópicos de ajuda no banco local(indexDB) da máquina do usuário utilizando o framework dexiejs
- [mapper](#): Mapeia json para classes bem definidas. Uso interno

#HSLIDE
## Classes
Help, Search, Summary
#VSLIDE
### Help
``` json
{
    "id":"chave do help",
    "title":["sub sub titulo","sub titulo","titulo do help"],
    "tags":["lista","de","tags"],
    "path":"/caminho/para/o/arquivo.md",
    "document":"documento carregado.",
    "order":"ordem do help, caso não definida será considerada a ordem de inserção na base"
    "version":"versão do documento. Controlado automaticamente"
}
```
#VSLIDE
### Search
``` json
{
    "id":"chave do help",
    "title": "titulo do help",
    "tags":["lista","de","tags"],
    "path":"/caminho/para/o/arquivo.md",
    "order":"ordem do help"
}
```
#VSLIDE
### Summary
``` json
*Em desenvolvimento*
```
#HSLIDE
### Providerss 
Todos os métodos retornam [**Promessas**](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Promise)

Tipo      | Fonte        | Método | Parâmetros                        | Retorno
--------- | ------------ | ------ | --------------------------------- | -----------
Provider  | helpProvider | add    | [Help]() or [[Help]()]            | N/A
Provider  | helpProvider | delete | N/A                               | N/A


#VSLIDE
### Services
Tipo      | Fonte        | Método | Parâmetros                        | Retorno
--------- | ------------ | ------ | --------------------------------- | -----------
Service   | help         | get    | [Search]()                        | [Help]    
Service   | help         | update | N/A                               | [String]
Service   | helpDialog   | show   | [help.get]()      | N/A
  
#VSLIDE
### Directives
Tipo      | Fonte        | Parâmetros                       
--------- | ------------ | -----------------------------
Directive | helpSign     | [help.get]() 
Directive | helpLink     | [help.get]() 
Directive | *helpSearch  | [help.get]() 
Directive | *helpCards   | [help.get]()
Directive | *helpSummary | [help.get]()


#HSLIDE
# Exemplos
``` html
<help-sign tags='cadastro,vinculo'></help-sign>
<help-link>dolor sit amet</help-link>
```
#VSLIDE?image=dialog.png

#VSLIDE?image=dialogbusca.png

#VSLIDE?image=dialogddl.png


#HSLIDE
## Documentação
[helper](http://x-oc-config.sefa.pa.gov.br/gitlab/sefa/sefa-ui/wikis/helper)