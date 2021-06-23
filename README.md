# Projeto API REST com Spring Boot, utilizando dependências para desenvolver um sistema


### Dependências:
 Spring Boot DevTools<br/>
 Lombok (getters, setters, codes, hascodes)<br/>
 Spring Web<br/>
 Spring Data JPA<br/>
 Spring Boot Actuator<br/>
 H2 Database SQL <br/>

<br />
<p>PersonController anotação @RestController : controlador que será acessado por uma API REST<br/>
                           @RequestMapping("/api/v1/people") --- caminho de acesso principal para API</p>

@GetMapping : operação do tipo get http que será acessado pelo browser<br/>
              toda requisição de pagina pelo get é feito pelo browser<br/>

O Heroku só reconhece aplicativos até o Java 8, por isso a necessidade de se criar um 
arquivo system.properties na pasta target (java.runtime.version=11)<br/>

Anotações interessantes do Lombok:<br/>
@Data (inserir getters e setters)<br/>
@Builder (padrão de construção de objetos)<br/>
@AllArgasConstructor e @NoArgsConstructor (vai inserir os construtores)<br/>

@Entity (indica que a classe é uma entidade)<br/>
@Id (entidade precisa definir chave primária)<br/>
@GeneratedValue(strategy=GenerationType.IDENTITY) (estratégia de geração do ID)<br/>
@Enumerated(EnumType.String) (validação, indicar com a anotação, EnumType com valor String)<br/>
@Column(nullable=false) (criar uma regra para uma constraint de banco de dados)<br/>
@OneToMany (criar relacionamento de uma pessoa com muitos telefones)<br/>
(fetch=fetchType.LAZY, cascade={CascadeType.PERSIST, CascadeType.MERGE, CascadeType.REMOVE})<br/>
fetch é a estratégia, .LAZY para performático e cascade quando será inserido o dado<br/>

### Com o JPA repository não é preciso abrir e fechar a conecção com o banco de dados<br/>
### JPA reposotiry vai gerenciar a instância e todo o ciclo de vida do objeto (DTO), e injetar na classe construtor<br/>
### DTO(data tranfer object) (criação de objetos para fazer a tranferência de dados) Não receber dados diretamente na classe service, pacote dto vai receber e gerar clones das entidades para instanciar e tratar os objetos<br/>

 @PostMapping (criação de pessoas)<br/>
 @Autowired (injeta no construtor, ou no atributo, ou no setter os dados do tipo repository que serão utilizados)<br/>
 @RequestBody (aviso de requisição do objeto DTO, no projeto o Person person)<br/>
 @Service (no Spring, gerenciar toda regra de negócio da aplicação)<br/>
 @Enumerated(EnumType.String) (validação das entradas no DTO Phone)<br/>
 @NotEmpty (Nunca pode ser vazio)<br/>
 @Size(min=13, max=14)  (determina o tamanho do campo informado)<br/>
 @CPF (hibernator.validator.constructor...nesse validator até o formato do CPF é verificado)<br/>
 @Valid (vai validar cada um dos dados do DTO) (fazer a validação através de cada uma das anotações de cada atributo utilizado)<br/>
 Validação no Controller : (após anotação @Request colocar @Valid para validar a regra e rodar cada um dos atributos)<br/>

 É preciso fazer a converção de tipos string date birthday para date e para evitar códigos extensos se utiliza o mapStruct<br/>
 mapStruct: com uma interface e apenas um método, se faz a conversão de uma DTO para uma entidade e converte uma entidade para uma DTO<br/>
 Quando se cria a interface PersonMapper para fazer funcionar o mapstruct com as configurações, plugins e builds do Maven<br/>
 é necessário ter o processamento de anotações do mapstruct e lombok configurados no POM, após isso criar a interface PersonMapper<br/>
 marcar a interface com a anotação @Mapper para que o mapstruct entenda que vai processar essa classe<br/>
 é necessário ter o anotation processors para que ao chamar o metodo toModel será pego todos os dados do DTO para serem mapeados<br/>
 Para converetr um DTO para um banco de dados por convenção se faz o uso do "toModel" , é preciso utilizar esse nome para que o mapstruct faça essa conversão<br/>
 @Mapping(target="birthDate", source="birthDate", dateFormat="dd-MM-yyyy") para que o mapstruct faça a conversão correta dos dados da data de aniversário(pegar a string e converter em dados para o banco de dados)<br/>

 @AllArgsConstructor(onConstructor = @__(@Autowired)) para tirar o construtor padrão  @Autowired(classe do PersonController)<br/>
 @AllArgsConstructor(onConstructor = @__(@Autowired)) na classe PersonService a mesma coisa<br/>

 Testes unitários na classe PersonService no createPerson para tentar cobir todas as funcionalidades e todos os cenários possíveis<br/>
 No caso do createPerson apenas um cenário possível, o de criar um person<br/>
 mas no findById são duas possibilidades, retornar um person válido ou id inválido que retorna uma exception<br/>

 Importante, o pacote de service no diretório test deve ser criada no mesmo nível do pacote service que será testado<br/>
 Vale tanto para os pacotes e para as classes testadas<br/>
