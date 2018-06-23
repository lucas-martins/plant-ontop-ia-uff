# Tutorial

Este tutorial irá mostrar como utilizar o sistema Ontop através da linguagem SPARQL para acessar as definições das estruturas das plantas definidas pela ontologia Plant Ontology Consortium.

Este tutorial é baseado noa artigo: "Plant Ontology Consortium e sua Aplicação no Sistema Ontop".

Requisitos
------------

* Java 7 or 8
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
