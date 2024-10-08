# Unit Testing Project

## Overview

This project is a simple Product Management System implemented in Java using Spring Boot. The system allows users to perform CRUD (Create, Read, Update, Delete) operations on products. The project includes unit and integration testing using **JUnit** and **Mockito**, along with code coverage analysis using **Jacoco**. Continuous Integration (CI) is set up with **GitHub Actions**, and the code quality is analyzed using **SonarCloud**.
## Project Structure

- **`src/main/java`**: Contains the main application code.
- **`src/test/java`**: Contains unit tests for the application.

### Main Classes

- **`MiniProjetTestApplication`**: The main class that starts the Spring Boot application.
- **`ProductController`**: Controller handling product-related HTTP requests.
- **`ProductService`**: Service managing product-related business logic.
- **`ProductRepository`**: Repository interface for product persistence.

## Unit Tests

### ProductControllerTest

Tests for `ProductController` methods. Uses `MockMvc` to simulate HTTP requests and validate responses.

#### Example Test Cases

- **`testGetAllProducts`**: Verifies that all products are returned correctly.

  ```java
  @Test
  void testGetAllProducts() throws Exception {
      when(productService.getAllProducts()).thenReturn(List.of(product));
      mockMvc.perform(get(baseUrl + "/getAllProducts"))
              .andExpect(status().isOk())
              .andExpect(content().contentType(MediaType.APPLICATION_JSON))
              .andExpect(jsonPath("$[0].product_id").value(1L))
              .andExpect(jsonPath("$[0].product_title").value("Iphone"))
              .andExpect(jsonPath("$[0].product_description").value("it is an amazing phone"))
              .andExpect(jsonPath("$[0].product_price").value(1000.5));
      verify(productService, times(1)).getAllProducts();
  }

    ```

  - **`testAddOneProduct`**: Verifies that a product is added successfully.

    ```java
      @Test
      void testAddOneProduct() throws Exception {
          doNothing().when(productService).addOneProduct(product);
          mockMvc.perform(post(baseUrl + "/addOneProduct")
              .contentType(MediaType.APPLICATION_JSON)
              .content(objectMapper.writeValueAsString(product)))
              .andExpect(status().isCreated())
              .andExpect(content().string("Product is added successfully"));
          verify(productService, times(1)).addOneProduct(product);
      }
      ```
    
### ProductServiceTest

- **`testGetAllProducts`**: Verifies that all products are returned correctly.

  ```java
    @Test
    void testGetAllProducts() {
        Mockito.when(productRepository.findAll()).thenReturn(List.of(product));
        List<Product> products = productService.getAllProducts();
        assertNotNull(products);
        assertEquals(1, products.size());
        assertEquals(1L, products.get(0).getProduct_id());
        assertEquals("Iphone", products.get(0).getProduct_title());
        assertEquals("this phone is modern", products.get(0).getProduct_description());
        assertEquals(200.5, products.get(0).getProduct_price());
        Mockito.verify(productRepository, Mockito.times(1)).findAll();
    }

    ```

    - **`testUpdateOneProduct_Found`**: Verifies that a product is updated successfully when it is found.

    ```java
    @Test
    void testUpdateOneProduct_Found() {
        Mockito.when(productRepository.findById(1L)).thenReturn(Optional.of(product));
        boolean result = productService.updateOneProduct(1L, productUpdate);
        assertTrue(result);
        Mockito.verify(productRepository, Mockito.times(1)).findById(1L);
    }
    ```

## Setup and Configuration

### Dependencies

Ensure your `pom.xml` includes the following dependencies:


```xml
		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-api</artifactId>
			<version>5.10.0</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-engine</artifactId>
			<version>5.10.0</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-params</artifactId>
			<version>5.10.0</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.junit.platform</groupId>
			<artifactId>junit-platform-launcher</artifactId>
			<version>1.10.3</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.mockito</groupId>
			<artifactId>mockito-core</artifactId>
			<version>5.11.0</version>
			<scope>test</scope>
		</dependency>
 ```
    
#### command to run test :
```bash
  mvn test
```

## Code Coverage
To check code coverage, use tools like JaCoCo. Ensure that your IDE or build tool is configured to include coverage reports.

### generating report with command:
```bash
  mvn clean verify
```
and open the file : **('target/site/jacoco/index.html')** in browser
![Link Text](image/coverage_image_1.png)

or you can use simply intellij:
![Link Text](image/coverage_image_2.png)


