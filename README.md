# Projeto API REST com Spring Boot, utilizando dependências para desenvolver um sistema


### Dependências:
### Spring Boot DevTools
### Lombok (getters, setters, codes, hascodes)
### Spring Web
### Spring Data JPA
### Spring Boot Actuator
### H2 Database SQL

#####
##### PersonController anotação @RestController --- controlador que será acessado por uma API REST
#####                           @RequestMapping("/api/v1/people") --- caminho de acesso principal para API
#####
##### @GetMapping : operação do tipo get http que será acessado pelo browser
#####              toda requisição de pagina pelo get é feito pelo browser
#####
##### O Heroku só reconhece aplicativos até o Java 8, por isso a necessidade de se criar um 
##### arquivo system.properties na pasta target (java.runtime.version=11)
#####
#### Anotações interessantes do Lombok:
##### @Data (inserir getters e setters)
##### @Builder (padrão de construção de objetos)
##### @AllArgasConstructor e @NoArgsConstructor (vai inserir os construtores)
#####
##### @Entity (indica que a classe é uma entidade)
##### @Id (entidade precisa definir chave primária)
##### @GeneratedValue(strategy=GenerationType.IDENTITY) (estratégia de geração do ID)
##### @Enumerated(EnumType.String) (validação, indicar com a anotação, EnumType com valor String)
##### @Column(nullable=false) (criar uma regra para uma constraint de banco de dados)
##### @OneToMany (criar relacionamento de uma pessoa com muitos telefones)
##### (fetch=fetchType.LAZY, cascade={CascadeType.PERSIST, CascadeType.MERGE, CascadeType.REMOVE})
##### fetch é a estratégia, .LAZY para performático e cascade quando será inserido o dado
####
#### Com o JPA repository não é preciso abrir e fechar a conecção com o banco de dados
#### JPA reposotiry vai gerenciar a instância e todo o ciclo de vida do objeto (DTO), e injetar na classe construtor
#### DTO(data tranfer object) (criação de objetos para fazer a tranferência de dados) Não receber dados diretamente na classe service, pacote dto vai receber e gerar clones das entidades para instanciar e tratar os objetos
####
##### @PostMapping (criação de pessoas)
##### @Autowired (injeta no construtor, ou no atributo, ou no setter os dados do tipo repository que serão utilizados)
##### @RequestBody (aviso de requisição do objeto DTO, no projeto o Person person)
##### @Service (no Spring, gerenciar toda regra de negócio da aplicação)
##### @Enumerated(EnumType.String) (validação das entradas no DTO Phone)
##### @NotEmpty (Nunca pode ser vazio)
##### @Size(min=13, max=14)  (determina o tamanho do campo informado)
##### @CPF (hibernator.validator.constructor...nesse validator até o formato do CPF é verificado)
##### @Valid (vai validar cada um dos dados do DTO) (fazer a validação através de cada uma das anotações de cada atributo utilizado)
##### Validação no Controller : (após anotação @Request colocar @Valid para validar a regra e rodar cada um dos atributos)
#####
##### É preciso fazer a converção de tipos string date birthday para date e para evitar códigos extensos se utiliza o mapStruct
##### mapStruct: com uma interface e apenas um método, se faz a conversão de uma DTO para uma entidade e converte uma entidade para uma DTO
##### Quando se cria a interface PersonMapper para fazer funcionar o mapstruct com as configurações, plugins e builds do Maven
##### é necessário ter o processamento de anotações do mapstruct e lombok configurados no POM, após isso criar a interface PersonMapper
##### marcar a interface com a anotação @Mapper para que o mapstruct entenda que vai processar essa classe
##### é necessário ter o anotation processors para que ao chamar o metodo toModel será pego todos os dados do DTO para serem mapeados
##### Para converetr um DTO para um banco de dados por convenção se faz o uso do "toModel" , é preciso utilizar esse nome para que o mapstruct faça essa conversão
##### @Mapping(target="birthDate", source="birthDate", dateFormat="dd-MM-yyyy") para que o mapstruct faça a conversão correta dos dados da data de aniversário(pegar a string e converter em dados para o banco de dados)
#####
##### @AllArgsConstructor(onConstructor = @__(@Autowired)) para tirar o construtor padrão  @Autowired(classe do PersonController)
##### @AllArgsConstructor(onConstructor = @__(@Autowired)) na classe PersonService a mesma coisa


