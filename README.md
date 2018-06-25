# Tutorial

Este tutorial irá mostrar como utilizar o sistema Ontop através da linguagem SPARQL para acessar as definições das estruturas das plantas definidas pela ontologia Plant Ontology Consortium.

Este tutorial é baseado noa artigo: "Plant Ontology Consortium e sua Aplicação no Sistema Ontop".

Requisitos
------------

* Java 7 ou 8
* [H2 (banco de dados relacional)](https://sourceforge.net/projects/ontop4obda/files/sample-data/)
* [Pacote mais recente Ontop Protégé](https://sourceforge.net/projects/ontop4obda/files/)
* [Plant Ontology Consortium](http://archive.plantontology.org/download)

Criação do Banco de Dados
-------------

Prodecimento:
1. Unzip o arquivo de H2 *(h2.zip)*
2. Inicie o banco de dados:
   * No Mac/Linux: abra um terminal, vá em *h2/bin* e execute `sh h2.sh`
   * No Windows: clique no executável `h2w.bat`
3. Após ser automaticamente redirecionado para interface web de H2, conecte-se com os parâmetros padrões:
     * JDBC URL:  *jdbc:h2:tcp://localhost/./helloworld*
     * User name: *sa*
     * No password

4. Execute o script SQL do projeto para criar o banco de dados, suas tabelas e entradas.
5. Vá na aba "Entities" e logo após, na sub-aba "Data Properties".
5. Para executar as consultas SPARQL, criar as seguintes "Data Properties": `hasDefinitionCardinal`,`hasDefinitionCell`,`hasDefinitionCollective`,`hasDefinitionEmbryo`,`hasDefinitionMulti`,`hasDefinitionPortion`,`hasDefinitionRhizoid`,`hasDefinitionTrichome`,`hasDefinitionTrichomeApex`,`hasDefinitionTrichomeTip`,`hasDefinitionVitro`,`hasDefinitionWhole`,`hasStructureCardinal`,`hasStructureCell`,`hasStructureCollective`,`hasStructureEmbryo`,`hasStructureMulti`.`hasStructurePortion`,`hasStructureRhizoid`,`hasStructureTrichome`,`hasStructureTrichomeApex`,`hasStructureTrichomeTip`,`hasStructureVitro`,`hasStructureWhole`,`hasSubDefinitionCardinal`,`hasSubDefinitionCell`,`hasSubDefinitionCollective`,`hasSubDefinitionMulti`,`hasSubDefinitionPortion`,`hasSubDefinitionRhizoid`,`hasSubDefinitionTrichome`,`hasSubDefinitionVitro`,`hasSubDefinitionWhole`,`hasSubStructureCardinal`,`hasSubStructureCell`,`hasSubStructureCollective`,`hasSubStructureMulti`,`hasSubStructurePortion`,`hasSubStructureRhizoid`,`hasSubStructureTrichome`,`hasSubStructureVitro`,`hasSubStructureWhole`,`hasSubTypeCollective`,`hasSubTypeMulti`,`hasSubTypeDefinitionCollective`,`hasSubTypeDefinitionMulti`. 

Ontologia: classes e propriedades
--------------------------------


1. Unzip o arquivo Protégé e entre em sua pasta
2. Execute-o (*run.bat* no Windows, *run.sh* no Mac/Linux)
3. Registre o driver H2 JDBC: vá em "Preferences", "JDBC Drivers" e adicione uma entrada com a seguinte informação:
     * Description: *h2*
     * Class Name: *org.h2.Driver*
     * Driver file (jar): */path/to/h2/bin/h2-1.3.176.jar*
     
4. Baixe o arquivo OWL da ontologia deste projeto.
5. Vá em "File/Open..." para carregar o arquivo da ontologia.

Mapeamentos
--------

1. Vá na aba "Ontop mapping"
2. Adicional uma nova fonte de dados (dê um nome, ex., *PlantStructreDB*)
3. Defina os parâmetros da conexão abaixo:
    * Connection URL: *jdbc:h2:tcp://localhost/./helloworld*
    * Username: *sa*
    * Password: (deixe em branco)
    * Driver class: *org.h2.Driver* (escolha no menu drop down)
4. Teste a conexão utilizando o botão “Test Connection”
5. Mude para a aba “Mapping Manager” na aba de mapeamentos Ontop
6. Selecione sua fonte de dados
7. Clique em "Create" para criar um novo mapeamento


#### Mapping 1: Plant Structure
 * Target: 
```turtle
:structureid{STRUCTUREID} owl:bottomDataProperty {NAME}^^xsd:string ; owl:topDataProperty {DEFINITION}^^xsd:string . 
```
 * Source:
```sql
SELECT "STRUCTUREID", "NAME", "DEFINITION"
FROM "PUBLIC"."tbl_structure"
```

#### Mapping 2: Cardinal Part of Multi-Tissue Plant Structure
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureCardinal {STRUCTURE}^^xsd:string ; :hasDefinitionCardinal {DEFINITION}^^xsd:string ; :hasSubStructureCardinal {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionCardinal {SUB_DEFINITION}^^xsd:string .  
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_cardinal_part_of_multi_tissue_plant_structure"
```

#### Mapping 3: Collective Plant Structure
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureCollective {STRUCTURE}^^xsd:string ; :hasDefinitionCollective {DEFINITION}^^xsd:string ; :hasSubStructureCollective {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionCollective {SUB_DEFINITION}^^xsd:string ; :hasSubTypeCollective {SUB_TYPE}^^xsd:string ; :hasSubTypeDefinitionCollective {SUB_TYPE_DEFINITION}^^xsd:string .   
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION","SUB_TYPE","SUB_TYPE_DEFINITION"
FROM "PUBLIC"."tbl_collective_plant_structure"
```

#### Mapping 4: Embryo Plant Structure
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureEmbryo {STRUCTURE}^^xsd:string ; :hasDefinitionEmbryo {DEFINITION}^^xsd:string .   
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION"
FROM "PUBLIC"."tbl_embryo_plant_structure"
```


#### Mapping 5: In Vitro Plant Structure
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureVitro {STRUCTURE}^^xsd:string ; :hasDefinitionVitro {DEFINITION}^^xsd:string ; :hasSubStructureVitro {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionVitro {SUB_DEFINITION}^^xsd:string .    
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_in_vitro_plant_structure"
```

#### Mapping 6: Multi-Tissue Plant Structure
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureMulti {STRUCTURE}^^xsd:string ; :hasDefinitionMulti {DEFINITION}^^xsd:string ; :hasSubStructureMulti {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionMulti {SUB_DEFINITION}^^xsd:string ; :hasSubTypeMulti {SUB_TYPE}^^xsd:string ; :hasSubTypeDefinitionMulti {SUB_TYPE_DEFINITION}^^xsd:string .     
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION","SUB_TYPE","SUB_TYPE_DEFINITION"
FROM "PUBLIC"."tbl_multi_tissue_plant_structure"
```

#### Mapping 7: Plant Cell
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureCell {STRUCTURE}^^xsd:string ; :hasDefinitionCell {DEFINITION}^^xsd:string ; :hasSubStructureCell {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionCell {SUB_DEFINITION}^^xsd:string .  
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_plant_cell"
```

#### Mapping 8: Portion of Plant Tissue
 * Target: 
```turtle
:{STRUCTUREID} :hasStructurePortion {STRUCTURE}^^xsd:string ; :hasDefinitionPortion {DEFINITION}^^xsd:string ; :hasSubStructurePortion {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionPortion {SUB_DEFINITION}^^xsd:string . 
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_portion_of_plant_tissue"
```

#### Mapping 9: Rhizoid
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureRhizoid {STRUCTURE}^^xsd:string ; :hasDefinitionRhizoid {DEFINITION}^^xsd:string ; :hasSubStructureRhizoid {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionRhizoid {SUB_DEFINITION}^^xsd:string .  
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_rhizoid"
```

#### Mapping 10: Trichome
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureTrichome {STRUCTURE}^^xsd:string ; :hasDefinitionTrichome {DEFINITION}^^xsd:string ; :hasSubStructureTrichome {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionTrichome {SUB_DEFINITION}^^xsd:string .  
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_trichome"
```

#### Mapping 11: Trichome Apex
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureTrichomeApex {STRUCTURE}^^xsd:string ; :hasDefinitionTrichomeApex {DEFINITION}^^xsd:string . 
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION"
FROM "PUBLIC"."tbl_trichome_apex"
```

#### Mapping 12: Trichome Tip
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureTrichomeTip {STRUCTURE}^^xsd:string ; :hasDefinitionTrichomeTip {DEFINITION}^^xsd:string .  
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION"
FROM "PUBLIC"."tbl_trichome_tip"
```

#### Mapping 13: Whole Plant
 * Target: 
```turtle
:{STRUCTUREID} :hasStructureWhole {STRUCTURE}^^xsd:string ; :hasDefinitionWhole {DEFINITION}^^xsd:string ; :hasSubStructureWhole {SUB_STRUCTURE}^^xsd:string ; :hasSubDefinitionWhole {SUB_DEFINITION}^^xsd:string .  
```
 * Source:
```sql
SELECT "STRUCTUREID", "STRUCTURE", "DEFINITION", "SUB_STRUCTURE", "SUB_DEFINITION"
FROM "PUBLIC"."tbl_whole_plant"
```

# SPARQL

1. Selecione Ontop no menu "Reasoner"
2. Inicie o "Reasoner" Ontop
3. Execute a seguinte consulta para obter as definições das estruturas das plantas:
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid owl:bottomDataProperty ?name; owl:topDataProperty ?definition}
``` 

Dica: clique com o botão direito do mouse no campo de consulta SPARQL para visualizar a consulta SQL gerada.

### Outras Consultas

Para obter as definições de cada uma das subestruturas de cada estrutura execute as seguintes consultas:
1. Cardinal Part of Multi-Tissue Plant Structure
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureCardinal ?structure; :hasDefinitionCardinal  ?definition; :hasSubStructureCardinal ?sub_structure; :hasSubDefinitionCardinal ?sub_definition }
```

2. Collective Plant Structure
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureCollective ?structure; :hasDefinitionCollective  ?definition; :hasSubStructureCollective ?sub_structure; :hasSubDefinitionCollective ?sub_definition; :hasSubTypeCollective ?sub_type; :hasSubTypeDefinitionCollective ?sub_type_definition }
```
3. Embryo Plant Structure
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureEmbryo ?structure; :hasDefinitionEmbryo ?definition;}
```

4. In Vitro Plant Structure
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureVitro ?structure; :hasDefinitionVitro  ?definition; :hasSubStructureVitro ?sub_structure; :hasSubDefinitionVitro ?sub_definition }
```

5. Multi-Tissue Plant Structure
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureMulti ?structure; :hasDefinitionMulti  ?definition; :hasSubStructureMulti ?sub_structure; :hasSubDefinitionMulti ?sub_definition; :hasSubTypeMulti ?sub_type; :hasSubTypeDefinitionMulti ?sub_type_definition }
```

6. Plant Cell
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureCell ?structure; :hasDefinitionCell  ?definition; :hasSubStructureCell ?sub_structure; :hasSubDefinitionCell ?sub_definition }
```

7. Portion of Plant Tissue
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructurePortion ?structure; :hasDefinitionPortion  ?definition; :hasSubStructurePortion ?sub_structure; :hasSubDefinitionPortion ?sub_definition }
```

8. Rhizoid
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureRhizoid ?structure; :hasDefinitionRhizoid  ?definition; :hasSubStructureRhizoid ?sub_structure; :hasSubDefinitionRhizoid ?sub_definition }
```

9. Trichome
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureTrichome ?structure; :hasDefinitionTrichome  ?definition; :hasSubStructureTrichome ?sub_structure; :hasSubDefinitionTrichome ?sub_definition } 
```

10. Trichome Apex
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureTrichomeApex ?structure; :hasDefinitionTrichomeApex  ?definition}
```

11. Trichome Tip
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureTrichomeTip ?structure; :hasDefinitionTrichomeTip  ?definition}
```

12. Whole Plant
```sparql
PREFIX : <http://purl.obolibrary.org/obo/po.owl#>
SELECT * WHERE {?structureid :hasStructureWhole ?structure; :hasDefinitionWhole  ?definition; :hasSubStructureWhole ?sub_structure; :hasSubDefinitionWhole ?sub_definition }
``` 
