# Workshop StringBoot3 com JPA

## Aplicação de Curso 

Este é um projeto Java construído usando Spring Boot 3 e JPA. A aplicação simula um sistema de e-commerce, incluindo entidades como Usuários, Pedidos, Produtos, Categorias e Pagamentos. Está configurada com um perfil de teste que carrega dados iniciais no banco de dados.

## Tecnologias Utilizadas

- Java
- Spring Boot 3
- Spring Data JPA
- Banco de Dados H2 (para fins de teste)
- Maven

```plaintext
src/main/java/com/educandoweb/course
|
├── CourseApplication.java                  // Classe principal da aplicação
├── config
│   └── TestConfig.java                     // Configuração para o perfil de teste
├── entities
│   ├── Category.java                       // Entidade Categoria
│   ├── Order.java                          // Entidade Pedido
│   ├── OrderItem.java                      // Entidade Item de Pedido
│   ├── Payment.java                        // Entidade Pagamento
│   ├── Product.java                        // Entidade Produto
│   └── User.java                           // Entidade Usuário
├── entities/enums
│   └── OrderStatus.java                    // Enumeração para status de pedidos
├── entities/pk
│   └── OrderItemPK.java                    // Classe para chave composta de Item de Pedido
├── repositories
│   ├── CategoryRepository.java             // Repositório para Categoria
│   ├── OrderItemRepository.java            // Repositório para Item de Pedido
│   ├── OrderRepository.java                // Repositório para Pedido
│   ├── ProductRepository.java              // Repositório para Produto
│   └── UserRepository.java                 // Repositório para Usuário
├── resources
│   ├── CategoryResource.java               // Recurso para Categoria
│   ├── OrderResource.java                  // Recurso para Pedido
│   ├── ProductResource.java                // Recurso para Produto
│   └── UserResource.java                   // Recurso para Usuário
├── resources/exceptions
│   ├── ResourceExceptionHandler.java       // Manipulador de exceções para recursos
│   └── StandardError.java                  // Estrutura padrão para mensagens de erro
├── services
│   ├── CategoryService.java                // Serviço para Categoria
│   ├── OrderService.java                   // Serviço para Pedido
│   ├── ProductService.java                 // Serviço para Produto
│   └── UserService.java                    // Serviço para Usuário
├── services/exceptions
│   ├── DatabaseException.java              // Exceção para erros de banco de dados
│   └── ResourceNotFoundException.java      // Exceção para recursos não encontrados

```

## Repositories (Repositórios)

Cada entidade principal (Category, Order, Product, User) possui uma interface de repositório que extende `JpaRepository`

```
public interface NomeEntidadeRepository extends JpaRepository<Entidade, Tipo da chave primária> {

}
```

## Services (Serviços)

Cada entidade principal (Category, Order, Product, User) possui uma classe de serviço responsável pela lógica de negócios

```
@Service
public class NomeEntidadeService {
    @Autowired
    private nomeEntidadeRepository repository;

    // Serviços (Exemplo entidade Category)

    public List<Category> findAll() {
        return repository.findAll();
    }
    
    public Category findById(Long id) {
        Optional<Category> category = repository.findById(id);
        return category.orElseThrow(() -> new ResourceNotFoundException(id));
    }
}

```

## Resources (Recursos)

Cada entidade principal (Category, Order, Product, User) possui uma classe de recurso que expõe endpoints RESTful.

```

// Exemplo com a entidade Category

@RestController
@RequestMapping(value = "/categories")
public class CategoryResource {
    @Autowired
    private CategoryService service;
    
    @GetMapping
    public ResponseEntity<List<Category>> findAll() {
        List<Category> list = service.findAll();
        return ResponseEntity.ok().body(list);
    }
    
    @GetMapping(value = "/{id}")
    public ResponseEntity<Category> findById(@PathVariable Long id) {
        Category category = service.findById(id);
        return ResponseEntity.ok().body(category);
    }
}
```



