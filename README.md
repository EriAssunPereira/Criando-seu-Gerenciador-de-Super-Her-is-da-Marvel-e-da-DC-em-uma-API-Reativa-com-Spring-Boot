# Criando-seu-Gerenciador-de-Super-Her-is-da-Marvel-e-da-DC-em-uma-API-Reativa-com-Spring-Boot

## Introdução ao Projeto de Gerenciamento de Heróis

Neste projeto, vamos criar uma **API reativa** para gerenciar um catálogo de super-heróis da Marvel e da DC. Utilizaremos o **Spring Boot** com o **Spring WebFlux** para construir nossa API, aproveitando o modelo de programação reativo para lidar com o alto volume de requisições de forma eficiente e responsiva.

---

## Configuração do Ambiente

Antes de começarmos, é necessário configurar nosso ambiente de desenvolvimento. Isso inclui a instalação do **Spring Boot**, configuração do **DynamoDB localmente** para simular o ambiente de produção, e a preparação do nosso projeto para utilizar a **library Reactor**.

---

## Desenvolvimento da API

Com o ambiente pronto, partimos para o desenvolvimento da API. Aqui, definiremos os **endpoints** que permitirão aos usuários interagir com o catálogo de heróis, como adicionar novos heróis, buscar por heróis existentes, atualizar seus dados ou removê-los do sistema.

```java
@GetMapping("/heroes")
Flux<Hero> getAllHeroes() {
    return heroRepository.findAll();
}

@PostMapping("/heroes")
Mono<Hero> createHero(@RequestBody Hero hero) {
    return heroRepository.save(hero);
}
```

---

## Testes Unitários

Testar nossa API é crucial para garantir sua qualidade e funcionamento. Utilizaremos o **JUnit** para escrever testes unitários que validarão cada parte do nosso sistema. Por exemplo, podemos testar se a adição de um novo herói funciona como esperado.

```java
@Test
public void testCreateHero() {
    Hero hero = new Hero("Spider-Man", "Marvel");
    when(heroRepository.save(any(Hero.class))).thenReturn(Mono.just(hero));

    webTestClient.post()
        .uri("/heroes")
        .contentType(MediaType.APPLICATION_JSON)
        .bodyValue(hero)
        .exchange()
        .expectStatus().isCreated()
        .expectBody()
        .jsonPath("$.name").isNotEmpty()
        .jsonPath("$.name").isEqualTo("Spider-Man");
}
```

---

## Documentação da API

Por fim, documentaremos nossa API utilizando ferramentas como **Postman** e **Swagger**. Isso facilitará para outros desenvolvedores entenderem como interagir com nossa API e quais recursos estão disponíveis.

```yaml
paths:
  /heroes:
    get:
      summary: Retorna uma lista de todos os heróis
      responses:
        '200':
          description: Uma lista de heróis
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Hero'
```

---

Espero que este texto modular ajude a entender melhor como estruturar e descrever seu projeto de API reativa com Spring Boot. Lembre-se de adaptar os exemplos práticos para atender às necessidades específicas do seu catálogo de heróis.
